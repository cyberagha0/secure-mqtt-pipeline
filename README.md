# Secure MQTT Pipeline: IoT Cybersecurity Project

## Overview

This repository documents my **8-project IoT Cybersecurity Externship with Hydroficient through Extern**.

The project follows the security lifecycle of an IoT water-management system, beginning with architecture and threat modeling and progressing into Python security automation, MQTT testing, encryption, device identity, replay-attack prevention, real-time monitoring, and anomaly detection.

Rather than treating each security concept independently, the projects build on one another to demonstrate how an insecure IoT communication pipeline can be **analyzed, attacked in a controlled environment, hardened, and monitored**.

> **Note:** The Grand Marina environment used throughout this repository is a simulated project scenario. All security testing documented here was performed as part of the externship learning environment.

---

## Project Scenario

The simulated environment represents an IoT water-management deployment at **The Grand Marina Hotel**.

HYDROLOGIC IoT devices collect operational telemetry such as water pressure, flow rate, and gate position and communicate with cloud services using **MQTT**.

Because the system can also receive commands that affect physical equipment, a cybersecurity compromise could have consequences beyond data loss.

### Simplified Architecture

```text
Sensors / IoT Devices
        │
        │ Telemetry
        ▼
   MQTT Broker
        │
        ▼
 Cloud / Dashboard
        │
        │ Commands
        ▼
   MQTT Broker
        │
        ▼
   IoT Devices
        │
        ▼
 Physical System
```

---

## Project Roadmap

| Project | Focus |
| --- | --- |
| [Project 1](week-01/README.md) | Understanding IoT Systems & Threat Modeling |
| [Project 2](week-02/README.md) | Learning Python for IoT Security |
| [Project 3](week-03/README.md) | Building an Insecure MQTT Pipeline |
| [Project 4](week-04/README.md) | Securing the Pipeline & Measuring the Cost |
| [Project 5](week-05/README.md) | Enforcing Device Identity & Provisioning |
| [Project 6](week-06/README.md) | Defeating Replay Attacks |
| [Project 7](week-07/README.md) | Building a Real-Time Security Dashboard |
| [Project 8](week-08/README.md) | Adding AI-Powered Anomaly Detection |

---

## Security Journey

The repository follows a progression from understanding the system to implementing defensive controls:

```text
IoT Architecture
       ↓
Threat Modeling
       ↓
Python Security Automation
       ↓
Insecure MQTT Pipeline
       ↓
Attack & Risk Analysis
       ↓
TLS / Pipeline Hardening
       ↓
Device Identity
       ↓
Replay Protection
       ↓
Real-Time Monitoring
       ↓
Anomaly Detection
```

---

## Key Areas Covered

Throughout the externship, I worked with concepts including:

- IoT architecture and security
- MQTT publish/subscribe communication
- Threat modeling
- Python for security automation
- MQTT security testing
- Network traffic analysis
- TLS encryption
- Device authentication and identity
- Secure device provisioning
- Replay attack prevention
- Security logging and monitoring
- Real-time security dashboards
- Anomaly detection

---

## Technologies & Concepts

`Python` `MQTT` `IoT Security` `TLS` `Threat Modeling` `Network Security` `Device Identity` `Authentication` `Replay Protection` `Security Monitoring` `Anomaly Detection`

---

## Repository Structure

```text
secure-mqtt-pipeline/
│
├── README.md
│
├── week-01/
│   └── README.md
├── week-02/
│   └── README.md
├── week-03/
│   └── README.md
├── week-04/
│   └── README.md
├── week-05/
│   └── README.md
├── week-06/
│   └── README.md
├── week-07/
│   └── README.md
└── week-08/
    └── README.md
```

Each project folder documents the objectives, technical work, security concepts, findings, and lessons learned during that stage of the externship.

---

## Key Takeaway

This externship demonstrated that securing IoT systems requires more than protecting network traffic.

A secure IoT environment requires multiple layers of defense:

**secure communication, trusted device identity, message integrity, replay protection, monitoring, and detection.**

The project gave me practical experience following an IoT system from initial architecture and threat analysis through the implementation of defensive security controls and continuous monitoring.

---

## Disclaimer

This repository is intended for **educational and portfolio purposes**. The Grand Marina environment is a simulated scenario provided as part of the Hydroficient Cybersecurity Externship through Extern. Any attack techniques demonstrated during the project were performed only within authorized training environments.
