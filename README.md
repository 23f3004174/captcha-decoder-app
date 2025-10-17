# CAPTCHA OCR Decoder

## Overview
A simple web app to upload a CAPTCHA image and decode its text using OCR. The front-end is a single HTML page; the back-end is a small Python Flask server that uses pytesseract (Tesseract OCR) and Pillow to process the image.

Features:
- Image upload form with instant preview
- Optional pre-processing (binarization) for noisy CAPTCHAs
- Python OCR via pytesseract with Page Segmentation Mode optimized for single-line text
- Displays decoded text directly on the page

## Setup

1) Install Tesseract OCR (required by pytesseract)
- macOS (Homebrew):
  - brew install tesseract
- Ubuntu/Debian:
  - sudo apt-get update
  - sudo apt-get install -y tesseract-ocr
- Windows:
  - Download and install from https://github.com/UB-Mannheim/tesseract/wiki
  - After installation, note the path to tesseract.exe (e.g., C:\Program Files\Tesseract-OCR\tesseract.exe)

2) Install Python dependencies
- Create/activate a virtual environment if desired
- Install required packages:
  - pip install flask pillow pytesseract

3) Create the backend file
- Open index.html and locate the section titled:
  - “Backend (Python Flask) — copy this into app.py”
- Copy that code into a new file named app.py in the same directory as index.html.

4) (Optional) Configure Tesseract binary path
- If Tesseract isn’t on your PATH, set an environment variable before running:
  - Windows (PowerShell):
    - $env:TESSERACT_CMD = "C:\Program Files\Tesseract-OCR\tesseract.exe"
  - macOS/Linux (bash/zsh):
    - export TESSERACT_CMD=/usr/local/bin/tesseract
- The backend reads TESSERACT_CMD to locate the Tesseract binary.

5) Run the server
- python app.py
- Open your browser at http://127.0.0.1:5000/

## Usage
- Click “Choose CAPTCHA image” to select an image file (PNG/JPG/GIF, etc.)
- Optionally enable “Enhance pre-processing (binarize)” to improve results on noisy CAPTCHAs
- Click “Decode CAPTCHA”
- The detected text will be shown on the page under “OCR completed.”
- Click “Clear” to reset the form

Notes:
- The backend uses pytesseract with --psm 7 (treat image as a single text line), which often works well for CAPTCHA-like images.
- For best results, try the “Enhance pre-processing” option if the default pass yields empty or incorrect text.