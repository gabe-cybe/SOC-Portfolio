# Lab SOC

Lab personnel, local et isolé, pour s'entraîner à la détection et au triage d'alertes.
Monté avec un objectif de sobriété : une seule machine virtuelle, l'hôte Windows servant de poste « attaquant ».

## Architecture

| Machine | OS | Rôle | IP (réseau privé) |
|---|---|---|---|
| soc-wazuh | Ubuntu Server 22.04 LTS | Cible SSH + Wazuh (manager, indexeur, tableau de bord) | 192.168.56.10 |
| hôte | Windows 11 | Poste d'administration + génération des attaques | 192.168.56.1 |

**Ressources VM :** 4 Go RAM, 2 vCPU, disque VDI dynamique 40 Go (≈ 12 Go réellement consommés).
**Hyperviseur :** VirtualBox.

### Schéma réseau

```
                 Réseau privé hôte (Host-Only)
                      192.168.56.0/24
                             │
   ┌─────────────────────┐   │   ┌───────────────────────────────┐
   │  Hôte Windows 11    │   │   │  VM Ubuntu  (soc-wazuh)        │
   │  192.168.56.1       │◀──┴──▶│  192.168.56.10                │
   │  « attaquant »      │ SSH   │  sshd (cible)                 │
   │  + navigateur       │ HTTPS │  Wazuh manager + dashboard    │
   └─────────────────────┘       └───────────────────────────────┘
          │  NAT (adaptateur 1 de la VM) ──▶ Internet (mises à jour, install Wazuh)
```

- **Adaptateur 1 (NAT)** : accès Internet de la VM (mises à jour, installation de Wazuh).
- **Adaptateur 2 (réseau privé hôte)** : liaison isolée hôte ↔ VM. C'est par là que transitent les attaques simulées et l'accès au tableau de bord.

> ⚠️ Toutes les attaques sont menées **uniquement** dans ce lab isolé, jamais vers une machine tierce.

## Journal de montage

### Séance du 2026-10-10 — mise en place du socle

1. **Installation de VirtualBox** sur l'hôte Windows 11.
2. **Création de la VM `soc-wazuh`** : 4 Go RAM, 2 vCPU, disque dynamique 40 Go. Option « installation automatique » désactivée pour maîtriser la configuration.
3. **Installation d'Ubuntu Server 22.04 LTS** (sans environnement graphique).
4. **Configuration réseau à deux adaptateurs** :
   - Adaptateur 1 en NAT (Internet).
   - Adaptateur 2 en réseau privé hôte (Host-Only), réseau `192.168.56.0/24`.
5. **Activation du serveur SSH** sur la VM :
   ```bash
   sudo apt update
   sudo apt install -y openssh-server
   sudo systemctl enable --now ssh
   ```
6. **Attribution d'une adresse IP fixe** à l'interface du réseau privé (`enp0s8`), rendue permanente via netplan :
   ```bash
   echo 'network: {version: 2, ethernets: {enp0s8: {dhcp4: false, addresses: [192.168.56.10/24]}}}' \
     | sudo tee /etc/netplan/99-hostonly.yaml
   sudo chmod 600 /etc/netplan/99-hostonly.yaml
   sudo netplan apply
   ```
7. **Vérification de la liaison hôte → VM** depuis Windows : `ping` OK, port TCP 22 ouvert, connexion SSH établie depuis l'hôte.

**État à la fin de la séance :** réseau isolé fonctionnel, VM accessible en SSH depuis l'hôte, adresse IP persistante.

## Configuration (à compléter au fil des séances)

- Installation de Wazuh (manager + tableau de bord) : _à venir_
- Agent Wazuh / sources de logs surveillées : _à venir_
- Règles Wazuh personnalisées : _à venir_

## Prochaines étapes

- [ ] Installer Wazuh (script all-in-one) et accéder au tableau de bord depuis l'hôte.
- [ ] Exercice 1 : brute force SSH depuis l'hôte → détection dans Wazuh → rapport de triage.

## Exercices réalisés

| Semaine | Exercice | Investigation |
|---|---|---|
| S1 | Brute force SSH | [lien](../01-Alert-Triage/ssh-bruteforce/investigation.md) _(à venir)_ |
| — | Échecs RDP (4625) | [lien](../01-Alert-Triage/rdp-failed-logons/investigation.md) _(nécessite une VM Windows)_ |

## Évolutions envisagées du lab

Au-delà du socle actuel (sobre à dessein), le lab pourra être étendu selon les besoins des exercices :
- VM Windows (poste client + Sysmon) pour les exercices de détection Windows (Event 4625, 4688, persistance…).
- Contrôleur de domaine Active Directory.
- Suricata / Zeek pour l'analyse réseau.
- VM Kali dédiée à l'attaque (en remplacement de l'hôte).
