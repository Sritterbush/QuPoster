# QuPoster

**Turn physical event posters into structured datasets.**

QuPoster ([quposter.com](https://quposter.com)) is an analog-to-digital interactive presentation solution. By using a modular design that can be overlaid onto any baseboard or template, interactive presentations are made reusable at a low cost. QuPoster supports both **Qu**antitative and **Qu**alitative data collection, leveraging a computer vision pipeline to digitize and store results.

---

## How It Works

1. **Modular Print:** Each segment where data will be collected is printed with a high-contrast border for automated de-skewing/cropping, and a QR code for data tracking.
2. **QR Indexing:** The corner QR code tracks the `Topic`, `Instance`, and `Segment` needed for organizing the data collected.
3. **Physical Engagement:** Attendees are encouraged to interact. Examples include placing dot stickers for heatmap voting or Post-It notes for qualitative feedback.
4. **Capture & Routing:** Post-event, presenters scan or photograph the removed sheets and upload them. The QR code automatically orients and routes the image.
5. **Computer Vision & OCR Pipeline:**
   * **Quantitative:** Normalizes white balance, detects dot stickers, and logs counts by color and region.
   * **Qualitative:** Segments individual Post-It notes, saves cropped images to cloud object storage, extracts note/ink colors, and runs Handwritten Text Recognition (HTR/OCR).
6. **Human-in-the-Loop Validation:** Presenters can review the images alongside the OCR output, verifying or editing results before committing the final records.

---

## Quantitative Support Roadmap

1. **Initial Support:** Support for the identification, counting, and grouping by color of dot stickers with no overlap.
2. **X/Y Coordinate Tracking:** Ability to identify and convert Scatter Plot and Likert Slider-style voting, identify regions of a segment that indicate different votes, and report overlaps.
3. **Handling of Irregular Shapes:** Ability to detect overlapped shapes and recognize/categorize non-round shapes.
4. **Stamps:** Ability to detect the use of stamped shapes, recognize incomplete stamps, and group by stamp shape and color.
5. **Missing Tabs:** Support for perforated tear-off strips along a sheet, with the ability to tally missing tabs.
6. **Maps:** Ability to recognize maps and associate votes with designated regions.
7. **3D Voting:** Ability to recognize 3D shapes used for voting, such as push pins.

## Qualitative Support Roadmap

1. **Initial Support:** Accurate ingestion of Post-It notes for image cropping, minimizing cropped image size while maintaining legibility for validation, plus basic HTR/OCR.
2. **Color Categorization:** Accurate categorization of background color and text color for Post-It notes.
3. **Refined HTR/OCR:** More reliable text estimates, even when dealing with overlapped Post-It notes hiding partial letters. Includes warnings when text is fully hidden, coupled with a cropped image of the issue area.
4. **Embedded Survey Support:** Ability to link online survey or form data directly to the corresponding poster segment.
5. **Complex HTR/OCR:** Ability to interpret cursive and non-standard handwriting.
6. **Audio Capture Support:** Ability to record audio responses via app and associate them with the appropriate segment, providing transcribed responses alongside local audio recordings for validation.

---

## Repository Structure

```text
quposter/
├── database/
│   ├── schema/          # Core T-SQL star schema (Dim_* and Fact_* tables)
│   └── migrations/      # Incremental database updates
├── pipeline/
│   ├── cv_extraction/   # OpenCV border detection, dot counting, Post-It cropping
│   ├── ocr/             # Handwritten text recognition (TrOCR / PaddleOCR)
│   └── qr_routing/      # QR/barcode decoding and ID parsing
├── web/                 # quposter.com upload portal, validation UI, and public dashboards
├── templates/           # Printable 8.5x11 layouts and QR code generation scripts
└── docs/                # Architecture diagrams and physical print specifications