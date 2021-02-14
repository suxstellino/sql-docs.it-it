---
title: Avvisi di Posta elettronica database e di posta elettronica con SQL Agent in Linux
description: Informazioni su come usare DB Mail e come configurare gli avvisi di posta elettronica con SQL Server Agent (mssql-server-agent) in Linux.
author: VanMSFT
ms.author: vanto
ms.date: 02/20/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: tbd
ms.openlocfilehash: 882ab5c8bff2acd7c93cc8cbeeba135152ce76ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348175"
---
# <a name="db-mail-and-email-alerts-with-sql-agent-on-linux"></a>Avvisi di Posta elettronica database e di posta elettronica con SQL Agent in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

I passaggi seguenti illustrano come configurare e usare Posta elettronica database con SQL Server Agent (**mssql-server-agent**) in Linux. 

## <a name="1-enable-db-mail"></a>1. Abilitare Posta elettronica database

```sql
USE master 
GO 
sp_configure 'show advanced options',1 
GO 
RECONFIGURE WITH OVERRIDE 
GO 
sp_configure 'Database Mail XPs', 1 
GO 
RECONFIGURE  
GO  
```

## <a name="2-create-a-new-account"></a>2. Creare un nuovo account
```sql
EXECUTE msdb.dbo.sysmail_add_account_sp 
@account_name = 'SQLAlerts', 
@description = 'Account for Automated DBA Notifications', 
@email_address = 'sqlagenttest@gmail.com', 
@replyto_address = 'sqlagenttest@gmail.com', 
@display_name = 'SQL Agent', 
@mailserver_name = 'smtp.gmail.com', 
@port = 587, 
@enable_ssl = 1, 
@username = 'sqlagenttest@gmail.com', 
@password = '<password>' 
GO
```

## <a name="3-create-a-default-profile"></a>3. Creare un profilo predefinito

```sql
EXECUTE msdb.dbo.sysmail_add_profile_sp 
@profile_name = 'default', 
@description = 'Profile for sending Automated DBA Notifications' 
GO
```

## <a name="4-add-the-database-mail-account-to-a-database-mail-profile"></a>4. Aggiungere un account di Posta elettronica database a un profilo di Posta elettronica database
```sql
EXECUTE msdb.dbo.sysmail_add_principalprofile_sp 
@profile_name = 'default', 
@principal_name = 'public', 
@is_default = 1 ; 
 ```
 
## <a name="5-add-account-to-profile"></a>5. Aggiungere l'account al profilo 
```sql
EXECUTE msdb.dbo.sysmail_add_profileaccount_sp   
@profile_name = 'default',   
@account_name = 'SQLAlerts',   
@sequence_number = 1;  
 ```
 
## <a name="6-send-test-email"></a>6. Inviare un messaggio di posta elettronica di prova
> [!NOTE]
> Potrebbe essere necessario passare al client di posta elettronica e abilitare "allow less secure clients to send mail" (Consenti ai client meno sicuri di inviare posta elettronica). Non tutti i client riconoscono Posta elettronica database come daemon di posta elettronica.

```
EXECUTE msdb.dbo.sp_send_dbmail 
@profile_name = 'default', 
@recipients = 'recipient-email@gmail.com', 
@Subject = 'Testing DBMail', 
@Body = 'This message is a test for DBMail' 
GO
```

## <a name="7-set-db-mail-profile-using-mssql-conf-or-environment-variable"></a>7. Impostare il profilo di Posta elettronica database usando mssql-conf o una variabile di ambiente
Per registrare il profilo di Posta elettronica database, è possibile usare l'utilità mssql-conf o variabili di ambiente. In questo caso, il profilo verrà chiamato default.

```bash
# via mssql-conf
sudo /opt/mssql/bin/mssql-conf set sqlagent.databasemailprofile default
# via environment variable
MSSQL_AGENT_EMAIL_PROFILE=default
```

## <a name="8-set-up-an-operator-for-sqlagent-job-notifications"></a>8. Configurare un operatore per le notifiche dei processi SQLAgent 

```sql
EXEC msdb.dbo.sp_add_operator 
@name=N'JobAdmins',  
@enabled=1, 
@email_address=N'recipient-email@gmail.com',  
@category_name=N'[Uncategorized]' 
GO 
```

## <a name="9-send-email-when-agent-test-job-succeeds"></a>9. Inviare un messaggio di posta elettronica quando il processo 'Agent Test Job' ha esito positivo 

```
EXEC msdb.dbo.sp_update_job 
@job_name='Agent Test Job', 
@notify_level_email=1, 
@notify_email_operator_name=N'JobAdmins' 
GO
```

## <a name="next-steps"></a>Passaggi successivi
Per altre informazioni su come usare SQL Server Agent per creare, pianificare ed eseguire processi, vedere [Eseguire un processo di SQL Server Agent in Linux](sql-server-linux-run-sql-server-agent-job.md).
