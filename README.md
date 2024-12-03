# DeepSomatic

[![release](https://img.shields.io/badge/release-v1.8.0-green?logo=github)](https://github.com/google/deepvariant/releases)
[![announcements](https://img.shields.io/badge/announcements-blue)](https://groups.google.com/d/forum/deepvariant-announcements)
[![blog](https://img.shields.io/badge/blog-orange)](https://goo.gl/deepvariant)

DeepSomatic is an extension of deep learning-based variant caller
[DeepVariant](https://github.com/google/deepvariant) that takes aligned reads
(in BAM or CRAM format) from tumor and normal data, produces pileup image
tensors from them, classifies each tensor using a convolutional neural network,
and finally reports somatic variants in a standard VCF or gVCF file.

DeepSomatic supports somatic variant-calling from tumor-normal and tumor-only
sequencing data.

## Code availability

DeepSomatic is integrated with
[DeepVariant](https://github.com/google/deepvariant) to utilize the high-quality
end-to-end testing and feature development of DeepVariant.

Here are the scripts that describe the core components of DeepSomatic:

*   [run_deepsomatic](https://github.com/google/deepvariant/blob/r1.8.0/scripts/run_deepsomatic.py):
    The DeepSomatic runner script.

*   [make_examples_somatic](https://github.com/google/deepvariant/blob/r1.8.0/deepvariant/make_examples_somatic.py):
    The `make_examples` step for DeepSomatic.

*   [call_variants](https://github.com/google/deepvariant/blob/r1.8.0/deepvariant/call_variants.py):
    Inference script that generates the variant calls.

*   [postprocess_variants](https://github.com/google/deepvariant/blob/r1.8.0/deepvariant/postprocess_variants.py):
    Updated with `process_somatic` option to process somatic variants.

*   [dockerfile](https://github.com/google/deepvariant/blob/r1.8.0/Dockerfile.deepsomatic):
    The Dockerfile for DeepSomatic.

Integrating DeepSomatic within DeepVariant helps to maintain
high-quality code health with integrated testing and feature development.

## Case studies

The following case studies show example runs for supported technologies:

### Tumor-normal case-studies

*   Illumina WGS tumor-normal [case study](docs/deepsomatic-case-study-wgs.md).

*   Illumina WES tumor-normal [case study](docs/deepsomatic-case-study-wes.md).

*   PacBio tumor-normal [case study](docs/deepsomatic-case-study-pacbio.md).

*   ONT tumor-normal [case study](docs/deepsomatic-case-study-ont.md).

*   FFPE WGS tumor-normal [case study](docs/deepsomatic-case-study-ffpe-wgs.md).

*   FFPE WES tumor-normal [case study](docs/deepsomatic-case-study-ffpe-wes.md).

### Tumor-only case-studies

*   Illumina WGS tumor-only
    [case study](docs/deepsomatic-case-study-wgs-tumor-only.md).

*   PacBio tumor-only
    [case study](docs/deepsomatic-case-study-pacbio-tumor-only.md).

*   ONT tumor-only [case study](docs/deepsomatic-case-study-ont-tumor-only.md).

For details around runtime and accuracy expectations, please see the [DeepSomatic metrics page](docs/metrics.md).

## How to Cite

If you use DeepSomatic in your work, please cite:

[DeepSomatic: Accurate somatic small variant discovery for multiple sequencing technologies]( https://doi.org/10.1101/2024.08.16.608331)

## How to run DeepSomatic

```bash
sudo docker run \
-v ${INPUT_DIR}:${INPUT_DIR} \
-v ${OUTPUT_DIR}:${OUTPUT_DIR} \
google/deepsomatic:"${BIN_VERSION}" \
run_deepsomatic \
--model_type=WGS \ ** Can be WGS,WES,PACBIO,ONT,FFPE_WGS,FFPE_WES,WGS_TUMOR_ONLY,PACBIO_TUMOR_ONLY,ONT_TUMOR_ONLY **
--ref=${INPUT_DIR}/REF.fasta \ **Path to reference fasta file.
--reads_normal=${INPUT_DIR}/normal.bam \ **Path to normal bam file.
--reads_tumor=${INPUT_DIR}/tumor.bam \ * Path to tumor bam file.
--output_vcf=${OUTPUT_DIR}/OUTPUT.vcf.gz \ **Path to output VCF file.
--output_gvcf=${OUTPUT_DIR}/OUTPUT.g.vcf.gz \ **Path to output gVCF file.
--sample_name_tumor="tumor" \
--sample_name_normal="normal" \
--num_shards=$(nproc) \ **Total number of threads to use.
--logging_dir=${OUTPUT_DIR}/logs \ **Log output directory.
--intermediate_results_dir ${OUTPUT_DIR}/intermediate_results_dir \
--regions=chr1 \ **Region of the genome, if not provided then runs on whole genome
--use_default_pon_filtering=false \ **Set to true for default PON filtering for tumor-only variant calling**
--dry_run=false **Default is false. If set to true, commands will be printed out but not executed.
```

Please follow the [Quick Start](docs/deepsomatic-quick-start.md) for more
details on different setups like Docker and Singuarity. available for
DeepSomatic

### Example output

DeepSomatic utilizes FILTER in VCF format to report identified germline and
somatic variants. The description of the filters can be found in the header:

```bash
##FILTER=<ID=PASS,Description="All filters passed">
##FILTER=<ID=RefCall,Description="Genotyping model thinks this site is reference.">
##FILTER=<ID=LowQual,Description="Confidence in this variant being real is below calling threshold.">
##FILTER=<ID=NoCall,Description="Site has depth=0 resulting in no call.">
##FILTER=<ID=GERMLINE,Description="Non somatic variants">
```

For example, the variants reported below:

```bash
# CHROM POS     ID  REF ALT QUAL    FILTER      INFO    FORMAT              SAMPLE_NAME
chr1    14001   .   A   G   3.7     GERMLINE    .       GT:GQ:DP:AD:VAF:PL  0/0:4:8:4,4:0.5:1,0,34
chr1    14002   .   T   A   0       RefCall     .       GT:GQ:DP:AD:VAF:PL  0/0:51:60:57,2:0.0333333:0,51,58
chr1    14003   .   C   G   43.8    PASS        .       GT:GQ:DP:AD:VAF:PL  1/1:43:74:0,74:1:43,52,0
```

In this example:

* The variant with `GERMLINE` FILTER status is identified as a germline variant
* The variant with `RefCall` FILTER status is homozygous to the reference
* The variant with `PASS` FILTER status is a **somatic variant**.

### Prerequisites

*   Unix-like operating system (cannot run on Windows)
*   Python 3.10

## Contribution Guidelines

Please [open a pull request](https://github.com/google/deepsomatic/compare) if
you wish to contribute to DeepSomatic. Note, we have not set up the
infrastructure to merge pull requests externally. If you agree, we will test and
submit the changes internally and mention your contributions in our
[release notes](https://github.com/google/deepsomatic/releases). We apologize
for any inconvenience.

If you have any difficulty using DeepSomatic, feel free to
[open an issue](https://github.com/google/deepsomatic/issues/new). If you have
general questions not specific to DeepSomatic, we recommend that you post on a
community discussion forum such as [BioStars](https://www.biostars.org/).

## License

[BSD-3-Clause license](LICENSE)

## Disclaimer

This is not an official Google product.

NOTE: the content of this research code repository (i) is not intended to be a
medical device; and (ii) is not intended for clinical use of any kind, including
but not limited to diagnosis or prognosis.
