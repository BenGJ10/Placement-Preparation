# System Design Roadmap

### ❗️Go to this repository for detailed explanations and resources: [System Design Principles](https://github.com/BenGJ10/System-Design-Principles)


This roadmap covers everything you need to study for **System Design Interviews (High-Level Design)**, organized in a structured order.

---

## 1. Foundations of System Design
- **What is System Design?**
- Functional vs. Non-Functional Requirements
- Latency vs. Throughput
- Horizontal vs. Vertical Scaling
- **Availability, Reliability, Fault Tolerance**
- **Consistency**
- **Maintainability**
- **CAP Theorem**
- **Concurrency vs. Parallelism**

---

## 2. Networking & Communication
- Basics of HTTP/HTTPS
- TCP/IP and UDP
- DNS (Domain Name System)
- CDN (Content Delivery Networks)
- API Communication
  - REST APIs
  - gRPC
  - WebSockets
- Reverse Proxy (Nginx, HAProxy)

---

## 3. Databases & Storage
- Relational Databases (SQL)
- Non-Relational Databases (NoSQL)
  - Key-Value Stores
  - Document Stores
  - Wide Column Stores
  - Graph Databases
- Indexing & Query Optimization
- **Database Scaling**
  - Replication (Master-Slave, Master-Master)
  - Sharding & Partitioning
- **Consistency Models**
  - Strong Consistency
  - Eventual Consistency
  - Causal Consistency
- Database Caching (Redis, Memcached)

---

## 4. Core Distributed Systems Concepts
- Distributed Systems Basics
- Leader Election (Zookeeper, etcd)
- Consensus Algorithms (Raft, Paxos)
- Quorum & Voting Mechanisms
- Eventual Consistency (Amazon Dynamo Model)
- Vector Clocks & Versioning

---

## 5. High Availability & Scalability
- **Load Balancing**
  - Layer 4 vs. Layer 7 Load Balancing
  - Load Balancing Algorithms (Round Robin, Least Connections, IP Hash)
- Auto Scaling & Elasticity
- Health Checks & Failover
- Multi-Region Deployment
- Redundancy & Removing Single Point of Failure (SPOF)

---

## 6. Performance Optimization
- **Caching**
  - CDN Caching
  - Application Caching (Redis, Memcached)
  - Cache Eviction Policies (LRU, LFU, FIFO)
- Rate Limiting & Throttling
- Connection Pooling
- Asynchronous Processing
  - Message Queues (RabbitMQ, SQS)
  - Publish-Subscribe (Kafka, Pub/Sub)
- Data Compression
- Query Optimization

---

## 7. Architecture Patterns
- **Monolith vs. Microservices**
- Service-Oriented Architecture (SOA)
- Event-Driven Architecture
- CQRS (Command Query Responsibility Segregation)
- Saga Pattern (Distributed Transactions)
- API Gateway Pattern
- Service Discovery (Consul, Eureka, Zookeeper)

---

## 8. Reliability & Fault Handling
- **Fault Tolerance**
- Circuit Breaker Pattern
- Retry Mechanisms
- Backpressure Handling
- Graceful Degradation
- Bulkheads & Isolation
- Disaster Recovery Strategies (RPO, RTO)

---

## 9. Monitoring, Logging & Maintainability
- Observability (Logs, Metrics, Traces)
- Monitoring Tools (Prometheus, Grafana, Datadog, CloudWatch)
- Logging Systems (ELK Stack, Splunk)
- Distributed Tracing (Jaeger, Zipkin)
- CI/CD Pipelines
- Deployment Strategies
  - Blue-Green Deployments
  - Canary Releases
  - Rolling Updates
- Rollback Mechanisms

---

## 10. Security & Compliance
- Authentication & Authorization
  - OAuth 2.0
  - JWT (JSON Web Tokens)
  - SSO (Single Sign-On)
- Data Encryption
  - SSL/TLS
  - Encryption at Rest vs. In Transit
- DDoS Mitigation
- Web Application Firewall (WAF)
- Secure API Design
- Compliance Standards (GDPR, HIPAA, PCI-DSS)

---

## 11. Advanced Topics
- Distributed File Systems (HDFS, Ceph)
- Object Storage (Amazon S3, Google Cloud Storage)
- Event Streaming (Kafka, Kinesis, Pulsar)
- Serverless Architectures (AWS Lambda, GCP Cloud Functions)
- Containerization & Orchestration
  - Docker
  - Kubernetes
- Edge Computing
- Hybrid & Multi-Cloud Architectures

---

## 12. Case Studies & System Design Problems
- **Beginner-Friendly**
  - URL Shortener (TinyURL)
  - Pastebin
  - Rate Limiter
- **Intermediate**
  - Chat Application (WhatsApp, Messenger)
  - News Feed System (Facebook, Twitter)
  - Distributed Caching System
- **Advanced**
  - YouTube / Netflix (Video Streaming)
  - Uber / Lyft (Ride-Sharing)
  - Amazon / Flipkart (E-commerce)
  - Google Drive / Dropbox (File Storage)
  - Payment Gateway (Stripe, PayPal)

---

## Key Resources
- **Books**
  - Designing Data-Intensive Applications – Martin Kleppmann
  - System Design Interview Vol 1 & 2 – Alex Xu
- **Websites**
  - System Design Primer (GitHub)
  - High Scalability Blog
- **Practice**
  - LeetCode System Design
  - Grokking the System Design Interview (Educative)

---



# Low-Level Design (LLD) Roadmap

This roadmap covers everything you need to study for **Low-Level Design Interviews**, with a focus on **OOP, principles, design patterns, and practice problems**.

---

## 1. Foundations of LLD
- **Object-Oriented Programming (OOP)**
  - Classes & Objects
  - Encapsulation
  - Abstraction
  - Inheritance
  - Polymorphism
- Interfaces & Abstract Classes
- Static vs. Dynamic Binding
- Composition vs. Inheritance
- Access Modifiers & Visibility

---

## 2. Design Principles
- **SOLID Principles**
  - Single Responsibility Principle (SRP)
  - Open/Closed Principle (OCP)
  - Liskov Substitution Principle (LSP)
  - Interface Segregation Principle (ISP)
  - Dependency Inversion Principle (DIP)
- DRY (Don’t Repeat Yourself)
- KISS (Keep It Simple, Stupid)
- YAGNI (You Aren’t Gonna Need It)
- Law of Demeter (Principle of Least Knowledge)

---

## 3. UML & Diagrams
- **Class Diagrams**
- Sequence Diagrams
- Activity Diagrams
- State Diagrams
- Use Case Diagrams
- Component Diagrams
- Deployment Diagrams

---

## 4. Design Patterns
### Creational Patterns
- Singleton
- Factory Method
- Abstract Factory
- Builder
- Prototype

### Structural Patterns
- Adapter
- Bridge
- Composite
- Decorator
- Facade
- Flyweight
- Proxy

### Behavioral Patterns
- Strategy
- Observer
- Command
- Chain of Responsibility
- Iterator
- Mediator
- Memento
- State
- Template Method
- Visitor

---

## 5. Concurrency in LLD
- Threads & Synchronization
- Locks, Semaphores, Monitors
- Producer-Consumer Problem
- Reader-Writer Problem
- Deadlock & Livelock
- Thread-safe Data Structures
- Executors & Thread Pools

---

## 6. Common LLD Problems (Practice)
- **Beginner**
  - Design a Parking Lot
  - Design a Library Management System
  - Design a Tic-Tac-Toe Game
  - Design a Stack/Queue
- **Intermediate**
  - Design a Snake & Ladder Game
  - Design a Splitwise System
  - Design a Vending Machine
  - Design a Movie Ticket Booking System
  - Design a Logger System
- **Advanced**
  - Design an Elevator System
  - Design a Chess Game
  - Design an Online Food Delivery System (Zomato/Swiggy)
  - Design a Ride-Sharing System (Uber/Ola)
  - Design a Payment Wallet (Paytm)

---

## 7. Testing & Best Practices
- Unit Testing
- Test-Driven Development (TDD)
- Mocking & Stubs
- Code Reviews & Clean Code
- Refactoring Techniques

---

## Key Resources
- **Books**
  - Head First Design Patterns – Eric Freeman
  - Design Patterns: Elements of Reusable Object-Oriented Software – GoF
  - Clean Code – Robert C. Martin
- **Websites**
  - Refactoring Guru (design patterns explained visually)
  - GeeksforGeeks LLD Section
  - Design Gurus (LLD practice problems)
- **Courses**
  - Grokking the Object-Oriented Design Interview (Educative)

---
