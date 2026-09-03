# ServiceHub – Microservice Platform

ServiceHub is a **cloud-native home service request management system** developed using a **microservice architecture** for the **ITS 2130 – Enterprise Cloud Architecture** final project.

The system is designed to connect customers who need home services with service providers through a distributed backend architecture. The application provides a centralized platform for managing users, service requests, and service providers while demonstrating cloud-native deployment and scalability using **Google Cloud Platform (GCP)**.

This repository contains the **platform-level components** of ServiceHub — the Config Server, Eureka Service Registry, and API Gateway — together with the frontend application as a Git submodule.

---

## 👨‍🎓 Student Information

| Information         | Details                          |
| -------------------- | -------------------------------- |
| **Student Name**     | Yashoda Gunawardhana             |
| **Student ID**       | `241711077`                      |
| **Project**          | ServiceHub                       |
| **Component**        | platforms                        |
| **GCP Project ID**   | `project-a6d8ea92-fb5d-4ed6-99d` |

---

## 📌 Project Overview

ServiceHub is designed as a distributed home service management platform consisting of three main business microservices:

* **User Service** – Manages customer, provider, and administrator information.
* **Request Service** – Manages home service requests and request-related operations.
* **Provider Service** – Manages service provider information and provider-related services.

The backend is supported by three Spring Cloud platform components:

* **Config Server** – Centralized configuration management.
* **Eureka Server** – Service registration and discovery.
* **API Gateway** – Single entry point for backend API requests.

The frontend application communicates with the backend through the API Gateway, while the services communicate with each other through service discovery.

The project is deployed on **Google Cloud Platform** with infrastructure designed to support high availability, health checking, load balancing, automatic scaling, and process-level recovery.

---

## 🏗️ System Architecture

```text
                         ┌─────────────────────────┐
                         │    ServiceHub Frontend   │
                         │     React + Vite Web     │
                         └────────────┬────────────┘
                                      │
                                      ▼
                         ┌─────────────────────────┐
                         │       API Gateway        │
                         │          :8080            │
                         └────────────┬────────────┘
                                      │
                     ┌────────────────┼────────────────┐
                     │                │                │
                     ▼                ▼                ▼
              ┌────────────┐  ┌────────────┐  ┌────────────┐
              │   User     │  │  Request   │  │  Provider  │
              │  Service   │  │  Service   │  │  Service   │
              │   :8081    │  │   :8082    │  │   :8083    │
              └──────┬─────┘  └──────┬─────┘  └──────┬─────┘
                     │               │               │
                     ▼               ▼               ▼
                   MySQL          MongoDB       Provider Data

                         ┌─────────────────────────┐
                         │      Eureka Server       │
                         │          :8761            │
                         │   Service Discovery       │
                         └─────────────────────────┘

                         ┌─────────────────────────┐
                         │      Config Server       │
                         │          :8888            │
                         │ Centralized Configuration │
                         └─────────────────────────┘
```

### Request Flow

```text
User
  │
  ▼
ServiceHub Web (Frontend)
  │
  ▼
API Gateway :8080
  │
  ▼
Eureka Service Discovery :8761
  │
  ├── User Service :8081
  ├── Request Service :8082
  └── Provider Service :8083
```

Configuration is provided through the **Config Server (`:8888`)**, while the **Eureka Server (`:8761`)** manages service registration and discovery.

---

## 🧩 Platform Components

### 1. Config Server

The Config Server provides centralized and externalized configuration management for all ServiceHub microservices.

**Default Port:** `8888`

Responsibilities:

* Centralized application configuration
* Externalized service configuration
* Consistent configuration across service instances
* Configuration management for distributed services

### 2. Eureka Server

The Eureka Server acts as the **service registry and discovery server**, enabling service discovery between the API Gateway and backend microservices.

**Default Port:** `8761`

Responsibilities:

* Service registration
* Service discovery
* Health/status monitoring
* Dynamic service instance discovery
* Supporting horizontal scaling of backend services

Expected registered services:

```text
API-GATEWAY
USER-SERVICE
REQUEST-SERVICE
PROVIDER-SERVICE
```

### 3. API Gateway

The API Gateway provides a single entry point for all backend API requests.

**Default Port:** `8080`

Responsibilities:

* Routing client requests to backend microservices
* Service discovery integration
* Centralized API entry point
* Supporting scalable service communication

Frontend requests are routed through the API Gateway instead of directly accessing individual microservices.

---

## 🔧 Microservices

| Service           | Port | Database | Responsibility                                          |
| ------------------ | ---: | -------- | -------------------------------------------------------- |
| User Service       | 8081 | MySQL    | User, customer, provider and administrator management    |
| Request Service    | 8082 | MongoDB  | Service request creation and management                  |
| Provider Service   | 8083 | MySQL    | Service provider information and service management      |

The backend services communicate through the Spring Cloud ecosystem and are registered with Eureka for service discovery.

---

## 📦 Repository Structure

ServiceHub follows the required **Polyrepo architecture with Git Submodules**.

```text
servicehub-platform/
│
├── backend/
│   ├── platform/
│   │   ├── config-server/       # Git Submodule
│   │   ├── eureka-server/       # Git Submodule
│   │   └── api-gateway/         # Git Submodule
│   │
│   └── services/
│       ├── user-service/        # Git Submodule
│       ├── request-service/     # Git Submodule
│       └── provider-service/    # Git Submodule
│
├── frontend/
│   └── servicehub-web/          # Git Submodule
│
├── .gitmodules
├── .gitignore
└── README.md
```

Each platform component and microservice is maintained in its own GitHub repository. The parent repository uses **Git Submodules** to organize the individual repositories as required by the project architecture.

The project consists of three main repositories:

1. **Backend – Platform Repository** (Config Server, Eureka Server, API Gateway)
2. **Backend – Services Repository** (User Service, Request Service, Provider Service)
3. **Frontend – Web Application Repository**

---

## 🛠️ Technology Stack

### Backend

* Java 25
* Spring Boot
* Spring Cloud
* Spring Cloud Config
* Spring Cloud Netflix Eureka
* Spring Cloud Gateway
* Spring Data
* Maven

### Databases

ServiceHub demonstrates both relational and non-relational database technologies as required by the project:

* **MySQL** – Relational database
* **MongoDB** – Non-relational database

### Frontend

* React
* TypeScript
* Vite
* Axios
* React Router
* Tailwind CSS

### Cloud & Infrastructure

* Google Cloud Platform (GCP)
* Compute Engine
* Managed Instance Groups
* Instance Templates
* Machine Images / Disk Images
* Health Checks
* Load Balancing
* Cloud SQL
* Firestore
* Cloud Storage
* Cloud NAT
* Cloud Router
* VPC Network
* Firewall Rules
* Cloud DNS
* Cloud Run
* Service Accounts
* Workload Identity Federation

### Process Management

* PM2

---

## ☁️ Cloud Deployment

ServiceHub is deployed on **Google Cloud Platform** following the required cloud deployment models.

### Backend – IaaS

The backend platform and microservices are deployed using an **Infrastructure as a Service (IaaS)** approach with Google Compute Engine virtual machines.

The deployment includes:

* Compute Engine VM instances
* Instance Templates
* Machine Images
* Managed Instance Groups
* Health Checks
* Load Balancing
* Multi-zone deployment
* Automatic horizontal scaling
* VPC networking, Firewall Rules, Cloud NAT, Cloud Router
* PM2 process management

### Frontend – PaaS / Serverless

The frontend web application is deployed using a **PaaS / Serverless** deployment model, and communicates with the deployed backend through the API Gateway.

### Cloud Storage

ServiceHub uses Google Cloud Storage for file/object storage as part of the cloud architecture.

---

## 🔄 High Availability & Auto Scaling

The ServiceHub deployment is designed to support cloud-native reliability and scalability. The backend infrastructure supports:

* Multiple VM instances
* Managed Instance Groups
* Automatic horizontal scaling
* Health checks
* Multi-zone deployment
* Load balancing
* Fault tolerance
* Service continuity

Backend services are not deployed as single fixed instances. The infrastructure is designed to allow additional instances to be created automatically according to the configured scaling requirements, allowing the platform to continue serving requests when individual service instances become unavailable.

---

## 🔁 Process Management with PM2

ServiceHub applications are managed using **PM2** on the deployed virtual machines. PM2 provides:

* Application process management
* Automatic application restart after failure
* Application logs
* Process monitoring
* Automatic startup after VM restart

The applications are not containerized and are managed as processes on the virtual machines.

---

## 🔐 Security

The cloud deployment uses Google Cloud networking and access-control mechanisms including:

* VPC Network
* Firewall Rules
* Service Accounts
* IAM
* Cloud NAT
* Private/internal communication where appropriate
* Restricted service access

Sensitive credentials and configuration values should not be committed to Git repositories.

---

## 🚀 Getting Started

### Prerequisites

Install the following software:

* JDK 25
* Maven
* Git
* Node.js
* npm
* MySQL
* MongoDB
* PM2

### Clone the Platform Repository

```bash
git clone https://github.com/yashodha-gunawardana/servicehub-platform.git
cd servicehub-platform
```

### Initialize Git Submodules

```bash
git submodule update --init --recursive
```

---

## 🔨 Build the Platform Components

### Config Server

```bash
cd config-server
./mvnw clean package -DskipTests
```

### Eureka Server

```bash
cd ../eureka-server
./mvnw clean package -DskipTests
```

### API Gateway

```bash
cd ../api-gateway
./mvnw clean package -DskipTests
```

---

## ▶️ Run the Platform

The recommended startup order is:

```text
1. Config Server
        ↓
2. Eureka Server
        ↓
3. API Gateway
        ↓
4. Backend Microservices (User, Request, Provider)
        ↓
5. Frontend Application
```

Start the Config Server first so that centralized configuration is available to the other components. Then start Eureka Server, followed by the API Gateway, backend microservices, and finally the frontend.

---

## 🔌 Default Ports

| Component         |   Port |
| ------------------ | -----: |
| Config Server      | `8888` |
| Eureka Server      | `8761` |
| API Gateway        | `8080` |
| User Service       | `8081` |
| Request Service    | `8082` |
| Provider Service   | `8083` |

---

## 🧪 Service Verification

The following endpoints can be used to verify the platform components during deployment:

| Component        | Endpoint                |
| ------------------ | ------------------------ |
| Config Server      | `http://<HOST>:8888`    |
| Eureka Server      | `http://<HOST>:8761`    |
| API Gateway        | `http://<HOST>:8080`    |

The Eureka dashboard should display registered ServiceHub services and their current status.

---

## 🔎 Service Discovery

Eureka Server maintains the service registry for ServiceHub.

```text
EUREKA-SERVER
      │
      ├── API-GATEWAY
      ├── USER-SERVICE
      ├── REQUEST-SERVICE
      └── PROVIDER-SERVICE
```

This allows services to communicate using service names instead of relying on fixed instance addresses, and lets the API Gateway route requests to the appropriate backend service.

---

## 📁 Git Submodule Commands

### Initialize Submodules

```bash
git submodule update --init --recursive
```

### Update Submodules

```bash
git submodule update --remote --merge
```

### Check Submodule Status

```bash
git submodule status
```

---

## 📚 ITS 2130 Requirements

The ServiceHub project follows the architecture and deployment requirements defined for the **ITS 2130 – Enterprise Cloud Architecture** final project.

Key requirements demonstrated by this project include:

* Microservice Architecture
* Java 25
* Spring Boot
* Spring Cloud
* Spring Data
* Relational Database
* Non-relational Database
* Config Server
* Eureka Service Registry
* API Gateway
* Polyrepo architecture
* Git Submodules
* GCP deployment
* IaaS backend deployment
* PaaS / Serverless frontend deployment
* Cloud Storage
* High Availability
* Automatic Scaling
* Multi-zone deployment
* Health Checks
* PM2 process management

The official guidelines require the project to be successfully deployed to GCP and emphasize cloud deployment as the primary focus of the evaluation. The required platform components are Config Server, Eureka Service Registry, and API Gateway. The repository structure follows the required polyrepo approach with Git Submodules.

---

## 📖 Related Repositories

### Platform Repository

Contains:

* Config Server
* Eureka Server
* API Gateway

### Services Repository

Contains:

* User Service
* Request Service
* Provider Service

### Frontend Repository

Contains the ServiceHub web application that consumes the backend APIs through the API Gateway.

---

## 🎯 Project Goal

The main goal of ServiceHub is to demonstrate how a real-world home service management application can be designed using **microservices and deployed as a cloud-native system**.

```text
Microservices
     +
Spring Cloud
     +
Service Discovery
     +
Centralized Configuration
     +
API Gateway
     +
Cloud Databases
     +
GCP Infrastructure
     +
High Availability
     +
Auto Scaling
     +
Process Recovery
```

---

