---
description: Definizione e modifica di un filtro colonne
title: Definire e modificare un filtro colonne | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- filters [SQL Server replication], column
- modifying filters, column
- modifying filters
- column filters [SQL Server replication]
ms.assetid: d7c3186a-9a8c-45d8-ab34-05beec4c26dd
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: e266e188719bd5ed3d84adcf81d49aa175f22872
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076759"
---
# <a name="define-and-modify-a-column-filter"></a>Definizione e modifica di un filtro colonne
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  In questo argomento viene descritto come definire e modificare un filtro colonne in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
-   **Per definire e modificare un filtro colonne, utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Alcune colonne non possono essere filtrate. Per altre informazioni, vedere [Filtrare i dati pubblicati](../../../relational-databases/replication/publish/filter-published-data.md). Se si modifica un filtro di colonna dopo l'inizializzazione delle sottoscrizioni, dopo la modifica è necessario generare un nuovo snapshot e reinizializzare tutte le sottoscrizioni. Per altre informazioni sui requisiti per la modifica delle proprietà, vedere [Modificare le proprietà di pubblicazioni e articoli](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 Definire i filtri colonne nella pagina **Articoli** della Creazione guidata nuova pubblicazione. Per altre informazioni sull'uso della Creazione guidata nuova pubblicazione, vedere [Creare una pubblicazione](../../../relational-databases/replication/publish/create-a-publication.md).  
  
 Definire e modificare i filtri di colonna nella pagina **Articoli** della finestra di dialogo **Proprietà pubblicazione - \<Publication>** . Per altre informazioni sulla modifica delle proprietà di pubblicazione, vedere [Visualizzare e modificare le proprietà della pubblicazione](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-define-a-column-filter"></a>Per definire un filtro colonne  
  
1.  Nella pagina **Articoli** della Creazione guidata nuova pubblicazione espandere la tabella da filtrare nel riquadro **Oggetti da pubblicare** .  
  
2.  Deselezionare la casella di controllo accanto a ogni colonna che si desidera filtrare.  
  
#### <a name="to-modify-column-filtering"></a>Per modificare l'applicazione di filtri di colonna  
  
1.  Nella pagina **Articoli** della finestra di dialogo **Proprietà pubblicazione - \<Publication>** espandere la tabella da filtrare nel riquadro **Oggetti da pubblicare**.  
  
2.  Deselezionare la casella di controllo accanto a ogni colonna che si desidera filtrare e verificare che la casella di controllo sia selezionata per ogni colonna da includere nell'articolo.  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 Durante la creazione di articoli di tabella è possibile definire le colonne da includere nell'articolo e modificare le colonne dopo aver definito l'articolo. È possibile creare e modificare a livello di programmazione le colonne filtrate tramite le stored procedure di replica.  
  
> [!NOTE]  
>  Nelle procedure seguenti si presuppone che la tabella sottostante non sia modificata. Per informazioni sulla replica di modifiche DDL (Data Definition Language) in tabelle pubblicate, vedere [Apportare modifiche allo schema nei database di pubblicazione](../../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).  
  
#### <a name="to-define-a-column-filter-for-an-article-published-in-a-snapshot-or-transactional-publication"></a>Per definire un filtro di colonna per un articolo pubblicato di una pubblicazione snapshot o transazionale  
  
1.  Definire l'articolo da filtrare. Per altre informazioni, vedere [definire un articolo](../../../relational-databases/replication/publish/define-an-article.md).  
  
2.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_articlecolumn](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md). Verranno definite le colonne da includere o rimuovere dall'articolo.  
  
    -   Se si intende pubblicare solo alcune colonne di una tabella contenente molte colonne, eseguire [sp_articlecolumn](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) una volta per ogni colonna da aggiungere. Specificare il nome della colonna per **\@column** e il valore **add** per **\@operation**.  
  
    -   Se si intende pubblicare la maggior parte delle colonne di una tabella contenente molte colonne, eseguire [sp_articlecolumn](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md), specificando il valore **null** per **\@column** e il valore **add** per **\@operation** per aggiungere tutte le colonne. Eseguire quindi [sp_articlecolumn](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md), una volta per ogni colonna da escludere, specificando il valore **drop** per **\@operation** e il nome della colonna esclusa per **\@column**.  
  
3.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_articleview](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md). Specificare il nome della pubblicazione per **\@publication** e il nome dell'articolo filtrato per **\@article**. Verranno creati gli oggetti di sincronizzazione per l'articolo filtrato.  
  
#### <a name="to-change-a-column-filter-to-include-additional-columns-for-an-article-published-in-a-snapshot-or-transactional-publication"></a>Per modificare un filtro colonne in modo da includere colonne aggiuntive per un articolo pubblicato di una pubblicazione snapshot o transazionale  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_articlecolumn](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) una volta per ogni colonna da aggiungere. Specificare il nome della colonna per **\@column** e il valore **add** per **\@operation**.  
  
2.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_articleview](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md). Specificare il nome della pubblicazione per **\@publication** e il nome dell'articolo filtrato per **\@article**. Se per la pubblicazione esistono sottoscrizioni, specificare il valore **1** per **\@change_active**. Verranno creati nuovamente gli oggetti di sincronizzazione per l'articolo filtrato.  
  
3.  Rieseguire il processo dell'agente snapshot per la pubblicazione per generare uno snapshot aggiornato.  
  
4.  Reinizializzazione delle sottoscrizioni. Per altre informazioni, vedere [Reinizializzare le sottoscrizioni](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
#### <a name="to-change-a-column-filter-to-remove-columns-for-an-article-published-in-a-snapshot-or-transactional-publication"></a>Per modificare un filtro colonne in modo da rimuovere colonne per un articolo pubblicato di una pubblicazione snapshot o transazionale  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_articlecolumn](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) una volta per ogni colonna da rimuovere. Specificare il nome della colonna per **\@column** e il valore **drop** per **\@operation**.  
  
2.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_articleview](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md). Specificare il nome della pubblicazione per **\@publication** e il nome dell'articolo filtrato per **\@article**. Se per la pubblicazione esistono sottoscrizioni, specificare il valore **1** per **\@change_active**. Verranno creati nuovamente gli oggetti di sincronizzazione per l'articolo filtrato.  
  
3.  Rieseguire il processo dell'agente snapshot per la pubblicazione per generare uno snapshot aggiornato.  
  
4.  Reinizializzazione delle sottoscrizioni. Per altre informazioni, vedere [Reinizializzare le sottoscrizioni](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
#### <a name="to-define-a-column-filter-for-an-article-published-in-a-merge-publication"></a>Per definire un filtro di colonna per un articolo pubblicato di una pubblicazione di tipo merge  
  
1.  Definire l'articolo da filtrare. Per altre informazioni, vedere [definire un articolo](../../../relational-databases/replication/publish/define-an-article.md).  
  
2.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_mergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md). Verranno definite le colonne da includere o rimuovere dall'articolo.  
  
    -   Se si intende pubblicare solo alcune colonne di una tabella contenente molte colonne, eseguire [sp_mergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md) una volta per ogni colonna da aggiungere. Specificare il nome della colonna per **\@column** e il valore **add** per **\@operation**.  
  
    -   Se si intende pubblicare la maggior parte delle colonne di una tabella contenente molte colonne, eseguire [sp_mergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md), specificando il valore **null** per **\@column** e il valore **add** per **\@operation** per aggiungere tutte le colonne. Eseguire quindi [sp_mergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md), una volta per ogni colonna da escludere, specificando il valore **drop** per **\@operation** e il nome della colonna esclusa per **\@column**.  
  
#### <a name="to-change-a-column-filter-to-include-additional-columns-for-an-article-published-in-a-merge-publication"></a>Per modificare un filtro colonne in modo da includere colonne aggiuntive per un articolo pubblicato di una pubblicazione di tipo merge  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_mergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md) una volta per ogni colonna da aggiungere. Specificare il nome della colonna per **\@column**, il valore **add** per **\@operation** e il valore **1** per **\@force_invalidate_snapshot** e **\@force_reinit_subscription**.  
  
2.  Rieseguire il processo dell'agente snapshot per la pubblicazione per generare uno snapshot aggiornato.  
  
3.  Reinizializzazione delle sottoscrizioni. Per altre informazioni, vedere [Reinizializzare le sottoscrizioni](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
#### <a name="to-change-a-column-filter-to-remove-columns-for-an-article-published-in-a-merge-publication"></a>Per modificare un filtro colonne in modo da rimuovere colonne per un articolo pubblicato di una pubblicazione di tipo merge  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_mergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md) una volta per ogni colonna da rimuovere. Specificare il nome della colonna per **\@column**, il valore **drop** per **\@operation** e il valore **1** per **\@force_invalidate_snapshot** e **\@force_reinit_subscription**.  
  
2.  Rieseguire il processo dell'agente snapshot per la pubblicazione per generare uno snapshot aggiornato.  
  
3.  Reinizializzazione delle sottoscrizioni. Per altre informazioni, vedere [Reinizializzare le sottoscrizioni](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Esempio (Transact-SQL)  
 In questo esempio di replica transazionale la colonna `DaysToManufacture` viene rimossa da un articolo basato sulla tabella `Product` .  
  
 [!code-sql[HowTo#sp_AddTranArticle](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-colu_1.sql)]  
  
 In questo esempio di replica di tipo merge la colonna `CreditCardApprovalCode` viene rimossa da un articolo basato sulla tabella `SalesOrderHeader` .  
  
 [!code-sql[HowTo#sp_AddMergeArticle](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-colu_2.sql)]  
  
## <a name="see-also"></a>Vedere anche  
 [Modificare le proprietà di pubblicazioni e articoli](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [Filtrare i dati pubblicati](../../../relational-databases/replication/publish/filter-published-data.md)   
 [Filtrare i dati pubblicati per la replica di tipo merge](../../../relational-databases/replication/merge/filter-published-data-for-merge-replication.md)  
  
  
