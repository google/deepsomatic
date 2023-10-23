# Runtime and accuracy metrics for all release models

## WGS (Illumina)

Below are the numbers from an Illumina WGS run on whole genome.

Dataset details:

```bash
Sample: HCC1395
Normal coverage: 50x
Tumor coverage: 58x
```

### Runtime

Runtime is  all chromosomes.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 385m39.963s
call_variants                    | 423m13.271s
postprocess_variants (with gVCF) | 26m46.040s
total                            | ~835m = ~13.92h

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp   fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         1769   1500  269   126    0     0  0.922509      0.908759      0.934757  0.922509   0.847937         0.830641         0.864095   0          0      2875001522  0.093565
1     SNVs        39447        37814  37450  364  1997    0     0  0.949375      0.947179      0.951506  0.949375   0.990374         0.989352         0.991321   0          0      2875001522  0.126609
5  records        41073        39583  38950  633  2123    0     0  0.948312      0.946139      0.950421  0.948312   0.984008         0.982737         0.985209   0          0      2875001522  0.220174
```

## PacBio

Below are the numbers from a PacBio run on whole genome.

Dataset details:

```bash
Sample: HCC1395
Normal coverage: 48x
Tumor coverage: 58x
```

### Runtime

Runtime is  all chromosomes.

Stage                            | Time (wall time)
-------------------------------- | ------------------
make_examples_somatic            | 586m0.088s
call_variants                    | 409m54.403s
postprocess_variants (with gVCF) | 46m39.075s
total                            | ~1043m = ~17.38h

### Accuracy

somp.py results

```
      type  total.truth  total.query     tp    fp    fn  unk  ambi    recall  recall_lower  recall_upper   recall2  precision  precision_lower  precision_upper  na  ambiguous  fp.region.size   fp.rate
0   indels         1626         1483   1226   257   400    0     0  0.753998      0.732625      0.774467  0.753998   0.826703         0.806811         0.845320   0          0      2875001522  0.089391
1     SNVs        39447        38389  36992  1397  2455    0     0  0.937765      0.935348      0.940117  0.937765   0.963609         0.961701         0.965448   0          0      2875001522  0.485913
5  records        41073        39872  38218  1654  2855    0     0  0.930490      0.928000      0.932919  0.930490   0.958517         0.956527         0.960441   0          0      2875001522  0.575304
```

## How to reproduce the metrics on this page

For simplicity and consistency, we report runtime with a
[CPU instance with 64 CPUs](https://github.com/google/deepvariant/blob/r1.6/docs/deepvariant-details.md#command-for-a-cpu-only-machine-on-google-cloud-platform)
This is NOT the fastest or cheapest configuration.

Use `gcloud compute ssh` to log in to the newly created instance.

Download and run any of the following case study scripts:

```
# Get the script.
curl -O https://raw.githubusercontent.com/google/deepvariant/r1.6/scripts/inference_deepsomatic.sh

# WGS
bash inference_deepsomatic.sh --model_preset WGS

# PACBIO
bash inference_deepsomatic.sh --model_preset PACBIO
```

Runtime metrics are taken from the resulting log after each stage of
DeepSomatic.

The accuracy metrics came from the som.py extension of hap.py program.
