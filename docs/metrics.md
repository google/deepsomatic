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

## WGS (Illumina)

Below are the numbers from an Illumina WGS run.

Dataset details:

```bash
Sample: HCC1395
Normal coverage: 50x
Tumor coverage: 60x
```

### Runtime

Runtime is  all chromosomes.
Reported runtime is an average of 5 runs.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 67m1.66s
call_variants                    | 212m35.57s
postprocess_variants (no gVCF)   | 1m5.76s
vcf_stats_report (optional)      | 3m6.48s
total                            | 293m38.21s (~4h53m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp   fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         1766   1515  251   111    0     0  0.931734      0.918703      0.943231  0.931734   0.857871         0.841004         0.873566   0          0      2875001522  0.087304
1     SNVs        39447        37909  37509  400  1938    0     0  0.950871      0.948705      0.952971  0.950871   0.989448         0.988382         0.990440   0          0      2875001522  0.139130
5  records        41073        39675  39024  651  2049    0     0  0.950113      0.947976      0.952187  0.950113   0.983592         0.982306         0.984807   0          0      2875001522  0.226435
```

## WES (Illumina)

Below are the numbers from an Illumina WES run.

Dataset details:

```bash
Sample: HCC1395
Normal coverage: 140x
Tumor coverage: 120x
```

### Runtime

Runtime is  all chromosomes.
Reported runtime is an average of 5 runs.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 9m17.72s
call_variants                    | 4m55.99s
postprocess_variants (no gVCF)   | 0m5.09s
vcf_stats_report (optional)      | 0m7.29s
total                            | 18m35.83s

### Accuracy

somp.py results

```
      type  total.truth  total.query    tp  fp  fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels           48           47    44   3   4    0     0  0.916667      0.813945      0.971215  0.916667   0.936170         0.839363         0.981696   0          0      2875001522  0.001043
1     SNVs         1159         1104  1094  10  65    0     0  0.943917      0.929551      0.956071  0.943917   0.990942         0.983992         0.995334   0          0      2875001522  0.003478
5  records         1207         1151  1138  13  69    0     0  0.942833      0.928664      0.954883  0.942833   0.988705         0.981310         0.993655   0          0      2875001522  0.004522
```

## PacBio

Below are the numbers from a PacBio run.

Dataset details:

```bash
Sample: HCC1395
Normal coverage: 45x
Tumor coverage: 60x
```

### Runtime

Runtime is  all chromosomes.
Reported runtime is an average of 5 runs.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 164m33.85s
call_variants                    | 160m15.53s
postprocess_variants (no gVCF)   | 1m46.21s
vcf_stats_report (optional)      | 4m39.58s
total                            | 340m38.03s (~5h40m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp    fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         1646   1298   348   328    0     0  0.798278      0.778253      0.817242  0.798278   0.788578         0.768356         0.807786   0          0      2875001522  0.121043
1     SNVs        39447        39090  37282  1808  2165    0     0  0.945116      0.942836      0.947331  0.945116   0.953748         0.951632         0.955796   0          0      2875001522  0.628869
5  records        41073        40736  38580  2156  2493    0     0  0.939303      0.936963      0.941581  0.939303   0.947074         0.944868         0.949216   0          0      2875001522  0.749913
```

## ONT

Below are the numbers from a ONT run.

Dataset details:

```bash
Sample: HCC1395
Normal coverage: 33x
Tumor coverage: 50x
```

### Runtime

Runtime is  all chromosomes.
Reported runtime is an average of 5 runs.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 120m50.85s
call_variants                    | 342m3.23s
postprocess_variants (no gVCF)   | 3m34.63s
vcf_stats_report (optional)      | 9m7.72s
total                            | 484m9.62s (~8h4m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp    fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         1240   1027   213   599    0     0  0.631611      0.607948      0.654806  0.631611   0.828226         0.806479         0.848442   0          0      2875001522  0.074087
1     SNVs        39447        31411  30603   808  8844    0     0  0.775800      0.771665      0.779896  0.775800   0.974277         0.972482         0.975984   0          0      2875001522  0.281043
5  records        41073        32651  31630  1021  9443    0     0  0.770092      0.766004      0.774142  0.770092   0.968730         0.966800         0.970576   0          0      2875001522  0.355130
```

## FFPE WGS

Below are the numbers from a FFPE run.

Dataset details:

```bash
Sample: HCC1395
Normal coverage: 50x
Tumor coverage: 90x
```

### Runtime

Runtime is  all chromosomes.
Reported runtime is an average of 5 runs.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 117m18.53s
call_variants                    | 411m54.19s
postprocess_variants (no gVCF)   | 2m5.70s
vcf_stats_report (optional)      | 6m7.85s
total                            | 548m32.26s (~9h8m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp    fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         1550   1288   262   338    0     0  0.792128      0.771896      0.811321  0.792128   0.830968         0.811701         0.849000   0          0      2875001522  0.091130
1     SNVs        39447        33333  32175  1158  7272    0     0  0.815651      0.811802      0.819455  0.815651   0.965260         0.963253         0.967185   0          0      2875001522  0.402782
5  records        41073        34883  33463  1420  7610    0     0  0.814720      0.810941      0.818455  0.814720   0.959292         0.957181         0.961328   0          0      2875001522  0.493913
```

## FFPE WES

Below are the numbers from a FFPE WES run.

Dataset details:

```bash
Sample: HCC1395
Normal coverage: 185x
Tumor coverage: 190x
```

### Runtime

Runtime is  all chromosomes.
Reported runtime is an average of 5 runs.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 14m26.69s
call_variants                    | 5m10.49s
postprocess_variants (no gVCF)   | 0m5.41s
vcf_stats_report (optional)      | 0m7.46s
total                            | 31m16.63s

### Accuracy

somp.py results

```
      type  total.truth  total.query   tp  fp   fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels           48           35   30   5   18    0     0  0.625000      0.484005      0.751421  0.625000   0.857143         0.714849         0.943328   0          0      2875001522  0.001739
1     SNVs         1159          962  937  25  222    0     0  0.808456      0.785047      0.830326  0.808456   0.974012         0.962490         0.982693   0          0      2875001522  0.008696
5  records         1207          997  967  30  240    0     0  0.801160      0.777938      0.822940  0.801160   0.969910         0.957906         0.979193   0          0      2875001522  0.010435
```

## WGS tumor-only

Below are the numbers from a WGS tumor-only run.

Dataset details:

```bash
Sample: HCC1395
Tumor coverage: 60x
```

### Runtime

Runtime is  all chromosomes.
Reported runtime is an average of 5 runs.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 36m20.64s
call_variants                    | 107m41.56s
postprocess_variants (no gVCF)   | 1m44.98s
vcf_stats_report (optional)      | 3m37.17s
total                            | 160m20.35s (~2h40m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp     fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         4315   1399   2916   227    0     0  0.860394      0.842912      0.876593  0.860394   0.324218         0.310373         0.338298   0          0      2875001522  1.014260
1     SNVs        39447        59386  36190  23196  3257    0     0  0.917434      0.914687      0.920119  0.917434   0.609403         0.605474         0.613321   0          0      2875001522  8.068170
5  records        41073        63698  37589  26109  3484    0     0  0.915175      0.912452      0.917841  0.915175   0.590113         0.586289         0.593928   0          0      2875001522  9.081386
```

## ONT tumor-only

Below are the numbers from a ONT tumor-only run.

Dataset details:

```bash
Sample: HCC1395
Tumor coverage: 50x
```

### Runtime

Runtime is  all chromosomes.
Reported runtime is an average of 5 runs.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 61m31.58s
call_variants                    | 240m40.88s
postprocess_variants (no gVCF)   | 4m23.34s
vcf_stats_report (optional)      | 10m16.97s
total                            | 327m5.35s (~3h27m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp     fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         2165    945   1220   681    0     0  0.581181      0.557074      0.604999  0.581181   0.436490         0.415695         0.457454   0          0      2875001522  0.424348
1     SNVs        39447        51371  30570  20801  8877    0     0  0.774964      0.770823      0.779065  0.774964   0.595083         0.590833         0.599322   0          0      2875001522  7.235127
5  records        41073        53535  31515  22020  9558    0     0  0.767292      0.763187      0.771360  0.767292   0.588680         0.584507         0.592844   0          0      2875001522  7.659126
```

## PacBio tumor-only

Below are the numbers from a PacBio tumor-only run.

Dataset details:

```bash
Sample: HCC1395
Tumor coverage: 60x
```

### Runtime

Runtime is  all chromosomes.
Reported runtime is an average of 5 runs.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 89m18.49s
call_variants                    | 133m52.69s
postprocess_variants (no gVCF)   | 2m54.10s
vcf_stats_report (optional)      | 6m10.06s
total                            | 242m29.53s (~4h2m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp     fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         2379   1278   1101   348    0     0  0.785978      0.765545      0.805394  0.785978   0.537201         0.517129         0.557181   0          0      2875001522  0.382956
1     SNVs        39447        56874  37768  19106  1679    0     0  0.957437      0.955411      0.959395  0.957437   0.664064         0.660174         0.667938   0          0      2875001522  6.645562
5  records        41073        59253  39046  20207  2027    0     0  0.950649      0.948522      0.952712  0.950649   0.658971         0.655146         0.662780   0          0      2875001522  7.028518
```

## How to reproduce the metrics on this page

For simplicity and consistency, we report runtime with a
[CPU instance with 96 CPUs](https://github.com/google/deepvariant/blob/r1.8/docs/deepvariant-details.md#command-for-a-cpu-only-machine-on-google-cloud-platform)
This is NOT the fastest or cheapest configuration.

Use `gcloud compute ssh` to log in to the newly created instance.

Download and run any of the following case study scripts:

```
# Get the script.
curl -O https://raw.githubusercontent.com/google/deepvariant/r1.8/scripts/inference_deepsomatic.sh

# WGS
bash inference_deepsomatic.sh --model_preset WGS

# PACBIO
bash inference_deepsomatic.sh --model_preset PACBIO

# ONT
bash inference_deepsomatic.sh --model_preset ONT

# FFPE_WGS
bash inference_deepsomatic.sh --model_preset FFPE_WGS

# FFPE_WES
bash inference_deepsomatic.sh --model_preset FFPE_WES

# WGS_TUMOR_ONLY
bash inference_deepsomatic.sh --model_preset WGS_TUMOR_ONLY

# PACBIO_TUMOR_ONLY
bash inference_deepsomatic.sh --model_preset PACBIO_TUMOR_ONLY

# ONT_TUMOR_ONLY
bash inference_deepsomatic.sh --model_preset ONT_TUMOR_ONLY
```

Runtime metrics are taken from the resulting log after each stage of
DeepSomatic.

The accuracy metrics came from the som.py extension of hap.py program.
