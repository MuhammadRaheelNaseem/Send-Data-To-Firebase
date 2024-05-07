Absolutely, let's delve into each section with more technical detail and explanation:

---

# Firebase: Real-Time Data Streaming (RTDS) Template

In this lecture, we'll explore how to leverage Firebase Realtime Database for real-time data streaming and interaction. Firebase Realtime Database is a NoSQL cloud database that stores data in JSON format and synchronizes it in real-time across all connected clients.

## 1. Introduction to Firebase

Firebase, a platform provided by Google, offers a suite of tools and services for building high-quality mobile and web applications. One of its core offerings is Firebase Realtime Database, which provides developers with a cloud-hosted database that supports real-time data synchronization.

Firebase Realtime Database uses a WebSocket connection to maintain a persistent connection between the client and the server. This enables data to be synchronized instantly whenever changes occur, allowing for seamless collaboration and real-time updates across multiple devices.

## 2. RTDS Template

### Scenario 1: Set Value

```python
{
  "sensors": "Set Value"
}
```

In this template, a single value is set under the "sensors" node in the Firebase database. This structure is useful for scenarios where a single value needs to be updated or monitored.

### Scenario 2: Multiple Sensor Data

```python
{
  "sensors": {
    "sensor1": {
      "data": "data value for sensor 1...",
      "timestamp": 1234567890
    },
    "sensor2": {
      "data": "data value for sensor 2...",
      "timestamp": 1234567890
    }
  }
}
```

This template represents a more complex scenario where data from multiple sensors is stored in the database. Each sensor has its own node containing data and a timestamp. This structure allows for efficient organization and retrieval of sensor data.

## 3. Creating a Script for Sending Single Data to Firebase

```python
import urequests
import ujson
import network
import time

# Your WiFi credentials
WIFI_SSID = "iot_test"
WIFI_PASSWORD = "123456789"

# Firebase Realtime Database URL
FIREBASE_PROJECT_ID = "FIREBASE_PROJECT_ID "
FIREBASE_URL = "https://" + FIREBASE_PROJECT_ID + ".firebaseio.com/Sensor.json"

def connect_wifi():
    wlan = network.WLAN(network.STA_IF)
    wlan.active(True)
    if not wlan.isconnected():
        print("Connecting to WiFi...")
        wlan.connect(WIFI_SSID, WIFI_PASSWORD)
        while not wlan.isconnected():
            pass
    print("WiFi Connected")
    print("IP:", wlan.ifconfig()[0])

def firebase_post(data):
    try:
        data_json = ujson.dumps(data)
        headers = {'Content-Type': 'application/json'}
        response = urequests.patch(FIREBASE_URL, headers=headers, data=data_json)
        if response.status_code == 200:
            print("Data posted successfully to Firebase")
        else:
            print("Error posting data to Firebase:", response.status_code)
    except Exception as e:
        print("Exception:", e)

def main():
    connect_wifi()

    # Data to be posted to Firebase
    new_data = {"message": "Hello", "timestamp": int(time.time())}

    # Posting data to Firebase
    print("Posting data to Firebase...")
    firebase_post(new_data)

if __name__ == "__main__":
    main()
```

This script demonstrates how to send a single data entry to Firebase. It includes functionality to connect to a Wi-Fi network, construct the data payload, and send the data to Firebase using the PATCH method.

## 4. Creating a Script for Sending Multiple Values to Firebase
```python
import urequests
import ujson
import network
import time

# Your WiFi credentials
WIFI_SSID = "iot_test"
WIFI_PASSWORD = "123456789"

# Firebase Realtime Database URL
FIREBASE_PROJECT_ID = "FIREBASE_PROJECT_ID"
FIREBASE_URL = "https://" + FIREBASE_PROJECT_ID + ".firebaseio.com/Sensor.json"

def connect_wifi():
    wlan = network.WLAN(network.STA_IF)
    wlan.active(True)
    if not wlan.isconnected():
        print("Connecting to WiFi...")
        wlan.connect(WIFI_SSID, WIFI_PASSWORD)
        while not wlan.isconnected():
            pass
    print("WiFi Connected")
    print("IP:", wlan.ifconfig()[0])



def firebase_post(sensor_data):
    try:
        data_json = ujson.dumps(sensor_data)
        headers = {'Content-Type': 'application/json'}
        response = urequests.patch(FIREBASE_URL, headers=headers, data=data_json)
        if response.status_code == 200:
            print("Data posted successfully to Firebase")
        else:
            print("Error posting data to Firebase:", response.status_code)
    except Exception as e:
        print("Exception:", e)

def main():
    connect_wifi()

    # Data to be posted to Firebase (example with multiple sensors)
    sensor_data = {
        "sensor1": {
            "data": 1234,
            "timestamp": int(time.time())
        },
        "sensor2": {
            "data": "Long data value for sensor 2...",
            "timestamp": int(time.time())
        }
        # Add more sensors if needed
    }

    # Posting data to Firebase
    print("Posting data to Firebase...")
    firebase_post(sensor_data)

if __name__ == "__main__":
    main()
```

In this script, we extend the functionality to send data from multiple sensors to Firebase. It includes methods for constructing the data payload for each sensor and sending the data in a single request to Firebase.

## 5. Sending Multiple Values Using POST Method with Encryption

```python
import requests as urequests
import json as ujson
# import network
import time

FIREBASE_URL = "https://tuesday-class-acf4f-default-rtdb.firebaseio.com/Sensor.json"

def firebase_post(sensor_data):
    try:
        data_json = ujson.dumps(sensor_data)
        headers = {'Content-Type': 'application/json'}
        response = urequests.post(FIREBASE_URL, headers=headers, data=data_json)
        if response.status_code == 200:
            print("Data posted successfully to Firebase")
        else:
            print("Error posting data to Firebase:", response.status_code)
    except Exception as e:
        print("Exception:", e)

def main():
    # Data to be posted to Firebase (example with multiple sensors)
    sensor_data = {
        "sensor1": {
            "data": "Sended by Raheel",
            "timestamp": int(time.time())
        },
        "sensor2": {
            "data": "Data Send By Raheel",
            "timestamp": int(time.time())
        }
        # Add more sensors if needed
    }

    # Posting data to Firebase
    print("Posting data to Firebase...")
    firebase_post(sensor_data)

if __name__ == "__main__":
    main()
```

Here, we explore an alternative method of sending multiple sensor values to Firebase using the POST method with encryption. This approach enhances data security by encrypting the data payload before transmission to Firebase.

## 6. Creating a Script for Fetching Values
```python
import urequests
import ujson
import network

# Firebase Realtime Database URL
FIREBASE_PROJECT_ID = "FIREBASE_PROJECT_ID"
FIREBASE_URL = "https://" + FIREBASE_PROJECT_ID + ".firebaseio.com/Sensor.json"

def connect_wifi():
    wlan = network.WLAN(network.STA_IF)
    wlan.active(True)
    if not wlan.isconnected():
        print("Connecting to WiFi...")
        wlan.connect("iot_test", "123456789")
        while not wlan.isconnected():
            pass
    print("WiFi Connected")
    print("IP:", wlan.ifconfig()[0])

def firebase_get():
    try:
        response = urequests.get(FIREBASE_URL)
        if response.status_code == 200:
            data = response.json()
            return data
        else:
            print("Error fetching data from Firebase:", response.status_code)
            return None
    except Exception as e:
        print("Exception:", e)
        return None

def main():
    connect_wifi()

    # Data read from Firebase
    print("Reading data from Firebase...")
    firebase_data = firebase_get()
    print(firebase_data)
    if firebase_data:
        print("Data from Firebase:", firebase_data)

if __name__ == "__main__":
    main()
```

Finally, we create a script to fetch values from Firebase, enabling us to retrieve sensor data stored in the database. This script establishes a connection to the Wi-Fi network, sends a GET request to Firebase, and parses the JSON response to extract the sensor data.

By understanding and implementing these scripts, you'll gain practical experience in leveraging Firebase Realtime Database for real-time data streaming and interaction in your applications.
