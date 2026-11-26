#  IoT-Based Smart Irrigation System

An intelligent **IoT + Machine Learning-based irrigation system** that monitors real-time environmental conditions and automatically determines whether crops require watering and for how long.

The system combines **ESP32 sensors, Machine Learning models, a Flask REST API, and Blynk** to provide automated irrigation, remote monitoring, and manual pump control.

---

##  Project Overview

Traditional irrigation methods often rely on fixed schedules or manual decisions, which can lead to **overwatering, underwatering, and inefficient water usage**.

This project addresses the problem by collecting real-time environmental data and using Machine Learning to make irrigation decisions.

### System Flow

```text
         Crop / Soil
             │
             ▼
      ┌───────────────┐
      │ ESP32 Sensors │
      └───────┬───────┘
              │
     Soil Moisture
     Temperature
     Humidity
              │
              ▼
       ┌─────────────┐
       │ Flask REST  │
       │     API     │
       └──────┬──────┘
              │
              ▼
       ┌─────────────┐
       │ ML Models   │
       │             │
       │ Irrigation  │
       │ Decision    │
       │     +       │
       │ Duration    │
       └──────┬──────┘
              │
              ▼
       ┌─────────────┐
       │ ESP32 Relay  │
       │ Pump Control │
       └─────────────┘
              │
              ▼
          Irrigation
```

The system supports two operating modes:

- **Automatic Mode** – Machine Learning determines whether irrigation is required and predicts the irrigation duration.
- **Manual Mode** – The user controls the irrigation pump remotely through the Blynk mobile application.

---

##  Features

-  Real-time temperature monitoring
-  Real-time soil moisture monitoring
-  Humidity monitoring
-  ML-based irrigation decision making
-  Irrigation duration prediction
-  Remote monitoring through Blynk
-  Remote manual pump control
-  Automatic pump shut-off after the predicted duration
-  Model retraining support
-  Live sensor data visualization
-  REST API for communication between ESP32 and ML backend

---

##  Machine Learning

The Machine Learning component uses environmental and crop-related information to support irrigation decisions.

### Input Parameters

The system can use parameters such as:

- Soil moisture
- Temperature
- Humidity
- Crop type
- Growth stage

### ML Outputs

The system predicts:

1. **Whether irrigation is required**
   - `Water`
   - `No Water`

2. **Required irrigation duration**

The trained models are stored using `Joblib` and loaded by the Flask backend for real-time predictions.

---

##  Technology Stack

### Software

| Technology | Purpose |
|---|---|
| **Python** | ML development and backend logic |
| **Scikit-learn** | Machine Learning models |
| **Pandas** | Data processing |
| **NumPy** | Numerical operations |
| **Joblib** | Model serialization |
| **Flask** | REST API / ML inference server |
| **Matplotlib** | Data visualization |

### Hardware

| Component | Purpose |
|---|---|
| **ESP32** | Sensor data collection and device control |
| **DHT22** | Temperature and humidity measurement |
| **Soil Moisture Sensor** | Soil moisture measurement |
| **Relay Module** | Pump switching/control |
| **Water Pump** | Automated irrigation |

### IoT Platform

**Blynk** – Used for remote monitoring and manual control.

---

##  Project Structure

```text
smart-irrigation/
│
├── backend/
│   ├── iot.py
│   ├── server.py
│   └── ...
│
├── models/
│   └── *.pkl
│
├── esp32/
│   └── *.ino
│
├── data/
│   └── ...
│
├── requirements.txt
│
└── README.md
```

> Update the structure above according to the actual files and folders in your repository.

---

#  Setup Instructions

## 1. Clone the Repository

```bash
git clone https://github.com/your-username/smart-irrigation.git
cd smart-irrigation
```

---

## 2. Install Dependencies

Install the required Python packages:

```bash
pip install pandas numpy scikit-learn flask joblib matplotlib
```

Or, if you have a `requirements.txt` file:

```bash
pip install -r requirements.txt
```

---

## 3. Train the Machine Learning Models

Run the model training script:

```bash
python backend/iot.py
```

This processes the training data and generates the trained ML model files used by the Flask API.

---

## 4. Start the Flask Server

Run:

```bash
python backend/server.py
```

The server will start on:

```text
http://0.0.0.0:5000
```

The ESP32 communicates with the Flask server through the prediction endpoint.

---

## 5. Configure ESP32

Open the `.ino` file using **Arduino IDE**.

Update the following:

- Wi-Fi SSID
- Wi-Fi password
- Flask server IP address
- Blynk authentication credentials

Example:

```cpp
const char* serverName = "http://YOUR_IP:5000/predict";
```

Replace `YOUR_IP` with the local IP address of the computer running the Flask server.

---

## 6. Upload Code to ESP32

1. Connect the ESP32 to your computer.
2. Open the `.ino` file in Arduino IDE.
3. Select the appropriate ESP32 board.
4. Select the correct COM port.
5. Upload the program.
6. Open Serial Monitor to verify sensor readings and API communication.

---

#  Blynk Controls

The Blynk dashboard provides real-time monitoring and control.

| Virtual Pin | Function |
|---|---|
| `V0` | Soil Moisture |
| `V1` | Temperature |
| `V2` | Humidity |
| `V3` | Manual Pump Control |
| `V4` | Crop Type |
| `V5` | Growth Stage |
| `V6` | Auto / Manual Mode |

---

#  Operating Modes

###  Automatic Mode

```text
Sensor Data
     ↓
ESP32
     ↓
Flask API
     ↓
Machine Learning Model
     ↓
Irrigation Decision
     ↓
Duration Prediction
     ↓
Relay
     ↓
Water Pump
```

The system automatically activates the pump when irrigation is required and stops it after the predicted irrigation duration.

###  Manual Mode

In Manual Mode, the user can control the pump directly through the Blynk application.

```text
Blynk App
    ↓
ESP32
    ↓
Relay
    ↓
Water Pump
```

---

#  API

The Flask backend provides an API endpoint for receiving sensor data and returning ML predictions.

### Prediction Endpoint

```text
POST /predict
```

The ESP32 sends environmental information to the Flask server, which processes the data using the trained ML models.

Example request structure:

```json
{
    "soil_moisture": 45,
    "temperature": 28,
    "humidity": 65,
    "crop_type": "Tomato",
    "growth_stage": "Vegetative"
}
```

The API returns the irrigation decision and predicted duration.

> Update the example request/response according to the exact implementation in your Flask API.

---

#  Configuration

Before running the project, configure:

- Wi-Fi credentials
- Blynk authentication token
- Flask server IP address
- Sensor pin configuration
- Relay pin configuration

For security, avoid committing API keys, passwords, Wi-Fi credentials, or Blynk tokens to GitHub.

Use a configuration file or environment variables where appropriate.

---

#  Important Notes

- Ensure the **ESP32 and Flask server are connected to the same network**.
- Verify the Flask server's local IP address before uploading the ESP32 code.
- Update the server URL in the `.ino` file.
- Ensure the soil moisture sensor is correctly calibrated.
- Ensure the relay module is connected correctly before connecting the water pump.
- Keep credentials and API tokens out of the public repository.

---

#  Future Improvements

Potential improvements include:

- Weather API integration
- Rainfall prediction
- Real-time weather-based irrigation decisions
- Advanced ML models for crop-specific irrigation
- Cloud deployment of the Flask API
- Database integration for historical sensor data
- ML model performance monitoring
- Water consumption analytics
- Mobile notifications for low soil moisture
- Multi-field / multi-crop support

---

#  Authors

**Disha Desai**  
**Sharvani Surve**  
**Aastha Kamble**  
**Vaishnavi Bavkar**

---

#  License

This project is developed for **academic and educational purposes**.
