# GoRide Microservices Platform
 
A simple, decoupled ride-sharing application built with a microservices architecture. It features separate services for User, Captain (Driver), and Ride coordination, communicating asynchronously via RabbitMQ and accessed through a unified API Gateway.
 
## 🌟 Features
 
* **User & Captain Auth:** Complete signup, signin, and secure logout (with JWT blacklisting).
* **Ride Management:** Users create rides, and Captains can accept them, with status managed by the Ride service.
* **Asynchronous Communication:** Services are decoupled and communicate using RabbitMQ for events like new ride requests and acceptance.
* **Real-time Notifications:** Uses a **Long Polling** pattern for Captains to receive new ride requests instantly and for Users to monitor ride acceptance.
* **Data Optimization:** Ride history data is fetched via the Ride service and cached using **Redis** to improve performance.
* **Captain Availability:** Captains can toggle their availability status.
 
## 🧱 Architecture Overview
 
| Service | Port | Key Responsibilities | Technologies |
| :--- | :--- | :--- | :--- |
| **Gateway** | `3000` | API routing and entry point. | Express, `express-http-proxy` |
| **User** | `3001` | Auth, User Data, Ride History (with Redis caching). | Node.js, Mongoose, Redis, RabbitMQ, JWT |
| **Captain** | `3002` | Auth, Availability, Live Ride Request Notifications. | Node.js, Mongoose, RabbitMQ, JWT |
| **Ride** | `3003` | Ride creation and status management. | Node.js, Mongoose, RabbitMQ |
 
## ⚙️ Technology Stack
 
* **Runtime:** Node.js (type: module)
* **Framework:** Express
* **Database:** MongoDB (Mongoose)
* **Message Broker:** RabbitMQ (`amqplib`)
* **Caching:** Redis (`ioredis`)
* **Authentication:** JWT, Bcrypt
* **API Gateway:** `express-http-proxy`
 
## 🚀 Setup and Installation
 
### Prerequisites
 
You must have **MongoDB**, **RabbitMQ**, and **Redis** running locally.
 
### 1. Configuration
 
1.  In the `user`, `captain`, and `ride` directories, rename `.env.sample` to `.env`.
2.  Update `MONGO_URL` and `RABBITMQ_URL` in each `.env` file to match your environment.
 
### 2. Run All Services
 
Run `npm install` in all four directories (`gateway`, `user`, `captain`, `ride`), then start the services.
 
```bash
# In separate terminal windows, start each service:
 
cd gateway && node app.js    # Runs on port 3000
cd user && npm start         # Runs on port 3001
cd captain && npm start      # Runs on port 3002
cd ride && node app.js       # Runs on port 3003
 
