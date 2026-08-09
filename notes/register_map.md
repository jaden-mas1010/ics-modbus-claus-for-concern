# Modbus Register Map – ICS/Modbus (Claus for Concern)

This register map documents the PLC memory layout used during analysis of the ICS/Modbus – Claus for Concern challenge. It provides context for the attack scripts and helps SOC teams understand which registers were targeted and why.

---

## 1. Overview

Modbus organizes PLC memory into 16‑bit registers.  
The most commonly abused area is **Holding Registers (4xxxx)** because they are read/write and often control process logic.

This map reflects the registers observed during enumeration (`discovery.py`) and those manipulated by attack scripts.

---

## 2. Holding Registers (4xxxx)

| Register | Purpose / Description | Normal Value | Attack Value | Related Script |
|---------|------------------------|--------------|--------------|----------------|
| 0001    | Tank Fill Level        | 0–100        | Forced to 0 or 100 | `attack_stop_fill.py`, `attack_move_fill.py` |
| 0002    | Flow Direction Control | 1 = Forward, 2 = Reverse | 1 | 2 | `attack_move_fill2.py` |
| 0003    | Pump State             | 0 = Off, 1 = On | 1 | 0 | `attack_stop_fill2.py` |
| 0004    | System Mode            | 1 = Normal, 2 = Maintenance | 1 | 2 | `set_registry.py` |
| 0005    | Shutdown Trigger       | 0 = OK | 9999 | 9999 | `attack_shutdown.py`, `attack_shutdown2.py` |
| 0006    | Fault Indicator        | 0 = No Fault | 1 | 1 | `attack_shutdown2.py` |
| 0010–0020 | Sensor Values        | Varies | Overwritten | `set_registry.py` |

---

## 3. Coils (0xxxx)

| Coil | Purpose | Normal State | Attack State |
|------|----------|--------------|--------------|
| 00001 | Pump Enable | ON | OFF |
| 00002 | Valve Open  | ON | OFF |
| 00003 | Alarm       | OFF | ON |

These coils were read during reconnaissance but not directly manipulated in attack scripts.

---

## 4. Input Registers (3xxxx)

Read‑only sensor values:

| Register | Description |
|----------|-------------|
| 30001    | Temperature Sensor |
| 30002    | Pressure Sensor |
| 30003    | Flow Rate Sensor |

Used for situational awareness during reconnaissance (`discovery.py`).

---

## 5. Notes for SOC Analysts

- **Registers 0001–0006** are the most critical for process control.  
- Any write to **0005** (shutdown trigger) should be treated as **high severity**.  
- Unexpected changes to **flow direction (0002)** or **pump state (0003)** indicate possible manipulation.  
- High‑frequency writes across **0010–0020** may indicate attempts to corrupt sensor data.  

---

## Summary

This register map provides context for the attack scripts and detection rules in this repository.  
It helps analysts understand how Modbus register manipulation impacts industrial processes and supports accurate alert triage.
