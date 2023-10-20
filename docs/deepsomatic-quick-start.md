# DeepSomatic quick start

This is an explanation of how to use DeepSomatic.

## Background

To get started, you'll need the DeepSomatic programs (and some packages they
depend on), some test data, and of course a place to run them.

We've provided a Docker image, and some test data in a bucket on Google Cloud
Storage. The instructions below show how to download the data through the
corresponding public URLs from these data.

This setup requires a machine with the AVX instruction set. To see if your
machine meets this requirement, you can check the `/proc/cpuinfo` file, which
lists this information under "flags". If you do not have the necessary
instructions, see the next section for more information on how to build your own
Docker image.

### Use Docker to run DeepSomatic in one command.

## Get Docker image, models, and test data

### Get Docker image

```bash
BIN_VERSION="1.6.0"

sudo apt -y update
sudo apt-get -y install docker.io
sudo docker pull google/deepsomatic:"${BIN_VERSION}"
```

### Download test data

Before you start running, you need to have the following input files:

1.  A reference genome in [FASTA] format and its corresponding index file
    (.fai).

1.  An aligned reads file in [BAM] format and its corresponding index file
    (.bai). You get this by aligning the reads from a sequencing instrument,
    using an aligner like [BWA] for example.

We've prepared a small test data bundle for use in this quick start guide that
can be downloaded to your instance from the public URLs.

Download the test bundle:

```bash
INPUT_DIR="${PWD}/deepsomatic-quickstart-testdata"
DATA_HTTP_DIR="https://storage.googleapis.com/deepvariant/deepsomatic-case-studies/quick-start"

mkdir -p ${INPUT_DIR}
wget -P ${INPUT_DIR} "${DATA_HTTP_DIR}"/S1395_WGS_ilm_normal.bwa.dedup.chr1.quickstart.bam
wget -P ${INPUT_DIR} "${DATA_HTTP_DIR}"/S1395_WGS_ilm_normal.bwa.dedup.chr1.quickstart.bam.bai
wget -P ${INPUT_DIR} "${DATA_HTTP_DIR}"/S1395_WGS_ilm_tumor.bwa.dedup.chr1.quickstart.bam
wget -P ${INPUT_DIR} "${DATA_HTTP_DIR}"/S1395_WGS_ilm_tumor.bwa.dedup.chr1.quickstart.bam.bai
wget -P ${INPUT_DIR} "${DATA_HTTP_DIR}"/GRCh38_no_alts_chr1.fasta
wget -P ${INPUT_DIR} "${DATA_HTTP_DIR}"/GRCh38_no_alts_chr1.fasta.fai
wget -P ${INPUT_DIR} "${DATA_HTTP_DIR}"/SEQC2_truth.chr1.quick_start.vcf.gz
wget -P ${INPUT_DIR} "${DATA_HTTP_DIR}"/SEQC2_truth.chr1.quick_start.vcf.gz.tbi
wget -P ${INPUT_DIR} "${DATA_HTTP_DIR}"/SEQC2_truth.chr1.quick_start.bed
```

This should create a subdirectory in the current directory containing the actual
data files:

```bash
ls -1 ${INPUT_DIR}
```

outputting:

```
GRCh38_no_alts_chr1.fasta
GRCh38_no_alts_chr1.fasta.fai
S1395_WGS_ilm_normal.bwa.dedup.chr1.quickstart.bam
S1395_WGS_ilm_normal.bwa.dedup.chr1.quickstart.bam.bai
S1395_WGS_ilm_tumor.bwa.dedup.chr1.quickstart.bam
S1395_WGS_ilm_tumor.bwa.dedup.chr1.quickstart.bam.bai
SEQC2_truth.chr1.quick_start.bed
SEQC2_truth.chr1.quick_start.vcf.gz
SEQC2_truth.chr1.quick_start.vcf.gz.tbi
```

## Run DeepSomatic with one command

DeepSomatic consists of 3 main binaries: `make_somatic_examples`, `call_variants`, and
`postprocess_variants`. To make it easier to run, we create one entrypoint that
can be directly run as a docker command.

```bash
OUTPUT_DIR="${PWD}/quickstart-output"
mkdir -p "${OUTPUT_DIR}"
```

You can run everything with the following command:

```bash
sudo docker run \
-v ${INPUT_DIR}:${INPUT_DIR} \
-v ${OUTPUT_DIR}:${OUTPUT_DIR} \
google/deepsomatic:"${BIN_VERSION}" \
run_deepsomatic \
--model_type=WGS \
--ref=${INPUT_DIR}/GRCh38_no_alts_chr1.fasta \
--reads_normal=${INPUT_DIR}/S1395_WGS_ilm_normal.bwa.dedup.chr1.quickstart.bam \
--reads_tumor=${INPUT_DIR}/S1395_WGS_ilm_tumor.bwa.dedup.chr1.quickstart.bam \
--output_vcf=${OUTPUT_DIR}/HCC1395_deepsomatic_quickstart.vcf.gz \
--output_gvcf=${OUTPUT_DIR}/HCC1395_deepsomatic_quickstart.g.vcf.gz \
--sample_name_tumor="tumor" \
--sample_name_normal="normal" \
--num_shards=1 \
--logging_dir=${OUTPUT_DIR}/logs \
--intermediate_results_dir ${OUTPUT_DIR}/intermediate_results_dir \
--regions=chr1:10,000,000-10,100,000
```

NOTE: If you want to look at all the commands being run, you can add
`--dry_run=true` to the command above, which will print out all the commands
but not execute them.

This will generate 5 files and 1 directory in `${OUTPUT_DIR}`:

```bash
ls -1 ${OUTPUT_DIR}
```

outputting:

```
HCC1395_deepsomatic_quickstart.g.vcf.gz
HCC1395_deepsomatic_quickstart.g.vcf.gz.tbi
HCC1395_deepsomatic_quickstart.vcf.gz
HCC1395_deepsomatic_quickstart.vcf.gz.tbi
HCC1395_deepsomatic_quickstart.visual_report.html
intermediate_results_dir
logs
```

The directory "intermediate_results_dir" exists because
`--intermediate_results_dir /output/intermediate_results_dir` is specified. This
directory contains the intermediate output of make_examples_somatic and
call_variants steps.

## Notes on GPU image

If you are using GPUs, you can pull the GPU version, and make sure you run with
`--gpus 1`. `call_variants` is the only step that uses the GPU, and can only use
one at a time. `make_examples_somatic` and `postprocess_variants` do not run on
GPU.

```bash
sudo docker run --gpus 1 \
-v ${INPUT_DIR}:${INPUT_DIR} \
-v ${OUTPUT_DIR}:${OUTPUT_DIR} \
google/deepsomatic:"${BIN_VERSION}-gpu" \
run_deepsomatic \
...
```

## Notes on Singularity

### CPU version

```bash
# Pull the image.
singularity pull docker://google/deepsomatic:"${BIN_VERSION}"

# Run DeepSomatic.
singularity run -B /usr/lib/locale/:/usr/lib/locale/ \
docker://google/deepsomatic:"${BIN_VERSION}" \
run_deepsomatic \
--model_type=WGS \
--ref=${INPUT_DIR}/GRCh38_no_alts_chr1.fasta \
--reads_normal=${INPUT_DIR}/S1395_WGS_ilm_normal.bwa.dedup.chr1.quickstart.bam \
--reads_tumor=${INPUT_DIR}/S1395_WGS_ilm_tumor.bwa.dedup.chr1.quickstart.bam \
--output_vcf=${OUTPUT_DIR}/HCC1395_deepsomatic_quickstart.vcf.gz \
--output_gvcf=${OUTPUT_DIR}/HCC1395_deepsomatic_quickstart.g.vcf.gz \
--sample_name_tumor="tumor" \
--sample_name_normal="normal" \
--num_shards=1 \ ** Set the number of threads **
--logging_dir=${OUTPUT_DIR}/logs \
--intermediate_results_dir ${OUTPUT_DIR}/intermediate_results_dir \
--regions=chr1:10,000,000-10,100,000
```

### GPU version

```
# Pull the image.
singularity pull docker://google/deepsomatic:"${BIN_VERSION}-gpu"

# Run DeepSomatic.
# Using "--nv" and "${BIN_VERSION}-gpu" is important.
singularity run --nv -B /usr/lib/locale/:/usr/lib/locale/ \
  docker://google/deepsomatic:"${BIN_VERSION}-gpu" \
  run_deepsomatic \
  ...
```

## Evaluating the results

Here we use the `hap.py`
([https://github.com/Illumina/hap.py](https://github.com/Illumina/hap.py))
program from Illumina to evaluate the resulting 10 kilobase vcf file. This
serves as a quick check to ensure the three DeepSomatic commands ran correctly.

```bash
sudo docker pull jmcdani20/hap.py:v0.3.12

sudo docker run -it \
-v ${INPUT_DIR}:${INPUT_DIR} \
-v ${OUTPUT_DIR}:${OUTPUT_DIR} \
jmcdani20/hap.py:v0.3.12 /opt/hap.py/bin/som.py \
${INPUT_DIR}/SEQC2_truth.chr1.quick_start.vcf.gz \
${OUTPUT_DIR}/HCC1395_deepsomatic_quickstart.vcf.gz \
--restrict-regions ${INPUT_DIR}/SEQC2_truth.chr1.quick_start.bed \
-r ${INPUT_DIR}/GRCh38_no_alts_chr1.fasta \
-o ${OUTPUT_DIR}/s1395_deepsomatic_chr1_quickstart \
--feature-table generic
```

You should see output similar to the following.

```
Benchmarking Summary:
      type  total.truth  total.query  tp  fp  fn  unk  ambi  recall  recall_lower  recall_upper  recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size  fp.rate
1     SNVs            1            1   1   0   0    0     0       1         0.025             1        1          1            0.025                1   0          0       248956422        0
5  records            1            1   1   0   0    0     0       1         0.025             1        1          1            0.025                1   0          0       248956422        0
```
