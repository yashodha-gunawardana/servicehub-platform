# ServiceHub Platform

## Enterprise Cloud Architecture – Microservices Platform

ServiceHub is a cloud-based home service request platform designed using a **microservices architecture**. The platform connects customers with service providers and provides a scalable backend infrastructure using Spring Cloud components.

This repository contains the **platform-level components** of the ServiceHub application, including the Configuration Server, Eureka Service Registry, and API Gateway, together with the frontend application as a Git submodule.

---

## 👨‍🎓 Student Information

| Information | Details |
|---|---|
| Student Name | Yashoda Gunawardhana |
| Student ID | 241711077 |
| Project | ServiceHub |
| Component | platforms |
| GCP Project ID | project-a6d8ea92-fb5d-4ed6-99d |

---

## 🏗️ Architecture

ServiceHub follows a distributed microservices architecture:

```
                         ┌──────────────────────┐
                         │   ServiceHub Web      │
                         │   React + Vite        │
                         └──────────┬────────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │     API Gateway       │
                         │      Port 8080        │
                         └──────────┬────────────┘
                                    │
                  ┌─────────────────┼─────────────────┐
                  │                 │                  │
                  ▼                 ▼                  ▼
          ┌──────────────┐  ┌──────────────┐  ┌──────────────┐
          │ User Service │  │   Request    │  │   Provider   │
          │   :8081      │  │   Service    │  │   Service    │
          │              │  │    :8082     │  │    :8083     │
          └──────┬───────┘  └──────┬───────┘  └──────┬───────┘
                 │                 │                  │
                 ▼                 ▼                  ▼
              MySQL            MongoDB          Provider Data


                  ┌──────────────────────────────┐
                  │       Eureka Server           │
                  │          :8761                │
                  │     Service Discovery         │
                  └──────────────────────────────┘

                  ┌──────────────────────────────┐
                  │       Config Server           │
                  │          :8888                │
                  │ Centralized Configuration     │
                  └──────────────────────────────┘
```

---

## 🚀 Platform Components

### Config Server

The Config Server provides centralized configuration management for all ServiceHub microservices.

**Port:** `8888`

Responsibilities:

- Centralized application configuration
- Externalized service configuration
- Consistent configuration across service instances
- Configuration management for distributed services

---

### Eureka Server

The Eureka Server acts as the **service registry and discovery server** for the ServiceHub microservices.

**Port:** `8761`

Responsibilities:

- Service registration
- Service discovery
- Health/status monitoring
- Dynamic service instance discovery
- Supporting horizontal scaling of backend services

---

### API Gateway

The API Gateway provides a single entry point for frontend requests.

**Port:** `8080`

Responsibilities:

- Routing client requests to backend microservices
- Service discovery integration
- Centralized API entry point
- Supporting scalable service communication

---

## 🔧 Microservices

The ServiceHub backend consists of the following application services:

| Service | Port | Database | Responsibility |
|---|---:|---|---|
| User Service | 8081 | MySQL | User, customer, provider and administrator management |
| Request Service | 8082 | MongoDB | Service request creation and management |
| Provider Service | 8083 | MySQL | Service provider information and service management |

The backend services communicate through the Spring Cloud ecosystem and are registered with Eureka for service discovery.

---

## 🛠️ Technology Stack

### Backend

- Java 25
- Spring Boot
- Spring Cloud
- Spring Data
- Spring Cloud Config
- Netflix Eureka
- Spring Cloud Gateway
- Maven

### Databases

- MySQL
- MongoDB

### Frontend

- React
- TypeScript
- Vite
- Axios
- React Router
- Tailwind CSS

### Cloud Platform

- Google Cloud Platform (GCP)
- Compute Engine
- Managed Instance Groups
- Instance Templates
- Machine Images
- Cloud SQL
- Firestore / MongoDB-compatible non-relational storage
- Cloud Storage
- VPC Network
- Cloud Load Balancing
- Cloud NAT
- Cloud Router
- Cloud DNS
- Cloud Run
- Firewall Rules
- Service Accounts

### Process Management

- PM2

---

## 📁 Repository Structure

```
servicehub-platform/
│
├── backend/
│   ├── platform/
│   │   ├── config-server/
│   │   ├── eureka-server/
│   │   └── api-gateway/
│   │
│   └── services/
│       ├── user-service/
│       ├── request-service/
│       └── provider-service/
│
├── frontend/
│   └── servicehub-web/
│
├── .gitmodules
├── .gitignore
└── README.md
```

The project follows a **polyrepo architecture using Git submodules**, allowing each microservice and platform component to maintain its own independent repository.

---

## 🔄 Request Flow

A typical request follows this flow:

```
User
  │
  ▼
ServiceHub Web
  │
  ▼
API Gateway :8080
  │
  ▼
Eureka Service Registry :8761
  │
  ├──► User Service :8081
  │
  ├──► Request Service :8082
  │
  └──► Provider Service :8083
```

Configuration is provided through:

```
Config Server :8888
```

---

## 💻 Local Development

### Prerequisites

Install the following:

- JDK 25
- Maven
- Node.js
- npm
- MySQL
- MongoDB
- Git

### Clone the Repository

```bash
git clone https://github.com/yashodha-gunawardana/servicehub-platform.git
cd servicehub-platform
```

### Initialize Git Submodules

```bash
git submodule update --init --recursive
```

### Start the Platform

Start the components in the following order:

```
1. Config Server
2. Eureka Server
3. API Gateway
4. User Service
5. Request Service
6. Provider Service
7. Frontend
```

This order allows the backend services to obtain centralized configuration and register with Eureka before receiving requests through the API Gateway.

---

## ☁️ Google Cloud Deployment

ServiceHub is designed for deployment on **Google Cloud Platform** using a scalable cloud architecture.

### Backend – IaaS

The backend microservices are deployed using Google Compute Engine infrastructure.

The deployment includes:

- Compute Engine VM instances
- Instance Templates
- Machine Images
- Managed Instance Groups
- Horizontal Auto Scaling
- Health Checks
- Multiple availability zones
- VPC networking
- Firewall rules
- Cloud NAT
- Cloud Router
- Load Balancing

### Frontend – PaaS / Serverless

The frontend is deployed separately using a serverless/PaaS-oriented deployment model.

### Cloud Storage

ServiceHub uses Google Cloud Storage for file/object storage as part of the cloud architecture.

---

## 📈 Scalability and High Availability

The backend architecture is designed to support horizontal scaling.

Managed Instance Groups allow multiple instances of backend services to run simultaneously.

The architecture supports:

- Automatic instance creation
- Horizontal scaling
- Health-based instance management
- Multiple VM instances
- Multi-zone deployment
- Service discovery through Eureka
- Centralized configuration
- Load balancing

This allows the platform to continue serving requests when individual service instances become unavailable.

---

## 🔐 Security

The cloud deployment uses Google Cloud networking and access-control mechanisms including:

- VPC Network
- Firewall Rules
- Service Accounts
- IAM
- Cloud NAT
- Private/internal communication where appropriate
- Restricted service access

Sensitive credentials and configuration values should not be committed to Git repositories.

---

## 🧪 Service Verification

The following endpoints can be used to verify the platform components during deployment:

| Component | Endpoint |
|---|---|
| Config Server | `http://<HOST>:8888` |
| Eureka Server | `http://<HOST>:8761` |
| API Gateway | `http://<HOST>:8080` |

The Eureka dashboard should display registered ServiceHub services and their current status.

---

## 📊 Service Discovery

Each backend service registers itself with Eureka.

Example:

```
EUREKA-SERVER
      │
      ├── API-GATEWAY
      ├── USER-SERVICE
      ├── REQUEST-SERVICE
      └── PROVIDER-SERVICE
```

The API Gateway uses service discovery to route requests to the appropriate backend service.

---

## 🔀 Git Submodules

This repository uses Git submodules to manage independently maintained ServiceHub components.

To initialize all submodules:

```bash
git submodule update --init --recursive
```

To update submodules:

```bash
git submodule update --remote --merge
```

---



