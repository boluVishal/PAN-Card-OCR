# PAN Card OCR

A small OCR project that extracts structured information from an Indian PAN card image.

![PAN Card to JSON](PANOcr1.jpg?raw=true "PAN Card OCR")

## Problem

Extract key fields from a PAN card image and return them in a structured format. The current implementation attempts to identify:

- Name
- Father's name
- Year of birth
- PAN number

## How it works

The existing pipeline follows these steps:

1. Load the input image.
2. Convert the image to a high-contrast black-and-white representation.
3. Run Tesseract OCR with `pytesseract`.
4. Clean and split the extracted text into lines.
5. Match likely names against `src/namedb.csv`.
6. Parse a date-like value to identify the year of birth.
7. Find the PAN number from the remaining OCR output.
8. Write the extracted fields to JSON.

## Repository structure

```text
.
├── Dockerfile
├── PANOcr1.jpg
├── README.md
├── requirements.txt
└── src/
    ├── box.py
    ├── crop_morphology.py
    ├── crop_to_box.py
    ├── namedb.csv
    ├── out.txt
    ├── tpan.py
    └── tpanold.py
```

## Dependencies

The project uses:

- Python
- OpenCV
- NumPy
- Pillow
- pytesseract
- python-dateutil
- SciPy

The exact Python package versions currently used by the project are listed in `requirements.txt`.

Tesseract OCR must also be installed on the system because `pytesseract` is only the Python wrapper.

## Usage

The main extraction script is `src/tpan.py` and expects an image path as its first command-line argument.

```bash
cd src
python tpan.py <path-to-pan-card-image>
```

The script writes the extracted values as JSON. The current code expects the output location referenced by the script to exist before execution.

Example output shape:

```json
{
  "Name": "...",
  "Father Name": "...",
  "Date of Birth": "...",
  "PAN": "..."
}
```

## Notes

This is an older OCR implementation and uses heuristic text matching rather than a trained document-understanding model. OCR quality and image layout can therefore affect the extracted result.
