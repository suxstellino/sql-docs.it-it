---
description: sysmail_configure_sp (Transact-SQL)
title: sysmail_configure_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_configure_sp_TSQL
- sysmail_configure_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_configure_sp
ms.assetid: 73b33c56-2bff-446a-b495-ae198ad74db1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c150f6707a5f0ec537613617182b9da9889d85a9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188655"
---
# <a name="sysmail_configure_sp-transact-sql"></a>sysmail_configure_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica le impostazioni di configurazione per Posta elettronica database. Le impostazioni di configurazione specificate con **sysmail_configure_sp** si applicano all'intera [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] istanza.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sysmail_configure_sp [ [ @parameter_name = ] 'parameter_name' ]  
    [ , [ @parameter_value = ] 'parameter_value' ]  
    [ , [ @description = ] 'description' ]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ **@parameter_name** =] **'**_parameter_name_**'**  
 Nome del parametro da modificare.  
  
 [ **@parameter_value** =] **'**_parameter_value_**'**  
 Nuovo valore del parametro  
  
 [ **@description** =] **'**_Description_**'**  
 Descrizione del parametro.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Posta elettronica database utilizza i parametri seguenti:  
  
| Nome parametro | Descrizione | Valore predefinito |
| -------------- | ----------- | ------------- |
|*AccountRetryAttempts*|Numero di tentativi di invio del messaggio di posta elettronica da parte del processo di posta elettronica esterno, utilizzando ogni account nel profilo specificato.|**1**|  
|*AccountRetryDelay*|Tempo di attesa, in secondi, del processo di posta elettronica esterno tra tentativi di invio di un messaggio.|**5000**|  
|*DatabaseMailExeMinimumLifeTime*|Periodo minimo di tempo, in secondi, durante il quale il processo di posta elettronica esterno resta attivo. Quando Posta elettronica database invia molti messaggi, aumentare questo valore per far restare Posta elettronica database attivo ed evitare l'overhead di avvii e arresti frequenti.|**600**|  
|*DefaultAttachmentEncoding*|Codifica predefinita per gli allegati di posta elettronica.|MIME|  
|*MaxFileSize*|Dimensioni massime di un allegato, in byte.|**1 milione**|  
|*ProhibitedExtensions*|Elenco delimitato da virgole delle estensioni che non possono essere inviate come allegato a un messaggio di posta elettronica.|**exe,dll,vbs,js**|  
|*LoggingLevel*|Consente di specificare i messaggi registrati nel log di Posta elettronica database. Uno dei valori numerici seguenti:<br /><br /> 1 - Modalità normale. Solo registrazione degli errori.<br /><br /> 2 - Modalità estesa. Registrazione di messaggi di errore, di avviso e informativi.<br /><br /> 3 - Modalità dettagliata. Registrazione di messaggi di errore, di avviso, informativi, di riuscita, nonché di messaggi interni aggiuntivi. Utilizzare questa modalità per la risoluzione dei problemi.|**2**|  
  
 Il stored procedure **sysmail_configure_sp** si trova nel database **msdb** ed è di proprietà dello schema **dbo** . La procedura deve essere eseguita con un nome in tre parti se il database corrente non è **msdb**.  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni di esecuzione per questa procedura vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="examples"></a>Esempio  
 **A.  Impostazione di Posta elettronica database per eseguire il tentativo con ogni account 10 volte**  
  
 Nell'esempio seguente viene illustrato come impostare Posta elettronica database per ritentare ogni account dieci volte prima di considerare l'account irraggiungibile.  
  
```  
EXECUTE msdb.dbo.sysmail_configure_sp  
    'AccountRetryAttempts', '10' ;  
```  
  
 **B. Impostazione delle dimensioni massime per gli allegati su 2 MB.**  
  
 Nell'esempio seguente viene illustrato come impostare le dimensioni massime per gli allegati su 2 MB.  
  
```  
EXECUTE msdb.dbo.sysmail_configure_sp  
    'MaxFileSize', '2097152' ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Posta elettronica database](../../relational-databases/database-mail/database-mail.md)   
 [sysmail_help_configure_sp &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sysmail-help-configure-sp-transact-sql.md)   
 [Stored procedure di Posta elettronica database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
