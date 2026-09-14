# Week 1: IoT Architecture and Security Fundamentals

## Overview

During Week 1 of my **Hydroficient Cybersecurity Externship through Extern**, I focused on understanding IoT architecture, MQTT communication, data flows, and the security risks associated with connected physical systems.

The project used a simulated environment involving **The Grand Marina Hotel**, where HYDROLOGIC IoT devices monitor and control critical water infrastructure.

---

## Project Environment

The simulated Grand Marina environment included:

- 500 guest rooms across 15 floors
- 12 restaurants
- Pool and spa facilities
- Commercial kitchen and laundry
- Approximately 2,000 guests
- Three HYDROLOGIC IoT devices

| Device | Location | Systems Served |
| --- | --- | --- |
| Device 01 | Main Building | Guest rooms, lobbies, restaurants |
| Device 02 | Pool/Spa Wing | Pool, spa, fitness center |
| Device 03 | Kitchen/Laundry Wing | Kitchen and laundry facilities |

The devices monitor information such as:

- Upstream and downstream water pressure
- Flow rate
- Gate positions
- Water consumption
- System performance

Operators can also remotely control parts of the system, including gate positions and emergency water shutoff.

---

## IoT Architecture

I learned to analyze an IoT environment by identifying four major components:

| Component | Function |
| --- | --- |
| Sensor | Collects information from the physical environment |
| Network/Broker | Transfers and routes messages |
| Subscriber | Receives and processes information |
| Actuator | Performs a physical action |

### Sensor Data Flow

```text
Pressure Sensor
      ↓
HYDROLOGIC Device
      ↓
Network
      ↓
MQTT Broker
      ↓
Cloud / Dashboard
      ↓
Operator
```

### Command Flow

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

This demonstrated that IoT communication is **bidirectional**. Sensor information travels toward the monitoring system, while commands travel back to physical devices.

---

## MQTT Fundamentals

I learned how **MQTT (Message Queuing Telemetry Transport)** is used for communication between IoT devices and cloud systems.

MQTT uses a **publish/subscribe model**:

```text
Publisher
    ↓
MQTT Broker
    ↓
Subscriber
```

Devices publish messages to specific topics, while applications subscribe to the topics containing the information they need.

### Example MQTT Topics

```text
hydroficient/grandmarina/device-01/pressure/upstream

hydroficient/grandmarina/device-01/pressure/downstream

hydroficient/grandmarina/device-01/flow/rate

hydroficient/grandmarina/commands/device-01/gate/set

hydroficient/grandmarina/commands/device-01/shutoff
```

I also learned how MQTT wildcards work:

| Wildcard | Purpose |
| --- | --- |
| `#` | Matches everything below a topic level |
| `+` | Matches any single topic level |

For example:

```text
hydroficient/grandmarina/#
```

could subscribe to all messages associated with the Grand Marina environment.

---

## Security Analysis

After understanding the architecture, I examined the system from an attacker's perspective.

The main attack surfaces included:

```text
IoT Device
    ↓
Network
    ↓
MQTT Broker
    ↓
Cloud Infrastructure
    ↓
Dashboard
    ↓
Physical Controls
```

I identified several potential attack scenarios.

### Eavesdropping

An attacker with network access could potentially monitor MQTT traffic and learn:

- Device identifiers
- MQTT topic structures
- Sensor readings
- Operational patterns
- Command topics

### Device Spoofing

An attacker could attempt to impersonate a legitimate IoT device and publish false sensor readings.

### Replay Attack

A legitimate MQTT message could potentially be captured and retransmitted later.

This highlighted the importance of timestamps and message validation.

### Command Injection

Unauthorized access to MQTT command topics could allow an attacker to send malicious instructions to IoT devices.

Potential consequences include:

- Changing gate positions
- Triggering emergency shutoffs
- Manipulating water flow
- Disrupting hotel operations

### Denial of Service

An attacker could flood the MQTT broker with messages and interfere with legitimate sensor readings or commands.

---

## Network Segmentation Risk

One scenario involved an attacker accessing the hotel's guest Wi-Fi.

The intended architecture should isolate the guest network from the IoT environment:

```text
Guest Wi-Fi
     X
     X  BLOCKED
     X
IoT Network
```

A network misconfiguration could potentially create an unintended path:

```text
Guest Wi-Fi
     ↓
Misconfigured Network
     ↓
IoT Network
     ↓
MQTT Broker
```

This demonstrated why **network segmentation** is important when protecting IoT infrastructure.

---

## Security Controls Identified

Based on the attack surface analysis, I identified several important security controls:

- TLS encryption for MQTT communication
- Strong device authentication
- User authentication
- MQTT topic-level authorization
- Network segmentation
- Restricted access to command topics
- Logging and monitoring
- Device identity validation
- Protection of administrative dashboards
- Monitoring for abnormal message rates

Command topics require especially strong protection because they can directly affect physical systems.

---

## Skills Developed

During Week 1, I developed experience with:

- IoT architecture
- MQTT
- Publish/subscribe communication
- MQTT topics and wildcards
- Sensors and actuators
- IoT data-flow analysis
- Attack surface identification
- Network segmentation
- IoT threat analysis
- Cyber-physical security
- Security control identification

---

## Key Takeaway

The most important lesson from Week 1 was that **IoT security extends beyond protecting information**.

A compromised traditional system may result in stolen or manipulated data. A compromised IoT system can potentially cause changes in the **physical world**.

Understanding the architecture, communication paths, devices, protocols, and control mechanisms is therefore the first step toward properly securing an IoT environment.

---

> **Project Note:** This repository documents my cybersecurity work and learning completed during the Hydroficient Cybersecurity Externship through Extern. The Grand Marina environment is a simulated project scenario used as part of the externship.
