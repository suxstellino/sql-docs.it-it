---
title: Gestire un server di pubblicazione replicato come parte di un gruppo di disponibilità
description: Informazioni su come gestire e amministrare un database che funge da server di pubblicazione in una replica SQL e fa anche parte di un gruppo di disponibilità Always On.
ms.custom: seodec18
ms.date: 05/18/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], interoperability
- replication [SQL Server], AlwaysOn Availability Groups
ms.assetid: 55b345fe-2eb9-4b04-a900-63d858eec360
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3068eaae089c4a9e7cd6ab2599eb04695d56aa6f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348437"
---
# <a name="manage-a-replicated-publisher-database-as-part-of-an-always-on-availability-group"></a>Gestire un server di pubblicazione replicato come parte di un gruppo di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  In questo argomento verranno illustrate alcune considerazioni speciali per la gestione di un database di pubblicazione quando si usano gruppi di disponibilità AlwaysOn.  
  
##  <a name="maintaining-a-published-database-in-an-availability-group"></a><a name="MaintainPublDb"></a> Gestione di un database di pubblicazione in un gruppo di disponibilità  
 La gestione di un database di pubblicazione AlwaysOn è sostanzialmente analoga a quella di un database di pubblicazione standard, con le considerazioni seguenti:  
  
-   L'amministrazione deve avvenire nell'host della replica primaria. In [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]le pubblicazioni vengono visualizzate nella cartella **Pubblicazioni locali** per l'host della replica primaria e per le repliche secondarie leggibili. Dopo il failover potrebbe essere necessario aggiornare manualmente [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] per riflettere la modifica se la replica secondaria promossa a primaria non era leggibile.  
  
-   In Monitoraggio replica le informazioni sulla pubblicazione vengono sempre visualizzate nel server di pubblicazione originale. Queste informazioni possono tuttavia essere visualizzate in Monitoraggio replica da qualsiasi replica aggiungendo il server di pubblicazione originale come server.  
  
-   Se l'amministrazione viene effettuata nella replica primaria corrente mediante oggetti RMO (Replication Management Objects) o stored procedure, nei casi in cui si specifica il nome del server di pubblicazione è necessario specificare il nome dell'istanza in cui il database è stato abilitato per la replica, ovvero il server di pubblicazione originale. Per determinare il nome appropriato, utilizzare la funzione **PUBLISHINGSERVERNAME** . Quando un database di pubblicazione viene unito in join a un gruppo di disponibilità, i metadati di replica archiviati nelle repliche di database secondarie sono identici a quelli della replica primaria. Ne consegue che, per i database di pubblicazione abilitati per la replica nel database primario, il nome dell'istanza del server di pubblicazione archiviato in tabelle di sistema nel database secondario equivale al nome del database primario e non a quello del database secondario. Ciò influisce sulla manutenzione e sulla configurazione della replica se viene eseguito il failover del database di pubblicazione su un database secondario. Se ad esempio si configura la replica con stored procedure in un database secondario dopo il failover e si vuole usare una sottoscrizione pull a un database di pubblicazione abilitato in una replica diversa, è necessario specificare il nome del server di pubblicazione originale anziché di quello corrente come parametro *\@publisher* di **sp_addpullsubscription** o **sp_addmergepullsubscription**. Se tuttavia si abilita un database di pubblicazione dopo il failover, il nome dell'istanza del server di pubblicazione archiviato nelle tabelle di sistema corrisponde al nome dell'host primario corrente. In questo caso si userà il nome host della replica primaria corrente per il parametro *\@publisher*.  
  
    > [!NOTE]  
    >  Per alcune procedure, ad esempio **sp_addpublication**, il parametro *\@publisher* è supportato solo per server di pubblicazione che non sono istanze di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], quindi non è rilevante per [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Always On.  
  
-   Per sincronizzare una sottoscrizione in [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] dopo un failover, sincronizzare le sottoscrizioni pull dal Sottoscrittore e le sottoscrizioni push dal server di pubblicazione attivo.  
  
##  <a name="removing-a-published-database-from-an-availability-group"></a><a name="RemovePublDb"></a> Rimozione di un database di pubblicazione da un gruppo di disponibilità  
 Considerare i problemi seguenti se un database pubblicato viene rimosso da un gruppo di disponibilità o se un gruppo di disponibilità che dispone di un database del membro pubblicato viene eliminato.  
  
-   Se il database di pubblicazione nel server di pubblicazione originale viene rimosso dalla replica primaria di un gruppo di disponibilità, è necessario eseguire **sp_redirect_publisher** senza specificare un valore per il parametro *\@redirected_publisher* per rimuovere il reindirizzamento per la coppia server di pubblicazione/database.  
  
    ```  
    EXEC sys.sp_redirect_publisher   
        @original_publisher = 'MyPublisher',  
        @published_database = 'MyPublishedDB';  
    ```  
  
     Il database rimarrà nello stato di recupero nella replica primaria e dovrà essere ripristinato. Una volta effettuata questa operazione, la replica dovrebbe funzionare invariata rispetto al server di pubblicazione originale.  
  
-   In caso di failover del database di pubblicazione dal server di pubblicazione originale a una replica e se il database viene rimosso dalla replica primaria del gruppo di disponibilità, usare la stored procedure **sp_redirect_publisher** per reindirizzare in modo esplicito il server di pubblicazione originale al nuovo server di pubblicazione. Il database rimarrà nello stato di recupero e dovrà essere ripristinato. Una volta effettuata questa operazione, la replica dovrebbe continuare a funzionare come nel gruppo di disponibilità.  
  
    ```  
    EXEC sys.sp_redirect_publisher   
        @original_publisher = 'MyPublisher',  
        @published_database = 'MyPublishedDB',  
        @redirected_publisher = 'MyNewPublisher';  
    ```  
  
     Non rimuovere il server remoto per il server di pubblicazione originale dal distributore, anche se non è più possibile accedere al server. I metadati del server per il server di pubblicazione originale sono necessari nel distributore per rispondere alle query sui metadati di pubblicazione.  
  
-   Se viene rimosso un gruppo di disponibilità completo, il comportamento di un database replicato membro è lo stesso di un database pubblicato rimosso da un gruppo di disponibilità. È possibile riprendere la replica dall'ultima replica primaria non appena viene ripristinato il database e modificato il reindirizzamento. Se il database viene ripristinato nel server di pubblicazione originale, il reindirizzamento deve essere rimosso. Se il database viene ripristinato in un host diverso, il reindirizzamento deve essere effettuato in modo esplicito al nuovo host.  
  
    > [!NOTE]  
    >  Quando viene rimosso un gruppo di disponibilità contenente database membro pubblicati o viene rimosso un database pubblicato da un gruppo di disponibilità, tutte le copie dei database pubblicati rimarranno nello stato di recupero. Se ripristinati, saranno tutti visualizzati come database pubblicato. Sarà necessario mantenere una sola copia con i metadati di pubblicazione. Per disabilitare la replica per una copia del database pubblicato, è necessario prima rimuovere tutte le sottoscrizioni e le pubblicazioni dal database.  
  
     Eseguire **sp_dropsubscription** per rimuovere le sottoscrizioni della pubblicazione. Accertarsi di impostare il parametro *\@ignore_distributor* su 1 per mantenere i metadati per il database di pubblicazione attivo nel server di distribuzione.  
  
    ```  
    USE MyDBName;  
    GO  
  
    EXEC sys.sp_dropsubscription   
        @subscriber = 'MySubscriber',  
        @publication = 'MyPublication',  
        @article = 'all',  
        @ignore_distributor = 1;  
    ```  
  
     Eseguire **sp_droppublication** per rimuovere tutte le pubblicazioni. Impostare di nuovo il parametro *\@ignore_distributor* su 1 per mantenere i metadati per il database di pubblicazione attivo nel server di distribuzione.  
  
    ```  
    EXEC sys.sp_droppublication   
        @publication = 'MyPublication',  
        @ignore_distributor = 1;  
    ```  
  
     Eseguire **sp_replicationdboption** per disabilitare la replica per il database.  
  
    ```  
    EXEC sys.sp_replicationdboption  
        @dbname = 'MyDBName',  
        @optname = 'publish',  
        @value = 'false';  
    ```  
  
     A questo punto la copia del database pubblicato può essere mantenuta o eliminata.  

## <a name="remove-original-publisher"></a>Rimuovere un server di pubblicazione originale

È possibile che in alcune istanze (sostituzione del server precedente, aggiornamento del sistema operativo e così via) si voglia rimuovere un server di pubblicazione originale da un gruppo di disponibilità Always On. Per rimuovere il server di pubblicazione dal gruppo di disponibilità, attenersi alla procedura descritta in questa sezione. 

Si supponga di avere i server N1, N2 e D1, dove N1 e N2 sono la replica primaria e secondaria del gruppo di disponibilità AG1, N1 è il server di pubblicazione originale di una pubblicazione transazionale e D1 è il server di distribuzione. Si vuole sostituire il server di pubblicazione originale N1 con il nuovo server di pubblicazione N3. 

Per rimuovere il server di pubblicazione, attenersi alla procedura descritta di seguito: 

1. Installare e configurare SQL Server nel nodo N3. La versione di SQL Server deve essere identica a quella del server di pubblicazione originale. 
1. Sul server di distribuzione D1 aggiungere N3 come server di pubblicazione usando [sp_adddistpublisher](../../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md). 
1. Configurare N3 come server di pubblicazione con D1 come server di distribuzione. 
1. Aggiungere N3 come replica al gruppo di disponibilità AG1. 
1. Nella replica N3 verificare che i sottoscrittori push per la pubblicazione siano visualizzati come server collegati. Usare [sp_addlinkedserver](../../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) o SQL Server Management Studio. 
1. Dopo avere eseguito la sincronizzazione di N3, eseguire il failover del gruppo di disponibilità a N3 come primario. 
1. Rimuovere N1 dal gruppo di disponibilità AG1. 

Tenere in considerazione quanto segue:
- Non rimuovere il server remoto del server di pubblicazione originale (N1 in questo caso) o i metadati ad esso associato dal server di distribuzione, anche se non è più possibile accedere al server. I metadati del server per il server di pubblicazione originale sono necessari nel server di distribuzione per fornire i risultati alle query sui metadati di pubblicazione e senza di questi la replica ha esito negativo. 
- Per SQL Server 2014, dopo la rimozione del server di pubblicazione originale, non sarà possibile usare il nome del server di pubblicazione originale per amministrare la replica in Monitoraggio replica. Se si tenta di registrare una o più repliche nuove come server di pubblicazione in Monitoraggio replica, le informazioni non vengono visualizzate poiché non vi sono metadati associati. Per l'amministrazione della replica in questo scenario, è necessario fare clic con il pulsante destro del mouse su singole pubblicazioni e sottoscrizioni in SQL Server Management Studio (SSMS).
- Per SQL Server 2016 SP2-CU3, SQL Server 2017 CU6 e versioni successive, registrare il listener del server di pubblicazione del gruppo di disponibilità in Monitoraggio replica per amministrare la replica usando SQL Server Management Studio versione 17.7 e successive. 
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Configurare la replica per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server.md)  
  
-   [Replica, Rilevamento modifiche, Change Data Capture e Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)  
  
-   [Domande frequenti sull'amministrazione della replica](../../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)  
  
-   [Sottoscrittori della replica e gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Prerequisiti, restrizioni e raccomandazioni per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Gruppi di disponibilità Always On: Interoperabilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)   
 [Replica di SQL Server](../../../relational-databases/replication/sql-server-replication.md)  
  
  
