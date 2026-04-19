# Exercise 4: MQTT Publish/Subscribe Architecture

## 🎯 Objective
[cite_start]To deploy an enterprise-grade MQTT broker (EMQX) locally via Docker, configure Node-RED as an automated MQTT publishing client, and utilize Postman as an MQTT subscribing client to explore event-driven communication[cite: 313].

## 🛠️ Tools & Technologies
* [cite_start]**Docker Desktop**: To host the EMQX Broker container [cite: 314-315].
* [cite_start]**EMQX Broker**: An enterprise-grade MQTT message broker[cite: 313, 369].
* [cite_start]**Node-RED**: Acting as the IoT Edge Publisher[cite: 380].
* [cite_start]**Postman**: Acting as the MQTT Subscriber (BETA feature) [cite: 427-428, 461].

## 📦 Docker Configuration
The EMQX broker was deployed using Docker Compose with the following port mappings:
* [cite_start]**18083**: EMQX Dashboard Management[cite: 330, 365].
* [cite_start]**1883**: Standard MQTT protocol[cite: 332].

## 📡 MQTT Implementation Details

### 1. Publisher (Node-RED)
[cite_start]Node-RED was configured to simulate an IoT device publishing JSON telemetry data every 5 seconds[cite: 383]:
* [cite_start]**Topic**: `campus/lab1/temperature`[cite: 405, 413].
* [cite_start]**Payload**: `{"temperature": 25.5, "sensor": "ESP32_01"}`[cite: 383, 393].

### 2. Subscriber (Postman)
[cite_start]Postman was utilized to subscribe to the topic `campus/lab1/temperature` using the broker URL `mqtt://localhost:1883` [cite: 463, 467-469]. [cite_start]Real-time data streams were successfully observed in the Postman message window [cite: 504-505].

### 3. Topic Hierarchies & Wildcards
[cite_start]Experiments were conducted using MQTT wildcards to demonstrate efficient data filtering [cite: 506-507]:
* [cite_start]**Multi-Level Wildcard (`#`)**: Subscribing to `campus/#` allowed receiving messages from all sub-topics including `lab1/temperature`, `lab2/temperature`, and `lab1/humidity` [cite: 515-517].
* [cite_start]**Single-Level Wildcard (`+`)**: Subscribing to `campus/+/temperature` allowed receiving data from `lab1` and `lab2` temperature sensors but filtered out the `humidity` topic [cite: 520-522].

## 🧠 Architectural Analysis

### Q1: The Role of the MQTT Broker
[cite_start]Unlike HTTP's direct client-to-server model, MQTT utilizes a broker as a central mediator [cite: 526-527]. 
* [cite_start]**Advantage**: MQTT is a lightweight messaging protocol[cite: 461]. [cite_start]It is more efficient for IoT devices due to low bandwidth overhead and its ability to handle intermittent connectivity better than HTTP[cite: 528].

### Q2: Publisher vs. Subscriber Roles
* [cite_start]**Publisher**: The entity that sends data to a specific topic on the broker[cite: 530].
* [cite_start]**Subscriber**: The entity that listens for and receives data from specific topics via the broker[cite: 530].
* [cite_start]**Dual Role**: A single IoT device (e.g., ESP32) can be both a publisher and a subscriber simultaneously[cite: 531]. 
* [cite_start]**Real-world Example**: A smart thermostat publishes temperature data to a broker while simultaneously subscribing to a "control" topic to receive remote settings or updates[cite: 532].

### Q3: Client Disconnection Handling
[cite_start]In a standard MQTT setup, if a subscriber (Postman) disconnects while a publisher (Node-RED) continues to send data, the data is typically lost unless specific features like **QoS (Quality of Service)** or **Retained Messages** are configured to store the last known value at the broker [cite: 535-536].
