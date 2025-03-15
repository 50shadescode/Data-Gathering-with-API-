# Data-Gathering-with-API-
NASA API Project - Astronomy Picture of the Day & Asteroid Data

Project Overview

This project utilizes NASA's public APIs to retrieve and display:
The Astronomy Picture of the Day (APOD), including its title and description.
Near-Earth Object (NEO) asteroid data from the "Asteroids - NeoWs" API, processing and exporting it into a structured CSV file.

Prerequisites

Ensure you have the following installed in your Python environment:
requests (for API calls)
pandas (for data processing)
IPython.display (for displaying images in Jupyter Notebook)

You can install the necessary libraries using:
pip install requests pandas
Steps to Run the Project

1. Import Required Libraries

import requests
import pandas as pd
from IPython.display import display, Image

2. Set Up Your API Key

Before running the script, store your API key as a variable:

API_KEY = "hdcci4kzr27sUF7vmG9re1CX5VWI9oOWh65D5d6Y"

3. Fetch Astronomy Picture of the Day (APOD)

APOD_URL = f"https://api.nasa.gov/planetary/apod?api_key={API_KEY}"
response = requests.get(APOD_URL)
apod_data = response.json()

if "url" in apod_data:
    display(Image(url=apod_data["url"]))
    print(f"Title: {apod_data['title']}\nExplanation: {apod_data['explanation']}")

This retrieves the APOD, displays the image, and prints its title and description.

4. Fetch Near-Earth Object (NEO) Data

NEO_URL = f"https://api.nasa.gov/neo/rest/v1/feed?api_key={API_KEY}"
response = requests.get(NEO_URL)
asteroid_data = response.json()

5. Extract and Structure the Data into a Pandas DataFrame

asteroids = []
for date in asteroid_data["near_earth_objects"]:
    for asteroid in asteroid_data["near_earth_objects"][date]:
        asteroids.append({
            "Asteroid ID": asteroid["id"],
            "Asteroid name": asteroid["name"],
            "Minimal estimated diameter (km)": asteroid["estimated_diameter"]["kilometers"]["estimated_diameter_min"],
            "Absolute magnitude": asteroid["absolute_magnitude_h"],
            "Relative velocity (km/s)": asteroid["close_approach_data"][0]["relative_velocity"]["kilometers_per_second"]
        })

df = pd.DataFrame(asteroids)
print(df.head())

This script processes the raw API response and extracts the necessary details.

6. Save Data to a CSV File

df.to_csv("asteroid_data.csv", index=False)
print("CSV file saved successfully.")

Output
APOD Image: Displays NASA's Astronomy Picture of the Day along with its title and explanation.
Asteroid Data CSV: Saves a cleaned dataset of near-Earth asteroids with relevant attributes.

Notes
The API key is required for authentication.
The script should be executed in a Python environment that supports Jupyter Notebook or an IDE with display capabilities.
The dataset may vary daily as NASA updates its database.
