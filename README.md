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

### VM di WSUS (Windows Server Update Services)

La VM di **WSUS** gestisce l'aggiornamento dei server Windows nel dominio, con le seguenti caratteristiche principali:

- **Ricerca degli aggiornamenti**: La VM esegue una ricerca degli aggiornamenti per **Windows Server 2022** due volte al giorno.
- **Approva automaticamente** gli aggiornamenti critici e di sicurezza per tutti i server in dominio tramite una regola configurata nel WSUS.
- Una **GPO** (Group Policy Object) è configurata per automatizzare l'installazione degli aggiornamenti sui server. Questa GPO specifica la modalità di notifica, di installazione e altre impostazioni riguardanti **Windows Update**, per garantire che gli aggiornamenti vengano installati senza richiedere l'intervento manuale degli amministratori, mantenendo i server sicuri e aggiornati.

### VM ITFENDC01 e ITFENDC02 (Domain Controllers)

Le VM **ITFENDC01** (VM 110) e **ITFENDC02** (VM 111) sono i **Domain Controllers** (Active Directory) della rete e gestiscono:

- **Dominio**: Sono responsabili della gestione del dominio, inclusa l'autenticazione degli utenti e dei dispositivi.
- **DNS**: Forniscono il servizio **DNS** per la risoluzione dei nomi all'interno della rete.
- **GPO** (Group Policy Objects): Gestiscono le **policy** di configurazione per tutti i server in dominio, assicurando che le configurazioni di sicurezza e altre impostazioni siano uniformi.

In **Active Directory** sono definiti vari gruppi di utenti, che stabiliscono:

- Quali utenti possono accedere alle macchine, differenziando tra macchine di **produzione** e di **sviluppo**.
- Quali utenti possono accedere a specifici **servizi** e risorse in base ai loro ruoli e permessi.

Questa configurazione centralizzata garantisce un controllo preciso su chi può accedere a determinate risorse e semplifica la gestione della sicurezza e delle autorizzazioni nella rete.
