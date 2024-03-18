# DeepSomatic PacBio case study

In this case study, we show an example of running DeepSomatic on PacBio
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

The data are originally from
https://downloads.pacbcloud.com/public/revio/2023Q2/HCC1395/, which has more
information about the data.

```bash
BASE="${HOME}/deepsomatic-pacbio-case-study"

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
HTTPDIR=https://storage.googleapis.com/deepvariant/deepsomatic-case-studies/deepsomatic-pacbio-case-study

curl ${HTTPDIR}/HCC1395.pacbio.normal.GRCh38.chr1.bam > ${INPUT_DIR}/HCC1395.pacbio.normal.GRCh38.chr1.bam
curl ${HTTPDIR}/HCC1395.pacbio.normal.GRCh38.chr1.bam.bai > ${INPUT_DIR}/HCC1395.pacbio.normal.GRCh38.chr1.bam.bai
curl ${HTTPDIR}/HCC1395.pacbio.tumor.GRCh38.chr1.bam > ${INPUT_DIR}/HCC1395.pacbio.tumor.GRCh38.chr1.bam
curl ${HTTPDIR}/HCC1395.pacbio.tumor.GRCh38.chr1.bam.bai > ${INPUT_DIR}/HCC1395.pacbio.tumor.GRCh38.chr1.bam.bai

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
BIN_VERSION="1.6.1"

sudo docker pull google/deepsomatic:"${BIN_VERSION}"

sudo docker run \
-v ${INPUT_DIR}:${INPUT_DIR} \
-v ${OUTPUT_DIR}:${OUTPUT_DIR} \
google/deepsomatic:"${BIN_VERSION}" \
run_deepsomatic \
--model_type=PACBIO \
--ref=${INPUT_DIR}/GRCh38_no_alt_analysis_set.fasta \
--reads_normal=${INPUT_DIR}/HCC1395.pacbio.normal.GRCh38.chr1.bam \
--reads_tumor=${INPUT_DIR}/HCC1395.pacbio.tumor.GRCh38.chr1.bam \
--output_vcf=${OUTPUT_DIR}/HCC1395_deepsomatic_output.vcf.gz \
--sample_name_tumor="HCC1395Tumor" \
--sample_name_normal="HCC1395Normal" \
--num_shards=$(nproc) \
--logging_dir=${OUTPUT_DIR}/logs \
--intermediate_results_dir=${OUTPUT_DIR}/intermediate_results_dir \
--regions=chr1
```

By specifying `--model_type PACBIO`, you'll be using a model that is best suited
for PacBio data.

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
      type  total.truth  total.query    tp   fp   fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels          133          130   106   24   27    0     0  0.796992      0.722688      0.858537  0.796992   0.815385         0.742158         0.874754   0          0       248956422  0.096402
1     SNVs         3440         3295  3189  106  251    0     0  0.927035      0.917983      0.935368  0.927035   0.967830         0.961389         0.973450   0          0       248956422  0.425777
5  records         3573         3425  3295  130  278    0     0  0.922194      0.913068      0.930637  0.922194   0.962044         0.955249         0.968058   0          0       248956422  0.522180
```
