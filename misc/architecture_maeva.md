```text
               Internet
                  |
          (1) Tunnel VPN chiffré (WireGuard / SSL VPN)
                  |
        +--------------------------+
        | Firewall Pro (VPN Server)|
        | - MFA                   |
        | - ACL / règles          |
        | - Logs                  |
        +------------+------------+
                     |
                 Réseau cabinet (LAN)
                     |
      +--------------+-------------------+
      |                                  |
+-----+------------------+       +-------+--------+
| PC HÔTE WINDOWS (APP)  |       |  (optionnel)   |
| - Application pro      |       | Serveur/BDD    |
| - (BDD locale ou LAN)  |       | si séparée     |
| - RDP activé           |       +----------------+
| - Chiffrement disque   |
+-----------+------------+
            ^
            | (2) RDP uniquement via VPN
            |
   +--------+---------+      +------------------+
   | PC Pro (domicile)|      | PC Collaborateur |
   | Client VPN + RDP |      | Client VPN + RDP |
   +------------------+      +------------------+
```
