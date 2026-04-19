# Exercise 4: MQTT Publish/Subscribe Architecture

## 🎯 Objective
To deploy an enterprise-grade MQTT broker (EMQX) locally via Docker, configure Node-RED as an automated MQTT publishing client, and utilize Postman as an MQTT subscribing client to explore event-driven communication.

## 🛠️ Tools & Technologies
- **Docker Desktop**: To host the EMQX Broker container.
- **EMQX Broker**: An enterprise-grade MQTT message broker.
- **Node-RED**: Acting as the IoT Edge Publisher.
- **Postman**: Acting as the MQTT Subscriber (BETA feature).

## 📦 Docker Configuration
The EMQX broker was deployed using Docker Compose with the following port mappings:
- **18083**: EMQX Dashboard Management
- **1883**: Standard MQTT protocol

## 📡 MQTT Implementation Details

### 1. Publisher (Node-RED)
Node-RED was configured to simulate an IoT device publishing JSON telemetry data every 5 seconds:
- **Topic**: campus/lab1/temperature
- **Payload**: {"temperature": 25.5, "sensor": "ESP32_01"}

### 2. Subscriber (Postman)
Postman was utilized to subscribe to the topic campus/lab1/temperature using the broker URL mqtt://localhost:1883. Real-time data streams were successfully observed in the Postman message window.

### 3. Topic Hierarchies & Wildcards
Experiments were conducted using MQTT wildcards to demonstrate efficient data filtering:
- **Multi-Level Wildcard (#)**: Subscribing to campus/# allowed receiving messages from all sub-topics including lab1/temperature, lab2/temperature, and lab1/humidity.
- **Single-Level Wildcard (+)**: Subscribing to campus/+/temperature allowed receiving data from lab1 and lab2 temperature sensors but filtered out the humidity topic.

## 🧠 Architectural Analysis

### Q1: The Role of the MQTT Broker
Unlike HTTP's direct client-to-server model, MQTT utilizes a broker as a central mediator.
- **Advantage**: MQTT is a lightweight messaging protocol. It is more efficient for IoT devices due to low bandwidth overhead and its ability to handle intermittent connectivity better than HTTP.

### Q2: Publisher vs. Subscriber Roles
- **Publisher**: The entity that sends data to a specific topic on the broker.
- **Subscriber**: The entity that listens for and receives data from specific topics via the broker.
- **Dual Role**: A single IoT device (e.g., ESP32) can be both a publisher and a subscriber simultaneously.
- **Real-world Example**: A smart thermostat publishes temperature data to a broker while simultaneously subscribing to a "control" topic to receive remote settings or updates.

### Q3: Client Disconnection Handling
In a standard MQTT setup, if a subscriber (Postman) disconnects while a publisher (Node-RED) continues to send data, the data is typically lost unless specific features like QoS (Quality of Service) or Retained Messages are configured to store the last known value at the broker.
