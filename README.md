[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.on006126-blue)](https://doi.org/10.82901/nemar.on006126)

# TDCS Neuromodulated Motor Imagery TMS Dataset

## Research/Experiment Description

[Your research description goes here]

## BIDS Report

 The TDCS Modulation of Visual Cortex in Motor Imagery dataset was created by
Anthony Mensah, Gleb Perevoznyuk, Artyom Batov, and Aleksandra S. Pleskovskaya
and conforms to BIDS version 1.7.0. This report was generated with MNE-BIDS
(https://doi.org/10.21105/joss.01896). The dataset consists of 5 participants
(comprised of 3 male and 3 female participants; comprised of 6 right hand, 0
left hand and 0 ambidextrous; ages ranged from 18.0 to 30.0 (mean = 23.0, std =
5.1)) and 3 recording sessions: An, Ca, and Sh. Data was recorded using an EEG
system (Brain Products) sampled at 5000.0 Hz with line noise at 60.0 Hz. There
were 90 scans in total. Recording durations ranged from 363.7 to 2910.98 seconds
(mean = 429.21, std = 270.19), for a total of 38629.34 seconds of data recorded
over all scans. For each dataset, there were on average 3.0 (std = 0.0)
recording channels per scan, out of which 3.0 (std = 0.0) were used in analysis
(0.0 +/- 0.0 were removed from analysis).

﻿References
----------
Appelhoff, S., Sanderson, M., Brooks, T., Vliet, M., Quentin, R., Holdgraf, C., Chaumon, M., Mikulan, E., Tavabi, K., Höchenberger, R., Welke, D., Brunner, C., Rockhill, A., Larson, E., Gramfort, A. and Jas, M. (2019). MNE-BIDS: Organizing electrophysiological data into the BIDS format and facilitating their analysis. Journal of Open Source Software 4: (1896).https://doi.org/10.21105/joss.01896

Pernet, C. R., Appelhoff, S., Gorgolewski, K. J., Flandin, G., Phillips, C., Delorme, A., Oostenveld, R. (2019). EEG-BIDS, an extension to the brain imaging data structure for electroencephalography. Scientific Data, 6, 103.https://doi.org/10.1038/s41597-019-0104-8


## NEMAR curation changes (2026-05-21, revised 2026-05-27)

BIDS validator: 49 errors + 2252 warnings -> 0 errors + 1981 warnings. Raw `.eeg` / `.vhdr` / `.vmrk` binary payloads unchanged. The `acq_time` timestamps in the `*_scans.tsv` files are preserved exactly as published (including the trailing `Z` UTC indicator, which is valid per the BIDS "date-time" type — an offset is optional, not forbidden).

### `participants.tsv`
- Dropped the `sub-AlBy15` row. Why: `participants.tsv` listed 6 participants but only 5 subject directories exist (`sub-AnSt01`, `sub-FeKl03`, `sub-IrCh04`, `sub-KiKo09`, `sub-SoNi11`); `sub-AlBy15` has no data directory in this dataset or in the OpenNeuro source (`ds006126`), so the row was a dangling reference to data that was never published. Removing it makes the participant table consistent with the subjects actually present. The true cohort is 5 participants (3 male, 2 female, all right-handed); the auto-generated BIDS Report block above was left unchanged and still reflects the original 6-row breakdown ("3 male and 3 female", "6 right hand").

### `sessions.tsv`
- Rewrote `session_id` column from bare labels (`An`, `Ca`, `Sh`) to BIDS-canonical full entity form (`ses-An`, `ses-Ca`, `ses-Sh`) matching the on-disk `ses-<label>` directory names. Why: closes `TSV_VALUE_INCORRECT_TYPE:session_id` — BIDS requires the column cells to use the same form as the directory entity, not the bare label.

### `sessions.json`
- Added `session_id` column definition (descriptor only). Why: documents the new BIDS-canonical id format used in `sessions.tsv`.
- Renamed key `posistive_electrode` -> `positive_electrode` to match the actual TSV header. Why: closes the `TSV_ADDITIONAL_COLUMNS_UNDEFINED:positive_electrode` warning — the original column definition was unreachable due to the misspelling, leaving the `positive_electrode` TSV column undocumented.

### `sub-{FeKl03,IrCh04,SoNi11}/ses-{An,Ca,Sh}/eeg/*_events.tsv` (24 files, 1320 rows total)
- Replaced empty cells in the `tms rmt` and `working amplitude` columns with `n/a`. Why: closes 48 `TSV_VALUE_INCORRECT_TYPE` errors (24 each for `tms rmt` and `working amplitude`) — BIDS rejects empty cells in numeric columns; `n/a` is the BIDS-canonical missing-data marker. Affected sessions are those without TMS application (the FeKl03 Anodal/Cathodal, IrCh04 Cathodal, and SoNi11 Sham runs), where these per-session calibration columns have no value to record. Numeric magnitudes for sessions with TMS (already filled, e.g. `63.0` / `70.0`) were untouched.

### `dataset_description.json`
- Bumped `BIDSVersion` from `1.7.0` to `1.11.1`. Why: set to the BIDS version of the validator the dataset was validated against (BIDS 1.11.1), so the declared conformance matches the schema it actually passed. The auto-generated BIDS Report block above still states the source's original "1.7.0" and was left unchanged.
- Cleared the placeholder `ReferencesAndLinks: ["https://example.com/publication"]` to `[]`. Why: the URL was a placeholder, not a real reference; an empty array is preferable to a fake link.
- Dropped the placeholder `SourceDatasets` entry `{"Gimme": "https://example.com/source_dataset"}` (also containing the unknown `Gimme` key); kept the genuine `{"DOI": "doi:10.18112/openneuro.ds006126.v1.0.0"}` entry. Why: the placeholder carried no information and used a non-BIDS field name.

### All 90 `sub-*/ses-*/eeg/*_eeg.json`
- Renamed the key `MiscChannelCount` -> `MISCChannelCount` (BIDS canonical spelling has all-uppercase MISC). Why: closes 270 `SIDECAR_KEY_RECOMMENDED:MISCChannelCount` warnings — the validator did not recognise the PascalCase spelling, so the `MISCChannelCount: 0` value that was already documented was treated as missing.

### Out of mechanical scope (left as warnings)
- 1980 `SIDECAR_KEY_RECOMMENDED` warnings remain across `CogAtlasID`, `CogPOID`, `CapManufacturer`, `CapManufacturersModelName`, `HeadCircumference`, `HardwareFilters`, `SubjectArtefactDescription`, and `StimulusPresentation`. Per the NEMAR curation rule "never invent metadata", these require equipment/study context not documented in the existing dataset and are deferred to the original authors.
- 1 `JSON_KEY_RECOMMENDED:HEDVersion` warning — the dataset does not use HED tags, so the field is optional.
