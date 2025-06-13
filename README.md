# Predicting Handover to the Nearest Base Station

This project predicts the optimal hand-off (handover) to the nearest or most suitable base station (BTS) in a cellular network, aiming to prevent call drops as users move. It leverages geospatial and operational BTS data, graph-based modeling, and neural networks to recommend the best candidate BTS for handover.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Data Columns](#data-columns)
- [Requirements](#requirements)
- [Usage](#usage)
- [Model Details](#model-details)
- [Output](#output)
- [Notes](#notes)

---

## Overview

Seamless connectivity in mobile networks is critical. This project predicts the best next BTS for a user device to connect to, based on the current BTS and user location.

The workflow includes:
- Data cleaning and preprocessing
- Feature engineering (geographical and categorical features)
- Proximity-based graph construction
- Graph Convolutional Network (GCN) model for handover prediction
- Real-time recommendation logic considering model output and operational factors

---

## Features

- Cleans and filters BTS data for active sites with valid geolocation
- Constructs a proximity-based graph of BTS nodes
- Encodes categorical and numerical features for modeling
- Trains a GCN to predict likely handover targets
- Provides top-N handover recommendations for a given BTS and user location

---

## Data Columns

The following columns are expected in the input Excel file:

- `bts_id`
- `bts_name`
- `bts_site_id`
- `omcr_name`
- `bsc_name`
- `circle_id`
- `circle_name`
- `ssa_id`
- `ssa_name`
- `city_name`
- `vendor_name`
- `dist_name`
- `media`
- `operator_name`
- `bts_site_location`
- `bts_ip_id`
- `bts_ip_id_new`
- `bts_type`
- `bts_status`
- `bts_area`
- `site_category`
- `tower_type`
- `site_type`
- `latitude`
- `longitude`
- `cell`
- `nameWebsite`
- `e_p`
- `outsrc_name`
- `phase_name`
- `ofc_name`
- `SDCA_NAME`

**Key columns for modeling:**  
`bts_id`, `bts_status`, `latitude`, `longitude`, `vendor_name`, `tower_type`, `site_category`, `bts_area`

---

## Requirements

- Python 3.x
- pandas
- numpy
- scikit-learn
- geopy
- tensorflow
- Jupyter/Colab environment recommended for interactive use

---

## Usage

1. **Prepare your data**  
   Ensure your Excel file contains the required columns listed above.

2. **Run the script**  
   - Upload your data file when prompted.
   - The script will preprocess the data, train the model, and save the trained model to disk.

3. **Predict handover**  
   Use the `predict_handover` function to get top handover recommendations for a given BTS and user location.

```python
model, engineer, adj, df = train_model('your_data.xlsx')
recommendations = predict_handover(model, engineer, adj, df, current_bts_id, (user_lat, user_lon))
print(recommendations)
```

---

## Model Details

- **Feature Engineering:** Standardizes latitude/longitude, one-hot encodes categorical features.
- **Graph Construction:** Builds adjacency matrix based on BTS proximity (default threshold: 8 km).
- **Model:** Custom GCN implemented in TensorFlow, with dense layers and adjacency-based propagation.
- **Training:** Early stopping and model checkpointing for robust training.
- **Prediction:** Combines model probabilities with real-time geodesic distance and site importance for ranking.

---

## Output

- Trained model file: `bts_handover_model.keras`
- Console output: Recommended BTS handover targets for a given scenario, including their IDs, names, and coordinates.
