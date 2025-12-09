```mermaid
graph LR
    subgraph "Nœud A (i7) - Intelligence"
        Hive[🐝 TheHive]
        MISP[🐡 MISP]
        Cortex
        Elastic
    end

    subgraph "Nœud B (i5) - Détection & Simulation"
        subgraph "ELK Stack"
            LS[Logstash]
            ES[Elasticsearch]
            Kib[Kibana]
            Alert[ElastAlert 2]
        end

        subgraph "Zone Simulation (Docker)"
            Kali[🔴 Kali Container]
            Victim[💀 Ubuntu Victim + Filebeat]
        end

        OPN[🔥 VM OPNsense]
    end

    %% Flux Attaque
    Kali -- "1. Brute Force SSH" --> Victim

    %% Flux Logs
    Victim -- "2. Logs Auth (Port 5044)" --> LS
    LS -- "3. Logs Parsés (JSON)" --> ES
    ES <--> Kib

    %% Flux Alerting
    Alert -- "4. Requête (Query)" --> ES
    Alert -- "5. Alerte (API)" --> Hive
    Hive -.-> MISP
````
