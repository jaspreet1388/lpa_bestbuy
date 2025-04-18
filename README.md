# Best Buy Cloud-Native App – Full Stack Lab Project

## Overview

This project is a **cloud-native, microservices-based demo application** for Best Buy, inspired by the Algonquin Pet Store architecture with a key enhancement — replacing the RabbitMQ service with a **managed order queue** like **Azure Service Bus**.

As a Full-Stack Cloud-Native Developer at Best Buy, I designed, developed, and deployed this application on a **Kubernetes cluster**, integrating **AI-powered product descriptions and images** using **GPT-4** and **DALL·E**.

---

## Application Architecture

This architecture closely follows the Algonquin Pet Store pattern, with modifications to use managed cloud services for scalability and maintenance.

![image](https://github.com/user-attachments/assets/24e465c9-0cad-4666-9108-0142e845f81e)




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

All deployment manifests are located in the deployment folder.

---

## Microservice Repositories

| Service         | Repository Link                         |
|----------------|------------------------------------------|
| Store-Front     | [GitHub Link](https://github.com/jaspreet1388/lpa-store-front.git)       |
| Store-Admin     | [GitHub Link](https://github.com/jaspreet1388/lpa-store-admin.git)       |
| Order-Service   | [GitHub Link](https://github.com/jaspreet1388/lpa-order-service.git)     |
| Product-Service | [GitHub Link](https://github.com/jaspreet1388/lpa-product-service.git)   |
| Makeline-Service| [GitHub Link](https://github.com/jaspreet1388/lpa-makeline-service.git)  |
| AI-Service      | [GitHub Link](https://github.com/jaspreet1388/lpa-ai-service.git)        |

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
Connecting Order-Service to Azure Service Bus
az group create --name bestbuy --location canadacentral

az servicebus namespace create \
  --name bestbuyapp \
  --resource-group bestbuy

az servicebus queue create \
  --name orders \
  --namespace-name bestbuyapp \
  --resource-group bestbuy

 **Step 2: Set Up Authentication**
Option A: Using Managed Identity (Recommended)
SERVICEBUSBID=$(az servicebus namespace show \
  --name bestbuyapp \
  --resource-group bestbuy \
  --query id -o tsv)

az role assignment create \
  --assignee chha0038@algonquinlive.com \
  --role "Azure Service Bus Data Sender" \
  --scope $SERVICEBUSBID

**Step 3: Save Environment Variables**
Ensure you are inside the order-service directory.

HOSTNAME=$(az servicebus namespace show \
  --name bestbuyapp \
  --resource-group bestbuy \
  --query serviceBusEndpoint -o tsv | sed 's/https:\/\///;s/:443\///')

cat << EOF > .env
USE_WORKLOAD_IDENTITY_AUTH=true
AZURE_SERVICEBUS_FULLYQUALIFIEDNAMESPACE=$HOSTNAME
ORDER_QUEUE_NAME=orders
EOF

source .env

**Step 3: Save Environment Variables**
Ensure you are inside the order-service directory.

HOSTNAME=$(az servicebus namespace show \
  --name bestbuyapp \
  --resource-group bestbuy \
  --query serviceBusEndpoint -o tsv | sed 's/https:\/\///;s/:443\///')

cat << EOF > .env
USE_WORKLOAD_IDENTITY_AUTH=true
AZURE_SERVICEBUS_FULLYQUALIFIEDNAMESPACE=$HOSTNAME
ORDER_QUEUE_NAME=orders
EOF

source .env

---

## Demo Video

Watch the complete walkthrough of the application showcasing:

Working Store Front  
AI-generated Descriptions and Images   
Admin Management Console



---

## Deployment Instructions

1. **Clone the repositories**

   ```bash
   git clone https://github.com/your-username/store-front.git
   git clone https://github.com/your-username/store-admin.git
   ...
