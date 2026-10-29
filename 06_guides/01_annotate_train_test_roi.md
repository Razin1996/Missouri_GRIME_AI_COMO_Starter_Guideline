# 01 — Annotate, Train, Test, and Run ROI Analyzer

**GRIME-AI pre-workshop system check — Missouri**

**Goal:** Confirm that GRIME-AI can (1) open images in SAGE, (2) create and export one staff-gage annotation, (3) train a model using that new annotation and the supplied training data, (4) apply the model to images not used for training, and (5) run ROI Analyzer and save its results.

**Estimated input:** The supplied example package, GRIME-AI installed on your computer, and enough free disk space for image outputs and model checkpoints. Training SAM2 is best tested on a CUDA-compatible NVIDIA GPU; CPU training may be very slow. The exact interface may vary slightly by installed GRIME-AI version.

## Part A — Annotate one image in SAGE

1. In GRIME-AI, open **Tools → SAGE** (or launch `sage` from the activated Conda environment).

<img src="./graphics/sage_start.png" alt="SAGE Interface" width="500">

2. Use the image-folder selector in SAGE to open `04_participant_annotations/new_image/`.

<img src="./graphics/sage_1.png" alt="SAGE Interface" width="500">

3. Select the image. Zoom in until the staff gage is clearly visible.
4. In the **Label Manager**, create a label by putting **`Staff`** in the `New label name` field and hit `Add` button. Preserve this exact spelling to match the supplied model-training annotations.

<img src="./graphics/sage_2.png" alt="SAGE Interface" width="200">

5. In the `Label Class` panel, click `Staff` to select it as the annotation class. Next, navigate to the `Segmentation Mode` panel and select the `Manual Polygon` tool. Click around the visible outline of the staff gage to place vertices along its edges. Once the entire staff gage is outlined, right-click to finish drawing the polygon.

<img src="./graphics/sage_3.png" alt="SAGE Interface" width="300">

6. Inspect the colored mask overlay. The mask should cover the **gage**, not nearby water, banks, vegetation, shadows, or the mounting post unless part of the intended target.

7. Click **Save COCO 1.0 (All Images)**. Save to the participant annotation location offered by SAGE and note the output path shown in the confirmation dialog. Keep the original image with the exported annotation dataset; move/copy them together if needed.

<img src="./graphics/sage_4.png" alt="SAGE Interface" width="200">

8. Check that the exported dataset contains a readable COCO `.json` file and that its listed image can still be located. Confirm SAGE reports at least **one image and one annotation** saved.

**PASS:** One image has a visually correct `Staff` mask and the COCO export completes successfully.

## Part B — Train a model with given datasets

1. Return to GRIME-AI. Open **Deep Learning → Train Model**.
2. In the input/dataset browser, choose the parent directory that allows GRIME-AI to find both the supplied `01_training_COCO/` dataset and the COCO dataset saved in `04_participant_annotations/`. Training-data folders must contain their annotation file and referenced images.
3. Review **Available Image Folders**. Move the supplied training folder **and** your newly annotated folder into **Selected Image Folders** using **Move to Right**. If only one folder appears, inspect the paths and COCO export rather than silently training on just one.

![Screenshot 07 — Training dataset folder selection](screenshots/Step_07.png)
4. Click **Validate**. Resolve errors about missing images, unreadable annotations, or mismatched categories. You should be able to confirm that both datasets are included.

![Screenshot 08 — Successful dataset validation](screenshots/Step_08.png)
5. Under model architecture, select **SAM2**.
6. Under **Label Selection**, select **`Staff`**.
7. Set **Site Name** to something unique, for example `Missouri_Staff_SmokeTest`.
8. For a short functionality test, set **Epochs = 8**. Leave learning rate, optimizer, weight decay, and other advanced options at their defaults unless instructed otherwise by the organizer. Use the default training/validation split and record it; the separate `02_test_images/` folder must remain excluded from training and validation.

![Screenshot 09 — SAM2 model training settings](screenshots/Step_09.png)
9. Select **GPU/CUDA** if your system supports it. If you receive an out-of-memory error, reduce the **batch size** and retry. A CPU-only run may take substantially longer.
10. Click **Train** and monitor the training progress and application log. Wait for the run to finish successfully.

![Screenshot 10 — Training completion and checkpoint](screenshots/Step_10.png)
11. Locate the saved model checkpoint and companion metadata using the path shown in GRIME-AI (often under your GRIME-AI Models directory). Optionally copy both to `03_models/` for easy reference.

**PASS:** Dataset validation succeeds for both inputs, training finishes without an error, and a trained model file plus associated metadata exist.

**If it fails:** Capture the error text and a screenshot. Check that both COCO dataset folders are valid, the `Staff` class is present, and your device has adequate memory.

## Part C — Test the trained model on unseen images

1. Open **Deep Learning → Segment Images**.
2. Select **SAM2** as the model architecture.
3. Turn on **Save Predicted Masks**, **Copy Original Images**, and **Save Diagnostic Panels**, if these options are available in your version.
4. Leave the **SAM2 Spatial Filter** at its recommended default, generally **Mahalanobis**, for this first test.
5. Under **Select Model → Browse**, choose the checkpoint you trained in Part B. Confirm its metadata loads and shows the `Staff` label.

![Screenshot 11 — Segment Images model selection](screenshots/Step_11.png)
6. Set **Output Folder** to `05_outputs/test_inference/`.
7. Use **Add Folder** to select **only** `02_test_images/`. Do not add the training folder or the image newly annotated in Part A.

![Screenshot 12 — Test image and output folder setup](screenshots/Step_12.png)
8. In **Labels**, select **`Staff`**.
9. Click **Segment Images**. Wait until processing is complete and check the application log for failures.

![Screenshot 13 — Segmentation progress and outputs](screenshots/Step_13.png)
10. Open the output folder. Confirm files were created for the processed test images. Examine at least three diagnostic panels or image/mask pairs (or all results if there are fewer than three).

![Screenshot 14 — Predicted staff-gage mask example](screenshots/Step_14.png)
11. For each checked image, confirm the mask is near the staff gage rather than covering unrelated objects. Record any missed gage or false detections. This is a **visual smoke test**, not a numerical accuracy assessment unless the test images have separate ground-truth masks.

**PASS:** Inference completes on images excluded from training, output files are saved, and the predicted masks can be opened and inspected.

**If it fails:** Verify the model's companion metadata is present, the correct architecture is selected, the images are readable, and the output folder is writable.

## Part D — Test ROI Analyzer

> **Interface check:** ROI Analyzer controls can vary by GRIME-AI build. The sequence below describes the required actions, not unverified button labels. If your build does not offer a named option, record the visible control and take a screenshot rather than guessing.

1. Return to the main GRIME-AI window and open **ROI Analyzer** from the analysis/exploration tools in your installed version.
2. Load the image collection associated with `05_outputs/test_inference/`. If ROI Analyzer requires original images rather than prediction masks, load the **copied original images** from this output folder (or `02_test_images/`) and use mask outputs as a separate visual reference. Do not assume ROI Analyzer automatically imports segmentation masks.

![Screenshot 15 — ROI Analyzer image selection](screenshots/Step_15.png)
3. Display the first image. Draw or define **one fixed rectangular or polygonal ROI** surrounding the staff-gage region, using the ROI creation control offered in your version.
4. Name it `Staff_Gage_ROI` if your interface supports ROI naming, and **save/apply** the ROI definition.

![Screenshot 16 — Staff gage ROI definition](screenshots/Step_16.png)
5. Move between the first, middle, and last images. Confirm that the same ROI stays aligned with the staff-gage location for a fixed camera. If the camera moved significantly, record that limitation.
6. Select an available ROI measurement or feature (for example, mean pixel intensity or an available color/texture metric). Run the calculation across the loaded images.

![Screenshot 17 — ROI analysis settings and run](screenshots/Step_17.png)
7. Export or save the available result table, CSV, or figure to `05_outputs/roi_analysis/`. Record the precise output file and selected metric.

![Screenshot 18 — ROI Analyzer exported results](screenshots/Step_18.png)
8. Open the saved result and verify that it is not empty and that its image count matches the processed collection, allowing for any explicitly skipped files.

**PASS:** ROI Analyzer loads images, accepts a saved ROI, runs a feature calculation across the collection, and saves a readable result.

**Important:** A basic ROI measurement is not automatically a staff-gage water-level estimate. Do not interpret ROI pixel intensity or total predicted gage area as stage without additional calibration and waterline processing.

## Final pre-workshop checklist

- [ ] GRIME-AI starts successfully. **Version:** __________
- [ ] SAGE opens the selected image.
- [ ] One `Staff` annotation is saved to COCO format.
- [ ] Both supplied and newly created COCO datasets appear in training selection.
- [ ] **Validate** completes without errors.
- [ ] SAM2 training finishes. **Model/checkpoint path:** __________
- [ ] Test inference runs on independent images. **Number of test images:** __________
- [ ] Predicted masks/diagnostic panels are saved and visually inspected.
- [ ] ROI Analyzer loads images and accepts a defined ROI.
- [ ] ROI measurement is exported. **Result path:** __________

**If anything fails:** Send the organizer (1) your GRIME-AI version, (2) the step number, (3) a screenshot of the application, and (4) the complete error message or relevant log excerpt. Do not send only “it isn't working.”
