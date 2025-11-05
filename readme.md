# GoRide Microservices Platform
 
A simple, decoupled ride-sharing application built with a microservices architecture. It features separate services for User, Captain (Driver), and Ride coordination, communicating asynchronously via RabbitMQ and accessed through a unified API Gateway.
 
## 🌟 Features
 
* **User & Captain Management:** Complete authentication flow (signup/signin/logout) for both users and captains.
* **Ride Request & Creation:** Users can request a ride by providing pickup and destination, which creates a ride record and publishes an event to a queue.
* **Real-time Ride Notification (Long Polling):** Captains use long polling (`/wait-for-new-ride`) to receive real-time notifications for new ride requests from the RabbitMQ queue.
* **Asynchronous Communication:** Services are decoupled and communicate using a message queue (RabbitMQ) for events like new ride requests and ride acceptance.
* **Ride History Caching:** Ride history data is fetched from the Ride service and cached using Redis to reduce database load and improve response time.
* **Secure Token Management:** Uses blacklisting for JWT tokens to securely handle user and captain logout.
 
## 🧱 Architecture Overview
 
The application is composed of four main services, routing through the API Gateway:
 
| Service | Port | Key Responsibilities | Technologies |
| :--- | :--- | :--- | :--- |
| **Gateway** | `3000` | Routes all incoming requests to the appropriate service. | `express`, `express-http-proxy` |
| **User** | `3001` | User Auth, User Data, Ride History (with Redis caching). | Node.js/Express, Mongoose, Redis, RabbitMQ, JWT, Bcrypt |
| **Captain** | `3002` | Captain Auth, Availability Toggle, Receives Live Ride Requests. | Node.js/Express, Mongoose, RabbitMQ, JWT, Bcrypt |
| **Ride** | `3003` | Manages the creation, acceptance, and status updates of all rides. | Node.js/Express, Mongoose, RabbitMQ |
 
## ⚙️ Technology Stack
 
* **Backend Runtime:** Node.js (type: module)
* **Web Framework:** Express
* **Database:** MongoDB via Mongoose ORM
* **Message Broker:** RabbitMQ (`amqplib`)
* **Caching:** Redis (`ioredis`)
* **Authentication:** JWT and Bcrypt
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
 
