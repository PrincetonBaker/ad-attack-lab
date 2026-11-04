# unconstrained-delegation

**ATT&CK:** TODO (e.g., T1558.003 Kerberoasting, T1649 Steal or Forge Authentication Certificates, T1187/T1557 for relay)
**Lab date:** YYYY-MM-DD · **Operator:** Princeton Baker

## 1. Misconfiguration
What is planted, where it lives in Ansible, and why it shows up in real environments.

## 2. Enumeration
Commands and tools (for example BloodHound/SharpHound, Certify/Certipy, PowerView, Rubeus) and what the output showed.

## 3. Exploitation
Step-by-step with screenshots. Note the OPSEC considerations: noise level and alternatives.

## 4. Impact
The privilege you started with, the privilege you reached, and the business impact.

## 5. Detection
| Data source | Event / signal | Logic |
|---|---|---|
| DC 4624/4769 + WS01 Sysmon | Coerced auth (PrinterBug/PetitPotam) to WS01; LSASS access on WS01 (Sysmon 10) | DC$ TGT landing on non-DC host with TRUSTED_FOR_DELEGATION |

Sigma rule: `detections/04-unconstrained-delegation.yml` (TODO)

## 6. Remediation
The fix plus the control mapping (CIS / NIST 800-53).

## 7. Lessons learned
