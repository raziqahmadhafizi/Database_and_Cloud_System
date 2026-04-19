# 🚀 Exercise 4: MQTT Publish/Subscribe Architecture

## 🎯 Objective
[cite_start]To deploy an enterprise-grade **MQTT broker (EMQX)** locally via Docker, configure **Node-RED** as an automated MQTT publishing client, and utilize **Postman** as an MQTT subscribing client to explore event-driven communication[cite: 313].

---

## 🛠️ Tools & Technologies

* [cite_start]💻 **Docker Desktop**: Used to host the EMQX Broker container in a localized environment [cite: 314-315].
* [cite_start]🐝 **EMQX Broker**: An enterprise-grade MQTT message broker for efficient message distribution[cite: 313, 369].
* [cite_start]🔴 **Node-RED**: Configured as the **IoT Edge Publisher** to simulate sensor data[cite: 380].
* [cite_start]🚀 **Postman**: Utilized as the **MQTT Subscriber** to monitor real-time data streams [cite: 427-428, 461].

---

## 📦 Docker Configuration

The EMQX broker was deployed using **Docker Compose** with the following critical port mappings:

| Port | Description |
| :--- | :--- |
| `18083` | [cite_start]**EMQX Dashboard Management** (Web UI) [cite: 330, 365] |
| `1883` | [cite_start]**Standard MQTT Protocol** (Connection Port) [cite: 332] |

---

## 📡 MQTT Implementation Details

### 1️⃣ Publisher (Node-RED)
[cite_start]Node-RED was programmed to act as an IoT device publishing JSON telemetry data periodically [cite: 380-381]:
* [cite_start]**Topic**: `campus/lab1/temperature` [cite: 405]
* [cite_start]**Payload**: `{"temperature": 25.5, "sensor": "ESP32_01"}` [cite: 383]
* [cite_start]**Interval**: Automatic publish every **5 seconds**[cite: 383, 399].

### 2️⃣ Subscriber (Postman)
[cite_start]Postman connected to the broker via `mqtt://localhost:1883` to listen for incoming messages[cite: 463, 469]:
* [cite_start]Successful real-time data reception was confirmed in the **Message Stream** window [cite: 504-505].

### 3️⃣ Topic Hierarchies & Wildcards
[cite_start]Experimental topic filtering was performed using wildcards [cite: 506-507]:
* [cite_start]**Multi-Level (`#`)**: Subscribing to `campus/#` captured data from all sub-topics under the campus hierarchy [cite: 515-517].
* [cite_start]**Single-Level (`+`)**: Subscribing to `campus/+/temperature` filtered data from specific labs but excluded other data types like humidity [cite: 520-522].

---

## 🧠 Architectural Analysis

> **Q1: The Role of the Broker**
> [cite_start]Unlike the direct request-response model of HTTP, MQTT uses a central mediator known as a broker [cite: 526-527]. [cite_start]The broker allows for **decoupled communication**, making it significantly more lightweight and efficient for low-power IoT devices[cite: 528].

> **Q2: Publisher vs. Subscriber**
> [cite_start]A **Publisher** sends data to a topic, while a **Subscriber** receives data from topics it is interested in [cite: 529-530]. [cite_start]An IoT device can perform **both roles simultaneously**, such as publishing sensor data and subscribing to control commands from a server [cite: 531-532].

> **Q3: Client Disconnection**
> [cite_start]In MQTT, if a subscriber disconnects, incoming data is typically lost unless features like **QoS (Quality of Service)** or **Retained Messages** are used by the broker to store the latest information for the client [cite: 535-536].
