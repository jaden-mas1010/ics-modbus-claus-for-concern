# Attacker Chain – ICS/Modbus (Claus for Concern)

This attacker chain outlines how an adversary can abuse insecure-by-design Modbus communication to interact with a PLC, read/write registers, and influence industrial processes.

## 1. Reconnaissance

The attacker identifies an exposed ICS/OT network segment where a PLC is reachable over TCP port **502** (Modbus).  
Because Modbus lacks authentication, any host with network access can communicate with the device.

Key actions:
- Network scanning to discover Modbus-enabled devices  
- Identifying PLC model, function support, and register layout  
- Mapping holding registers to understand memory structure  

## 2. Modbus Interaction

Once the PLC is discovered, the attacker begins interacting with it using standard Modbus function codes.

Typical actions:
- **FC03 – Read Holding Registers** to enumerate memory  
- **FC01/FC02 – Read Coils/Inputs** to understand process state  
- **FC06 – Write Single Register** to modify values  
- **FC16 – Write Multiple Registers** for bulk manipulation  

This phase reveals operational data and exposes writable registers that control process logic.

## 3. Register Abuse

The attacker identifies critical registers that influence PLC behaviour.  
By writing malicious values, the attacker can alter process flow or disrupt operations.

Examples:
- Overwriting configuration values  
- Manipulating sensor/actuator states  
- Injecting unexpected data into control logic  

Because Modbus has no integrity or authentication, the PLC accepts these writes as legitimate.

## 4. Impact on Industrial Process

Unauthorized register manipulation can lead to:

- Incorrect process states  
- Fault conditions  
- Shutdowns or unsafe behaviour  
- Loss of visibility for operators  

The attacker effectively gains control over parts of the industrial process without needing credentials or bypassing complex security controls.

## 5. SOC Detection Opportunities

SOC teams can detect malicious Modbus activity by monitoring:

- Unexpected Modbus write operations  
- High-frequency register access  
- Access from non-ICS hosts  
- Abnormal function code usage  
- Deviations from normal register patterns  

Detection rules (Suricata/Sigma) can flag suspicious Modbus traffic and help analysts respond quickly.

---

This attacker chain demonstrates how insecure ICS protocols like Modbus can be abused and highlights the importance of monitoring OT networks for unauthorized activity.
