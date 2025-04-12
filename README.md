AiCUnet Architektur

![CodeRabbit Pull Request Reviews](https://img.shields.io/coderabbit/prs/github/gastollino/mmd-analyse?utm_source=oss&utm_medium=github&utm_campaign=gastollino%2Fmmd-analyse&labelColor=171717&color=FF570A&link=https%3A%2F%2Fcoderabbit.ai&label=CodeRabbit+Reviews)


Architecture Overview (Mermaid)

```mermaid


flowchart TB
    %% ========= FRONTEND UI =========
    subgraph Frontend_Web_UI[Frontend Web UI]
        A2[Komponente A2]
        A1[Komponente A1]
        Login[Komponente Login]
        Dashboard[Komponente Dashboard]
    end

    A2 --> Gateway
    A1 --> Gateway
    Login --> Gateway
    Dashboard --> Gateway

    %% ========= GATEWAY =========
    Gateway[API Gateway\nAuth, Routing, Monitoring]

    %% ========= SECURITY =========
    subgraph Security
        AuthN[Authentication Layer]
        AuthZ[Authorization Layer]
    end
    Gateway --> AuthN --> AuthZ

    %% ========= MICROSERVICES =========
    subgraph Microservices
        S1[Service liest/schreibt DB1]
        S2[Service liest/schreibt DB2]
        S3[Service sendet Event]
        S4[Service loggt Daten / sendet Event]
        S5[Service unten erklärt - siehe Service5Logic]
    end

    Gateway --> S1
    Gateway --> S2
    Gateway --> S3
    Gateway --> S4
    S1 --> DB1
    S2 --> DB2
    S3 --> DB3
    S4 --> DBLogs

    %% ========= SERVICE 5 DETAIL =========
    subgraph Service5Logic[Service 5: Exec Unit]
        L1[Event Listener: hört auf DokumentGeprueft]
        L2[Validiert Daten lokal]
        L3[Fuehrt Aktion aus - z.B. Versand starten]
        L4[Bei Fehler: sendet Event AktionFehlgeschlagen]
    end
    EventBus --> L1
    L1 --> L2 --> L3 --> L4
    L3 --> LogService
    L4 --> DLQ

    %% ========= DATENBANKEN =========
    subgraph Datenbanken
        DB1[(DB 1)]
        DB2[(DB 2)]
        DB3[(DB 3)]
        DBLogs[(DB Logs)]
    end

    %% ========= AUTOMATISIERUNG =========
    subgraph Automatisierung
        EventBus[Event Bus]
        Workflow[Workflow Engine]
    end

    EventBus --> Workflow
    DBLogs --> EventBus

    %% ========= SAGA PATTERN =========
    subgraph SagaPattern[Saga Muster Coordinator]
        SagaStart["Start Saga (z.B. Bestellung)"]
        SagaStep1[1: Zahlung verarbeiten]
        SagaStep2[2: Versand starten]
        SagaFail[Kompensationslogik bei Fehler]
    end

    SagaStart --> SagaStep1 --> SagaStep2
    SagaStep1 --> SagaFail

    %% ========= MONITORING =========
    subgraph Monitoring
        LogService["Logging Service (z.B. ELK)"]
        Alerts["Alerting (z.B. Prometheus)"]
    end
    S3 --> LogService
    S4 --> LogService
    SagaStep1 --> LogService
    SagaStep2 --> LogService
    LogService --> Alerts

    %% ========= FEHLERBEHANDLUNG =========
    subgraph Fehlerbehandlung
        Retry[Retry Mechanismus]
        Timeout[Timeout Logik]
        CBreaker[Circuit Breaker]
        DLQ[Dead Letter Queue]
    end
SagaStep1 --> Retry --> Timeout --> CBreaker --> DLQ

%% AI Architecture Review 
%% This system uses a modular, event-driven microservice architecture
%% It supports auth, automation, logging, monitoring, and is built for future scaling
%% Optional components are clearly marked and isolated

AI Architecture Review

If you are using CodeRabbit, you can enable architectural feedback by placing this in the .mmd file:

# Please analyze this architecture and suggest technical improvements

