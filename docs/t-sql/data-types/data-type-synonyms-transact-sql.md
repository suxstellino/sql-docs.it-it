---
description: Sinonimi dei tipi di dati (Transact-SQL)
title: Sinonimi dei tipi di dati (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- data types [SQL Server], synonyms
- alternate names [SQL Server]
- synonyms [SQL Server], data types
ms.assetid: 390eef67-1a49-4185-a971-e07765be9717
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: de20f321f030d0ddfe7e7eb274577f5b4c3d3a70
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202306"
---
# <a name="data-type-synonyms-transact-sql"></a>Sinonimi dei tipi di dati (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

I sinonimi dei tipi di dati sono disponibili in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per compatibilità con ISO. Nella tabella seguente vengono elencati i sinonimi e i tipi di dati di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a cui viene eseguito il mapping.
  
|Sinonimo|Tipo di dati di sistema di SQL Server|  
|---|---|
|**Binary varying**|**varbinary**|  
|**char varying**|**varchar**|  
|**character**|**char**|  
|**character**|**char(1)**|  
|**character(**_n_**)**|**char(n)**|  
|**character varying(**_n_**)**|**varchar(n)**|  
|**Dec**|**decimal**|  
|**Double precision**|**float**|  
|**float**[ **(** _n_ **)** ] for _n_ = 1-7|**real**|  
|**float**[ **(** _n_ **)** ] for _n_ = 8-15|**float**|  
|**integer**|**int**|  
|**national character(**_n_**)**|**nchar(n)**|  
|**national char(**_n_**)**|**nchar(n)**|  
|**national character varying(**_n_**)**|**nvarchar(n)**|  
|**national char varying(**_n_**)**|**nvarchar(n)**|  
|**national text**|**ntext**|  
|**timestamp**|rowversion|  
  
I sinonimi dei tipi di dati possono essere usati in alternativa al nome del tipo di dati di base corrispondente in istruzioni DDL (Data Definition Language). Tali istruzioni includono CREATE TABLE, CREATE PROCEDURE e DECLARE *\@variable*. I sinonimi non sono tuttavia visibili dopo la creazione dell'oggetto. In fase di creazione infatti all'oggetto viene assegnato il tipo di dati di base associato al sinonimo e la presenza del sinonimo nell'istruzione con cui è stato creato l'oggetto non viene registrata.
  
Agli oggetti che derivano dall'oggetto originale, ad esempio colonne del set di risultati o espressioni, viene assegnato il tipo di dati di base. Qualsiasi funzione per metadati che usa l'oggetto originale o qualsiasi oggetto derivato visualizzerà il tipo di dati di base, non il sinonimo, incluse:

* Operazioni sui metadati, ad esempio **sp_help** e altre stored procedure di sistema,
* Viste degli schemi delle informazioni, e
* Operazioni sui metadati di API di accesso ai dati che visualizzano i tipi di dati delle colonne della tabella o del set di risultati.
  
È possibile, ad esempio, creare una tabella specificando `national character varying`:
  
```sql
CREATE TABLE ExampleTable (PriKey int PRIMARY KEY, VarCharCol national character varying(10))  
```  
  
A `VarCharCol` viene assegnato il tipo di dati **nvarchar(10)** e tutte le funzioni per i metadati successive visualizzeranno la colonna come colonna di tipo **nvarchar(10)**. Le funzioni per i metadati non visualizzano mai le colonne come colonne di tipo **national character varying(10)**.
  
## <a name="see-also"></a>Vedere anche
[Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
  
  
