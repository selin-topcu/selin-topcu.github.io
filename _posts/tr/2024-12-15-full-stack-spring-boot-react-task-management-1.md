---
title: Full Stack Spring Boot React Microservices Görev Yönetimi - 1
date: 2024-12-15 13:14 +0300
categories: [Full Stack App, Full Stack Spring Boot React Microservices Task Management]
tags: [spring-boot, react, microservices, task-management, java, mysql, redux, tailwindcss, material-ui, full-stack, web-development, api, rest, deployment, docker, cloud, devops, frontend, backend, scalable, responsive, user-interface, full-stack-development, javascript, java-backend, react-frontend, create]
author: selin-topcu
lang: tr
---

# Create App

* [Spring initalizr](https://start.spring.io) generate app

![](/assets/img/2024-12-16/20241216_140114_image.png)

## Configuration application.yaml
* src/main/resources/application.properties refactor
  * application.properties > application.yaml

#### Add user service configuration to yaml file

```yaml

server:
  port: 5001
  
spring:
  application:
    name: USER-SERVICE
```
#### Add mysql configuration to yaml file

```yaml

  datasource:
    url: jdbc:mysql://localhost:3306/task_management_user_service
    username: root
    password: root
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    hibernate:
      ddl-auto: update
```

---
