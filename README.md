# Weather-API
Python Weather Data Analysis Tool. 
This aplication allows the user to input any city, which will retrieve: Current Weather and Historical Statistics, such as the average, median and mode for the last 7 days. The 7 day date range is prior days as at current when running the applciation.

No API key is required for the URL's used:

Longitude & Latitude retrieval: "https://geocoding-api.open-meteo.com/v1/search?name={city}&count=1"

Actual Weather data retrieval: "https://historical-forecast-api.open-meteo.com/v1/forecast"

Open-Meteo requires the Longitude and Latitude of the input city to retrieve the weather data. 
The date format required is ISO6801, which is yyyy-mm-dd.

The following third party libraries require installation.

```pip install requests```

```pip install pandas```

**How to install a virtual environment to run this project:**
1. Open the Command Prompt screen (cmd)
2. Chose an environment name, such as _test_
3. Use the command python -m venv _test_ or py -m venv _test_
4. Active the environment and use _test_\Scripts\activate (Windows) or source_test_/bin/activate (Mac)





**bold**

_italics_

yap yap

