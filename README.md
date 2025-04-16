# 8915_LabAssignment

> [!Note]
> CI/CD pipeline for each microservices pipeline is implemented.

## Deployment Files subfolder:

There are three subfolders, `Deployment`, `Service` and `Envs`, for `Envs`, it contains `ConfigMaps` and `Secrets`, since some of the service depend on secrets and some depend on configmaps.

In `Deployment` and `Service` folder, the files are named `{service_name}.yaml`, for example, `/Deployment/ai-service.yml` is the deployment file for ai service. In `Envs` folder, the files are named `{service_name}_{secret|configmap}.yaml`.

Also, in each folder, I have a `all-in-one-{deployment|svc|envs}.yaml`, if you prefer read all configs in one file.

## Updated Application Architecture:

<img width="715" alt="Screenshot 2025-04-08 at 7 06 26 PM" src="https://github.com/user-attachments/assets/267d33bf-2c70-4ff8-8f95-5392076774d5" />

## Application and Architecture Explanation:

The architecture above can be divided into three parts: front-end, back-end and managed services.
- Front-end:
  - Store-front, the web application that customers will use. It connects to order service and product service, which used to submit an order, and fetching products list from back-end services. Once an order is submitted, it will be sent as a message to Azure Service Bus. It is exposed to public internet by using kubernetes load balancer.
  - Store-admin, the web application that for only employees. It connects to product service, makeline service, and AI service. The product service is used to update current inventories based on given input, the ai service is used to generate description and images based on the given context, the makeline service is to manipulating the exist orders.
- Back-end:
  - product service: Service to provide CRUD operation for products in mongoDB, it connects to mongoDB by using connection string, and exposed to other internal services by Cluster IP.
  - order service: Service to send the order as a message to azure service bus.
  - makeline service: Service to consume the messages from azure service bus, also it provides function to let admin to do CRUD on exist orders.
  - ai service: Service that utilize OpenAI apis to generate text description or image on demand.
- managed services
  MongoDB, Azure Service bus and OpenAI APIs are all manages services, by using the provided SDK and connection credentials, the services will be able to communicate with them.


## Table of Microservice Repositories:
| Service              | Link                                               |
|----------------------|----------------------------------------------------|
| **Store-Front**      | https://github.com/Zhiyuanlu000217/store-front-25w |
| **Store-Admin**      | https://github.com/Zhiyuanlu000217/store-admin-25W |
| **Order-Service**    | https://github.com/Zhiyuanlu000217/order-service-25W |
| **Product-Service**  | https://github.com/Zhiyuanlu000217/product-service-25W |
| **Makeline-Service** | https://github.com/Zhiyuanlu000217/makeline-service-25W |
| **AI-Service**       | https://github.com/Zhiyuanlu000217/ai-service-25W |


## Table of Docker Images:
| Image Name | Docker Hub Link |
|------------|-----------------|
| store-admin | https://hub.docker.com/repository/docker/zeelu1/store-admin |
| store-front-25w | https://hub.docker.com/repository/docker/zeelu1/store-front-25w |
| makeline-service | https://hub.docker.com/repository/docker/zeelu1/makeline-service |
| order-service | https://hub.docker.com/repository/docker/zeelu1/order-service |
| ai-service | https://hub.docker.com/repository/docker/zeelu1/ai-service |
| product-service | https://hub.docker.com/repository/docker/zeelu1/product-service |

## Youtube link of Demo(roughly 3 minutes):
https://youtu.be/VxiiBoh8JFg

## Issues or limitations in the implementation 

1. The image built by Mac M3 pro has an architecture conflict with AKS instance. Resolved by specify building platforms
2. I won't use next.js + typescript next time, pass the npm build might take infinite times :(
