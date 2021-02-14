---
description: Errori comuni con Posta elettronica database
title: Errori comuni con Posta elettronica database | Microsoft Docs
ms.custom: ''
ms.date: 04/22/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- architecture [SQL Server], Database Mail
- Database Mail [SQL Server], architecture
- Database Mail [SQL Server], components
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 0df7b437ff897966502291975a99d56a64003f08
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272362"
---
# <a name="common-errors-with-database-mail"></a>Errori comuni con Posta elettronica database 
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

Questo articolo descrive alcuni errori comuni riscontrati con Posta elettronica database e le relative soluzioni.

## <a name="could-not-find-stored-procedure-sp_send_dbmail"></a>Impossibile trovare la stored procedure 'sp_send_dbmail'
La stored procedure [sp_send_dbmail](../system-stored-procedures/sp-send-dbmail-transact-sql.md) è installata nel database msdb. È necessario eseguire **sp_send_dbmail** dal database msdb oppure specificare un nome in tre parti per la stored procedure.

Esempio:
```sql
EXEC msdb.dbo.sp_send_dbmail ...
```

Oppure:

```sql
USE msdb;
GO
EXEC dbo.sp_send_dbmail ...
```

Usare la [Configurazione guidata Posta elettronica database](configure-database-mail.md) per abilitare e configurare Posta elettronica database.

## <a name="profile-not-valid"></a>Profilo non valido
Questo messaggio può avere due cause, ovvero il profilo non esiste oppure l'utente che esegue [sp_send_dbmail (Transact-SQL)](../system-stored-procedures/sp-send-dbmail-transact-sql.md) non ha le autorizzazioni necessarie per accedere al profilo.

Per controllare le autorizzazioni per un profilo, eseguire la stored procedure [sysmail_help_principalprofile_sp (Transact-SQL)](../system-stored-procedures/sysmail-help-principalprofile-sp-transact-sql.md) con il nome del profilo. Usare la stored procedure [sysmail_add_principalprofile_sp (Transact-SQL)](../system-stored-procedures/sysmail-help-principalprofile-sp-transact-sql.md) o la [Configurazione guidata Posta elettronica database](configure-database-mail.md) per concedere l'autorizzazione di accesso a un profilo a un utente o un gruppo di msdb.

## <a name="permission-denied-on-sp_send_dbmail"></a>Autorizzazione negata per sp_send_dbmail

In questo argomento viene descritto come risolvere il problema segnalato da un messaggio di errore in cui si informa che l'utente che sta tentando di inviare messaggi di Posta elettronica database non è autorizzato a eseguire sp_send_dbmail.

Il testo del messaggio di errore è il seguente:

```
EXECUTE permission denied on object 'sp_send_dbmail', 
database 'msdb', schema 'dbo'.
```

Per poter inviare messaggi di Posta elettronica database, è necessario essere un utente nel database msdb e un membro del ruolo del database DatabaseMailUserRole nel database msdb. Per aggiungere utenti o gruppi di msdb a questo ruolo, usare SQL Server Management Studio oppure eseguire l'istruzione seguente per l'utente o il ruolo che deve inviare messaggi di Posta elettronica database.

```sql
EXEC msdb.dbo.sp_addrolemember @rolename = 'DatabaseMailUserRole'
    ,@membername = '<user or role name>';
GO
```
Per altre informazioni, vedere [sp_addrolemember](../system-stored-procedures/sp-addrolemember-transact-sql.md) e [sp_droprolemember](../system-stored-procedures/sp-droprolemember-transact-sql.md).

## <a name="database-mail-queued-no-entries-in-sysmail_event_log-or-windows-application-event-log"></a>Posta elettronica accodata, nessuna voce in sysmail_event_log o nel registro eventi applicazioni di Windows 

Posta elettronica database si basa su Service Broker per l'accodamento dei messaggi di posta elettronica. Se l'esecuzione di Posta elettronica database viene arrestata o se il recapito dei messaggi di Service Broker non è attivato nel database **msdb**, Posta elettronica database accoda i messaggi nel database ma non è in grado di recapitarli. In questo caso, i messaggi di Service Broker rimangono nella coda della posta di Service Broker. Service Broker non attiva il programma esterno, quindi non ci sono voci di log in **sysmail_event_log** né aggiornamenti allo stato dell'elemento in **sysmail_allitems** e nelle viste correlate.

Eseguire l'istruzione seguente per verificare se Service Broker è abilitato nel database **msdb**:

```sql
SELECT is_broker_enabled FROM sys.databases WHERE name = 'msdb';
```

Un valore 0 indica che il recapito dei messaggi di Service Broker non è attivato nel database msdb. Per correggere il problema, attivare Service Broker nel database con il comando Transact-SQL seguente:

```sql
USE master ;
GO

ALTER DATABASE msdb SET ENABLE_BROKER ;
GO
``` 

Posta elettronica database si basa su varie stored procedure interne. Per ridurre la superficie di attacco, queste stored procedure sono disabilitate in caso di nuova installazione di SQL Server. Per abilitare queste stored procedure, usare l'[opzione Database Mail XPs](../../database-engine/configure-windows/database-mail-xps-server-configuration-option.md) della stored procedure di sistema **sp_configure**, come nell'esempio seguente:

```sql
EXEC sp_configure 'show advanced options', 1;  
RECONFIGURE;
EXEC sp_configure 'Database Mail XPs', 1;  
RECONFIGURE  
GO  

Database Mail may be stopped in the **msdb** database. To check status of Database Mail, execute the following statement:

```sql
EXECUTE dbo.sysmail_help_status_sp;
```

Per avviare Posta elettronica database in un database host della posta elettronica, eseguire il comando seguente nel database msdb:

```sql
EXECUTE dbo.sysmail_start_sp;
```

Service Broker esamina la durata del dialogo per i messaggi all'attivazione. Pertanto, eventuali messaggi rimasti nella coda di trasmissione di Service Broker più a lungo della durata del dialogo generano immediatamente un errore. Posta elettronica database aggiorna lo stato dei messaggi non riusciti nella tabella [sysmail_allitems](../system-catalog-views/sysmail-allitems-transact-sql.md) e nelle viste correlate. È necessario che l'utente decida se ripetere l'invio dei messaggi di posta elettronica. Per altre informazioni sulla configurazione della durata del dialogo predefinita usata da Posta elettronica database, vedere [sysmail_configure_sp (Transact-SQL)](../system-stored-procedures/sysmail-configure-sp-transact-sql.md).



##  <a name="see-also"></a><a name="RelatedContent"></a> Vedere anche
  
-  [Panoramica di Posta elettronica database](database-mail.md)
-  [Creare un profilo di Posta elettronica database](create-a-database-mail-profile.md)
  
  
