# User Management Microservices Platform

Платформа управления пользователями на основе микросервисной архитектуры с использованием Spring Cloud, Apache Kafka и Docker.

[![Java](https://img.shields.io/badge/Java-17-orange.svg)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.4.0-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Spring Cloud](https://img.shields.io/badge/Spring%20Cloud-2024.0.0-blue.svg)](https://spring.io/projects/spring-cloud)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## 📋 Описание

Демонстрационный проект, реализующий микросервисную архитектуру для системы управления пользователями с асинхронной обработкой событий. Проект демонстрирует применение современных паттернов проектирования, технологий Spring Cloud и event-driven архитектуры.

## 🏗️ Архитектура

Проект состоит из следующих микросервисов:

### Инфраструктурные сервисы

- **Discovery Service** (Eureka Server) — service registry для динамического обнаружения сервисов
- **Config Service** (Spring Cloud Config) — централизованное управление конфигурацией
- **Gateway Service** (Spring Cloud Gateway) — API Gateway, единая точка входа

### Бизнес-сервисы

- **User Service** — управление пользователями (CRUD операции)
- **Notification Service** — асинхронная отправка уведомлений по email

### Общий модуль

- **Common** — shared модуль с общими DTO, utilities и конфигурациями

## 🛠️ Технологический стек

### Backend
- **Java 17** — язык программирования
- **Spring Boot 3.4.0** — фреймворк для создания микросервисов
- **Spring Cloud 2024.0.0** — набор инструментов для микросервисной архитектуры
  - Spring Cloud Netflix Eureka — service discovery
  - Spring Cloud Config — configuration management
  - Spring Cloud Gateway — API gateway

### Messaging & Integration
- **Apache Kafka 7.6.0** — message broker для асинхронного взаимодействия
- **Zookeeper 7.6.0** — координация для Kafka

### Data & Persistence
- **PostgreSQL** — реляционная база данных (опционально: укажите если используете)
- **Hibernate** — ORM framework

### DevOps & Tools
- **Docker & Docker Compose** — контейнеризация и оркестрация
- **Maven** — система сборки и управление зависимостями
- **GreenMail** — тестовый SMTP-сервер для разработки

## 🚀 Быстрый старт

### Предварительные требования

- Java 17+
- Docker & Docker Compose
- Maven 3.6+

### Запуск инфраструктуры

1. Клонируйте репозиторий: git clone https://github.com/Lanctole/user-management-microservices.git
cd user-management-microservices
2. Запустите инфраструктурные сервисы (Kafka, Zookeeper, GreenMail): docker-compose up -d
3. Настройте конфигурацию базы данных:
Скопируйте example файл и укажите свои настройки
cp src/main/resources/hibernate.example.properties src/main/resources/hibernate.properties
Отредактируйте hibernate.properties

### Запуск микросервисов

Запускайте сервисы в следующем порядке:
1. Discovery Service (порт 8761)
cd discovery-service
mvn spring-boot:run

2. Config Service (порт 8888)
cd ../config-service
mvn spring-boot:run

3. Gateway Service (порт 8080)
cd ../gateway-service
mvn spring-boot:run

4. User Service (порт 8081)
cd ../user-service
mvn spring-boot:run

5. Notification Service (порт 8082)
cd ../notification-service
mvn spring-boot:run

### Проверка работоспособности

- **Eureka Dashboard**: http://localhost:8761
- **API Gateway**: http://localhost:8080
- **GreenMail Web UI**: http://localhost:9090

## 📚 API Documentation

API документация доступна через Swagger UI: http://localhost:8080/swagger-ui.html

### Основные эндпоинты

| Метод | Endpoint | Описание |
|-------|----------|----------|
| GET | `/api/users` | Получить список пользователей |
| GET | `/api/users/{id}` | Получить пользователя по ID |
| POST | `/api/users` | Создать нового пользователя |
| PUT | `/api/users/{id}` | Обновить пользователя |
| DELETE | `/api/users/{id}` | Удалить пользователя |

## 🔄 Event-Driven Architecture

Проект использует асинхронную обработку событий через Apache Kafka:

### Kafka Topics

- `user.created` — событие создания пользователя
- `user.updated` — событие обновления пользователя
- `user.deleted` — событие удаления пользователя
- `notification.email` — очередь email-уведомлений

### Поток данных

User Service → Kafka Topic → Notification Service → Email (GreenMail)
