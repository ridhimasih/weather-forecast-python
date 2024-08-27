# weather-forecast-python
A Python command-line application that fetches and displays current weather information and a 5-day forecast for any city using the OpenWeatherMap API.
import requests
Let's walk through building a Weather Forecast Application in Python step by step. This project will fetch weather data using an external API and display it to the user.

Project Steps:
1.Setting Up the Environment
2.Getting an API Key
3.Fetching Weather Data
4.Parsing and Displaying Data
5.Handling Errors and User Input

1. Setting Up the Environment
To start, ensure you have Python installed on your machine. You will also need the requests library to make HTTP requests to the weather API.

Install requests:
Run the following command to install the requests package:

bash
Copy code
pip install requests

2. Getting an API Key
We will use the OpenWeatherMap API to fetch weather data. To do this, you need an API key.

Go to OpenWeatherMap and sign up for a free account.
After signing up, go to your dashboard and generate an API key.

3. Fetching Weather Data
To get weather data, we will call the OpenWeatherMap API. Here’s how to structure the URL:

API Endpoint:

plaintext
Copy code
https://api.openweathermap.org/data/2.5/weather?q={city_name}&appid={API_key}&units=metric
Replace:

{city_name} with the name of the city.
{API_key} with your API key.
units=metric to get temperatures in Celsius.

4. Parsing and Displaying Data
Once you fetch the weather data, you'll need to parse the response (which is in JSON format) and display it in a user-friendly way.

5. Handling Errors and User Input
Handle invalid city names and network errors using try-except blocks to ensure your app is robust and doesn’t crash on invalid input.

# Function to fetch weather data
def get_weather(city_name, api_key):
    # Construct the URL to fetch data
    url = f"http://api.openweathermap.org/data/2.5/weather?q={city_name}&appid={api_key}&units=metric"
    
    try:
        # Make the request to the OpenWeatherMap API
        response = requests.get(url)
        
        # If response status code is not 200 (OK), raise an exception
        response.raise_for_status()
        
        # Parse the JSON data
        data = response.json()
        
        # Extract useful information from the response
        weather = {
            "city": data["name"],
            "temperature": data["main"]["temp"],
            "humidity": data["main"]["humidity"],
            "description": data["weather"][0]["description"],
            "wind_speed": data["wind"]["speed"]
        }
        
        return weather
    
    except requests.exceptions.HTTPError as http_err:
        print(f"HTTP error occurred: {http_err}")
    except requests.exceptions.RequestException as err:
        print(f"Error occurred: {err}")
    except KeyError:
        print("Could not retrieve data for this city. Please check the city name.")
    return None

# Main function to run the application
def main():
    # Replace this with your actual API key from OpenWeatherMap
    api_key = "your_api_key_here"
    
    # Ask user to input the city name
    city = input("Enter the name of the city: ")
    
    # Fetch weather data for the entered city
    weather_data = get_weather(city, api_key)
    
    # If weather data was successfully fetched, display it
    if weather_data:
        print("\nWeather Details:")
        print(f"City: {weather_data['city']}")
        print(f"Temperature: {weather_data['temperature']}°C")
        print(f"Humidity: {weather_data['humidity']}%")
        print(f"Weather Description: {weather_data['description']}")
        print(f"Wind Speed: {weather_data['wind_speed']} m/s")
    else:
        print("Failed to retrieve weather data. Please try again.")

# Run the application
if __name__ == "__main__":
    main()


    Step-by-Step Breakdown of the Code:
Imports:

We import the requests library to handle HTTP requests.
get_weather() Function:

This function takes the city name and API key as arguments.
It constructs the API URL and sends a GET request to OpenWeatherMap.
It parses the JSON response and extracts key weather information (temperature, humidity, description, wind speed, etc.).
Handles errors such as invalid city names or network issues using try-except.
main() Function:

Asks the user for a city name and passes it to the get_weather() function.
If the weather data is successfully fetched, it prints the weather information. Otherwise, it prints an error message.


Error Handling:
If the city name is invalid or there is an issue with the network, the application will handle the error gracefully. For example:

kotlin
Copy code
Enter the name of the city: invalid_city_name

Could not retrieve data for this city. Please check the city name.
Next Steps / Enhancements:
Additional Features:

Display a 5-day forecast using the forecast API endpoint.
Allow the user to choose between different temperature units (Celsius, Fahrenheit).
GUI Version:

Use libraries like Tkinter or PyQt to create a graphical interface for the application.
Save Weather Data:

Store weather history in a file or a database so the user can refer to past weather reports.
Error Handling Improvements:

Provide more detailed error messages (e.g., "City not found" vs. "API key error").

