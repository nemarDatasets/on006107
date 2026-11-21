[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.on006107-blue)](https://doi.org/10.82901/nemar.on006107)

<!-- nemar:provenance -->
## Provenance

This dataset is mirrored on NEMAR from OpenNeuro.

- Source: [ds006107](https://openneuro.org/datasets/ds006107)
- DOI: [10.18112/openneuro.ds006107.v1.0.0](https://doi.org/10.18112/openneuro.ds006107.v1.0.0)

## Channel names follow the data headers (18 Oct 2026)

In 167 recordings the channel list of channels.tsv did not equal the data-file header (names, order or count), so `mne_bids.read_raw_bids` stopped with 'Channel mismatch' (or 'Duplicate channel names'); EEGDash renamed the data channels by position to the channels.tsv names, which gives wrong labels wherever the two lists are not aligned. Since 18 Oct 2026 every channels.tsv lists exactly the header channels, in header order, with the header names (MNE names: duplicated header labels numbered -0, -1).

Rows were matched to header channels BY NAME, never by position (exact; then case/whitespace; then the recorder prefix 'EEG '/'POL ', '_' and zero padding; then a trailing '-<reference>' token). Matched rows keep all their columns (type, units, status, ...); 18189 rows took the header name (the header labels carry '-AV' suffix).

2 header channels had no channels.tsv row (1 files) and got a new row: type from the name (ECG/EMG/EOG/trigger) or from the other contacts of the same shaft, else MISC; units from the header; status n/a. Examples: `sub-32_ses-01_task-sws_run-01_ieeg.edf`: B35-AV, B36-AV.

47 channels.tsv rows had no channel in the data file (6 files) and were removed; they are kept verbatim under `sourcedata/channels-removed-2026-10-18/` (same relative path). Examples: `sub-06_ses-01_task-sws_run-01_ieeg.edf`: B35, B36; `sub-148_ses-01_task-sws_run-01_ieeg.edf`: C1, C2, C3, C4, C5; `sub-150_ses-01_task-sws_run-01_ieeg.edf`: C1, C2, C3, C4, C5.

1 files listed the channels in another order than the header; rows now follow the header order.

EEGDash's positional renaming had given wrong labels to 76 channels in 2 files (rows missing, extra or out of order before that position).

electrodes.tsv is unchanged: it keeps the contacts (e.g. `A1`), while channels follow the recording headers (e.g. `A1-AV`). A channel pairs with a contact by dropping the `-AV` suffix (and any `POL ` or `EEG ` prefix, zero-padding the contact number). This pairs 17795 of the 18191 channels.tsv rows with an electrodes.tsv row; the other 396 have no row in electrodes.tsv, so they have no position.

## Electrode coordinate system (19 Oct 2026; revised 20 Nov 2026)

A coordsystem.json (`fsaverage`, `mm`) sits next to each of the 167 electrodes.tsv. This deposit does not state a coordinate system, so the frame is inferred, not documented: every contact's x/y/z (17812 contacts in 167 files) equals a vertex of the fsaverage pial surface (within 0.001 mm), which is consistent with FreeSurfer surface-based normalization of subdural contacts. fsaverage is FreeSurfer's template whose volumetric frame is FreeSurfer's MNI305 space. Other deposits from the same group give identical coordinates and call the space MNI305 or fsaverage. Each sidecar's iEEGCoordinateSystemDescription states the same evidence.

Limitation: these sidecars declare the frame and units only. electrodes.tsv is unchanged and no coordinate value changed. Positions do not attach to channels in mne_bids or EEGDash, because the channel names (data-header labels, e.g. prefixed labels or bipolar derivations) differ from the contact names in electrodes.tsv; a channel-to-contact mapping is not part of this change. This dataset mirrors OpenNeuro; the same change belongs upstream.
