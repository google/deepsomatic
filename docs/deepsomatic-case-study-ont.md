# DeepSomatic ONT case study

In this case study, we show an example of running DeepSomatic on ONT
data. We use HCC1395 as an example for this case study.

## Data details

For this case-study, we use HCC1395 as an example. We run the analysis on `chr1`
that we hold out during training.

Please see the [metrics page](metrics.md) for details on runtime and data.

## Prepare environment

### Tools

[Docker](https://docs.docker.com/get-docker/) will be used to run DeepSomatic
and [hap.py](https://github.com/illumina/hap.py),

### Download input data

We will be using GRCh38 for this case study.


```bash
BASE="${HOME}/deepsomatic-ont-case-study"

# Set up input and output directory data
INPUT_DIR="${BASE}/input/data"
OUTPUT_DIR="${BASE}/output"

## Create local directory structure
mkdir -p "${INPUT_DIR}"
mkdir -p "${OUTPUT_DIR}"
mkdir -p "${OUTPUT_DIR}/sompy_output"

# Download bam files to input directory
HTTPDIR=https://storage.googleapis.com/deepvariant/deepsomatic-case-studies/deepsomatic-chr1-case-studies
# Download the reference files
curl ${HTTPDIR}/GCA_000001405.15_GRCh38_no_alt_analysis_set.chr1.fna > ${INPUT_DIR}/GCA_000001405.15_GRCh38_no_alt_analysis_set.chr1.fna
curl ${HTTPDIR}/GCA_000001405.15_GRCh38_no_alt_analysis_set.chr1.fna.fai > ${INPUT_DIR}/GCA_000001405.15_GRCh38_no_alt_analysis_set.chr1.fna.fai

# Download the bam files
curl ${HTTPDIR}/HCC1395_ont.normal.chr1.bam > ${INPUT_DIR}/HCC1395_ont.normal.chr1.bam
curl ${HTTPDIR}/HCC1395_ont.normal.chr1.bam.bai > ${INPUT_DIR}/HCC1395_ont.normal.chr1.bam.bai
curl ${HTTPDIR}/HCC1395_ont.tumor.chr1.bam > ${INPUT_DIR}/HCC1395_ont.tumor.chr1.bam
curl ${HTTPDIR}/HCC1395_ont.tumor.chr1.bam.bai > ${INPUT_DIR}/HCC1395_ont.tumor.chr1.bam.bai

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
BIN_VERSION="1.8.0"

sudo docker pull google/deepsomatic:"${BIN_VERSION}"

sudo docker run \
-v ${INPUT_DIR}:${INPUT_DIR} \
-v ${OUTPUT_DIR}:${OUTPUT_DIR} \
google/deepsomatic:"${BIN_VERSION}" \
run_deepsomatic \
--model_type=ONT \
--ref=${INPUT_DIR}/GCA_000001405.15_GRCh38_no_alt_analysis_set.chr1.fna \
--reads_normal=${INPUT_DIR}/HCC1395_ont.normal.chr1.bam \
--reads_tumor=${INPUT_DIR}/HCC1395_ont.tumor.chr1.bam \
--output_vcf=${OUTPUT_DIR}/HCC1395_deepsomatic_output.vcf.gz \
--sample_name_tumor="HCC1395Tumor" \
--sample_name_normal="HCC1395Normal" \
--num_shards=$(nproc) \
--logging_dir=${OUTPUT_DIR}/logs \
--intermediate_results_dir=${OUTPUT_DIR}/intermediate_results_dir \
--regions=chr1
```

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
-r ${INPUT_DIR}/GCA_000001405.15_GRCh38_no_alt_analysis_set.chr1.fna \
-o ${OUTPUT_DIR}/sompy_output/deepsomatic.chr1.sompy.output \
--feature-table generic \
-R ${INPUT_DIR}/High-Confidence_Regions_v1.2.bed \
-l chr1
```

The output:

```
      type  total.truth  total.query    tp  fp   fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels          133          104    86  18   47    0     0  0.646617      0.562921      0.724009  0.646617   0.826923         0.745769         0.890185   0          0       248956422  0.072302
1     SNVs         3440         2660  2619  41  821    0     0  0.761337      0.746877      0.775358  0.761337   0.984586         0.979363         0.988757   0          0       248956422  0.164687
5  records         3573         2764  2705  59  868    0     0  0.757067      0.742801      0.770917  0.757067   0.978654         0.972754         0.983553   0          0       248956422  0.236989
```
