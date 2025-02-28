# Computer Vision Project 2025

This project implements various vehicle detection methods using computer vision techniques:
1. CV2 background removal
2. YOLOv8 object detection
3. Semantic segmentation with SegFormer

## Environment Setup

```bash
# Create a virtual environment
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

## Methods

### 1. Background Subtraction (CV2)
Uses OpenCV's background subtractor to detect moving objects in video.

### 2. YOLOv8 Object Detection
Implements YOLOv8 to identify and classify vehicles with bounding boxes.

### 3. SegFormer Semantic Segmentation
Uses a pre-trained SegFormer model to perform pixel-level segmentation of vehicles.