# PAN Card OCR

Extracts structured information from images of Indian PAN (Permanent Account Number) cards using Tesseract OCR.

**Output fields:** Name, Father's Name, Date of Birth, PAN number — formatted according to Indian government standards.

![Sample PAN Card](PANOcr1.jpg)

## How it works

1. Take the card image
2. Detect and crop to the text region using morphological operations (`crop_morphology.py`, `crop_to_box.py`)
3. Convert to grayscale
4. Run through Tesseract OCR
5. Post-process the raw text:
   - Match name against a name database (`namedb.csv`) using fuzzy matching (`difflib`)
   - Assume the second name found is the father's name
   - Extract year of birth using regex
   - Extract PAN number pattern

## Running it

Install dependencies:
```bash
pip install -r requirements.txt
```

Or with Docker:
```bash
docker build -t pan-ocr .
docker run pan-ocr python src/tpan.py [input_image]
```

Usage:
```bash
python src/tpan.py path/to/pan_card.jpg
```

Output is a JSON object with the extracted fields.

## Dependencies

- Python 3
- OpenCV (`opencv-python`)
- Tesseract + `pytesseract`
- NumPy, SciPy, Pillow
- `dateparser`, `difflib`

## Project structure

```
src/
  tpan.py              — main entry point
  tpanold.py           — earlier version
  box.py               — bounding box utilities
  crop_morphology.py   — morphological cropping
  crop_to_box.py       — crop to detected text box
  namedb.csv           — name reference database
```
