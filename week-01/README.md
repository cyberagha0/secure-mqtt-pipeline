# Week 1: Understanding IoT Systems & Threat Modeling

## Overview

Week 1 focused on understanding the architecture of an IoT water-management system and analyzing its security risks.

The simulated environment was **The Grand Marina Hotel**, where three HYDROLOGIC IoT devices monitor and control water infrastructure across guest rooms, restaurants, pool/spa facilities, and kitchen/laundry operations.

The week is divided into three steps:

1. **Understanding the IoT System & MQTT Architecture**
2. **CIA Analysis & Attack Mapping**
3. **Threat Modeling**

---

# Step 1: Understanding the IoT System & MQTT Architecture

## IoT Architecture

The first step was understanding how the system operates before trying to secure it.

The HYDROLOGIC environment contains:

- Pressure and flow sensors
- IoT devices
- MQTT communication
- MQTT broker
- Cloud infrastructure
- Management dashboard
- Physical controls such as gates and emergency shutoff

### Telemetry Flow

```text
Pressure / Flow Sensor
        ↓
HYDROLOGIC Device
        ↓
MQTT Broker
        ↓
Cloud / Dashboard
        ↓
Operator
```

Commands travel in the opposite direction:

```text
Operator
    ↓
Dashboard
    ↓
MQTT Broker
    ↓
HYDROLOGIC Device
    ↓
Gate / Valve
    ↓
Physical Water System
```

This introduced an important IoT security concept: **a cyberattack can create a physical impact**.

---

## MQTT Communication

The system uses **MQTT**, a lightweight publish/subscribe messaging protocol commonly used by IoT devices.

The basic communication model is:

```text
Publisher → MQTT Broker → Subscriber
```

Devices publish telemetry to MQTT topics while dashboards and other systems subscribe to the information they need.

Example topics:

```text
hydroficient/grandmarina/device-01/pressure/upstream
hydroficient/grandmarina/device-01/flow/rate
hydroficient/grandmarina/commands/device-01/shutoff
```

I also learned how MQTT wildcards can provide access to multiple topics:

```text
# → Everything below a topic level
+ → Any single topic level
```

For example:

```text
hydroficient/grandmarina/#
```

could subscribe to all messages beneath the Grand Marina topic hierarchy if permissions allowed it.

---

## Initial Attack Surface

After mapping the communication flow, I identified several potential attack points:

```text
IoT Device
    ↓
Network
    ↓
MQTT Broker
    ↓
Cloud
    ↓
Dashboard
    ↓
Physical Controls
```

Potential attacks included:

- Eavesdropping on MQTT traffic
- Spoofing devices or data
- Replaying legitimate commands
- Injecting unauthorized commands
- Flooding the MQTT broker
- Compromising dashboard access

This architecture analysis provided the foundation for the formal risk assessment in Step 2.

---

# Step 2: CIA Analysis & Attack Mapping

After understanding how the IoT system communicates, I evaluated the security requirements of its critical assets using the **CIA Triad**:

- **Confidentiality** — preventing unauthorized disclosure
- **Integrity** — ensuring information and commands remain accurate
- **Availability** — ensuring systems and controls remain accessible when needed

## Asset CIA Analysis

I rated each asset from **1 (low importance) to 5 (critical)** for Confidentiality, Integrity, and Availability.

| Asset | C | I | A | Primary Concern |
|---|---:|---:|---:|---|
| Pressure/Flow Readings | 3 | 5 | 4 | Integrity |
| Gate Control Commands | 3 | 4 | 5 | Availability |
| Emergency Shutoff | 3 | 4 | 5 | Availability |
| Dashboard Credentials | 5 | 4 | 3 | Confidentiality |
| Consumption/Savings Data | 3 | 4 | 2 | Integrity |
| Leak Detection Alerts | 3 | 4 | 5 | Availability |

### Key Finding

The analysis showed that **Confidentiality is not always the highest priority in cybersecurity**.

For pressure and flow readings, I rated **Integrity as 5** because inaccurate sensor data could lead to incorrect decisions or physical damage.

For emergency shutoff and leak detection alerts, **Availability received a 5** because these capabilities need to remain accessible during an emergency.

Dashboard credentials were different. I rated **Confidentiality as 5** because stolen privileged credentials could provide an attacker with access to the system.

---

## Attack Mapping

I mapped six attack techniques to scenarios within the Grand Marina environment.

| Attack | Scenario | CIA Impact |
|---|---|---|
| Eavesdropping | Capture sensor traffic traveling toward the broker | Confidentiality |
| Spoofing | Introduce or modify sensor information | Integrity |
| Replay Attack | Capture and reuse a legitimate control command | Integrity |
| Man-in-the-Middle | Intercept and modify messages sent to the dashboard | Confidentiality & Integrity |
| Denial of Service | Flood the MQTT broker with traffic | Availability |
| Unauthorized Access | Steal privileged credentials and gain system control | Confidentiality, Integrity & Availability |

---

## Threat Prioritization

I ranked the six attacks based on their potential impact on the environment:

1. **Unauthorized Access**
2. **Man-in-the-Middle**
3. **Replay Attack**
4. **Spoofing**
5. **Denial of Service**
6. **Eavesdropping**

### Highest Risk: Unauthorized Access

I ranked **Unauthorized Access** as the highest risk because compromising a privileged account could potentially affect all three parts of the CIA Triad.

```text
Compromised Credentials
        ↓
Unauthorized Dashboard Access
        ↓
View System Information
        +
Modify Controls
        +
Disrupt Operations
```

This made credential compromise potentially more damaging than an attack that only exposes information.

---

## Step 2 Deliverable

📄 [View My Complete Asset CIA Analysis](./Asset-CIA-Analysis.pdf)

The full assessment includes:

- CIA ratings and justifications
- Six mapped attack scenarios
- Targeted assets
- CIA impact analysis
- Final threat-priority ranking

---

# Step 3: STRIDE Threat Modeling

The final step of Week 1 moved from identifying individual risks to building a structured threat model for the Grand Marina IoT environment.

Before using a formal framework, I first practiced thinking from an attacker's perspective by asking:

- What is valuable?
- What is exposed?
- What is weak?
- What path could an attacker use?

## Attacker Mindset Exercise

For my attack scenario, I selected **data theft** as the primary objective.

I considered the hotel's guest-accessible wired and wireless networks as possible initial entry points because an attacker could potentially stay at the hotel, observe the environment, identify exposed systems, and search for weaknesses.

The hypothetical attack path was:

```text
Gain legitimate guest access
        ↓
Evaluate exposed wired / wireless networks
        ↓
Identify a vulnerable entry point
        ↓
Attempt to move toward IoT infrastructure
        ↓
Access valuable operational data
```

This exercise helped me understand that attackers often look for the **easiest path to a valuable asset**, rather than attacking the most obvious security control directly.

---

## STRIDE Framework

I then used the **STRIDE** framework to analyze threats systematically.

| STRIDE | Threat | Security Concern |
|---|---|---|
| S | Spoofing | Authentication |
| T | Tampering | Integrity |
| R | Repudiation | Accountability / Non-repudiation |
| I | Information Disclosure | Confidentiality |
| D | Denial of Service | Availability |
| E | Elevation of Privilege | Authorization |

STRIDE provided a repeatable checklist for evaluating each component of the IoT environment.

---

## Applying STRIDE to the Grand Marina

The main components analyzed were:

- HYDROLOGIC devices
- MQTT broker
- Cloud server
- Management dashboard
- Operators

Examples of threats included:

| Threat | Example |
|---|---|
| Spoofing | Fake device sends readings while pretending to be a legitimate HYDROLOGIC unit |
| Tampering | MQTT sensor readings or control commands are modified |
| Repudiation | An operator denies performing a critical action |
| Information Disclosure | Unauthorized access exposes telemetry or operational data |
| Denial of Service | Broker or dashboard becomes unavailable |
| Elevation of Privilege | A lower-privileged account gains administrative capabilities |

One of the most concerning scenarios was **tampering with sensor data**. If a pressure or flow reading is changed before reaching the dashboard, operators could believe the system is functioning normally while a real physical problem continues.

---

## Threat Model Process

I combined the work from all three steps into a formal threat model.

The process included:

```text
Understand System Architecture
        ↓
Identify Assets
        ↓
Map Data Flows
        ↓
Apply CIA Triad
        ↓
Think Like an Attacker
        ↓
Apply STRIDE
        ↓
Rate Likelihood & Impact
        ↓
Prioritize Risk
        ↓
Recommend Mitigations
```

The final threat model included:

- System description
- Data-flow analysis
- Asset inventory
- STRIDE threat analysis
- Likelihood and impact ratings
- Risk prioritization
- Recommended security controls

---

## Key Security Recommendations

The assessment identified several controls that could reduce risk:

- Multi-factor authentication
- TLS-encrypted communications
- Device certificates
- Strong access controls
- Network segmentation
- Audit logging
- Rate limiting
- Security monitoring and alerting

The most important lesson was that **security should break an attack path at multiple points rather than depend on a single control**.

---

## Step 3 Deliverable

📄 [View My Complete Grand Marina Threat Model](./Grand-Marina-Threat-Model.pdf)

The threat model documents my complete Week 1 security assessment, including STRIDE analysis, risk ratings, and mitigation recommendations.

---

# Week 1 Summary

Week 1 progressed from understanding the system to performing a structured security assessment:

```text
Step 1
IoT Architecture & MQTT
        ↓
Step 2
CIA Analysis & Attack Mapping
        ↓
Step 3
STRIDE & Threat Modeling
```

By the end of Week 1, I had practiced:

- IoT architecture analysis
- MQTT fundamentals
- CIA Triad analysis
- Attack-surface identification
- Threat prioritization
- Attacker-mindset analysis
- STRIDE threat modeling
- Risk assessment
- Security mitigation planning

---

## Week 1 Deliverables

- 📄 [Asset CIA Analysis](./Asset-CIA-Analysis.pdf)
- 📄 [Grand Marina Threat Model](./Threat-Model.pdf)

---

## Skills

`IoT Security` `MQTT` `CIA Triad` `STRIDE` `Threat Modeling` `Risk Assessment` `Attack Surface Analysis` `Network Security` `Defense in Depth`
