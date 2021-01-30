---
title: Definire una relazione tra record logici degli articoli di tabelle di merge
description: Informazioni su come definire la relazione dei record logici tra le tabelle correlate usate per gli articoli di replica di tipo merge.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- merge replication logical records [SQL Server replication]
- articles [SQL Server replication], logical records
- logical records [SQL Server replication]
ms.assetid: ff847b3a-c6b0-4eaf-b225-2ffc899c5558
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 397ecaba4cf6b8e56f3dbcb6366dca8121ae345d
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076789"
---
# <a name="define-a-logical-record-relationship-between-merge-table-articles"></a>Definizione di una relazione tra record logici degli articoli di tabelle di merge
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come definire una relazione tra record logici tra articoli di tabella del merge in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o RMO (Replication Management Objects).  
  
 La replica di tipo merge consente di definire una relazione tra righe correlate in tabelle diverse. Queste righe possono quindi essere elaborate come un'unità transazionale durante la sincronizzazione. È possibile definire un record logico tra due articoli indipendentemente dal fatto che per essi sia stata definita una relazione tra filtri di join. Per altre informazioni, vedere [Raggruppare modifiche alle righe correlate con record logici](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
-   **Per definire una relazione tra record logici degli articoli di tabelle di merge, utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Oggetti RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Se si aggiunge, modifica o elimina un record logico dopo che sono state inizializzate sottoscrizioni per la pubblicazione, è necessario generare un nuovo snapshot e reinizializzare tutte le sottoscrizioni in seguito alla modifica. Per altre informazioni sui requisiti per la modifica delle proprietà, vedere [Modificare le proprietà di pubblicazioni e articoli](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 Definire record logici nella finestra di dialogo **Aggiungi join** disponibile nella Creazione guidata nuova pubblicazione e nella finestra di dialogo **Proprietà pubblicazione - \<Publication>** . Per altre informazioni sull'uso della creazione guidata e l'accesso alla finestra di dialogo, vedere [Creare una pubblicazione](../../../relational-databases/replication/publish/create-a-publication.md) e [Visualizzare e modificare le proprietà della pubblicazione](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
 È possibile definire record logici nella finestra di dialogo **Aggiungi join** solo se vengono applicati a un filtro join di una pubblicazione di tipo merge e la pubblicazione soddisfa i requisiti per l'utilizzo di partizioni pre-calcolate. Per definire record logici che non vengono applicati a filtri di join e per impostare il rilevamento e la risoluzione dei conflitti a livello di record logici, è necessario utilizzare le stored procedure.  
  
#### <a name="to-define-a-logical-record-relationship"></a>Per definire una relazione tra record logici  
  
1.  Nella pagina **Filtro righe tabella** della Creazione guidata nuova pubblicazione o nella pagina **Filtro righe** della finestra di dialogo **Proprietà pubblicazione - \<Publication>** selezionare un filtro di riga nel riquadro **Tabelle filtrate**.  
  
     A una relazione tra record logici è associato un filtro join che estende un filtro di riga. È pertanto necessario definire un filtro di riga prima di poter estendere il filtro con un join e applicare una relazione tra record logici. Dopo aver definito un filtro join, è possibile estenderlo con un altro filtro join. Per ulteriori informazioni sulla definizione di filtri join, vedere [Definizione e modifica di un filtro di join tra articoli di merge](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
2.  Fare clic su **Aggiungi** e quindi su **Aggiungi join per estendere il filtro selezionato**.  
  
3.  Nella finestra di dialogo **Aggiungi join** definire un filtro join e quindi selezionare la casella di controllo **Record logico**.  
  
4.  Se è visualizzata la finestra di dialogo **Proprietà pubblicazione - \<Publication>** fare clic su **OK** per salvare e chiudere la finestra di dialogo.  
  
#### <a name="to-delete-a-logical-record-relationship"></a>Per eliminare una relazione tra record logici  
  
-   Eliminare solo la relazione tra record logici oppure eliminare la relazione e il filtro join ad essa associato.  
  
     Per eliminare solo la relazione tra record logici:  
  
    1.  Nella pagina **Filtro righe** della Creazione guidata nuova pubblicazione o nella pagina **Filtro righe** della finestra di dialogo **Proprietà pubblicazione - \<Publication>** selezionare il filtro join associato alla relazione tra record logici nel riquadro **Tabelle filtrate** e quindi fare clic su **Modifica**.  
  
    2.  Nella finestra di dialogo **Modifica join** deselezionare la casella di controllo **Record logico**.  
  
    3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
     Per eliminare la relazione tra record logici e il filtro join ad essa associato:  
  
    -   Nella pagina **Filtro righe** della Creazione guidata nuova pubblicazione o nella finestra di dialogo **Proprietà pubblicazione - \<Publication>** selezionare un filtro nel riquadro **Tabelle filtrate** e quindi fare clic su **Elimina**. Se il filtro di join eliminato è esteso da altri join, anch'essi verranno eliminati.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 Per specificare a livello di programmazione relazioni tra record logici tra gli articoli, è possibile utilizzare stored procedure di replica.  
  
#### <a name="to-define-a-logical-record-relationship-without-an-associated-join-filter"></a>Per definire una relazione tra record logici senza un filtro di join associato  
  
1.  Se la pubblicazione contiene eventuali articoli con filtro, eseguire [sp_helpmergepublication](../../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)e notare il valore di **use_partition_groups** nel set di risultati.  
  
    -   Se il valore è **1**, le partizioni calcolate vengono già utilizzate.  
  
    -   Se il valore è **0**, eseguire [sp_changemergepublication](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md) nel database di pubblicazione del server di pubblicazione. Specificare il valore **use_partition_groups** per **\@property** e il valore **true** per **\@value**.  
  
        > [!NOTE]  
        >  Se la pubblicazione non supporta le partizioni calcolate, non sarà possibile utilizzare i record logici. Per altre informazioni vedere "Requisiti per l'uso delle partizioni pre-calcolate" nell'argomento [Ottimizzare le prestazioni dei filtri con parametri con le partizioni pre-calcolate](../../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).  
  
    -   Se il valore è NULL, è necessario eseguire l'agente snapshot per generare lo snapshot iniziale per la pubblicazione.  
  
2.  Se gli articoli che includeranno il record logico non esistono, eseguire [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) nel database di pubblicazione del server di pubblicazione. Specificare una delle opzioni di rilevamento e risoluzione dei conflitti seguenti per il record logico:  
  
    -   Per rilevare e risolvere conflitti che si verificano all'interno di righe correlate del record logico, specificare il valore **true** per **\@logical_record_level_conflict_detection** e **\@logical_record_level_conflict_resolution**.  
  
    -   Per usare la funzionalità standard di rilevamento e risoluzione dei conflitti a livello di riga o di colonna, specificare il valore **false** per **\@logical_record_level_conflict_detection** e **\@logical_record_level_conflict_resolution**, che corrisponde all'impostazione predefinita.  
  
3.  Ripetere il passaggio 2 per ogni articolo che includerà il record logico. È necessario utilizzare la stessa opzione di rilevamento e risoluzione dei conflitti per ogni articolo del record logico. Per altre informazioni, vedere [Rilevamento e risoluzione dei conflitti nei record logici](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-resolving-in-logical-record.md).  
  
4.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergefilter](../../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md). Specificare **\@publication**, il nome di un articolo della relazione per **\@article**, il nome del secondo articolo per **\@join_articlename**, un nome per la relazione per **\@filtername**, una clausola che definisce la relazione tra i due articoli per **\@join_filterclause**, il tipo di join per **\@join_unique_key** e uno dei valori seguenti per **\@filter_type**:  
  
    -   **2** : consente di definire una relazione logica.  
  
    -   **3** : consente di definire una relazione logica con un filtro join.  
  
    > [!NOTE]  
    >  Se il filtro join non viene utilizzato, la direzione della relazione tra i due articoli non ha rilevanza.  
  
5.  Ripetere il passaggio 2 per tutte le altre relazioni tra record logici incluse nella pubblicazione.  
  
#### <a name="to-change-conflict-detection-and-resolution-for-logical-records"></a>Per modificare l'opzione di rilevamento e risoluzione dei conflitti per i record logici  
  
1.  Per rilevare e risolvere conflitti che si verificano all'interno di righe correlate nel record logico:  
  
    -   Nel database di pubblicazione del server di pubblicazione eseguire [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Specificare il valore **logical_record_level_conflict_detection** per **\@property** e il valore **true** per **\@value**. Specificare il valore **1** per **\@force_invalidate_snapshot** e **\@force_reinit_subscription**.  
  
    -   Nel database di pubblicazione del server di pubblicazione eseguire [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Specificare il valore **logical_record_level_conflict_resolution** per **\@property** e il valore **true** per **\@value**. Specificare il valore **1** per **\@force_invalidate_snapshot** e **\@force_reinit_subscription**.  
  
2.  Per utilizzare l'opzione standard di rilevamento e risoluzione dei conflitti a livello di riga o di colonna:  
  
    -   Nel database di pubblicazione del server di pubblicazione eseguire [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Specificare il valore **logical_record_level_conflict_detection** per **\@property** e il valore **false** per **\@value**. Specificare il valore **1** per **\@force_invalidate_snapshot** e **\@force_reinit_subscription**.  
  
    -   Nel database di pubblicazione del server di pubblicazione eseguire [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Specificare il valore **logical_record_level_conflict_resolution** per **\@property** e il valore **false** per **\@value**. Specificare il valore **1** per **\@force_invalidate_snapshot** e **\@force_reinit_subscription**.  
  
#### <a name="to-remove-a-logical-record-relationship"></a>Per rimuovere una relazione tra record logici  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire la query seguente per restituire informazioni su tutte le relazioni tra record logici definite per la pubblicazione specificata:  
  
     [!code-sql[HowTo#sp_ReturnMergeLogicalRecords](../../../relational-databases/replication/codesnippet/tsql/define-a-logical-record-_1.sql)]  
  
     Si noti il nome della relazione tra record logici da rimuovere nella colonna **filtername** del set di risultati.  
  
    > [!NOTE]  
    >  Questa query restituisce le stesse informazioni di [sp_helpmergefilter](../../../relational-databases/system-stored-procedures/sp-helpmergefilter-transact-sql.md). La stored procedure di sistema restituisce tuttavia solo le informazioni sulle relazioni tra record logici che corrispondono anche a filtri join.  
  
2.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_dropmergefilter](../../../relational-databases/system-stored-procedures/sp-dropmergefilter-transact-sql.md). Specificare **\@publication**, il nome di uno degli articoli della relazione per **\@article** e il nome della relazione ottenuto nel passaggio 1 per **\@filtername**.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Esempio (Transact-SQL)  
 In questo esempio le partizioni pre-calcolate vengono abilitate in una pubblicazione esistente e viene creato un record logico che comprende i due nuovi articoli per le tabelle `SalesOrderHeader` e `SalesOrderDetail` .  
  
 [!code-sql[HowTo#sp_AddMergeLogicalRecord](../../../relational-databases/replication/codesnippet/tsql/define-a-logical-record-_2.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilizzo di RMO (Replication Management Objects)  
  
> [!NOTE]  
>  La replica di tipo merge consente di specificare che i conflitti vengano rilevati e risolti a livello di record logico. Queste opzioni tuttavia non possono essere impostate tramite RMO.  
  
#### <a name="to-define-a-logical-record-relationship-without-an-associated-join-filter"></a>Per definire una relazione tra record logici senza un filtro di join associato  
  
1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergePublication> , impostare le proprietà <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> e <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> per la pubblicazione, quindi impostare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sulla connessione creata al passaggio 1.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per recuperare le proprietà dell'oggetto. Se questo metodo restituisce **false**, le proprietà della pubblicazione sono state definite in modo non corretto nel passaggio 2 oppure la pubblicazione non esiste.  
  
4.  Se la proprietà <xref:Microsoft.SqlServer.Replication.MergePublication.PartitionGroupsOption%2A> è impostata su <xref:Microsoft.SqlServer.Replication.PartitionGroupsOption.False>, impostarla su <xref:Microsoft.SqlServer.Replication.PartitionGroupsOption.True>.  
  
5.  Se gli articoli che dovranno includere il record logico non esistono, creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergeArticle> e impostare le proprietà seguenti:  
  
    -   Nome dell'articolo per <xref:Microsoft.SqlServer.Replication.Article.Name%2A>.  
  
    -   Nome della pubblicazione per <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>.  
  
    -   (Facoltativo) Se l'articolo è filtrato orizzontalmente, specificare la clausola del filtro di riga per la proprietà <xref:Microsoft.SqlServer.Replication.MergeArticle.FilterClause%2A> . Utilizzare questa proprietà per specificare un filtro di riga statico o con parametri. Per altre informazioni sui filtri di riga con parametri, vedere [Filtri di riga con parametri](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
     Per altre informazioni, vedere [definire un articolo](../../../relational-databases/replication/publish/define-an-article.md).  
  
6.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.Article.Create%2A> .  
  
7.  Ripetere i passaggi 5 e 6 per ogni articolo che includerà il record logico.  
  
8.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergeJoinFilter> per definire la relazione tra record logici tra gli articoli. Impostare quindi le proprietà seguenti:  
  
    -   Nome dell'articolo figlio nella relazione tra record logici per la proprietà <xref:Microsoft.SqlServer.Replication.MergeJoinFilter.ArticleName%2A> .  
  
    -   Nome dell'articolo padre esistente nella relazione tra record logici per la proprietà <xref:Microsoft.SqlServer.Replication.MergeJoinFilter.JoinArticleName%2A> .  
  
    -   Nome per la relazione tra record logici per la proprietà <xref:Microsoft.SqlServer.Replication.MergeJoinFilter.FilterName%2A> .  
  
    -   Espressione che definisce la relazione per la proprietà <xref:Microsoft.SqlServer.Replication.MergeJoinFilter.JoinFilterClause%2A> .  
  
    -   Valore di <xref:Microsoft.SqlServer.Replication.FilterTypes.LogicalRecordLink> per la proprietà <xref:Microsoft.SqlServer.Replication.MergeJoinFilter.FilterTypes%2A> . Se la relazione tra record logici è anche un filtro join, specificare un valore di <xref:Microsoft.SqlServer.Replication.FilterTypes.JoinFilterAndLogicalRecordLink> per questa proprietà. Per altre informazioni, vedere [Raggruppare modifiche alle righe correlate con record logici](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
9. Chiamare il metodo <xref:Microsoft.SqlServer.Replication.MergeArticle.AddMergeJoinFilter%2A> sull'oggetto che rappresenta l'articolo figlio nella relazione. Passare l'oggetto <xref:Microsoft.SqlServer.Replication.MergeJoinFilter> indicato nel passaggio 8 per definire la relazione.  
  
10. Ripetere i passaggi 8 e 9 per tutte le altre relazioni tra record logici incluse nella pubblicazione.  
  
###  <a name="example-rmo"></a><a name="PShellExample"></a> Esempio (RMO)  
 In questo esempio viene creato un record logico che comprende i due nuovi articoli per le tabelle `SalesOrderHeader` e `SalesOrderDetail` .  
  
 [!code-cs[HowTo#rmo_CreateLogicalRecord](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createlogicalrecord)]  
  
 [!code-vb[HowTo#rmo_vb_CreateLogicalRecord](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createlogicalrecord)]  
  
## <a name="see-also"></a>Vedere anche  
 [Definizione e modifica di un filtro di join tra articoli di merge](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)   
 [Definizione e modifica di un filtro di riga con parametri per un articolo di merge](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)   
 [Definire e modificare un filtro di riga statico](../../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md)   
 [Raggruppare modifiche alle righe correlate con record logici](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md)   
 [Ottimizzare le prestazioni dei filtri con parametri con le partizioni pre-calcolate](../../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md)   
 [Raggruppare modifiche alle righe correlate con record logici](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md)  
  
  
