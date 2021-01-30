---
description: SHUTDOWN (Transact-SQL)
title: SHUTDOWN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SHUTDOWN_TSQL
- SHUTDOWN
dev_langs:
- TSQL
helpviewer_keywords:
- SQL Server, stopping
- shutting down SQL Server
- SHUTDOWN statement
- stopping SQL Server
- immediately stopping SQL Server
ms.assetid: c8b03ff9-688c-4fe8-86e8-bd6bd401c9a4
author: cawrites
ms.author: chadam
ms.openlocfilehash: 05a6ab1f592bc1e06c8a0fbbd8b3b4908a0d6156
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184659"
---
# <a name="shutdown-transact-sql"></a>SHUTDOWN (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Arresta immediatamente SQL Server.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
SHUTDOWN [ WITH NOWAIT ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 WITH NOWAIT  
 Facoltativa. Viene arrestato [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] senza eseguire i checkpoint in ogni database. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene chiuso dopo il tentativo di interruzione di tutti i processi degli utenti. All'avvio successivo del server, verrà eseguita una operazione di rollback per le transazioni non completate.  
  
## <a name="remarks"></a>Osservazioni  
 A meno che non venga usata l'opzione WITHNOWAIT, SHUTDOWN arresta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eseguendo le operazioni seguenti:  
  
1.  Disabilitazione degli account di accesso, tranne quelli dei membri dei ruoli predefiniti del server **sysadmin** e **serveradmin**.  
  
    > [!NOTE]  
    >  Per visualizzare un elenco di tutti gli utenti correnti, eseguire **sp_who**.  
  
2.  Attesa del completamento delle istruzioni Transact-SQL o delle stored procedure in esecuzione. Per visualizzare un elenco di tutti i processi e i blocchi attivi, eseguire rispettivamente **sp_who** e **sp_lock**.  
  
3.  Inserimento di un checkpoint in ogni database.  
  
 Tramite l'istruzione SHUTDOWN è possibile ridurre la quantità di lavoro per il recupero automatico richiesta quando i membri del ruolo predefinito del server **sysadmin** riavviano [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Per arrestare l'esecuzione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è possibile utilizzare altri strumenti e metodi. Tali strumenti e metodi creano un checkpoint in tutti i database. È possibile scaricare dalla cache dei dati tutti i dati di cui è stato eseguito il commit e arrestare il server:  
  
-   Utilizzando Gestione configurazione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Eseguendo **net stop mssqlserver** dal prompt dei comandi per un'istanza predefinita oppure eseguendo **net stop mssql$**_instancename_ da un prompt dei comandi per un'istanza denominata.  
  
-   Utilizzando Servizi nel Pannello di controllo.  
  
 Se **sqlservr.exe** è stato avviato dal prompt dei comandi, per arrestare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] premere CTRL+C. In questo modo, tuttavia, non viene inserito un checkpoint.  
  
> [!NOTE]  
>  Se si utilizza uno di questi metodi per arrestare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene inviato il messaggio `SERVICE_CONTROL_STOP` a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="permissions"></a>Autorizzazioni  
 Le autorizzazioni per l'istruzione SHUTDOWN vengono assegnate ai membri dei ruoli predefiniti del server **sysadmin** e **serveradmin** e non sono trasferibili.  
  
## <a name="see-also"></a>Vedere anche  
 [CHECKPOINT &#40;Transact-SQL&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md)   
 [sp_lock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-lock-transact-sql.md)   
 [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)   
 [Applicazione sqlservr](../../tools/sqlservr-application.md)   
 [Avviare, arrestare, sospendere, riprendere, riavviare il motore di database, SQL Server Agent o SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)  
  
  
