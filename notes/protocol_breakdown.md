# Modbus Protocol Breakdown

This document provides a clear, SOC‑focused breakdown of the Modbus protocol as used in the ICS/Modbus – Claus for Concern challenge. It explains how Modbus communicates, how PLC registers are structured, and why the protocol is insecure by design.

---

## 1. What is Modbus?

Modbus is a legacy industrial protocol used for communication between PLCs, sensors, and control systems.  
It operates over TCP port **502** and is widely deployed in ICS/OT environments.

Key characteristics:
- No authentication  
- No encryption  
- No integrity checking  
- Accepts commands from any reachable host  

This makes Modbus extremely vulnerable to unauthorized read/write operations.

---

## 2. Modbus Data Model

Modbus organizes PLC memory into four main data types:

### **Coils (0xxxx)**
- Single-bit values  
- Represent digital outputs (ON/OFF)

### **Discrete Inputs (1xxxx)**
- Single-bit values  
- Represent digital inputs (sensor states)

### **Input Registers (3xxxx)**
- Read-only 16‑bit values  
- Represent sensor measurements

### **Holding Registers (4xxxx)**
- Read/write 16‑bit values  
- Control logic, configuration, actuator states  
- **Most commonly abused by attackers**

---

## 3. Function Codes Used in Attacks

Attackers typically abuse the following Modbus function codes:

### **FC01 – Read Coils**
Used to understand actuator states.

### **FC03 – Read Holding Registers**
Used to enumerate PLC memory and identify critical values.

### **FC06 – Write Single Register**
Used to change one control value at a time.

### **FC16 – Write Multiple Registers**
Used for bulk manipulation of process logic.

These write operations allow attackers to directly influence industrial processes.

---

## 4. Why Modbus Is Insecure

Modbus was designed for isolated industrial networks.  
Modern ICS environments, however, are often connected to corporate networks or remote access systems.

Security weaknesses:
- No authentication → any host can issue commands  
- No encryption → values visible in plaintext  
- No integrity → PLC cannot verify if data is legitimate  
- No role separation → operators and attackers look identical to the PLC  

This makes Modbus ideal for attacker manipulation.

---

## 5. How Attack Scripts Exploit Modbus

Your Python scripts demonstrate realistic Modbus abuse:

- `discovery.py` → Enumerates registers and device info  
- `set_registry.py` → Writes arbitrary values to PLC registers  
- `attack_shutdown*.py` → Forces shutdown/fault conditions  
- `attack_stop_fill*.py` → Interrupts fill operations  
- `attack_move_fill*.py` → Manipulates movement/flow logic  

Each script uses FC06 or FC16 to overwrite critical holding registers.

---

## 6. SOC Monitoring Considerations

SOC teams should monitor:
- Unexpected Modbus write operations  
- Access from non‑ICS hosts  
- Abnormal function code usage  
- Register values outside normal ranges  
- High‑frequency register changes  

Modbus traffic should be restricted to known engineering workstations and HMIs.

---

## Summary

Modbus is a powerful but insecure protocol.  
Understanding its structure and weaknesses is essential for detecting and responding to ICS threats.  
This breakdown supports the attacker chain and SOC alert documentation in this repository.
