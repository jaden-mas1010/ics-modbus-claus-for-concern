# Attacker Chain – ICS/Modbus (Claus for Concern)

This attacker chain explains how an adversary can abuse insecure‑by‑design Modbus communication to interact with a PLC, enumerate registers, and manipulate process behaviour. It also maps each attack phase to the Python scripts used during analysis.

---

## 1. Reconnaissance

The attacker identifies a PLC exposed on TCP port **502** (Modbus).  
Because Modbus has **no authentication**, any host with network access can communicate with the device. 

**Related script:**  
- `discovery.py` — Scans coils, holding registers, and device information.

Key actions:
- Identify Modbus‑enabled devices  
- Determine PLC model and supported function codes  
- Map holding registers to understand memory layout 

---

## 2. Modbus Interaction

After discovering the PLC, the attacker interacts with it using standard Modbus function codes. 

Typical operations:
- **FC03 – Read Holding Registers** (enumerate memory)  
- **FC01/FC02 – Read Coils/Inputs** (process state)  
- **FC06 – Write Single Register** (modify values)  
- **FC16 – Write Multiple Registers** (bulk manipulation) 

**Related script:**  
- `set_registry.py` — Writes values to specific PLC registers.

This phase reveals operational data and exposes writable registers that influence process logic.

---

## 3. Register Manipulation (Attack Phase)

The attacker identifies critical registers that control process behaviour.  
By writing malicious values, the attacker can disrupt operations or force unsafe states. 

Your attack scripts represent different impact scenarios:

### **Shutdown Attacks**
- `attack_shutdown.py`  
- `attack_shutdown2.py`  

These scripts write values that trigger shutdown conditions or force the PLC into a fault/safe state.

### **Stop Fill Attacks**
- `attack_stop_fill.py`  
- `attack_stop_fill2.py`  

These scripts manipulate registers controlling tank fill operations, halting flow or preventing filling.

### **Movement / Flow Manipulation**
- `attack_move_fill.py`  
- `attack_move_fill2.py`  

These scripts alter actuator/sensor states to redirect flow or disrupt normal movement.

Examples of register abuse:  
- Overwriting configuration values  
- Manipulating sensor/actuator states  
- Injecting unexpected data into control logic 

Because Modbus lacks integrity and authentication, the PLC accepts these writes as legitimate commands.

---

## 4. Impact on Industrial Process

Unauthorized register manipulation can lead to:  
- Incorrect process states  
- Fault conditions  
- Forced shutdowns  
- Loss of operator visibility  
- Unsafe or unexpected behaviour 

The attacker effectively gains control over parts of the industrial process without needing credentials or bypassing complex security controls.

---

## 5. SOC Detection Opportunities

SOC teams can detect malicious Modbus activity by monitoring:  
- Unexpected Modbus write operations  
- High‑frequency register access  
- Access from non‑ICS hosts  
- Abnormal function code usage  
- Deviations from normal register patterns 

Detection rules (Suricata/Sigma) can flag suspicious Modbus traffic and help analysts respond quickly.

---

## Summary

This attacker chain demonstrates how insecure ICS protocols like Modbus can be abused using simple Python scripts.  
Referencing the scripts clarifies the attack flow, while the process explanation provides SOC‑ready analytical value. 
