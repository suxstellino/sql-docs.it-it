---
description: sys.syscolumns (Transact-SQL)
title: Colonne sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.syscolumns
- sys.syscolumns_TSQL
- syscolumns_TSQL
- syscolumns
dev_langs:
- TSQL
helpviewer_keywords:
- syscolumns system table
- sys.syscolumns compatibility view
ms.assetid: 863fd87b-ff33-4ac5-9aa9-df21140681da
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b37f7981477541ee02b044ef86a8654ace544811
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755501"
---
# <a name="syssyscolumns-transact-sql"></a>sys.syscolumns (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Restituisce una riga per ogni colonna di ogni tabella e vista e una riga per ogni parametro di una stored procedure nel database.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**sysname**|Nome della colonna o del parametro della procedura.|  
|**id**|**int**|ID di oggetto della tabella a cui appartiene la colonna o ID della stored procedure a cui è associato il parametro.|  
|**xtype**|**tinyint**|Tipo di archiviazione fisica da **sys. Types**.|  
|**typestat**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xusertype**|**smallint**|ID del tipo di dati esteso definito dall'utente. Causa un errore di overflow o restituisce NULL se il numero di tipi di dati è maggiore di 32.767.|  
|**length**|**smallint**|Lunghezza massima di archiviazione fisica da **sys**. **tipi**.|  
|**xprec**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xscale**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**colid**|**smallint**|ID di colonna o di parametro.|  
|**xoffset**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**bitpos**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**riservati**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**colstat**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**cdefault**|**int**|ID del valore predefinito della colonna.|  
|**dominio**|**int**|ID della regola o vincolo CHECK per la colonna.|  
|**number**|**smallint**|Numero di sottoprocedura quando la procedura è raggruppata.<br /><br /> 0 = Voci non di procedura|  
|**colorder**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**autoval**|**varbinary(8000)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**offset**|**smallint**|Offset nella riga in cui appare la colonna.|  
|**CollationID**|**int**|ID delle regole di confronto della colonna. NULL per le colonne non di tipo carattere.|  
|**Stato**|**tinyint**|Mappa di bit utilizzata per descrivere una proprietà della colonna o del parametro:<br /><br /> 0x08 = La colonna ammette valori Null.<br /><br /> 0x10 = il riempimento ANSI era attivo quando sono state aggiunte colonne **varchar** o **varbinary** . Gli spazi vuoti finali vengono conservati per **varchar** e gli zeri finali vengono conservati per le colonne **varbinary** .<br /><br /> 0x40 = Il parametro è un parametro OUTPUT.<br /><br /> 0x80 = La colonna è una colonna Identity.|  
|**type**|**tinyint**|Tipo di archiviazione fisica da **sys**. **tipi**.|  
|**UserType**|**smallint**|ID del tipo di dati definito dall'utente da **sys. Types**. Causa un errore di overflow o restituisce NULL se il numero di tipi di dati è maggiore di 32.767.|  
|**printfmt**|**varchar(255)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**prec**|**smallint**|Livello di precisione della colonna.<br /><br /> -1 = **XML** o tipo di valore di grandi dimensioni.|  
|**scale**|**int**|Scala della colonna.<br /><br /> NULL = Tipo di dati non numerico.|  
|**IsComputed**|**int**|Flag che indica se si tratta di una colonna calcolata:<br /><br /> 0 = Non calcolata<br /><br /> 1 = Calcolata|  
|**isoutparam**|**int**|Indica se il parametro della procedura è un parametro di output:<br /><br /> 1 = True<br /><br /> 0 = False|  
|**IsNullable**|**int**|Indica se la colonna ammette valori Null:<br /><br /> 1 = True<br /><br /> 0 = False|  
|**confronto**|**sysname**|Nome delle regole di confronto della colonna. NULL se non si tratta di una colonna di tipo carattere.|  
  
## <a name="see-also"></a>Vedere anche  
 [Mapping di tabelle di sistema a viste di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Viste della compatibilità &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
