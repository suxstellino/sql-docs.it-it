---
description: Database model
title: Database model | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- template databases [SQL Server]
- model database [SQL Server], about model databases
- model database [SQL Server]
ms.assetid: 4e4f739b-fd27-4dce-8be6-3d808040d8d7
author: stevestein
ms.author: sstein
ms.openlocfilehash: 9ef6744fa152bfb5c66cc82ba71de62bc5bfb1a9
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421973"
---
# <a name="model-database"></a>Database model
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
   Il database **model** viene usato come modello per tutti i database creati in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Poiché il database **tempdb** viene creato ogni volta che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene avviato, il database **model** deve essere sempre presente in un sistema [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . L'intero contenuto del database **model** , incluse le opzioni del database, viene copiato nel nuovo database. Alcune impostazioni del database **model** vengono inoltre usate per la creazione di un nuovo database **tempdb** all'avvio, pertanto in un sistema **il database** model [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve essere sempre presente.  
  
 I database utente appena creati utilizzano lo stesso [modello di recupero](../../relational-databases/backup-restore/recovery-models-sql-server.md) del database model. La stringa predefinita è configurabile dall'utente. Per informazioni sull'attuale modello di recupero, vedere [Visualizzazione o modifica del modello di recupero di un database &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md).  
  
> [!IMPORTANT]  
>  Se si modifica il database **modello** con le informazioni sul modello specifiche dell'utente, è consigliabile eseguire il backup del **modello**. Per altre informazioni, vedere [Backup e ripristino di Database di sistema &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md).  
  
## <a name="model-usage"></a>Utilizzo del database model  
 Quando viene eseguita un'istruzione CREATE DATABASE, la prima parte del database viene creata copiando i contenuti del database **model** . La parte restante del nuovo database viene riempita con pagine vuote.  
  
 Se si modifica il database **model** , le modifiche vengono ereditate da tutti i database creati successivamente. È ad esempio possibile impostare autorizzazioni o opzioni di database oppure aggiungere oggetti, ad esempio tabelle, funzioni o stored procedure. Le proprietà dei file del database **model** rappresentano un'eccezione e vengono ignorate, eccetto le dimensioni iniziali del file di dati. La dimensione iniziale predefinita del file di dati e di log del database modello è 8 MB.  
  
## <a name="physical-properties-of-model"></a>Proprietà fisiche del database model  
 Nella tabella seguente sono illustrati i valori di configurazione iniziali dei file di dati e di log del database **model** .  
  
|File|Nome logico|Nome fisico|Aumento di dimensioni del file|  
|----------|------------------|-------------------|-----------------|  
|Dati primari|modeldev|model.mdf|Aumento automatico di 64 MB fino a quando il disco risulta pieno.|  
|File di log|modellog|modellog.ldf|Aumento automatico di 64 MB fino a un massimo di 2 terabyte.|  

Per SQL Server 2014, vedere [Database model](/previous-versions/sql/2014/relational-databases/databases/model-database?view=sql-server-2014&preserve-view=true) per i valori predefiniti di aumento delle dimensioni dei file.  

 Per spostare il database **modello** o i file di log, vedere [Spostare i database di sistema](../../relational-databases/databases/move-system-databases.md).  
  
### <a name="database-options"></a>Opzioni di database  
 Nella tabella seguente vengono elencati i valori predefiniti per ogni opzione di database del database **model** ed è indicato se è possibile modificare le varie opzioni. Per visualizzare le impostazioni correnti di queste opzioni, usare la vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) .  
  
|Opzione di database|Valore predefinito|Modificabile|  
|---------------------|-------------------|---------------------|  
|ALLOW_SNAPSHOT_ISOLATION|OFF|Sì|  
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
|DB_CHAINING|OFF|No|  
|ENCRYPTION|OFF|No|  
|MIXED_PAGE_ALLOCATION|ATTIVA|No|  
|NUMERIC_ROUNDABORT|OFF|Sì|  
|PAGE_VERIFY|CHECKSUM|Sì|  
|PARAMETERIZATION|SEMPLICE|Sì|  
|QUOTED_IDENTIFIER|OFF|Sì|  
|READ_COMMITTED_SNAPSHOT|OFF|Sì|  
|RECOVERY|Dipende dall'edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *|Sì|  
|RECURSIVE_TRIGGERS|OFF|Sì|  
|Opzioni relative a Service Broker|DISABLE_BROKER|No|  
|TRUSTWORTHY|OFF|No|  
  
 *Per verificare l'attuale modello di recupero del database, vedere [Visualizzazione o modifica del modello di recupero di un database &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md) o [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
 Per una descrizione di queste opzioni di database, vedere [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
## <a name="restrictions"></a>Restrizioni  
 Nel database **model** non è possibile eseguire le operazioni seguenti:  
  
- Aggiunta di file o di filegroup.  
- Modifica delle regole di confronto. Le regole di confronto predefinite corrispondono a quelle del server.  
- Modifica del proprietario del database. **model** è di proprietà di **sa**.  
- Eliminazione del database.  
- Eliminazione dell'utente **guest** dal database.  
- Abilitazione dell'acquisizione dei dati delle modifiche.  
- Partecipazione al mirroring del database.  
- Rimozione del filegroup primario, del file di dati primario o del file di log.  
- Ridenominazione del filegroup primario o del database.  
- Impostazione del database su OFFLINE.  
- Impostazione del filegroup primario su READ_ONLY.  
- Creazione di procedure, viste o trigger utilizzando l'opzione WITH ENCRYPTION. La chiave di crittografia è correlata al database in cui viene creato l'oggetto. Gli oggetti crittografati creati nel database **model** possono essere usati solo in **model**.  
  
## <a name="related-content"></a>Contenuto correlato  
- [Database di sistema.](../../relational-databases/databases/system-databases.md)  
- [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
- [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
- [Spostare file del database](../../relational-databases/databases/move-database-files.md)  
  
  
