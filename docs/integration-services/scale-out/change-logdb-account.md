---
title: Modificare l'account per la registrazione di SSIS Scale Out | Microsoft Docs
description: Informazioni su come modificare l'account usato per la registrazione di SQL Server Integration Services (SSIS) Scale Out.
ms.custom: performance
ms.date: 06/29/2020
ms.prod: sql
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.reviewer: maghan
ms.openlocfilehash: b36b01a416a5468def2baa66cd4395b0944a49cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354903"
---
# <a name="change-the-account-for-scale-out-logging"></a>Modificare l'account per la registrazione di Scale Out

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Durante l'esecuzione di pacchetti SSIS in Scale Out, i messaggi di evento vengono registrati nel database SSISDB con un account utente creato automaticamente, denominato **##MS_SSISLogDBWorkerAgentLogin##** . L'account di accesso dell'utente usa l'autenticazione di SQL Server.

Se si vuole modificare l'account usato per la registrazione in Scale Out, eseguire le operazioni seguenti:

> [!NOTE]
> Se si usa un account utente di Windows per la registrazione, usare lo stesso account che esegue il servizio Scale Out Worker. In caso contrario, l'accesso a SQL Server non riesce.

## <a name="1-create-a-user-for-ssisdb"></a>1. Creare un utente di SSISDB
Per istruzioni sulla creazione di un utente del database, vedere [Creare un utente di database](../../relational-databases/security/authentication-access/create-a-database-user.md).

## <a name="2-add-the-user-to-the-database-role-ssis_cluster_worker"></a>2. Aggiungere l'utente al ruolo del database ssis_cluster_worker

Per istruzioni sull'aggiunta di un ruolo del database, vedere [Aggiungere un ruolo](../../relational-databases/security/authentication-access/join-a-role.md).

## <a name="3-update-the-logging-information-in-ssisdb"></a>3. Aggiornare le informazioni di registrazione in SSISDB
Chiamare la stored procedure `[catalog].[update_logdb_info]` con la stringa del nome del server SQL Server e la stringa di connessione come parametri, come illustrato nell'esempio seguente:

```sql
SET @serverName = CONVERT(sysname, SERVERPROPERTY('servername'))
SET @connectionString = 'Data Source=' + @serverName + ';Initial Catalog=SSISDB;Integrated Security=SSPI;'
EXEC [internal].[update_logdb_info] @serverName, @connectionString
GO
```

## <a name="4-restart-the-scale-out-worker-service"></a>4. Riavviare il servizio Scale Out Worker
Riavviare il servizio Scale Out Worker per rendere effettiva la modifica.

## <a name="next-steps"></a>Passaggi successivi
-   [Integration Services Scale Out Manager](integration-services-ssis-scale-out-manager.md)
