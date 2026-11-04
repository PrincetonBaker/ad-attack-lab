# ad-attack-lab

A small Active Directory forest built entirely as code, with deliberately planted misconfigurations. I use it to practice red team tradecraft (CRTO and SpecterOps RTO style) and to write a detection for every attack path.

> ## ⚠️ Legal / lab-only disclaimer
> This lab is **intentionally insecure**. Build it only on an isolated host-only network that you own. Only run these techniques against systems you have written authorization to test. Nothing here is meant for use against production or third-party systems.

## Scope
| Attack path | Planted misconfiguration | Write-up |
|---|---|---|
| Kerberoasting | `svc_sql` has an SPN and a weak password | [writeups/01-kerberoasting.md](writeups/01-kerberoasting.md) |
| ADCS ESC1 | `VulnUser` template: enrollee supplies subject, Client Auth EKU, Domain Users can enroll | [writeups/02-adcs-esc1.md](writeups/02-adcs-esc1.md) |
| ADCS ESC8 | Web Enrollment (HTTP, NTLM, no EPA) on the CA | [writeups/03-adcs-esc8.md](writeups/03-adcs-esc8.md) |
| Unconstrained delegation | `WS01$` has TRUSTED_FOR_DELEGATION | [writeups/04-unconstrained-delegation.md](writeups/04-unconstrained-delegation.md) |

## Architecture
```
 host-only 192.168.56.0/24   domain: corp.lab
 ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌──────────────┐
 │ DC01 .10   │  │ CA01 .11   │  │ WS01 .20   │  │ attacker .50 │
 │ AD DS, DNS │  │ AD CS +    │  │ Win10/11   │  │ (bring your  │
 │            │  │ Web Enroll │  │ unconstr.  │  │  own Kali)   │
 └────────────┘  └────────────┘  └────────────┘  └──────────────┘
 Sysmon + Windows audit policy on every Windows host (for detection notes)
```

## Usage
```bash
cd vagrant && vagrant up            # builds DC01, CA01, WS01 (VirtualBox)
cd ../ansible && ansible-playbook -i inventory.yml site.yml
```
Requirements: Vagrant 2.4+, VirtualBox 7, Ansible 2.15+ with `ansible.windows`, `microsoft.ad`, and `community.windows` (see `ansible/requirements.yml`), and about 16 GB of RAM.

Teardown: `cd vagrant && vagrant destroy -f`

## Status
Scaffold. The roles are skeletons, and items marked `TODO` are still to be implemented and tested.
