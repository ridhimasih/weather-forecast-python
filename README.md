# weather-forecast-python
A Python command-line application that fetches and displays current weather information and a 5-day forecast for any city using the OpenWeatherMap API.
import requests

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
