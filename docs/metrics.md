# Runtime and accuracy metrics for all release models

## Setup

The runtime and accuracy reported in this page are generated using
`n2-standard-96` GCP instances which has the following configuration:

```bash
GCP instance type: n2-standard-96
CPUs: 96-core (vCPU)
Memory: 384GiB
GPUs: 0
```

Details of metrics can be found here:

* [Sample sheet](https://storage.googleapis.com/deepvariant/deepsomatic-case-study-outputs/1.10.0/deepsomatic_case_study_summaries/sample_sheet.tsv)
* [Runtime summary report](https://storage.googleapis.com/deepvariant/deepsomatic-case-study-outputs/1.10.0/deepsomatic_case_study_summaries/runtimes.md)
* [Accuracy summary report](https://storage.googleapis.com/deepvariant/deepsomatic-case-study-outputs/1.10.0/deepsomatic_case_study_summaries/sompy.summary.md)

Sample sheet contains details of the input files used to generate this report.

Note: Each model type uses different coverages.

## Accuracy

### Tumor-normal accuracy

| Model type          | sample   | type   |   total.truth |   total.query |    tp |    fp |   fn |   precision |   recall |   f1_score |
|:--------------------|:---------|:-------|--------------:|--------------:|------:|------:|-----:|------------:|---------:|-----------:|
| wgs                 | HCC1395  | SNVs   |         39447 |         38070 | 37653 |   417 | 1794 |    0.989046 | 0.954521 |   0.971477 |
| wgs                 | HCC1395  | indels |          1626 |          1727 |  1514 |   213 |  112 |    0.876665 | 0.931119 |   0.903072 |
| wes                 | HCC1395  | SNVs   |          1159 |          1104 |  1094 |    10 |   65 |    0.990942 | 0.943917 |   0.966858 |
| wes                 | HCC1395  | indels |            48 |            45 |    42 |     3 |    6 |    0.933333 | 0.875    |   0.903226 |
| pacbio              | HCC1395  | SNVs   |         39447 |         38517 | 37001 |  1516 | 2446 |    0.960641 | 0.937993 |   0.949182 |
| pacbio              | HCC1395  | indels |          1626 |          1620 |  1314 |   306 |  312 |    0.811111 | 0.808118 |   0.809612 |
| ont                 | HCC1395  | SNVs   |         39447 |         31582 | 30605 |   977 | 8842 |    0.969065 | 0.775851 |   0.861761 |
| ont                 | HCC1395  | indels |          1626 |          1262 |  1039 |   223 |  587 |    0.823296 | 0.638991 |   0.719529 |
| ffpe-wgs            | HCC1395  | SNVs   |         39447 |         34145 | 32250 |  1895 | 7197 |    0.944501 | 0.817553 |   0.876454 |
| ffpe-wgs            | HCC1395  | indels |          1626 |          1612 |  1267 |   345 |  359 |    0.78598  | 0.779213 |   0.782582 |
| ffpe-wes            | HCC1395  | SNVs   |          1159 |           990 |   956 |    34 |  203 |    0.965657 | 0.824849 |   0.889716 |
| ffpe-wes            | HCC1395  | indels |            48 |            47 |    40 |     7 |    8 |    0.851064 | 0.833333 |   0.842105 |

### Tumor-only accuracy

| Model type          | sample   | type   |   total.truth |   total.query |    tp |    fp |   fn |   precision |   recall |   f1_score |
|:--------------------|:---------|:-------|--------------:|--------------:|------:|------:|-----:|------------:|---------:|-----------:|
| wgs-tumor-only      | HCC1395  | SNVs   |         39447 |         44624 | 35663 |  8961 | 3784 |    0.799189 | 0.904074 |   0.848402 |
| wgs-tumor-only      | HCC1395  | indels |          1626 |          3772 |  1379 |  2393 |  247 |    0.365589 | 0.848093 |   0.51093  |
| wes-tumor-only      | HCC1395  | SNVs   |          1159 |           987 |   948 |    39 |  211 |    0.960486 | 0.817947 |   0.883504 |
| wes-tumor-only      | HCC1395  | indels |            48 |            55 |    41 |    14 |    7 |    0.745455 | 0.854167 |   0.796117 |
| pacbio-tumor-only   | HCC1395  | SNVs   |         39447 |         56068 | 37263 | 18805 | 2184 |    0.664604 | 0.944635 |   0.780254 |
| pacbio-tumor-only   | HCC1395  | indels |          1626 |          2223 |  1232 |   991 |  394 |    0.554206 | 0.757688 |   0.640166 |
| ont-tumor-only      | HCC1395  | SNVs   |         39447 |         51417 | 30553 | 20864 | 8894 |    0.59422  | 0.774533 |   0.6725   |
| ont-tumor-only      | HCC1395  | indels |          1626 |          2088 |   933 |  1155 |  693 |    0.446839 | 0.573801 |   0.502423 |
| ffpe-wgs-tumor-only | HCC1395  | SNVs   |         39447 |         37784 | 30686 |  7098 | 8761 |    0.812143 | 0.777905 |   0.794655 |
| ffpe-wgs-tumor-only | HCC1395  | indels |          1626 |          1949 |  1190 |   759 |  436 |    0.61057  | 0.731857 |   0.665734 |
| ffpe-wes-tumor-only | HCC1395  | SNVs   |          1159 |          1225 |   921 |   304 |  238 |    0.751837 | 0.794651 |   0.772651 |
| ffpe-wes-tumor-only | HCC1395  | indels |            48 |           106 |    40 |    66 |    8 |    0.377358 | 0.833333 |   0.519481 |

## Runtime

Each case study was run 5x times and the runtimes were averaged.

| Model type          | sample   | mean runtime    |
|:--------------------|:---------|:----------------|
| wgs                 | HCC1395  | 2h 14m 41s      |
| wes                 | HCC1395  | 10m 47s         |
| pacbio              | HCC1395  | 3h 36m 18s      |
| ont                 | HCC1395  | 4h 6m 46s       |
| ffpe-wgs            | HCC1395  | 5h 49m          |
| ffpe-wes            | HCC1395  | 17m 40s         |
| wgs-tumor-only      | HCC1395  | 1h 30m 30s      |
| wes-tumor-only      | HCC1395  | 5m 58s          |
| pacbio-tumor-only   | HCC1395  | 2h 23m 52s      |
| ont-tumor-only      | HCC1395  | 2h 39m 56s      |
| ffpe-wes-tumor-only | HCC1395  | 7m 9s           |
| ffpe-wgs-tumor-only | HCC1395  | 2h 8m 54s       |

## How to reproduce the metrics on this page

For simplicity and consistency, we report runtime with a
[CPU instance with 96 CPUs](https://github.com/google/deepvariant/blob/r1.10/docs/deepvariant-details.md#command-for-a-cpu-only-machine-on-google-cloud-platform)
This is NOT the fastest or cheapest configuration.

Use `gcloud compute ssh` to log in to the newly created instance.

Download and run any of the following case study scripts:

```
# Get the script.
curl -O https://raw.githubusercontent.com/google/deepvariant/r1.10/scripts/inference_deepsomatic.sh

# WGS
bash inference_deepsomatic.sh --model_preset WGS

# WES
bash inference_deepsomatic.sh --model_preset WES

# PACBIO
bash inference_deepsomatic.sh --model_preset PACBIO

# ONT
bash inference_deepsomatic.sh --model_preset ONT

# FFPE_WGS
bash inference_deepsomatic.sh --model_preset FFPE_WGS

# FFPE_WES
bash inference_deepsomatic.sh --model_preset FFPE_WES

# WGS_TUMOR_ONLY
bash inference_deepsomatic.sh --model_preset WGS_TUMOR_ONLY --use_default_pon_filtering

# WES_TUMOR_ONLY
bash inference_deepsomatic.sh --model_preset WES_TUMOR_ONLY --use_default_pon_filtering

# PACBIO_TUMOR_ONLY
bash inference_deepsomatic.sh --model_preset PACBIO_TUMOR_ONLY --use_default_pon_filtering

# ONT_TUMOR_ONLY
bash inference_deepsomatic.sh --model_preset ONT_TUMOR_ONLY --use_default_pon_filtering

# FFPE_WGS_TUMOR_ONLY
bash inference_deepsomatic.sh --model_preset FFPE_WGS_TUMOR_ONLY --use_default_pon_filtering

# FFPE_WES_TUMOR_ONLY
bash inference_deepsomatic.sh --model_preset FFPE_WES_TUMOR_ONLY --use_default_pon_filtering
```

Runtime metrics are taken from the resulting log after each stage of
DeepSomatic.

The accuracy metrics came from the som.py extension of hap.py program.
