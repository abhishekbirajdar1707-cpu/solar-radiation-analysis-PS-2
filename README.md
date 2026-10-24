# Solar Radiation: Analysis and Prediction (STAC WARS PS 2)

Analysis of the Voyage-123 solar observation dataset: visualisation, inferences, a day-wise solar report and a machine-learning model that predicts solar radiation.

**Full report with plots, inferences and scores:** [`report/Report.pdf`](report/Report.pdf)

## Dataset
`data/SolarPrediction.csv` has 32,686 readings taken every 5 minutes from 1 Sep to 31 Dec 2016 (118 days). Columns: UNIXTime, Data (date), Time, Radiation, Temperature, Pressure, Humidity, WindDirection(Degrees), Speed, TimeSunRise, TimeSunSet. There are no missing values.

## Repository structure
```
├── README.md
├── requirements.txt
├── data/
│   └── SolarPrediction.csv
├── notebooks/
│   └── solar_analysis.ipynb      # all code: cleaning, Parts A, B, C and the ML bonus
├── plots/                        # 8 saved figures used in the report
└── report/
    ├── Report.pdf                # final report
    └── daily_solar_report.csv    # day-wise max and average radiation
```

## How to run
```bash
git clone https://github.com/abhishekbirajdar1707-cpu/solar-radiation-analysis-PS-2.git
cd solar-radiation-analysis-PS-2
python3 -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook notebooks/solar_analysis.ipynb
```
Run all cells from top to bottom. Plots are saved to `plots/`.

## What was done
| Part | Summary |
|------|---------|
| Data preparation | Built a proper datetime column from the date and time columns, sorted oldest-first, converted sunrise and sunset to datetimes |
| A. Visualisation | Radiation over time, by hour of day, vs temperature / humidity / pressure / wind speed, correlation heatmap, outlier boxplots, bonus monthly-by-hour plot |
| B. Inferences | Peak time, strongest relationship, temperature-humidity patterns, outliers and data gaps, plus two bonus insights |
| C. Solar day analysis | Daily max and average radiation (Pandas groupby), best day, peak time and its conditions, time after sunrise, top 5 days |
| Bonus. Prediction | Random Forest regression vs a Linear Regression baseline, evaluated with RMSE and R² |

## Key findings
- Radiation peaks at **12:00-12:59** (average about 727 W/m²) and is near zero at night.
- **Temperature** has the strongest relationship with radiation (r = 0.73). Radiation falls sharply when humidity exceeds 80%.
- Outliers: humidity readings above 100% (4,311 rows, likely sensor error), wind gusts up to 40.5, and a single 1,601 W/m² spike on 4 Sep.
- Highest radiation: **2016-09-04 at 12:15:04**, about 6 h 07 min after sunrise. The top 5 days by average radiation are all in September.
- December average radiation is about 39% lower than September to November, more than shorter daylight alone explains.

## Model results (test set)
| Model | RMSE (W/m²) | R² |
|-------|-------------|----|
| Linear Regression | 191.27 | 0.624 |
| **Random Forest** | **75.81** | **0.941** |
| Random Forest (train on past, test on future) | 134.54 | 0.743 |

Most useful features: Temperature (56.8%) and hour of day (30.4%). The random split is optimistic because neighbouring 5-minute readings are near-identical, so the train-on-past / test-on-future score is a more realistic estimate for new days.

## Tools
Python, NumPy, Pandas, Matplotlib, Seaborn, scikit-learn, Jupyter.
