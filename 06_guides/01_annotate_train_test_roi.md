# Walkthrough 1 — Annotate one COMO image, train, segment, analyze ROI

**Audience:** Missouri learners. **Prerequisites:** current GRIME-AI installation; CUDA NVIDIA GPU for practical SAM2 training/inference; `01_training_COCO/` and `02_test_images/` populated. UI wording below is grounded in the public July 2026 wiki and should be smoke-tested against the installed October build. Recommended first training run: <12 epochs, following PEP2026 tutorial. **An image used to train must not be used as an independent test sample.**

## A. Open / annotate in SAGE
1. Start GRIME-AI and open **SAGE**.
2. Click the folder picker in the SAGE image browser; choose `02_test_images/` *only if you will reassign that selected image to training and remove it from test*, or select a separate spare image from the source download.
3. Click a clear daylight image where the staff gage is visible, preferably one not already represented in the supplied training annotations.
4. Choose **Manual Polygon** to trace the long narrow gage (reliable geometric ground truth) OR **SAM2 Points** to guide automatic segmentation (left-click on staff gage, right-click on background), then press **Segment** or Enter.
5. Inspect overlay at high zoom. Exclude river, rocks, ruler shadow and surrounding mounting structure. If needed, add negative prompts or redo a more precise Manual Polygon.
6. Rename the mask **Staff** by double-clicking the mask name; ensure category matches the supplied COCO training set exactly.
7. Choose **Save COCO 1.0 (All Images)** and export to `04_participant_annotations/`. Verify COCO JSON and source image files were written.
8. Place or merge exported participant dataset into the training dataset root as a separate valid COCO image folder; do **not** simply concatenate two COCO JSONs because image/annotation/category IDs can collide. GRIME-AI recursively recognizes multiple dataset folders, so selecting both folders avoids an unsafe JSON merge.
9. Keep this newly annotated image out of the independent test set.

## B. Train a staff-gage segmentation model
10. Open **Deep Learning** (brain icon), **Train Model** tab.
11. Browse to the common parent containing the original supplied training folder and the participant COCO folder. In **Available Image Folders**, move **both** valid folders to **Selected Image Folders** (use **Move to Right**).
12. Press **Validate** and resolve any missing image, JSON, or label errors before proceeding.
13. Select architecture **SAM2** (official docs describe SegFormer-LoRA and YOLOv11-seg as experimental for this workflow).
14. Set **Label Selection** to **Staff**; use a unique **Site Name**, e.g. `COMO_Missouri_Staff_Demo`.
15. Keep default optimizer, learning rate, weight decay, blob-filter and batch size initially; select GPU and reduce batch size if CUDA runs out of memory. For the first demo use **8–10 epochs** (PEP2026 recommends fewer than 12). Keep training/validation percentages linked, using available default split; record the split.
16. Click **Train**. Watch progress and terminal for errors. Find model and metadata under the GRIME-AI Models/SAM2 output path, often in `Documents/GRIME-AI/Models/sam2/` (actual path depends on installation). Copy trained model and metadata to `03_models/`.

## C. Segment genuinely unseen test imagery
17. Open **Deep Learning → Segment Images**.
18. Set **SAM2** architecture. Enable **Save Predicted Masks**, **Copy Original Images**, and **Save Diagnostic Panels**; optionally probability maps.
19. Keep the default recommended **Mahalanobis** spatial filter for first run.
20. Under **Select Model**, browse to the newly trained `.torch` model. Confirm metadata target label **Staff** and correct site name.
21. Set **Output Folder** to `05_outputs/test_inference/`, add ONLY `02_test_images/` under **Image Folders**, choose target label **Staff**, click **Segment Images**.
22. Inspect diagnostic panels for false positives on rocks, water or markings and missed gage sections. Record filenames where detection fails. If independently annotated test masks are available, compute segmentation IoU/Dice on those; otherwise report *qualitative review only*, not measured accuracy.

## D. Analyze ROI on predictions
23. Open **ROI Analyzer** in the installed build (the repository documents this feature but the July wiki does not provide reliable click-by-click controls for the latest GUI). Load the segmentation output image collection or the copied originals plus masks, as supported by the current ROI Analyzer interface.
24. Define a fixed narrow ROI covering the staff-gage region and name it `COMO_staff_gage_ROI`. Check position on at least the first, middle, and last time-series image.
25. Choose supported per-ROI output (e.g., segmentation coverage/pixel area or available intensity metrics); run on the prediction series, then export the ROI table/CSV to `05_outputs/roi_analysis/`. Do **not** equate detected gage area with water level: level estimation requires calibrated staff coordinates, visible tick references, and a waterline method.
26. Verify at least one output row per processed image and visually compare changing ROI results with segmentation overlays. Save screenshots of the GUI settings used. **The exact ROI Analyzer controls must be confirmed on the actual installed build before giving this as a foolproof classroom instruction.**

**Participant success criteria:** one SAGE annotation exported as valid COCO, two dataset folders validated, one trained SAM2 model, predictions on untouched test images, one ROI CSV/plot and one documented inspection of a failure case.
