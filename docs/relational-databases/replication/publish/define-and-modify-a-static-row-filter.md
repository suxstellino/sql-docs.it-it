---
description: Definizione e modifica di un filtro di riga statico
title: Definire e modificare un filtro di riga statico | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- modifying filters, static row
- static row filters
- filters [SQL Server replication], static row
ms.assetid: a6ebb026-026f-4c39-b6a9-b9998c3babab
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 20632762a98c264deff09182f5f24a0602ab8d6c
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076769"
---
# <a name="define-and-modify-a-static-row-filter"></a>Definizione e modifica di un filtro di riga statico
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  In questo argomento viene descritto come definire e modificare un filtro di riga statico in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Indicazioni](#Recommendations)  
  
-   **Per definire e modificare un filtro di riga statico, utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Se si aggiunge, modifica o elimina un filtro di riga statico dopo che sono state inizializzate sottoscrizioni per la pubblicazione, è necessario generare un nuovo snapshot e reinizializzare tutte le sottoscrizioni in seguito alla modifica. Per altre informazioni sui requisiti per la modifica delle proprietà, vedere [Modificare le proprietà di pubblicazioni e articoli](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
-   Se la pubblicazione è abilitata per la replica transazionale peer-to-peer, non sarà possibile filtrare le tabelle.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Poiché questi filtri sono statici, tutti i sottoscrittori riceveranno lo stesso subset di dati. Se è necessario filtrare dinamicamente le righe in un articolo di tabella appartenente a una tabella di tipo merge, in modo che ogni sottoscrittore riceva una partizione diversa dei dati, vedere [Definizione e modifica di un filtro di riga con parametri per un articolo di merge](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md). La replica di tipo merge consente inoltre di filtrare righe correlate in base a un filtro di riga esistente. Per altre informazioni, vedere [Definizione e modifica di un filtro di join tra articoli di merge](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 Definire, modificare ed eliminare i filtri di riga statici nella pagina **Filtro righe tabella** della Creazione guidata nuova pubblicazione o nella pagina **Filtra righe** della finestra di dialogo **Proprietà pubblicazione - \<Publication>** . Per altre informazioni sull'uso della creazione guidata e l'accesso alla finestra di dialogo, vedere [Creare una pubblicazione](../../../relational-databases/replication/publish/create-a-publication.md) e [Visualizzare e modificare le proprietà della pubblicazione](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-define-a-static-row-filter"></a>Per definire un filtro di riga statico  
  
1.  L'operazione eseguita nella pagina **Filtro righe tabella** della Creazione guidata nuova pubblicazione o nella pagina **Filtra righe** della finestra di dialogo **Proprietà pubblicazione - \<Publication>** dipende dal tipo di pubblicazione:  
  
    -   Per una pubblicazione snapshot o transazionale, fare clic su **Aggiungi**.  
  
    -   Per una pubblicazione di tipo merge, fare clic su **Aggiungi** e quindi su **Aggiungi filtro**.  
  
2.  Nella finestra di dialogo **Aggiungi filtro** selezionare una tabella da filtrare nell'elenco a discesa.  
  
3.  Creare un'istruzione per il filtro nell'area di testo **Istruzione per il filtro** . È possibile digitare direttamente nell'area di testo nonché trascinare colonne dalla casella di riepilogo **Colonne** .  
  
    > [!NOTE]  
    >  Per la clausola WHERE è consigliabile usare nomi in due parti. I nomi in tre e quattro parti non sono supportati. Se la pubblicazione proviene da un server di pubblicazione Oracle, è necessario che la clausola WHERE sia conforme alla sintassi Oracle.  
  
    -   L'area di testo **Istruzione per il filtro** contiene il testo predefinito, nel formato seguente:  
  
        ```sql  
        SELECT <published_columns> FROM [schema].[tablename] WHERE  
        ```  
  
    -   Il testo predefinito non può essere modificato. Digitare la clausola di filtro dopo la parola chiave WHERE usando la sintassi SQL standard. La clausola di filtro completa sarà simile alla seguente:  
  
        ```sql  
        SELECT <published_columns> FROM [HumanResources].[Employee] WHERE [LoginID] = 'adventure-works\ranjit0'  
        ```  
  
    -   In un filtro di riga statico può essere inclusa una funzione definita dall'utente. La clausola di filtro completa per un filtro di riga statico con una funzione definita dall'utente sarà simile alla seguente:  
  
        ```sql  
        SELECT <published_columns> FROM [Sales].[SalesOrderHeader] WHERE MyFunction([Freight]) > 100  
        ```  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  Se è visualizzata la finestra di dialogo **Proprietà pubblicazione - \<Publication>** fare clic su **OK** per salvare e chiudere la finestra di dialogo.  

#### <a name="to-modify-a-static-row-filter"></a>Per modificare un filtro di riga statico  
  
1.  Nella pagina **Filtro righe tabella** della Creazione guidata nuova pubblicazione o nella pagina **Filtra righe** della finestra di dialogo **Proprietà pubblicazione - \<Publication>** selezionare un filtro nel riquadro **Tabelle filtrate** e quindi fare clic su **Modifica**.  
  
2.  Nella finestra di dialogo **Modifica filtro** modificare il filtro.  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-delete-a-static-row-filter"></a>Per eliminare un filtro di riga statico  
  
1.  Nella pagina **Filtro righe tabella** della Creazione guidata nuova pubblicazione o nella pagina **Filtra righe** della finestra di dialogo **Proprietà pubblicazione - \<Publication>** selezionare un filtro nel riquadro **Tabelle filtrate** e quindi fare clic su **Elimina**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 Quando si creano articoli di tabella, è possibile definire una clausola WHERE per escludere le righe di un articolo. È inoltre possibile modificare un filtro di riga dopo che è stato definito. È possibile creare e modificare a livello di programmazione i filtri di riga statici tramite le stored procedure di replica.  
  
#### <a name="to-define-a-static-row-filter-for-a-snapshot-or-transactional-publication"></a>Per definire un filtro di riga statico per una pubblicazione snapshot o transazionale  
  
1.  Definire l'articolo da filtrare. Per altre informazioni, vedere [definire un articolo](../../../relational-databases/replication/publish/define-an-article.md).  
  
2.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_articlefilter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md). Specificare il nome dell'articolo per **\@article**, il nome della pubblicazione per **\@publication**, il nome del filtro per **\@filter_name** e la clausola di filtro per **\@filter_clause** (senza includere `WHERE`).  
  
3.  Se non è ancora stato definito un filtro di colonna, vedere [Definizione e modifica di un filtro colonne](../../../relational-databases/replication/publish/define-and-modify-a-column-filter.md). In caso contrario, eseguire [sp_articleview &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md). Specificare il nome della pubblicazione per **\@publication**, il nome dell'articolo filtrato per **\@article** e la clausola di filtro specificata nel passaggio 2 per **\@filter_clause**. Verranno creati gli oggetti di sincronizzazione per l'articolo filtrato.  
  
#### <a name="to-modify-a-static-row-filter-for-a-snapshot-or-transactional-publication"></a>Per modificare un filtro di riga statico per una pubblicazione snapshot o transazionale  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_articlefilter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md). Specificare il nome dell'articolo per **\@article**, il nome della pubblicazione per **\@publication**, il nome del nuovo filtro per **\@filter_name** e la nuova clausola di filtro per **\@filter_clause** (senza includere `WHERE`). Poiché questa modifica invaliderà i dati nelle sottoscrizioni esistenti, specificare il valore **1** per **\@force_reinit_subscription**.  
  
2.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_articleview &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md). Specificare il nome della pubblicazione per **\@publication**, il nome dell'articolo filtrato per **\@article** e la clausola di filtro specificata nel passaggio 1 per **\@filter_clause**. Verrà ricreata la vista che definisce l'articolo filtrato.  
  
3.  Rieseguire il processo dell'agente snapshot per la pubblicazione per generare uno snapshot aggiornato. Per altre informazioni, vedere [Creazione e applicazione dello snapshot iniziale](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
4.  Reinizializzazione delle sottoscrizioni. Per altre informazioni, vedere [Reinizializzare le sottoscrizioni](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
#### <a name="to-delete-a-static-row-filter-for-a-snapshot-or-transactional-publication"></a>Per eliminare un filtro di riga statico per una pubblicazione snapshot o transazionale  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_articlefilter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md). Specificare il nome dell'articolo per **\@article**, il nome della pubblicazione per **\@publication**, il valore NULL per **\@filter_name** e il valore NULL per **\@filter_clause**. Poiché questa modifica invaliderà i dati nelle sottoscrizioni esistenti, specificare il valore **1** per **\@force_reinit_subscription**.  
  
2.  Rieseguire il processo dell'agente snapshot per la pubblicazione per generare uno snapshot aggiornato. Per altre informazioni, vedere [Creazione e applicazione dello snapshot iniziale](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
3.  Reinizializzazione delle sottoscrizioni. Per altre informazioni, vedere [Reinizializzare le sottoscrizioni](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
#### <a name="to-define-a-static-row-filter-for-a-merge-publication"></a>Per definire un filtro di riga statico per una pubblicazione di tipo merge  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Specificare la clausola di filtro per **\@subset_filterclause** (senza includere `WHERE`). Per altre informazioni, vedere [definire un articolo](../../../relational-databases/replication/publish/define-an-article.md).  
  
2.  Se non è ancora stato definito un filtro di colonna, vedere [Definizione e modifica di un filtro colonne](../../../relational-databases/replication/publish/define-and-modify-a-column-filter.md).  
  
#### <a name="to-modify-a-static-row-filter-for-a-merge-publication"></a>Per modificare un filtro di riga statico per una pubblicazione di tipo merge  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_changemergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Specificare il nome della pubblicazione per **\@publication**, il nome dell'articolo filtrato per **\@article**, il valore **subset_filterclause** per **\@property** e la nuova clausola di filtro per **\@value** (senza includere `WHERE`). Poiché questa modifica invaliderà i dati nelle sottoscrizioni esistenti, specificare il valore 1 per **\@force_reinit_subscription**.  
  
2.  Rieseguire il processo dell'agente snapshot per la pubblicazione per generare uno snapshot aggiornato. Per altre informazioni, vedere [Creazione e applicazione dello snapshot iniziale](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
3.  Reinizializzazione delle sottoscrizioni. Per altre informazioni, vedere [Reinizializzare le sottoscrizioni](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Esempi (Transact-SQL)  
 In questo esempio di replica transazionale l'articolo viene filtrato orizzontalmente per rimuovere tutti i prodotti non più supportati.  
  
 [!code-sql[HowTo#sp_AddTranArticle](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-stat_1.sql)]  
  
 In questo esempio di replica di tipo merge gli articoli vengono filtrati orizzontalmente per restituire solo le righe che appartengono al venditore specificato. Viene utilizzato anche un filtro join. Per altre informazioni, vedere [Definizione e modifica di un filtro di join tra articoli di merge](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
 [!code-sql[HowTo#sp_AddMergeArticle](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-stat_2.sql)]  
  
## <a name="see-also"></a>Vedere anche  
 [Definizione e modifica di un filtro di riga con parametri per un articolo di merge](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)   
 [Modificare le proprietà di pubblicazioni e articoli](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [Filtrare i dati pubblicati](../../../relational-databases/replication/publish/filter-published-data.md)   
 [Filtrare i dati pubblicati per la replica di tipo merge](../../../relational-databases/replication/merge/filter-published-data-for-merge-replication.md)  
  
  
