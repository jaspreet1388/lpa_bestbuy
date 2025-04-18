# Best Buy Cloud-Native App – Full Stack Lab Project

## Overview

This project is a **cloud-native, microservices-based demo application** for Best Buy, inspired by the **Algonquin Pet Store (On Steroids)** architecture with a key enhancement — replacing the RabbitMQ service with a **managed order queue** like **Azure Service Bus**.

As a Full-Stack Cloud-Native Developer at Best Buy, I designed, developed, and deployed this application on a **Kubernetes cluster**, integrating **AI-powered product descriptions and images** using **GPT-4** and **DALL·E**.

---

## Application Architecture

This architecture closely follows the Algonquin Pet Store pattern, with modifications to use managed cloud services for scalability and maintenance.

![Best Buy Architecture Diagram](./architecture-diagram.png)

> ✅ *Note: Replace the above link with your actual Draw.io exported image.*

---

## Microservices Overview

| Service           | Description                                                       | Notes                                       |
|------------------|-------------------------------------------------------------------|---------------------------------------------|
| Store-Front       | Customer-facing app for browsing and placing orders              | React/Angular/Vue frontend                  |
| Store-Admin       | Employee-facing admin interface to manage products and orders     | Internal frontend app                       |
| Order-Service     | Accepts orders and pushes them to the Azure Service Bus queue     | Replaces RabbitMQ                           |
| Product-Service   | Handles product CRUD operations                                    | Connects to MongoDB                         |
| Makeline-Service  | Consumes order messages from the queue and fulfills them          | Uses Azure SDK to consume messages          |
| AI-Service        | Generates AI-powered product descriptions and images              | Uses GPT-4 and DALL·E via OpenAI APIs       |
| MongoDB           | Stores persistent data for products and orders                    | Deployed as a StatefulSet in Kubernetes     |

---

## AI Integration

The **AI-Service** leverages the following:

- **GPT-4** for generating engaging product descriptions
- **DALL·E** for generating product images
- Deployed via **Azure OpenAI** or **OpenAI APIs**
- All prompts and responses are handled securely with secrets in Kubernetes

---

## Kubernetes Deployment

The entire application is containerized and deployed using Kubernetes:

- **ConfigMaps** for application configurations
- **Secrets** for API keys, credentials
- **StatefulSets** for MongoDB
- **Deployments** for stateless services
- **Service Accounts + RBAC** for secure access

All deployment manifests are located in the [`Deployment Files`](./Deployment%20Files) folder.

---

## Microservice Repositories

| Service         | Repository Link                         |
|----------------|------------------------------------------|
| Store-Front     | [GitHub Link](https://github.com/your-username/store-front)       |
| Store-Admin     | [GitHub Link](https://github.com/your-username/store-admin)       |
| Order-Service   | [GitHub Link](https://github.com/your-username/order-service)     |
| Product-Service | [GitHub Link](https://github.com/your-username/product-service)   |
| Makeline-Service| [GitHub Link](https://github.com/your-username/makeline-service)  |
| AI-Service      | [GitHub Link](https://github.com/your-username/ai-service)        |

---

## Docker Images

| Service         | Docker Image Link                                      |
|----------------|--------------------------------------------------------|
| Store-Front     | [Docker Hub](https://hub.docker.com/r/yourname/store-front)     |
| Store-Admin     | [Docker Hub](https://hub.docker.com/r/yourname/store-admin)     |
| Order-Service   | [Docker Hub](https://hub.docker.com/r/yourname/order-service)   |
| Product-Service | [Docker Hub](https://hub.docker.com/r/yourname/product-service) |
| Makeline-Service| [Docker Hub](https://hub.docker.com/r/yourname/makeline-service)|
| AI-Service      | [Docker Hub](https://hub.docker.com/r/yourname/ai-service)      |

---

## Demo Video

Watch the complete walkthrough of the application showcasing:

✅ Working Store Front  
✅ AI-generated Descriptions and Images  
✅ Integration with Azure Service Bus  
✅ Admin Management Console

🔗 [Watch Demo on YouTube](https://www.youtube.com/watch?v=your-demo-link)

---

## Deployment Instructions

1. **Clone the repositories**

   ```bash
   git clone https://github.com/your-username/store-front.git
   git clone https://github.com/your-username/store-admin.git
   ...
