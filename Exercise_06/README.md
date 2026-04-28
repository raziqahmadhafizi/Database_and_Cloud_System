# Exercise 6: Database Foundations (MongoDB Integration)

## 🎯 Objectives
The primary goal of this exercise is to integrate persistent storage into the IoT architecture by:
1.  **Database Deployment**: Deploying a NoSQL MongoDB instance locally using Docker.
2.  **Node-RED Integration**: Integrating the MongoDB driver into Node-RED to handle data persistence.
3.  **CRUD Operations**: Upgrading HTTP APIs to perform Create, Read, Update, and Delete operations on persistent storage.

---

## 🛠️ System Configuration

### 1. Database (MongoDB)
* **Image**: `mongodb/mongodb-community-server`
* **Port**: `27017:27017` (Maps container port to local machine).
* **Type**: NoSQL Document-based database.

### 2. API Middleware (Node-RED)
* **Nodes**: `http in`, `function`, `mongodb4`, `http response`.
* **Database Driver**: `node-red-contrib-mongodb4`.

---

## 🔬 CRUD Operations Analysis

### A. CREATE & READ (POST & GET)
* **POST /api/sensor**: Receives JSON data (e.g., temperature, humidity) and saves it as a new document in the collection.
* **GET /api/sensor**: Queries the collection and returns all stored sensor records in a JSON array.

### B. UPDATE & DELETE
* **PUT /api/sensor**: Updates existing records using filters (e.g., `device_id`) and the `$set` operator to modify specific fields.
* **DELETE /api/sensor**: Removes specific documents from the database based on criteria like `status: "error"`.

---

## 🧠 Discussion & Architectural Analysis (Part 5)

### 1. Dynamic Schema Generation (Schema-less)
**Concept**: MongoDB does not require a predefined structure or "CREATE TABLE" query. It is schema-less, meaning documents in the same collection can have different fields.
**Benefit**: This is highly beneficial for IoT prototyping where sensor data formats change frequently. Developers can add new sensor parameters (e.g., adding "CO2 levels") without having to migrate or redefine the database structure.

### 2. Middleware Abstraction & Security
**Defense**: In this architecture, the ESP32 (client) only communicates with Node-RED via HTTP, and Node-RED handles the database queries.
**Security Risk**: Exposing port `27017` directly to the public internet is a massive security risk as it allows unauthorized entities to attempt brute-force attacks or exploit database vulnerabilities directly. Using Node-RED as middleware provides a security layer for authentication and data validation before any write/read operations occur.

---

## 📂 Project Files
* `README.md`: Project documentation and analysis.
* `docker-compose.yml`: Docker configuration for MongoDB.
* `exercise_6.json`: Exported Node-RED CRUD flows.
* `/screenshots`: Evidence of CRUD execution and MongoDB storage.
