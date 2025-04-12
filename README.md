AiCUnet Architektur

![CodeRabbit Pull Request Reviews](https://img.shields.io/coderabbit/prs/github/gastollino/mmd-analyse?utm_source=oss&utm_medium=github&utm_campaign=gastollino%2Fmmd-analyse&labelColor=171717&color=FF570A&link=https%3A%2F%2Fcoderabbit.ai&label=CodeRabbit+Reviews)


Architecture Overview (Mermaid)

```mermaid


flowchart TB
    %% ========= FRONTEND UI =========
    subgraph Frontend_Web_UI ["Frontend Web UI"]
        A2["Komponente A2"]
        A1["Komponente A1"]
        Login["Komponente Login"]
        Dashboard["Komponente Dashboard"]
    end

    A2 -->|"HTTP Request"| Gateway
    A1 -->|"GraphQL"| Gateway
    Login -->|"Auth Request"| Gateway
    Dashboard -->|"Data Fetch"| Gateway

    %% ========= API GATEWAY =========
    Gateway["API Gateway\n• Authentication\n• Rate Limiting\n• Request Routing\n• Monitoring"]

    %% ========= SECURITY LAYER =========
    subgraph Security ["Security Layer"]
        AuthN["Auth Service\n(JWT, OAuth2)"]
        AuthZ["Policy Service\n(RBAC, ABAC)"]
        Audit["Audit Logger"]
    end
    Gateway --> AuthN --> AuthZ --> Audit

    %% ========= CORE MICROSERVICES =========
    subgraph Microservices ["Business Microservices"]
        Order["Order Service\n• Creates orders\n• Manages lifecycle"]
        Inventory["Inventory Service\n• Stock management\n• Reservations"]
        Payment["Payment Service\n• Processes payments\n• Refunds"]
        Notification["Notification Service\n• Email/SMS\n• Event-driven"]
    end

    Gateway --> Order --> EventBus
    Gateway --> Inventory
    Gateway --> Payment
    Notification -->|"Sends"| EmailService["Email Service"]
    Notification -->|"Pushes"| MobileGateway["Mobile Gateway"]

    %% ========= DATA STORES =========
    subgraph Databases ["Persistence Layer"]
        OrderDB[("Orders DB\n(PostgreSQL)")]
        InventoryDB[("Inventory DB\n(MongoDB)")]
        PaymentDB[("Payments DB\n(PostgreSQL)")]
        EventStore[("Event Store\n(Kafka)")]
    end

    Order --> OrderDB
    Inventory --> InventoryDB
    Payment --> PaymentDB
    EventBus --> EventStore

    %% ========= EVENT ARCHITECTURE =========
    subgraph Event_System ["Event-Driven Architecture"]
        EventBus["Event Bus\n(Kafka Topics)"]
        CQRS["CQRS Processor"]
        Saga["Saga Orchestrator"]
    end

    Order -->|"Publishes"| EventBus
    Inventory -->|"Subscribes"| EventBus
    Payment -->|"Subscribes"| EventBus
    EventBus --> CQRS --> ReadDB[("Read DB\n(Elasticsearch)")]
    EventBus --> Saga

    %% ========= OBSERVABILITY =========
    subgraph Observability ["Monitoring & Tracing"]
        Metrics["Prometheus\n+Grafana"]
        Logging["ELK Stack"]
        Tracing["Jaeger"]
        Health["Health Checks"]
    end

    Order -->|"Metrics"| Metrics
    Inventory -->|"Logs"| Logging
    Payment -->|"Traces"| Tracing
    Health -->|"Pings"| AllServices

    %% ========= DEPLOYMENT =========
    subgraph Deployment ["CI/CD Pipeline"]
        Build["Build\n(Docker Images)"]
        Test["Test\n(JUnit, Cypress)"]
        Deploy["Deploy\n(Kubernetes)"]
    end

    Build --> Test --> Deploy

    %% ========= LEGEND & NOTES =========
    classDef service fill:#e3f2fd,stroke:#64b5f6
    classDef storage fill:#e8f5e9,stroke:#81c784
    classDef event fill:#f3e5f5,stroke:#ba68c8
    classDef infra fill:#fff3e0,stroke:#ffb74d

    class Order,Inventory,Payment,Notification service
    class OrderDB,InventoryDB,PaymentDB,EventStore,ReadDB storage
    class EventBus,CQRS,Saga event
    class Observability,Deployment infra

    click Gateway "https://docs.aicunet.com/api-gateway" _blank
    click AuthN "https://docs.aicunet.com/auth" _blank

Description

This system uses a modular, event-driven microservice architecture.
It supports auth, automation, logging, monitoring, and is built for future scaling.
Optional components are clearly marked and isolated.


AI Architecture Review

If you are using CodeRabbit, you can enable architectural feedback by placing this in the .mmd file:

# Please analyze this architecture and suggest technical improvements

