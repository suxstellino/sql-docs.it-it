---
description: sp_help_fulltext_catalogs_cursor (Transact-SQL)
title: sp_help_fulltext_catalogs_cursor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_fulltext_catalogs_cursor
- sp_help_fulltext_catalogs_cursor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_fulltext_catalogs_cursor
ms.assetid: d44478d1-0cc4-415e-9d1a-6dccb64674fa
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 78a98cd5bdff674eae681534185099afcb2e9e4a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198353"
---
# <a name="sp_help_fulltext_catalogs_cursor-transact-sql"></a>sp_help_fulltext_catalogs_cursor (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Utilizza un cursore per restituire l'ID, il nome, la directory radice, lo stato e il numero di tabelle indicizzate full-text per il catalogo full-text specificato.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilizzare invece la vista del catalogo [sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_fulltext_catalogs_cursor [ @cursor_return= ] @cursor_variable OUTPUT ,   
     [ @fulltext_catalog_name = ] 'fulltext_catalog_name'   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @cursor_return = ] @cursor_variable OUTPUT` Variabile di output di tipo **Cursor**. Il cursore restituito è di tipo scorrevole, dinamico e di sola lettura.  
  
`[ @fulltext_catalog_name = ] 'fulltext_catalog_name'` Nome del catalogo full-text. *fulltext_catalog_name* è di **tipo sysname**. Se questo parametro viene omesso oppure è NULL, vengono restituite informazioni su tutti i cataloghi full-text associati al database corrente.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (esito positivo) o 1 (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**fulltext_catalog_id**|**smallint**|ID del catalogo full-text.|  
|**NOME**|**sysname**|Nome del catalogo full-text.|  
|**PATH**|**nvarchar(260)**|A partire da [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], questa clausola non ha alcun effetto.|  
|**STATO**|**int**|Stato di popolamento dell'indice full-text del catalogo:<br /><br /> 0 = inattivo<br /><br /> 1 = popolamento completo in corso<br /><br /> 2 = sospeso<br /><br /> 3 = rallentato<br /><br /> 4 = Recupero in corso<br /><br /> 5 = Chiusura<br /><br /> 6= popolamento incrementale in corso<br /><br /> 7 = compilazione dell'indice in corso<br /><br /> 8 = disco pieno Paused<br /><br /> 9 = rilevamento modifiche|  
|**NUMBER_FULLTEXT_TABLES**|**int**|Numero di tabelle indicizzate full-text associate al catalogo|  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione vengono assegnate per impostazione predefinita al ruolo **public** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite informazioni relative al catalogo full-text `Cat_Desc`.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @mycursor CURSOR;  
EXEC sp_help_fulltext_catalogs_cursor @mycursor OUTPUT, 'Cat_Desc';  
FETCH NEXT FROM @mycursor;  
WHILE (@@FETCH_STATUS <> -1)  
   BEGIN  
      FETCH NEXT FROM @mycursor;  
   END  
CLOSE @mycursor;  
DEALLOCATE @mycursor;  
GO   
```  
  
## <a name="see-also"></a>Vedere anche  
 [FULLTEXTCATALOGPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/fulltextcatalogproperty-transact-sql.md)   
 [sp_fulltext_catalog &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-fulltext-catalog-transact-sql.md)   
 [sp_help_fulltext_catalogs &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
