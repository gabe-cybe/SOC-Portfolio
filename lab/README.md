# Lab SOC

## Architecture
| Machine | OS | Rôle | IP |
|---|---|---|---|
| wazuh | | SIEM / manager Wazuh | |
| dc01 | Windows Server | Active Directory + agent Wazuh + Sysmon | |
| win-client | Windows 10/11 | Poste utilisateur + agent + Sysmon | |
| linux-srv | Ubuntu | Serveur SSH + agent Wazuh | |
| kali | Kali Linux | Machine attaquante | |

<Schéma réseau ici>

## Configuration
- Config Sysmon utilisée :
- Règles Wazuh personnalisées :
- Suricata / Zeek :

## Exercices réalisés
| Semaine | Exercice | Investigation |
|---|---|---|
| S1 | Brute force SSH (Hydra) | [lien](../01-Alert-Triage/ssh-bruteforce/investigation.md) |
| S2 | Échecs RDP (4625) | [lien](../01-Alert-Triage/rdp-failed-logons/investigation.md) |
| S3 | Atomic Red Team T1059.001 (PowerShell) | |
| S4 | Analyse d'en-têtes d'un mail test | |
| S5–6 | Scan Nmap → détection Suricata/Wazuh | |
| S7–8 | Compte admin + persistance (tâche planifiée / cron) | |
| S9 | Enrichissement des IOC collectés | |
| S10 | Rejeu d'une alerte avec la checklist "suspicious login" | |

> Toutes les attaques sont menées uniquement dans ce lab isolé.
