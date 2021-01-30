---
description: COLUMNS (Transact-SQL)
title: COLONNE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- COLUMNS
- COLUMNS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- COLUMNS view
- INFORMATION_SCHEMA.COLUMNS view
ms.assetid: bbf7ac4a-7444-4351-a590-a9f71e0bc495
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 510409a417ff35edb932b0689f6cfb17967ce171
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210270"
---
# <a name="columns-transact-sql"></a>COLUMNS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce una riga per ogni colonna accessibile dall'utente corrente nel database corrente.  
  
 Per recuperare informazioni da queste viste, specificare il nome completo di **INFORMATION_SCHEMA**_.view_name_.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**TABLE_CATALOG**|**nvarchar (** 128 **)**|Qualificatore della tabella.|  
|**TABLE_SCHEMA**|**nvarchar (** 128 **)**|Nome dello schema che contiene la tabella.<br /><br /> **&#42;&#42; importanti &#42;&#42;** Non utilizzare viste INFORMATION_SCHEMA per determinare lo schema di un oggetto. INFORMATION_SCHEMA visualizzazioni rappresentano solo un subset dei metadati di un oggetto. L'unica modalità affidabile per cercare lo schema di un oggetto consiste nell'eseguire una query sulla vista del catalogo sys.objects.|  
|**TABLE_NAME**|**nvarchar (** 128 **)**|Nome della tabella.|  
|**COLUMN_NAME**|**nvarchar (** 128 **)**|Nome colonna.|  
|**ORDINAL_POSITION**|**int**|Numero di identificazione della colonna.|  
|**COLUMN_DEFAULT**|**nvarchar (** 4000 **)**|Valore predefinito della colonna.|  
|**IS_NULLABLE**|**varchar (** 3 **)**|Impostazione relativa al supporto di valori Null nella colonna. Se nella colonna sono consentiti valori NULL, in questa colonna viene restituito YES. In caso contrario, viene restituito NO.|  
|**DATA_TYPE**|**nvarchar (** 128 **)**|Tipo di dati di sistema.|  
|**CHARACTER_MAXIMUM_LENGTH**|**int**|Lunghezza massima espressa in caratteri per i dati di tipo binario, carattere, text o image.<br /><br /> -1 per i dati di tipo **XML** e per valori di grandi dimensioni. Per gli altri tipi di dati viene restituito NULL. Per altre informazioni, vedere [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).|  
|**CHARACTER_OCTET_LENGTH**|**int**|Lunghezza massima espressa in byte per i dati di tipo binario, carattere, text o image.<br /><br /> -1 per i dati di tipo **XML** e per valori di grandi dimensioni. Per gli altri tipi di dati viene restituito NULL.|  
|**NUMERIC_PRECISION**|**tinyint**|Precisione dei dati numerici approssimati, dei dati numerici esatti, dei dati integer o dei dati in valuta. Per gli altri tipi di dati viene restituito NULL.|  
|**NUMERIC_PRECISION_RADIX**|**smallint**|Base di precisione dei dati numerici approssimati, dei dati numerici esatti, dei dati integer o dei dati in valuta. Per gli altri tipi di dati viene restituito NULL.|  
|**NUMERIC_SCALE**|**int**|Scala dei dati numerici approssimati, dei dati numerici esatti, dei dati integer o dei dati in valuta. Per gli altri tipi di dati viene restituito NULL.|  
|**DATETIME_PRECISION**|**smallint**|Codice di sottotipo per i tipi di dati **DateTime** e ISO **Interval** . Per gli altri tipi di dati viene restituito NULL.|  
|**CHARACTER_SET_CATALOG**|**nvarchar (** 128 **)**|Restituisce il **database master**. Indica il database in cui si trova il set di caratteri, se la colonna è di tipo carattere o di dati di **testo** . Per gli altri tipi di dati viene restituito NULL.|  
|**CHARACTER_SET_SCHEMA**|**nvarchar (** 128 **)**|Viene restituito sempre NULL.|  
|**CHARACTER_SET_NAME**|**nvarchar (** 128 **)**|Restituisce il nome univoco del set di caratteri se la colonna è di tipo carattere o di dati di tipo **Text** . Per gli altri tipi di dati viene restituito NULL.|  
|**COLLATION_CATALOG**|**nvarchar (** 128 **)**|Viene restituito sempre NULL.|  
|**COLLATION_SCHEMA**|**nvarchar (** 128 **)**|Viene restituito sempre NULL.|  
|**COLLATION_NAME**|**nvarchar (** 128 **)**|Restituisce il nome univoco per le regole di confronto se la colonna è un tipo di dati carattere o **testo** . Per gli altri tipi di dati viene restituito NULL.|  
|**DOMAIN_CATALOG**|**nvarchar (** 128 **)**|Restituisce il nome del database in cui è stato creato il tipo di dati definito dall'utente se la colonna contiene un tipo di dati alias. Per gli altri tipi di dati viene restituito NULL.|  
|**DOMAIN_SCHEMA**|**nvarchar (** 128 **)**|Restituisce il nome dello schema del tipo di dati definito dall'utente se la colonna contiene un tipo di dati definito dall'utente. Per gli altri tipi di dati viene restituito NULL.<br /><br /> **&#42;&#42; importanti &#42;&#42;** Non utilizzare viste INFORMATION_SCHEMA per determinare lo schema di un tipo di dati. L'unica modalità affidabile per cercare lo schema di un tipo consiste nell'utilizzare la funzione TYPEPROPERTY.|  
|**DOMAIN_NAME**|**nvarchar (** 128 **)**|Restituisce il nome del tipo di dati definito dall'utente se la colonna contiene un tipo di dati definito dall'utente. Per gli altri tipi di dati viene restituito NULL.|  
  
## <a name="remarks"></a>Commenti  
 Colonna **ORDINAL_POSITION** della **INFORMATION_SCHEMA.** La visualizzazione colonne non è compatibile con lo schema di bit delle colonne restituite dalla funzione COLUMNS_UPDATED. Per ottenere uno schema di bit compatibile con COLUMNS_UPDATED, è necessario fare riferimento alla proprietà **ColumnID** della funzione di sistema COLUMNPROPERTY quando si esegue una query sull' **INFORMATION_SCHEMA. Visualizzazione colonne** . Ad esempio:  
  
```  
USE AdventureWorks2012;  
GO  
SELECT TABLE_NAME, COLUMN_NAME, COLUMNPROPERTY(OBJECT_ID(TABLE_SCHEMA + '.' + TABLE_NAME), COLUMN_NAME, 'ColumnID') AS COLUMN_ID  
FROM AdventureWorks2012.INFORMATION_SCHEMA.COLUMNS  
WHERE TABLE_NAME = 'Person';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Viste di sistema &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)   
 [Viste degli schemi delle informazioni &#40;&#41;Transact-SQL ](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [ Set di caratterisys.sys&#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syscharsets-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)   
 [COLUMNS_UPDATED &#40;Transact-SQL&#41;](../../t-sql/functions/columns-updated-transact-sql.md)  
  
