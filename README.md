# MC2D archive entries

This repository analyses the MC2D-related archive entries, mainly:

* `campi-v1` - https://archive.materialscloud.org/record/2022.84

but also the initial work in

* `mounet-v4` - https://archive.materialscloud.org/record/2020.158

The anaylsis consists of various consistency checks, e.g. if the various UUIDs match and are present in AiiDA export file, and cleans up the `campi-v1` data to prepare for a `campi-v2` release.

The primary analysis is done in 

* `analysis.ipynb`

but `data-mismatch/` also contains preliminary analysis on the `campi-v1` data.

In order to fully run the analysis, `campi-v1` data needs to be fully downloaded and extracted. Additionally, AiiDA checks need to be performed on the prnmarvelsrv3, that contains the full AiiDA data.