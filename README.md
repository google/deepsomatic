# DeepSomatic

[![release](https://img.shields.io/badge/release-v1.6.0-green?logo=github)](https://github.com/google/deepvariant/releases)
[![announcements](https://img.shields.io/badge/announcements-blue)](https://groups.google.com/d/forum/deepvariant-announcements)
[![blog](https://img.shields.io/badge/blog-orange)](https://goo.gl/deepvariant)

DeepSomatic is an extension of deep learning-based variant caller
[DeepVariant](https://github.com/google/deepvariant) that takes aligned reads
(in BAM or CRAM format) from tumor and normal data, produces pileup image
tensors from them, classifies each tensor using a convolutional neural network,
and finally reports somatic variants in a standard VCF or gVCF file.

DeepSomatic supports somatic variant-calling from tumor-normal sequencing data.

The following case studies show example runs for supported technologies:

* Illumina tumor-normal whole genome sequencing [case study](docs/deepsomatic-illumina-case-study.md).

* PacBio tumor-normal whole genome sequencing [case study](docs/deepsomatic-pacbio-case-study.md).

This is the first release of DeepSomatic. Properties such as runtime, accuracy across different sample preparations, and supported technologies will evolve with future releases. Your active feedback will help us prioritize use cases most important for the genomics community.

For details around runtime and accuracy expectations, please see the [DeepSomatic metrics page](docs/metrics.md).

NOTE: At this time, DeepSomatic has not been trained for or optimized for
FFPE-prepared samples. You will likely not be able to successfully run
FFPE-prepared data.

## How to run DeepSomatic

```bash
sudo docker run \
-v ${INPUT_DIR}:${INPUT_DIR} \
-v ${OUTPUT_DIR}:${OUTPUT_DIR} \
google/deepsomatic:"${BIN_VERSION}" \
run_deepsomatic \
--model_type=WGS \ ** Can be either WGS or PACBIO **
--ref=${INPUT_DIR}/REF.fasta \ **Path to reference fasta file.
--reads_normal=${INPUT_DIR}/normal.bam \ **Path to normal bam file.
--reads_tumor=${INPUT_DIR}/normal.bam \ * Path to tumor bam file.
--output_vcf=${OUTPUT_DIR}/OUTPUT.vcf.gz \ **Path to output VCF file.
--output_gvcf=${OUTPUT_DIR}/OUTPUT.g.vcf.gz \ **Path to output gVCF file.
--sample_name_tumor="tumor" \
--sample_name_normal="normal" \
--num_shards=$(nproc) \ **Total number of threads to use.
--logging_dir=${OUTPUT_DIR}/logs \ **Log output directory.
--intermediate_results_dir ${OUTPUT_DIR}/intermediate_results_dir \
--regions=chr1 **Region of the genome, if not provided then runs on whole genome \
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

In this example, the variant with `GERMLINE` INFO field is identified as a
germline variant, the one with `RefCall` is homozygous to the reference and
the variant with `PASS` in the info field is a somatic variant.

### Prerequisites

*   Unix-like operating system (cannot run on Windows)
*   Python 3.8

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
