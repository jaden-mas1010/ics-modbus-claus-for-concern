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

Modbus was designed
