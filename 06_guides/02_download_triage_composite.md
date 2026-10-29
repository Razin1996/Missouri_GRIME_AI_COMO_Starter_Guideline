# Walkthrough 2 — One month of daytime COMO images, triage, composite slice

**Goal:** retrieve an approximately one-month midday sequence (e.g., June 1–30, 2025; verify available dates at the provider), quality-control, and build a chronological composite slice image. Exact date range is a demo example, not confirmation that imagery is available throughout that interval.

1. Start GRIME-AI. Open **Retrieve Data → NEON Sites** and locate **COMO / Como Creek**. Expand site details to confirm site ID.
2. In **Download Manager**, select the camera/imagery product (if multiple site products are listed, choose the visible-light stream containing the staff gage). Set start **2025-06-01** and end **2025-06-30**.
3. Set the time-of-day filter to **11:00–13:00** in the provider/displayed timezone, then verify the saved image timestamps. Where timestamps are UTC rather than local time, adjust the window to correspond to local noon; avoid assuming 12:00 UTC is local noon.
4. Browse to a dedicated output directory, e.g. `05_outputs/downloaded_June2025/`. Click **Download** and inspect the resulting folder; if empty, choose a confirmed available month and document the substitution. For a one-image-per-day demonstration, retain the timestamp closest to midday for each date.
5. Open **Data Exploration** and set the **Images Folder** path to the downloaded directory. Click **Fetch Image List**. Check that previews are valid and chronologically sorted.
6. Open **Image Triage** (navigation button, Data Exploration, or Tools menu). Point it at the downloaded images. Enable **Generate Report** and **Move Bad Images**; optionally **Save Polylines**. Run triage.
7. Open the triage CSV; note total, accepted, and removed images and rejection reasons (blur, low contrast, bad exposure, etc.). Review a few rejected images to ensure the criteria are not too aggressive. Move Bad Images sequesters files instead of deleting them; do not overwrite your raw archive.
8. Set the Data Exploration image folder to the **accepted** images only, run **Fetch Image List** again and inspect chronological ordering.
9. Click **Composite Slice**. In the preview, position the vertical slice over the staff-gage/stream region (or center as a first pass). Choose a thin slice width, initially **5–10 pixels** as a demonstration setting (not a documented software default).
10. Click **Generate** and save/export the resulting composite under `05_outputs/composite_slice/`, e.g. `COMO_June2025_noon_composite.png`.
11. Inspect the composite: each strip represents an image timestamp, so ordering matters. Record source image count, dates, actual time window, slice x-position, slice width and number of skipped images in `05_outputs/composite_slice/README.txt`.
12. Repeat with a slice centered on the stream if the gage region yields little temporal contrast. Keep both outputs and describe what changes in the scene over the month.

**Participant success criteria:** about 1 month of valid imagery (or an explicitly documented alternate month), triage CSV, segregated bad images, chronological accepted set, at least one saved composite slice image.

References: https://github.com/GRIME-Lab/GRIME-AI/wiki/02-Data-Retrieval and https://github.com/GRIME-Lab/GRIME-AI/wiki/03-Data-Preparation
