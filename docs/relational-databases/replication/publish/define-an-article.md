---
description: Definizione di un articolo
title: Definire un articolo | Microsoft Docs
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
- articles [SQL Server replication], defining
- sp_addmergearticle
- adding articles
- sp_addarticle
- articles [SQL Server replication], adding
ms.assetid: 220584d8-b291-43ae-b036-fbba3cc07a2e
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: b93fbfea46931eb460b842a3a34ae13853726792
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766369"
---
# <a name="define-an-article"></a>Definizione di un articolo
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come definire un articolo in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o RMO (Replication Management Objects).  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per definire un articolo tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Oggetti RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   I nomi di articolo non possono includere i caratteri seguenti: %, *, [,], |: ?" , ' , \ , / , < , >. Se si desidera replicare oggetti del database che includono uno qualsiasi di questi caratteri, è necessario specificare un nome di articolo diverso dal nome dell'oggetto.  
  
##  <a name="security"></a><a name="Security"></a> Sicurezza  
 Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali, utilizzare i [servizi di crittografia](/previous-versions/aa719848(v=vs.71)) offerti da [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows .NET Framework.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 Creare le pubblicazioni e definire gli articoli utilizzando la Creazione guidata nuova pubblicazione. Dopo aver creato una pubblicazione, visualizzare e modificare le relative proprietà nella finestra di dialogo **Proprietà pubblicazione - \<Publication>** . Per informazioni sulla creazione di una pubblicazione da un database Oracle, vedere [Creare una pubblicazione da un database Oracle](../../../relational-databases/replication/publish/create-a-publication-from-an-oracle-database.md).  
  
#### <a name="to-create-a-publication-and-define-articles"></a>Per creare una pubblicazione e definire articoli  
  
1.  Connettersi al server di pubblicazione in [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] e quindi espandere il nodo del server.  
  
2.  Espandere la cartella **Replica** e quindi fare clic con il pulsante destro del mouse sulla cartella **Pubblicazioni locali** .  
  
3.  Fare clic su **Nuova pubblicazione**.  
  
4.  Eseguire i vari passaggi della Creazione guidata nuova pubblicazione per:  

    -   Specificare un server di distribuzione se la distribuzione non è stata configurata sul server. Per altre informazioni sulla configurazione della distribuzione, vedere [Configurare la pubblicazione e la distribuzione](../../../relational-databases/replication/configure-publishing-and-distribution.md).  
  
         Se nella pagina **Server di distribuzione** si specifica che il server di pubblicazione deve agire come server di distribuzione per se stesso (server di distribuzione locale) e il server non è configurato come server di distribuzione, la Creazione guidata nuova pubblicazione eseguirà la configurazione del server. Nella pagina **Cartella snapshot** è necessario specificare una cartella snapshot predefinita per il server di distribuzione. La cartella snapshot è semplicemente una directory designata come condivisione. Gli agenti che eseguono letture e scritture in questa cartella devono disporre di autorizzazioni sufficienti per accedervi. Per altre informazioni sulle impostazioni di sicurezza appropriate per la cartella, vedere [Proteggere la cartella snapshot](../../../relational-databases/replication/security/secure-the-snapshot-folder.md).  
  
         Se si specifica che un altro server deve agire come server di distribuzione, è necessario immettere una password nella pagina **Password amministrativa** per le connessioni effettuate dal server di pubblicazione a quello di distribuzione. Questa password deve corrispondere a quella specificata quando il server di pubblicazione è stato attivato nel server di distribuzione remoto.  
  
         Per altre informazioni, vedere [Configure Distribution](../../../relational-databases/replication/configure-distribution.md).  
  
    -   Scegliere un database di pubblicazione.  
  
    -   Selezionare un tipo di pubblicazione. Per altre informazioni, vedere [Tipi di replica](../../../relational-databases/replication/types-of-replication.md).  
  
    -   Specificare i dati e gli oggetti di database da pubblicare; facoltativamente, filtrare le colonne dagli articoli di tabelle e impostare le proprietà degli articoli.  
  
    -   Facoltativamente, filtrate le righe dagli articoli di tabelle. Per altre informazioni, vedere [Filtrare i dati pubblicati](../../../relational-databases/replication/publish/filter-published-data.md).  
  
    -   Impostare la pianificazione dell'agente snapshot.  
  
    -   Specificare le credenziali con cui gli agenti di replica seguenti vengono eseguiti e stabiliscono le connessioni:  
  
         \- Agente snapshot per tutte le pubblicazioni.  
  
         \- Agente di lettura log per tutte le pubblicazioni transazionali.  
  
         \- Agente di lettura coda per le pubblicazioni transazionali che consentono l'aggiornamento delle sottoscrizioni.  
  
         Per ulteriori informazioni, vedere [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md) e [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md).  
  
    -   Facoltativamente, creare lo script della pubblicazione. Per altre informazioni, vedere [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
    -   Specificare un nome per la pubblicazione.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 Dopo aver creato una pubblicazione, è possibile creare gli articoli a livello di programmazione tramite le codice stored procedure di replica. Le stored procedure utilizzate per creare un articolo dipendono dal tipo di pubblicazione per il quale viene definito l'articolo. Per altre informazioni, vedere [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md).  
  
#### <a name="to-define-an-article-for-a-snapshot-or-transactional-publication"></a>Per definire un articolo per una pubblicazione snapshot o transazionale  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md). Specificare il nome della pubblicazione cui appartiene l'articolo per `@publication`, il nome dell'articolo per `@article`, l'oggetto di database da pubblicare per `@source_object` ed eventuali altri parametri facoltativi. Usare `@source_owner` per specificare la proprietà dello schema dell'oggetto, se diversa da **dbo**. Se l'articolo non è un articolo di tabella basato su log, specificare il tipo di articolo per `@type`. Per altre informazioni, vedere [Specificare i tipi di articolo &#40;programmazione Transact-SQL della replica&#41;](../../../relational-databases/replication/publish/specify-article-types-replication-transact-sql-programming.md).  
  
2.  Per filtrare in senso orizzontale le righe di una tabella o visualizzare un articolo, utilizzare [sp_articlefilter](../../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md) per definire la clausola di filtro. Per altre informazioni, vedere [Define and Modify a Static Row Filter](../../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md).  
  
3.  Per filtrare in senso verticale le colonne di una tabella o visualizzare un articolo, utilizzare [sp_articlecolumn](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md). Per altre informazioni, vedere [Define and Modify a Column Filter](../../../relational-databases/replication/publish/define-and-modify-a-column-filter.md).  
  
4.  Eseguire [sp_articleview](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) se l'articolo è filtrato.  
  
5.  Se per la pubblicazione esistono sottoscrizioni e [sp_helppublication](../../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md) restituisce il valore **0** nella colonna **immediate_sync** , è necessario chiamare [sp_addsubscription](../../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) per aggiungere l'articolo a ogni sottoscrizione esistente.  
  
6.  Se per la pubblicazione esistono sottoscrizioni pull, eseguire [sp_refreshsubscriptions](../../../relational-databases/system-stored-procedures/sp-refreshsubscriptions-transact-sql.md) nel server di pubblicazione per creare un nuovo snapshot per le sottoscrizioni pull esistenti contenente solo il nuovo articolo.  
  
    > [!NOTE]  
    >  Per le sottoscrizioni non inizializzate tramite snapshot, non è necessario eseguire [sp_refreshsubscriptions](../../../relational-databases/system-stored-procedures/sp-refreshsubscriptions-transact-sql.md) poiché tale procedura viene eseguita da [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md).  
  
#### <a name="to-define-an-article-for-a-merge-publication"></a>Per definire un articolo per una pubblicazione di tipo merge  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Specificare il nome della pubblicazione per `@publication`, il nome dell'articolo per `@article` e l'oggetto da pubblicare per `@source_object`. Per filtrare in senso orizzontale le righe della tabella, specificare un valore per `@subset_filterclause`. Per ulteriori informazioni, vedere [Definizione e modifica di un filtro di riga con parametri per un articolo di merge](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md) e [Definizione e modifica di un filtro di riga statico](../../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md). Se l'articolo non è un articolo di tabella, specificare il tipo di articolo per `@type`. Per altre informazioni, vedere [Specificare i tipi di articolo &#40;programmazione Transact-SQL della replica&#41;](../../../relational-databases/replication/publish/specify-article-types-replication-transact-sql-programming.md).  
  
2.  (Facoltativo) Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergefilter](../../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md) per definire un filtro di join tra due articoli. Per altre informazioni, vedere [Definizione e modifica di un filtro di join tra articoli di merge](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
3.  (Facoltativo) Nel database di pubblicazione del server di pubblicazione eseguire [sp_mergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md) per filtrare le colonne della tabella. Per altre informazioni, vedere [Define and Modify a Column Filter](../../../relational-databases/replication/publish/define-and-modify-a-column-filter.md).  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Esempi (Transact-SQL)  
 In questo esempio si definisce un articolo basato sulla tabella `Product` per una pubblicazione transazionale, in cui l'articolo è filtrato in senso orizzontale e verticale.  
  
 [!code-sql[HowTo#sp_AddTranArticle](../../../relational-databases/replication/codesnippet/tsql/define-an-article_1.sql)]  
  
 In questo esempio si definiscono gli articoli per una pubblicazione di tipo merge, in cui l'articolo `SalesOrderHeader` è filtrato in modo statico in base a **SalesPersonID** e l'articolo `SalesOrderDetail` è filtrato con filtro di join in base a `SalesOrderHeader`.  
  
 [!code-sql[HowTo#sp_AddMergeArticle](../../../relational-databases/replication/codesnippet/tsql/define-an-article_2.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilizzo di RMO (Replication Management Objects)  
 È possibile definire articoli a livello di programmazione tramite gli oggetti RMO (Replication Management Objects). Le classi RMO utilizzate per la definizione di un articolo dipendono dal tipo di pubblicazione per la quale l'articolo viene definito.  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Esempi (RMO)  
 Nell'esempio seguente un articolo con filtri di riga e di colonna viene aggiunto a una pubblicazione transazionale.  
  
 [!code-cs[HowTo#rmo_CreateTranArticles](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createtranarticles)]  
  
 [!code-vb[HowTo#rmo_vb_CreateTranArticles](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createtranarticles)]  
  
 Nell'esempio seguente tre articoli vengono aggiunti a una pubblicazione di tipo merge. Gli articoli contengono filtri di colonna e vengono utilizzati due filtri join per propagare un filtro di riga con parametri negli altri articoli.  
  
 [!code-cs[HowTo#rmo_CreateMergeArticles](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergearticles)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergeArticles](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergearticles)]  
  
## <a name="see-also"></a>Vedere anche  
 [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md)   
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Aggiungere ed eliminare articoli in pubblicazioni esistenti](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md)   
 [Filtrare i dati pubblicati](../../../relational-databases/replication/publish/filter-published-data.md)   
 [Pubblicare dati e oggetti di database](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)  
  
