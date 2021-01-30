---
description: sp_help_fulltext_tables_cursor (Transact-SQL)
title: sp_help_fulltext_tables_cursor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_fulltext_tables_cursor
- sp_help_fulltext_tables_cursor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_fulltext_tables_cursor
ms.assetid: 155791eb-8832-4596-8487-7fc70dfba5b9
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e7f5ce76745803b1a10caf74f6870202aa508268
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159800"
---
# <a name="sp_help_fulltext_tables_cursor-transact-sql"></a>sp_help_fulltext_tables_cursor (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Utilizza un cursore per restituire un elenco di tabelle registrate per l'indicizzazione full-text.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Usare invece la nuova vista del catalogo **sys.fulltext_indexes** . Per ulteriori informazioni, vedere [sys.fulltext_indexes &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_help_fulltext_tables_cursor [ @cursor_return = ] @cursor_variable OUTPUT   
     [ , [ @fulltext_catalog_name = ] 'fulltext_catalog_name' ]   
     [ , [ @table_name = ] 'table_name' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @cursor_return = ] @cursor_variable OUTPUT` Variabile di output di tipo **Cursor**. Il cursore restituito è di tipo scorrevole, dinamico e di sola lettura.  
  
`[ @fulltext_catalog_name = ] 'fulltext_catalog_name'` Nome del catalogo full-text. *fulltext_catalog_name* è di **tipo sysname** e il valore predefinito è null. Se *fulltext_catalog_name* viene omesso o è null, vengono restituite tutte le tabelle con indicizzazione full-text associate al database. Se *fulltext_catalog_name* viene specificato, ma *table_name* viene omesso o è null, vengono recuperate le informazioni relative all'indice full-text per ogni tabella con indicizzazione full-text associata a questo catalogo. Se vengono specificati sia *fulltext_catalog_name* che *table_name* , viene restituita una riga se *table_name* è associato a *fulltext_catalog_name*; in caso contrario, viene generato un errore.  
  
`[ @table_name = ] 'table_name'` Nome di tabella costituito da una o due parti per il quale sono richiesti i metadati full-text. *table_name* è di **tipo nvarchar (517)** e il valore predefinito è null. Se viene specificato solo *table_name* , viene restituita solo la riga pertinente per *table_name* .  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (esito positivo) o 1 (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**TABLE_OWNER**|**sysname**|Proprietario della tabella. Corrisponde al nome dell'utente del database che ha creato la tabella.|  
|**TABLE_NAME**|**sysname**|Nome della tabella.|  
|**FULLTEXT_KEY_INDEX_NAME**|**sysname**|Indice che impone il vincolo UNIQUE sulla colonna impostata come colonna chiave univoca.|  
|**FULLTEXT_KEY_COLID**|**int**|ID di colonna dell'indice univoco identificato da FULLTEXT_KEY_NAME.|  
|**FULLTEXT_INDEX_ACTIVE**|**int**|Specifica se le colonne contrassegnate per l'indicizzazione full-text nella tabella corrente sono soggette all'esecuzione di query:<br /><br /> 0 = Inattivo<br /><br /> 1 = Attivo|  
|**FULLTEXT_CATALOG_NAME**|**sysname**|Catalogo full-text contenente i dati dell'indice full-text.|  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione vengono assegnate per impostazione predefinita ai membri del ruolo **public** .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituiti i nomi delle tabelle indicizzate full-text associate al catalogo full-text `Cat_Desc`.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @mycursor CURSOR;  
EXEC sp_help_fulltext_tables_cursor @mycursor OUTPUT, 'Cat_Desc';  
FETCH NEXT FROM @mycursor;  
WHILE (@@FETCH_STATUS <> -1)  
   BEGIN  
      FETCH NEXT FROM @mycursor;  
   END;  
CLOSE @mycursor;  
DEALLOCATE @mycursor;  
GO   
```  
  
## <a name="see-also"></a>Vedere anche  
 [INDEXPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/indexproperty-transact-sql.md)   
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [sp_fulltext_table &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-fulltext-table-transact-sql.md)   
 [sp_help_fulltext_tables &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
