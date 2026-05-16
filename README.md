# Plasma Height Detection in Centrifuge Tube using Computer Vision

A computer vision based system for automatically detecting and measuring the plasma layer height from centrifuge tube images using Python and OpenCV.

---

# Project Overview

Plasma separation is commonly used in medical and laboratory analysis. After centrifugation, blood separates into multiple layers:

- Plasma (upper yellow layer)
- Buffy coat (thin middle layer)
- Packed red blood cells (bottom dark-red layer)

Manual measurement of plasma height can be time-consuming and may introduce human error.

This project automatically detects the plasma region from an image of a centrifuge tube and calculates the plasma height in pixels.

The system also generates a visualization by drawing a tight bounding box around the detected plasma layer.

---

# Objective

The main objectives of this project are:

- Automatically detect plasma in centrifuge tube images
- Measure plasma height
- Reduce manual measurement error
- Provide visual confirmation using bounding boxes
- Build a reusable computer vision pipeline

---

# How the System Works

The program follows several image processing stages:

```text
Input Image

↓

Convert image into HSV color space

↓

Extract center region

↓

Detect plasma upper boundary

↓

Detect plasma lower boundary

↓

Create plasma mask

↓

Find left and right plasma boundaries

↓

Draw bounding box

↓

Calculate plasma height
```

---

# Main Sections of Code

The code is divided into several logical sections.

---

## 1. Load Image

Code:

```python
img = cv2.imread(image_path)
```

Purpose:

Reads the input centrifuge tube image.

If the image cannot be loaded:

- Error message appears
- Program exits

---

## 2. Convert RGB to HSV Color Space

Code:

```python
hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
```

Purpose:

Converts image into HSV format.

HSV is used because:

- Hue represents color
- Saturation represents color intensity
- Value represents brightness

HSV is more suitable for detecting plasma colors than RGB.

---

## 3. Extract Center Strip

Code:

```python
strip = hsv[:, left:right, :]
```

Purpose:

Uses only the center region of the tube.

Reason:

Tube walls often produce:

- reflections
- shadows
- glare

Ignoring edges improves detection accuracy.

---

## 4. Detect Plasma Upper Boundary

Purpose:

Find where the plasma region begins.

The algorithm searches for:

- High saturation
- High brightness
- Continuous yellow region

Logic:

```text
High saturation

AND

High brightness

↓

Plasma detected
```

---

## 5. Detect Plasma Lower Boundary

Purpose:

Detect the transition between:

```text
Plasma

↓

Packed red blood cells
```

The algorithm looks for:

- Significant brightness drop
- Dark red region

---

## 6. Calculate Plasma Height

Formula:

```text
Plasma Height

=

Plasma Bottom − Plasma Top
```

Result:

```python
height_px = plasma_bottom - plasma_top
```

Output:

```text
Plasma height = XXX pixels
```

---

## 7. Create Plasma Mask

Purpose:

Detect plasma-colored pixels only.

The algorithm applies HSV thresholds:

```python
yellow_mask = (
    (H > 12) &
    (H < 45) &
    (S > 55) &
    (V > 155)
)
```

Purpose:

Keep:

- Yellow/orange plasma pixels

Remove:

- Background
- Tube walls
- Dark blood cells

---

## 8. Detect Horizontal Plasma Boundaries

Purpose:

Find left and right edges of plasma.

The algorithm:

- Computes plasma percentage in each column
- Removes noise
- Keeps valid plasma columns

This creates a tighter bounding box.

---

## 9. Draw Bounding Box

Code:

```python
cv2.rectangle()
```

Purpose:

Draw a green rectangle around detected plasma.

Also draws:

```python
cv2.line()
```

for a center reference line.

---

## 10. Display Final Output

The system displays:

- Original image
- Detected plasma layer
- Bounding box
- Plasma height

Example output:

```text
Detected Plasma Layer

Height: 185 pixels
```

---

# Technologies Used

- Python
- OpenCV
- NumPy
- Matplotlib

---

# Installation

Install required libraries:

```bash
pip install opencv-python numpy matplotlib
```

---

# Project Structure

```bash
Plasma-Height-Detection/

│
├── plasma_detection.py
├── testtube.jpg
├── README.md
└── requirements.txt
```

---

# How to Run

Clone repository:

```bash
git clone https://github.com/yourusername/Plasma-Height-Detection.git
```

Move into project folder:

```bash
cd Plasma-Height-Detection
```

Run:

```bash
python plasma_detection.py
```

---

# Input

Input image:

```text
testtube.jpg
```

The image should contain:

- A centrifuge tube
- Visible plasma layer
- Good lighting conditions

---

# Output

The program outputs:

### Terminal

```text
Processing image: testtube.jpg

Plasma height (pixels): 185

Final result → Plasma height = 185 pixels
```

### Visualization

Displays:

- Plasma region
- Tight green bounding box
- Reference center line
- Height information

---

# Possible Improvements

Future improvements:

- Convert pixel height into millimeters
- Support multiple tubes in one image
- Real-time camera detection
- Deep learning based segmentation
- Automatic calibration
- GUI application

---

# Applications

Possible use cases:

- Medical laboratories
- Hematology research
- Automated blood analysis
- Biomedical image processing
- Clinical decision support systems

---

# Author

Mahadi Hassan

LinkedIn:

https://www.linkedin.com/in/mahadi-hassan-b25813343/
