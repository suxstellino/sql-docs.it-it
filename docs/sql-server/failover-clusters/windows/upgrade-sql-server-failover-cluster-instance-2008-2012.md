---
title: 'Aggiornare le istanze di SQL Server: cluster Windows Server 2012 e versioni precedenti'
description: Descrive come aggiornare le istanze del cluster di failover di SQL Server in esecuzione in Windows Server 2008, Windows Server 2008 R2 e Windows Server 2012.
ms.date: 06/15/2020
ms.custom: seo-lt-2019
ms.prod: sql
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- upgrading failover clusters
- clusters [SQL Server], upgrading
- failover clustering [SQL Server], upgrading
author: cawrites
ms.author: chadam
ms.openlocfilehash: 90ab761cdc9a84008803ebb4bc5493eb87778f20
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642677"
---
# <a name="upgrade-sql-server-instances-running-on-windows-server-20082008-r22012-clusters"></a>Aggiornare istanze di SQL Server in esecuzione in cluster di Windows Server 2008/2008 R2/2012

[!INCLUDE[nextref-longhorn-md](../../../includes/nextref-longhorn-md.md)], [!INCLUDE[winserver2008r2-md](../../../includes/winserver2008r2-md.md)] e [!INCLUDE[win8srv-md](../../../includes/win8srv-md.md)] impediscono ai cluster WSFC di eseguire aggiornamenti sul posto del sistema operativo, limitando la versione consentita di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per i cluster. Dopo aver aggiornato il cluster almeno alla versione [!INCLUDE[winblue-server-2-md](../../../includes/winblue-server-2-md.md)], il cluster rimarrà aggiornato.

## <a name="prerequisites"></a>Prerequisiti

-   Prima di eseguire una delle strategie di migrazione, è necessario preparare un cluster Windows Server Failover Cluster (WSFC) parallelo con Windows Server 2016/2012 R2. Tutti i nodi che comprendono istanze di cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] devono essere aggiunti al cluster Windows con le istanze del cluster di failover parallele installate. I computer autonomi **non devono** essere aggiunti al cluster WSFC prima della migrazione. I database utente devono essere sincronizzati nel nuovo ambiente prima della migrazione.
-   Tutte le istanze di destinazione devono eseguire la stessa versione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dell'istanza parallela corrispondente nell'ambiente originale, con gli stessi nomi e ID di istanza, e devono essere installate con le stesse funzionalità. I percorsi di installazione e la struttura di directory devono essere identici nei computer di destinazione. Ciò non vale per i nomi rete virtuale delle istanze di cluster di failover, che devono essere diversi prima della migrazione. Tutte le funzionalità abilitate dall'istanza originale (Always On, FILESTREAM e così via) devono essere abilitate nell'istanza di destinazione.

-   Prima della migrazione, nel cluster parallelo non devono essere installati [!INCLUDE[sshadrc-md](../../../includes/sshadrc-md.md)].

-   Il tempo di inattività durante la migrazione di un cluster che usa esclusivamente gruppi di disponibilità (con o senza istanze di cluster di failover di SQL Server) può essere limitato notevolmente tramite l'uso di gruppi di disponibilità distribuiti. In questo caso, tuttavia, tutte le istanze devono eseguire [!INCLUDE[sssql15-md](../../../includes/sssql15-md.md)] RTM o versione successiva.

-   Tutte le strategie di migrazione richiedono il ruolo sysadmin di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Tutti gli utenti di Windows usati da servizi di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], ad esempio account di Windows che eseguono agenti di replica, devono avere autorizzazioni a livello del sistema operativo per ogni computer nel nuovo ambiente.

-   Tutte le condivisioni file e tutte le unità di rete mappate usate nell'ambiente cluster [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] originale devono esistere ancora ed essere ancora accessibili da parte del cluster di destinazione con le stesse autorizzazioni delle istanze originali.

-   Le porte TCP/IP di ascolto da parte dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] originale non devono essere in uso e devono consentire il traffico in ingresso nel computer di destinazione.
-   Tutti i servizi correlati a SQL devono essere installati ed eseguiti dallo stesso utente di Windows.

-   Le istanze di destinazione devono essere installate con le stesse impostazioni locali delle istanze originali.

## <a name="migration-scenarios"></a>Scenari di migrazione

La strategia di migrazione appropriata dipende da alcuni parametri della topologia di cluster di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], in particolare dall'uso di [!INCLUDE[sshadrc-md](../../../includes/sshadrc-md.md)] e delle istanze di cluster di failover di SQL Server. La strategia da scegliere dipende anche dai requisiti dell'ambiente di destinazione. Se il nuovo ambiente richiede che ogni computer o ogni istanza di cluster di failover di SQL Server mantenga il nome rete virtuale originale o se la topologia di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dipende dal fatto che le nuove istanze ereditino tutti gli oggetti di sistema delle istanze originali, è necessario scegliere una strategia che esegua la migrazione di tali elementi.

|                                   | Richiede tutti gli oggetti server e tutti i nomi rete virtuale | Richiede tutti gli oggetti server e tutti i nomi rete virtuale | Non richiede gli oggetti server/i nomi rete virtuale\* | Non richiede gli oggetti server/i nomi rete virtuale\* |
|-----------------------------------|--------------------------------------|--------------------------------------------------------------------|------------|------------|
| **_Gruppi di disponibilità (S/N_)**                  | **_S_**                              | **_N_**                                                            | **_S_**    | **_N_**    |
| **Il cluster usa solo istanze di cluster di failover di SQL Server**         | [Scenario 3](#scenario-3-windows-cluster-has-both-sql-fcis-and-sql-server-availability-groups)                           | [Scenario 2](#scenario-2-windows-clusters-with-sql-server-failover-cluster-instances-fcis)                                                        | [Scenario 1](#scenario-1-windows-cluster-with-sql-server-availability-groups-and-no-failover-cluster-instances-fcis) | [Scenario 2](#scenario-2-windows-clusters-with-sql-server-failover-cluster-instances-fcis) |
| **Il cluster usa istanze autonome** | [Scenario 5](#scenario-5-windows-cluster-with-standalone-sql-server-instances-and-availability-groups)                           | [Scenario 4](#scenario-4-windows-cluster-with-standalone-sql-server-instances-and-no-availability-groups)                                                         | [Scenario 1](#scenario-1-windows-cluster-with-sql-server-availability-groups-and-no-failover-cluster-instances-fcis) | [Scenario 4](#scenario-4-windows-cluster-with-standalone-sql-server-instances-and-no-availability-groups) |

\* Esclusi i nomi di listener di gruppi di disponibilità

## <a name="scenario-1-windows-cluster-with-sql-server-availability-groups-and-no-failover-cluster-instances-fcis"></a>Scenario 1: Cluster Windows con gruppi di disponibilità di SQL Server e nessuna istanza del cluster di failover
Se la configurazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] usa esclusivamente gruppi di disponibilità e non usa istanze del cluster di failover, è possibile eseguire la migrazione in un nuovo cluster creando una distribuzione parallela di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in un altro cluster Windows con Windows Server 2016/2012 R2. Dopo questa operazione è possibile creare un gruppo di disponibilità distribuito in cui il cluster di destinazione sia un cluster secondario rispetto al cluster di produzione corrente. Ciò richiede l'aggiornamento a [!INCLUDE[sssql15-md](../../../includes/sssql15-md.md)] o versione successiva.

###  <a name="to-perform-the-upgrade"></a>Per eseguire l'aggiornamento

1.  Se necessario, aggiornare tutte le istanze a [!INCLUDE[sssql15-md](../../../includes/sssql15-md.md)] o versione successiva. Le istanze parallele devono eseguire la stessa versione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

2.  Creare un gruppo di disponibilità per il cluster di destinazione. Se il nodo primario del cluster di destinazione non è un'istanza di cluster di failover, creare un listener.

3.  Formare un gruppo di disponibilità distribuito in cui il cluster di destinazione corrisponda al gruppo di disponibilità secondario.

    >[!NOTE]
    >Il parametro LISTENER\_URL di T-SQL per la creazione del gruppo di disponibilità distribuito si comporta in modo diverso per i gruppi di disponibilità con un'istanza di cluster di failover che funge da istanza primaria. Se questo è il caso del gruppo di disponibilità primario o secondario, usare come URL del listener il nome di rete virtuale dell'istanza di cluster di failover di SQL Server primaria anziché il nome di rete del listener, insieme alla porta dell'endpoint di mirroring del database.

4.  Aggiungere il gruppo di disponibilità secondario al gruppo di disponibilità distribuito.

5.  Aggiungere i database secondari nel gruppo di disponibilità secondario.

    >[!NOTE]
    >Questa operazione viene eseguita automaticamente se il gruppo di disponibilità di destinazione usa il seeding automatico. Ciò è possibile solo se il percorso dei dati e quello del log sono gli stessi per tutte le repliche.

6.  Interrompere tutto il traffico al gruppo di disponibilità primario e consentire la sincronizzazione di quello secondario.

7.  Quando lo stato corrisponde a SYNCHRONIZED, modificare i criteri di commit in entrambi i gruppi di disponibilità in SYNCHRONOUS_COMMIT e indirizzare il failover al cluster di destinazione.

8.  Eliminare il gruppo di disponibilità distribuito.

9.  Eliminare o rinominare il listener nel gruppo di disponibilità originale.

10. Rinominare o creare un nuovo listener del gruppo di disponibilità con il nome del listener del gruppo di disponibilità originale.

    >[!NOTE]
    >Finché è presente il record DNS relativo al listener del gruppo di disponibilità originale, qualsiasi tentativo di creare un listener con questo nome avrà esito negativo.

11. Riprendere il traffico verso il listener.

## <a name="scenario-2-windows-clusters-with-sql-server-failover-cluster-instances-fcis"></a>Scenario 2: Cluster Windows con istanze del cluster di failover di SQL Server

Se l'ambiente di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] usa solo istanze del cluster di failover di SQL Server, è possibile eseguire la migrazione in un nuovo cluster creando un ambiente parallelo di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in un altro cluster Windows con Windows Server 2016/2012 R2. La migrazione al cluster di destinazione viene eseguita tramite il "furto" dei nomi rete virtuale delle istanze di cluster di failover di SQL Server precedenti e l'acquisizione di tali nomi nei nuovi cluster. Questa operazione crea tempo di inattività aggiuntivo a seconda dei tempi di propagazione del DNS.

###  <a name="to-perform-the-upgrade"></a>Per eseguire l'aggiornamento

1.  Eseguire un backup completo e interrompere il traffico verso il cluster di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] originale.

2.  Eseguire un backup della parte finale del log dei database utente ed effettuare il ripristino con RESTORE WITH RECOVERY nel nuovo ambiente.

3.  In Gestione cluster di failover nel cluster di destinazione arrestare tutti i ruoli del cluster delle istanze di cluster di failover di SQL Server.

4.  Sempre in Gestione cluster di failover nel cluster di destinazione riportare online i dischi cluster usati da ognuna delle istanze di cluster di failover di SQL Server.

5.  In Gestione cluster di failover nel cluster originale arrestare tutti i ruoli del cluster delle istanze di cluster di failover di SQL Server.

6.  Sempre in Gestione cluster di failover nel cluster originale riportare online i dischi cluster usati da ognuna delle istanze di cluster di failover di SQL Server.

7.  Copiare i database di sistema dai computer originali ai rispettivi computer di destinazione paralleli.

8.  In Gestione cluster di failover nell'ambiente originale modificare il nome della risorsa "Nome server" di ogni ruolo di istanza di cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

9.  Riportare online solo la risorsa Nome server rinominata per ognuno dei ruoli di istanza di cluster di failover di SQL Server.

10. In Gestione cluster di failover nel cluster di destinazione rinominare ora tutte le risorse "Nome server" dei ruoli delle istanze di cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] assegnando a tali risorse il nome precedentemente usato dal cluster originale.

    >[!NOTE]
    >Eventuali errori dovuti al fatto che i nomi erano in precedenza assegnati ad altri computer cesseranno quando i record DNS relativi ai nomi in questione saranno stati eliminati.

11. Dopo che tutte le istanze di cluster di failover sono state rinominate, riavviare tutti i computer nel nuovo cluster.

12. Man mano che i computer tornano online dopo il riavvio, avviare ognuno dei ruoli di istanza di cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in Gestione cluster di failover.

## <a name="scenario-3-windows-cluster-has-both-sql-fcis-and-sql-server-availability-groups"></a>Scenario 3: Cluster Windows con istanze del cluster di failover e gruppi di disponibilità di SQL Server

Se la configurazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] non usa istanze di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] autonome, ma solo istanze di cluster di failover di SQL Server all'interno di almeno un gruppo di disponibilità, è possibile eseguire la migrazione in un nuovo cluster tramite metodi simili a quelli dello scenario senza gruppi di disponibilità e senza istanze autonome. Prima di copiare le tabelle di sistema nei dischi condivisi delle istanze di cluster di failover di destinazione, è necessario rilasciare tutti i gruppi di disponibilità nell'ambiente originale. Dopo la migrazione di tutti i database nei computer di destinazione, i gruppi di disponibilità verranno ricreati con gli stessi nomi di schema e di listener. In questo modo, nel cluster di destinazione le risorse WSFC verranno formate e gestite correttamente. **È necessario abilitare Always On in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Configuration Manager per ogni computer nell'ambiente di destinazione prima della migrazione.**

### <a name="to-perform-the-upgrade"></a>Per eseguire l'aggiornamento

1.  Arrestare il traffico verso [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

2.  Eseguire un backup della parte finale del log dei database utente ed effettuare il ripristino con RESTORE WITH RECOVERY per il database da usare come primario nel nuovo ambiente e con NORECOVERY per i database da usare come secondari.

3.  In Gestione cluster di failover nel cluster di destinazione arrestare tutti i ruoli del cluster delle istanze di cluster di failover di SQL Server.

4.  Sempre in Gestione cluster di failover nel cluster di destinazione riportare online i dischi cluster usati da ognuna delle istanze di cluster di failover di SQL Server.

5.  Nel cluster originale eliminare il gruppo di disponibilità.

6.  In Gestione cluster di failover nel cluster originale arrestare tutti i ruoli del cluster delle istanze di cluster di failover di SQL Server.

7.  Sempre in Gestione cluster di failover nel cluster originale riportare online i dischi cluster usati da ognuna delle istanze di cluster di failover di SQL Server.

8.  Copiare i database di sistema dai computer originali ai rispettivi computer di destinazione paralleli.

9.  In Gestione cluster di failover nell'ambiente originale modificare il nome della risorsa "Nome server" di ogni ruolo di istanza di cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

10. Riportare online solo la risorsa Nome server rinominata per ognuno dei ruoli di istanza di cluster di failover di SQL Server.

11. In Gestione cluster di failover nel cluster di destinazione rinominare ora tutte le risorse "Nome server" dei ruoli delle istanze di cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] assegnando a tali risorse il nome precedentemente usato dal cluster originale.

12. Dopo che tutte le istanze di cluster di failover sono state rinominate, riavviare tutti i computer nel nuovo cluster.

13. Man mano che i computer tornano online dopo il riavvio, avviare ognuno dei ruoli di istanza di cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in Gestione cluster di failover.

14. Quando tutte le istanze sono online, formare il gruppo di disponibilità nella replica in cui i database sono stati ripristinati con RESTORE WITH RECOVERY.

15. Aggiungere tutte le repliche secondarie e tutti i database secondari al gruppo di disponibilità.

16. Nel nuovo gruppo di disponibilità creare un listener con lo stesso nome del listener del gruppo di disponibilità originale.

## <a name="scenario-4-windows-cluster-with-standalone-sql-server-instances-and-no-availability-groups"></a>Scenario 4: Cluster Windows con istanze di SQL Server autonome e nessun gruppo di disponibilità

La migrazione di un cluster con istanze autonome è un processo simile alla migrazione di un cluster di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] contenente solo istanze di cluster di failover. Tuttavia, anziché modificare il nome rete virtuale della risorsa cluster nome rete dell'istanza di cluster di failover, si modifica il nome del computer originale autonomo e si "ruba" il nome del computer precedente nel computer di destinazione. Questa operazione comporta un tempo di inattività aggiuntivo rispetto agli scenari senza istanze autonome, poiché non è possibile aggiungere il computer autonomo di destinazione al cluster WSFC finché non è stato acquisito il nome rete del computer precedente.

###  <a name="to-perform-the-upgrade"></a>Per eseguire l'aggiornamento

1.  Arrestare il traffico verso [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

2.  Eseguire un backup della parte finale del log dei database utente ed effettuare il ripristino con RESTORE WITH RECOVERY nel nuovo ambiente in ogni computer.

3.  In Gestione cluster di failover nel cluster di destinazione arrestare tutti i ruoli del cluster delle istanze di cluster di failover di SQL Server.

4.  Sempre in Gestione cluster di failover nel cluster di destinazione riportare online i dischi cluster usati da ognuna delle istanze di cluster di failover di SQL Server.

5.  In Gestione cluster di failover nel cluster originale arrestare tutti i ruoli del cluster delle istanze di cluster di failover di SQL Server e arrestare le istanze autonome di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Configuration Manager.

6.  Rinominare tutti i computer autonomi nel cluster originale con un nuovo nome computer univoco. Riavviare questi computer come indicato.

7.  In Gestione cluster di failover nel cluster originale riportare online i dischi cluster usati da ognuna delle istanze di cluster di failover di SQL Server.

8.  Copiare i database di sistema nei computer di destinazione.

9.  In Gestione cluster di failover nell'ambiente originale modificare il nome della risorsa "Nome server" di ogni ruolo di istanza di cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] usando un nome nuovo univoco.

10. Riportare online solo la risorsa Nome server rinominata per ognuno dei ruoli di istanza di cluster di failover di SQL Server.

11. Per le istanze autonome parallele, rinominare i computer usando i nomi dei computer autonomi originali (eliminare il nome del server precedente, aggiungere un nuovo nome del server con un parametro locale). Riavviare il computer secondo le istruzioni.

12. Dopo il riavvio, aggiungere i computer autonomi al cluster WSFC di destinazione.

13. In Gestione cluster di failover nel cluster di destinazione rinominare ora tutte le risorse "Nome server" dei ruoli delle istanze di cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] assegnando a tali risorse il nome precedentemente usato dal cluster originale.

14. Dopo che tutte le istanze di cluster di failover sono state rinominate, riavviare tutti i computer nel nuovo cluster.

15. Man mano che i computer tornano online dopo il riavvio, avviare ognuno dei ruoli di istanza di cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in Gestione cluster di failover.

## <a name="scenario-5-windows-cluster-with-standalone-sql-server-instances-and-availability-groups"></a>Scenario 5: Cluster Windows con istanze di SQL Server autonome e gruppi di disponibilità

La migrazione di un cluster che usa gruppi di disponibilità con repliche autonome è un processo simile alla migrazione di un cluster con istanze del cluster di failover tramite gruppi di disponibilità. È comunque necessario eliminare i gruppi di disponibilità originali e crearli di nuovo nel cluster di destinazione. Il processo tuttavia comporta un tempo di inattività maggiore a causa dei costi aggiuntivi per la migrazione di istanze autonome. **È necessario abilitare Always On per ogni istanza di cluster di failover nell'ambiente di destinazione prima della migrazione.**

###  <a name="to-perform-the-upgrade"></a>Per eseguire l'aggiornamento

1.  Arrestare il traffico verso [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

2.  Eseguire un backup della parte finale del log dei database utente ed effettuare il ripristino con RESTORE WITH RECOVERY per il database da usare come primario nel nuovo ambiente e con NORECOVERY per ogni database da usare come secondario.

3.  Eliminare il gruppo di disponibilità nel cluster originale.

4.  In Gestione cluster di failover nel cluster di destinazione arrestare tutti i ruoli del cluster delle istanze di cluster di failover di SQL Server.

5.  Sempre in Gestione cluster di failover nel cluster di destinazione riportare online i dischi cluster usati da ognuna delle istanze di cluster di failover di SQL Server.

6.  In Gestione cluster di failover nel cluster originale arrestare tutti i ruoli del cluster delle istanze di cluster di failover di SQL Server e arrestare le istanze autonome di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Configuration Manager.

7.  Rinominare tutti i computer autonomi nel cluster originale con un nuovo nome computer univoco. Riavviare questi computer come indicato.

8.  In Gestione cluster di failover nel cluster originale riportare online i dischi cluster usati da ognuna delle istanze di cluster di failover di SQL Server.

9.  Copiare i database di sistema nei computer di destinazione.

10. In Gestione cluster di failover nell'ambiente originale modificare il nome della risorsa "Nome server" di ogni ruolo di istanza di cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] usando un nome nuovo univoco.

11. Riportare online solo la risorsa Nome server rinominata per ognuno dei ruoli di istanza di cluster di failover di SQL Server.

12. Per le istanze autonome parallele, rinominare i computer usando i nomi dei computer autonomi originali (eliminare e aggiungere il nome di server in SQL Server). Riavviare il computer secondo le istruzioni.

13. Dopo il riavvio, aggiungere i computer autonomi al cluster WSFC di destinazione.

14. In Gestione cluster di failover nel cluster di destinazione rinominare ora tutte le risorse "Nome server" dei ruoli delle istanze di cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] assegnando a tali risorse il nome precedentemente usato dal cluster originale.

15. Dopo che tutte le istanze di cluster di failover sono state rinominate, riavviare tutti i computer nel nuovo cluster.

16. Man mano che i computer tornano online dopo il riavvio, avviare ognuno dei ruoli di istanza di cluster di failover di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in Gestione cluster di failover.

17. Quando tutte le istanze sono online, ricreare il gruppo di disponibilità in quella da usare come primaria.

18. Aggiungere le repliche secondarie e i database secondari.

19. Ricreare il listener del gruppo di disponibilità con lo stesso nome dell'originale.

## <a name="specific-concerns-for-individual-features"></a>Problemi specifici relativi alle singole funzionalità

### [!INCLUDE[sshadrc-md](../../../includes/sshadrc-md.md)]

-   **Endpoint del mirroring del database**

    Dal punto di vista di SQL Server, nella nuova istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] viene eseguita la migrazione dell'endpoint del mirroring del database e delle tabelle di sistema. Prima di eseguire la migrazione, assicurarsi che nei firewall siano applicate regole appropriate e che non ci siano altri processi in ascolto sulla stessa porta.

-   **Gruppi di disponibilità**

    La migrazione di gruppi di disponibilità e listener non può essere eseguita tra un'istanza e l'altra. Non è facile ricreare nell'ambiente di destinazione le risorse WSFC create dal gruppo di disponibilità. Anziché tentare di eseguire la migrazione di gruppi di disponibilità, l'obiettivo è eliminare i gruppi di disponibilità e ricrearli nel cluster di destinazione.

-   **Listener dei gruppi di disponibilità**

    Analogamente ai gruppi di disponibilità stessi, i listener vengono eliminati e ricreati anziché essere sottoposti direttamente alla migrazione.

### <a name="replication"></a>Replica

-   **Database di distribuzione, server di pubblicazione e sottoscrittori remoti**

    La relazione tra un database di distribuzione e un server di pubblicazione si basa solo sul nome rete virtuale dei computer che li ospitano, nome che viene risolto correttamente nel nuovo computer. Anche la migrazione dei processi di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent implica la migrazione delle tabelle di sistema, perché l'esecuzione dei diversi agenti di replica possa continuare come di consueto. Prima della migrazione è necessario che tutti gli account di Windows che eseguono [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent stesso o un qualsiasi processo di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent abbiano le stesse autorizzazioni nell'ambiente di destinazione. La comunicazione con il server di pubblicazione e con i sottoscrittori viene eseguita come di consueto.

-   **Cartella snapshot**

    Prima della migrazione è necessario che tutte le condivisioni di rete usate da qualsiasi funzionalità di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] siano accessibili da parte dei computer nell'ambiente di destinazione con le stesse autorizzazioni dell'ambiente originale. È necessario verificare che questa condizione sia soddisfatta prima di eseguire la migrazione.

### <a name="service-broker"></a>Service Broker

-   **Endpoint di Service Broker**

    Dal punto di vista di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], l'endpoint non crea alcun problema. Prima della migrazione è necessario assicurarsi che nessun processo sia in ascolto sulla stessa porta e che nessuna regola del firewall blocchi tale porta o che quest'ultima sia consentita in modo specifico da una regola del firewall.

-   **Certificati**

    Anche i certificati devono essere sottoposti a backup e ripristino nei computer di destinazione nel caso in cui debbano essere ripristinati in un nuovo computer.

-   **Route**

    Le route dipendono dal nome rete virtuale della destinazione, che sia per i nomi computer che per i nomi rete di istanze di cluster di failover di SQL Server viene risolto in modo appropriato nei computer corretti nel nuovo ambiente. Anche eventuali altri nomi rete virtuale a cui si faccia riferimento devono essere reindirizzati ai nuovi computer.

-   **Associazioni ai servizi remoti**

    Dopo la migrazione, le associazioni ai servizi remoti funzionano come previsto, man mano che viene eseguita la migrazione degli utenti che usano tali associazioni.

### <a name="ssnoversion-agent"></a>[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent

-   **Processi**

    I processi devono essere sottoposti a migrazione insieme ai database di sistema. Nel computer di destinazione, SQL Server Agent o gli utenti che eseguono un processo SQL Server Agent hanno le stesse autorizzazioni specificate nei prerequisiti.

-   **Avvisi e operatori**

    Gli avvisi e gli operatori devono essere sottoposti a migrazione con i database di sistema.

### <a name="filestream"></a>FILESTREAM

-   **Porte di condivisione file di Windows**

    Per usare FILESTREAM, devono esistere regole che consentano il traffico in ingresso attraverso le porte di condivisione file di Windows 139 e 445

-   **Condivisione di Windows**

    Il percorso condivisione di Windows dipende dalla risorsa nome di istanza di cluster di failover di SQL Server, con accesso tramite `\\ServerName\ShareName`. La migrazione di FILESTREAM richiede l'abilitazione di FILESTREAM in tutti i nodi in un'istanza di cluster di failover di destinazione. Se si usa una condivisione di Windows, i nodi devono essere configurati per l'uso dello stesso nome di condivisione di Windows del computer originale. Dopo che l'istanza di cluster di failover di destinazione ha ottenuto il nome server corretto, può ospitare la condivisione di Windows tramite il percorso desiderato.

-   **Dati FILESTREAM**

    I dati FILESTREAM vengono inclusi nel backup.

### <a name="integration-services"></a>Integration Services

-   **Progetti SSIS**

    La migrazione dei progetti SSIS viene eseguita insieme a quella del database SSIS. Dopo lo spostamento del database SSIS, i pacchetti sono eseguibili immediatamente prima dello spostamento delle tabelle di sistema.

-   **Origini dati basate su file**

    File flat, file di Excel, origini XML e altri file devono essere accessibili nella stessa posizione specificata dal pacchetto SSIS.

## <a name="next-steps"></a>Passaggi successivi
- [Completare l'aggiornamento al motore di database](../../../database-engine/install-windows/complete-the-database-engine-upgrade.md)
- [Modificare la modalità di compatibilità del database e usare l'archivio query](../../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md)
- [Vantaggi delle nuove funzionalità di SQL Server 2016](../../what-s-new-in-sql-server-2017.md)
- [Aggiornare un'istanza del cluster di failover di SQL Server](upgrade-a-sql-server-failover-cluster-instance.md)
- [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)
- [Aggiungere funzionalità a un'istanza di SQL Server 2016 (programma di installazione)](../../../database-engine/install-windows/add-features-to-an-instance-of-sql-server-setup.md)