AiCUnet Architektur

![CodeRabbit Pull Request Reviews](https://img.shields.io/coderabbit/prs/github/gastollino/mmd-analyse?utm_source=oss&utm_medium=github&utm_campaign=gastollino%2Fmmd-analyse&labelColor=171717&color=FF570A&link=https%3A%2F%2Fcoderabbit.ai&label=CodeRabbit+Reviews)


Architecture Overview (Mermaid)

```mermaid


flowchart TB
    %% FRONTEND UI
    subgraph Frontend_Web_UI["Frontend Web UI"]
        A2["Komponente A2"]
        A1["Komponente A1"]
        Login["Komponente Login"]
        Dashboard["Komponente Dashboard"]
    end

    A2 --> Gateway
    A1 --> Gateway
    Login --> Gateway
    Dashboard --> Gateway

    %% API GATEWAY
    Gateway["API Gateway\nAuth, Routing, Monitoring"]

    %% SECURITY
    subgraph Security
        AuthN["Authentication Layer"]
        AuthZ["Authorization Layer"]
    end
    Gateway --> AuthN --> AuthZ

    %% MICROSERVICES
    subgraph Microservices
        S1["Service 1"]
        S2["Service 2"]
        S3["Service 3"]
        S4["Service 4"]
        S5["Service 5"]
    end

    Gateway --> S1
    Gateway --> S2
    Gateway --> S3
    Gateway --> S4

    %% DATABASES
    subgraph Datenbanken
        DB1[("DB 1")]
        DB2[("DB 2")]
        DB3[("DB 3")]
        DBLogs[("DB Logs")]
    end

    S1 --> DB1
    S2 --> DB2
    S3 --> DB3
    S4 --> DBLogs

    %% EVENT SYSTEM
    subgraph Automatisierung
        EventBus["Event Bus"]
        Workflow["Workflow Engine"]
    end

    S3 --> EventBus
    S4 --> EventBus
    EventBus --> Workflow
    DBLogs --> EventBus

    %% SAGA PATTERN
    subgraph SagaPattern["Saga Coordinator"]
        SagaStart["Start Saga"]
        Step1["Step 1"]
        Step2["Step 2"]
        Kompensation["Kompensation"]
    end

    SagaStart --> Step1 --> Step2
    Step1 --> Kompensation

    %% MONITORING
    subgraph Monitoring
        LogService["Logging Service"]
        AlertSystem["Alert System"]
    end

    S3 --> LogService
    S4 --> LogService
    LogService --> AlertSystem

    %% ERROR HANDLING
    subgraph Fehlerbehandlung
        Retry["Retry"]
        Timeout["Timeout"]
        CBreaker["Circuit Breaker"]
        DLQ["Dead Letter Queue"]
    end

    Step1 --> Retry --> Timeout --> CBreaker --> DLQ

Description

This system uses a modular, event-driven microservice architecture.
It supports auth, automation, logging, monitoring, and is built for future scaling.
Optional components are clearly marked and isolated.


AI Architecture Review

If you are using CodeRabbit, you can enable architectural feedback by placing this in the .mmd file:

# Please analyze this architecture and suggest technical improvements

