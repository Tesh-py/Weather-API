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
4. Active the environment and use _test_\Scripts\activate (Windows) or source _test_/bin/activate (Mac)

**Comments/Notes on the python code:**

<ins>_import libraries_</ins>
* requests: allows HTTP requests
* json: data transfer on the web
* datetime: allows date conversions
* pandas/numpy: mathematical calculations

Part 1: The user will need to input a city name, if there is no data for that city, it will reflect errors (depending on the issue). These could be:
- Incorrect city input
- No weather data
- Website errors

Part 2: Info Status Code 200 means the input city found the data requested. 
- This will retrieve the City's Longitude and Latitude first
- It will print: Longitude, Latitude, Current Date & Time and the 7 day date range

Part 3: The calculations for Current Date & Time and the 7 daye range includes:
- datetime.now() for the date and time as at when the user inputs the city
- current.strftime ensure the correct date format as yyyy-mm-dd hh:mm:ss or yyyy-mm-dd
- This is the format required for Open-Meteo to retrive the weather data, as their website uses date format ISO6801
- timedelta is used to calculate the date range between 7 days prior and current date

Part 4: info = requests.get(URL1, timeout=10)
- the information retrived from URL1 is stored in info
- ensuring a 10 second timeout if there is no response
- if the info status code is 200, it will retrieve the Longitude and Latitude of the input city
- else it will provide errors. Such as No city found, or info status error code, or other exceptions (HTTP request issue, Value error or Key error)

Part 5: with the above Longitude & Longitude, current date & time and 7 day date range, we can use URL2 to retrive actual weather data.
- params is a dictionary that includes all specific data required from the URL2
- data = requests.get(URL2, params=params, timeout=10) retrieves the data and saves it into "data"

Part 6 : Info Status Code 200 means the Longitude & Latitude retrieved prior will result in information.
- the requested information is current weather temperature in degrees celsius and will print this information

Part 7: a blank list for historical weather temperatures is created to avoid "possibly used before assignment" error.
- if all of the above is successful, it will retrieve and save the 7 day date range weather temperature data to run the statistical calculations

Part 8: the average, median and mode is calculated.
- arr: used numpy to create a historical data array
- ave1: used numpy average calculation
- ave2: round off the ave1 output to align to all other data ouput (1 decimal)
- med1: used numpy median calculation
- med2: round off the med1 output to align to all other data ouput (1 decimal)
  
  

