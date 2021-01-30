---
description: SET STATISTICS IO (Transact-SQL)
title: SET STATISTICS IO (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET_STATISTICS_IO_TSQL
- IO
- IO_TSQL
- SET STATISTICS IO
dev_langs:
- TSQL
helpviewer_keywords:
- disk I/O statistics [SQL Server]
- I/O [SQL Server], disk activity information
- disks [SQL Server], statement statistics
- STATISTICS IO option
- statements [SQL Server], statistical information
- SET STATISTICS IO statement
- statistical information [SQL Server], disk activity
ms.assetid: 7033aac9-a944-4156-9ff4-6ef65717a28b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 27084c92d8c3dfcd8b85317d1d2a9d0a70dd713d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206937"
---
# <a name="set-statistics-io-transact-sql"></a>SET STATISTICS IO (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Determina la visualizzazione in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di informazioni sulla quantità di attività del disco generata dalle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
SET STATISTICS IO { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Osservazioni
 Quando STATISTICS IO è impostato su ON, le informazioni statistiche vengono visualizzate, mentre quando è impostato su OFF le informazioni non vengono visualizzate.   
  
 Dopo che questa opzione è stata impostata su ON, tutte le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] restituiscono le informazioni statistiche fino a quando l'opzione non viene impostata su OFF.  
  
 Nella tabella seguente viene visualizzato un elenco di elementi di output e la relativa descrizione.  
  
|Elemento di output|Significato|  
|-----------------|-------------|  
|**Tabella**|Nome della tabella.|  
|**Scan count**|Numero di ricerche o analisi avviate dopo aver raggiunto il livello foglia in qualsiasi direzione per recuperare tutti i valori al fine di costruire il set di dati finale per l'output.<br /><br /> Il conteggio analisi è 0 se l'indice usato è un indice univoco o cluster in una chiave primaria e se si sta cercando un solo valore. Ad esempio: `WHERE Primary_Key_Column = <value>`.<br /><br /> Il conteggio analisi è 1 quando si cerca un valore usando un indice cluster non univoco definito in una colonna chiave non primaria. Questo processo viene eseguito per verificare la presenza di valori duplicati per il valore di chiave che si sta cercando. Ad esempio: `WHERE Clustered_Index_Key_Column = <value>`.<br /><br /> Il conteggio analisi è N quando N è il numero delle diverse ricerche o analisi avviate sul lato sinistro o destro al livello foglia dopo aver individuato un valore di chiave usando la chiave dell'indice.|  
|**logical reads**|Numero di pagine lette dalla cache dei dati.|  
|**physical reads**|Numero di pagine lette dal disco.|  
|**read-ahead reads**|Numero di pagine inserite nella cache per la query.|  
|**lob logical reads**|Numero di pagine lette dalla cache dei dati. Include **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)** o pagine di indice columnstore.|  
|**lob physical reads**|Numero di pagine lette dal disco. Include **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)** o pagine di indice columnstore.|  
|**lob read-ahead reads**|Numero di pagine inserite nella cache per la query. Include **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)** o pagine di indice columnstore.|

 L'opzione SET STATISTICS IO viene impostata in fase di esecuzione, non in fase di analisi.

> [!NOTE]  
> Durante il recupero di colonne LOB da parte di istruzioni Transact-SQL, alcune operazioni di recupero possono richiedere più volte l'attraversamento dell'albero LOB. Per questo motivo SET STATISTICS IO può segnalare un numero di letture logiche superiore al previsto.

## <a name="permissions"></a>Autorizzazioni  
 Per utilizzare l'opzione SET STATISTICS IO, gli utenti devono disporre delle autorizzazioni appropriate per eseguire l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'autorizzazione SHOWPLAN non è necessaria.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene illustrato il numero di letture logiche e fisiche utilizzate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per l'elaborazione delle istruzioni.  
  
```sql
USE AdventureWorks2012;  
GO         
SET STATISTICS IO ON;  
GO  
SELECT *   
FROM Production.ProductCostHistory  
WHERE StandardCost < 500.00;  
GO  
SET STATISTICS IO OFF;  
GO  
```  
  
 Set di risultati:  
  
```  
Table 'ProductCostHistory'. Scan count 1, logical reads 5, physical   
reads 0, read-ahead reads 0, lob logical reads 0, lob physical reads 0,   
lob read-ahead reads 0.  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET SHOWPLAN_ALL &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-all-transact-sql.md)   
 [SET STATISTICS TIME &#40;Transact-SQL&#41;](../../t-sql/statements/set-statistics-time-transact-sql.md)  
  
  
