# Nmap Report — Day 2

## Target
- IP : 192.168.254.129
- OS (supposé) : Linux 2.6.9 – 2.6.33 (détection Nmap : Linux 2.6.X)
- Date : 2026-02-16 16:31 (EST)
- Réseau : Lab VMware (Host-only) — MAC VMware 00:0C:29:FC:B7:FB

## Commandes utilisées
- `ip a`
- `ping -c 4 192.168.254.129`
- `arp -a`
- `nmap -T4 -Pn 192.168.254.129 -oN reports/nmap_quick.txt`
- `nmap -sS -sV -O --script=default -p- 192.168.254.129 -oN reports/nmap_full.txt`
- `nmap -sV --top-ports 1000 192.168.254.129 -oN reports/nmap_top1000.txt`

## Résultats (ports/services)
> Note : Les versions ci-dessous proviennent du scan `-sV` (2e/3e Nmap).

| Port | Proto | Service | Version | Risque (1 phrase) |
|------|-------|---------|---------|------------------|
| 21   | TCP   | FTP     | vsftpd 2.3.4 + anonymous allowed | Service en clair + accès anonyme : risque d’exposition de fichiers / mauvaises permissions. |
| 22   | TCP   | SSH     | OpenSSH 4.7p1 (Debian 8ubuntu1) | Accès distant : risque si identifiants faibles / service ancien non durci. |
| 23   | TCP   | Telnet  | Linux telnetd | Telnet = authentification en clair : très risqué sur un réseau non isolé. |
| 25   | TCP   | SMTP    | Postfix smtpd (STARTTLS, VRFY, etc.) | Surface mail : peut révéler des infos (VRFY), relai mal config, chiffrement faible. |
| 53   | TCP   | DNS     | ISC BIND 9.4.2 | Service DNS : versions anciennes + risques de fuite d’info / mauvaise config. |
| 80   | TCP   | HTTP    | Apache 2.2.8 (Ubuntu) DAV/2 | Surface web : risque de failles applicatives + WebDAV peut exposer des fonctions sensibles si mal
configuré. |
| 445  | TCP   | SMB     | Samba 3.0.20-Debian | SMB critique : risques élevés si partages/permissions faibles (et message signing disabled). |
| 513  | TCP   | rlogin  | login? | Service legacy : peut permettre accès distant non sécurisé (selon config). |
| 514  | TCP   | rsh     | Netkit rshd | Service legacy : commandes distantes, très risqué sans isolation. |
| 1524 | TCP   | bindshell | “Metasploitable root shell” | Indique une shell root exposée : risque maximal (accès direct). |
| 2121 | TCP   | FTP     | ProFTPD 1.3.1 | FTP en clair : exposition credentials/données + surface d’attaque service. |
| 3306 | TCP   | MySQL   | 5.0.51a-3ubuntu5 | Base de données : risque si comptes par défaut / accès réseau autorisé. |
| 5432 | TCP   | PostgreSQL | 8.3.0–8.3.7 (SSL cert ancien) | DB exposée : risque auth faible + TLS/certificat obsolète/mauvaise config. |
| 5900 | TCP   | VNC     | Protocol 3.3 (VNC Auth) | Bureau à distance : risque brute force / accès interactif si mot de passe faible. |
| 8009 | TCP   | AJP13   | Apache Jserv Protocol v1.3 | AJP (Tomcat) : peut exposer des routes internes si mal configuré. |
| 8180 | TCP   | HTTP (Tomcat) | Apache Tomcat/5.5 (Coyote/JSP 1.1) | Serveur d’applis : surface web importante (admin pages, apps, configs). |


## Faits importants observés (NSE / scripts)
- `ftp-anon: Anonymous FTP login allowed` (FTP 21/tcp).
- SMTP : support de STARTTLS + commandes comme `VRFY` (exposition d’informations possible).
- SMB : `message_signing: disabled` (signalé comme dangereux par Nmap).
- HTTP : page “Metasploitable2 - Linux” sur 80/tcp.
- 1524/tcp identifié comme **bindshell / root shell** (risque critique).

## Hypothèses (interprétation)
- La cible ressemble fortement à **Metasploitable 2** (beaucoup de services legacy/volontairement vulnérables + banner “Metasploitable2 - Linux”).
- La machine expose une **grande surface d’attaque réseau** (FTP/Telnet/SMB/RPC/NFS/DB/Tomcat/VNC/IRC).
- Priorités d’analyse (sans exploitation) :

