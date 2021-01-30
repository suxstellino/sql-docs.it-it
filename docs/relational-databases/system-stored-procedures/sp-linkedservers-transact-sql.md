---
description: sp_linkedservers (Transact-SQL)
title: sp_linkedservers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_linkedservers
- sp_linkedservers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_linkedservers
ms.assetid: d8f82f78-8a1f-4831-bcee-7c36c6e7dfbb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a376e29a988e64242fc06d0c550814f51d8ad1f6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99103071"
---
# <a name="sp_linkedservers-transact-sql"></a>sp_linkedservers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce l'elenco dei server collegati definiti nel server locale.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_linkedservers  
```  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (esito positivo) o un numero diverso da zero (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**SRV_NAME**|**sysname**|Nome del server collegato.|  
|**SRV_PROVIDERNAME**|**nvarchar (** 128 **)**|Nome descrittivo del provider OLE DB che gestisce l'accesso al server collegato specificato.|  
|**SRV_PRODUCT**|**nvarchar (** 128 **)**|Nome del prodotto del server collegato.|  
|**SRV_DATASOURCE**|**nvarchar (** 4000 **)**|Proprietà dell'origine dei dati OLE DB corrispondente al server collegato specificato.|  
|**SRV_PROVIDERSTRING**|**nvarchar (** 4000 **)**|Proprietà della stringa del provider OLE DB corrispondente al server collegato.|  
|**SRV_LOCATION**|**nvarchar (** 4000 **)**|Proprietà della posizione OLE DB corrispondente al server collegato specificato.|  
|**SRV_CAT**|**sysname**|Proprietà del catalogo OLE DB corrispondente al server collegato specificato.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione SELECT per lo schema.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_catalogs &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-catalogs-transact-sql.md)   
 [sp_column_privileges &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-column-privileges-transact-sql.md)   
 [sp_columns_ex &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-columns-ex-transact-sql.md)   
 [sp_foreignkeys &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-foreignkeys-transact-sql.md)   
 [sp_indexes &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-indexes-transact-sql.md)   
 [sp_primarykeys &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-primarykeys-transact-sql.md)   
 [sp_table_privileges &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-table-privileges-transact-sql.md)   
 [sp_tables_ex &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-tables-ex-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Stored procedure per query distribuite &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)  
  
  
