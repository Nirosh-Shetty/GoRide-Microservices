# GoRide Microservices Platform
 
A simple, decoupled ride-sharing application built with a microservices architecture. The platform consists of separate services for User management, Captain (Driver) management, and Ride coordination, communicating asynchronously via RabbitMQ and accessed through a unified API Gateway.
 
## 🌟 Features
 
* **User & Captain Management:** Complete authentication flow (signup/signin/logout) for both users and captains.
* **Ride Request & Creation:** Users can request a ride by providing pickup and destination, which creates a ride record and publishes an event to a queue.
* **Real-time Ride Notification (Long Polling):** Captains use long polling (`/wait-for-new-ride`) to receive real-time notifications for new ride requests from the RabbitMQ queue.
* **Asynchronous Communication:** Services are decoupled and communicate using a message queue (RabbitMQ) for events like new ride requests and ride acceptance.
* **Captain Availability Toggle:** Captains can toggle their `isAvailable` status to control whether they receive new ride requests.
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
 
You must have the following running locally:
 
* **MongoDB** (Database)
* **RabbitMQ** (Message Broker)
* **Redis** (Caching)
 
### 1. Configuration
 
In the `user`, `captain`, and `ride` service directories, rename the `.env.sample` file to `.env` and configure your settings. You must ensure `MONGO_URL` and `RABBITMQ_URL` are correct for your local environment.
 
* `user/.env.sample`
* `captain/.env.sample`
* `ride/.env.sample`
 
### 2. Install Dependencies
 
You must run the installation command in each of the four service directories (`gateway`, `user`, `captain`, `ride`):
 
```bash
# In each of the four directories:
cd <service-name>
npm install
 
