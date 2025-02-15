# Pipecat x Function Callable Models (API Triggers from LLM Responses)

##### This project extends Pipecat with Mistral AI to detect and automatically execute API calls when a request requires external data, such as weather information. The chatbot identifies relevant user queries, fetches real-time data using the OpenWeather API, and provides a smart response.

#### Features:

##### Mistral AI Integration : Uses an LLM to understand and respond to queries.

##### API Function Calling : Detects when an API call is needed (e.g., weather queries).

##### Real-time Weather Data : Fetches live weather information from OpenWeather.

##### Dynamic City Recognition : Extracts city names from user input.

##### User Prompt Handling : If no city is provided, the chatbot asks for one.

## Installation & Setup

### 1. Create and Activate Conda Environment

##### conda create --name pipecatfinal
##### conda activate pipecatfinal

### 2. Install Jupyter Notebook

##### pip install notebook

### 3. Check Python Version in Jupyter Notebook

##### Pipecat requires Python 3.10 or higher, so verify your Python version first:


```python
!python --version
```

    Python 3.12.8
    

### 4. Install Pipecat

##### After verifying your Python version, install Pipecat with:

pip install "pipecat-ai[cartesia,openai]"

### 5. Install Mistral AI and Other Requirements

##### Mistral AI requires an API key. Install it using:

pip install mistralai

### 6. Set Up API Keys

##### Set API keys for Mistral AI and OpenWeather API in jupyter notebook:


```python
import os

# Set API key for Mistral AI
os.environ["MISTRAL_API_KEY"] = "jZ0LwrCqyyIQuyG4oL1dpkL9kK8ZXgh8"

# Set OpenWeather API key
os.environ["OPENWEATHER_API_KEY"] = "826fc967f2f08c11068a989501e0c2ba"
```


```python
# Verify API keys are set correctly
print("Mistral API Key:", os.getenv("MISTRAL_API_KEY"))
print("OpenWeather API Key:", os.getenv("OPENWEATHER_API_KEY"))
```

    Mistral API Key: jZ0LwrCqyyIQuyG4oL1dpkL9kK8ZXgh8
    OpenWeather API Key: 826fc967f2f08c11068a989501e0c2ba
    

### 7. Import Required Libraries


```python
import requests
import pipecat
import re
from mistralai.client import MistralClient
```

##### These libraries are required for handling API calls, processing text, and interacting with Mistral AI and Pipecat.

### 8. Initialize Mistral Client


```python
client = MistralClient(api_key=os.environ["MISTRAL_API_KEY"])
```

##### This ensures that Mistral AI is properly configured to handle chatbot queries.

## How to Use the Chatbot

### Chatbot Function

This chatbot uses Mistral AI to process user queries and detect weather-related requests.


```python
#function to chat with Mistral
def chat_with_mistral(prompt):
    response = client.chat(
        model="mistral-small", 
        messages=[{"role": "user", "content": prompt}]
    )
    return response.choices[0].message.content

print(chat_with_mistral("Hello, how are you?"))

# Function to fetch weather data from OpenWeather API
def get_weather(city):
    api_key = os.getenv("OPENWEATHER_API_KEY")
    if not api_key:
        return "API key is missing. Set the OPENWEATHER_API_KEY."

    url = f"http://api.openweathermap.org/data/2.5/weather?q={city}&appid={api_key}&units=metric"
    response = requests.get(url)

    if response.status_code == 200:
        data = response.json()
        weather = data["weather"][0]["description"]
        temp = data["main"]["temp"]
        return f"The weather in {city} is {weather} with a temperature of {temp}°C."
    else:
        return f"Error fetching weather data: {response.json().get('message', 'Unknown error')}"

# Function to extract city name from user input
def extract_city(prompt):
    """Extracts a city name from a user's input using regex."""
    match = re.search(r"\b(?:in|weather|temperature|for)?\s*([A-Za-z\s]+)$", prompt, re.IGNORECASE)
    if match:
        city = match.group(1).strip()
        if len(city) > 2 and city.lower() not in ["weather", "temperature", "like"]:
            return city
    return None

# chatbot function
def chat_with_mistral(prompt):
    # Check if the message contains "weather"
    if "weather" in prompt.lower():
        city = extract_city(prompt)

        # If no valid city was found, ask the user
        if not city:
            city = input("Which city's weather do you want to know? ").strip()
            return get_weather(city)

        # Call the weather API function
        return get_weather(city)

    # Otherwise, send the message to Mistral AI
    response = client.chat(
        model="mistral-small",
        messages=[{"role": "user", "content": prompt}]
    )
    return response.choices[0].message.content

# Test chatbot interactions
print(chat_with_mistral("What's the weather like?"))  
print(chat_with_mistral("Weather Tokyo"))  
print(chat_with_mistral("Tell me a joke."))  
```

    Hello! I'm just a computer program, so I don't have feelings, but I'm here and ready to assist you with any questions you have to the best of my ability. How can I help you today?
    

    Which city's weather do you want to know?  frankfurt
    

    The weather in frankfurt is overcast clouds with a temperature of 0.84°C.
    The weather in Tokyo is broken clouds with a temperature of 8.71°C.
    Of course, I'd be happy to share a joke with you! Here it is:
    
    Why don't scientists trust atoms?
    
    Because they make up everything!
    
    I hope that brought a smile to your face. Would you like to ask me anything else?
    

## API Integration

##### This chatbot detects when a user asks for the weather and automatically triggers an API call to OpenWeather to fetch real-time weather data.

#### Example API Call:

##### https://api.openweathermap.org/data/2.5/weather?q=<CITY_NAME>&appid=YOUR_API_KEY&units=metric

#### Technologies Used

##### Pipecat – Framework for function calling.

##### Mistral AI – Large language model for chatbot responses.

##### OpenWeather API – Provides live weather data.

##### Python – Core programming language.
