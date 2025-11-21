# Geospatial Assessment of Environmental Liabilities: Water Stress and Climate Resilience along Tanzania's Belt and Road Initiative Corridors.

This repository contains all supplementary materials for the manuscript submitted to Environmental Challenges. It includes the complete analysis code (Google Earth Engine JavaScript API and Python scripts), pre-processed datasets, detailed results, and the full set of supplementary tables and figures necessary to fully reproduce the study's findings of the paper titled "Quantifying Environmental Liabilities Along Tanzania's Development Corridors: A Geospatial Assessment of Water Stress and Climate Resilience."

## Quick Start

### Installation
```bash
git clone https://github.com/AugustKizito/tanzania-bri-environmental-risk.git
cd tanzania-bri-environmental-risk
pip install -r requirements.txt
# Calculate Water Stress Index
python scripts/water_stress_analysis.py

# Generate Climate Resilience Index components
python scripts/climate_resilience_decomposition.py

# Create spatial vulnerability maps
python scripts/spatial_heterogeneity_mapping.pytanzania-bri-environmental-risk/
├── scripts/               # Analytical code
├── data/synthetic/        # Synthetic datasets
├── docs/                  # Documentation
├── requirements.txt       # Python dependencies
└── README.md             # This fileData Availability
Synthetic datasets in /data/synthetic/ replicate the structure and statistical properties of original data while preserving confidentiality. Original data sources include:

Sentinel-2 MSI (ESA Copernicus)

WorldClim Bioclimatic Variables

MODIS Vegetation Indices

Author-compiled BRI infrastructure dataCore geospatial libraries
geopandas>=0.12.0
rasterio>=1.3.0
xarray>=2023.0.0

Analysis and modeling
scikit-learn>=1.2.0
numpy>=1.21.0
pandas>=1.5.0
scipy>=1.10.0

Visualization
matplotlib>=3.7.0
seaborn>=0.12.0
contextily>=1.3.0

Google Earth Engine
earthengine-api>=0.1.350Utilities
jupyter>=1.0.0
tqdm>=4.65.0
## **3. Basic Script Templates**

**A. water_stress_analysis.py** (save in `scripts/` folder):
```python
"""
Water Stress Index (WSI) calculation from MNDWI temporal variability
"""

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

def calculate_ws(mndwi_series):
    """
    Calculate Water Stress Index as coefficient of variation of MNDWI
    
    Parameters:
    mndwi_series: pandas Series of MNDWI values
    
    Returns:
    wsi: Water Stress Index value
    """
    cv = mndwi_series.std() / mndwi_series.mean()
    return cv

def main():
    """Main analysis workflow"""
    print("Loading synthetic MNDWI data...")
    
    # Load synthetic data
    mndwi_data = pd.read_csv('../data/synthetic/mndwi_timeseries.csv')
    
    # Calculate WSI for each location
    mndwi_data['WSI'] = mndwi_data.groupby('location_id')['mndwi_value'].transform(calculate_ws)
    
    # Identify high-stress areas (WSI > 0.80)
    high_stress = mndwi_data[mndwi_data['WSI'] > 0.80]
    print(f"High water stress locations: {high_stress['location_id'].nunique()}")
    
    # Save results
    mndwi_data.to_csv('../data/synthetic/ws_results.csv', index=False)
    print("Analysis complete. Results saved to ../data/synthetic/ws_results.csv")

if __name__ == "__main__":
    main()
