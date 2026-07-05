# Air Dust Size Measurement

Air Dust Particle size measurement using ML models — analysis and experiments to compare reference instrument data with low-cost sensor (LCS) readings, preprocess hourly-averaged data, explore sensor channel agreement, and build calibration / ML models for PM2.5 estimation.

## Table of contents
- Project overview
- Files
- Data
- Notebook: what it does
- Requirements
- How to run
- Reproducibility notes
- Results & examples
- Suggested next steps
- Contributing
- License
- Contact

## Project overview
This repository contains exploratory analysis and preprocessing for air dust (particulate matter, PM) size and concentration measurements. The goal is to compare a reference instrument dataset (high-quality measurements) with a low-cost sensor (LCS) dataset, clean and aggregate data to hourly averages, evaluate sensor channels, and prepare input for machine learning calibration models to improve LCS accuracy.

## Files
- `Air_Dust_size_measurment.ipynb` — main Jupyter/Colab notebook with data loading, cleaning, hourly-averaging, visualization and analysis steps.
- `data/` (expected) — place your CSVs here (see Data section).
  - `Autumn_Oct&Nov_B12491_AV.csv` — reference instrument / seasonal dataset (example name used in notebook).
  - `NE_50_E-BAM_AV_2023.csv` — low-cost sensor dataset (example name used in notebook).
- `README.md` — this file (project description and instructions).

## Data
Two primary CSV datasets are used in the notebook:
- Reference instrument (Autumn): contains columns such as `Time`, `ConcRT(ug/m3)`, `ConcHR(ug/m3)`, `AT(C)`, `RH(%)`, etc.
- LCS (Low-cost sensor): contains `TIMESTAMP`, PM channel columns e.g., `PM 2.5 CF-1-A`, `PM 2.5 CF-1-B`, humidity, pressure, etc.

Note: Update the dataset filenames or paths in the notebook if your files are located elsewhere. The notebook expects date/time formats like `%d-%m-%Y %H:%M` or `%d-%m-%Y %H:%M:%S` depending on the file.

## Notebook: what it does
- Imports libraries (pandas, numpy, matplotlib, etc.).
- Loads the two CSV files into DataFrames.
- Inspects dataset heads to understand columns and shapes.
- Cleans the reference dataset by dropping rows with missing key fields and removing unrealistic values.
- Converts timestamps to datetime, groups or resamples data to hourly averages, and formats time strings for merging/visualization.
- For the LCS dataset: filters out extreme outliers on channel readings, computes channel average PM2.5 (`PM₂.₅-AVG`), resamples to hourly averages.
- Visualizations: scatter plots to check agreement between LCS channels, time-series comparisons, and other exploratory plots to inform calibration modeling.
- Prepares data for machine learning calibration (feature selection, alignment of timestamps between reference and LCS).
- (Optional) Suggested steps to train calibration models (e.g., linear regression, random forest, gradient boosting) and evaluate using metrics like RMSE, MAE, and R².

## Requirements
- Python 3.8+
- pandas
- numpy
- matplotlib
- scikit-learn (if you train ML models)
- jupyter or use Google Colab (recommended for ease)

Install with pip:

```bash
pip install pandas numpy matplotlib scikit-learn jupyter
```

## How to run
1. Using Google Colab (recommended)
   - Open the notebook link in Colab or upload the notebook to Colab.
   - Upload the CSV files to Colab (or mount Google Drive and update file paths).
   - Run the notebook cells top-to-bottom.

2. Locally
   - Ensure required packages are installed.
   - Place datasets in a `data/` folder or update the notebook file paths.
   - Start Jupyter: `jupyter notebook`
   - Open `Air_Dust_size_measurment.ipynb` and run cells.

## Tips and reproducibility notes
- Timestamp formats: confirm the string format used in your CSVs matches the `pd.to_datetime` format used in the notebook (the notebook uses formats like `%d-%m-%Y %H:%M` and `%d-%m-%Y %H:%M:%S`). Update the format strings if your inputs differ.
- When resampling to hourly averages: the notebook uses pandas resample/groupby with hourly bins. Make sure the index is a datetime index before resampling.
- Missing data: the notebook drops NA rows for key reference columns and performs range filtering. If you want to preserve more data, consider imputing or flagging missing rows instead of dropping them.
- Outliers: LCS channel clipping (e.g., `<= 2000`) is an arbitrary bound—adjust according to sensor range and expected environmental values.

## Results & examples
- Scatter plots showing agreement between LCS sensor channels (CF-1-A vs CF-1-B).
- Hourly-averaged time series for reference vs LCS average PM₂.₅.

Example ML calibration workflow (suggested):
- Merge reference and LCS hourly data on aligned timestamps.
- Features: LCS PM channel averages, humidity, temperature, pressure, wind, etc.
- Models to try: `LinearRegression`, `RandomForestRegressor`, `GradientBoostingRegressor`.
- Evaluate with train/test split and metrics (RMSE, MAE, R²). Use cross-validation.

## Suggested next steps / improvements
- Add a cleaning step for timestamp timezone normalization.
- Implement automated alignment logic for time windows when sampling rates differ (e.g., nearest-hour join or rolling-mean alignment).
- Add a dedicated script (e.g., `src/preprocess.py`) to produce a cleaned CSV that downstream ML scripts can consume.
- Save model artifacts and evaluation reports in a `results/` folder.
- Add unit tests for preprocessing functions (if you convert notebook parts into `.py` modules).
- Explore humidity and temperature correction features for calibration models.

## Contributing
- Create issues for bugs or feature requests.
- If converting notebook code to scripts, please follow the repository structure and add tests.
- Provide a short PR description and mention reproducible steps.

## License
This project does not include a license file yet. If you want, I can add an MIT license or another license of your choice.

## Contact
For questions or help, reach out via your GitHub profile: `https://github.com/ajaypayer` or add your preferred contact information here.
