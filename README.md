import requests
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd
from datetime import datetime
API_KEY = '1cad2ad3e8fc1bd9a38edb90988e41fe'
CITY = 'London'
URL = f'http://api.openweathermap.org/data/2.5/forecast?q={CITY}&appid={API_KEY}&units=metric'
response = requests.get(URL)
data = response.json()
dates = []
temperatures = []
humidity = []
weather_descriptions = []
for entry in data['list']:
    dates.append(datetime.fromtimestamp(entry['dt']))
    temperatures.append(entry['main']['temp'])
    humidity.append(entry['main']['humidity'])
    weather_descriptions.append(entry['weather'][0]['description'])
weather_df = pd.DataFrame({
    'Date': dates,
    'Temperature': temperatures,
    'Humidity': humidity,
    'Description': weather_descriptions
})
sns.set(style="whitegrid")
plt.figure(figsize=(14, 10))
plt.subplot(2, 1, 1)
sns.lineplot(x='Date', y='Temperature', data=weather_df, marker='o')
plt.title('Temperature Trend')
plt.xlabel('Date')
plt.ylabel('Temperature (°C)')
plt.subplot(2, 1, 2)
sns.lineplot(x='Date', y='Humidity', data=weather_df, marker='o', color='orange')
plt.title('Humidity Trend')
plt.xlabel('Date')
plt.ylabel('Humidity (%)')
plt.tight_layout()
plt.show()
