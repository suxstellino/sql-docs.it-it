---
description: ROUTINES (Transact-SQL)
title: ROUTINE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- ROUTINES_TSQL
- ROUTINES
dev_langs:
- TSQL
helpviewer_keywords:
- ROUTINES view
- INFORMATION_SCHEMA.ROUTINES view
ms.assetid: c75561b2-c9a1-48a1-9afa-a5896b6454cf
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 25ea15fd105733e04dd571dc925df394b1520d47
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192087"
---
# <a name="routines-transact-sql"></a>ROUTINES (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce una riga per ogni stored procedure e funzione accessibile dall'utente corrente nel database corrente. Le colonne in cui viene descritto il valore restituito sono valide solo per le funzioni. Per le stored procedure in queste colonne viene restituito NULL.  
  
 Per recuperare informazioni da queste viste, specificare il nome completo del INFORMATION_SCHEMA. *view_name*.  
  
> [!NOTE]  
>  La colonna ROUTINE_DEFINITION include le istruzioni di origine con cui è stata creata la funzione o la stored procedure. È probabile che queste istruzioni contengano ritorni a capo incorporati. Se questa colonna viene restituita a un'applicazione che visualizza i risultati in un formato testo, i ritorni a capo incorporati nei risultati di ROUTINE_DEFINITION possono influire sulla formattazione del set di risultati. Se si seleziona la colonna ROUTINE_DEFINITION, apportare le modifiche necessarie per i ritorni a capo incorporati, ad esempio restituendo il set di risultati in una griglia oppure restituendo ROUTINE_DEFINITION in una casella di testo specifica.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|SPECIFIC_CATALOG|**nvarchar (** 128 **)**|Nome specifico del catalogo. Questo nome corrisponde a ROUTINE_CATALOG.|  
|SPECIFIC_SCHEMA|**nvarchar (** 128 **)**|Nome specifico dello schema.<br /><br /> Importante non utilizzare viste INFORMATION_SCHEMA per determinare lo schema di un oggetto. **\* \* \* \*** INFORMATION_SCHEMA visualizzazioni rappresentano solo un subset dei metadati di un oggetto. L'unica modalità affidabile per cercare lo schema di un oggetto consiste nell'eseguire una query sulla vista del catalogo sys.objects.|  
|SPECIFIC_NAME|**nvarchar (** 128 **)**|Nome specifico del catalogo. Questo nome corrisponde a ROUTINE_NAME.|  
|ROUTINE_CATALOG|**nvarchar (** 128 **)**|Nome del catalogo della funzione.|  
|ROUTINE_SCHEMA|**nvarchar (** 128 **)**|Nome dello schema che contiene la funzione.<br /><br /> Importante non utilizzare viste INFORMATION_SCHEMA per determinare lo schema di un oggetto. **\* \* \* \*** INFORMATION_SCHEMA visualizzazioni rappresentano solo un subset dei metadati di un oggetto. L'unica modalità affidabile per cercare lo schema di un oggetto consiste nell'eseguire una query sulla vista del catalogo sys.objects.|  
|ROUTINE_NAME|**nvarchar (** 128 **)**|Nome della funzione.|  
|ROUTINE_TYPE|**nvarchar (** 20 **)**|Restituisce PROCEDURE per le stored procedure e FUNCTION per le funzioni.|  
|MODULE_CATALOG|**nvarchar (** 128 **)**|NULL Riservato per utilizzi futuri.|  
|MODULE_SCHEMA|**nvarchar (** 128 **)**|NULL Riservato per utilizzi futuri.|  
|MODULE_NAME|**nvarchar (** 128 **)**|NULL Riservato per utilizzi futuri.|  
|UDT_CATALOG|**nvarchar (** 128 **)**|NULL Riservato per utilizzi futuri.|  
|UDT_SCHEMA|**nvarchar (** 128 **)**|NULL Riservato per utilizzi futuri.|  
|UDT_NAME|**nvarchar (** 128 **)**|NULL Riservato per utilizzi futuri.|  
|DATA_TYPE|**nvarchar (** 128 **)**|Tipo di dati del valore restituito dalla funzione. Restituisce **Table** se una funzione con valori di tabella.|  
|CHARACTER_MAXIMUM_LENGTH|**int**|Lunghezza massima, espressa in caratteri, se viene restituito un tipo di dati character.<br /><br /> -1 per i dati di tipo **XML** e per valori di grandi dimensioni.|  
|CHARACTER_OCTET_LENGTH|**int**|Lunghezza massima, espressa in byte, se viene restituito un tipo di dati character.<br /><br /> -1 per i dati di tipo **XML** e per valori di grandi dimensioni.|  
|COLLATION_CATALOG|**nvarchar (** 128 **)**|Viene restituito sempre NULL.|  
|COLLATION_SCHEMA|**nvarchar (** 128 **)**|Viene restituito sempre NULL.|  
|COLLATION_NAME|**nvarchar (** 128 **)**|Nome delle regole di confronto del valore restituito. Per i tipi di dati diversi da character viene restituito NULL.|  
|CHARACTER_SET_CATALOG|**nvarchar (** 128 **)**|Viene restituito sempre NULL.|  
|CHARACTER_SET_SCHEMA|**nvarchar (** 128 **)**|Viene restituito sempre NULL.|  
|CHARACTER_SET_NAME|**nvarchar (** 128 **)**|Nome del set di caratteri del valore restituito. Per i tipi di dati diversi da character viene restituito NULL.|  
|NUMERIC_PRECISION|**smallint**|Precisione numerica del valore restituito. Per i tipi di dati non numerici viene restituito NULL.|  
|NUMERIC_PRECISION_RADIX|**smallint**|Radice di precisione numerica del valore restituito. Per i tipi di dati non numerici viene restituito NULL.|  
|NUMERIC_SCALE|**smallint**|Scala del valore restituito. Per i tipi di dati non numerici viene restituito NULL.|  
|DATETIME_PRECISION|**smallint**|Precisione frazionaria di un secondo se il valore restituito è di tipo **DateTime**. In caso contrario, viene restituito NULL.|  
|INTERVAL_TYPE|**nvarchar (** 30 **)**|NULL Riservato per utilizzi futuri.|  
|INTERVAL_PRECISION|**smallint**|NULL Riservato per utilizzi futuri.|  
|TYPE_UDT_CATALOG|**nvarchar (** 128 **)**|NULL Riservato per utilizzi futuri.|  
|TYPE_UDT_SCHEMA|**nvarchar (** 128 **)**|NULL Riservato per utilizzi futuri.|  
|TYPE_UDT_NAME|**nvarchar (** 128 **)**|NULL Riservato per utilizzi futuri.|  
|SCOPE_CATALOG|**nvarchar (** 128 **)**|NULL Riservato per utilizzi futuri.|  
|SCOPE_SCHEMA|**nvarchar (** 128 **)**|NULL Riservato per utilizzi futuri.|  
|SCOPE_NAME|**nvarchar (** 128 **)**|NULL Riservato per utilizzi futuri.|  
|MAXIMUM_CARDINALITY|**bigint**|NULL Riservato per utilizzi futuri.|  
|DTD_IDENTIFIER|**nvarchar (** 128 **)**|NULL Riservato per utilizzi futuri.|  
|ROUTINE_BODY|**nvarchar (** 30 **)**|Restituisce SQL per le funzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] ed EXTERNAL per le funzioni scritte esternamente.<br /><br /> Le funzioni sono sempre di tipo SQL.|  
|ROUTINE_DEFINITION|**nvarchar (** 4000 **)**|Restituisce i primi 4000 caratteri del testo di definizione della funzione o della stored procedure se la funzione o la stored procedure non è crittografata. In caso contrario, viene restituito NULL.<br /><br /> Per assicurarsi di ottenere la definizione completa, eseguire una query sulla funzione [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md) o sulla colonna di definizione della vista del catalogo [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) .|  
|EXTERNAL_NAME|**nvarchar (** 128 **)**|NULL Riservato per utilizzi futuri.|  
|EXTERNAL_LANGUAGE|**nvarchar (** 30 **)**|NULL Riservato per utilizzi futuri.|  
|PARAMETER_STYLE|**nvarchar (** 30 **)**|NULL Riservato per utilizzi futuri.|  
|IS_DETERMINISTIC|**nvarchar (** 10 **)**|Restituisce YES se la routine è deterministica.<br /><br /> Restituisce NO se la routine non è deterministica.<br /><br /> Restituisce sempre NO per le stored procedure.|  
|SQL_DATA_ACCESS|**nvarchar (** 30 **)**|Restituisce uno dei valori seguenti:<br /><br /> NONE = La funzione non contiene SQL.<br /><br /> CONTAINS = È possibile che la funzione contenga SQL<br /><br /> READS = È possibile che la funzione legga dati SQL.<br /><br /> MODIFIES = È possibile che la funzione modifichi dati SQL.<br /><br /> Restituisce READS per tutte le funzioni e MODIFIES per tutte le stored procedure.|  
|IS_NULL_CALL|**nvarchar (** 10 **)**|Specifica se la routine deve essere chiamata quando uno degli argomenti è NULL.|  
|SQL_PATH|**nvarchar (** 128 **)**|NULL Riservato per utilizzi futuri.|  
|SCHEMA_LEVEL_ROUTINE|**nvarchar (** 10 **)**|Restituisce YES per le funzioni valutate a livello di schema e NO negli altri casi.<br /><br /> Restituisce sempre YES.|  
|MAX_DYNAMIC_RESULT_SETS|**smallint**|Numero massimo di set di risultati dinamici restituiti dalla routine.<br /><br /> Restituisce 0 per le funzioni.|  
|IS_USER_DEFINED_CAST|**nvarchar (** 10 **)**|Restituisce YES per le funzioni cast definite dall'utente e NO negli altri casi.<br /><br /> Restituisce sempre NO.|  
|IS_IMPLICITLY_INVOCABLE|**nvarchar (** 10 **)**|Restituisce YES se è possibile richiamare in modo implicito la routine e NO se non è possibile richiamare in modo implicito la funzione.<br /><br /> Restituisce sempre NO.|  
|CREATED|**datetime**|Ora di creazione della routine.|  
|LAST_ALTERED|**datetime**|Ora dell'ultima modifica della funzione.|  
  
## <a name="see-also"></a>Vedere anche  
 [Viste di sistema &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)   
 [Viste degli schemi delle informazioni &#40;&#41;Transact-SQL ](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.procedures &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-procedures-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)  
  
