---
description: INDEXPROPERTY (Transact-SQL)
title: INDEXPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- INDEXPROPERTY
- INDEXPROPERTY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- INDEXPROPERTY function
- indexes [SQL Server], viewing
- indexes [SQL Server], properties
ms.assetid: 998d5788-4871-44a8-8125-0d9390868b84
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f5a6a6491baa20201ead80fe59b85ae5697624b3
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237452"
---
# <a name="indexproperty-transact-sql"></a>INDEXPROPERTY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce il valore delle proprietà di indice o statistiche specificate in base al numero di identificazione della tabella, al nome dell'indice o delle statistiche e al nome della proprietà. Restituisce NULL per gli indici XML.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
INDEXPROPERTY ( object_ID , index_or_statistics_name , property )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *object_ID*  
 Espressione che contiene il numero di identificazione della tabella o vista indicizzata di cui si desidera ottenere informazioni sulla proprietà dell'indice. *object_ID* è di tipo **int**.  
  
 *index_or_statistics_name*  
 Espressione che contiene il nome dell'indice o delle statistiche per cui si desidera ottenere informazioni sulle proprietà. *index_or_statistics_name* è di tipo **nvarchar(128)**.  
  
 *property*  
 Espressione che contiene il nome della proprietà del database da restituire. *property* è di tipo **varchar(128)**. I valori possibili sono i seguenti.  
  
> [!NOTE]  
>  Se non specificato diversamente, viene restituito NULL quando *property* non è un nome di proprietà valido, *object_ID* non è un ID di oggetto valido, *object_ID* è un tipo di oggetto non supportato per la proprietà specificata oppure il chiamante non ha l'autorizzazione necessaria per visualizzare i metadati dell'oggetto.  
  
|Proprietà|Descrizione|valore|  
|--------------|-----------------|-----------|  
|**IndexDepth**|Dettagli dell'indice.|Numero di livelli dell'indice.<br /><br /> NULL = indice XML o input non valido.|  
|**IndexFillFactor**|Valore del fattore di riempimento utilizzato quando l'indice è stato creato o ricompilato per l'ultima volta.|Fattore di riempimento|  
|**IndexID**|ID dell'indice della tabella o vista indicizzata specificata.|ID dell'indice|  
|**IsAutoStatistics**|Le statistiche sono state generate mediante l'opzione AUTO_CREATE_STATISTICS di ALTER DATABASE.|1 = True<br /><br /> 0 = False o indice XML.|  
|**IsClustered**|Indice cluster.|1 = True<br /><br /> 0 = False o indice XML.|  
|**IsDisabled**|L'indice è disabilitato.|1 = True<br /><br /> 0 = False<br /><br /> NULL = Input non valido.|  
|**IsFulltextKey**|L'indice è la chiave di indicizzazione full-text e semantica per una tabella.|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> 1 = True<br /><br /> 0 = False o indice XML.<br /><br /> NULL = Input non valido.|  
|**IsHypothetical**|L'indice è ipotetico e non può essere utilizzato direttamente come percorso di accesso ai dati. Gli indici ipotetici contengono statistiche a livello di colonna e vengono gestiti e utilizzati da Ottimizzazione guidata motore di database.|1 = True<br /><br /> 0 = false o indice XML.<br /><br /> NULL = Input non valido.|  
|**IsPadIndex**|All'indice è associata la quantità di spazio da lasciare aperto in ogni nodo interno.|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> 1 = True<br /><br /> 0 = False o indice XML.|  
|**IsPageLockDisallowed**|Valore per il blocco di pagina impostato mediante l'opzione ALLOW_PAGE_LOCKS di ALTER INDEX.|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> 1 = il blocco di pagina è disattivato.<br /><br /> 0 = il blocco di pagina è attivato.<br /><br /> NULL = Input non valido.|  
|**IsRowLockDisallowed**|Valore per il blocco di riga impostato tramite l'opzione ALLOW_ROW_LOCKS di ALTER INDEX.|**Si applica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive.<br /><br /> 1 = il blocco di riga è disattivato.<br /><br /> 0 = il blocco di riga è attivato.<br /><br /> NULL = Input non valido.|  
|**IsStatistics**|*index_or_statistics_name* rappresenta le statistiche create dall'istruzione CREATE STATISTICS o dall'opzione AUTO_CREATE_STATISTICS di ALTER DATABASE.|1 = True<br /><br /> 0 = False o indice XML.|  
|**IsUnique**|Indice univoco.|1 = True<br /><br /> 0 = False o indice XML.|  
|**IsColumnstore**|L'indice è un indice columnstore con ottimizzazione per la memoria xVelocity.|**Si applica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive.<br /><br /> 1 = True<br /><br /> 0 = False| 
|**IsOptimizedForSequentialKey**|L'ottimizzazione dell'indice per gli inserimenti dell'ultima pagina è abilitata.|**Si applica a**: [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] e versioni successive. <br><br>1 = True<br><br>0 = False| 
  
## <a name="return-types"></a>Tipi restituiti  
 **int**  
  
## <a name="exceptions"></a>Eccezioni  
 Restituisce NULL in caso di errore o se un chiamante non dispone dell'autorizzazione necessaria per visualizzare l'oggetto.  
  
 Un utente può visualizzare esclusivamente i metadati delle entità a sicurezza diretta di cui è proprietario o per cui ha ricevuto un'autorizzazione. Di conseguenza, le funzioni predefinite di creazione dei metadati come INDEXPROPERTY possono restituire NULL se l'utente non dispone di alcuna autorizzazione per l'oggetto. Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente vengono restituiti i valori delle proprietà **IsClustered**, **IndexDepth** e **IndexFillFactor** per l'indice `PK_Employee_BusinessEntityID` della tabella `Employee` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT   
    INDEXPROPERTY(OBJECT_ID('HumanResources.Employee'),  
        'PK_Employee_BusinessEntityID','IsClustered')AS [Is Clustered],  
    INDEXPROPERTY(OBJECT_ID('HumanResources.Employee'),  
        'PK_Employee_BusinessEntityID','IndexDepth') AS [Index Depth],  
    INDEXPROPERTY(OBJECT_ID('HumanResources.Employee'),  
        'PK_Employee_BusinessEntityID','IndexFillFactor') AS [Fill Factor];  
  
```  
  
 Set di risultati:  
  
```  
Is Clustered Index Depth Fill Factor   
------------ ----------- -----------   
1            2           0  
  
(1 row(s) affected)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Nell'esempio seguente vengono esaminate le proprietà di uno degli indici nella tabella `FactResellerSales`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT   
INDEXPROPERTY(OBJECT_ID('dbo.FactResellerSales'),  
    'ClusteredIndex_6d10fa223e5e4c1fbba087e29e16a7a2','IsClustered') AS [Is Clustered],  
INDEXPROPERTY(OBJECT_ID('dbo.FactResellerSales'),  
    'ClusteredIndex_6d10fa223e5e4c1fbba087e29e16a7a2','IsColumnstore') AS [Is Columnstore Index],  
INDEXPROPERTY(OBJECT_ID('dbo.FactResellerSales'),  
    'ClusteredIndex_6d10fa223e5e4c1fbba087e29e16a7a2','IndexFillFactor') AS [Fill Factor];  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [Statistiche](../../relational-databases/statistics/statistics.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
 [sys.stats_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md)  
  
  

