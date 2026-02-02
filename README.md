# finedust-nn

![Static Badge](https://img.shields.io/badge/status-done-brightgreen?style=for-the-badge)
![Static Badge](https://img.shields.io/badge/type-class_project-blue?style=for-the-badge)

An AI project for fine dust (PM10, PM2.5) prediction using time-series and image data. Developed for the 2023 Artificial Intelligence class at Seoul Science High School (서울과학고등학교). The project combines tabular weather/air-quality data with wind-map images to improve prediction, and explores which data sources help most.

## How to Start

### Environment

-   Python 3.8+
-   Jupyter Notebook or JupyterLab
-   Git

### Dependencies

-   Data & numerics: pandas, numpy, openpyxl, xlrd
-   ML/DL: tensorflow (Keras), scikit-learn
-   Visualization: matplotlib, seaborn
-   Data collection: requests, selenium
-   Image: Pillow (PIL)

### Quick Start

```bash
# Clone the repository
git clone https://github.com/davidKMJ/finedust-nn.git
cd finedust-nn

# Create a virtual environment (recommended)
python -m venv venv
source venv/bin/activate   # On Windows: venv\Scripts\activate

# Install dependencies
pip install pandas numpy openpyxl xlrd tensorflow scikit-learn matplotlib seaborn requests selenium Pillow

# Run notebooks in order
# 1. eda.ipynb  — data collection & preprocessing
# 2. ml.ipynb   — model training & evaluation
```

Place Seoul air quality, weather, and reference files in `content/` as in the project layout. Preprocessed CSVs (`df_final_2022.csv`, `df_real_final_2022.csv`) and optional wind-map images in `windmap/` are produced by the EDA notebook. The service keys in the files are deleted and no longer valid.

## Key Features

1. Multi-source data – Seoul air quality (PM10, PM2.5), weather (temp, wind, humidity, pressure, cloud, visibility, etc.), and CO (ppm) from Seoul Open API
2. Wind-map images – Hourly wind maps from [earth.nullschool.net](https://earth.nullschool.net) (crawled with Selenium) for synoptic-scale wind patterns
3. Tabular RNN/LSTM – 24-hour and 1-week (168 h) sequences to capture daily/weekly periodicity
4. CNN on wind maps – Learns spatial patterns in wind maps for PM prediction
5. Combined model – LSTM (tabular) + CNN (wind map) with transfer learning; final MAE ~3.66 for PM10/PM2.5
6. Feedback-driven design – Traffic data replaced with CO; wind maps justified by model improvement; sequence length tuned for periodicity

## Technical Stack

-   Python – Main language
-   TensorFlow / Keras – SimpleRNN, LSTM, CNN, combined Sequential/Functional models; EarlyStopping
-   scikit-learn – Train/test split, MinMaxScaler, OneHotEncoder
-   pandas / NumPy – Data loading, merging, sequence creation
-   matplotlib / seaborn – EDA and result plots
-   Selenium – Wind-map screenshot crawling
-   Pillow – Image loading and GIF creation (e.g. `top_*_pm10_windmap.gif`, `bottom_*_pm10_windmap.gif`)

## Project Structure

```
finedust-nn/
├── content/                    # Raw and reference data
│   ├── 서울시 대기질 자료 제곽_2022.csv/xlsx  # Seoul air quality 2022
│   ├── 01월~12월 서울시 교통량 조사자료(2022).xlsx
│   ├── 지점명.xlsx, 운형정보.xlsx, 현상번호.xlsx  # Station & weather codes
│   ├── 리턴파라미터정보.xlsx, 입력파라미터정보_미세먼지.xlsx
│   └── 고속도로_위치별지점id.xls, 일반도로_위치별지점id.xls, 미세먼지station리스트.xls
├── eda.ipynb                   # EDA & preprocessing
├── ml.ipynb                    # Models & evaluation
├── df_merged_2022.csv, df_final_2022.csv, df_real_final_2022.csv  # Preprocessed data
├── df_combined.csv             # Optional CO/time-series merge output
├── model3.h5, model4.h5, model8.h5, model_combined1.h5   # Saved Keras models
├── top_*_pm10_windmap.gif, bottom_*_pm10_windmap.gif     # Wind-map visualizations
└── README.md
```

## Data Sources

-   Seoul air quality: [Seoul Metropolitan Government](https://data.seoul.go.kr) – hourly PM10, PM2.5 (2022)
-   Weather: Korea Meteorological Administration – station-based hourly weather (reference: [data.go.kr](https://www.data.go.kr))
-   CO (ppm): Seoul Open API – [TimeAverageCityAir](http://data.seoul.go.kr/dataList/OA-2221/S/1/datasetView.do)
-   Wind maps: [earth.nullschool.net](https://earth.nullschool.net) – wind/surface layer (crawled for Korea region)
