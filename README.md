Traffic Congestion Risk Zones

An exploratory data analysis (EDA) project for understanding traffic congestion patterns using the VANET Traffic Congestion Dataset. The notebook loads the dataset, performs basic preprocessing and feature engineering, encodes congestion labels, and visualizes relationships between traffic variables.

Overview

Traffic congestion can be studied through a combination of traffic-flow, vehicle-density, road-condition, communication, and environmental variables. This project explores these variables to understand how different traffic congestion levels are represented in the dataset.

The analysis currently focuses on:

Loading and inspecting the VANET traffic congestion dataset

Checking dataset structure and data types

Detecting missing values

Basic preprocessing

Timestamp-based feature engineering

Encoding congestion classes

Checking duplicate records

Visualizing congestion-class distribution

Comparing vehicle density with average speed

Examining correlations between numerical traffic features

Performing IQR-based outlier analysis

Dataset

The project uses the VANET Traffic Congestion Dataset downloaded through kagglehub:

ucimachinelearning/vanet-traffic-congestion-dataset

The downloaded dataset contains:

vanet_traffic_data.csv

Dataset size

Rows: 195,714

Columns: 27

Numerical columns: 24

Object columns: 3

Congestion Classes

The label column contains four congestion classes:

Encoded Value

Congestion Level

0

Free-Flow

1

Gridlock

2

Heavy

3

Moderate

The labels are encoded using LabelEncoder.

Features

The dataset contains traffic, road, environmental, and communication-related variables.

Traffic features

avg_speed_kmph

density_veh_per_km

avg_wait_time_s

occupancy_pct

flow_veh_per_hr

queue_length_veh

avg_accel_ms2

heading_deg

Traffic-control / incident features

signal_state_num

incident_num

Environmental features

temp_c

visibility_km

rain_intensity_mmph

weather_factor

VANET / communication features

channel_busy_ratio_pct

msg_rate_hz

avg_comm_delay_ms

rssi_dbm

packet_loss_pct

Derived traffic features

speed_density_ratio

congestion_pressure

wireless_congestion_intensity

throughput_per_queued_vehicle

acceleration_directionality

Metadata

timestamp

road_segment_id

label

Project Workflow

Dataset
   │
   ▼
Data Loading
   │
   ▼
Data Inspection
   │
   ├── Shape / Columns
   ├── Data Types
   ├── Descriptive Statistics
   └── Missing-Value Check
   │
   ▼
Data Preprocessing
   │
   ├── Numerical missing values → Median
   └── Timestamp conversion
   │
   ▼
Feature Engineering
   │
   ├── Timestamp Hour
   ├── Timestamp Day of Week
   └── Timestamp Month
   │
   ▼
Label Encoding
   │
   ▼
Exploratory Data Analysis
   │
   ├── Class Distribution
   ├── Speed vs Congestion
   ├── Density vs Speed
   └── Correlation Heatmap
   │
   ▼
Outlier Analysis
   │
   └── IQR Method

Data Preprocessing

Missing values

The notebook checks missing values across all columns.

For numerical columns, missing values are filled using the corresponding median:

numeric_cols = df.select_dtypes(include="number").columns

df[numeric_cols] = df[numeric_cols].fillna(
    df[numeric_cols].median()
)

The inspection in the notebook shows zero missing values for the displayed columns.

Timestamp processing

The timestamp column is converted to a datetime object and used to create:

timestamp_hour

timestamp_day_of_week

timestamp_month

Because all records shown in the analysis belong to the same month, timestamp_month is subsequently removed.

Road segment

road_segment_id is inspected to identify the different road segments represented in the dataset.

Duplicate records

The notebook checks duplicate rows using:

df.duplicated().sum()

The displayed result is zero duplicate records.

Exploratory Data Analysis

1. Traffic Congestion Class Distribution

A count plot is used to visualize the number of observations belonging to each congestion class.

sns.countplot(data=df, x="label")

This provides an overview of the distribution of the four congestion categories.

2. Average Speed vs Traffic Congestion

A strip plot is used to examine the relationship between average vehicle speed and congestion level.

Average Speed
     ▲
 80  │       Free-Flow
 60  │
 40  │       Moderate
 20  │                 Heavy
  0  │                         Gridlock
     └──────────────────────────────►
              Congestion Level

The visualization shows distinct speed ranges associated with the congestion labels in the analyzed dataset.

3. Traffic Density vs Average Speed

A scatter plot compares:

density_veh_per_km

avg_speed_kmph

with points grouped by the congestion label.

The plotted data forms visually distinct groups, showing that vehicle density and average speed provide useful separation among the four congestion classes.

4. Correlation Analysis

A correlation matrix is generated from numerical features.

The encoded label is excluded from the correlation calculation:

numeric_df = df.select_dtypes(include="number").drop(
    columns=["label_encoded"],
    errors="ignore"
)

corr = numeric_df.corr()

A heatmap is then used to visualize relationships among the numerical traffic features.

Outlier Analysis

The notebook uses the Interquartile Range (IQR) method.

For each numerical feature:

IQR = Q3 - Q1

Lower Bound = Q1 - 1.5 × IQR
Upper Bound = Q3 + 1.5 × IQR

Values below the lower bound or above the upper bound are counted as outliers.

Boxplots are generated for the numerical features to visually inspect their distributions and potential outliers.

Visualizations Included

The notebook includes visual analysis for:

Traffic congestion class distribution

Average speed vs congestion level

Traffic density vs average speed

Correlation heatmap

Boxplots for numerical features

Timestamp hour distribution

Timestamp day-of-week distribution

Technologies Used

Python

Pandas — data loading and manipulation

NumPy — numerical operations

Matplotlib — visualization

Seaborn — statistical visualization

Scikit-learn

LabelEncoder

train_test_split

RobustScaler

KMeans

silhouette_score

KaggleHub — dataset download

Google Colab — notebook environment

Installation

Install the main dependencies with:

pip install pandas numpy matplotlib seaborn scikit-learn kagglehub

Running the Project

1. Open the notebook

Open:

TrafficCongestionRiskZones.ipynb

in Google Colab or another Jupyter-compatible environment.

2. Run the cells

Execute the notebook cells from top to bottom.

The dataset is downloaded through:

import kagglehub

path = kagglehub.dataset_download(
    "ucimachinelearning/vanet-traffic-congestion-dataset"
)

The CSV file is then loaded with Pandas.

Project Structure

TrafficCongestionRiskZones/
│
├── TrafficCongestionRiskZones.ipynb
└── README.md

The exact repository structure may contain additional files depending on how the project is organized outside the provided notebook.

Current Scope

This version of the project is primarily an exploratory data analysis and preprocessing study.

The notebook imports KMeans and silhouette_score, but the provided material does not show a completed K-Means clustering experiment. Therefore, clustering results and model-performance claims are not included in this README.

Similarly, the provided notebook material does not show a completed supervised machine-learning training/evaluation pipeline, so no accuracy, precision, recall, F1-score, or prediction results are claimed here.

Potential Next Steps

Based on the current analysis, the project can be extended with:

Feature scaling using RobustScaler

K-Means clustering of traffic states

Silhouette-score evaluation

Supervised congestion-level classification

Train/test splitting

Model comparison

Feature-importance analysis

Traffic congestion risk-zone visualization

Model evaluation using classification metrics

Deployment of a congestion prediction model

These are suggested extensions and are not part of the completed analysis shown in the provided notebook.

Key Takeaways from the Current Analysis

The dataset contains 195,714 traffic observations across 27 columns.

It represents four congestion categories: Free-Flow, Gridlock, Heavy, and Moderate.

Traffic speed and vehicle density show clearly separated patterns across the labeled congestion groups in the visualizations.

The dataset includes a broad range of traffic, environmental, and VANET communication features.

Timestamp information is transformed into hour and day-of-week features.

Numerical missing values are handled with median imputation.

Correlation analysis is used to inspect relationships among numerical variables.

IQR-based boxplot analysis is used to inspect potential outliers.

Author

Mostofa Kamal Joy
