---
description: sp_describe_cursor (Transact-SQL)
title: sp_describe_cursor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_describe_cursor
- sp_describe_cursor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_describe_cursor
ms.assetid: 0c836c99-1147-441e-998c-f0a30cd05275
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0787f088205aab72c437e22bfac265ef05ccbe19
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200784"
---
# <a name="sp_describe_cursor-transact-sql"></a>sp_describe_cursor (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea un report degli attributi di un cursore del server.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_describe_cursor [ @cursor_return = ] output_cursor_variable OUTPUT   
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
 Nome di una variabile di cursore dichiarata per ricevere l'output del cursore. *output_cursor_variable* è un **cursore**, non prevede alcun valore predefinito e non deve essere associato ad alcun cursore al momento della chiamata sp_describe_cursor. Il cursore restituito è di tipo scorrevole, dinamico e di sola lettura.  
  
 [ @cursor_source =] {N'local ' | N'Global ' | N'variable '}  
 Specifica se il cursore di cui viene generato il report viene specificato utilizzando il nome di un cursore locale, di un cursore globale o di una variabile di cursore. Il parametro è di **tipo nvarchar (30)**.  
  
 [ @cursor_identity =] N'*local_cursor_name*']  
 Nome di un cursore creato da un'istruzione DECLARE CURSOR con la parola chiave LOCAL o impostato sul valore predefinito LOCAL. *local_cursor_name* è di **tipo nvarchar (128)**.  
  
 [ @cursor_identity =] N'*global_cursor_name*']  
 Nome di un cursore creato da un'istruzione DECLARE CURSOR con la parola chiave GLOBAL o impostato sul valore predefinito GLOBAL. *global_cursor_name* è di **tipo nvarchar (128)**.  
  
 *global_cursor_name* può essere anche il nome di un cursore API del server aperto da un'applicazione ODBC che viene quindi denominata chiamando SQLSetCursorName.  
  
 [ @cursor_identity =] N'*input_cursor_variable*']  
 Nome di una variabile di cursore associata a un cursore aperto. *input_cursor_variable* è di **tipo nvarchar (128)**.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 nessuno  
  
## <a name="cursors-returned"></a>Cursori restituiti  
 sp_describe_cursor incapsula il set di risultati in un [!INCLUDE[tsql](../../includes/tsql-md.md)] parametro di output del **cursore** . In questo modo i batch, le stored procedure e i trigger [!INCLUDE[tsql](../../includes/tsql-md.md)] possono elaborare l'output una riga alla volta. Questo significa inoltre che non è possibile richiamare direttamente la procedura da funzioni API del database. Il parametro di output del **cursore** deve essere associato a una variabile di programma, ma le API del database non supportano l'associazione di parametri o variabili del **cursore** .  
  
 Nella tabella seguente viene descritto il formato del cursore restituito da sp_describe_cursor. Tale formato corrisponde a quello restituito da sp_cursor_list.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|reference_name|**sysname**|Nome utilizzato per fare riferimento al cursore. Se il riferimento al cursore è stato impostato tramite il nome specificato in un'istruzione DECLARE CURSOR, il nome di riferimento corrisponde al nome del cursore. Se il riferimento al cursore è stato impostato tramite una variabile, il nome di riferimento corrisponde al nome della variabile.|  
|cursor_name|**sysname**|Nome del cursore che deriva da un'istruzione DECLARE CURSOR. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se il cursore è stato creato mediante l'impostazione di una variabile di cursore su un cursore, cursor_name restituisce il nome della variabile di cursore. Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa colonna di output restituisce un nome generato dal sistema.|  
|cursor_scope|**tinyint**|1 = LOCAL<br /><br /> 2 = GLOBAL|  
|status|**int**|Stessi valori restituiti dalla funzione di sistema CURSOR_STATUS:<br /><br /> 1 = Il cursore a cui si fa riferimento tramite il nome o la variabile è aperto. Se il cursore è di tipo insensitive, statico o keyset, il set di risultati contiene almeno una riga. Se invece è dinamico, contiene zero o più righe.<br /><br /> 0 = Il cursore a cui si fa riferimento tramite il nome o la variabile è aperto, ma non contiene righe. I cursori dinamici non restituiscono mai questo valore.<br /><br /> -1 = Il cursore a cui si fa riferimento tramite il nome o la variabile è chiuso.<br /><br /> -2 = Si applica solo alle variabili di cursore. Alla variabile non è assegnato alcun cursore. È possibile che un parametro OUTPUT abbia assegnato un cursore alla variabile, ma la stored procedure ha chiuso il cursore prima di completare l'operazione.<br /><br /> -3 = Non esiste alcun cursore o variabile di cursore con il nome specificato oppure alla variabile non è stato assegnato alcun cursore.|  
|model|**tinyint**|1 = Insensitive (o statico)<br /><br /> 2 = keyset<br /><br /> 3 = dinamica<br /><br /> 4 = Fast forward-only|  
|Concorrenza|**tinyint**|1 = Di sola lettura.<br /><br /> 2 = Blocchi di scorrimento.<br /><br /> 3 = Ottimistica.|  
|scrollable|**tinyint**|0 = Forward-only<br /><br /> 1 = Scorrevole|  
|open_status|**tinyint**|0 = Chiuso<br /><br /> 1 = Aperto|  
|cursor_rows|**Decimal (10, 0)**|Numero di righe risultanti nel set dei risultati. Per altre informazioni, vedere [@@CURSOR_ROWS &#40;Transact-SQL&#41;](../../t-sql/functions/cursor-rows-transact-sql.md).|  
|fetch_status|**smallint**|Stato dell'ultimo recupero sul cursore. Per altre informazioni, vedere [@@FETCH_STATUS &#40;Transact-SQL&#41;](../../t-sql/functions/fetch-status-transact-sql.md).<br /><br /> 0 = Recupero corretto.<br /><br /> -1 = Recupero non riuscito o non compreso entro i limiti del cursore.<br /><br /> -2 = La riga richiesta è mancante.<br /><br /> -9 = Nessun recupero eseguito sul cursore.|  
|column_count|**smallint**|Numero di colonne nel set dei risultati del cursore.|  
|row_count|**Decimal (10, 0)**|Numero di righe modificate dall'ultima operazione eseguita sul cursore. Per altre informazioni, vedere [@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md).|  
|last_operation|**tinyint**|Ultima operazione eseguita sul cursore:<br /><br /> 0 = Non è stata eseguita alcuna operazione.<br /><br /> 1 = OPEN<br /><br /> 2 = FETCH<br /><br /> 3 = INSERIMENTO<br /><br /> 4 = UPDATE<br /><br /> 5 = ELIMINA<br /><br /> 6 = CLOSE<br /><br /> 7 = DEALLOCATE|  
|cursor_handle|**int**|Valore univoco che identifica il cursore nell'ambito del server.|  
  
## <a name="remarks"></a>Commenti  
 La stored procedure sp_describe_cursor descrive gli attributi globali per un cursore del server, ad esempio la possibilità di scorrimento o aggiornamento. Utilizzare sp_describe_cursor_columns per ottenere una descrizione degli attributi del set dei risultati restituito dal cursore. Utilizzare sp_describe_cursor_tables per ottenere un report delle tabelle di base a cui fa riferimento il cursore. Per ottenere un report relativo ai cursori del server [!INCLUDE[tsql](../../includes/tsql-md.md)] visibili nella connessione, utilizzare la stored procedure sp_cursor_list.  
  
 Un'istruzione DECLARE CURSOR potrebbe richiedere un tipo di cursore non supportato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite l'istruzione SELECT inclusa nell'istruzione DECLARE CURSOR [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] converte in modo implicito il cursore in un tipo supportato per l'istruzione SELECT. Se nell'istruzione DECLARE CURSOR viene specificato TYPE_WARNING, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] invia all'applicazione un messaggio informativo relativo al completamento della conversione. è quindi possibile chiamare sp_describe_cursor per determinare il tipo di cursore implementato.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo public.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene aperto un cursore globale e viene utilizzata la stored procedure `sp_describe_cursor` per creare un report degli attributi del cursore.  
  
```  
USE AdventureWorks2012;  
GO  
-- Declare and open a global cursor.  
DECLARE abc CURSOR STATIC FOR  
SELECT LastName  
FROM Person.Person;  
  
OPEN abc;  
  
-- Declare a cursor variable to hold the cursor output variable  
-- from sp_describe_cursor.  
DECLARE @Report CURSOR;  
  
-- Execute sp_describe_cursor into the cursor variable.  
EXEC master.dbo.sp_describe_cursor @cursor_return = @Report OUTPUT,  
        @cursor_source = N'global', @cursor_identity = N'abc';  
  
-- Fetch all the rows from the sp_describe_cursor output cursor.  
FETCH NEXT from @Report;  
WHILE (@@FETCH_STATUS <> -1)  
BEGIN  
    FETCH NEXT from @Report;  
END  
  
-- Close and deallocate the cursor from sp_describe_cursor.  
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
 [sp_describe_cursor_columns &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-describe-cursor-columns-transact-sql.md)   
 [sp_describe_cursor_tables &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-describe-cursor-tables-transact-sql.md)  
  
  
