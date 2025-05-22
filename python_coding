"""
The below uses an API website to retrieve weather information.
It will retrieve current weather for the chosen city, as well as historical
statistics.

"""
#import requests is used to retrieve the co-ordinates of the input city (latitude & longitude)
#Weather API uses latitude & longitude to retrieve weather data
import json
from datetime import datetime, timedelta
import requests
import pandas as pd
import numpy as np

#user input
#now() function pulls current date and time
#however it has been formatted to be viewed as yyyy-mm-dd hh:mm:ss

print ("You will be prompted to input your city name.")
print ("To exit the city input prompt, please input:\033[1m Done! \033[0m")

while True:
    city_input = input("Please input your city name: ").strip()
    if len(city_input) < 1 :
        print("City name cannot be empty, please try again.")
        continue
    if city_input == "Done!":
        exit()
    break

#ensure neat city reflects for print
city = city_input.capitalize()
current = datetime.now()
current2 = current.strftime("%Y-%m-%d %H:%M:%S")
current3 = current.strftime("%Y-%m-%d")
N = 7
hist7 = current - timedelta(days=N)
hist7b = hist7.strftime("%Y-%m-%d")

URL1 = f"https://geocoding-api.open-meteo.com/v1/search?name={city}&count=1"
#below stores the information retrieved from the url
info = requests.get(URL1, timeout=10) #timeout within 10seconds
#if status code is 200, then the request was successful
#if succesful, then the below will request the latitude and longitude
#if not successful, error message
#if the status code is not 200, any other will also yield a error message with the status code too
#if either else give an error, it will exit the coding
#OR MAYBE DO WHILE LOOP TO TAKE YOU BACK TO ENTER CITY AGAIN!!!!!!!!!

#if you want to get out the request loop and exit, input is "Done!"
#try except for any errors and to allow you to input the city again/ends or ends loop of

try:
    if info.status_code == 200:
        data = info.json()
        if data.get('results'):
            latitude = data['results'][0]['latitude']
            longitude = data['results'][0]['longitude']
            print("City Selected:", city)
            print(f"Latitude: {latitude}, Longitude: {longitude}")
            print("Current Date and Time:", current2)
            print ("Seven Day Date Range:", "from", hist7b, "to", current3)
        else:
            print("City not found. Please ensure correct city has been captured.")
            exit()
    else:
        print(f"Error: {info.status_code}")
        exit()
except (requests.exceptions.RequestException, ValueError, KeyError) as e:
    print(f"An error occured: {e}")
    exit()

#we need to format the above now and 7 day date ranges into the
#open-meteo date format of ISO6801, which is yyyy-mm-dd.
#"Start date" is the date 7 days ago and "End date" is now current date
start_date = hist7.strftime("%Y-%m-%d")
end_date = current.strftime("%Y-%m-%d")
#once we have the latitude & longitude and date range, we use it to find the
#required weather in our API website, as well as the historical data
URL2 = "https://historical-forecast-api.open-meteo.com/v1/forecast"
#this will search for current weather if the statement is True
#date format is as per open-meteo requirement of iso8601 date format
params = {
            "latitude": latitude,
            "longitude": longitude,
            "start_date": start_date,
	        "end_date": end_date,
            "hourly": "temperature_2m",  
            "current_weather": True,          
        }
#the get function will use the paramaters above to retrieve the information required
data = requests.get(URL2, params=params, timeout=10)
#status code 200 as per above + error message
#this prompt is to pull current weather temperature in degrees celcius
#(temperature unit used in South Africa)
#Error message if there is no current weather temperature available

try:
    if data.status_code == 200:
        weather = data.json()
        print("Current Temperature:", weather["current_weather"]["temperature"], "°C")
    else:
        print("Error: No data available")
        exit()
except (ValueError, KeyError) as ee:
    print(f"An error occurred: {ee}")
    exit()

#create list to store the historical temperatures (only)
#first have to get the hourly info, as to then pull the temperatures
hist_weather = [] #to avoid "possibly used before assignment" error under arr,
#create an empty list for a reference point
try:
    if data.status_code == 200:
        weather = data.json()
        hist_hourly = weather.get("hourly",{})
        hist_weather = hist_hourly.get("temperature_2m",[])
    else:
        print("Error:No history data available")
        exit()
except (ValueError, KeyError) as ee:
    print(f"An error occurred: {ee}")
    exit()

arr = np.array(hist_weather)
ave1 = np.average(arr)
ave2 = round(ave1, 1)
med1 = np.median(arr)
med2 = round(med1, 1)
#mode can give more than one value, so needs formatting to ensure readability for the user
mod1 = pd.Series(arr).mode()
mod_con = mod1.tolist()
#panda series cannot be read in json format, need to convert it -
#but this gives a weird output if there is only 1 value

print("Seven Day Average Temperature:", ave2,"°C")
print("Seven Day Median Temperature:", med2,"°C")
#ensure no string type info pulling through if there is only one value
if len(mod1) ==1:
    print("Seven Day Mode Temperature/s:", mod1.iloc[0],"°C")
else:
    MOD2 = ", ".join([f"{temp}°C" for temp in mod1])
    print(f"Seven Day Mode Temperatures: {MOD2}")

#to list is required, due to pandas not being bale to convert to JSON
#panda series cannot be read in json format, need to convert it
#but this gives a weird output if there is only 1 value
# if 1 value, it will use to list conversion
# if multiple values, it will use mod2 that has been joined

#output info to save as a JSON document
#how do i include degrees celcius in the output of the json???????????

output_data = {
     "City Selected": city,
     "Latitude": latitude,
     "Longitude": longitude,
     "Current Date and Time": current2,
     "Seven Day Date Range: Start Date":hist7b, 
     "Seven Day Date Range: End Date":current3,
     "Current Temperature(°C)": weather["current_weather"]["temperature"], 
     "Seven Day Average Temperature(°C)": ave2,
     "Seven Day Median Temperature(°C)": med2,
     "Seven Day Mode Temperature/s(°C)": mod_con,
     }
#ensures json is encoded in UTF-8, else degree symbol does not reflect on text editors
DOC = f"{city}_{current3}_weather.json"
with open(DOC, "w", encoding="utf-8") as f:
    json.dump(output_data, f, indent=4, ensure_ascii= False)
#ascii=false ensure the degrees celcius does not pull through as \u00b0C on the json docuement
print(f"Thank you, your JSON document has been saved: {DOC}")
