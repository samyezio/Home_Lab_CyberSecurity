# CyberLab — Topology

## Objectif
Lab cybersécurité isolé et reproductible (VMware).
## Virtualisation 
-VMware WorkStation
## Machines
- KALI (Attacker) : Kali Linux
  - IP : 192.168.254.128
  - Rôle : scans (Nmap), capture trafic (Wireshark)
- TARGET (Victim) : Metasploitable2 
  - IP : 192.168.254.129
  - Rôle : machine cible

## Réseau VMware
- Mode : Host-Only (VMnet1)
- Isolation : on a utilisé l'isolation des VMs pour     qu'elles puisse communiqué dans un réseau privée 
- DHCP : ON

## Snapshots
- KALI : BASELINE_KALI 
- TARGET : BASELINE_TARGET 

## Tests de connectivité
- Ping KALI -> TARGET : OK
- Ping TARGET -> KALI : OK

