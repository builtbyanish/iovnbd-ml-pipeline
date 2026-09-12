# IO-VNBD ML Data Pipeline

A robust Python data processing pipeline for the [IO-VNBD (Inertial and Odometry Vehicle Navigation Benchmark Dataset)](https://github.com/onyekpeu/IO-VNBD). 

This tool automates the extraction, cleaning, and merging of hundreds of fragmented per-trip CSV files into two unified, ML-ready datasets specifically designed for GNSS-denied dead-reckoning and navigation projects.

##  Features

- **Automated Alignment:** Automatically aligns vehicle ECU data (`V-` files) and smartphone IMU/GPS data (`S-` files) on a shared time axis for synchronised trips.
- **Unified Schema:** Standardizes column namespaces (`veh_*` and `phone_*`) to prevent feature collisions during model training.
- **Encoding & Edge-Case Handling:** Handles dataset-specific quirks, such as decoding `latin1` degree symbols (e.g., m/s²) and tracking missing pairs.
- **Ground Truth & Outage Tagging:** Cross-references GPS outage records to flag rows where signal was lost, and explicitly tags rows with/without ground truth.
- **Trip Boundary Preservation:** Outputs a long-format dataset that strictly preserves `trip_id` boundaries, ensuring zero data leakage between independent driving sessions during windowing.

## Output Deliverables

The pipeline distills the IO-VNBD dataset into exactly two final CSV files:

1. `synchronised_combined.csv` (~850k rows, 32 trips)
   - Contains fully time-aligned rows containing both Vehicle and Smartphone telemetry.
   - Ideal for training supervised learning models (`has_ground_truth=True`).
   
2. `unsynchronised_combined.csv` (~5.3M rows, 356 trips)
   - Contains single-source trips in a unified schema (missing modalities are left as `NaN`).
   - Ideal for self-supervised pretraining or testing (`has_ground_truth=False`).

##  Usage

**1. Clone this repository**
```bash
git clone https://github.com/YourUsername/iovnbd-ml-pipeline.git
cd iovnbd-ml-pipeline
