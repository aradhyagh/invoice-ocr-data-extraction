# Invoice OCR to DataFrame

## Project Overview

This project automates the process of extracting structured data from invoice images using **OCR (Optical Character Recognition)** with **Tesseract** and **OpenCV**.  
It processes uploaded invoice images, detects all words along with their coordinates, and outputs a structured **pandas DataFrame** containing each word’s position and confidence.

---

## 🚀 Key Features

- Read invoice images and apply preprocessing (grayscale, thresholding).
- Use `pytesseract.image_to_data()` to extract word-level text data with coordinates.
- Organize extracted words into a structured DataFrame.
- Optionally visualize bounding boxes around detected words.
- Export extracted data to CSV for further processing or analysis.

---

## ⚡ Usage Example

```python
import cv2
import pytesseract
import pandas as pd

# Load image
image = cv2.imread('path_to_invoice_image.jpg')

# Convert to grayscale
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

# Apply thresholding
processed = cv2.threshold(gray, 150, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)[1]

# Extract OCR data with word locations
ocr_data = pytesseract.image_to_data(processed, output_type=pytesseract.Output.DATAFRAME)

# Filter out empty text results
ocr_data = ocr_data[ocr_data['text'].notna() & (ocr_data['text'].str.strip() != '')]

# Save extracted data to CSV
ocr_data.to_csv('extracted_invoice_data.csv', index=False)

# Example: Print first few rows
print(ocr_data.head())
