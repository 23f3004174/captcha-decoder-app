# CAPTCHA OCR Decoder

## Overview
A simple web app that lets you upload a CAPTCHA image and automatically decodes its text using Tesseract OCR. The front-end is a single HTML page; the back-end is a small Python Flask server using pytesseract and Pillow to process the image.

What’s inside:
- Image upload with drag-and-drop and live preview
- Automatic OCR as soon as an image is selected (toggleable)
- Optional pre-processing (binarization + denoise) for noisy CAPTCHAs
- Tesseract OCR with selectable Page Segmentation Mode (PSM)
- Optional character whitelist to focus on expected symbols
- Results shown on the page with a copy-to-clipboard button

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
- Recommended: create/activate a virtual environment
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
- Drag and drop a CAPTCHA image into the upload area, or click to choose a file.
- Keep “Enhance pre-processing” enabled for better results on noisy images.
- Choose a PSM suited to your CAPTCHA (7: single line; 8: single word; 13: raw line).
- Optionally set “Allowed characters (whitelist)” to limit recognition to certain characters, e.g. ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789.
- The app will auto-decode after selection if the toggle is on; or click “Decode CAPTCHA”.
- The recognized text is displayed on the right. Click “Copy” to copy it.

Notes:
- Max file size is 5 MB.
- The backend normalizes whitespace and returns a single line of text.
- If results are empty or incorrect, try different PSMs and enable preprocessing.

## Improvements (Round 2)
This revision enhances the original project with:
- Drag-and-drop uploader with instant preview and auto-decode on select
- Copy-to-clipboard for decoded text
- PSM selection for better tuning across CAPTCHA styles
- Character whitelist input (with A-Z, a-z, 0-9 range expansion support)
- More robust pre-processing pipeline: EXIF auto-orientation, autocontrast, median denoise, optional binarization, and stroke thickening
- Better status and timing indicators, plus a small health check endpoint

## Troubleshooting
- Ensure Tesseract is installed and accessible. If pytesseract cannot find it, set TESSERACT_CMD as described.
- If OCR is poor:
  - Enable “Enhance pre-processing”
  - Try a different PSM (7 or 8 often work well for CAPTCHAs)
  - Provide a character whitelist that matches your CAPTCHA (e.g., uppercase letters and digits only)
- Check the server logs for errors and confirm the /healthz endpoint returns status ok.