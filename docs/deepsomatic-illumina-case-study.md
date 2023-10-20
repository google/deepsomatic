# DeepSomatic Illumina WGS case study

In this case study, we show an example of running DeepSomatic on Illumina NovaSeq WGS
data. We use HCC1395 as an example for this case study.

## Data details

For this case-study, we use HCC1395 as an example. We run the analysis on `chr1`
that we hold out during training.

## Prepare environment

### Tools

[Docker](https://docs.docker.com/get-docker/) will be used to run DeepVariant
and [hap.py](https://github.com/illumina/hap.py),

### Download input data

We will be using GRCh38 for this case study.


```bash
BASE="${HOME}/deepsomatic-illumina-case-study"

# Set up input and output directory data
INPUT_DIR="${BASE}/input/data"
OUTPUT_DIR="${BASE}/output"

## Create local directory structure
mkdir -p "${INPUT_DIR}"
mkdir -p "${OUTPUT_DIR}"
mkdir -p "${OUTPUT_DIR}/sompy_output"

# Download reference to input directory
FTPDIR=ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/000/001/405/GCA_000001405.15_GRCh38/seqs_for_alignment_pipelines.ucsc_ids
curl ${FTPDIR}/GCA_000001405.15_GRCh38_no_alt_analysis_set.fna.gz | gunzip > ${INPUT_DIR}/GRCh38_no_alt_analysis_set.fasta
curl ${FTPDIR}/GCA_000001405.15_GRCh38_no_alt_analysis_set.fna.fai > ${INPUT_DIR}/GRCh38_no_alt_analysis_set.fasta.fai

# Download bam files to input directory
HTTPDIR=https://storage.googleapis.com/deepvariant/deepsomatic-case-studies/deepsomatic-wgs-case-study

curl ${HTTPDIR}/S1395_WGS_ilm_normal.bwa.dedup.chr1.bam > ${INPUT_DIR}/S1395_WGS_ilm_normal.bwa.dedup.chr1.bam
curl ${HTTPDIR}/S1395_WGS_ilm_normal.bwa.dedup.chr1.bam.bai > ${INPUT_DIR}/S1395_WGS_ilm_normal.bwa.dedup.chr1.bam.bai
curl ${HTTPDIR}/S1395_WGS_ilm_tumor.bwa.dedup.chr1.bam > ${INPUT_DIR}/S1395_WGS_ilm_tumor.bwa.dedup.chr1.bam
curl ${HTTPDIR}/S1395_WGS_ilm_tumor.bwa.dedup.chr1.bam.bai > ${INPUT_DIR}/S1395_WGS_ilm_tumor.bwa.dedup.chr1.bam.bai

# Download truth VCF
DATA_HTTP_DIR=https://storage.googleapis.com/deepvariant/deepsomatic-case-studies/SEQC2-S1395-truth
wget -P ${INPUT_DIR} "${DATA_HTTP_DIR}"/High-Confidence_Regions_v1.2.bed
wget -P ${INPUT_DIR} "${DATA_HTTP_DIR}"/high-confidence_sINDEL_sSNV_in_HC_regions_v1.2.1.merged.vcf.gz
wget -P ${INPUT_DIR} "${DATA_HTTP_DIR}"/high-confidence_sINDEL_sSNV_in_HC_regions_v1.2.1.merged.vcf.gz.tbi
```

## Running DeepSomatic with one command

DeepVariant pipeline consists of 3 steps: `make_examples_somatic`, `call_variants`, and
`postprocess_variants`. You can run DeepSomatic with one command using the
`run_deepvariant` script.

### Running on a CPU-only machine

```bash
BIN_VERSION="1.6.0"

sudo docker pull google/deepsomatic:"${BIN_VERSION}"

sudo docker run \
-v ${INPUT_DIR}:${INPUT_DIR} \
-v ${OUTPUT_DIR}:${OUTPUT_DIR} \
google/deepsomatic:"${BIN_VERSION}" \
run_deepsomatic \
--model_type=WGS \
--ref=${INPUT_DIR}/GRCh38_no_alt_analysis_set.fasta \
--reads_normal=${INPUT_DIR}/S1395_WGS_ilm_normal.bwa.dedup.chr1.bam \
--reads_tumor=${INPUT_DIR}/S1395_WGS_ilm_tumor.bwa.dedup.chr1.bam \
--output_vcf=${OUTPUT_DIR}/HCC1395_deepsomatic_output.vcf.gz \
--sample_name_tumor="HCC1395Tumor" \
--sample_name_normal="HCC1395Normal" \
--num_shards=$(nproc) \
--logging_dir=${OUTPUT_DIR}/logs \
--intermediate_results_dir=${OUTPUT_DIR}/intermediate_results_dir \
--regions=chr1
```

By specifying `--model_type WGS`, you'll be using a model that is best suited for Illumina Whole Genome Sequencing data.

NOTE: If you want to run each of the steps separately, add `--dry_run=true`
to the command above to figure out what flags you need in each step. Based on
the different model types, different flags are needed in the `make_examples`
step.

`--intermediate_results_dir` flag is optional. By specifying it, the
intermediate outputs of `make_examples_somatic` and `call_variants` stages can be found in the directory.

```bash
sudo docker pull pkrusche/hap.py:latest
# Run hap.py
sudo docker run \
-v ${INPUT_DIR}:${INPUT_DIR} -v ${OUTPUT_DIR}:${OUTPUT_DIR} \
pkrusche/hap.py:latest \
/opt/hap.py/bin/som.py \
-N ${INPUT_DIR}/high-confidence_sINDEL_sSNV_in_HC_regions_v1.2.1.merged.vcf.gz \
${OUTPUT_DIR}/HCC1395_deepsomatic_output.vcf.gz \
-r ${INPUT_DIR}/GRCh38_no_alt_analysis_set.fasta \
-o ${OUTPUT_DIR}/sompy_output/deepsomatic.chr1.sompy.output \
--feature-table generic \
-R ${INPUT_DIR}/High-Confidence_Regions_v1.2.bed \
-l chr1
```

The output:

```
      type  total.truth  total.query    tp  fp   fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels          133          156   125  31    8    0     0  0.939850      0.889721      0.971155  0.939850   0.801282         0.733470         0.858037   0          0       248956422  0.124520
1     SNVs         3440         3319  3290  29  150    0     0  0.956395      0.949183      0.962840  0.956395   0.991262         0.987653         0.994017   0          0       248956422  0.116486
5  records         3573         3475  3415  60  158    0     0  0.955779      0.948666      0.962155  0.955779   0.982734         0.977992         0.986673   0          0       248956422  0.241006
```
