---
description: sys.fn_helpcollations (Transact-SQL)
title: sys.fn_helpcollations (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_helpcollations
- fn_helpcollations_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_helpcollations function
- collations [SQL Server], supported
- fn_helpcollations function
ms.assetid: b5082e81-1fee-4e2c-b567-5412eaee41c1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016|| = azure-sqldw-latest ||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8185f4977f784a0b2590d6a5fdf4a210dd2cd328
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101424"
---
# <a name="sysfn_helpcollations-transact-sql"></a>sys.fn_helpcollations (Transact-SQL)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce un elenco di tutte le regole di confronto supportate.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```
fn_helpcollations ()  
```  
  
## <a name="tables-returned"></a>Tabelle restituite

 **fn_helpcollations** restituisce le informazioni seguenti.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|Nome|**sysname**|Nome standard delle regole di confronto|  
|Descrizione|**nvarchar (1000)**|Descrizione delle regole di confronto|  
  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono supportate le regole di confronto di Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta inoltre un numero limitato (<80) di regole di confronto denominate regole di confronto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , che sono state sviluppate prima delle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] regole di confronto di Windows supportate. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le regole di confronto sono ancora supportate per compatibilità con le versioni precedenti, ma non devono essere usate per nuove attività di sviluppo. Per altre informazioni sulle regole di confronto di Windows, vedere [Nome delle regole di confronto di Windows &#40;Transact-SQL&#41;](../../t-sql/statements/windows-collation-name-transact-sql.md). Per altre informazioni sulle regole di confronto, vedere [Regole di confronto e supporto Unicode](../../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Esempi

 Nell'esempio seguente vengono restituiti tutti i nomi delle regole di confronto che iniziano con la lettera `L` e la cui descrizione è "binary sort".

> [!Note]
> Le query di Azure sinapsi Analytics su fn_helpcollations () devono essere eseguite nel database master.  
  
```sql  
SELECT Name, Description FROM fn_helpcollations()  
WHERE Name like 'L%' AND Description LIKE '% binary sort';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Name                   Description  
 -------------------    ------------------------------------  
 Lao_100_BIN            Lao-100, binary sort  
 Latin1_General_BIN     Latin1-General, binary sort  
 Latin1_General_100_BIN Latin1-General-100, binary sort  
 Latvian_BIN            Latvian, binary sort  
 Latvian_100_BIN        Latvian-100, binary sort  
 Lithuanian_BIN         Lithuanian, binary sort  
 Lithuanian_100_BIN     Lithuanian-100, binary sort  
  
 (7 row(s) affected)  
 ```
  
## <a name="see-also"></a>Vedere anche

[COLLATE &#40;Transact-SQL&#41;](~/t-sql/statements/collations.md)   
[COLLATIONPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/collation-functions-collationproperty-transact-sql.md)  
[Supporto delle regole di confronto del database per Azure sinapsi Analytics](https://azure.microsoft.com/blog/database-collation-support-for-azure-sql-data-warehouse-2)  
