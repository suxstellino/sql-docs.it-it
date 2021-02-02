---
description: Visualizzazione e modifica delle proprietà delle sottoscrizioni pull
title: Visualizzare e modificare le proprietà delle sottoscrizioni pull | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- modifying subscriptions
- viewing replication properties
- modifying replication properties, pull subscriptions
- pull subscriptions [SQL Server replication], modifying
- subscriptions [SQL Server replication], pull
- pull subscriptions [SQL Server replication], properties
- modifying subscriptions, SQL Server Management Studio
ms.assetid: 1601e54f-86f0-49e8-b023-87a5d1def033
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 91e312326e1d45ad4c5d747133baaa7c2c25e5d5
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250544"
---
# <a name="view-and-modify-pull-subscription-properties"></a>Visualizzazione e modifica delle proprietà delle sottoscrizioni pull
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  In questo argomento viene descritto come modificare le proprietà delle sottoscrizioni pull in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o RMO (Replication Management Objects).  
  
 **Contenuto dell'articolo**  
  
-   **Per visualizzare e modificare le proprietà delle sottoscrizioni pull tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Oggetti RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 Visualizzare le proprietà delle sottoscrizioni pull dal server di pubblicazione o dal Sottoscrittore nella finestra di dialogo **Proprietà sottoscrizione - \<Publisher>: \<PublicationDatabase>** , disponibile in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Nel Sottoscrittore è disponibile un numero maggiore di proprietà ed è inoltre possibile modificare le proprietà. Le proprietà possono inoltre essere visualizzate sul server di pubblicazione nella scheda **Tutte le sottoscrizioni** , disponibile in Monitoraggio replica. Per informazioni sull'avvio di Monitoraggio replica, vedere [Avviare Monitoraggio replica](../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
#### <a name="to-view-pull-subscription-properties-from-the-publisher-in-management-studio"></a>Per visualizzare le proprietà delle sottoscrizioni pull dal server di pubblicazione in Management Studio  
  
1.  Connettersi al server di pubblicazione in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]e quindi espandere il nodo del server.  
  
2.  Espandere la cartella **Replica** e quindi la cartella **Pubblicazioni locali** .  
  
3.  Espandere la pubblicazione appropriata, fare clic con il pulsante destro del mouse su una sottoscrizione, quindi su **Proprietà**.  
  
4.  Visualizzare le proprietà e quindi fare clic su **OK**.  

#### <a name="to-view-and-modify-pull-subscription-properties-from-the-subscriber-in-management-studio"></a>Per visualizzare e modificare le proprietà delle sottoscrizioni pull dal Sottoscrittore in Management Studio  
  
1.  Connettersi al Sottoscrittore in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]e quindi espandere il nodo del server.  
  
2.  Espandere la cartella **Replica** e quindi la cartella **Sottoscrizioni locali** .  
  
3.  Fare clic con il pulsante destro del mouse su una sottoscrizione e quindi scegliere **Proprietà**.  
  
4.  Se necessario, modificare le proprietà e quindi fare clic su **OK**.  
  
#### <a name="to-view-pull-subscription-properties-from-the-publisher-in-replication-monitor"></a>Per visualizzare le proprietà delle sottoscrizioni pull dal server di pubblicazione in Monitoraggio replica  
  
1.  Espandere un gruppo di server di pubblicazione nel riquadro a sinistra di Monitoraggio replica, espandere un server di pubblicazione e quindi fare clic su una pubblicazione.  
  
2.  Fare clic sulla scheda **Tutte le sottoscrizioni** .  
  
3.  Fare clic con il pulsante destro del mouse su una sottoscrizione e quindi scegliere **Proprietà**.  
  
4.  Visualizzare le proprietà e quindi fare clic su **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 È possibile modificare le sottoscrizioni pull e accedere alle relative proprietà a livello di programmazione utilizzando stored procedure di replica. Le stored procedure utilizzate dipendono dal tipo di pubblicazione a cui appartiene la sottoscrizione.  
  
#### <a name="to-view-the-properties-of-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>Per visualizzare le proprietà di una sottoscrizione pull di una pubblicazione snapshot o transazionale  
  
1.  Nel Sottoscrittore eseguire [sp_helppullsubscription](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md). Specificare `@publisher`, `@publisher_db`e `@publication`. In tal modo verranno restituite le informazioni sulla sottoscrizione archiviate nelle tabelle di sistema del Sottoscrittore.  
  
2.  Nel Sottoscrittore eseguire [sp_helpsubscription_properties](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md). Specificare `@publisher`, `@publisher_db`, `@publication`, nonché uno dei seguenti valori per `@publication_type`:  
  
    -   **0** : la sottoscrizione appartiene a una pubblicazione transazionale.  
  
    -   **1** : la sottoscrizione appartiene a una pubblicazione snapshot.  
  
3.  Nel server di pubblicazione eseguire [sp_helpsubscription](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md). Specificare `@publication` e `@subscriber`.  
  
4.  Nel server di pubblicazione eseguire [sp_helpsubscriberinfo](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md), specificando `@subscriber`. In tal modo verranno visualizzate le informazioni sul Sottoscrittore.  
  
#### <a name="to-change-the-properties-of-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>Per modificare le proprietà di una sottoscrizione pull di una pubblicazione snapshot o transazionale  
  
1.  Nel Sottoscrittore eseguire [sp_change_subscription_properties](../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md), specificando `@publisher`, `@publisher_db`, `@publication`, il valore **0** (transazionale) o **1** (snapshot) per `@publication_type`, la proprietà della sottoscrizione da modificare come `@property`e il nuovo valore come `@value`.  
  
2.  (Facoltativo) Nel database di sottoscrizione del Sottoscrittore eseguire [sp_changesubscriptiondtsinfo](../../relational-databases/system-stored-procedures/sp-changesubscriptiondtsinfo-transact-sql.md). Specificare l'ID del processo dell'agente di distribuzione per `@jobid` e le proprietà del pacchetto DTS (Data Transformation Services) seguenti:  
  
    -   `@dts_package_name`  
  
    -   `dts_package_password`  
  
    -   `@dts_package_location`  
  
     In questo modo le proprietà del pacchetto DTS di una sottoscrizione verranno modificate.  
  
    > [!NOTE]  
    >  Per ottenere l'ID del processo, eseguire [sp_helpsubscription](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md).  
  
#### <a name="to-view-the-properties-of-a-pull-subscription-to-a-merge-publication"></a>Per visualizzare le proprietà di una sottoscrizione pull di una pubblicazione di tipo merge  
  
1.  Nel Sottoscrittore eseguire [sp_helpmergepullsubscription](../../relational-databases/system-stored-procedures/sp-helpmergepullsubscription-transact-sql.md). Specificare `@publisher`, `@publisher_db`e `@publication`.  
  
2.  Nel Sottoscrittore eseguire [sp_helpsubscription_properties](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md). Specificare `@publisher`, `@publisher_db`, `@publication` e il valore 2 per `@publication_type`.  
  
3.  Nel server di pubblicazione eseguire [sp_helpmergesubscription](../../relational-databases/system-stored-procedures/sp-helpmergesubscription-transact-sql.md) per visualizzare le informazioni sulla sottoscrizione. Per restituire informazioni su una sottoscrizione specifica, è necessario specificare `@publication`, `@subscriber` e il valore **pull** per @subscription_type.  
  
4.  Nel server di pubblicazione eseguire [sp_helpsubscriberinfo](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md), specificando `@subscriber`. In tal modo verranno visualizzate le informazioni sul Sottoscrittore.  
  
#### <a name="to-change-the-properties-of-a-pull-subscription-to-a-merge-publication"></a>Per modificare le proprietà di una sottoscrizione pull di una pubblicazione di tipo merge  
  
1.  Nel Sottoscrittore eseguire [sp_changemergepullsubscription](../../relational-databases/system-stored-procedures/sp-changemergepullsubscription-transact-sql.md). Specificare `@publication`, `@publisher`, `@publisher_db`, la proprietà della sottoscrizione da modificare come `@property` e il nuovo valore come `@value`.  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilizzo di RMO (Replication Management Objects)  
 Le classi RMO utilizzate per la visualizzazione o la modifica delle proprietà di una sottoscrizione pull dipendono dal tipo di pubblicazione per cui viene creata la sottoscrizione pull.  
  
#### <a name="to-view-or-modify-properties-of-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>Per visualizzare o modificare le proprietà di una sottoscrizione pull di una pubblicazione snapshot o transazionale  
  
1.  Creare una connessione al Sottoscrittore tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.TransPullSubscription>.  
  
3.  Impostare le proprietà <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>e <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A> .  
  
4.  Impostare la connessione del passaggio 1 per la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per recuperare le proprietà dell'oggetto. Se questo metodo restituisce **false**, le proprietà della sottoscrizione sono state definite in modo non corretto nel passaggio 3 oppure la sottoscrizione non esiste nel server.  
  
6.  (Facoltativo) Per modificare le proprietà, specificare un nuovo valore per una delle proprietà dell'oggetto <xref:Microsoft.SqlServer.Replication.TransPullSubscription> che è possibile impostare, quindi chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> .  
  
7.  (Facoltativo) Per visualizzare le nuove impostazioni, chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.Refresh%2A> per ricaricare le proprietà per l'articolo.  
  
8.  Chiudere tutte le connessioni.  
  
#### <a name="to-view-or-modify-properties-of-a-pull-subscription-to-a-merge-publication"></a>Per visualizzare o modificare le proprietà di una sottoscrizione pull di una pubblicazione di tipo merge  
  
1.  Creare una connessione al Sottoscrittore tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergePullSubscription>.  
  
3.  Impostare le proprietà <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>e <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A> .  
  
4.  Impostare la connessione del passaggio 1 per la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per recuperare le proprietà dell'oggetto. Se questo metodo restituisce **false**, le proprietà della sottoscrizione sono state definite in modo non corretto nel passaggio 3 oppure la sottoscrizione non esiste nel server.  
  
6.  (Facoltativo) Per modificare le proprietà, specificare un nuovo valore per una delle proprietà dell'oggetto <xref:Microsoft.SqlServer.Replication.MergePullSubscription> che è possibile impostare, quindi chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> .  
  
7.  (Facoltativo) Per visualizzare le nuove impostazioni, chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.Refresh%2A> per ricaricare le proprietà per l'articolo.  
  
8.  Chiudere tutte le connessioni.  
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare le informazioni ed eseguire attività usando Monitoraggio replica](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)   
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Sottoscrizione delle pubblicazioni](../../relational-databases/replication/subscribe-to-publications.md)  
  
  
