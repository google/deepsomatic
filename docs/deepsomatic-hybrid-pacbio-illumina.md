# DeepSomatic Hybrid (PacBio Tumor and Illumina Normal) Model case study

In this case study, we show an example of running DeepSomatic using a custom
hybrid model that uses PacBio for tumor data and Illumina for a normal sample.

This experimental model is being shared as part of the r1.9 documentation.
Inclusion of updated version of this model in future releases is not guaranteed.

We use HCC1395 as an example for this case study.

## Data details

For this case study, we use HCC1395 as an example and run the analysis on chr1,
which we hold out from training.

## Prepare environment

### Tools

[Docker](https://docs.docker.com/get-docker/) will be used to run DeepSomatic
and [som.py](https://github.com/Illumina/hap.py/blob/master/doc/sompy.md) will
be used to analyze the results.

You will also need `gsutil` to download datasets.

### Download input data

We will be using GRCh38 for this case study.

```bash
BASE="${HOME}/deepsomatic-hybrid-pb_t-illumina_n-case-study"

# Set up input and output directory data
INPUT_DIR="${BASE}/input/data"
OUTPUT_DIR="${BASE}/output"

## Create local directory structure
mkdir -p "${INPUT_DIR}"
mkdir -p "${OUTPUT_DIR}"
mkdir -p "${OUTPUT_DIR}/sompy_output"

# Download PacBio Tumor Bam
gsutil cp gs://deepvariant/deepsomatic-case-studies/deepsomatic-chr1-case-studies/HCC1395.GRCh38.tumor.chr1.nygc.bam* ${INPUT_DIR}/

# Download Illumina Normal WGS
gsutil cp gs://deepvariant/deepsomatic-case-studies/deepsomatic-chr1-case-studies/HCC1395_wgs.normal.chr1.bam* ${INPUT_DIR}/

# Download a reference genome
gsutil cp gs://deepvariant/deepsomatic-case-studies/deepsomatic-chr1-case-studies/GCA_000001405.15_GRCh38_no_alt_analysis_set.chr1.fna* ${INPUT_DIR}/

# Download truth VCF
gsutil -m cp gs://deepvariant/deepsomatic-case-studies/SEQC2-S1395-truth/High-Confidence_Regions_v1.2.bed \
gs://deepvariant/deepsomatic-case-studies/SEQC2-S1395-truth/high-confidence_sINDEL_sSNV_in_HC_regions_v1.2.1.merged.vcf.gz* \
gs://deepvariant/deepsomatic-case-studies/SEQC2-S1395-truth/seqc2_hg38.exome_regions.bed ${INPUT_DIR}

# Download the model
gsutil -m cp gs://deepvariant/models/experimental/deepsomatic_hybrid/exp_157994267/* ${INPUT_DIR}
```

## Running DeepSomatic with one command

DeepSomatic pipeline consists of 3 steps: `make_examples_somatic`,
`call_variants`, and `postprocess_variants`. You can run DeepSomatic with one
command using the `run_deepsomatic` script.

### Running on a CPU-only machine

First, make sure you can run Docker without sudo:

```bash
sudo usermod -a -G docker ${USER}
newgrp docker
```

Then, run DeepSomatic with the hybrid model:

```bash
# Make sure these are set:
BASE="${HOME}/deepsomatic-hybrid-pb_t-illumina_n-case-study"
INPUT_DIR="${BASE}/input/data"
OUTPUT_DIR="${BASE}/output"

# Set DeepSomatic version:
BIN_VERSION="1.9.0"

docker pull google/deepsomatic:"${BIN_VERSION}"
docker run \
  -v ${INPUT_DIR}:${INPUT_DIR} \
  -v ${OUTPUT_DIR}:${OUTPUT_DIR} \
  google/deepsomatic:"${BIN_VERSION}" \
  run_deepsomatic \
    --model_type=PACBIO \
    --ref=${INPUT_DIR}/GCA_000001405.15_GRCh38_no_alt_analysis_set.chr1.fna \
    --reads_tumor=${INPUT_DIR}/HCC1395.GRCh38.tumor.chr1.nygc.bam \
    --reads_normal=${INPUT_DIR}/HCC1395_wgs.normal.chr1.bam \
    --output_vcf=${OUTPUT_DIR}/HCC1395_ds_hybrid_output.vcf.gz \
    --sample_name_tumor="HCC1395Tumor" \
    --num_shards=$(nproc) \
    --logging_dir=${OUTPUT_DIR}/logs \
    --make_examples_extra_args="vsc_min_fraction_snps=0.02,vsc_min_count_snps=1,parse_sam_aux_fields=true,sort_by_haplotypes=true,phase_reads=true,realign_reads=true,alt_aligned_pileup=diff_channels,min_mapping_quality=5,track_ref_reads=true,partition_size=25000,vsc_max_fraction_snps_for_non_target_sample=0.5,vsc_min_fraction_indels=0.1,vsc_max_fraction_indels_for_non_target_sample=0.5,channel_list='BASE_CHANNELS,haplotype,insert_size',max_reads_per_partition=0,max_reads_for_dynamic_bases_per_region=3000,pileup_image_width=221" \
    --intermediate_results_dir=${OUTPUT_DIR}/intermediate_results_dir \
    --customized_model=${INPUT_DIR}/checkpoint-265728-0.94768-1 \
    --regions=chr1
```

NOTE: If you want to run each of the steps separately, add `--dry_run=true`
to the command above to figure out what flags you need in each step. Based on
the different model types, different flags are needed in the `make_examples`
step.

`--intermediate_results_dir` flag is optional. By specifying it, the
intermediate outputs of `make_examples_somatic` and `call_variants` stages can
be found in the directory.

To use som.py, you might encounter this error message if you pull from
`pkrusche/hap.py:latest`.

```
Docker Image Format v1 and Docker Image manifest version 2, schema 1 support has been removed. Suggest the author of docker.io/pkrusche/hap.py:latest to upgrade the image to the OCI Format or Docker Image manifest v2, schema 2. More information at https://docs.docker.com/go/deprecated-image-specs/
```

You can get around the issue by building your own:

```bash
git clone https://github.com/illumina/hap.py.git
cd hap.py
# Modify the Dockerfile to make sure the build works:
sed -i -e 's/RUN pip install bx-python/RUN pip install --no-binary :all: bx-python/' Dockerfile
docker build -t local-happy:latest .
```

Then, you can use your own version to run som.py:

```bash
# Run som.py
docker run \
  -v ${INPUT_DIR}:${INPUT_DIR} -v ${OUTPUT_DIR}:${OUTPUT_DIR} \
  local-happy:latest \
  /opt/hap.py/bin/som.py \
  -N ${INPUT_DIR}/high-confidence_sINDEL_sSNV_in_HC_regions_v1.2.1.merged.vcf.gz \
  ${OUTPUT_DIR}/HCC1395_ds_hybrid_output.vcf.gz \
  -r ${INPUT_DIR}/GCA_000001405.15_GRCh38_no_alt_analysis_set.chr1.fna \
  -o ${OUTPUT_DIR}/sompy_output/deepsomatic.chr1.sompy.output \
  --feature-table generic \
  -R ${INPUT_DIR}/High-Confidence_Regions_v1.2.bed \
  -l chr1
```

The output:

```
      type  total.truth  total.query    tp   fp   fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper   na  ambiguous  fp.region.size   fp.rate
0   indels          133          131   107   24   26    0     0  0.804511      0.730987      0.864953  0.804511   0.816794         0.744032         0.875742  0.0        0.0       248956422  0.096402
1     SNVs         3440         3427  3262  165  178    0     0  0.948256      0.940474      0.955284  0.948256   0.951853         0.944301         0.958642  0.0        0.0       248956422  0.662767
5  records         3573         3558  3369  189  204    0     0  0.942905      0.934936      0.950157  0.942905   0.946880         0.939145         0.953889  0.0        0.0       248956422  0.759169
```

If you have any questions or feedback about this model, you can let us know by
opening an issue on GitHub at https://github.com/google/deepsomatic/issues.
