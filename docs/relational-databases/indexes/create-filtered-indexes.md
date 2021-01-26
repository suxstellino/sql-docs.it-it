---
description: Creare indici filtrati
title: Creare indici filtrati | Microsoft Docs
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- filtered indexes [SQL Server], about filtered indexes
- designing indexes [SQL Server], filtered
- filtered indexes [SQL Server]
- nonclustered indexes [SQL Server], filtered
- indexes [SQL Server], filtered
ms.assetid: 25e1fcc5-45d7-4c53-8c79-5493dfaa1c74
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 546ec1476ecae0da0fd119c7b45d06c2cc4406f6
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/25/2021
ms.locfileid: "98764907"
---
# <a name="create-filtered-indexes"></a>Creare indici filtrati
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  In questo argomento si descrive come creare un indice filtrato in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Un indice filtrato è un indice non cluster ottimizzato, particolarmente indicato per coprire query per le quali i dati vengono selezionati da un subset ben definito. Un indice di questo tipo utilizza un predicato del filtro per indicizzare una parte di righe nella tabella. Se confrontato con indici di tabella completa, un indice filtrato progettato correttamente consente di migliorare le prestazioni delle query e di ridurre i costi di gestione e di archiviazione dell'indice stesso.  
  
 Rispetto agli indici di tabella completa, gli indici filtrati consentono di ottenere i vantaggi seguenti:  
  
-   **Prestazioni di esecuzione delle query e qualità del piano migliorate**  
  
     Un indice filtrato progettato correttamente migliora le prestazioni di esecuzione delle query e la qualità del piano di esecuzione poiché è caratterizzato da dimensioni minori rispetto a un indice non cluster di tabella completa e dispone di statistiche filtrate. Queste ultime sono più accurate delle statistiche di tabella completa poiché coprono solo le righe nell'indice filtrato.  
  
-   **Costi di manutenzione dell'indice ridotti**  
  
     La manutenzione di un indice viene eseguita solo quando le istruzioni DML (Data Manipulation Language) influiscono sui dati relativi all'indice. Un indice filtrato consente di ridurre i costi di gestione rispetto a un indice non cluster di tabella completa poiché è caratterizzato da dimensioni minori e viene gestito solo se i dati relativi vengono modificati. È possibile disporre di un numero elevato di indici filtrati, soprattutto quando in essi sono contenuti dati modificati raramente. In modo analogo, se in un indice filtrato sono contenuti solo i dati modificati di frequente, la dimensione minore dell'indice consente di ridurre il costo di aggiornamento delle statistiche.  
  
-   **Costi di archiviazione dell'indice ridotti**  
  
     La creazione di un indice filtrato può ridurre lo spazio di archiviazione su disco per gli indici non cluster nel caso in cui non sia necessario un indice di tabella completa. È possibile sostituire un indice non cluster di tabella completa con più indici filtrati senza aumentare in modo significativo i requisiti di archiviazione.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Considerazioni sulla progettazione](#Design)  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per creare un indice filtrato tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="design-considerations"></a><a name="Design"></a> Considerazioni sulla progettazione  
  
-   Quando una colonna dispone solo di un numero ridotto di valori rilevanti per le query, è possibile creare un indice filtrato sul subset di valori. Ad esempio, quando la maggior parte dei valori di una colonna è costituita da valori NULL e la query esegue la selezione solo dai valori non NULL, è possibile creare un indice filtrato per le righe di dati non NULL. L'indice risultante sarà minore e sarà possibile gestirlo con costi ridotti rispetto a un indice non cluster di tabella completa definito sulle stesse colonne chiave.  
  
-   Se in una tabella sono presenti righe di dati eterogenei, è possibile creare un indice filtrato per una o più categorie di dati. In questo modo è possibile migliorare le prestazioni delle query in queste righe di dati restringendo lo stato attivo di una query a un'area specifica della tabella. L'indice risultante sarà di nuovo più piccolo e sarà possibile gestirlo con costi ridotti rispetto a un indice non cluster di tabella completa.  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Non è possibile creare un indice filtrato in una vista. Con Query Optimizer è tuttavia possibile sfruttare i vantaggi offerti da un indice filtrato definito in una tabella a cui si fa riferimento in una vista e, se i risultati della query saranno corretti, viene considerato un indice filtrato per una query per la quale la selezione viene effettuata da una vista.

-   Non è possibile creare un indice filtrato su una tabella quando la colonna a cui si accede nell'espressione di filtro è di un tipo di dati CLR.
  
-   Rispetto alle viste indicizzate gli indici filtrati offrono i vantaggi seguenti:  
  
    -   Costi di manutenzione dell'indice ridotti. Query Processor, ad esempio, utilizza una quantità inferiore di risorse della CPU per aggiornare un indice filtrato rispetto a una vista indicizzata.  
  
    -   Qualità del piano migliorata. Durante la compilazione della query, ad esempio, Query Optimizer preferisce in molte situazioni utilizzare l'indice filtrato anziché la vista indicizzata equivalente.  
  
    -   L'indice online viene ricompilato. È possibile ricompilare gli indici filtrati mentre sono disponibili per le query. La ricompilazione dell'indice online non è supportata per le viste indicizzate. Per altre informazioni, vedere l'opzione REBUILD per [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
    -   Indici non univoci. Gli indici filtrati possono essere non univoci, mentre le viste indicizzate devono essere univoche.  
  
-   Gli indici filtrati sono definiti in una tabella e supportano solo [operatori di confronto](../../t-sql/language-elements/comparison-operators-transact-sql.md) semplici. Se è necessaria un'espressione di filtro in cui viene fatto riferimento a più tabelle o in cui è presente della logica complessa, è necessario creare una vista. Gli indici filtrati non supportano gli operatori `LIKE`. 
  
-   Non è necessario che una colonna nell'espressione che definisce l'indice filtrato sia una colonna chiave o inclusa nella definizione dell'indice stesso se l'espressione che definisce l'indice filtrato è equivalente al predicato della query e la query non restituisce la colonna in tale espressione con i risultati della query.  
  
-   Una colonna nell'espressione che definisce l'indice filtrato deve essere una colonna chiave o inclusa nella definizione dell'indice se il predicato della query la utilizza in un confronto non equivalente all'espressione che definisce l'indice filtrato.  
  
-   Una colonna nell'espressione che definisce l'indice filtrato deve essere una colonna chiave o inclusa nella definizione dell'indice se è presente nel set di risultati della query.  
  
-   Non è necessario che la chiave di indice cluster della tabella sia una colonna chiave o inclusa nella definizione dell'indice filtrato poiché viene inclusa automaticamente in tutti gli indici non cluster, inclusi quelli filtrati.  
  
-   Se l'operatore di confronto specificato nell'espressione che definisce l'indice filtrato determina una conversione dei dati implicita o esplicita, si verificherà un errore se la conversione viene eseguita sul lato sinistro di un operatore di confronto. Una soluzione consiste nello scrivere l'espressione che definisce l'indice filtrato con l'operatore di conversione dei dati (CAST o CONVERT) sul lato destro dell'operatore di confronto.  

- Rivedere le opzioni SET necessarie per la creazione dell'indice filtrato nella sintassi [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È richiesta l'autorizzazione ALTER per la tabella o la vista. L'utente deve essere un membro del ruolo predefinito del server **sysadmin** o dei ruoli predefiniti del database **db_ddladmin** e **db_owner** . Per modificare l'espressione dell'indice filtrato, utilizzare CREATE INDEX WITH DROP_EXISTING.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-create-a-filtered-index"></a>Per creare un indice filtrato  
  
1.  In Esplora oggetti fare clic sul segno più per espandere il database contenente la tabella in cui si desidera creare un indice filtrato.  
  
2.  Fare clic sul segno più per espandere la cartella **Tabelle** .  
  
3.  Fare clic sul segno più per espandere la tabella in cui si desidera creare un indice filtrato.  
  
4.  Fare clic con il pulsante destro del mouse sulla cartella **Indici**, scegliere **Nuovo indice** e selezionare **Indice non cluster**.  
  
5.  Nella pagina **Generale** della finestra di dialogo **Nuovo indice** immettere il nome del nuovo indice nella casella **Nome indice** .  
  
6.  In **Colonne chiave indice** fare clic su **Aggiungi**.  
  
7.  Nella finestra di dialogo **Seleziona colonne da**_nome\_tabella_ selezionare le caselle di controllo delle colonne della tabella da aggiungere all'indice univoco.  
  
8.  Fare clic su **OK**.  
  
9. Nella pagina **Filtro** immettere l'espressione SQL che verrà usata per creare l'indice filtrato in **Espressione filtro**.  
  
10. Fare clic su **OK**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-create-a-filtered-index"></a>Per creare un indice filtrato  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Looks for an existing filtered index named "FIBillOfMaterialsWithEndDate"  
    -- and deletes it from the table Production.BillOfMaterials if found.   
    IF EXISTS (SELECT name FROM sys.indexes  
        WHERE name = N'FIBillOfMaterialsWithEndDate'  
        AND object_id = OBJECT_ID (N'Production.BillOfMaterials'))  
    DROP INDEX FIBillOfMaterialsWithEndDate  
        ON Production.BillOfMaterials  
    GO  
    -- Creates a filtered index "FIBillOfMaterialsWithEndDate"  
    -- on the table Production.BillOfMaterials   
    -- using the columms ComponentID and StartDate.  
  
    CREATE NONCLUSTERED INDEX FIBillOfMaterialsWithEndDate  
        ON Production.BillOfMaterials (ComponentID, StartDate)  
        WHERE EndDate IS NOT NULL ;  
    GO  
    ```  
  
     L'indice filtrato riportato in precedenza è valido per la query seguente. È possibile visualizzare il piano di esecuzione della query per determinare se in Query Optimizer è stato utilizzato l'indice filtrato.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT ProductAssemblyID, ComponentID, StartDate   
    FROM Production.BillOfMaterials  
    WHERE EndDate IS NOT NULL   
        AND ComponentID = 5   
        AND StartDate > '01/01/2008' ;  
    GO  
    ```  
  
#### <a name="to-ensure-that-a-filtered-index-is-used-in-a-sql-query"></a>Per assicurarsi che un indice filtrato venga utilizzato in una query SQL  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT ComponentID, StartDate FROM Production.BillOfMaterials  
        WITH ( INDEX ( FIBillOfMaterialsWithEndDate ) )   
    WHERE EndDate IN ('20000825', '20000908', '20000918');   
    GO  
    ```  
  
 Per altre informazioni, vedere [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
  
