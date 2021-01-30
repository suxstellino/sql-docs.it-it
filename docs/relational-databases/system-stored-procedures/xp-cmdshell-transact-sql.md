---
description: xp_cmdshell (Transact-SQL)
title: xp_cmdshell (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_cmdshell
- xp_cmdshell_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- xp_cmdshell
ms.assetid: 18935cf4-b320-4954-b6c1-e007fcefe358
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 73bea06c7919c40b1080458f3e6982d2c10ebe47
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99125007"
---
# <a name="xp_cmdshell-transact-sql"></a>xp_cmdshell (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Genera una shell dei comandi di Windows e passa una stringa per l'esecuzione. L'eventuale output viene restituito in forma di righe di testo.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
xp_cmdshell { 'command_string' } [ , no_output ]  
```  
  
## <a name="arguments"></a>Argomenti  
 **'** *command_string* **'**  
 Stringa che contiene un comando da passare al sistema operativo. *command_string* è di tipo **varchar (8000)** o **nvarchar (4000)** e non prevede alcun valore predefinito. *command_string* non può contenere più di un set di virgolette doppie. È necessaria una sola coppia di virgolette se sono presenti spazi nei percorsi di file o nei nomi di programma a cui viene fatto riferimento in *command_string*. In caso di problemi nell'utilizzo di spazi incorporati nelle stringhe, valutare l'utilizzo di nomi di file in formato FAT 8.3 come soluzione alternativa.  
  
 **no_output**  
 Parametro facoltativo che indica che non è richiesta la restituzione di output al client.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 L'istruzione `xp_cmdshell` seguente restituisce un elenco dei file con estensione exe contenuti nella directory corrente.  
  
```  
EXEC xp_cmdshell 'dir *.exe';  
GO  
```  
  
 Le righe vengono restituite in una colonna **nvarchar (255)** . Se viene utilizzata l'opzione **no_output** , verranno restituiti solo gli elementi seguenti:  
  
```  
The command(s) completed successfully.  
```  
  
## <a name="remarks"></a>Commenti  
 Il processo di Windows generato da **xp_cmdshell** dispone degli stessi diritti di sicurezza dell' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account del servizio.  
 
> [!IMPORTANT]
>  **xp_cmdshell** è una funzionalità molto potente ed è disabilitata per impostazione predefinita. **xp_cmdshell** può essere abilitata e disabilitata utilizzando la gestione basata su criteri o eseguendo **sp_configure**. Per ulteriori informazioni, vedere [Configurazione superficie](../../relational-databases/security/surface-area-configuration.md) di attacco e [xp_cmdshell opzione di configurazione del server](../../database-engine/configure-windows/xp-cmdshell-server-configuration-option.md).  
  
 **xp_cmdshell** funziona in modo sincrono. ovvero il controllo viene restituito al chiamante solo dopo il completamento del comando della shell. 
 
> [!IMPORTANT]
>  Se **xp_cmdshell** viene eseguito all'interno di un batch e restituisce un errore, il batch avrà esito negativo. Questa è una differenza funzionale Nelle versioni precedenti del [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] batch continuerà a essere eseguito.  
 
## <a name="xp_cmdshell-proxy-account"></a>Account proxy per xp_cmdshell  
 Quando viene chiamato da un utente che non è membro del ruolo predefinito del server **sysadmin** , **xp_cmdshell** si connette a Windows utilizzando il nome account e la password archiviati nella credenziale denominata **# #xp_cmdshell_proxy_account # #**. Se la credenziale proxy non esiste, **xp_cmdshell** avrà esito negativo.  
  
 È possibile creare le credenziali dell'account proxy eseguendo **sp_xp_cmdshell_proxy_account**. Questa stored procedure accetta un nome utente e una password di Windows come argomenti. Il comando seguente, ad esempio, crea una credenziale proxy per l'utente di dominio di Windows `SHIPPING\KobeR` con la password di Windows `sdfh%dkc93vcMt0`.  
  
```  
EXEC sp_xp_cmdshell_proxy_account 'SHIPPING\KobeR','sdfh%dkc93vcMt0';  
```  
  
 Per ulteriori informazioni, vedere [sp_xp_cmdshell_proxy_account &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-xp-cmdshell-proxy-account-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Poiché gli utenti malintenzionati a volte tentano di elevare i propri privilegi tramite **xp_cmdshell**, **xp_cmdshell** è disabilitato per impostazione predefinita. Usare **sp_configure** o la **gestione basata su criteri** per abilitarla. Per altre informazioni, vedere [Opzione di configurazione del server xp_cmdshell](../../database-engine/configure-windows/xp-cmdshell-server-configuration-option.md).  
  
 Quando è abilitata per la prima volta, **xp_cmdshell** richiede l'autorizzazione Control Server per l'esecuzione e il processo di Windows creato da **xp_cmdshell** ha lo stesso contesto di sicurezza dell' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account del servizio. L' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account del servizio ha spesso più autorizzazioni di quelle necessarie per il lavoro eseguito dal processo creato da **xp_cmdshell**. Per migliorare la sicurezza, l'accesso alle **xp_cmdshell** deve essere limitato agli utenti con privilegi elevati.  
  
 Per consentire a non amministratori di utilizzare **xp_cmdshell** e consentire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a di creare processi figlio con il token di sicurezza di un account con privilegi di meno, attenersi alla seguente procedura:  
  
1.  Creare e personalizzare un account utente locale di Windows o un account di dominio con i privilegi minimi richiesti dai processi.  
  
2.  Utilizzare la procedura di sistema **sp_xp_cmdshell_proxy_account** per configurare **xp_cmdshell** per l'utilizzo dell'account con privilegi minimi.  
  
    > [!NOTE]  
    >  È anche possibile configurare questo account proxy usando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] facendo clic con il pulsante destro del mouse su **Proprietà** sul nome del server in Esplora oggetti e cercando la scheda **sicurezza** per la sezione **account proxy server** .  
  
3.  In [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] , utilizzando il database master, eseguire l' `GRANT exec ON xp_cmdshell TO N'<some_user>';` istruzione per fornire a utenti specifici non **sysadmin** la possibilità di eseguire **xp_cmdshell**. L'utente specificato deve esistere nel database master.  
  
 Ora gli utenti non amministratori possono avviare i processi del sistema operativo con **xp_cmdshell** e questi processi vengono eseguiti con le autorizzazioni dell'account proxy configurato. Gli utenti che dispongono dell'autorizzazione CONTROL SERVER (membri del ruolo predefinito del server **sysadmin** ) continueranno a ricevere le autorizzazioni dell' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account del servizio per i processi figlio avviati dal **xp_cmdshell**.  
  
 Per determinare l'account di Windows utilizzato da **xp_cmdshell** all'avvio dei processi del sistema operativo, eseguire l'istruzione seguente:  
  
```  
xp_cmdshell 'whoami.exe'  
  
```  
  
 Per determinare il contesto di sicurezza per un altro accesso, eseguire gli elementi seguenti:  
  
```  
EXECUTE AS LOGIN = '<other_login>' ;  
GO  
xp_cmdshell 'whoami.exe' ;  
REVERT ;  
  
```  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-a-list-of-executable-files"></a>R. Restituzione di un elenco di file eseguibili  
 Nell'esempio seguente viene mostrato l'utilizzo della stored procedure estesa `xp_cmdshell` per eseguire un comando di directory.  
  
```  
EXEC master..xp_cmdshell 'dir *.exe'  
```  
  
### <a name="b-returning-no-output"></a>B. Esecuzione di un comando senza restituzione dell'output  
 Nell'esempio seguente viene mostrato l'utilizzo di `xp_cmdshell` per eseguire una stringa di comandi senza restituire alcun output al client.  
  
```  
USE master;  
  
EXEC xp_cmdshell 'copy c:\SQLbcks\AdvWorks.bck  
    \\server2\backups\SQLbcks', NO_OUTPUT;  
GO  
```  
  
### <a name="c-using-return-status"></a>C. Restituzione del codice di stato  
 Nell'esempio seguente, il `xp_cmdshell` stored procedure esteso suggerisce anche lo stato restituito. Il valore restituito viene archiviato nella variabile `@result`.  
  
```  
DECLARE @result int;  
EXEC @result = xp_cmdshell 'dir *.exe';  
IF (@result = 0)  
   PRINT 'Success'  
ELSE  
   PRINT 'Failure';  
```  
  
### <a name="d-writing-variable-contents-to-a-file"></a>D. Scrittura del contenuto di una variabile in un file  
 Nell'esempio seguente il contenuto della variabile `@var` viene scritto in un file denominato `var_out.txt` nella directory corrente del server.  
  
```  
DECLARE @cmd sysname, @var sysname;  
SET @var = 'Hello world';  
SET @cmd = 'echo ' + @var + ' > var_out.txt';  
EXEC master..xp_cmdshell @cmd;  
```  
  
### <a name="e-capturing-the-result-of-a-command-to-a-file"></a>E. Memorizzazione del risultato di un comando in un file  
 Nell'esempio seguente il contenuto della directory corrente viene scritto nel file `dir_out.txt` nella directory corrente del server.  
  
```  
DECLARE @cmd sysname, @var sysname;  
SET @var = 'dir/p';  
SET @cmd = @var + ' > dir_out.txt';  
EXEC master..xp_cmdshell @cmd;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure estese generali &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)   
 [Opzione di configurazione del server xp_cmdshell](../../database-engine/configure-windows/xp-cmdshell-server-configuration-option.md)   
 [Configurazione superficie di attacco](../../relational-databases/security/surface-area-configuration.md)   
 [sp_xp_cmdshell_proxy_account &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-xp-cmdshell-proxy-account-transact-sql.md)  
  
  
