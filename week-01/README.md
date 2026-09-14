# Week 1: Understanding IoT Systems & Threat Modeling

## Overview

Week 1 focused on understanding the architecture of an IoT water-management system and then analyzing its security risks.

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

A simplified telemetry flow looks like:

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

Instead of devices communicating directly with every application:

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
#  → Everything below a topic level
+  → Any single topic level
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

### Interesting Finding

The analysis showed that **Confidentiality is not always the highest priority in cybersecurity**.

For pressure readings, I rated Integrity as `5` because inaccurate sensor data could cause incorrect decisions or physical damage.

For emergency shutoff and leak alerts, Availability received a `5` because those capabilities need to remain accessible during an emergency.

Dashboard credentials were different. I rated Confidentiality as `5` because stolen privileged credentials could provide an attacker access to the system.

---

## Attack Mapping

I then mapped six attack techniques to realistic scenarios within the Grand Marina environment.

| Attack | Scenario | CIA Impact |
|---|---|---|
| Eavesdropping | Capture sensor traffic traveling toward the broker | Confidentiality |
| Spoofing | Introduce or modify sensor information | Integrity |
| Replay Attack | Capture and reuse a legitimate control command | Integrity |
| Man-in-the-Middle | Intercept and modify messages sent to the dashboard | Confidentiality & Integrity |
| Denial of Service | Flood the MQTT broker with traffic | Availability |
| Unauthorized Access | Steal privileged credentials and gain system control | C, I & A |

---

## Threat Prioritization

I ranked the attacks based on their potential impact on the environment:

1. **Unauthorized Access**
2. **Man-in-the-Middle**
3. **Replay Attack**
4. **Spoofing**
5. **Denial of Service**
6. **Eavesdropping**

### Highest Risk: Unauthorized Access

I ranked **Unauthorized Access** first because compromising a privileged account could potentially affect all three parts of the CIA Triad.

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

📄 **[View My Complete Asset CIA Analysis](./Tural-Aghabalayev-Asset-CIA-Analysis.pdf)**

The full assessment includes:

- CIA ratings and justifications
- Six mapped attack scenarios
- Targeted assets
- CIA impact analysis
- Final threat-priority ranking

---

# Step 3: Threat Modeling

> **Coming next:** Building the final threat model using the architecture and risk analysis from Steps 1 and 2.

---

## Week 1 Skills

`IoT Security` `MQTT` `CIA Triad` `Threat Analysis` `Risk Prioritization` `Attack Surface Analysis` `Network Security` `Cyber-Physical Security`
