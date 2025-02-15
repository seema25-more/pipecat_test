# Pipecat x Function Callable Models (API Triggers from LLM Responses)

##### This project extends Pipecat with Mistral AI to detect and automatically execute API calls when a request requires external data, such as weather information. The chatbot identifies relevant user queries, fetches real-time data using the OpenWeather API, and provides a smart response.

#### Features:

##### Mistral AI Integration - Uses an LLM to understand and respond to queries.

##### API Function Calling  - Detects when an API call is needed (e.g., weather queries).

##### Real-time Weather Data – Fetches live weather information from OpenWeather.

##### Dynamic City Recognition – Extracts city names from user input.

##### User Prompt Handling – If no city is provided, the chatbot asks for one.

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


```python
pip install "pipecat-ai[cartesia,openai]"
```

    Requirement already satisfied: pipecat-ai[cartesia,openai] in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (0.0.56)
    Requirement already satisfied: aiohttp~=3.11.11 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pipecat-ai[cartesia,openai]) (3.11.12)
    Requirement already satisfied: httpx~=0.27.2 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pipecat-ai[cartesia,openai]) (0.27.2)
    Requirement already satisfied: loguru~=0.7.3 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pipecat-ai[cartesia,openai]) (0.7.3)
    Requirement already satisfied: Markdown~=3.7 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pipecat-ai[cartesia,openai]) (3.7)
    Requirement already satisfied: numpy~=1.26.4 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pipecat-ai[cartesia,openai]) (1.26.4)
    Requirement already satisfied: Pillow~=11.1.0 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pipecat-ai[cartesia,openai]) (11.1.0)
    Requirement already satisfied: protobuf~=5.29.3 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pipecat-ai[cartesia,openai]) (5.29.3)
    Requirement already satisfied: pydantic~=2.10.5 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pipecat-ai[cartesia,openai]) (2.10.6)
    Requirement already satisfied: pyloudnorm~=0.1.1 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pipecat-ai[cartesia,openai]) (0.1.1)
    Requirement already satisfied: resampy~=0.4.3 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pipecat-ai[cartesia,openai]) (0.4.3)
    Requirement already satisfied: soxr~=0.5.0 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pipecat-ai[cartesia,openai]) (0.5.0.post1)
    Requirement already satisfied: cartesia~=1.3.1 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pipecat-ai[cartesia,openai]) (1.3.1)
    Requirement already satisfied: websockets~=13.1 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pipecat-ai[cartesia,openai]) (13.1)
    Requirement already satisfied: openai~=1.59.6 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pipecat-ai[cartesia,openai]) (1.59.9)
    Requirement already satisfied: python-deepcompare~=2.1.0 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pipecat-ai[cartesia,openai]) (2.1.0)
    Requirement already satisfied: aiohappyeyeballs>=2.3.0 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from aiohttp~=3.11.11->pipecat-ai[cartesia,openai]) (2.4.6)
    Requirement already satisfied: aiosignal>=1.1.2 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from aiohttp~=3.11.11->pipecat-ai[cartesia,openai]) (1.3.2)
    Requirement already satisfied: attrs>=17.3.0 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from aiohttp~=3.11.11->pipecat-ai[cartesia,openai]) (25.1.0)
    Requirement already satisfied: frozenlist>=1.1.1 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from aiohttp~=3.11.11->pipecat-ai[cartesia,openai]) (1.5.0)
    Requirement already satisfied: multidict<7.0,>=4.5 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from aiohttp~=3.11.11->pipecat-ai[cartesia,openai]) (6.1.0)
    Requirement already satisfied: propcache>=0.2.0 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from aiohttp~=3.11.11->pipecat-ai[cartesia,openai]) (0.2.1)
    Requirement already satisfied: yarl<2.0,>=1.17.0 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from aiohttp~=3.11.11->pipecat-ai[cartesia,openai]) (1.18.3)
    Requirement already satisfied: iterators>=0.2.0 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from cartesia~=1.3.1->pipecat-ai[cartesia,openai]) (0.2.0)
    Requirement already satisfied: requests>=2.31.0 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from cartesia~=1.3.1->pipecat-ai[cartesia,openai]) (2.32.3)
    Requirement already satisfied: anyio in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from httpx~=0.27.2->pipecat-ai[cartesia,openai]) (4.8.0)
    Requirement already satisfied: certifi in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from httpx~=0.27.2->pipecat-ai[cartesia,openai]) (2024.12.14)
    Requirement already satisfied: httpcore==1.* in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from httpx~=0.27.2->pipecat-ai[cartesia,openai]) (1.0.7)
    Requirement already satisfied: idna in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from httpx~=0.27.2->pipecat-ai[cartesia,openai]) (3.10)
    Requirement already satisfied: sniffio in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from httpx~=0.27.2->pipecat-ai[cartesia,openai]) (1.3.1)
    Requirement already satisfied: h11<0.15,>=0.13 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from httpcore==1.*->httpx~=0.27.2->pipecat-ai[cartesia,openai]) (0.14.0)
    Requirement already satisfied: colorama>=0.3.4 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from loguru~=0.7.3->pipecat-ai[cartesia,openai]) (0.4.6)
    Requirement already satisfied: win32-setctime>=1.0.0 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from loguru~=0.7.3->pipecat-ai[cartesia,openai]) (1.2.0)
    Requirement already satisfied: distro<2,>=1.7.0 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from openai~=1.59.6->pipecat-ai[cartesia,openai]) (1.9.0)
    Requirement already satisfied: jiter<1,>=0.4.0 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from openai~=1.59.6->pipecat-ai[cartesia,openai]) (0.8.2)
    Requirement already satisfied: tqdm>4 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from openai~=1.59.6->pipecat-ai[cartesia,openai]) (4.67.1)
    Requirement already satisfied: typing-extensions<5,>=4.11 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from openai~=1.59.6->pipecat-ai[cartesia,openai]) (4.12.2)
    Requirement already satisfied: annotated-types>=0.6.0 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pydantic~=2.10.5->pipecat-ai[cartesia,openai]) (0.7.0)
    Requirement already satisfied: pydantic-core==2.27.2 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pydantic~=2.10.5->pipecat-ai[cartesia,openai]) (2.27.2)
    Requirement already satisfied: scipy>=1.0.1 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pyloudnorm~=0.1.1->pipecat-ai[cartesia,openai]) (1.15.1)
    Requirement already satisfied: future>=0.16.0 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pyloudnorm~=0.1.1->pipecat-ai[cartesia,openai]) (1.0.0)
    Requirement already satisfied: numba>=0.53 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from resampy~=0.4.3->pipecat-ai[cartesia,openai]) (0.61.0)
    Requirement already satisfied: llvmlite<0.45,>=0.44.0dev0 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from numba>=0.53->resampy~=0.4.3->pipecat-ai[cartesia,openai]) (0.44.0)
    Requirement already satisfied: charset_normalizer<4,>=2 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from requests>=2.31.0->cartesia~=1.3.1->pipecat-ai[cartesia,openai]) (3.4.1)
    Requirement already satisfied: urllib3<3,>=1.21.1 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from requests>=2.31.0->cartesia~=1.3.1->pipecat-ai[cartesia,openai]) (2.3.0)
    Note: you may need to restart the kernel to use updated packages.
    

### 5. Install Mistral AI and Other Requirements

##### Mistral AI requires an API key. Install it using:


```python
pip install mistralai
```

    Requirement already satisfied: mistralai in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (0.4.2)
    Requirement already satisfied: httpx<1,>=0.25 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from mistralai) (0.27.2)
    Requirement already satisfied: orjson<3.11,>=3.9.10 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from mistralai) (3.10.15)
    Requirement already satisfied: pydantic<3,>=2.5.2 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from mistralai) (2.10.6)
    Requirement already satisfied: anyio in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from httpx<1,>=0.25->mistralai) (4.8.0)
    Requirement already satisfied: certifi in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from httpx<1,>=0.25->mistralai) (2024.12.14)
    Requirement already satisfied: httpcore==1.* in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from httpx<1,>=0.25->mistralai) (1.0.7)
    Requirement already satisfied: idna in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from httpx<1,>=0.25->mistralai) (3.10)
    Requirement already satisfied: sniffio in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from httpx<1,>=0.25->mistralai) (1.3.1)
    Requirement already satisfied: h11<0.15,>=0.13 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from httpcore==1.*->httpx<1,>=0.25->mistralai) (0.14.0)
    Requirement already satisfied: annotated-types>=0.6.0 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pydantic<3,>=2.5.2->mistralai) (0.7.0)
    Requirement already satisfied: pydantic-core==2.27.2 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pydantic<3,>=2.5.2->mistralai) (2.27.2)
    Requirement already satisfied: typing-extensions>=4.12.2 in c:\users\admin\anaconda3\envs\pipecatfinal\lib\site-packages (from pydantic<3,>=2.5.2->mistralai) (4.12.2)
    Note: you may need to restart the kernel to use updated packages.
    

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
        # Ensure it's not a generic word like 'weather' or 'temperature'
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

    Hello! I'm just a computer program, so I don't have feelings, but I'm here to help you with any language-related questions you have. Is there something specific you would like to know or practice?
    

    Which city's weather do you want to know?  berlin
    

    The weather in berlin is overcast clouds with a temperature of 0.2°C.
    The weather in Tokyo is overcast clouds with a temperature of 10.76°C.
    Sure, here's a joke for you: 
    
    Why don't scientists trust atoms?
    
    Because they make up everything!
    

## API Integration

##### This chatbot detects when a user asks for the weather and automatically triggers an API call to OpenWeather to fetch real-time weather data.

#### Example API Call:

##### https://api.openweathermap.org/data/2.5/weather?q=<CITY_NAME>&appid=YOUR_API_KEY&units=metric

#### Technologies Used

##### Pipecat – Framework for function calling.

##### Mistral AI – Large language model for chatbot responses.

##### OpenWeather API – Provides live weather data.

##### Python – Core programming language.


```python

```


```python

```
