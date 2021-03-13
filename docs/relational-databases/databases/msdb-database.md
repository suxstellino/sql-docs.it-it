---
description: Database msdb
title: Database msdb | Microsoft Docs
ms.custom: ''
ms.date: 11/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, msdb database
- alerts [SQL Server], msdb database
- jobs [SQL Server], msdb database
- msdb database [SQL Server]
ms.assetid: 5032cb2d-65a0-40dd-b569-4dcecdd58ceb
author: stevestein
ms.author: sstein
ms.openlocfilehash: 80ada8fcce7fe1480dca5e22d4a5b8be3e73b617
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421987"
---
# <a name="msdb-database"></a>Database msdb
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Il database **msdb** viene usato dall'agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per la pianificazione di avvisi e processi e da altre funzionalità, ad esempio [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[ssSB](../../includes/sssb-md.md)] e Posta elettronica database.  
  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ad esempio, l'intera cronologia di backup e ripristino online viene gestita in modo automatico nelle tabelle del database **msdb**. Queste informazioni includono il nome della parte che ha eseguito il backup, l'ora del backup e i dispositivi o i file in cui viene archiviato il backup. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] usa queste informazioni per proporre un piano per il ripristino di un database e l'applicazione di qualsiasi backup di log delle transazioni. Vengono inoltre registrati gli eventi di backup di tutti i database che sono stati creati con applicazioni personalizzate o strumenti di terze parti. Se, ad esempio, si usa un'applicazione [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] che chiama oggetti SMO (SQL Server Management Objects) per l'esecuzione di operazioni di backup, l'evento viene registrato nelle tabelle di sistema **msdb**, nel registro applicazioni di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows e nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per facilitare la protezione delle informazioni archiviate in **msdb**, è consigliabile considerare l'inserimento del log delle transazioni di **msdb** in uno spazio di archiviazione a tolleranza d'errore.  
  
 Per impostazione predefinita, **msdb** usa il modello di recupero con registrazione minima. Se si usano le tabelle di [cronologia di backup e ripristino](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md) , è consigliabile usare il modello di recupero per **msdb**. Per altre informazioni, vedere [Modelli di recupero &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md). Si noti che durante l'installazione o l'aggiornamento di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e ogni volta che si usa il file Setup.exe per ricompilare i database di sistema, il modello di recupero di **msdb** viene impostato automaticamente su SIMPLE.  
  
> [!IMPORTANT]  
>  Successivamente a qualsiasi operazione che aggiorna **msdb**, ad esempio per il backup o il ripristino di un database qualsiasi, è consigliabile eseguire il backup **msdb**. Per altre informazioni, vedere [Backup e ripristino di Database di sistema &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md).  
  
## <a name="physical-properties-of-msdb"></a>Proprietà fisiche del database msdb  
 Nella tabella seguente sono illustrati i valori di configurazione iniziali dei file di dati e di log del database **msdb** . Le dimensioni di questi file possono variare leggermente a seconda dell'edizione di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
|File|Nome logico|Nome fisico|Aumento di dimensioni del file|  
|----------|------------------|-------------------|-----------------|  
|Dati primari|MSDBData|MSDBData.mdf|Aumento automatico del 10% fino a quando il disco risulta pieno.|  
|File di log|MSDBLog|MSDBLog.ldf|Aumento automatico del 10% fino a un massimo di 2 terabyte.|  
  
 Per spostare il database **msdb** o i file di log, vedere [Spostare i database di sistema](../../relational-databases/databases/move-system-databases.md).  
  
### <a name="database-options"></a>Opzioni di database  
 Nella tabella seguente vengono elencati i valori predefiniti per ogni opzione di database del database **msdb** ed è indicato se è possibile modificare le varie opzioni. Per visualizzare le impostazioni correnti di queste opzioni, usare la vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) .  
  
|Opzione di database|Valore predefinito|Modificabile|  
|---------------------|-------------------|---------------------|  
|ALLOW_SNAPSHOT_ISOLATION|ATTIVA|No|  
|ANSI_NULL_DEFAULT|OFF|Sì|  
|ANSI_NULLS|OFF|Sì|  
|ANSI_PADDING|OFF|Sì|  
|ANSI_WARNINGS|OFF|Sì|  
|ARITHABORT|OFF|Sì|  
|AUTO_CLOSE|OFF|Sì|  
|AUTO_CREATE_STATISTICS|ON|Sì|  
|AUTO_SHRINK|OFF|Sì|  
|AUTO_UPDATE_STATISTICS|ON|Sì|  
|AUTO_UPDATE_STATISTICS_ASYNC|OFF|Sì|  
|CHANGE_TRACKING|OFF|No|  
|CONCAT_NULL_YIELDS_NULL|OFF|Sì|  
|CURSOR_CLOSE_ON_COMMIT|OFF|Sì|  
|CURSOR_DEFAULT|GLOBAL|Sì|  
|Opzioni relative alla disponibilità del database|ONLINE<br /><br /> MULTI_USER<br /><br /> READ_WRITE|No<br /><br /> Sì<br /><br /> Sì|  
|DATE_CORRELATION_OPTIMIZATION|OFF|Sì|  
|DB_CHAINING|ON|Sì|  
|ENCRYPTION|OFF|No|  
|MIXED_PAGE_ALLOCATION|ATTIVA|No|  
|NUMERIC_ROUNDABORT|OFF|Sì|  
|PAGE_VERIFY|CHECKSUM|Sì|  
|PARAMETERIZATION|SEMPLICE|Sì|  
|QUOTED_IDENTIFIER|OFF|Sì|  
|READ_COMMITTED_SNAPSHOT|OFF|No|  
|RECOVERY|SEMPLICE|Sì|  
|RECURSIVE_TRIGGERS|OFF|Sì|  
|Opzioni relative a Service Broker|ENABLE_BROKER|Sì|  
|TRUSTWORTHY|ATTIVA|Sì|  
  
 Per una descrizione di queste opzioni di database, vedere [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
## <a name="restrictions"></a>Restrizioni  
 Nel database **msdb** non è possibile eseguire le operazioni seguenti:  
  
-   Modifica delle regole di confronto. Le regole di confronto predefinite corrispondono a quelle del server.  
-   Eliminazione del database.  
-   Eliminazione dell'utente **guest** dal database.  
-   Abilitazione dell'acquisizione dei dati delle modifiche.  
-   Partecipazione al mirroring del database.  
-   Rimozione del filegroup primario, del file di dati primario o del file di log.  
-   Ridenominazione del filegroup primario o del database.  
-   Impostazione del database su OFFLINE.  
-   Impostazione del filegroup primario su READ_ONLY.  
  
## <a name="related-content"></a>Contenuto correlato  
- [Database di sistema.](../../relational-databases/databases/system-databases.md)  
- [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
- [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
- [Spostare file del database](../../relational-databases/databases/move-database-files.md)  
- [Posta elettronica database](../../relational-databases/database-mail/database-mail.md)  
- [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  
  
  
