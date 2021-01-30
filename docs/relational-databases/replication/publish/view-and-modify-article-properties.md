---
description: Visualizzazione e modifica delle proprietà degli articoli
title: Visualizzare e modificare le proprietà degli articoli | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- sp_changearticle
- sp_helparticle
- viewing replication properties
- sp_changemergearticle
- sp_helpmergearticle
- modifying replication properties, articles
- articles [SQL Server replication], modifying
- articles [SQL Server replication], properties
ms.assetid: e71831fa-3d39-4e4a-9706-4d3a497082cc
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 8901c5e21f257d9b84e625c7f59a7d13c899bd1b
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076979"
---
# <a name="view-and-modify-article-properties"></a>Visualizzazione e modifica delle proprietà degli articoli
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  In questo argomento viene descritto come modificare le proprietà degli articoli in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o RMO (Replication Management Objects).  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Indicazioni](#Recommendations)  
  
-   **Per visualizzare e modificare le proprietà degli articoli, utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Oggetti RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Alcune proprietà non possono essere modificate dopo la creazione di una pubblicazione, mentre altre proprietà non possono essere modificate se sono presenti sottoscrizioni alla pubblicazione. Le proprietà che non possono essere modificate vengono visualizzate come di sola lettura.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Dopo la creazione di una pubblicazione, per alcune modifiche delle proprietà è necessario un nuovo snapshot. Se la pubblicazione dispone di sottoscrizioni, per alcune modifiche è inoltre necessario reinizializzare tutte le sottoscrizioni. Per altre informazioni, vedere [Modificare le proprietà di pubblicazioni e articoli](../../../relational-databases/replication/publish/change-publication-and-article-properties.md) e [Aggiungere ed eliminare articoli in pubblicazioni esistenti](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 Le proprietà degli articoli possono essere visualizzate e modificate nella finestra di dialogo **Proprietà pubblicazione - \<Publication>** , disponibile in [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] e Monitoraggio replica. Per informazioni sull'avvio di Monitoraggio replica, vedere [Avviare Monitoraggio replica](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
-   Nella pagina **Generale** sono presenti il nome e la descrizione della pubblicazione, il nome del database, il tipo di pubblicazione e le impostazioni di scadenza della sottoscrizione.  
  
-   La pagina **Articoli** corrisponde alla pagina **Articoli** presente nella Creazione guidata nuova pubblicazione. Utilizzare questa pagina per aggiungere ed eliminare articoli e per modificare le proprietà e l'applicazione di filtri a colonne per gli articoli.  
  
-   La pagina **Filtra righe** corrisponde alla pagina **Filtro righe tabella** presente nella Creazione guidata nuova pubblicazione. Utilizzare questa pagina per aggiungere, modificare ed eliminare filtri di righe statici per tutti i tipi di pubblicazioni e aggiungere, modificare ed eliminare filtri di righe con parametri e filtri join per le pubblicazioni di tipo merge.  
  
-   La pagina **Snapshot** consente di specificare il formato e la posizione dello snapshot, se comprimere lo snapshot e gli script da eseguire prima e dopo l'applicazione dello snapshot.  
  
-   La pagina **Snapshot FTP** , per le pubblicazioni snapshot e transazionali e per le pubblicazioni di tipo merge nei server di pubblicazione che eseguono versioni precedenti a SQL Server 2005, consente di specificare se i Sottoscrittori possono scaricare file di snapshot tramite FTP (File Transfer Protocol).  
  
-   La pagina **Snapshot FTP e Internet** , per le pubblicazioni di tipo merge da server di pubblicazione che eseguono SQL Server 2005 o versione successiva, consente di specificare se i Sottoscrittori possono scaricare file di snapshot tramite FTP e sincronizzare le sottoscrizioni tramite HTTPS.  
  
-   Nella pagina **Opzioni della sottoscrizione** è possibile impostare alcune opzioni che si applicano a tutte le sottoscrizioni. Le opzioni variano in relazione al tipo di pubblicazione.  
  
-   La pagina **Elenco accesso pubblicazione** consente di specificare quali account di accesso e gruppi possono accedere a una pubblicazione.  
  
-   La pagina **Sicurezza agente** consente di accedere alle impostazioni degli account utilizzati per l'esecuzione degli agenti seguenti e per la creazione di connessioni ai computer in una topologia di replica: l'agente snapshot per tutte le pubblicazioni, l'agente di lettura log per tutte le pubblicazioni transazionali e l'agente di lettura coda per le pubblicazioni transazionali che consentono sottoscrizioni ad aggiornamento in coda.  
  
-   La pagina **Partizioni dati** , per le pubblicazioni di tipo merge da server di pubblicazione che eseguono SQL Server 2005 o versione successiva, consente di specificare se i Sottoscrittori delle pubblicazioni con filtri con parametri possono richiedere uno snapshot qualora non ne fosse disponibile uno. In questa pagina è inoltre possibile generare snapshot per una o più partizioni, una sola volta o in base a una pianificazione periodica.  
  
#### <a name="to-view-and-modify-article-properties"></a>Per visualizzare e modificare le proprietà degli articoli  
  
1.  Nella pagina **Articoli** della finestra di dialogo **Proprietà pubblicazione - \<Publication>** selezionare un articolo e quindi fare clic su **Proprietà articolo**.  
  
2.  Selezionare gli articoli a cui si applicano le modifiche delle proprietà:  
  
    -   Fare clic su **Imposta proprietà dell'articolo di \<ObjectType> evidenziato** per aprire la finestra di dialogo **Proprietà articolo - \<ObjectName>** . Le modifiche apportate alle proprietà in questa finestra di dialogo vengono applicate solo all'oggetto evidenziato nel riquadro degli oggetti nella pagina **Articoli**.  
  
    -   Fare clic su **Imposta proprietà di tutti gli articoli di \<ObjectType>** per aprire la finestra di dialogo **Proprietà di tutti gli articoli \<ObjectType>** . Le modifiche apportate alle proprietà in questa finestra di dialogo vengono applicate a tutti gli oggetti del tipo indicato nel riquadro degli oggetti all'interno della pagina **Articoli**, inclusi quelli non ancora selezionati per la pubblicazione.  
  
        > [!NOTE]  
        >  Le modifiche apportate alle proprietà nella finestra di dialogo **Proprietà di tutti gli articoli \<ObjectType>** sostituiscono tutte le modifiche eseguite precedentemente nella finestra di dialogo **Proprietà articolo - \<ObjectName>** . Se ad esempio si desidera impostare alcuni valori predefiniti per tutti gli articoli di un tipo di oggetto e, al contempo, alcune proprietà per singoli oggetti, è necessario impostare innanzitutto i valori predefiniti per tutti gli articoli, quindi le proprietà relative ai singoli oggetti.  
  
3.  Se necessario, modificare le proprietà e quindi fare clic su **OK**.  
  
4.  Fare clic su **OK** nella finestra di dialogo **Proprietà pubblicazione - \<Publication>** .  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 È possibile modificare gli articoli e restituire a livello di programmazione le relative proprietà tramite le stored procedure di replica. Le stored procedure utilizzate dipenderanno dal tipo di pubblicazione a cui appartiene l'articolo.  
  
#### <a name="to-view-the-properties-of-an-article-belonging-to-a-snapshot-or-transactional-publication"></a>Per visualizzare le proprietà di un articolo appartenente a una pubblicazione snapshot o transazionale  
  
1.  Eseguire [sp_helparticle](../../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md), specificando il nome della pubblicazione per il parametro `@publication` e il nome dell'articolo per il parametro `@article`. Se `@article` viene omesso, verranno restituite informazioni su tutti gli articoli della pubblicazione.  
  
2.  Eseguire [sp_helparticlecolumns](../../../relational-databases/system-stored-procedures/sp-helparticlecolumns-transact-sql.md) per gli articoli di tabella per elencare tutte le colonne disponibili nella tabella di base.  
  
#### <a name="to-modify-the-properties-of-an-article-belonging-to-a-snapshot-or-transactional-publication"></a>Per modificare le proprietà di un articolo appartenente a una pubblicazione snapshot o transazionale  
  
1.  Eseguire [sp_changearticle](../../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md), specificando la proprietà dell'articolo da modificare nel parametro `@property` e il nuovo valore di questa proprietà nel parametro `@value`.  
  
    > [!NOTE]  
    >  Se la modifica richiede la generazione di un nuovo snapshot, è necessario specificare anche il valore `1` per `@force_invalidate_snapshot` e, se richiede la reinizializzazione dei Sottoscrittori, è necessario specificare anche il valore `1` per `@force_reinit_subscription`. Per altre informazioni sulle proprietà che, in caso di modifica, richiedono un nuovo snapshot o una reinizializzazione, vedere [Modificare le proprietà di pubblicazioni e articoli](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
#### <a name="to-view-the-properties-of-an-article-belonging-to-a-merge-publication"></a>Per visualizzare le proprietà di un articolo appartenente a una pubblicazione di tipo merge  
  
1.  Eseguire [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md), specificando il nome della pubblicazione per il parametro `@publication` e il nome dell'articolo per il parametro `@article`. Se questi parametri vengono omessi, verranno restituite informazioni su tutti gli articoli della pubblicazione o del server di pubblicazione.  
  
2.  Eseguire [sp_helpmergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-helpmergearticlecolumn-transact-sql.md) per gli articoli di tabella per elencare tutte le colonne disponibili nella tabella di base.  
  
#### <a name="to-modify-the-properties-of-an-article-belonging-to-a-merge-publication"></a>Per modificare le proprietà di un articolo appartenente a una pubblicazione di tipo merge  
  
1.  Eseguire [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md), specificando la proprietà dell'articolo da modificare nel parametro `@property` e il nuovo valore di questa proprietà nel parametro `@value`.  
  
    > [!NOTE]  
    >  Se la modifica richiede la generazione di un nuovo snapshot, è necessario specificare anche il valore `1` per `@force_invalidate_snapshot` e, se richiede la reinizializzazione dei Sottoscrittori, è necessario specificare anche il valore `1` per `@force_reinit_subscription`. Per altre informazioni sulle proprietà che, in caso di modifica, richiedono un nuovo snapshot o una reinizializzazione, vedere [Modificare le proprietà di pubblicazioni e articoli](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Esempio (Transact-SQL)  
 In questo esempio di replica transazionale vengono restituite le proprietà dell'articolo pubblicato.  
  
 [!code-sql[HowTo#sp_helptranarticle](../../../relational-databases/replication/codesnippet/tsql/view-and-modify-article-_1.sql)]  
  
 In questo esempio di replica transazionale vengono modificate le opzioni dello schema per l'articolo pubblicato.  
  
 [!code-sql[HowTo#sp_changetranarticle](../../../relational-databases/replication/codesnippet/tsql/view-and-modify-article-_2.sql)]  
  
 In questo esempio di replica di tipo merge vengono restituite le proprietà dell'articolo pubblicato.  
  
 [!code-sql[HowTo#sp_helpmergearticle](../../../relational-databases/replication/codesnippet/tsql/view-and-modify-article-_3.sql)]  
  
 In questo esempio di replica di tipo merge vengono modificate le impostazioni di rilevamento dei conflitti per un articolo pubblicato.  
  
 [!code-sql[HowTo#sp_changemergearticle](../../../relational-databases/replication/codesnippet/tsql/view-and-modify-article-_4.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilizzo di RMO (Replication Management Objects)  
 È possibile modificare gli articoli e accedere alle relative proprietà a livello di programmazione utilizzando oggetti RMO (Replication Management Objects). Le classi RMO utilizzate per la visualizzazione o la modifica degli articoli dipendono dal tipo di pubblicazione cui appartiene l'articolo.  
  
#### <a name="to-view-or-modify-properties-of-an-article-that-belongs-to-a-snapshot-or-transactional-publication"></a>Per visualizzare o modificare le proprietà di un articolo che appartiene a una pubblicazione snapshot o transazionale  
  
1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.TransArticle>.  
  
3.  Impostare le proprietà <xref:Microsoft.SqlServer.Replication.Article.Name%2A>, <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>e <xref:Microsoft.SqlServer.Replication.Article.DatabaseName%2A> .  
  
4.  Impostare la connessione del passaggio 1 per la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per recuperare le proprietà dell'oggetto. Se questo metodo restituisce **false**, le proprietà dell'articolo sono state definite in modo non corretto nel passaggio 3 oppure l'articolo non esiste.  
  
6.  (Facoltativo) Per modificare le proprietà, specificare un nuovo valore per una delle proprietà dell'oggetto <xref:Microsoft.SqlServer.Replication.TransArticle> che è possibile impostare.  
  
7.  (Facoltativo) Se si specifica un valore **true** per <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A>, chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> per eseguire il commit delle modifiche nel server. Se si specifica un valore **false** per <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> (impostazione predefinita), le modifiche vengono inviate immediatamente al server.  
  
#### <a name="to-view-or-modify-properties-of-an-article-that-belongs-to-a-merge-publication"></a>Per visualizzare o modificare le proprietà di un articolo che appartiene a una pubblicazione di tipo merge  
  
1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergeArticle>.  
  
3.  Impostare le proprietà <xref:Microsoft.SqlServer.Replication.Article.Name%2A>, <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>e <xref:Microsoft.SqlServer.Replication.Article.DatabaseName%2A> .  
  
4.  Impostare la connessione del passaggio 1 per la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per recuperare le proprietà dell'oggetto. Se questo metodo restituisce **false**, le proprietà dell'articolo sono state definite in modo non corretto nel passaggio 3 oppure l'articolo non esiste.  
  
6.  (Facoltativo) Per modificare le proprietà, specificare un nuovo valore per una delle proprietà dell'oggetto <xref:Microsoft.SqlServer.Replication.MergeArticle> che è possibile impostare.  
  
7.  (Facoltativo) Se si specifica un valore **true** per <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A>, chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> per eseguire il commit delle modifiche nel server. Se si specifica un valore **false** per <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> (impostazione predefinita), le modifiche vengono inviate immediatamente al server.  
  
###  <a name="example-rmo"></a><a name="PShellExample"></a> Esempio (RMO)  
 In questo esempio viene modificato un articolo di merge per specificare il gestore della logica di business utilizzato dall'articolo.  
  
 [!code-cs[HowTo#rmo_ChangeMergeArticle_BLH](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_changemergearticle_blh)]  
  
 [!code-vb[HowTo#rmo_vb_ChangeMergeArticle_BLH](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_changemergearticle_blh)]  
  
## <a name="see-also"></a>Vedere anche  
 [Implementare un gestore della logica di business per un articolo di merge](../../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md)   
 [Pubblicare dati e oggetti di database](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Modificare le proprietà di pubblicazioni e articoli](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Rilevamento e risoluzione avanzati dei conflitti nella replica di tipo merge](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)  
  
  
