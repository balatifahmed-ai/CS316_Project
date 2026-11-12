# Next-hour solar radiation in eight Saudi cities

For our CS316 project, we used hourly Open-Meteo weather and solar reanalysis data to predict the next hour’s global horizontal irradiance (GHI), measured in W/m². The dataset contains 420,864 hourly records from 2020–2025 for Abha, Dammam, Jeddah, Medina, Najran, Riyadh, Sakaka, and Tabuk.

## Run the notebook

Open `Data Summary (6).ipynb` in Google Colab and run the cells from top to bottom. The download cell uses `gdown` and `unzip` to fetch and extract the project archive. If you already have the archive, extract the eight city CSVs under `openmeteo_history/` and skip the download cell. The analysis uses pandas, NumPy, Matplotlib, and scikit-learn.

## How we tested the models

We checked hourly coverage and data quality, then paired each city’s current inputs with its GHI one hour later. We trained on 2020–2023 and compared feature sets on 2024. We also tested four tree depths. We refit the selected models through 2024 and evaluated them on 70,080 forecast hours in 2025. All splits use the year of the predicted hour.

| Model | Inputs | 2025 MAE (W/m²) |
| --- | ---: | ---: |
| Persistence (repeat the current GHI) | 1 | 71.88 |
| Final linear regression | 17 | 22.80 |
| Compact decision tree | 4 | 10.30 |
| Full decision tree | 15 | 9.69 |

MAE is the average absolute error across all hours, including nighttime. The compact tree uses current GHI, its change since the previous hour, current cloud cover, and the forecast hour. It needs two consecutive GHI readings. Its average error was only 0.61 W/m² higher than the full tree’s. The notebook also compares errors by city and reports F1 scores after grouping radiation into four levels.

## Solar panel estimate

We estimated electricity output for one horizontal 400 W panel with 20% system losses. Using six-year average output, full household use, SAR 0.21/kWh, and a SAR 1,000 installation cost, estimated savings from the start of 2027 through the end of 2030 were SAR 617.9 in Najran, SAR 608.0 in Tabuk, and SAR 596.2 in Abha. These are scenario estimates based on historical reanalysis data.
