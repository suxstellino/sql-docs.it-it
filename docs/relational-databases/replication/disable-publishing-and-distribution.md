---
title: Disabilitare la pubblicazione e la distribuzione | Microsoft Docs
description: Informazioni su come disabilitare la pubblicazione e la distribuzione in SQL Server tramite SQL Server Management Studio, Transact-SQL o Replication Management Objects.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- disabling publishing
- publishing [SQL Server replication], disabling
- distribution disabling [SQL Server replication]
- removing replication
- replication [SQL Server], removing
- disabling replication
- disabling distribution
ms.assetid: 6d4a1474-4d13-4826-8be2-80050fafa8a5
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: c5ac700e65cc251f43afecaa910c3af67fcbdbdd
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076483"
---
# <a name="disable-publishing-and-distribution"></a>Disabilitazione della pubblicazione e della distribuzione
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  In questo argomento viene descritto come disabilitare la pubblicazione e la distribuzione in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] by using [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o RMO (Replication Management Objects).  
  
 È possibile eseguire le operazioni seguenti:  
  
-   Eliminare tutti i database di distribuzione dal database di distribuzione.  
  
-   Disabilitare tutti i server di pubblicazione che utilizzano il server di distribuzione ed eliminare da tali server tutte le pubblicazioni.  
  
-   Eliminare tutte le sottoscrizioni delle pubblicazioni. I dati dei database di pubblicazione e sottoscrizione non vengono eliminati, ma la relazione di sincronizzazione con i database di pubblicazione andrà perduta. L'eliminazione dei dati nel Sottoscrittore deve essere eseguita in modo manuale.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Prerequisiti](#Prerequisites)  
  
-   **Per disabilitare la pubblicazione e la distribuzione, utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Oggetti RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
  
-   Per disabilitare la pubblicazione e la distribuzione, è necessario che tutti i database di distribuzione e pubblicazione siano online. Se esistono *snapshot del database* per i database di distribuzione o di pubblicazione, è necessario eliminarli prima di disabilitare la pubblicazione e la distribuzione. Uno snapshot di database rappresenta una copia offline di sola lettura di un database e non è correlato a uno snapshot di replica. Per altre informazioni, vedere [Snapshot del database &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 È possibile disabilitare la pubblicazione e la distribuzione utilizzando la Disabilitazione guidata pubblicazione e distribuzione.  
  
#### <a name="to-disable-publishing-and-distribution"></a>Per disabilitare la pubblicazione e la distribuzione  
  
1.  Connettersi al server di pubblicazione o al server di distribuzione da disabilitare in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] e quindi espandere il nodo del server.  
  
2.  Fare clic con il pulsante destro del mouse sulla cartella **Replica** e quindi scegliere **Disabilita pubblicazione e distribuzione**.  
  
3.  Eseguire i vari passaggi della Disabilitazione guidata pubblicazione e distribuzione.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 La pubblicazione e la distribuzione della replica possono essere disabilitate a livello di programmazione tramite le stored procedure di replica.  
  
#### <a name="to-disable-publishing-and-distribution"></a>Per disabilitare la pubblicazione e la distribuzione  
  
1.  Arrestare tutti i processi correlati alla replica. Per un elenco di nomi di processo, vedere la sezione "Sicurezza agente in SQL Server Agent" sezione di [Modello di sicurezza dell'agente di replica](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
2.  Nel database di sottoscrizione di ogni Sottoscrittore eseguire [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) per rimuovere gli oggetti di replica dal database. Questa stored procedure non rimuoverà i processi di replica nel server di distribuzione.  
  
3.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) per rimuovere gli oggetti di replica dal database.  
  
4.  Se il database di pubblicazione utilizza un server di distribuzione remoto, eseguire [sp_dropdistributor](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md).  
  
5.  Nel server di distribuzione eseguire [sp_dropdistpublisher](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md). Questa stored procedure deve essere eseguita una volta per ogni server di pubblicazione registrato nel server di distribuzione.  
  
6.  Nel database di distribuzione eseguire [sp_dropdistributiondb](../../relational-databases/system-stored-procedures/sp-dropdistributiondb-transact-sql.md) per eliminare il database di distribuzione. Questa stored procedure deve essere eseguita una volta per ogni server di pubblicazione registrato nel server di distribuzione. Verranno anche rimossi gli eventuali processi dell'agente di lettura coda associati al database di distribuzione.  
  
7.  Nel server di distribuzione eseguire [sp_dropdistributor](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md) per rimuovere la designazione di server di distribuzione dal server.  
  
    > [!NOTE]  
    > Se prima dell'esecuzione di [sp_dropdistpublisher](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md) e [sp_dropdistributor](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md)non vengono eliminati tutti gli oggetti di pubblicazione e distribuzione della replica, queste procedure restituiranno un errore. Per eliminare tutti gli oggetti correlati alla replica quando un server di pubblicazione o un server di distribuzione viene eliminato, è necessario impostare il parametro `@no_checks` su **1**. Se un server di pubblicazione o un database di distribuzione è offline o non è raggiungibile, il parametro `@ignore_distributor` può essere impostato su **1** in modo che possa essere eliminato. Tuttavia, gli eventuali oggetti di pubblicazione e distribuzione rimasti devono essere rimossi manualmente.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Esempi (Transact-SQL)  
 In questo esempio di script vengono rimossi oggetti della replica dal database di sottoscrizione.  
  
 [!code-sql[HowTo#sp_removedbreplication](../../relational-databases/replication/codesnippet/tsql/disable-publishing-and-d_1.sql)]  
  
 In questo esempio di script vengono disabilitate la pubblicazione e la distribuzione in un server che è un server di pubblicazione e un database di distribuzione e il database di distribuzione viene eliminato.  
  
 [!code-sql[HowTo#sp_DropDistPub](../../relational-databases/replication/codesnippet/tsql/disable-publishing-and-d_2.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilizzo di RMO (Replication Management Objects)  
  
#### <a name="to-disable-publishing-and-distribution"></a>Per disabilitare la pubblicazione e la distribuzione  
  
1.  Rimuovere tutte le sottoscrizioni di pubblicazioni che utilizzano il database di distribuzione. Per ulteriori informazioni, vedere [Delete a Pull Subscription](../../relational-databases/replication/delete-a-pull-subscription.md) e [Delete a Push Subscription](../../relational-databases/replication/delete-a-push-subscription.md).  
  
2.  Rimuovere tutte le pubblicazioni che utilizzano il server di distribuzione e disabilitare la pubblicazione per tutti i database se il server di pubblicazione e il server di distribuzione si trovano nello stesso server. Per altre informazioni, vedere [Delete a Publication](../../relational-databases/replication/publish/delete-a-publication.md).  
  
3.  Creare una connessione al server di distribuzione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
4.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.DistributionPublisher>. Specificare la proprietà <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Name%2A> e passare l'oggetto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> ottenuto al passaggio 3.  
  
5.  (Facoltativo) Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per ottenere le proprietà dell'oggetto e verificare che il server di pubblicazione esista. Se il metodo restituisce **false**, il nome del server di pubblicazione impostato al passaggio 4 non è corretto oppure il server di pubblicazione non è utilizzato da questo server di distribuzione.  
  
6.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Remove%2A> . Passare il valore **true** per *force* se il server di pubblicazione e il server di distribuzione si trovano in server diversi e quando il server di pubblicazione deve essere disinstallato dal server di distribuzione senza prima verificare non esistano più pubblicazioni nel server di pubblicazione.  
  
7.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.ReplicationServer>. Passare l'oggetto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> indicato nel passaggio 3.  
  
8.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationServer.UninstallDistributor%2A> . Passare il valore **true** affinché *force* rimuova tutti gli oggetti di replica nel database di distribuzione senza prima verificare che tutti i database di pubblicazione locali siano stati disabilitati e i database di distribuzione siano stati disinstallati.  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Esempi (RMO)  
 In questo esempio viene rimossa la registrazione del server di pubblicazione nel database di distribuzione, viene eliminato il database di distribuzione e viene disinstallato il database di distribuzione.  
  
 [!code-cs[HowTo#rmo_DropDistPub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_dropdistpub)]  
  
 [!code-vb[HowTo#rmo_vb_DropDistPub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_dropdistpub)]  
  
 In questo esempio viene disinstallato il server di distribuzione senza prima disabilitare i database di pubblicazione locali o eliminare il database di distribuzione.  
  
 [!code-cs[HowTo#rmo_DropDistPubForce](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_dropdistpubforce)]  
  
 [!code-vb[HowTo#rmo_vb_DropDistPubForce](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_dropdistpubforce)]  
  
## <a name="see-also"></a>Vedere anche  
 [Replication Management Objects Concepts](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Replication System Stored Procedures Concepts](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)  
  
  
