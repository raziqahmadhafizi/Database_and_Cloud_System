# Exercise 5: MQTT Reliability, QoS Handshakes, and Retained Messages  

## 🎯 Laboratory Objectives  
The primary goal of this exercise is to master the reliability mechanisms of the MQTT protocol by:  
1. **Role Reversal**: Configuring Node-RED as a passive Subscriber and Postman as an active Publisher.  
2. **QoS Analysis**: Observing the specific network handshakes for QoS levels 0, 1, and 2 using Wireshark.  
3. **State Management**: Implementing the Retain flag to manage asynchronous device states via the broker.  

---

## 🛠️ System Configuration  

### 1. Subscriber (Node-RED)  
- **Node**: `mqtt in` node wired to a `debug` node  
- **Broker**: `localhost:1883` (EMQX Docker Container)  
- **Topic**: `campus/qos/test`  
- **QoS Level**: 2 (Ensures the client can handle all reliability levels up to Exactly Once)  

### 2. Publisher (Postman)  
- **Connection**: `mqtt://localhost:1883`  
- **Payload**: JSON telemetry data used to trigger specific handshakes  

---

## 🔬 Experimental Results & Wireshark Analysis  

Based on packet sniffing performed on the Loopback Adapter (filter: `mqtt`), the following transmission behaviors were observed for each QoS level:  

| QoS Level | Mechanism Name | Packet Count | Captured Sequence (Wireshark) |
|----------|----------------|-------------|-------------------------------|
| **QoS 0** | *At Most Once* | 1 | A single `Publish Message` is sent with no acknowledgement from the broker (Fire and Forget) |
| **QoS 1** | *At Least Once* | 2 | `Publish Message` followed by a `Publish Ack (PUBACK)` from the broker |
| **QoS 2** | *Exactly Once* | 4 | `Publish`, `PUBREC`, `PUBREL`, and `PUBCOMP` (4-way handshake) |

---

## 💾 Retained Messages Mechanism (Step 4)  

This test verified the broker's ability to cache the "last known state" of a device:  

1. **Subscriber Disconnected**  
   - The wire between the MQTT node and Debug node in Node-RED was deleted to simulate an offline state  

2. **Retained Publication**  
   - Postman published:
     ```json
     {"status": "OPEN"}
     ```
   - Topic: `campus/state/door`  
   - Retain flag set to **True**  

3. **Subscriber Reconnected**  
   - Upon re-deploying Node-RED, the message was received **instantly** without new publication  

4. **Packet Proof**  
   - Wireshark confirmed **Retain flag = 1** in the Publish header  

---

## 🧠 Discussion & Architectural Analysis  

### 1. QoS Selection for Battery-Powered Devices (ESP32)  
- **Recommended Level**: QoS 0 or QoS 1  
- **Analysis**:  
  - QoS 2 uses a 4-way handshake  
  - Forces WiFi radio to stay active longer  
  - Results in:
    - Faster battery drain  
    - Higher bandwidth consumption  

---

### 2. Standard MQTT vs. Retained Messages  

- **Standard Delivery**  
  - Only delivers messages to currently connected subscribers  
  - Messages are lost if the client is offline  

- **Retained Message**  
  - Broker stores the last message  
  - New subscribers receive it immediately upon connection  
  - Ensures latest state synchronization  

---

### 3. Industrial IoT (IIoT) Use Case  

- **Scenario**  
  - Monitoring:
    - Emergency Stop Status  
    - Gas Valve State  

- **Requirement**  
  - System must always reflect the **current state instantly**  

- **Risk Without Retain**  
  - Delayed updates  
  - Temporary blindness to critical system conditions  
  - Potential safety hazard  

---

## 📂 Project Files  

- `exercise_5.json` → Node-RED flow configuration  
- `docker-compose.yml` → EMQX broker setup  
- `/screenshots` → Wireshark evidence and Retain flag verification  
