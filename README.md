# QUALAR Air Quality — ETL + AQI Analytics (PCED 2023/2024)

This repository contains a course project developed for **Programming for Data Science and Engineering (PCED)**.
It implements an end-to-end workflow to integrate, clean, and analyze **Portuguese air-quality monitoring data**
from **QUALAR (Agência Portuguesa do Ambiente)**, covering **2001–2022**, including an AML (Lisbon Metropolitan Area)
focused analysis and **AQI (Air Quality Index)** computation.

## Objectives
- Consolidate QUALAR station/year files into a unified dataset
- Clean and standardize pollutant fields (and reshape into tidy/long format)
- Perform exploratory data analysis (EDA) across stations and years
- Apply formal data validity criteria for historical analysis:
  - **Valid day**: ≥ 75% hourly measurements (≥ 18 hours/day)
  - **Valid year**: ≥ 90% valid days (≥ 328 valid days/year)
  - Discard station–pollutant series with **< 2 valid years**
- Compute **daily AQI per pollutant** and **overall daily AQI**
- Analyze exceedances / violations (AML-focused)

## Data source
Data are obtained from **QUALAR (APA)**.  
**Note:** Raw data are **not included** in this repository.

## Project structure
The workflow is organized into 7 notebooks (run in order):

1. **01-Processamento_Ficheiros_Originais.ipynb**  
   Reads original QUALAR files and builds a single integrated dataset.  
   Output: `./02-dados-qualar-integrados/02-medicoes-um-ficheiro.csv`

2. **02-Mudanca_Formato_Correcoes.ipynb**  
   Cleans and standardizes columns, removes CO and benzene (project scope), converts to long/tidy format, removes missing rows,
   and sets negative values to 0.  
   Outputs:
   - `./02-dados-qualar-integrados/02_longo_com_correções.csv`
   - `./03-dados-qualar-longo-corrigido/03-medicoes-longo-AML.csv`

3. **03-AED_Medicoes.ipynb**  
   Exploratory data analysis (time series plots, station comparisons, KDE/boxplots, moving averages).

4. **04-Selecao_Estacoes_Poluentes.ipynb**  
   Applies the validity rules (valid day/year + minimum valid years) to identify reliable station–pollutant series.

5. **05-AML.ipynb**  
   Computes **daily AQI per pollutant** and **overall AQI** (overall AQI = max AQI across pollutants per day).  
   Output: `./04-dados-qualar-longo-AML/indice_qualidade_ar_diario.csv`

6. **06-AML.ipynb**  
   Exceedance / violation analysis (AML-focused).  
   Output: `./04-dados-qualar-longo-AML/PM10_violacoes.csv`

7. **07-AML.ipynb**  
   AML visual summaries and trend plots based on the AQI and violation outputs.

## Recommended local folders
The notebooks use relative paths. A typical setup is:

- `./01-dados-qualar/medicoes/` (raw QUALAR station/year files — not tracked)
- `./02-dados-qualar-integrados/` (generated CSVs)
- `./03-dados-qualar-longo-corrigido/` (tidy/clean datasets)
- `./04-dados-qualar-longo-AML/` (AML datasets + AQI/violations outputs)

Tip: add the data folders to `.gitignore` so you don’t accidentally upload raw datasets.

## Setup
Recommended: Python 3.10+

Install dependencies:

    pip install pandas numpy matplotlib openpyxl

## How to run
1) Place raw QUALAR files under `./01-dados-qualar/medicoes/`  
2) Run notebooks **01 → 07** in order.

## Notes
- AQI is computed using:
  - **PM10 / PM2.5**: daily mean
  - **SO2 / NO2 / O3**: daily maximum
  - Overall AQI: maximum across pollutants per day
- Validity criteria follow the project specification (≥75% hourly measurements per day; ≥90% valid days per year; ≥2 valid years).

## Credits
- Data source: **QUALAR (APA)**
- Project specification provided in the PCED 2023/2024 course materials.
