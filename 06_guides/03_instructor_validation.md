# Instructor dataset / model handoff checklist

## COCO verification
- Check that `01_training_COCO/` contains originals and COCO 1.0 JSON; parse `images`, `annotations`, `categories`; verify referenced paths exist.
- Verify `Staff` is a class, nonempty segmentation per image, nonzero image dimensions, no corrupt files, and no duplicated image or annotation IDs in a single COCO file.
- Check bounding shapes cover staff **gage**, not water or nearby marker. Inspect sample annotation overlays across varied lighting.
- Reserve a distinct test set (suggest 10–20 independent frames selected from other dates/conditions; not a guaranteed size).
- Retain provenance, attribution, timestamp and GRIME-AI build/conda package version.

## Model proof
- Validate dataset(s) in Train Model GUI; train the demo SAM2 checkpoint; copy the `.torch` file AND metadata into `03_models/`.
- Run Segment Images on unseen `02_test_images/` and inspect panels. Record error cases and any quantitative scores only if labeled ground truth exists.
- On the same installed version, rehearse ROI Analyzer with masks and write down actual button names, accepted inputs and CSV output.
- On another workstation, test a fresh extraction and both walkthroughs. Provide training/test counts, model checksum and known limitations.

## Explicit blockers in starter zip
This ZIP contains guides and empty directories only. The browser did not expose individual file names/download bytes within public HydroShare COMO page; the original 'attached' annotation file was not visible in this chat. Training was not run and no model is claimed.
