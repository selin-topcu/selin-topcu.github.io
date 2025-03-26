---
title: Spring Boot Microservice Fatura Uygulaması
date: 2025-03-26 17:10 +0300
categories: [Backend, Spring Boot Microservice Fatura Uygulaması]
tags: [
  spring-boot, java, postgresql, api, rest-api, deployment, backend, java-backend, 
  microservice, feign-client, jpa, flyway, elk-stack, logging, monitoring, prometheus,
  grafana, docker, containerization, swagger, openapi, kafka, event-driven, caching, 
  caffeine-cache, circuit-breaker, resilience4j, bulkhead, retry, spring-retry, 
  distributed-tracing, micrometer, fault-tolerance
]
author: selin-topcu
lang: tr
---


### Uygulama Detayları 

🔗 [Github Repo](https://github.com/selin-topcu/Spring-Boot-Microservice-Invoice-Project)

🚀 **Framework**  
   - Spring Boot  

🔌 **API'ler**  
   - Spring Boot  

🌍 **Mimari**  
   - Mikroservis  

🖧 **API Çağrıları**  
   - Open Feign Client  

💾 **Veritabanı Entegrasyonu**  
   - JPA  

🔄 **Veritabanı Göçleri (Migration)**  
   - Flyway  

📊 **Loglama ve Merkezi Loglama Çözümü**  
   - ELK Stack  

⚠️ **Hata Yönetimi**  
   - Spring Boot  

🕵️‍♂️ **Dağıtık İzleme ve Gözlemleme (Tracing & Monitoring)**  
   - Micrometer  

📈 **Uygulama İzleme (Monitoring)**  
   - Spring Boot, Prometheus, Grafana  

🐳 **Konteynerleştirme (Containerization)**  
   - Docker, Dockerfiles  

📜 **API Belgelendirme**  
   - OpenAPI (Swagger)  

🎉 **Olay Tabanlı Yapı (Event Driven)**  
   - Kafka  

🧠 **Önbellekleme (Caching)**  
   - Spring Boot Caching  
   - Caffeine Caching  

🛡️ **Dayanıklılık ve Güvenilirlik (Reliability)**  
   - API Gateway'de Bulkhead ve Circuit Breaker  
   - Open Feign'de Retry  
   - Kafka Template Send'de Spring Retry  


🌐 **Source**  
   - [Github Repo](https://github.com/selin-topcu/Spring-Boot-Microservice-Invoice-Project)
   - [Spring Initializr](https://start.spring.io/)
   - [JWT](https://jwt.io/)
   - [Elastic](https://www.elastic.co/guide/en/apm/agent/java/current/setup-attach-api.html)
   - [Grafana](https://grafana.com/grafana/dashboards/19004-spring-boot-statistics/)
   - [Docker Kafka Configuration](https://github.com/conduktor/kafka-stack-docker-compose/blob/master/zk-single-kafka-multiple.yml)
   - [Kafdrop](https://github.com/obsidiandynamics/kafdrop/blob/master/docker-compose/kafka-kafdrop/docker-compose.yaml)
   - [Microservice Project](https://www.udemy.com/course/master-building-enterprise-microservices-in-depth-project/)

📂 **Postman Collection**  
   - [demo-microservice.postman_collection.json](https://github.com/selin-topcu/Spring-Boot-Microservice-Invoice-Project/blob/main/demo-microservice.postman_collection.json)

🐳 **Docker Start**  
   - `docker-compose up -d`
   - `docker-compose -f docker-compose.yml -f extensions/fleet/fleet-compose.yml -f extensions/fleet/agent-apmserver-compose.yml up -d`

### Project Structure 
![demo-project](https://github.com/user-attachments/assets/31003a60-b7bb-40d2-a2cc-951b08e5e0eb)

### Keycloak
![image](https://github.com/user-attachments/assets/7c2e9e8d-c763-4c08-afcf-2bd783d2ca57)

### Elastic
![image](https://github.com/user-attachments/assets/72e6ebd4-98bf-4150-8547-4a09f4bea477)

### Prometheus
![image](https://github.com/user-attachments/assets/85907d2e-a2c6-4a25-90cb-18c1b0a75235)

### Grafana
![image](https://github.com/user-attachments/assets/c50a23e7-7474-4084-9844-1793d5033ab9)

### Open Api Swagger
![image](https://github.com/user-attachments/assets/4f81e873-bde7-4a0d-b4fd-3525f0c66208)

### Kafdrop
![image](https://github.com/user-attachments/assets/80a6f8c5-e9d1-4296-a860-63a54cc7e294)