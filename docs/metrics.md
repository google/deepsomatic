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

Runtime is all chromosomes.
Reported runtime is an average of 5 runs.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 64m30.62s
call_variants                    | 106m0.25s
postprocess_variants (no gVCF)   | 0m59.07s
vcf_stats_report (optional)      | 3m0.84s
total                            | 185m49.59s (~3h5m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp   fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         1782   1512  270   114    0     0  0.929889      0.916710      0.941541  0.929889   0.848485         0.831278         0.864561   0          0      2875001522  0.093913
1     SNVs        39447        37921  37500  421  1947    0     0  0.950643      0.948472      0.952747  0.950643   0.988898         0.987806         0.989916   0          0      2875001522  0.146435
5  records        41073        39703  39012  691  2061    0     0  0.949821      0.947678      0.951901  0.949821   0.982596         0.981274         0.983847   0          0      2875001522  0.240348
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

Runtime is all chromosomes.
Reported runtime is an average of 5 runs.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 8m17.49s
call_variants                    | 2m37.36s
postprocess_variants (no gVCF)   | 0m5.78s
vcf_stats_report (optional)      | 0m7.25s
total                            | 15m33.09s

### Accuracy

somp.py results

```
      type  total.truth  total.query    tp  fp  fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels           48           46    43   3   5    0     0  0.895833      0.786676      0.959125  0.895833   0.934783         0.836081         0.981291   0          0      2875001522  0.001043
1     SNVs         1159         1104  1094  10  65    0     0  0.943917      0.929551      0.956071  0.943917   0.990942         0.983992         0.995334   0          0      2875001522  0.003478
5  records         1207         1150  1137  13  70    0     0  0.942005      0.927749      0.954145  0.942005   0.988696         0.981294         0.993649   0          0      2875001522  0.004522
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

Runtime is all chromosomes.
Reported runtime is an average of 5 runs.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 198m23.38s
call_variants                    | 120m49.79s
postprocess_variants (no gVCF)   | 1m36.30s
vcf_stats_report (optional)      | 4m33.57s
total                            | 334m57.98s (~5h34m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp    fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         1637   1315   322   311    0     0  0.808733      0.789077      0.827291  0.808733   0.803299         0.783517         0.822009   0          0      2875001522  0.112000
1     SNVs        39447        38798  37236  1562  2211    0     0  0.943950      0.941648      0.946187  0.943950   0.959740         0.957750         0.961662   0          0      2875001522  0.543304
5  records        41073        40435  38551  1884  2522    0     0  0.938597      0.936244      0.940888  0.938597   0.953407         0.951320         0.955429   0          0      2875001522  0.655304
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

Runtime is all chromosomes.
Reported runtime is an average of 5 runs.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 121m23.55s
call_variants                    | 167m49.86s
postprocess_variants (no gVCF)   | 3m12.58s
vcf_stats_report (optional)      | 8m52.38s
total                            | 310m30.69s (~5h10m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp    fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         1260   1035   225   591    0     0  0.636531      0.612926      0.659651  0.636531   0.821429         0.799557         0.841826   0          0      2875001522  0.078261
1     SNVs        39447        31433  30595   838  8852    0     0  0.775598      0.771461      0.779694  0.775598   0.973340         0.971515         0.975078   0          0      2875001522  0.291478
5  records        41073        32693  31630  1063  9443    0     0  0.770092      0.766004      0.774142  0.770092   0.967485         0.965521         0.969367   0          0      2875001522  0.369739
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

Runtime is all chromosomes.
Reported runtime is an average of 5 runs.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 116m2.26s
call_variants                    | 252m45.09s
postprocess_variants (no gVCF)   | 2m10.30s
vcf_stats_report (optional)      | 7m8.43s
total                            | 389m7.11s (~6h29m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp    fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         1645   1300   345   326    0     0  0.799508      0.779525      0.818426  0.799508   0.790274         0.770100         0.809426   0          0      2875001522  0.120000
1     SNVs        39447        34094  32226  1868  7221    0     0  0.816944      0.813105      0.820737  0.816944   0.945210         0.942757         0.947588   0          0      2875001522  0.649739
5  records        41073        35739  33526  2213  7547    0     0  0.816254      0.812486      0.819977  0.816254   0.938079         0.935545         0.940542   0          0      2875001522  0.769739
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

Runtime is all chromosomes.
Reported runtime is an average of 5 runs.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 14m7.55s
call_variants                    | 3m39.45s
postprocess_variants (no gVCF)   | 0m6.25s
vcf_stats_report (optional)      | 0m8.99s
total                            | 29m38.02s

### Accuracy

somp.py results

```
      type  total.truth  total.query   tp  fp   fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels           48           47   40   7    8    0     0  0.833333      0.710001      0.917913  0.833333   0.851064         0.729682         0.930824   0          0      2875001522  0.002435
1     SNVs         1159          990  956  34  203    0     0  0.824849      0.802170      0.845908  0.824849   0.965657         0.952920         0.975678   0          0      2875001522  0.011826
5  records         1207         1037  996  41  211    0     0  0.825186      0.802994      0.845822  0.825186   0.960463         0.947293         0.971069   0          0      2875001522  0.014261
```

## WGS tumor-only

Below are the numbers from a WGS tumor-only run.

Dataset details:

```bash
Sample: HCC1395
Tumor coverage: 60x
```

### Runtime

Runtime is all chromosomes.
Reported runtime is an average of 5 runs.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 37m36.77s
call_variants                    | 54m41.93s
postprocess_variants (no gVCF)   | 1m37.85s
vcf_stats_report (optional)      | 3m33.53s
total                            | 108m19.06s (~1h48m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp     fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         3794   1381   2413   245    0     0  0.849323      0.831321      0.866084  0.849323   0.363996         0.348794         0.379405   0          0      2875001522  0.839304
1     SNVs        39447        44623  35663   8960  3784    0     0  0.904074      0.901138      0.906950  0.904074   0.799207         0.795471         0.802904   0          0      2875001522  3.116520
5  records        41073        48416  37044  11372  4029    0     0  0.901906      0.899001      0.904755  0.901906   0.765119         0.761327         0.768879   0          0      2875001522  3.955476
```

## PacBio tumor-only

Below are the numbers from a PacBio tumor-only run.

Dataset details:

```bash
Sample: HCC1395
Tumor coverage: 60x
```

### Runtime

Runtime is all chromosomes.
Reported runtime is an average of 5 runs.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 102m3.48s
call_variants                    | 52m56.60s
postprocess_variants (no gVCF)   | 2m39.18s
vcf_stats_report (optional)      | 6m2.34s
total                            | 173m50.57s (~2h52m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp     fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         2379   1278   1101   348    0     0  0.785978      0.765545      0.805394  0.785978   0.537201         0.517129         0.557181   0          0      2875001522  0.382956
1     SNVs        39447        56874  37768  19106  1679    0     0  0.957437      0.955411      0.959395  0.957437   0.664064         0.660174         0.667938   0          0      2875001522  6.645562
5  records        41073        59253  39046  20207  2027    0     0  0.950649      0.948522      0.952712  0.950649   0.658971         0.655146         0.662780   0          0      2875001522  7.028518
```

## ONT tumor-only

Below are the numbers from a ONT tumor-only run.

Dataset details:

```bash
Sample: HCC1395
Tumor coverage: 50x
```

### Runtime

Runtime is all chromosomes.
Reported runtime is an average of 5 runs.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 67m22.21s
call_variants                    | 95m5.18s
postprocess_variants (no gVCF)   | 4m4.65s
vcf_stats_report (optional)      | 10m15.89s
total                            | 186m11.42s (~3h6m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp     fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         2165    945   1220   681    0     0  0.581181      0.557074      0.604999  0.581181   0.436490         0.415695         0.457454   0          0      2875001522  0.424348
1     SNVs        39447        51371  30570  20801  8877    0     0  0.774964      0.770823      0.779065  0.774964   0.595083         0.590833         0.599322   0          0      2875001522  7.235127
5  records        41073        53535  31515  22020  9558    0     0  0.767292      0.763187      0.771360  0.767292   0.588680         0.584507         0.592844   0          0      2875001522  7.659126
```

## FFPE WGS tumor-only

Below are the numbers from a FFPE WGS tumor-only run.

Dataset details:

```bash
Sample: HCC1395
Tumor coverage: 90x
```

### Runtime

Runtime is all chromosomes.
Reported runtime is an average of 5 runs.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 66m31.13s
call_variants                    | 67m8.20s
postprocess_variants (no gVCF)   | 1m52.78s
vcf_stats_report (optional)      | 4m10.71s
total                            | 150m42.14s (~2h30m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp    fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         1960   1193   767   433    0     0  0.733702      0.711816      0.754758  0.733702   0.608673         0.586920         0.630107   0          0      2875001522  0.266782
1     SNVs        39447        37785  30686  7099  8761    0     0  0.777905      0.773782      0.781986  0.777905   0.812121         0.808159         0.816036   0          0      2875001522  2.469216
5  records        41073        39744  31879  7865  9194    0     0  0.776155      0.772104      0.780166  0.776155   0.802108         0.798170         0.806003   0          0      2875001522  2.735651
```

## FFPE WES tumor-only

Below are the numbers from a FFPE WES tumor-only run.

Dataset details:

```bash
Sample: HCC1395
Tumor coverage: 190x
```

### Runtime

Runtime is all chromosomes.
Reported runtime is an average of 5 runs.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 5m51.71s
call_variants                    | 1m12.73s
postprocess_variants (no gVCF)   | 0m6.74s
vcf_stats_report (optional)      | 0m7.25s
total                            | 11m3.69s

### Accuracy

somp.py results

```
      type  total.truth  total.query   tp   fp   fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels           48          107   41   66    7    0     0  0.854167      0.734870      0.932321  0.854167   0.383178         0.295172         0.477408   0          0      2875001522  0.022957
1     SNVs         1159         1225  921  304  238    0     0  0.794651      0.770677      0.817155  0.794651   0.751837         0.727073         0.775412   0          0      2875001522  0.105739
5  records         1207         1332  962  370  245    0     0  0.797017      0.773631      0.818981  0.797017   0.722222         0.697705         0.745775   0          0      2875001522  0.128696
```

## How to reproduce the metrics on this page

For simplicity and consistency, we report runtime with a
[CPU instance with 96 CPUs](https://github.com/google/deepvariant/blob/r1.9/docs/deepvariant-details.md#command-for-a-cpu-only-machine-on-google-cloud-platform)
This is NOT the fastest or cheapest configuration.

Use `gcloud compute ssh` to log in to the newly created instance.

Download and run any of the following case study scripts:

```
# Get the script.
curl -O https://raw.githubusercontent.com/google/deepvariant/r1.9/scripts/inference_deepsomatic.sh

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
