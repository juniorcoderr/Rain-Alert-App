## 🌧️ Rain Alert App

This Python project is a **weather-based SMS notification system** that alerts you when it's going to rain, so you can carry an umbrella before heading out! It fetches weather data using the **OpenWeatherMap API**, checks for upcoming rain based on weather condition codes, and sends an alert via **Twilio SMS**. The app is hosted and scheduled on **PythonAnywhere** to run automatically at fixed intervals.

---

### 📌 Features

- ✅ Fetches hourly weather forecast data for a given location
- ✅ Checks if rain is expected in the next 12 hours
- ✅ Sends an SMS alert using Twilio if rain is predicted
- ✅ Can be deployed on PythonAnywhere for automated periodic execution

---

### 🛠️ Technologies Used

- **Python** – Main programming language for scripting and logic  
- **Requests Library** – For making HTTP requests to external APIs  
- **Twilio** – Cloud communication platform to send SMS alerts  
- **OpenWeatherMap API** – Provides hourly weather forecast data  
- **PythonAnywhere** – Cloud platform for hosting and scheduling Python scripts  

---

### 📜 How It Works (Code Walkthrough)

```python
import requests
from twilio.rest import Client
```
- `requests` is used to make HTTP GET requests to the OpenWeatherMap API.
- `Client` from `twilio.rest` allows sending SMS using your Twilio account.

---

#### 1. **API Setup and Authentication**

```python
OWM_Endpoint = "https://api.openweathermap.org/data/2.5/forecast"
api_key = "your_openweather_api_key"
account_sid = 'your_twilio_account_sid'
auth_token = "your_twilio_auth_token"
```

- You define the endpoint for OpenWeatherMap’s 5-day/3-hour forecast API.
- Store your OpenWeather and Twilio credentials for authentication.

---

#### 2. **Request Weather Data**

```python
weather_params = {
    "lat": ,
    "lon": ,
    "appid": api_key,
    "cnt": 4,  # Fetch weather data for next 4 * 3 = 12 hours
}
response = requests.get(OWM_Endpoint, params=weather_params)
response.raise_for_status()
weather_data = response.json()
```

- `cnt=4` limits the data to the next 4 forecast periods (each is 3 hours apart).
- `raise_for_status()` throws an error if the request fails.
- The API response is parsed to JSON.

---

#### 3. **Rain Detection Logic**

```python
will_rain = False
for hour_data in weather_data["list"]:
    condition_code = hour_data["weather"][0]["id"]
    if int(condition_code) < 700:
        will_rain = True
```

- Loops through the weather forecasts.
- Each forecast includes a `weather` condition code.
- Codes `< 700` generally indicate precipitation (rain, snow, etc.).
- If any such code is found, it sets `will_rain` to `True`.

---

#### 4. **Send SMS Alert via Twilio**

```python
if will_rain:
    client = Client(account_sid, auth_token)
    message = client.messages.create(
        messaging_service_sid='your_messaging_service_sid',
        body="It's going to rain today. Remember to bring an umbrella ☔",
        to='+91xxxxxxxxxx'
    )
    print(message.status)
```

- If rain is predicted, initializes a Twilio client.
- Sends an SMS through a Messaging Service (preferred over a single sender number).
- Prints the message status (e.g., queued, sent, delivered).

---

### ☁️ Deployment on PythonAnywhere

- You can host this script on [PythonAnywhere](https://www.pythonanywhere.com/), a platform that allows running Python scripts in the cloud.
- Use their **scheduled tasks** feature to run the script automatically (e.g., every morning).

---

### 🔐 Security Tip

- Avoid hardcoding sensitive information (API keys, SID, tokens) in code.
- Use environment variables or a `.env` file with `python-dotenv` in production.

---

### 📦 Future Enhancements (Optional Ideas)

- Allow user input for dynamic location
- Add email notification option
- Include temperature and wind data in the alert
- Build a simple web interface to manage alerts
