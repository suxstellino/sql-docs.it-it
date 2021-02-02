---
title: Visualizzare le proprietà delle statistiche | Microsoft Docs
description: Informazioni su come visualizzare le statistiche di ottimizzazione query correnti per una tabella o una vista indicizzata in SQL Server usando SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.swb.statistics.details.f1
helpviewer_keywords:
- viewing statistics properties
- statistics [SQL Server], viewing properties
ms.assetid: 0eaa2101-006e-4015-9979-3468b50e0aaa
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 440b2e3dd7bcf7d1c7934a32ff95f9cd2299602d
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250224"
---
# <a name="view-statistics-properties"></a>Visualizzare le proprietà delle statistiche
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  È possibile visualizzare le statistiche di ottimizzazione delle query correnti per una tabella o una vista indicizzata in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'oggetto statistiche include un'intestazione con metadati relativi alle statistiche, un istogramma con la distribuzione dei valori nella prima colonna chiave dell'oggetto stesso e un vettore di densità per misurare la correlazione tra colonne. Per altre informazioni sugli istogrammi e sui vettori di densità, vedere [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Sicurezza](#Security)  
  
-   **Visualizzare le proprietà delle statistiche tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Per visualizzare l'oggetto statistiche, l'utente deve essere il proprietario della tabella oppure un membro del ruolo predefinito del server **sysadmin** o del ruolo predefinito del database **db_owner** o **db_ddladmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-view-statistics-properties"></a>Per visualizzare le proprietà delle statistiche  
  
1.  In **Esplora oggetti** fare clic sul segno più per espandere il database in cui si desidera creare una nuova statistica.  
  
2.  Fare clic sul segno più per espandere la cartella **Tabelle** .  
  
3.  Fare clic sul segno più per espandere la tabella in cui verranno visualizzate le proprietà della statistica.  
  
4.  Fare clic sul segno più per espandere la cartella **Statistiche** .  
  
5.  Fare clic con il pulsante destro del mouse sull'oggetto statistiche di cui si vogliono visualizzare le proprietà e scegliere **Proprietà**.  
  
6.  Nella finestra di dialogo **Proprietà statistiche -** _nome_statistiche_ selezionare **Dettagli** nel riquadro **Seleziona una pagina**.  
  
     Nella pagina **Dettagli** della finestra di dialogo **Proprietà statistiche -** _nome_statistiche_ vengono visualizzate le proprietà seguenti.  
  
     **Nome tabella**  
     Consente di visualizzare il nome della tabella descritta dalle statistiche.  
  
     **Nome statistiche**  
     Consente di visualizzare il nome dell'oggetto di database in cui sono archiviate le statistiche.  
  
     **Statistiche per l'indice nome_statistiche**  
     Questa casella di testo consente di visualizzare le proprietà restituite dall'oggetto statistiche. Queste proprietà sono divise in tre sezioni: Stats Header, Density Vector, and Histogram (Intestazione statistiche, Vettore di densità e Istogramma).  
  
     Tramite le informazioni seguenti vengono descritte le colonne restituite nel set di risultati per l'intestazione delle statistiche.  
  
     **Nome**  
     Nome dell'oggetto statistiche.  
  
     **Aggiornato**  
     Data e ora dell'ultimo aggiornamento delle statistiche.  
  
     **prime righe**  
     Numero totale di righe della tabella o della vista indicizzata al momento dell'ultimo aggiornamento delle statistiche. Se le statistiche vengono filtrate o corrispondono a un indice filtrato, il numero di righe potrebbe essere inferiore al numero di righe della tabella.  
  
     **Rows Sampled**  
     Numero totale di righe campionate per i calcoli statistici. Se Rows Sampled < Rows, l'istogramma e i risultati relativi alla densità visualizzati vengono stimati in base alle righe campionate.  
  
     **Passaggi**  
     Numero di intervalli nell'istogramma. Ogni intervallo comprende un insieme di valori di colonna seguiti da un valore di colonna pari al limite superiore. Gli intervalli dell'istogramma vengono definiti nella prima colonna chiave delle statistiche. Il numero massimo di intervalli è 200.  
  
     **Density**  
     Valore calcolato come 1/ *valori distinct* per tutti i valori nella prima colonna chiave dell'oggetto statistiche, ad eccezione dei valori limite dell'istogramma. Questo valore Density non viene utilizzato da Query Optimizer e viene visualizzato per compatibilità con le versioni precedenti rispetto a SQL Server 2008.  
  
     **Average Key Length**  
     Numero medio di byte per valore per tutte le colonne chiave nell'oggetto statistiche.  
  
     **String Index**  
     Il valore Yes indica che l'oggetto statistiche contiene statistiche di riepilogo delle stringhe per migliorare le stime relative alla cardinalità per i predicati della query che utilizzano l'operatore LIKE, ad esempio `WHERE ProductName LIKE '%Bike'`. Le statistiche di riepilogo delle stringhe vengono archiviate separatamente rispetto all'istogramma e vengono create nella prima colonna chiave dell'oggetto statistiche quando tale colonna è di tipo **char**, **varchar**, **nchar**, **nvarchar**, **varchar(max)** , **nvarchar(max)** , **text** o **ntext**.  
  
     **Espressione filtro**  
     Predicato per il subset di righe della tabella incluso nell'oggetto statistiche. NULL = statistiche non filtrate.  
  
     **Unfiltered Rows**  
     Numero totale di righe nella tabella prima dell'applicazione dell'espressione di filtro. Se l'espressione di filtro è NULL, Unfiltered Rows è uguale a Rows.  
  
     Tramite le informazioni seguenti vengono descritte le colonne restituite nel set di risultati per il vettore di densità.  
  
     **All Density**  
     Il valore Density viene calcolato come 1/ *valori distinct*. Nei risultati la densità viene visualizzata per ogni prefisso di colonna dell'oggetto statistiche, una riga per ogni densità. Un valore distinct è un elenco distinto dei valori delle colonne per riga e per prefisso di colonna. Se l'oggetto statistiche contiene, ad esempio, le colonne chiave (A, B, C), i risultati restituiscono la densità degli elenchi di valori distinct in ognuno di tali prefissi di colonna, ovvero (A), (A,B) e (A, B, C). Usando il prefisso (A, B, C), ciascuno di questi elenchi è un elenco di valori distinct, ovvero (3, 5, 6), (4, 4, 6), (4, 5, 6), (4, 5, 7). Usando il prefisso (A, B) agli stessi valori di colonna sono associati gli elenchi di valori distinct (3, 5), (4, 4), e (4, 5).  
  
     **Average Length**  
     Lunghezza media, in byte, per archiviare un elenco di valori di colonna per il prefisso di colonna. Se per ogni valore presente nell'elenco (3, 5, 6), ad esempio, sono necessari 4 byte, la lunghezza media è di 12 byte.  
  
     **Colonne**  
     Nomi delle colonne nel prefisso per cui sono visualizzati i valori di All Density e Average Length.  
  
     Tramite le informazioni seguenti vengono descritte le colonne restituite nel set di risultati per l'istogramma.  
  
     **RANGE_HI_KEY**  
     Valore di colonna pari al limite superiore per un intervallo dell'istogramma. Il valore di colonna viene denominato anche valore chiave.  
  
     **RANGE_ROWS**  
     Numero stimato di righe il cui valore di colonna è compreso in un intervallo dell'istogramma, escluso il limite superiore.  
  
     **EQ_ROWS**  
     Numero stimato di righe il cui valore di colonna è uguale al limite superiore dell'intervallo dell'istogramma.  
  
     **DISTINCT_RANGE_ROWS**  
     Numero stimato di righe con un valore distinct di colonna compreso in un intervallo dell'istogramma, escluso il limite superiore.  
  
     **AVG_RANGE_ROWS**  
     Numero medio di righe con valori di colonna duplicati compresi in un intervallo dell'istogramma, escluso il limite superiore (RANGE_ROWS / DISTINCT_RANGE_ROWS per DISTINCT_RANGE_ROWS > 0).  
  
7.  Fare clic su **OK**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-view-statistics-properties"></a>Per visualizzare le proprietà delle statistiche  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- The following example displays all statistics information for the AK_Address_rowguid index of the Person.Address table.   
    DBCC SHOW_STATISTICS ("Person.Address", AK_Address_rowguid);   
    GO  
    ```  
  
 Per altre informazioni, vedere [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md).  
  
#### <a name="to-find-all-of-the-statistics-on-a-table-or-view"></a>Per trovare tutte le statistiche su una tabella o una vista  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    /*Gets the following information: name and ID of the statistics, whether the statistics were created automatically or by the user, whether the statistics were created with the NORECOMPUTE option, and whether the statistics have a filter and, if so, what that filter is.  
    */  
    SELECT name AS statistics_name  
        ,stats_id  
        ,auto_created  
        ,user_created  
        ,no_recompute  
        ,has_filter  
        ,filter_definition  
    -- using the sys.stats catalog view  
    FROM sys.stats  
    -- for the Sales.SpecialOffer table  
    WHERE object_id = OBJECT_ID('Sales.SpecialOffer');  
    GO  
    ```  
  
 Per altre informazioni, vedere [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md).  
  
  
