# run-aspnetcore-microservices

A complete microservices solution built with **ASP.NET Core** and modern cloud‑native technologies.

This repository demonstrates a production‑style architecture using:

- ASP.NET Core 8
- Docker & Docker Compose
- Microservices
- MongoDB
- Redis
- SQL Server
- Ocelot API Gateway
- RabbitMQ
- Autofac
- MediatR (CQRS + Pipeline Behaviours)
- AutoMapper
- Event‑Driven Architecture

> ⚠️ **This repository is under active development.**  
> The migration to .NET 8 is complete, but additional improvements are ongoing.

---

## 🏗️ Architecture Overview

The solution implements a distributed microservices architecture with:

- **API Gateway (Ocelot)**
- **Independent services** (Catalog, Basket, Ordering)
- **Asynchronous communication** via RabbitMQ
- **Multiple persistence stores** (MongoDB, Redis, SQL Server)
- **CQRS + MediatR** for clean separation of concerns
- **Containerized deployment** using Docker

![aspnetrun-microservices](https://user-images.githubusercontent.com/1147445/79753821-34b93800-831f-11ea-86fc-617654557084.png)

---

## 🚀 Running the Services (Docker)

Once the environment is started with `docker-compose`, the services are available at:

- **Catalog.API**  
  http://host.docker.internal:8000/swagger/index.html

- **Basket.API**  
  http://host.docker.internal:8001/swagger/index.html

- **Ordering.API**  
  http://host.docker.internal:8002/swagger/index.html

- **RabbitMQ Management UI**  
  http://localhost:15672/

---

## 🔧 Migration to .NET 8 — Current Status

The entire solution has been successfully migrated to **.NET 8**, including:

### ✔ Ordering.API
- Updated to .NET 8
- MediatR configured using the classic `AddMediatR` (v11.1.0)
- RabbitMQ.Client upgraded to **6.8.1**
- Updated consumer to handle `ReadOnlyMemory<byte>` from RabbitMQ 6.x
- Startup and DI pipeline fully modernized

### ✔ Ordering.Application
- ValidationBehaviour updated to the correct MediatR signature
- AutoMapper 12.0.1 retained (see security note below)

### ✔ EventBusRabbitMQ
- Updated to .NET 8
- RabbitMQ.Client aligned to **6.8.1**
- Fully compatible with Ordering.API

### ✔ Other Services
- Catalog.API, Basket.API, APIGateway, and WebApp all compile and run on .NET 8
- Dockerfiles updated to .NET 8 base images

---

## ⚠️ AutoMapper Security Warning (NU1903)

You may see this warning during build:


Important notes:

- **All AutoMapper versions (12, 13, 14) currently have the same advisory.**
- There is **no patched version available** at this time.
- The vulnerability does **not apply** to this solution, because:
  - AutoMapper is only used for internal DTO mapping
  - No dynamic mapping or external JSON mapping is used
  - No untrusted input is passed into AutoMapper

- Future Migration will use MAPSTER instead of AutoMapper