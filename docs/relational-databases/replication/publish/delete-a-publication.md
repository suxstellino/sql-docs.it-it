---
title: Eliminare una pubblicazione | Microsoft Docs
description: Informazioni su come eliminare una pubblicazione in SQL Server tramite SQL Server Management Studio, Transact-SQL o Replication Management Objects.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- removing publications
- publications [SQL Server replication], deleting
- articles [SQL Server replication], deleting
- deleting publications
ms.assetid: 408a1360-12ee-4896-ac94-482ae839593b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: b1a87a8f9c2344b21cd67a24c35ab33615965300
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076729"
---
# <a name="delete-a-publication"></a>Eliminazione di una pubblicazione
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  In questo argomento viene descritto come eliminare una pubblicazione in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o RMO (Replication Management Objects).  
  
 **Contenuto dell'articolo**  
  
-   **Per eliminare una pubblicazione, utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Oggetti RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 Eliminare le pubblicazioni dalla cartella **Pubblicazioni locali** in [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
#### <a name="to-delete-a-publication"></a>Per eliminare una pubblicazione  
  
1.  Connettersi al server di pubblicazione in [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]e quindi espandere il nodo del server.  
  
2.  Espandere la cartella **Replica** e quindi la cartella **Pubblicazioni locali** .  
  
3.  Fare clic con il pulsante destro del mouse sulla pubblicazione che si desidera eliminare e quindi scegliere **Elimina**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 È possibile eliminare pubblicazioni a livello di programmazione tramite le stored procedure di replica. Le stored procedure utilizzate dipendono dal tipo di pubblicazione eliminato.  
  
> [!NOTE]  
>  L'eliminazione di una pubblicazione non comporta la rimozione degli oggetti pubblicati dal database di pubblicazione o degli oggetti corrispondenti dal database di sottoscrizione. Utilizzare il comando `DROP <object>` per rimuovere manualmente questi oggetti, se necessario.  
  
#### <a name="to-delete-a-snapshot-or-transactional-publication"></a>Per eliminare una pubblicazione snapshot o transazionale  
  
1.  Eseguire una delle operazioni seguenti:  
  
    -   Per eliminare una singola pubblicazione, eseguire [sp_droppublication](../../../relational-databases/system-stored-procedures/sp-droppublication-transact-sql.md) nel database di pubblicazione del server di pubblicazione.  
  
    -   Per eliminare tutte le pubblicazioni e rimuovere tutti gli oggetti di replica da un database pubblicato, eseguire [sp_removedbreplication](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) nel server di pubblicazione. Specificare il valore **tran** per **\@type**. (Facoltativo) Se il server di distribuzione non è accessibile oppure se lo stato del database è sospetto oppure offline, specificare il valore **1** per **\@force**. (Facoltativo) Specificare il nome del database per **\@dbname** se [sp_removedbreplication](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) non viene eseguita nel database di pubblicazione.  
  
        > [!NOTE]  
        >  Specificando il valore **1** per **\@force**, è possibile che nel database rimangano oggetti di pubblicazione correlati alla replica.  
  
2.  (Facoltativo) Se il database non contiene altre pubblicazioni, eseguire [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) per disabilitare la pubblicazione del database corrente usando la replica snapshot o transazionale.  
  
3.  (Facoltativo) Nel database di sottoscrizione del Sottoscrittore eseguire [sp_subscription_cleanup](../../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md) per rimuovere gli eventuali metadati di replica rimanenti nel database di sottoscrizione.  
  
#### <a name="to-delete-a-merge-publication"></a>Per eliminare una pubblicazione di tipo merge  
  
1.  Eseguire una delle operazioni seguenti:  
  
    -   Per eliminare una singola pubblicazione, eseguire [sp_dropmergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-dropmergepublication-transact-sql.md) nel database di pubblicazione del server di pubblicazione.  
  
    -   Per eliminare tutte le pubblicazioni e rimuovere tutti gli oggetti di replica da un database pubblicato, eseguire [sp_removedbreplication](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) nel server di pubblicazione. Specificare il valore **merge** per **\@type**. (Facoltativo) Se il server di distribuzione non è accessibile oppure se lo stato del database è sospetto oppure offline, specificare il valore **1** per **\@force**. (Facoltativo) Specificare il nome del database per **\@dbname** se [sp_removedbreplication](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) non viene eseguita nel database di pubblicazione.  
  
        > [!NOTE]  
        >  Specificando il valore **1** per **\@force**, è possibile che nel database rimangano oggetti di pubblicazione correlati alla replica.  
  
2.  (Facoltativo) Se il database non contiene altre pubblicazioni, eseguire [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) per disabilitare la pubblicazione del database corrente usando la replica di tipo merge.  
  
3.  (Facoltativo) Nel database di sottoscrizione del Sottoscrittore eseguire [sp_mergesubscription_cleanup &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-mergesubscription-cleanup-transact-sql.md) per rimuovere gli eventuali metadati di replica rimanenti nel database di sottoscrizione.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Esempi (Transact-SQL)  
 In questo esempio viene illustrato come rimuovere una pubblicazione transazionale e disabilitare la pubblicazione transazionale per un database. Si presuppone che in precedenza siano state rimosse tutte le sottoscrizioni. Per ulteriori informazioni, vedere [Delete a Pull Subscription](../../../relational-databases/replication/delete-a-pull-subscription.md) o [Delete a Push Subscription](../../../relational-databases/replication/delete-a-push-subscription.md).  
  
 [!code-sql[HowTo#sp_droppublication](../../../relational-databases/replication/codesnippet/tsql/delete-a-publication_1.sql)]  
  
 In questo esempio viene illustrato come rimuovere una pubblicazione di tipo merge e disabilitare la pubblicazione di tipo merge per un database. Si presuppone che in precedenza siano state rimosse tutte le sottoscrizioni. Per ulteriori informazioni, vedere [Delete a Pull Subscription](../../../relational-databases/replication/delete-a-pull-subscription.md) o [Delete a Push Subscription](../../../relational-databases/replication/delete-a-push-subscription.md).  
  
 [!code-sql[HowTo#sp_dropmergepublication](../../../relational-databases/replication/codesnippet/tsql/delete-a-publication_2.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilizzo di RMO (Replication Management Objects)  
 È possibile eliminare pubblicazioni a livello di programmazione tramite gli oggetti RMO (Replication Management Objects). Le classi RMO utilizzate per rimuovere una pubblicazione dipendono dal tipo di pubblicazione rimossa.  
  
#### <a name="to-remove-a-snapshot-or-transactional-publication"></a>Per rimuovere una pubblicazione snapshot o transazionale  
  
1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.TransPublication>.  
  
3.  Impostare le proprietà <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> e <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> per la pubblicazione, quindi impostare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sulla connessione creata nel passaggio 1.  
  
4.  Controllare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> per verificare che la pubblicazione sia esistente. Se il valore di questa proprietà è **false**, le proprietà di pubblicazione sono state definite in modo non corretto nel passaggio 3 oppure la pubblicazione non esiste.  
  
5.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.Publication.Remove%2A> .  
  
6.  (Facoltativo) Se per il database non esistono altre pubblicazioni transazionali, è possibile disabilitare il database per la pubblicazione transazionale come illustrato di seguito:  
  
    1.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.ReplicationDatabase>. Impostare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sull'istanza di <xref:Microsoft.SqlServer.Management.Common.ServerConnection> restituita al passaggio 1.  
  
    2.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Se il metodo restituisce **false**, verificare che il database esista.  
  
    3.  Impostare le proprietà <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.EnabledTransPublishing%2A> su **false**.  
  
    4.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> .  
  
7.  Chiudere le connessioni.  
  
#### <a name="to-remove-a-merge-publication"></a>Per rimuovere una pubblicazione di tipo merge  
  
1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergePublication>.  
  
3.  Impostare le proprietà <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> e <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> per la pubblicazione, quindi impostare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sulla connessione creata nel passaggio 1.  
  
4.  Controllare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> per verificare che la pubblicazione sia esistente. Se il valore di questa proprietà è **false**, le proprietà di pubblicazione sono state definite in modo non corretto nel passaggio 3 oppure la pubblicazione non esiste.  
  
5.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.Publication.Remove%2A> .  
  
6.  (Facoltativo) Se per il database non esistono altre pubblicazioni di tipo merge, è possibile disabilitare il database per la pubblicazione di tipo merge come illustrato di seguito:  
  
    1.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.ReplicationDatabase>. Impostare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sull'istanza di <xref:Microsoft.SqlServer.Management.Common.ServerConnection> restituita al passaggio 1.  
  
    2.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Se il metodo restituisce **false**, verificare che il database esista.  
  
    3.  Impostare le proprietà <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.EnabledMergePublishing%2A> su **false**.  
  
    4.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> .  
  
7.  Chiudere le connessioni.  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Esempi (RMO)  
 Nell'esempio seguente viene eliminata una pubblicazione transazionale. Se per il database non esistono altre pubblicazioni transazionali, verrà anche disabilitata la pubblicazione transazionale.  
  
 [!code-cs[HowTo#rmo_DropTranPub](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_droptranpub)]  
  
 [!code-vb[HowTo#rmo_vb_DropTranPub](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_droptranpub)]  
  
 Nell'esempio seguente viene eliminata una pubblicazione di tipo merge. Se per il database non esistono altre pubblicazioni di tipo merge, verrà anche disabilitata la pubblicazione di tipo merge.  
  
 [!code-cs[HowTo#rmo_DropMergePub](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_dropmergepub)]  
  
 [!code-vb[HowTo#rmo_vb_DropMergePub](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_dropmergepub)]  
  
## <a name="see-also"></a>Vedere anche  
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Pubblicare dati e oggetti di database](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
