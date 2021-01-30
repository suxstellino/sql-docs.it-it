---
description: sp_fulltext_column (Transact-SQL)
title: sp_fulltext_column (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fulltext_column_TSQL
- sp_fulltext_column
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_column
ms.assetid: a84cc45d-1b50-44af-85df-2ea033b8a6a9
author: markingmyname
ms.author: maghan
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4203f4ce8d603bd8715047b8a3f0ffb93e6b1e8b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185401"
---
# <a name="sp_fulltext_column-transact-sql"></a>sp_fulltext_column (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

  Specifica se una determinata colonna di una tabella viene inclusa o meno nell'indicizzazione full-text.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_fulltext_column [ @tabname= ] 'qualified_table_name' ,   
     [ @colname= ] 'column_name' ,   
     [ @action= ] 'action'   
     [ , [ @language= ] 'language_term' ]   
     [ , [ @type_colname= ] 'type_column_name' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @tabname = ] 'qualified_table_name'` È un nome di tabella in una o due parti. La tabella deve esistere nel database corrente La tabella deve disporre di un indice full-text. *qualified_table_name* è di **tipo nvarchar (517)** e non prevede alcun valore predefinito.  
  
`[ @colname = ] 'column_name'` Nome di una colonna in *qualified_table_name*. La colonna deve essere una colonna character, **varbinary (max)** o **Image** e non può essere una colonna calcolata. *column_name* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente di creare indici full-text di dati di testo archiviati in colonne con tipo di dati **varbinary (max)** o **Image** . Le immagini non vengono indicizzate.  
  
`[ @action = ] 'action'` Azione da eseguire. *Action* è di tipo **varchar (20)** e non prevede alcun valore predefinito. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**add**|Aggiunge *column_name* di *qualified_table_name* all'indice full-text inattivo della tabella. Questa azione abilita la colonna per l'indicizzazione full-text.|  
|**goccia**|Rimuove *column_name* di *qualified_table_name* dall'indice full-text inattivo della tabella.|  
  
`[ @language = ] 'language_term'` Lingua dei dati archiviati nella colonna. Per un elenco delle lingue incluse in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vedere [sys.fulltext_languages &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md).  
  
> [!NOTE]  
>  Se una colonna include dati in più lingue o in una lingua non supportata, utilizzare la lingua neutra. Il valore predefinito è specificato nell'opzione di configurazione "default full-text language".  
  
`[ @type_colname = ] 'type_column_name'` Nome di una colonna in *qualified_table_name* che include il tipo di documento *column_name*. Questa colonna deve essere di tipo **char**, **nchar**, **varchar** o **nvarchar**. Viene utilizzato solo quando il tipo di dati di *column_name* è di tipo **varbinary (max)** o **Image**. *type_column_name* è di **tipo sysname** e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Se l'indice full-text è attivo, l'eventuale processo di popolamento in corso viene arrestato. Inoltre, se in una tabella con indice full-text attivo è abilitato il rilevamento delle modifiche, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] assicura che l'indice sia aggiornato. Ad esempio, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] arresta l'eventuale processo di popolamento in corso nella tabella, elimina l'indice esistente e avvia un nuovo popolamento.  
  
 Se il rilevamento delle modifiche è attivato e devono essere aggiunte o eliminate colonne dall'indice full-text conservando tuttavia l'indice, è necessario disattivare la tabella, quindi aggiungere ed eliminare le colonne appropriate. In seguito a queste azioni l'indice viene bloccato. È quindi possibile attivare la tabella in un momento successivo, quando è opportuno avviare un processo di popolamento.  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente deve essere un membro del ruolo predefinito del database **db_ddladmin** o un membro del ruolo predefinito del database **db_owner** oppure il proprietario della tabella.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente la colonna `DocumentSummary` della tabella `Document` viene aggiunta all'indice full-text della tabella.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_column 'Production.Document', DocumentSummary, 'add';  
GO  
```  
  
 Nell'esempio seguente si presuppone che sia stato creato un indice full-text nella tabella `spanishTbl`. Per aggiungere la colonna `spanishCol` all'indice full-text, eseguire la stored procedure seguente:  
  
```  
EXEC sp_fulltext_column 'spanishTbl', 'spanishCol', 'add', 0xC0A;  
GO  
```  
  
 Quando si esegue la seguente query:  
  
```  
SELECT *   
FROM spanishTbl   
WHERE CONTAINS(spanishCol, 'formsof(inflectional, trabajar)')  
```  
  
 Il set di risultati deve includere le righe contenenti forme diverse del verbo spagnolo `trabajar` (lavorare), ad esempio `trabajo`, `trabajamos` e `trabajan`.  
  
> [!NOTE]  
>  È necessario che in tutte le colonne elencate in una singola clausola di funzione per query full-text sia applicata la stessa lingua.  
  
## <a name="see-also"></a>Vedere anche  
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [sp_help_fulltext_columns &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-fulltext-columns-transact-sql.md)   
 [sp_help_fulltext_columns_cursor &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-fulltext-columns-cursor-transact-sql.md)   
 [sp_help_fulltext_tables &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-transact-sql.md)   
 [sp_help_fulltext_tables_cursor &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-cursor-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Stored procedure per la ricerca full-text e la ricerca semantica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)  
  
  
