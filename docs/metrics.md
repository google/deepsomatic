# Runtime and accuracy metrics for all release models

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
make_examples_somatic            | 91m34.23s
call_variants                    | 288m15.12s
postprocess_variants (no gVCF)   | 4m52.52s
vcf_stats_report (optional)      | 1m27.04s
total                            | 397m55.94s (~6h37m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp   fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         1771   1497  274   129    0     0  0.920664      0.906777      0.933056  0.920664   0.845285         0.827885         0.861558   0          0      2875001522  0.095304
1     SNVs        39447        37783  37428  355  2019    0     0  0.948817      0.946610      0.950959  0.948817   0.990604         0.989593         0.991540   0          0      2875001522  0.123478
5  records        41073        39554  38925  629  2148    0     0  0.947703      0.945518      0.949824  0.947703   0.984098         0.982829         0.985295   0          0      2875001522  0.218782
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
make_examples_somatic            | 13m21.19s
call_variants                    | 6m38.14s
postprocess_variants (no gVCF)   | 0m11.24s
vcf_stats_report (optional)      | 0m13.06s
total                            | 24m0.70s

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
make_examples_somatic            | 236m38.41s
call_variants                    | 208m35.56s
postprocess_variants (no gVCF)   | 7m17.39s
vcf_stats_report (optional)      | 0m9.94s
total                            | 464m39.66s (~7h43m)

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
make_examples_somatic            | 172m36.48s
call_variants                    | 454m14.42s
postprocess_variants (no gVCF)   | 14m38.69s
vcf_stats_report (optional)      | 0m10.24s
total                            | 655m4.48s (~10h55m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp    fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         1242   1027   215   599    0     0  0.631611      0.607948      0.654806  0.631611   0.826892         0.805103         0.847160   0          0      2875001522  0.074783
1     SNVs        39447        31411  30603   808  8844    0     0  0.775800      0.771665      0.779896  0.775800   0.974277         0.972482         0.975984   0          0      2875001522  0.281043
5  records        41073        32653  31630  1023  9443    0     0  0.770092      0.766004      0.774142  0.770092   0.968671         0.966739         0.970519   0          0      2875001522  0.355826
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
make_examples_somatic            | 165m18.70s
call_variants                    | 617m2.60s
postprocess_variants (no gVCF)   | 8m25.83s
vcf_stats_report (optional)      | 1m33.19s
total                            | 804m31.57s (~13h24m)

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
make_examples_somatic            | 21m15.66s
call_variants                    | 7m20.05s
postprocess_variants (no gVCF)   | 0m10.33s
vcf_stats_report (optional)      | 0m12.43s
total                            | 39m46.02s

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
make_examples_somatic            | 51m30.88s
call_variants                    | 147m58.64s
postprocess_variants (no gVCF)   | 5m23.37s
vcf_stats_report (optional)      | 1m18.62s
total                            | 218m9.07s (~3h38m)

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
make_examples_somatic            | 91m52.49s
call_variants                    | 381m14.36s
postprocess_variants (no gVCF)   | 14m3.23s
vcf_stats_report (optional)      | 0m8.63s
total                            | 502m58.95s (~8h23m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp     fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         2157    945   1212   681    0     0  0.581181      0.557074      0.604999  0.581181   0.438108         0.417265         0.459118   0          0      2875001522  0.421565
1     SNVs        39447        51371  30570  20801  8877    0     0  0.774964      0.770823      0.779065  0.774964   0.595083         0.590833         0.599322   0          0      2875001522  7.235127
5  records        41073        53527  31515  22012  9558    0     0  0.767292      0.763187      0.771360  0.767292   0.588768         0.584595         0.592932   0          0      2875001522  7.656344
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
make_examples_somatic            | 118m19.70s
call_variants                    | 215m30.22s
postprocess_variants (no gVCF)   | 8m30.11s
vcf_stats_report (optional)      | 0m8.00s
total                            | 355m54.65s (~5h55m)

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp     fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         2365   1278   1087   348    0     0  0.785978      0.765545      0.805394  0.785978   0.540381         0.520256         0.560406   0          0      2875001522  0.378087
1     SNVs        39447        56874  37768  19106  1679    0     0  0.957437      0.955411      0.959395  0.957437   0.664064         0.660174         0.667938   0          0      2875001522  6.645562
5  records        41073        59239  39046  20193  2027    0     0  0.950649      0.948522      0.952712  0.950649   0.659127         0.655302         0.662936   0          0      2875001522  7.023648
```

## How to reproduce the metrics on this page

For simplicity and consistency, we report runtime with a
[CPU instance with 96 CPUs](https://github.com/google/deepvariant/blob/r1.7/docs/deepvariant-details.md#command-for-a-cpu-only-machine-on-google-cloud-platform)
This is NOT the fastest or cheapest configuration.

Use `gcloud compute ssh` to log in to the newly created instance.

Download and run any of the following case study scripts:

```
# Get the script.
curl -O https://raw.githubusercontent.com/google/deepvariant/r1.7/scripts/inference_deepsomatic.sh

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
