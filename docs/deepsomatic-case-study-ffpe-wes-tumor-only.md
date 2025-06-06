# DeepSomatic FFPE WES tumor-only case study

In this case study, we show an example of running DeepSomatic on FFPE WES
tumor-only data. We use HCC1395 as an example for this case study.

## Data details

For this case-study, we use HCC1395 as an example. We run the analysis on `chr1`
that we hold out during training.

Please see the [metrics page](metrics.md) for details on runtime and data.

## Allele frequency channel

For accurate tumor-only calling, we use the allele-frequency channel that uses
1000 genomes variant calls using DeepVariant to filter out germline variants
during inference. Currently, the default VCF is set to variant calls against
GRCh38 reference. If you want to customize this to your VCF then please do
so by using `--population_vcfs` parameter.

## Prepare environment

### Tools

[Docker](https://docs.docker.com/get-docker/) will be used to run DeepSomatic
and [hap.py](https://github.com/illumina/hap.py),

### Download input data

We will be using GRCh38 for this case study.


```bash
BASE="${HOME}/deepsomatic-ffpe-wes-tumor-only-case-study"

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

# Download the bam file
curl ${HTTPDIR}/HCC1395_ffpe_wes.tumor.chr1.bam > ${INPUT_DIR}/HCC1395_ffpe_wes.tumor.chr1.bam
curl ${HTTPDIR}/HCC1395_ffpe_wes.tumor.chr1.bam.bai > ${INPUT_DIR}/HCC1395_ffpe_wes.tumor.chr1.bam.bai

# Download truth VCF
DATA_HTTP_DIR=https://storage.googleapis.com/deepvariant/deepsomatic-case-studies/SEQC2-S1395-truth
wget -P ${INPUT_DIR} "${DATA_HTTP_DIR}"/High-Confidence_Regions_v1.2.bed
wget -P ${INPUT_DIR} "${DATA_HTTP_DIR}"/high-confidence_sINDEL_sSNV_in_HC_regions_v1.2.1.merged.vcf.gz
wget -P ${INPUT_DIR} "${DATA_HTTP_DIR}"/high-confidence_sINDEL_sSNV_in_HC_regions_v1.2.1.merged.vcf.gz.tbi
wget -P ${INPUT_DIR} "${DATA_HTTP_DIR}"/seqc2_hg38.exome_regions.bed
```

## Running DeepSomatic with one command

DeepVariant pipeline consists of 3 steps: `make_examples_somatic`, `call_variants`, and
`postprocess_variants`. You can run DeepSomatic with one command using the
`run_deepvariant` script.

### Running on a CPU-only machine

```bash
BIN_VERSION="1.9.0"

sudo docker pull google/deepsomatic:"${BIN_VERSION}"

sudo docker run \
-v ${INPUT_DIR}:${INPUT_DIR} \
-v ${OUTPUT_DIR}:${OUTPUT_DIR} \
google/deepsomatic:"${BIN_VERSION}" \
run_deepsomatic \
--model_type=FFPE_WES_TUMOR_ONLY \
--ref=${INPUT_DIR}/GCA_000001405.15_GRCh38_no_alt_analysis_set.chr1.fna \
--reads_tumor=${INPUT_DIR}/HCC1395_ffpe_wes.tumor.chr1.bam \
--output_vcf=${OUTPUT_DIR}/HCC1395_deepsomatic_output.vcf.gz \
--sample_name_tumor="HCC1395Tumor" \
--num_shards=$(nproc) \
--logging_dir=${OUTPUT_DIR}/logs \
--intermediate_results_dir=${OUTPUT_DIR}/intermediate_results_dir \
--use_default_pon_filtering=true \
--regions=chr1
```

By using `--use_default_pon_filtering=true` the somatic variants will be
filtered using the default PON vcf that contains variant calls from dbSNP,
gnomAD and 1000 genomes. If you plan to customize post-filtering, then you
can set this parameter to `false` and use custom filtering.

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
-T ${INPUT_DIR}/seqc2_hg38.exome_regions.bed \
-l chr1
```

The output:

```
      type  total.truth  total.query   tp  fp  fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels            7           13    6   7   1    0     0  0.857143      0.499203      1.000000  0.857143   0.461538         0.221123         0.717108   0          0       248956422  0.028117
1     SNVs          145          129   99  30  46    0     0  0.682759      0.603975      0.754328  0.682759   0.767442         0.689149         0.833890   0          0       248956422  0.120503
5  records          152          142  105  37  47    0     0  0.690789      0.614250      0.760141  0.690789   0.739437         0.662930         0.806296   0          0       248956422  0.148620
```
