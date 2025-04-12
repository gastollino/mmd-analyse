AiCUnet Architektur

![CodeRabbit Pull Request Reviews](https://img.shields.io/coderabbit/prs/github/gastollino/mmd-analyse?utm_source=oss&utm_medium=github&utm_campaign=gastollino%2Fmmd-analyse&labelColor=171717&color=FF570A&link=https%3A%2F%2Fcoderabbit.ai&label=CodeRabbit+Reviews)


Architecture Overview (Mermaid)

```mermaid


flowchart TB

%% FRONTEND UI
subgraph Frontend_UI
    A2[Component A2]
    A1[Component A1]
    Login[Login Page]
    Dashboard[Dashboard]
end

A2 --> Gateway
A1 --> Gateway
Login --> Gateway
Dashboard --> Gateway

%% API GATEWAY
Gateway[API Gateway - Routing & Auth]

%% SECURITY
subgraph Security
    AuthN[Authentication]
    AuthZ[Authorization]
    OAuth[OAuth2 Connect (optional)]
end

Gateway --> AuthN --> AuthZ
AuthN --> OAuth

%% MICROSERVICES
subgraph Microservices
    S1[Service 1 (DB1)]
    S2[Service 2 (DB2)]
    S3[Event Sender]
    S4[Logger]
    S5[Exec Service]
end

Gateway --> S1
Gateway --> S2
Gateway --> S3
Gateway --> S4
S1 --> DB1
S2 --> DB2
S3 --> DB3
S4 --> DBLogs

%% SERVICE 5
subgraph ExecLogic
    L1[Event Listener]
    L2[Validation Step]
    L3[Execute Action]
    L4[Send Error Event]
end

EventBus --> L1
L1 --> L2 --> L3 --> L4
L3 --> LogService
L4 --> DLQ

%% DATABASES
subgraph Databases
    DB1[(DB1)]
    DB2[(DB2)]
    DB3[(DB3)]
    DBLogs[(Log DB)]
    AuditDB[(Audit DB optional)]
    ReadReplicas[(Read DB optional)]
end

S3 --> AuditDB
S2 --> ReadReplicas

%% AUTOMATION
subgraph Automation
    EventBus[Event Bus]
    Workflow[Workflow Engine]
    EventStore[(Event Store optional)]
    SchemaReg[(Schema Registry optional)]
end

EventBus --> Workflow
DBLogs --> EventBus
S3 --> EventStore
S4 --> EventStore
S3 --> SchemaReg
S4 --> SchemaReg

%% SAGA PATTERN
subgraph Saga
    SagaStart[Saga Start]
    SagaStep1[Step 1 - Payment]
    SagaStep2[Step 2 - Shipping]
    SagaFail[Compensation Step]
end

SagaStart --> SagaStep1 --> SagaStep2
SagaStep1 -->|Fail| SagaFail

%% MONITORING
subgraph Monitoring
    LogService[Log Service]
    Alerts[Alert System]
    Tracing[Tracing (optional)]
    Health[Health Check]
end

S3 --> LogService
S4 --> LogService
SagaStep1 --> LogService
SagaStep2 --> LogService
LogService --> Alerts
S1 --> Tracing
S2 --> Tracing
S3 --> Tracing
S4 --> Tracing
Tracing --> Alerts
AllServices((All Services)) --> Health

%% ERROR HANDLING
subgraph Errors
    Retry[Retry]
    Timeout[Timeout]
    CBreaker[Circuit Breaker]
    DLQ[Dead Letter Queue]
end

SagaStep1 --> Retry --> Timeout --> CBreaker --> DLQ

%% CI/CD
subgraph CICD
    Pipeline[Test & Build]
    Staging[Staging]
    BlueGreen[Blue/Green Deploy]
end

Pipeline --> Staging --> BlueGreen

Description

This system uses a modular, event-driven microservice architecture.
It supports auth, automation, logging, monitoring, and is built for future scaling.
Optional components are clearly marked and isolated.


AI Architecture Review

If you are using CodeRabbit, you can enable architectural feedback by placing this in the .mmd file:

# Please analyze this architecture and suggest technical improvements

