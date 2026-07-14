# Weather Data ETL Pipeline

**AnalystLab Africa Data Analytics Internship — Batch B, Week 7**
**Project:** Data Pipelines & Automation

## Project Overview

This project builds a simple ETL (Extract, Transform, Load) pipeline in Python that retrieves real-time weather data for three cities — Mombasa, Nairobi, and London — using the OpenWeather API. The pipeline extracts raw weather data, transforms it into a clean, structured format, and loads it into a CSV file ready for analysis. A short comparative analysis of the three cities is also included.

The goal of this project is to understand how organizations automatically collect and prepare data from external sources (like APIs) before it can be analyzed — a core skill for any data analyst working with live or frequently updated data.

## Data Source

**OpenWeather API** — [https://openweathermap.org/api](https://openweathermap.org/api)

The **Current Weather Data** endpoint was used to retrieve live weather conditions for each city:
```
https://api.openweathermap.org/data/2.5/weather
```

Data was requested in metric units (Celsius, m/s) using each city name as a query parameter, along with a personal API key generated through a free OpenWeather account.

## Cities Selected

Three cities were deliberately chosen to represent distinct climate types, so the comparison would surface meaningful differences rather than three similar readings:

- **Mombasa, Kenya** — coastal, tropical
- **Nairobi, Kenya** — highland, tropical
- **London, UK** — temperate

## ETL Process

### 1. Extract
Using Python's `requests` library, the pipeline sends a GET request to the OpenWeather API for each city and receives the response as JSON. The raw JSON includes nested fields for coordinates, weather conditions, temperature/humidity/pressure, wind, and sunrise/sunset times.

### 2. Transform
A custom function (`extract_weather_fields`) pulls the relevant fields out of the nested JSON response into a flat dictionary per city:
- City name
- Temperature (°C)
- Humidity (%)
- Weather condition (main category + description)
- Wind speed (m/s)
- Date and time of reading

The three city dictionaries are combined into a single pandas DataFrame. The API's Unix timestamp (`dt` field) was converted into a readable datetime, and split into separate **UTC** and **East Africa Time (EAT, UTC+3)** columns, since OpenWeather returns timestamps in UTC by default — this avoids confusion when comparing cities across time zones.

### 3. Load
The final cleaned DataFrame is saved as a CSV file (`weather_data.csv`) for further analysis and reproducibility.

## Tools Used

- Python
- Jupyter Notebook (VSCode)
- `requests` — API calls
- `pandas` — data structuring and transformation
- OpenWeather API — data source

## Steps Taken

1. Created a free OpenWeather account and generated an API key.
2. Selected three cities representing different climate types (Mombasa, Nairobi, London).
3. Wrote a Python function to call the API and retrieve raw JSON weather data for each city.
4. Extracted relevant fields (temperature, humidity, condition, wind speed, datetime) into a flat structure.
5. Converted timestamps into both UTC and EAT for clarity.
6. Verified data types (numeric fields as float/int, datetime as proper datetime type).
7. Saved the cleaned dataset to `weather_data.csv`.
8. Performed a basic comparative analysis across the three cities.

## Key Findings

At the time of sampling, **Mombasa recorded the highest temperature (27°C)**, while **London was the coolest (22°C)** — a temperature range of **5°C** across the three cities. This reflects real geographic differences: Mombasa's coastal, low-altitude location keeps it consistently warm, while Nairobi's elevation and London's temperate climate typically produce cooler conditions.

Humidity told a different story than temperature: **London actually had the highest humidity (67%)**, ahead of Mombasa (60%), with Nairobi the driest at 40%. Combining temperature and humidity, **Mombasa emerged as the least comfortable city** overall, since its heat and humidity compound each other, while **Nairobi was the most comfortable**, benefiting from a moderate temperature and the lowest humidity of the three.

Worth noting: **London has been experiencing an unusual heatwave** in recent conditions, which makes its relatively high humidity (67%) and mild 22°C reading notable in context — London's typical July weather is usually cooler and drier, so this snapshot reflects broader unusual heat patterns affecting the UK this summer rather than typical conditions.

In terms of wind, **Mombasa was the windiest city (7.33 m/s)**, while **London was the calmest (2.24 m/s)** — over three times less windy than Mombasa, consistent with Mombasa's coastal exposure.

All three cities were experiencing cloudy conditions at the time of sampling, though the degree of cloud cover differed — Mombasa had "few clouds," Nairobi was "overcast," and London had "broken clouds" — showing that even cities sharing the same broad weather category can have meaningfully different conditions underneath.

## Notes on Real-Time Data

Because this pipeline pulls **live** weather data, values will differ slightly each time it is run. This is expected behavior for real-time API data rather than a fixed historical dataset, and is itself a key learning from this project — ETL pipelines built on live sources need to account for a data snapshot that changes over time, unlike static files.

## What I Learned

Building this pipeline provided hands-on experience with:
- Making API requests and handling authentication via API keys
- Parsing and navigating nested JSON structures
- Timezone handling (UTC vs local time) when working with API timestamps
- Structuring the ETL process end-to-end: Extract → Transform → Load
- Drawing geographic/contextual insights (altitude, coastal exposure, heatwaves) from live data rather than just reporting raw numbers
