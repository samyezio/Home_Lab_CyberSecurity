# Wireshark Report — Day 3

## Capture
- Fichier : pcap/lab_capture_day3.pcapng
- Date : 2026-02-19 
- Interface : eth0 (VMware Host-only)
- Cible : 192.168.254.129

## Scénario généré
- ICMP : `ping -c 4 192.168.254.129`
- DNS : `nslookup metasploitable.domain`
- HTTP : `curl -I http://192.168.254.129/`

## 5 paquets intéressants

1) ICMP echo request Type 0
- Filtre : `icmp`
- Ce que je vois : Kali (192.168.254.128) envoie un Echo Request vers 192.168.254.129.
- Pourquoi c’est intéressant : vérifie la connectivité et montre le trafic ICMP.

2) ICMP echo reply Type 0
- Filtre : `icmp`
- Ce que je vois : 192.168.254.129 répond avec un Echo Reply vers Kali.
- Pourquoi c’est intéressant : confirme que la cible est joignable et active.

3) DNS query (A record)
- Filtre : `dns`
- Ce que je vois : requête DNS pour résoudre `metasploitable.domain` (type A).
- Pourquoi c’est intéressant : montre la résolution de nom (nom → IP) et le serveur DNS utilisé. dans le cas normal mais comme je suis dans un Lab isolé Host-only, pas de DNS accessible → requête envoyée mais aucune réponse .

4) DNS response
- Filtre : `dns`
- Ce que je vois : réponse DNS (adresse IP retournée) OU échec (NXDOMAIN / timeout si lab isolé).
- Pourquoi c’est intéressant : valide si le lab a accès à un DNS et permet d’interpréter les problèmes réseau.

5) HTTP request/response
- Filtre : `http` (ou `ip.addr == 192.168.254.129`) et clique droit sur un paquet http et follow -> TCP stream pour avoir l'échange TCP .
- Ce que je vois : requête HTTP HEAD `/` envoyée à la cible, réponse avec status code (200).
- Pourquoi c’est intéressant : confirme que le service web (port 80) répond et expose des headers.
