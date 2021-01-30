---
description: sp_send_dbmail (Transact-SQL)
title: sp_send_dbmail (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sendmail_sp_TSQL
- sendmail_sp
- SP_SEND_DBMAIL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_send_dbmail
ms.assetid: f1d7a795-a3fd-4043-ac4b-c781e76dab47
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 72da3a1013869049b350440108dc4d4b631b5712
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190838"
---
# <a name="sp_send_dbmail-transact-sql"></a>sp_send_dbmail (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Invia un messaggio di posta elettronica ai destinatari specificati. Il messaggio può includere il set dei risultati di una query, file allegati o entrambi. Quando la posta elettronica viene inserita correttamente nella coda di Posta elettronica database, **sp_send_dbmail** restituisce la **mailitem_id** del messaggio. Questo stored procedure si trova nel database **msdb** .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_send_dbmail [ [ @profile_name = ] 'profile_name' ]  
    [ , [ @recipients = ] 'recipients [ ; ...n ]' ]  
    [ , [ @copy_recipients = ] 'copy_recipient [ ; ...n ]' ]  
    [ , [ @blind_copy_recipients = ] 'blind_copy_recipient [ ; ...n ]' ]  
    [ , [ @from_address = ] 'from_address' ]  
    [ , [ @reply_to = ] 'reply_to' ]   
    [ , [ @subject = ] 'subject' ]   
    [ , [ @body = ] 'body' ]   
    [ , [ @body_format = ] 'body_format' ]  
    [ , [ @importance = ] 'importance' ]  
    [ , [ @sensitivity = ] 'sensitivity' ]  
    [ , [ @file_attachments = ] 'attachment [ ; ...n ]' ]  
    [ , [ @query = ] 'query' ]  
    [ , [ @execute_query_database = ] 'execute_query_database' ]  
    [ , [ @attach_query_result_as_file = ] attach_query_result_as_file ]  
    [ , [ @query_attachment_filename = ] query_attachment_filename ]  
    [ , [ @query_result_header = ] query_result_header ]  
    [ , [ @query_result_width = ] query_result_width ]  
    [ , [ @query_result_separator = ] 'query_result_separator' ]  
    [ , [ @exclude_query_output = ] exclude_query_output ]  
    [ , [ @append_query_error = ] append_query_error ]  
    [ , [ @query_no_truncate = ] query_no_truncate ]   
    [ , [ @query_result_no_padding = ] @query_result_no_padding ]   
    [ , [ @mailitem_id = ] mailitem_id ] [ OUTPUT ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @profile_name = ] 'profile_name'` Nome del profilo da cui inviare il messaggio. Il *profile_name* è di tipo **sysname** e il valore predefinito è null. Il *profile_name* deve essere il nome di un profilo di posta elettronica database esistente. Quando non viene specificato alcun *profile_name* , **sp_send_dbmail** utilizza il profilo privato predefinito per l'utente corrente. Se l'utente non dispone di un profilo privato predefinito, **sp_send_dbmail** utilizza il profilo pubblico predefinito per il database **msdb** . Se l'utente non dispone di un profilo privato predefinito e non è disponibile un profilo pubblico predefinito per il database, è necessario specificare **\@ profile_name** .  
  
`[ @recipients = ] 'recipients'` Elenco delimitato da punti e virgola degli indirizzi di posta elettronica a cui inviare il messaggio. L'elenco dei destinatari è di tipo **varchar (max)**. Sebbene questo parametro sia facoltativo, è necessario specificare almeno uno dei **\@ destinatari**, **\@ copy_recipients** o **\@ blind_copy_recipients** oppure **sp_send_dbmail** restituisce un errore.  
  
`[ @copy_recipients = ] 'copy_recipients'` Elenco delimitato da punti e virgola degli indirizzi di posta elettronica in cui copiare il messaggio. L'elenco dei destinatari della copia è di tipo **varchar (max)**. Sebbene questo parametro sia facoltativo, è necessario specificare almeno uno dei **\@ destinatari**, **\@ copy_recipients** o **\@ blind_copy_recipients** oppure **sp_send_dbmail** restituisce un errore.  
  
`[ @blind_copy_recipients = ] 'blind_copy_recipients'` Elenco delimitato da punti e virgola degli indirizzi di posta elettronica a cui copiare il messaggio in modo cieco. L'elenco dei destinatari della copia cieca è di tipo **varchar (max)**. Sebbene questo parametro sia facoltativo, è necessario specificare almeno uno dei **\@ destinatari**, **\@ copy_recipients** o **\@ blind_copy_recipients** oppure **sp_send_dbmail** restituisce un errore.  
  
`[ @from_address = ] 'from_address'` Valore dell'indirizzo di posta elettronica del messaggio. Si tratta di un parametro facoltativo utilizzato per eseguire l'override delle impostazioni nel profilo di posta elettronica. Questo parametro è di tipo **varchar (max)**. Le impostazioni di sicurezza SMTP determinano se questi override vengono accettati. Se non è specificato alcun parametro, il valore predefinito è NULL.  
  
`[ @reply_to = ] 'reply_to'` È il valore di ' Reply to address ' del messaggio di posta elettronica. Accetta un solo indirizzo di posta elettronica come valore valido. Si tratta di un parametro facoltativo utilizzato per eseguire l'override delle impostazioni nel profilo di posta elettronica. Questo parametro è di tipo **varchar (max)**. Le impostazioni di sicurezza SMTP determinano se questi override vengono accettati. Se non è specificato alcun parametro, il valore predefinito è NULL.  
  
`[ @subject = ] 'subject'` Oggetto del messaggio di posta elettronica. L'oggetto è di tipo **nvarchar (255)**. Se l'oggetto viene omesso, il valore predefinito è "Messaggio SQL Server".  
  
`[ @body = ] 'body'` Corpo del messaggio di posta elettronica. Il corpo del messaggio è di tipo **nvarchar (max)** e il valore predefinito è null.  
  
`[ @body_format = ] 'body_format'` Formato del corpo del messaggio. Il parametro è di tipo **varchar (20)** e il valore predefinito è null. Se specificato, le intestazioni del messaggio in uscita vengono impostate in modo da indicare che per il corpo del messaggio è impostato il formato specificato. Il parametro può includere uno dei valori seguenti:  
  
-   TEXT  
  
-   HTML  
  
 Il valore predefinito è TEXT.  
  
`[ @importance = ] 'importance'` Importanza del messaggio. Il parametro è di tipo **varchar (6)**. Il parametro può includere uno dei valori seguenti:  
  
-   Basso  
  
-   Normale  
  
-   Alto  
  
 Il valore predefinito è Normal.  
  
`[ @sensitivity = ] 'sensitivity'` È la riservatezza del messaggio. Il parametro è di tipo **varchar (12)**. Il parametro può includere uno dei valori seguenti:  
  
-   Normale  
  
-   Personal  
  
-   Privato  
  
-   Riservato  
  
 Il valore predefinito è Normal.  
  
`[ @file_attachments = ] 'file_attachments'` Elenco delimitato da punti e virgola di nomi di file da aggiungere al messaggio di posta elettronica. I file nell'elenco devono essere specificati come percorsi assoluti. L'elenco degli allegati è di tipo **nvarchar (max)**. Per impostazione predefinita, Posta elettronica database limita le dimensioni degli allegati a 1 MB per file.  
 
 > [!IMPORTANT]
 > Questo parametro non è disponibile nel Istanza gestita SQL di Azure perché non è in grado di accedere al file system locale.
  
`[ @query = ] 'query'` Query da eseguire. I risultati della query possono essere allegati come file o inclusi nel corpo del messaggio di posta elettronica. La query è di tipo **nvarchar (max)** e può contenere qualsiasi istruzione valida [!INCLUDE[tsql](../../includes/tsql-md.md)] . Si noti che la query viene eseguita in una sessione separata, quindi le variabili locali nello script che chiama **sp_send_dbmail** non sono disponibili per la query.  
  
`[ @execute_query_database = ] 'execute_query_database'` Contesto del database in cui il stored procedure esegue la query. Il parametro è di tipo **sysname** e il valore predefinito è il database corrente. Questo parametro è applicabile solo se viene specificata la **\@ query** .  
  
`[ @attach_query_result_as_file = ] attach_query_result_as_file` Specifica se il set di risultati della query viene restituito come file allegato. *attach_query_result_as_file* è di tipo **bit** e il valore predefinito è 0.  
  
 Quando il valore è 0, i risultati della query vengono inclusi nel corpo del messaggio di posta elettronica, dopo il contenuto del parametro del **\@ corpo** . Con il valore 1, i risultati vengono restituiti come allegato. Questo parametro è applicabile solo se viene specificata la **\@ query** .  
  
`[ @query_attachment_filename = ] query_attachment_filename` Specifica il nome file da utilizzare per il set di risultati dell'allegato della query. *query_attachment_filename* è di tipo **nvarchar (255)** e il valore predefinito è null. Questo parametro viene ignorato quando *attach_query_result* è 0. Quando *attach_query_result* è 1 e questo parametro è NULL, posta elettronica database crea un nome file arbitrario.  
  
`[ @query_result_header = ] query_result_header` Specifica se i risultati della query includono le intestazioni di colonna. Il valore query_result_header è di tipo **bit**. Quando il valore è 1, i risultati della query contengono le intestazioni di colonna. Quando il valore è 0, i risultati della query non includono le intestazioni di colonna. Il valore predefinito di questo parametro è **1**. Questo parametro è applicabile solo se viene specificata la **\@ query** .  
 
   >[!NOTE]
   > Quando si imposta \@ query_result_header su 0 e si imposta query_no_truncate su 1, è possibile che si verifichi l'errore seguente \@ :
   > <br> Messaggio 22050, livello 16, stato 1, riga 12: Impossibile inizializzare la libreria sqlcmd con numero di errore-2147024809.
  
`[ @query_result_width = ] query_result_width` Lunghezza riga, in caratteri, da utilizzare per la formattazione dei risultati della query. Il *query_result_width* è di tipo **int** e il valore predefinito è 256. Il valore fornito deve essere compreso tra 10 e 32767. Questo parametro è applicabile solo se viene specificata la **\@ query** .  
  
`[ @query_result_separator = ] 'query_result_separator'` Carattere utilizzato per separare le colonne nell'output della query. Il separatore è di tipo **char (1)**. Il valore predefinito è ' ' (spazio).  
  
`[ @exclude_query_output = ] exclude_query_output` Specifica se restituire l'output dell'esecuzione della query nel messaggio di posta elettronica. **exclude_query_output** è di bit e il valore predefinito è 0. Quando questo parametro è 0, l'esecuzione del **sp_send_dbmail** stored procedure stampa il messaggio restituito come risultato dell'esecuzione della query nella console. Quando questo parametro è 1, l'esecuzione del **sp_send_dbmail** stored procedure non stampa alcun messaggio di esecuzione di query nella console di.  
  
`[ @append_query_error = ] append_query_error`Specifica se inviare il messaggio di posta elettronica quando un errore viene restituito dalla query specificata nell'argomento della **\@ query** . **append_query_error** è di **bit** e il valore predefinito è 0. Quando questo parametro è 1, Posta elettronica database invia il messaggio di posta elettronica e include il messaggio di errore della query nel corpo del messaggio di posta elettronica. Quando questo parametro è 0, Posta elettronica database non invia il messaggio di posta elettronica e **sp_send_dbmail** termina con il codice restituito 1, che indica un errore.  
  
`[ @query_no_truncate = ] query_no_truncate` Specifica se eseguire la query con l'opzione che consente di evitare il troncamento dei tipi di dati a lunghezza variabile di grandi dimensioni (**varchar (max)**, **nvarchar (max)**, **varbinary (max)**, **XML**, **Text**, **ntext**, **Image** e tipi di dati definiti dall'utente). Quando il parametro è impostato, i risultati della query non includono le intestazioni di colonna. Il valore *query_no_truncate* è di tipo **bit**. Quando il valore è 0 o viene omesso, la lunghezza delle colonne della query viene troncata a 256 caratteri. Quando il valore è 1, le colonne della query non vengono troncate. Il valore predefinito del parametro è 0.  
  
> [!NOTE]  
>  Se utilizzata con grandi quantità di dati, l' \@ opzione **query_no_truncate** utilizza risorse aggiuntive e può rallentare le prestazioni del server.  
  
`[ @query_result_no_padding ] @query_result_no_padding` Il tipo è bit. Il valore predefinito è 0. Quando si imposta su 1, i risultati della query non vengono riempiti, possibilmente riducendo le dimensioni del file. Se si imposta \@ query_result_no_padding su 1 e si imposta il \@ parametro query_result_width, il parametro \@ query_result_no_padding sovrascrive il parametro \@ query_result_width.  
  
 In questo caso, non si verifica alcun errore.  
 
  >[!NOTE]
  > Quando si imposta \@ query_result_no_padding su 1 e si specifica un parametro per query_no_truncate, è possibile che si verifichi l'errore seguente \@ :
  > <br> Messaggio 22050, livello 16, stato 1, riga 0: non è stato possibile eseguire la query perché le \@ opzioni query_result_no_append e query_no_truncate si escludono a \@ vicenda. 
  
 Se si imposta il \@ query_result_no_padding su 1 e si imposta il \@ parametro query_no_truncate, viene generato un errore.  
  
`[ @mailitem_id = ] mailitem_id [ OUTPUT ]` Il parametro di output facoltativo restituisce la *mailitem_id* del messaggio. Il *mailitem_id* è di tipo **int**.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 In caso di esito positivo viene restituito il codice 0. Qualsiasi altro valore indica esito negativo. Il codice di errore per l'istruzione che ha avuto esito negativo viene archiviato nella \@ \@ variabile di errore.  
  
## <a name="result-sets"></a>Set di risultati  
 In caso di esito positivo, viene restituito il messaggio "Posta elettronica accodata".  
  
## <a name="remarks"></a>Commenti  
 Prima di utilizzare, è necessario abilitare Posta elettronica database utilizzando la configurazione guidata Posta elettronica database o **sp_configure**.  
  
 **sysmail_stop_sp** interrompe posta elettronica database arrestando gli oggetti Service Broker utilizzati dal programma esterno. **sp_send_dbmail** accetta comunque la posta quando posta elettronica database viene interrotto utilizzando **sysmail_stop_sp**. Per avviare Posta elettronica database, utilizzare **sysmail_start_sp**.  
  
 Quando **\@ profile** non è specificato, **sp_send_dbmail** utilizza un profilo predefinito. Se l'utente che invia il messaggio di posta elettronica dispone di un profilo privato predefinito, Posta elettronica database utilizzerà tale profilo. Se l'utente non dispone di un profilo privato predefinito, **sp_send_dbmail** utilizza il profilo pubblico predefinito. Se non è disponibile un profilo privato predefinito per l'utente e nessun profilo pubblico predefinito, **sp_send_dbmail** restituisce un errore.  
  
 **sp_send_dbmail** non supporta i messaggi di posta elettronica senza contenuto. Per inviare un messaggio di posta elettronica, è necessario specificare almeno un **\@ corpo**, una **\@ query**, una **\@ file_attachments** o un **\@ oggetto**. In caso contrario, **sp_send_dbmail** restituisce un errore.  
  
 Per controllare l'accesso ai file, Posta elettronica database utilizza il contesto di sicurezza di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows dell'utente corrente. Di conseguenza, gli utenti autenticati con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'autenticazione non possono alleghi file utilizzando **\@ file_attachments**. Windows non consente a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di passare credenziali da un computer remoto a un altro computer remoto. Per questo motivo, Posta elettronica database potrebbe non essere in grado di allegare file da una condivisione di rete nei casi in cui il comando viene eseguito da un computer diverso dal computer in cui è in esecuzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Se vengono specificate sia **\@ query** che **\@ file_attachments** e non è possibile trovare il file, la query viene comunque eseguita, ma il messaggio di posta elettronica non viene inviato.  
  
 Se si specifica una query, il set dei risultati viene formattato come testo incorporato. I dati binari nel risultato vengono inviati in formato esadecimale.  
  
 I parametri **\@ Recipients**, **\@ copy_recipients** e **\@ blind_copy_recipients** sono elenchi delimitati da punti e virgola di indirizzi di posta elettronica. È necessario fornire almeno uno di questi parametri oppure **sp_send_dbmail** restituisce un errore.  
  
 Quando si esegue **sp_send_dbmail** senza un contesto di transazione, posta elettronica database avvia ed esegue il commit di una transazione implicita. Quando si esegue **sp_send_dbmail** dall'interno di una transazione esistente, posta elettronica database si basa sull'utente per eseguire il commit o il rollback di tutte le modifiche. Posta elettronica database non avvia una transazione interna.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione per **sp_send_dbmail** predefinite a tutti i membri del ruolo del database **DatabaseMailUser** nel database **msdb** . Tuttavia, quando l'utente che invia il messaggio non dispone dell'autorizzazione per utilizzare il profilo per la richiesta, **sp_send_dbmail** restituisce un errore e non invia il messaggio.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-sending-an-e-mail-message"></a>R. Invio di un messaggio di posta elettronica  
 Questo esempio Invia un messaggio di posta elettronica al proprio amico usando l'indirizzo di posta elettronica `myfriend@Adventure-Works.com` . L'oggetto del messaggio è `Automated Success Message`. Il corpo del messaggio contiene la frase `'The stored procedure finished successfully'`.  
  
```  
EXEC msdb.dbo.sp_send_dbmail  
    @profile_name = 'Adventure Works Administrator',  
    @recipients = 'yourfriend@Adventure-Works.com',  
    @body = 'The stored procedure finished successfully.',  
    @subject = 'Automated Success Message' ;  
```  
  
### <a name="b-sending-an-e-mail-message-with-the-results-of-a-query"></a>B. Invio di un messaggio di posta elettronica con i risultati di una query  
 Questo esempio Invia un messaggio di posta elettronica al proprio amico usando l'indirizzo di posta elettronica `yourfriend@Adventure-Works.com` . L'oggetto del messaggio è `Work Order Count` e il messaggio esegue una query che visualizza il numero degli ordini di lavoro con una data di scadenza `DueDate` minore di due giorni dopo il 30 aprile 2004. I risultati vengono allegati come file di testo.  
  
```  
EXEC msdb.dbo.sp_send_dbmail  
    @profile_name = 'Adventure Works Administrator',  
    @recipients = 'yourfriend@Adventure-Works.com',  
    @query = 'SELECT COUNT(*) FROM AdventureWorks2012.Production.WorkOrder  
                  WHERE DueDate > ''2004-04-30''  
                  AND  DATEDIFF(dd, ''2004-04-30'', DueDate) < 2' ,  
    @subject = 'Work Order Count',  
    @attach_query_result_as_file = 1 ;  
```  
  
### <a name="c-sending-an-html-e-mail-message"></a>C. Invio di un messaggio di posta elettronica HTML  
 Questo esempio Invia un messaggio di posta elettronica al proprio amico usando l'indirizzo di posta elettronica `yourfriend@Adventure-Works.com` . L'oggetto del messaggio è `Work Order List` e il messaggio contiene un documento HTML che visualizza gli ordini di lavoro con una data di scadenza `DueDate` minore di due giorni dopo il 30 aprile 2004. Il messaggio viene inviato in formato HTML.  
  
```  
DECLARE @tableHTML  NVARCHAR(MAX) ;  
  
SET @tableHTML =  
    N'<H1>Work Order Report</H1>' +  
    N'<table border="1">' +  
    N'<tr><th>Work Order ID</th><th>Product ID</th>' +  
    N'<th>Name</th><th>Order Qty</th><th>Due Date</th>' +  
    N'<th>Expected Revenue</th></tr>' +  
    CAST ( ( SELECT td = wo.WorkOrderID,       '',  
                    td = p.ProductID, '',  
                    td = p.Name, '',  
                    td = wo.OrderQty, '',  
                    td = wo.DueDate, '',  
                    td = (p.ListPrice - p.StandardCost) * wo.OrderQty  
              FROM AdventureWorks.Production.WorkOrder as wo  
              JOIN AdventureWorks.Production.Product AS p  
              ON wo.ProductID = p.ProductID  
              WHERE DueDate > '2004-04-30'  
                AND DATEDIFF(dd, '2004-04-30', DueDate) < 2   
              ORDER BY DueDate ASC,  
                       (p.ListPrice - p.StandardCost) * wo.OrderQty DESC  
              FOR XML PATH('tr'), TYPE   
    ) AS NVARCHAR(MAX) ) +  
    N'</table>' ;  
  
EXEC msdb.dbo.sp_send_dbmail @recipients='yourfriend@Adventure-Works.com',  
    @subject = 'Work Order List',  
    @body = @tableHTML,  
    @body_format = 'HTML' ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Posta elettronica database](../../relational-databases/database-mail/database-mail.md)   
 [Oggetti di configurazione Posta elettronica database](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Stored procedure di Posta elettronica database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)  
  
  
