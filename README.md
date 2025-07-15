
# NJ-ACIS-Precipitation-Scraper

## Project Overview

This project provides a Python script to programmatically download historical daily precipitation data for selected weather stations in New Jersey. It leverages the ACIS (Applied Climate Information System) API, developed by NOAA's Regional Climate Centers, to retrieve station metadata and time-series observations. The output is a clean CSV file containing daily precipitation amounts and associated flags, suitable for climate analysis, hydrological modeling, or other environmental research.

The script filters stations to include primarily COOP network stations within a defined geographical bounding box for New Jersey.

## Key Features

* **ACIS API Integration:** Utilizes the `StnMeta` endpoint to find relevant stations and the `StnData` endpoint to fetch daily observations.
* **Geographical Filtering:** Filters stations based on a defined latitude/longitude bounding box (defaulting to a broad New Jersey area).
* **COOP Station Prioritization:** Focuses on stations identified with a COOP (Cooperative Observer Program) network ID.
* **Daily Precipitation Data:** Retrieves daily `pcpn` (precipitation) element, along with observation time (`t`) and quality flags (`i`).
* **Data Cleaning:** Handles "M" (Missing) and "T" (Trace) values in precipitation data.
* **CSV Output:** Saves the compiled data into a structured CSV file.
* **API Respectful:** Includes a small `time.sleep` delay between station requests to avoid overwhelming the ACIS API.

## Setup Instructions

To run this script, you'll need Python 3.x and the `requests` library.

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/Bayo-sb/NJ-ACIS-Precipitation-Scraper.git](https://github.com/Bayo-sb/NJ-ACIS-Precipitation-Scraper.git)
    cd NJ-ACIS-Precipitation-Scraper
    ```
2.  **Install Python dependencies:**
    It's recommended to use a virtual environment.
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows: `venv\Scripts\activate`
    pip install requests
    ```

## Usage

1.  **Open the script:**
    You can open `download_nj_precip.py` (or whatever you name your script) in a text editor or an IDE.

2.  **Review/Adjust Parameters (Optional):**
    * **Bounding Box:** Modify `min_lat`, `max_lat`, `min_lon`, `max_lon` at the top of the script if you need to narrow or expand the geographical area within New Jersey.
    * **Date Range:** Adjust `sdate` (start date) and `edate` (end date) within the `payload` for the `StnData` request if you need a different historical period.
    * **Output File Name:** Change `nj_last_precipitation_data_1980_2024.csv` to your preferred output file name.

3.  **Run the script:**
    Execute the script from your terminal:
    ```bash
    python download_nj_precip.py
    ```

4.  **Monitor Progress:**
    The script will print progress messages to your console, indicating which station's data is being downloaded.

## Output File Structure

The script generates a CSV file named `nj_last_precipitation_data_1980_2024.csv` (or whatever you specify) with the following columns:

* **`StationName`**: The human-readable name of the weather station.
* **`StationID`**: The ACIS ID (idType 2) used to fetch data for that station.
* **`Date`**: The date of the observation (YYYY-MM-DD).
* **`Precip(in)`**: The daily precipitation amount in inches.
    * "M" indicates missing data.
    * "T" indicates a trace amount (less than 0.01 inches).
    * Numerical values are float representations.
* **`TimeObs`**: The time of observation for that precipitation measurement.
* **`Flags`**: Quality control flags associated with the measurement.

## Important Notes

* **API Usage:** This script interacts with a public API. While `time.sleep` is included, be mindful of excessive requests. ACIS may have rate limits or usage policies.
* **Data Format:** The `Precip(in)` column may contain 'M' or 'T' strings for missing or trace values. You will need to handle these during subsequent data loading and analysis (e.g., converting them to `NaN` or 0 for numerical processing).
* **Missing Coordinates:** The script includes COOP stations even if their coordinates are not available in the metadata, as long as they are in New Jersey.
* **Error Handling:** The script includes basic `try-except` blocks for network errors, but more robust error handling could be implemented for production use.

---
