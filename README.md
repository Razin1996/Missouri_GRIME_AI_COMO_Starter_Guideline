# Missouri GRIME-AI / NEON COMO starter package
Prepared 2026-10-29. STATUS: documentation and empty folder scaffolding, NOT an operational trained-model bundle.

## Start here
1. Download the PEP2026 NEON COMO resource: https://www.hydroshare.org/resource/b161beb15b2d41029ac9f5347be35d40/
2. Obtain or extract the staff-gage COCO image+annotation dataset into `01_training_COCO/`. Keep the image filenames/paths aligned with the COCO JSON.
3. Select independent, unannotated, non-overlapping COMO images and copy them to `02_test_images/`.
4. Follow `06_guides/01_annotate_train_test_roi.md` for the SAGE/model workflow.
5. Follow `06_guides/02_download_triage_composite.md` for the month-of-noon-images workflow.
6. Place trained model and companion metadata in `03_models/`, and outputs in `05_outputs/`.

### Important status and provenance
The HydroShare site is public and identifies a ~1.6 GB COMO resource, but its file inventory and annotation data were not accessible through the public static page during preparation. No user-provided attachment was available to validate. **No imagery, COCO annotation files, test dataset, or trained model is enclosed in this ZIP.** Never represent this scaffold as a validated model or complete dataset.

Keep `Staff` as the target label (as in official PEP2026 tutorial); validate category spelling and IDs in any actual COCO JSON. Keep a permanently separate test partition, ideally different days/conditions, with no adjacent frame leakage. A participant's new image used for training must not be scored as an independent test image.

Useful verified references:
- https://github.com/GRIME-Lab/GRIME-AI/wiki/08-SAGE
- https://github.com/GRIME-Lab/GRIME-AI/wiki/05-Model-Building
- https://github.com/GRIME-Lab/GRIME-AI/wiki/06-Model-Deployment
- https://github.com/GRIME-Lab/GRIME-AI/wiki/02-Data-Retrieval
- https://github.com/GRIME-Lab/GRIME-AI/wiki/03-Data-Preparation
- https://github.com/GRIME-Lab/GRIME-AI/wiki/Appendix-A-%E2%80%90-PEP2026-Tutorial

### Pre-delivery gate
- [ ] Training annotations and all referenced source images physically present
- [ ] COCO JSON parsed, filenames exist, segmentation geometries nonempty, `Staff` category confirmed
- [ ] Training data pass GRIME-AI `Validate`
- [ ] Test images withheld from training and from internal validation
- [ ] Model trained with specified build, checkpoint and metadata included
- [ ] Predictions and diagnostics manually inspected on test data
- [ ] ROI Analyzer workflow tested in installed build and screenshot captured
- [ ] Run through both guides on a clean machine
