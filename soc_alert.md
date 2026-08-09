# SOC Alert – Malicious Modbus Register Manipulation

## Alert Title
Unauthorized Modbus Write Operations to PLC Registers

## Alert Summary
A host on the network is issuing Modbus write commands (FC06/FC16) to a PLC.  
Modbus is an insecure-by-design protocol with no authentication, meaning any device with network access can modify PLC registers. These write operations may indicate an attempt to disrupt industrial processes, trigger shutdowns, or manipulate actuator/sensor states.

## Detection Logic
Trigger an alert when:
- A non-ICS asset communicates with a PLC over TCP/502  
- Modbus function codes **FC06** (Write Single Register) or **FC16** (Write Multiple Registers) are observed  
- Register values deviate from normal operational baselines  
- High-frequency write operations occur within a short time window  
- Writes target known critical registers (process control, movement, fill levels, shutdown flags)

## Related Attack Scripts
These scripts demonstrate different malicious Modbus write scenarios:

- **Shutdown manipulation:**  
  `attack_shutdown.py`, `attack_shutdown2.py`

- **Stop-fill manipulation:**  
  `attack_stop_fill.py`, `attack_stop_fill2.py`

- **Movement/fill manipulation:**  
  `attack_move_fill.py`, `attack_move_fill2.py`

- **General register writing:**  
  `set_registry.py`

- **Reconnaissance:**  
  `discovery.py`

## Indicators of Compromise (IoCs)
- Unexpected Modbus traffic from engineering workstations, laptops, or unknown hosts  
- Modbus writes to registers normally only read during operation  
- Sudden changes in PLC-controlled process states  
- PLC entering fault, safe, or shutdown mode without operator action  
- Register values outside expected ranges

## Recommended Response
1. **Isolate the source host** generating Modbus write commands.  
2. **Capture PCAPs** to confirm function codes and register targets.  
3. **Validate PLC state** with operators to determine process impact.  
4. **Review access controls** on ICS network segments.  
5. **Implement network segmentation** to restrict Modbus access.  
6. **Deploy IDS rules** to detect future unauthorized Modbus writes.

## Severity
**High** – Unauthorized register manipulation can cause physical disruption, unsafe conditions, or full process shutdown.

---

This alert provides SOC analysts with a clear understanding of the threat, detection opportunities, and response steps for malicious Modbus activity.
