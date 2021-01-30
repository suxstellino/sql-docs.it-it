---
title: Tipi di dati (Transact-SQL) | Microsoft Docs
description: Questo articolo fornisce un riepilogo dei diversi tipi di dati disponibili in SQL Server.
ms.date: 09/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- system data types [SQL Server]
- data types [SQL Server]
- data types [SQL Server], about data types
ms.assetid: a54f7373-b247-4d61-8fb8-7f2ec7a8d0a4
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6a0215238460e209391e6823f254ba19214141c1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204766"
---
# <a name="data-types-transact-sql"></a>Tipi di dati (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a ogni colonna, variabile locale, espressione e parametro è associato un tipo di dati. Un tipo di dati è un attributo che specifica il tipo di dati che l'oggetto può contenere, ovvero numeri interi, caratteri, valute, date e ore, stringhe binarie e così via.
  
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è disponibile un set di tipi di dati di sistema che definisce tutti i tipi di dati utilizzabili con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È anche possibile definire tipi di dati personalizzati in [!INCLUDE[tsql](../../includes/tsql-md.md)] o [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. I tipi di dati alias sono basati sui tipi di dati di sistema. Per altre informazioni sui tipi di dati alias, vedere [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md). I tipi definiti dall'utente derivano le loro caratteristiche dai metodi e dagli operatori di una classe che viene creata utilizzando uno dei linguaggi di programmazione supportati da [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)].
  
Quando due espressioni con tipi di dati, regole di confronto, precisione, scala o lunghezza diversi vengono combinati mediante un operatore, le caratteristiche del risultato vengono determinate come descritto di seguito.
-   Il tipo di dati del risultato viene determinato applicando le regole sulla precedenza dei tipi di dati ai tipi di dati delle espressioni di input. Per altre informazioni, vedere [Precedenza dei tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
-   Le regole di confronto del risultato sono determinate dalle regole di precedenza delle regole di confronto quando il tipo di dati del risultato è **char**, **varchar**, **text**, **nchar**, **nvarchar** o **ntext**. Per altre informazioni, vedere [Precedenza delle regole di confronto &#40;Transact-SQL&#41;](../../t-sql/statements/collation-precedence-transact-sql.md).  
-   La precisione, la scala e la lunghezza del risultato dipendono dalla precisione, dalla scala e dalla lunghezza delle espressioni di input. Per altre informazioni, vedere [Precisione, scala e lunghezza &#40;Transact-SQL&#41;](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono disponibili sinonimi dei tipi di dati per la compatibilità con ISO. Per altre informazioni, vedere [Sinonimi dei tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-synonyms-transact-sql.md).
  
## <a name="data-type-categories"></a>Categorie dei tipi di dati
I tipi di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono organizzati nelle categorie seguenti:
  
:::row:::
    :::column:::
        Dati numerici esatti
    :::column-end:::
    :::column:::
        Stringhe di testo Unicode
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        Numerici approssimati
    :::column-end:::
    :::column:::
        Stringhe binarie
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        Date e Time
    :::column-end:::
    :::column:::
        Altri tipi di dati
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        Stringhe di caratteri
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
 
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], a seconda delle caratteristiche relative all'archiviazione, alcuni tipi di dati appartengono ai gruppi seguenti:
-   Tipi di dati per valori di grandi dimensioni: **varchar(max)** e **nvarchar(max)**  
-   Tipi di dati per oggetti di grandi dimensioni: **text**, **ntext**, **image**, **varbinary(max)** e **xml**  
  
    > [!NOTE]  
    >  sp_help restituisce -1 per la lunghezza dei tipi di dati per valori di grandi dimensioni e **xml**.  
  
### <a name="exact-numerics"></a>Dati numerici esatti
  
:::row:::
    :::column:::
        [bigint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)
    :::column-end:::
    :::column:::
        [numeric](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [bit](../../t-sql/data-types/bit-transact-sql.md)
    :::column-end:::
    :::column:::
        [smallint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [decimal](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)
    :::column-end:::
    :::column:::
        [smallmoney](../../t-sql/data-types/money-and-smallmoney-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [int](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)
    :::column-end:::
    :::column:::
        [tinyint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [money](../../t-sql/data-types/money-and-smallmoney-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="approximate-numerics"></a>Numerici approssimati
  
:::row:::
    :::column:::
        [float](../../t-sql/data-types/float-and-real-transact-sql.md)
    :::column-end:::
    :::column:::
        [real](../../t-sql/data-types/float-and-real-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="date-and-time"></a>Date e Time
  
:::row:::
    :::column:::
        [date](../../t-sql/data-types/date-transact-sql.md)
    :::column-end:::
    :::column:::
        [datetimeoffset](../../t-sql/data-types/datetimeoffset-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [datetime2](../../t-sql/data-types/datetime2-transact-sql.md)
    :::column-end:::
    :::column:::
        [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [datetime](../../t-sql/data-types/datetime-transact-sql.md)
    :::column-end:::
    :::column:::
        [time](../../t-sql/data-types/time-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
  
### <a name="character-strings"></a>Stringhe di caratteri
  
:::row:::
    :::column:::
        [char](../../t-sql/data-types/char-and-varchar-transact-sql.md)
    :::column-end:::
    :::column:::
        [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [text](../../t-sql/data-types/ntext-text-and-image-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
 
### <a name="unicode-character-strings"></a>Stringhe di testo Unicode
  
:::row:::
    :::column:::
        [nchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)
    :::column-end:::
    :::column:::
        [nvarchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [ntext](../../t-sql/data-types/ntext-text-and-image-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
  

### <a name="binary-strings"></a>Stringhe binarie
  
:::row:::
    :::column:::
        [binary](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)
    :::column-end:::
    :::column:::
        [varbinary](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [image](../../t-sql/data-types/ntext-text-and-image-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="other-data-types"></a>Altri tipi di dati

:::row:::
    :::column:::
        [cursor](../../t-sql/data-types/cursor-transact-sql.md)
    :::column-end:::
    :::column:::
        [rowversion](../../t-sql/data-types/rowversion-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [hierarchyid](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
    :::column-end:::
    :::column:::
        [uniqueidentifier](../../t-sql/data-types/uniqueidentifier-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md)
    :::column-end:::
    :::column:::
        [xml](../../t-sql/xml/xml-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [Tipi di geometria spaziale](../../t-sql/spatial-geometry/spatial-types-geometry-transact-sql.md) 
    :::column-end:::
    :::column:::
        [Tipi di geografia spaziale](../../t-sql/spatial-geography/spatial-types-geography.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [tabella](../../t-sql/data-types/table-transact-sql.md) 
    :::column-end:::
    :::column:::
         
    :::column-end:::
:::row-end:::

  
## <a name="see-also"></a>Vedere anche
[CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)
[EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)  
[Espressioni &#40; Transact-SQL &#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
[Funzioni &#40;Transact-SQL&#41;](../../t-sql/functions/functions.md)  
[LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)  
[sp_droptype &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droptype-transact-sql.md)  
[sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)  
[sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)
  
  
