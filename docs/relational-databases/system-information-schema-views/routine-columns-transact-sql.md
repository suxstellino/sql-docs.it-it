---
description: ROUTINE_COLUMNS (Transact-SQL)
title: ROUTINE_COLUMNS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- ROUTINE_COLUMNS_TSQL
- ROUTINE_COLUMNS
dev_langs:
- TSQL
helpviewer_keywords:
- ROUTINE_COLUMNS view
- INFORMATION_SCHEMA.ROUTINE_COLUMNS view
ms.assetid: 91dbc61b-e4c0-4826-976c-b2fce88b7793
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f2a60f579dd5769d4c555e79de1b67cfd75660ca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189593"
---
# <a name="routine_columns-transact-sql"></a>ROUTINE_COLUMNS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce una riga per ogni colonna restituita dalle funzioni con valori di tabella a cui può accedere l'utente corrente del database corrente.  
  
 Per recuperare informazioni da questa visualizzazione, specificare il nome completo del **INFORMATION_SCHEMA.** _view_name_.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**TABLE_CATALOG**|**nvarchar (** 128 **)**|Nome del catalogo o del database della funzione con valori di tabella.|  
|**TABLE_SCHEMA**|**nvarchar (** 128 **)**|Nome dello schema che contiene la funzione con valori di tabella.<br /><br /> Importante non utilizzare viste INFORMATION_SCHEMA per determinare lo schema di un oggetto. <strong> \* \* \* \* </strong> INFORMATION_SCHEMA visualizzazioni rappresentano solo un subset dei metadati di un oggetto. L'unica modalità affidabile per cercare lo schema di un oggetto consiste nell'eseguire una query sulla vista del catalogo sys.objects.|  
|**TABLE_NAME**|**nvarchar (** 128 **)**|Nome della funzione con valori di tabella.|  
|**COLUMN_NAME**|**nvarchar (** 128 **)**|Nome colonna.|  
|**ORDINAL_POSITION**|**int**|Numero di identificazione della colonna.|  
|**COLUMN_DEFAULT**|**nvarchar (** 4000 **)**|Valore predefinito della colonna.|  
|**IS_NULLABLE**|**varchar (** 3 **)**|Se la colonna ammette valori NULL, restituisce YES. In caso contrario restituisce NO.|  
|**DATA_TYPE**|**nvarchar (** 128 **)**|Tipo di dati di sistema.|  
|**CHARACTER_MAXIMUM_LENGTH**|**int**|Lunghezza massima espressa in caratteri per i dati di tipo binario, carattere, text o image.<br /><br /> -1 per i dati di tipo **XML** e per valori di grandi dimensioni. In caso contrario, viene restituito NULL. Per altre informazioni, vedere [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).|  
|**CHARACTER_OCTET_LENGTH**|**int**|Lunghezza massima espressa in byte per i dati di tipo binario, carattere, text o image.<br /><br /> -1 per i dati di tipo **XML** e per valori di grandi dimensioni. In caso contrario, viene restituito NULL.|  
|**NUMERIC_PRECISION**|**tinyint**|Precisione dei dati numerici approssimati, dei dati numerici esatti, dei dati integer o dei dati in valuta. In caso contrario, viene restituito NULL.|  
|**NUMERIC_PRECISION_RADIX**|**smallint**|Base di precisione dei dati numerici approssimati, dei dati numerici esatti, dei dati integer o dei dati in valuta. In caso contrario, viene restituito NULL.|  
|**NUMERIC_SCALE**|**tinyint**|Scala dei dati numerici approssimati, dei dati numerici esatti, dei dati integer o dei dati in valuta. In caso contrario, viene restituito NULL.|  
|**DATETIME_PRECISION**|**smallint**|Codice di sottotipo per i tipi di dati **Integer** **DateTime** e ISO. Per gli altri tipi di dati restituisce NULL.|  
|**CHARACTER_SET_CATALOG**|**varchar (** 6 **)**|Restituisce il **database master**. Indica il database in cui si trova il set di caratteri se la colonna è di tipo carattere o di dati di tipo **Text** . In caso contrario, viene restituito NULL.|  
|**CHARACTER_SET_SCHEMA**|**varchar (** 3 **)**|Viene restituito sempre NULL.|  
|**CHARACTER_SET_NAME**|**nvarchar (** 128 **)**|Restituisce il nome univoco del set di caratteri se la colonna è di tipo carattere o di dati di tipo **Text** . In caso contrario, viene restituito NULL.|  
|**COLLATION_CATALOG**|**varchar (** 6 **)**|Viene restituito sempre NULL.|  
|**COLLATION_SCHEMA**|**varchar (** 3 **)**|Viene restituito sempre NULL.|  
|**COLLATION_NAME**|**nvarchar (** 128 **)**|Restituisce il nome univoco per l'ordinamento se la colonna è di tipo carattere o di dati di **testo** . In caso contrario, viene restituito NULL.|  
|**DOMAIN_CATALOG**|**nvarchar (** 128 **)**|Restituisce il nome del database in cui è stato creato il tipo di dati definito dall'utente se la colonna contiene un tipo di dati alias. In caso contrario, viene restituito NULL.|  
|**DOMAIN_SCHEMA**|**nvarchar (** 128 **)**|Se il tipo di dati della colonna è un tipo definito dall'utente, restituisce il nome dello schema contenente il tipo di dati definito dall'utente. In caso contrario, viene restituito NULL.<br /><br /> Importante non utilizzare viste INFORMATION_SCHEMA per determinare lo schema di un oggetto. <strong> \* \* \* \* </strong> INFORMATION_SCHEMA visualizzazioni rappresentano solo un subset dei metadati di un oggetto. L'unica modalità affidabile per cercare lo schema di un oggetto consiste nell'eseguire una query sulla vista del catalogo sys.objects.|  
|**DOMAIN_NAME**|**nvarchar (** 128 **)**|Restituisce il nome del tipo di dati definito dall'utente se la colonna contiene un tipo di dati definito dall'utente. In caso contrario, viene restituito NULL.|  
  
## <a name="see-also"></a>Vedere anche  
 [Viste di sistema &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)   
 [Viste degli schemi delle informazioni &#40;&#41;Transact-SQL ](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)  
  
