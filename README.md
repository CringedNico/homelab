# HOMELAB
## Virtualizzazione con Proxmox
### Architettura del Cluster

Il cluster è composto da:

1. **Due nodi fisici Proxmox** (**ITFENPRX01** e **ITFENPRX02**)

2. **Proxmox-qDevice** (**ITFENQDEV**): containerizzato con Docker sul NAS Synology. Questo dispositivo è configurato per partecipare al **quorum del cluster**, garantendo che il cluster rimanga operativo anche in caso di guasto a uno dei due nodi fisici, abilitando la funzionalità di **High Availability** (HA).

Lo **storage** è condiviso attraverso una SAN e utilizza il protocollo **iSCSI** per connettere e gestire i volumi di storage tra i nodi. La configurazione iSCSI permette a ciascun nodo di accedere ai volumi remoti come se fossero locali, migliorando la flessibilità e la scalabilità della tua infrastruttura.

Sono configurate due **LUN**:

- **LUN 1**: da **1TB** utilizzata per i dischi delle macchine virtuali (VM).
- **LUN 2**: da **500 GB** dedicata ai backup gestiti tramite **Veeam**.

Questa suddivisione permette una gestione efficiente dello storage, separando i dati operativi delle VM dai backup, e assicurando una maggiore sicurezza e facilità di recupero in caso di guasto.
