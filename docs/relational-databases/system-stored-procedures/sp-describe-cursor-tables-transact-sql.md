---
description: sp_describe_cursor_tables (Transact-SQL)
title: sp_describe_cursor_tables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_describe_cursor_tables_TSQL
- sp_describe_cursor_tables
dev_langs:
- TSQL
helpviewer_keywords:
- sp_describe_cursor_tables
ms.assetid: 02c0f81a-54ed-4ca4-aa4f-bb7463a9ab9a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b5c2e609eb3a521a991848b3b795b04b2ffbd01f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200802"
---
# <a name="sp_describe_cursor_tables-transact-sql"></a>sp_describe_cursor_tables (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea un report degli oggetti o delle tabelle di base a cui fa riferimento un cursore del server.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_describe_cursor_tables   
     [ @cursor_return = ] output_cursor_variable OUTPUT   
     { [ , [ @cursor_source = ] N'local'  
     , [ @cursor_identity = ] N'local_cursor_name' ]   
   | [ , [ @cursor_source = ] N'global'  
     , [ @cursor_identity = ] N'global_cursor_name' ]   
   | [ , [ @cursor_source = ] N'variable'  
     , [ @cursor_identity = ] N'input_cursor_variable' ]   
     }   
[;]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @cursor_return =] output *output_cursor_variable*  
 Nome di una variabile di cursore dichiarata per ricevere l'output del cursore. *output_cursor_variable* è un **cursore**, non prevede alcun valore predefinito e non deve essere associato ad alcun cursore al momento della chiamata sp_describe_cursor_tables. Il cursore restituito è di tipo scorrevole, dinamico e di sola lettura.  
  
 [ @cursor_source =] {N'local ' | N'Global ' | N'variable '}  
 Specifica se il cursore di cui viene generato il report viene specificato utilizzando il nome di un cursore locale, di un cursore globale o di una variabile di cursore. Il parametro è di **tipo nvarchar (30)**.  
  
 [ @cursor_identity =] N'*local_cursor_name*'  
 Nome di un cursore creato da un'istruzione DECLARE CURSOR con la parola chiave LOCAL specificata in modo esplicito o utilizzata per impostazione predefinita. *local_cursor_name* è di **tipo nvarchar (128)**.  
  
 [ @cursor_identity =] N'*global_cursor_name*'  
 Nome di un cursore creato da un'istruzione DECLARE CURSOR con la parola chiave GLOBAL o impostato sul valore predefinito GLOBAL. *global_cursor_name* può essere anche il nome di un cursore API del server aperto da un'applicazione ODBC che ha quindi denominato il cursore chiamando SQLSetCursorName. *global_cursor_name* è di **tipo nvarchar (128)**.  
  
 [ @cursor_identity =] N'*input_cursor_variable*'  
 Nome di una variabile di cursore associata a un cursore aperto. *input_cursor_variable* è di **tipo nvarchar (128)**.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 nessuno  
  
## <a name="cursors-returned"></a>Cursori restituiti  
 sp_describe_cursor_tables incapsula il report come parametro di [!INCLUDE[tsql](../../includes/tsql-md.md)] output del **cursore** . In questo modo i batch, le stored procedure e i trigger [!INCLUDE[tsql](../../includes/tsql-md.md)] possono elaborare l'output una riga alla volta. Ciò significa inoltre che non è possibile chiamare direttamente la procedura da funzioni API. Il parametro di output del **cursore** deve essere associato a una variabile di programma, ma le API non supportano i parametri o le variabili del **cursore** di binding.  
  
 Nella tabella seguente viene descritto il formato del cursore restituito da sp_describe_cursor_tables.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|table owner|**sysname**|ID utente del proprietario della tabella.|  
|Table_name|**sysname**|Nome dell'oggetto o della tabella di base. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] i cursori del server restituiscono sempre l'oggetto specificato dall'utente e non le tabelle di base.|  
|Optimizer_hints|**smallint**|Mappa di bit composta da uno o più dei valori seguenti:<br /><br /> 1 = Blocco a livello di riga (ROWLOCK)<br /><br /> 4 = Blocco a livello di pagina (PAGELOCK)<br /><br /> 8 = Blocco a livello di tabella (TABLOCK)<br /><br /> 16 = Blocco esclusivo a livello di tabella (TABLOCKX)<br /><br /> 32 = Blocco di aggiornamento (UPDLOCK)<br /><br /> 64 = Nessun blocco (NOLOCK)<br /><br /> 128 = Opzione recupero rapido prime n righe (FASTFIRSTROW)<br /><br /> 4096 = Semantica di lettura ripetibile se utilizzata con DECLARE CURSOR (HOLDLOCK)<br /><br /> Se si specificano più opzioni, il sistema utilizza l'opzione più restrittiva. La stored procedure sp_describe_cursor_tables, tuttavia, visualizza i flag specificati nella query.|  
|lock_type|**smallint**|Tipo di blocco di scorrimento richiesto in modo esplicito o implicito per ogni tabella di base sottostante del cursore. I possibili valori sono i seguenti:<br /><br /> 0 = Nessuna<br /><br /> 1 = Condiviso<br /><br /> 3 = Aggiornamento|  
|server_name|**sysname, Nullable**|Nome del server collegato contenente la tabella. NULL se si utilizza OPENQUERY o OPENROWSET.|  
|Objectid|**int**|ID di oggetto della tabella. 0 se si utilizza OPENQUERY o OPENROWSET.|  
|dbid|**int**|ID del database contenente la tabella specificata. 0 se si utilizza OPENQUERY o OPENROWSET.|  
|dbname|**sysname**, **Nullable**|Nome del database contenente la tabella specificata. NULL se si utilizza OPENQUERY o OPENROWSET.|  
  
## <a name="remarks"></a>Commenti  
 La stored procedure sp_describe_cursor_tables descrive le tabelle di base a cui fa riferimento un cursore del server. Utilizzare sp_describe_cursor_columns per ottenere una descrizione degli attributi del set dei risultati restituito dal cursore. Utilizzare sp_describe_cursor per ottenere una descrizione delle caratteristiche globali del cursore, come il supporto dello scorrimento e degli aggiornamenti. Utilizzare sp_cursor_list per ottenere un report dei cursori del server [!INCLUDE[tsql](../../includes/tsql-md.md)] visibili nella connessione.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo public.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene aperto un cursore globale ed eseguita la stored procedure `sp_describe_cursor_tables` per creare un report sulle tabelle a cui fa riferimento il cursore.  
  
```  
USE AdventureWorks2012;  
GO  
-- Declare and open a global cursor.  
DECLARE abc CURSOR KEYSET FOR  
SELECT LastName  
FROM Person.Person  
WHERE LastName LIKE 'S%';  
  
OPEN abc;  
GO  
-- Declare a cursor variable to hold the cursor output variable  
-- from sp_describe_cursor_tables.  
DECLARE @Report CURSOR;  
  
-- Execute sp_describe_cursor_tables into the cursor variable.  
EXEC master.dbo.sp_describe_cursor_tables  
      @cursor_return = @Report OUTPUT,  
      @cursor_source = N'global', @cursor_identity = N'abc';  
  
-- Fetch all the rows from the sp_describe_cursor_tables output cursor.  
FETCH NEXT from @Report;  
WHILE (@@FETCH_STATUS <> -1)  
BEGIN  
   FETCH NEXT from @Report;  
END  
  
-- Close and deallocate the cursor from sp_describe_cursor_tables.  
CLOSE @Report;  
DEALLOCATE @Report;  
GO  
  
-- Close and deallocate the original cursor.  
CLOSE abc;  
DEALLOCATE abc;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Cursori](../../relational-databases/cursors.md)   
 [CURSOR_STATUS &#40;&#41;Transact-SQL ](../../t-sql/functions/cursor-status-transact-sql.md)   
 [DECLARE CURSOR &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-cursor-transact-sql.md)   
 [sp_cursor_list &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursor-list-transact-sql.md)   
 [sp_describe_cursor &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-describe-cursor-transact-sql.md)   
 [sp_describe_cursor_columns &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-describe-cursor-columns-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
