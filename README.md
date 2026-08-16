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

The BIDS validator went from 49 errors + 2252 warnings to 0 errors + 1981 warnings. None of the raw recording files (`.eeg`, `.vhdr`, `.vmrk`) were touched — every change is to a text sidecar.

**Participant table (`participants.tsv`)**
- Removed the `sub-AlBy15` row. The table listed 6 participants, but only 5 subject folders are actually present (`sub-AnSt01`, `sub-FeKl03`, `sub-IrCh04`, `sub-KiKo09`, `sub-SoNi11`), and `sub-AlBy15` has no data here or in the OpenNeuro source — it pointed to a participant who was never published. Dropping the row makes the table match the subjects that exist. The real cohort is 5 participants (3 male, 2 female, all right-handed). The auto-generated BIDS Report block higher up in this README was left as-is and still describes the original 6-person breakdown.

**Session table (`sessions.tsv`)**
- Rewrote the `session_id` values from the bare labels (`An`, `Ca`, `Sh`) to the full BIDS form (`ses-An`, `ses-Ca`, `ses-Sh`) so they match the `ses-<label>` folder names on disk, which is what BIDS requires.

**Session sidecar (`sessions.json`)**
- Added a description for the `session_id` column, documenting the BIDS id format now used in `sessions.tsv`.
- Fixed a misspelled key, `posistive_electrode`, to `positive_electrode` so it matches the actual column header in the TSV. Because of the typo, the original description never applied to the real column, leaving it undocumented.

**Event tables (`tms rmt` and `working amplitude` columns, 24 `_events.tsv` files across `sub-{FeKl03,IrCh04,SoNi11}/ses-{An,Ca,Sh}`, 1320 rows total)**
- Filled the empty cells in these two numeric columns with `n/a`, the standard BIDS marker for missing data — BIDS does not allow blank cells in numeric columns. The empty cells are all in the sessions that had no TMS applied (FeKl03 Anodal/Cathodal, IrCh04 Cathodal, SoNi11 Sham), so there was no calibration value to record. The sessions that did use TMS already had their numbers (e.g. `63.0` / `70.0`), and those were left untouched.

**Dataset description (`dataset_description.json`)**
- `BIDSVersion` was `1.7.0`, older than what the validator checks against; set it to `1.11.1` so the declared version matches the schema the dataset now passes. The auto-generated BIDS Report block higher up still shows the original "1.7.0" and was left unchanged.
- Cleared a placeholder reference: `ReferencesAndLinks` held `"https://example.com/publication"`, which is not a real link, so it was emptied rather than left pointing nowhere.
- Removed a placeholder source entry, `{"Gimme": "https://example.com/source_dataset"}` — both the URL and the made-up `Gimme` field name were filler. The genuine source link (the OpenNeuro DOI `doi:10.18112/openneuro.ds006126.v1.0.0`) was kept.

**Recording sidecars (`_eeg.json`, all 90 recordings)**
- The misc-channel-count field was spelled `MiscChannelCount`; BIDS uses all-uppercase `MISCChannelCount`. Renamed it (the value, `0`, was already correct) so the validator recognizes it.

**Acquisition times (`scans.tsv`) — left exactly as published**
- The `acq_time` timestamps still carry their original trailing `Z` (UTC) marker. This is valid BIDS — a time-zone offset is optional, not forbidden — so the timestamps were left unchanged.

**Remaining warnings (1981) — left on purpose**
- 1980 of these are "recommended but missing" fields that need information about the equipment or study that isn't in the dataset (for example: cognitive-atlas IDs, cap manufacturer and model, head circumference, hardware filters, artefact notes, and stimulus-presentation details). Following the rule "never invent metadata," they were left blank rather than filled with guesses, and are best supplied by the original authors.
- The remaining 1 warning is a missing HED-version field; this dataset does not use HED tags, so the field is optional and left blank.
