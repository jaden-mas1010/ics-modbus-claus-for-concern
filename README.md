# ICS/Modbus – Claus for Concern

This repository contains my personal analysis and SOC‑focused documentation from the *ICS/Modbus – Claus for Concern* challenge. It explores how insecure‑by‑design industrial protocols like **Modbus** allow direct interaction with PLCs, enabling attackers to read/write registers and influence industrial processes.

## Overview

Industrial Control Systems often rely on legacy protocols such as Modbus, which lack authentication, encryption, and integrity checks. This challenge demonstrates how attackers can map PLC memory, manipulate registers, and disrupt operations through simple network access.

This repository includes my own work, such as:

- Modbus protocol and register structure breakdown  
- PLC register interpretation and behaviour analysis  
- Attacker chain walkthrough (recon → register abuse → impact)  
- SOC‑style alert documentation for malicious Modbus activity  
- Example detection logic (Suricata/Sigma)  
- Scripts and notes created during the challenge  
- Key OT/ICS security takeaways relevant to SOC operations  

## Why This Matters

ICS/OT environments are increasingly targeted by threat actors. Understanding how insecure protocols behave, how PLCs respond to unauthorized commands, and how to detect malicious activity is essential for modern SOC analysts—especially those working with critical infrastructure.

This case study demonstrates practical OT/ICS security knowledge and analytical skills applicable to real‑world monitoring and incident response.





