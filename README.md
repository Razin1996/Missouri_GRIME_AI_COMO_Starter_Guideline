
# Missouri GRIME-AI — COMO Starter Package

This repository provides example datasets, a pretrained staff-gage detection model, and step-by-step instructions to help participants test GRIME-AI before the workshop.

## Getting Started

1. Download or clone this repository to your computer.
2. Make sure the latest **GRIME-AI** is installed and working.
3. Follow the two guides in the `06_guides/` folder:

   - **[Guide 1 — Annotate, Train, Test, and Run ROI Analyzer](06_guides/01_annotate_train_test_roi.md)**  
     Learn how to annotate an image in SAGE, train a SAM2 model, test staff-gage detection, and analyze the results using ROI Analyzer.

   - **[Guide 2 — Download, Triage, and Generate a Composite Slice](06_guides/02_download_triage_composite.md)**  
     Learn how to download COMO imagery, run Image Triage, and create a composite slice image.

## Repository Structure

- `01_training_COCO/` — Example training images and COCO annotations.
- `02_test_images/` — Images for testing the model.
- `03_models/` — Pretrained staff-gage model checkpoint.
- `04_participant_annotations/` — Example image for practicing SAGE annotations.
- `05_outputs/` — Folder for saving generated results.
- `06_guides/` — Step-by-step instructions with screenshots.

## Additional Resources

For more information about GRIME-AI, visit the [official GRIME-AI GitHub repository](https://github.com/GRIME-Lab/GRIME-AI).
