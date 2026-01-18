# SAR Ship Detection with Gemma 3 270M

<img width="3788" height="1253" alt="image" src="https://github.com/user-attachments/assets/7e51ef0e-7280-4ff3-a20c-63ff680d70c6" />


Automated maritime surveillance system using Sentinel-1 SAR satellite imagery and Google's Gemma 3 270M language model for ship detection and classification.

## Overview

Production-ready pipeline for detecting and classifying ships from Synthetic Aperture Radar (SAR) data. Combines signal processing with lightweight AI for efficient maritime monitoring on standard hardware.

## Key Features

- Direct integration with Google Earth Engine for Sentinel-1 SAR data
- Automated ship detection using threshold-based bright-spot detection
- AI-powered classification via Gemma 3 270M (270M parameters, 540MB)
- Memory-optimized for Google Colab free tier
- CSV export for GIS integration
- Detailed ship metrics: position, size, type, confidence

## System Architecture

```
Sentinel-1 SAR → Denoising → Ship Detection → Feature Extraction → Gemma 3 270M → Classification
```

## Detected Ship Types

- Small Vessel (fishing boats, yachts, patrol boats)
- Fishing Vessel (trawlers, seiners)
- Medium Cargo (feeder ships, coastal cargo)
- Container Ship (large cargo vessels)
- Bulk Carrier (grain, coal carriers)
- Tanker (oil, chemical tankers)
- Large Ship (VLCC, large carriers)
- Offshore Platform (stationary structures)

## Installation

```bash
pip install earthengine-api opencv-python-headless scikit-image transformers accelerate bitsandbytes
```

## Quick Start

```python
# Initialize Google Earth Engine
PROJECT_ID = 'your-gee-project-id'
init_gee()

# Load Gemma 3 270M
model, tokenizer = load_gemma_model()

# Analyze maritime area
results = analyze_with_gemma(
    lon=103.85,  # Singapore Strait
    lat=1.28,
    size_km=5
)

# Export results
export_csv(results, 'singapore')
```

## Technical Specifications

**Image Processing:**
- Resolution: 512x512 pixels
- Pixel size: ~10 meters/pixel
- Coverage: 5x5 km default
- Preprocessing: Gaussian blur (7x7 kernel)

**Detection:**
- Method: Percentile-based thresholding (99.5th percentile)
- Morphological filtering for noise removal
- Size filtering: 10-300 pixels (100-30,000 m²)

**Classification:**
- Model: Gemma 3 270M instruction-tuned
- Input: Physical features (length, width, aspect ratio, intensity)
- Method: Text-based reasoning from measurements
- Confidence: 70-85% depending on feature clarity

**Performance:**
- Memory usage: <2GB (image + model)
- Speed: ~1 second per ship classification
- GPU: Optional (runs on CPU)

## Example Results

**Singapore Strait (103.85°E, 1.28°N):**
- Total ships: 17
- Medium Cargo: 16
- Bulk Carrier: 1
- Processing time: ~20 seconds

**Rotterdam Port (4.0°E, 51.95°N):**
- Total ships: Varies by date
- Mix of container ships, bulk carriers, tankers

## Applications

**Maritime Security:**
- Unauthorized vessel detection
- Restricted zone monitoring
- Dark vessel identification

**Port Operations:**
- Traffic density analysis
- Congestion monitoring
- Vessel classification statistics

**Environmental:**
- Oil spill source identification
- Offshore platform monitoring
- Maritime traffic patterns

**Research:**
- Ship detection algorithm validation
- Maritime activity trends
- Coastal surveillance

## Validation

Compare results with AIS (Automatic Identification System) ground truth:

**Free AIS Sources:**
- MarineTraffic (marinetraffic.com)
- VesselFinder (vesselfinder.com)
- AISHub (aishub.net)

**Academic Datasets:**
- NASA ShipDetectionNet
- Sentinel-1 Ship Detection Dataset (xView3)

## File Structure

```
SAR_Ship_Detection_Gemma3_270M.ipynb  # Main notebook
├── Authentication (Google Earth Engine)
├── Model Loading (Gemma 3 270M)
├── SAR Data Loader (512x512)
├── Ship Detector (threshold-based)
├── Gemma Classifier (feature-based)
├── Visualization (3-panel display)
└── Export Functions (CSV)
```

## Limitations

- Resolution: 512x512 limits detection of vessels 
- Small ships may be missed due to threshold settings
- Weather conditions affect SAR quality
- Classification relies on size-shape heuristics

## Future Improvements

- Higher resolution option (1024x1024)
- CFAR (Constant False Alarm Rate) detector
- Rotated bounding boxes for orientation
- Temporal tracking across multiple acquisitions
- Fine-tuned Gemma on SAR ship data
- AIS integration for automatic validation

## Model Details

**Gemma 3 270M:**
- Released: December 2024
- Parameters: 270 million
- Size: 540MB (float16)
- Quantization: 4-bit option available
- Context window: 8192 tokens
- Instruction-tuned for structured outputs

**Why Gemma 3 270M:**
- 10x faster than Gemma 2B
- CPU-friendly (no GPU required)
- Minimal memory footprint
- Excellent reasoning for feature-based classification
- State-of-the-art efficiency for tiny models

## Data Sources

**Sentinel-1:**
- Provider: European Space Agency (ESA) / Copernicus
- Instrument: C-band Synthetic Aperture Radar
- Mode: IW (Interferometric Wide Swath)
- Polarization: VV
- Revisit time: 6-12 days
- Resolution: 10m (ground range)

## Requirements

- Python 3.8+
- Google Earth Engine account (free)
- 4GB RAM minimum
- GPU optional (runs on CPU)
- Internet connection for Sentinel-1 download

## License

MIT License - Free for non profit use (check Google's License)

## Citation

```
SAR Ship Detection with Gemma 3 270M
Uses Sentinel-1 data from ESA/Copernicus
Uses Gemma 3 270M from Google DeepMind
```

## Contributing

Pull requests welcome for:
- Detection algorithm improvements
- Additional ship type categories
- AIS integration
- Performance optimizations
- Validation metrics

## Acknowledgments

- ESA/Copernicus for Sentinel-1 data
- Google DeepMind for Gemma 3 270M
- Google Earth Engine for data infrastructure
