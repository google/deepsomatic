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
make_examples_somatic            | 65m47.98s
call_variants                    | 104m40.07s
postprocess_variants (no gVCF)   | 1m25.83s
vcf_stats_report (optional)      | 3m10.37s
total                            | 186m24.69s (~3h7m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp   fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         1734   1518  216   108    0     0  0.933579      0.920699      0.944918  0.933579   0.875433         0.859267         0.890346   0          0      2875001522  0.075130
1     SNVs        39447        38076  37657  419  1790    0     0  0.954623      0.952535      0.956643  0.954623   0.988996         0.987910         0.990007   0          0      2875001522  0.145739
5  records        41073        39810  39175  635  1898    0     0  0.953790      0.951727      0.955788  0.953790   0.984049         0.982783         0.985245   0          0      2875001522  0.220869
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
make_examples_somatic            | 8m25.59s
call_variants                    | 2m36.78s
postprocess_variants (no gVCF)   | 0m6.54s
vcf_stats_report (optional)      | 0m7.69s
total                            | 15m36.85s

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
make_examples_somatic            | 192m37.10s
call_variants                    | 118m8.43s
postprocess_variants (no gVCF)   | 2m31.22s
vcf_stats_report (optional)      | 4m51.46s
total                            | 327m35.66s (~5h27m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp    fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         1627   1316   311   310    0     0  0.809348      0.789714      0.827882  0.809348   0.808851         0.789205         0.827399   0          0      2875001522  0.108174
1     SNVs        39447        38815  37220  1595  2227    0     0  0.943545      0.941234      0.945790  0.943545   0.958908         0.956899         0.960848   0          0      2875001522  0.554782
5  records        41073        40442  38536  1906  2537    0     0  0.938232      0.935873      0.940529  0.938232   0.952871         0.950773         0.954904   0          0      2875001522  0.662956
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
make_examples_somatic            | 121m32.42s
call_variants                    | 157m20.84s
postprocess_variants (no gVCF)   | 4m58.46s
vcf_stats_report (optional)      | 9m26.86s
total                            | 302m22.28s (~5h03m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp    fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         1270   1042   228   584    0     0  0.640836      0.617284      0.663888  0.640836   0.820472         0.798648         0.840838   0          0      2875001522  0.079304
1     SNVs        39447        31584  30590   994  8857    0     0  0.775471      0.771333      0.779568  0.775471   0.968528         0.966560         0.970411   0          0      2875001522  0.345739
5  records        41073        32854  31632  1222  9441    0     0  0.770141      0.766053      0.774191  0.770141   0.962805         0.960718         0.964811   0          0      2875001522  0.425043
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
make_examples_somatic            | 117m36.65s
call_variants                    | 249m54.32s
postprocess_variants (no gVCF)   | 3m11.81s
vcf_stats_report (optional)      | 7m37.80s
total                            | 389m27.44s (~6h30m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp   fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         1493   1334  159   292    0     0  0.820418      0.801200      0.838497  0.820418   0.893503         0.877096         0.908387   0          0      2875001522  0.055304
1     SNVs        39447        33102  32752  350  6695    0     0  0.830279      0.826550      0.833959  0.830279   0.989427         0.988282         0.990486   0          0      2875001522  0.121739
5  records        41073        34594  34086  508  6987    0     0  0.829888      0.826231      0.833499  0.829888   0.985315         0.984007         0.986543   0          0      2875001522  0.176696
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
make_examples_somatic            | 14m5.02s
call_variants                    | 3m41.74s
postprocess_variants (no gVCF)   | 0m7.02s
vcf_stats_report (optional)      | 0m9.41s
total                            | 29m20.69s

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
make_examples_somatic            | 36m39.15s
call_variants                    | 52m18.94s
postprocess_variants (no gVCF)   | 1m45.62s
vcf_stats_report (optional)      | 3m34.53s
total                            | 105m8.58s (~1h45m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp     fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         3794   1381   2413   245    0     0  0.849323      0.831321      0.866084  0.849323   0.363996         0.348794         0.379405   0          0      2875001522  0.839304
1     SNVs        39447        44623  35663   8960  3784    0     0  0.904074      0.901138      0.906950  0.904074   0.799207         0.795471         0.802904   0          0      2875001522  3.116520
5  records        41073        48416  37044  11372  4029    0     0  0.901906      0.899001      0.904755  0.901906   0.765119         0.761327         0.768879   0          0      2875001522  3.955476
```

## WES tumor-only

Below are the numbers from a WES tumor-only run.

Dataset details:

```bash
Sample: HCC1395
Tumor coverage: 120x
```

### Runtime

Runtime is all chromosomes.
Reported runtime is an average of 5 runs.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 4m40.28s
call_variants                    | 1m7.04s
postprocess_variants (no gVCF)   | 0m6.64s
vcf_stats_report (optional)      | 0m6.95s
total                            | 7m50.40s

### Accuracy

somp.py results

```
      type  total.truth  total.query   tp  fp   fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels           48           55   41  14    7    0     0  0.854167      0.734870      0.932321  0.854167   0.745455         0.619857         0.845991   0          0      2875001522  0.004870
1     SNVs         1159          987  948  39  211    0     0  0.817947      0.794952      0.839355  0.817947   0.960486         0.946956         0.971323   0          0      2875001522  0.013565
5  records         1207         1042  989  53  218    0     0  0.819387      0.796933      0.840311  0.819387   0.949136         0.934532         0.961252   0          0      2875001522  0.018435
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
make_examples_somatic            | 84m41.71s
call_variants                    | 48m33.46s
postprocess_variants (no gVCF)   | 2m58.36s
vcf_stats_report (optional)      | 6m5.78s
total                            | 152m29.16s (~2h32m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp     fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         2370   1265   1105   361    0     0  0.777983      0.757299      0.797678  0.777983   0.533755         0.513640         0.553788   0          0      2875001522  0.384348
1     SNVs        39447        56824  37655  19169  1792    0     0  0.954572      0.952484      0.956594  0.954572   0.662660         0.658765         0.666539   0          0      2875001522  6.667475
5  records        41073        59194  38920  20274  2153    0     0  0.947581      0.945394      0.949705  0.947581   0.657499         0.653669         0.661314   0          0      2875001522  7.051822
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
make_examples_somatic            | 63m4.31s
call_variants                    | 86m19.76s
postprocess_variants (no gVCF)   | 4m40.79s
vcf_stats_report (optional)      | 10m8.17s
total                            | 174m24.44s (~2h56m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp     fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         2206    930   1276   696    0     0  0.571956      0.547796      0.595860  0.571956   0.421578         0.401085         0.442276   0          0      2875001522  0.443826
1     SNVs        39447        51508  30603  20905  8844    0     0  0.775800      0.771665      0.779896  0.775800   0.594141         0.589895         0.598376   0          0      2875001522  7.271300
5  records        41073        53713  31533  22180  9540    0     0  0.767731      0.763628      0.771796  0.767731   0.587065         0.582896         0.591224   0          0      2875001522  7.714779
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
make_examples_somatic            | 61m58.86s
call_variants                    | 60m59.69s
postprocess_variants (no gVCF)   | 1m59.27s
vcf_stats_report (optional)      | 4m6.96s
total                            | 140m2.93s (~2h20m)

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
make_examples_somatic            | 5m46.29s
call_variants                    | 1m11.14s
postprocess_variants (no gVCF)   | 0m6.79s
vcf_stats_report (optional)      | 0m7.21s
total                            | 10m30.62s

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
