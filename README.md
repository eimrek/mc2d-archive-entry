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

## `campi-v2`

The `analysis.ipynb` script also aims to prepare a new version (`campi-v2`) of the `campi` dataset that could replace the current version.

For a full list of entry modifications, see the log `analysis.log`.

Here's a brief summary of the main changes:

* Various issues were fixed in `structure_2d.json`
    - issues included:
        - duplicates (70);
        - Missing parent `source_db` and/or `db_id`;
        - unsorted parents;
        - conflicting top-level parent with `all_3D_parents` (e.g. two different binding energies for the same parent);
    - See `analysis.log` for exact problems per entry.
    - Modified new format adopted:
        - No top-level parent. All parents are in a single source, just `all_3D_parents` with the 1st entry being the main parent used for optimization.
        - No `index_in_3D_parents` or `key` keys. these were inconsistent and misleading.
        - Just a space_group number. Any symbol or point group can be deduced from this.
        - Full hill formula instead of the old formula, which I was not sure what convention was used.
    - The analysis script tries to fix as many of the issues as possible and skips any problematic entries.
    - As a result, the final number of entries reduced from 3078 to 2942.
* `as_extracted_2d_structures.zip` didn't match with old `structure_2d.json`.
    - it had 3124 files as opposed to the 3078 in `structure_2d.json`, and it was not a simple superset.
    - these files were regenerated based on the uuids of the new `structure_2d.json`.
* `optimized_2d_structures.zip` - removed the intermediate `optimized_structures_new` folder.
* `README.txt` - fixed various issues, see e.g. `vimdiff campi-v1/README.txt campi-v2/README.txt`
* The AiiDA archive (`MC2D_export_20220622.aiida`) didn't contain many of the uuids presented in `structure_2d.json`, re-export the archive based on the uuids in 1) `mounet-v4/structures.json` and 2) the cleaned-up `structure_2d.json`.
    - Made the following AiiDA groups:
        - mounet18_optimized_2d_structures (258)
        - mounet18_bands (258)
        - mounet18_phonons (245)
        - campi23_extracted_2d_structures (2942)
        - campi23_optimized_2d_structures (2689)
        - campi23_band_gap (2295)
        - campi23_bands (2297)
        - campi23_3d_parents (754)
        - campi23_optimized_3d_parents_df2 (2677)
        - campi23_optimized_3d_parents_rvv10 (1547)
        - campi23_binding_energy_df2 (2928)
        - campi23_binding_energy_rvv10 (1649)
        - campi23_delta_df2 (1107)
        - campi23_delta_rvv10 (1077)
        - expanded_two_dimensional_database_initial_2D_structures_as_extracted_from_3D_parent (2942)
        - expanded_two_dimensional_database_bands_relaxed_2D_structures_pbe (2297)
        - expanded_two_dimensional_database_binding_energies_from_all_3D_parents (4577)
    - Note that I removed the group `expanded_two_dimensional_database_pw_scf_relaxed_2D_structures_pbe` from the README.txt and from the AiiDA export, as not all entries seemed to have a single consistent SCF calculation ran after the `vc-relax`, as the README.txt described.
        - the previous .aiida archive did contain this group, but it contained the `vc-relax` calculations instead of scf after the relaxation. 
