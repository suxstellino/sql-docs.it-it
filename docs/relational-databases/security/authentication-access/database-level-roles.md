---
title: Ruoli a livello di database | Microsoft Docs
description: SQL Server offre diversi ruoli che sono entità di sicurezza che raggruppano altre entità per gestire le autorizzazioni nei database.
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, azure-synapse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.roleproperties.database.f1
- sql13.swb.roleproperties.object.f1
- SQL13.SWB.DBROLEPROPERTIES.GENERAL.F1
- sql13.swb.roleproperties.general.f1
helpviewer_keywords:
- db_denydatareader role
- users [SQL Server], database roles
- database-level roles [SQL Server]
- db_denydatawriter role
- roles [SQL Server], database
- principals [SQL Server], database-level
- db_backupoperator role
- credentials [SQL Server], roles
- db_accessadmin role
- schemas [SQL Server], roles
- permissions [SQL Server], roles
- database roles [SQL Server], listed
- db_datareader role
- db_ddladmin role
- db_datawriter role
- db_securityadmin role
- db_owner role
- database roles [SQL Server]
- fixed database roles [SQL Server]
- authentication [SQL Server], roles
- groups [SQL Server], roles
ms.assetid: 7f3fa5f6-6b50-43bb-9047-1544ade55e39
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 86560767d969ef5ee7da97c989da7558421f0302
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344409"
---
# <a name="database-level-roles"></a>Ruoli a livello di database

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Per una facile gestione delle autorizzazioni dei database, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fornisce diversi *ruoli* rappresentanti entità di sicurezza all'interno delle quali sono raggruppate altre entità. I ruoli sono analoghi ai ***gruppi*** nel sistema operativo [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows. L'ambito delle autorizzazioni dei ruoli a livello di database è l'intero database.  

Per aggiungere e rimuovere utenti a un ruolo del database, usare le opzioni `ADD MEMBER` e `DROP MEMBER` dell'istruzione [ALTER ROLE](../../../t-sql/statements/alter-role-transact-sql.md) . [!INCLUDE[ssPDW_md](../../../includes/sspdw-md.md)] e Azure Synapse non supportano l'uso di `ALTER ROLE`. Usare invece le stored procedure [sp_addrolemember](../../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md) e [sp_droprolemember](../../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md) .
  
 Esistono due tipi di ruoli a livello di database: i *ruoli predefiniti del database* , che sono predefiniti nel database, e i *ruoli del database definiti dall'utente* , che possono essere creati.  
  
 I ruoli predefiniti del database vengono definiti a livello di database e sono presenti in ogni database. I membri del ruolo del database **db_owner** possono gestire l'appartenenza ai ruoli predefiniti del database. Nel database msdb sono presenti anche alcuni ruoli predefiniti del database per scopi specifici.  
  
 È possibile aggiungere qualsiasi account del database e altri ruoli [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ai ruoli a livello di database.
  
> [!TIP]  
>  Evitare di aggiungere ruoli del database definiti dall'utente come membri dei ruoli predefiniti, poiché in tal modo si potrebbe provocare un'imprevista intensificazione dei privilegi.  

Le autorizzazioni dei ruoli del database definiti dall'utente possono essere personalizzate usando le istruzioni GRANT, DENY e REVOKE. Per altre informazioni, vedere [Autorizzazioni (motore di database)](../../../relational-databases/security/permissions-database-engine.md).

Per un elenco di tutte le autorizzazioni, vedere il poster [Autorizzazioni del motore di database](https://aka.ms/sql-permissions-poster) . Non è possibile concedere autorizzazioni a livello di server ai ruoli del database. Gli account di accesso e altre entità a livello di server (come i ruoli del server) non possono essere aggiunti ai ruoli del database. Per sicurezza a livello di server in [!INCLUDE[ssNoVersion_md](../../../includes/ssnoversion-md.md)], usare invece i [ruoli del server](../../../relational-databases/security/authentication-access/server-level-roles.md) . Non è possibile concedere autorizzazioni a livello di server tramite ruoli in [!INCLUDE[ssSDS_md](../../../includes/sssds-md.md)] e Azure Synapse.

## <a name="fixed-database-roles"></a>ruoli predefiniti del database
  
 La tabella seguente contiene i ruoli predefiniti del database e le rispettive caratteristiche. Questi ruoli esistono in tutti i database. Fatta eccezione per il ruolo del database **pubblico**, non è possibile modificare le autorizzazioni concesse ai ruoli predefiniti del database.   
  
|Nome del ruolo predefinito del database|Descrizione|  
|-------------------------------|-----------------|  
|**db_owner**|I membri del ruolo predefinito del database **db_owner** possono eseguire tutte le attività di configurazione e di manutenzione sul database e anche eliminare il database in [!INCLUDE[ssNoVersion_md](../../../includes/ssnoversion-md.md)]. In [!INCLUDE[ssSDS_md](../../../includes/sssds-md.md)] e Azure Synapse alcune attività di manutenzione richiedono autorizzazioni a livello di server e non possono essere eseguite da ruoli **db_owners**.|  
|**db_securityadmin**|I membri del ruolo predefinito del database **db_securityadmin** possono modificare le appartenenze al ruolo solo per i ruoli personalizzati e gestire le autorizzazioni. I membri di questo ruolo possono potenzialmente elevare i propri privilegi ed è consigliabile monitorarne le azioni.|  
|**db_accessadmin**|I membri del ruolo predefinito del database **db_accessadmin** possono aggiungere o rimuovere le autorizzazioni di accesso al database per gli account di accesso di Windows, i gruppi di Windows e gli account di accesso di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
|**db_backupoperator**|I membri del ruolo predefinito del database **db_backupoperator** possono eseguire il backup del database.|  
|**db_ddladmin**|I membri del ruolo predefinito del database **db_ddladmin** possono eseguire qualsiasi comando DDL (Data Definition Language) in un database.|  
|**db_datawriter**|I membri del ruolo predefinito del database **db_datawriter** possono aggiungere, eliminare o modificare i dati di tutte le tabelle utente.|  
|**db_datareader**|Membri del ruolo predefinito del database **db_datareader** possono leggere tutti i dati di tutte le tabelle utente.|  
|**db_denydatawriter**|I membri del ruolo predefinito del database **db_denydatawriter** non possono aggiungere, modificare o eliminare dati delle tabelle utente contenute in un database.|  
|**db_denydatareader**|I membri del ruolo predefinito del database **db_denydatareader** non possono leggere i dati delle tabelle utente contenute in un database.|  

Non è possibile modificare le autorizzazioni concesse ai ruoli predefiniti del database. La figura seguente mostra le autorizzazioni assegnate ai ruoli predefiniti del database:

![fixed_database_role_permissions](../../../relational-databases/security/authentication-access/media/permissions-of-database-roles.png)

## <a name="special-roles-for-sssds_md-and-azure-synapse"></a>Ruoli speciali per [!INCLUDE[ssSDS_md](../../../includes/sssds-md.md)] e Azure Synapse

Questi ruoli del database si trovano solo nel database master virtuale. Le autorizzazioni di questi ruoli sono limitate alle azioni eseguite nel database master. Solo gli utenti di database nel database master possono essere aggiunti a questi ruoli. Gli account di accesso non possono essere aggiunti a questi ruoli, ma è possibile creare utenti in base agli account di accesso e quindi aggiungere questi utenti ai ruoli. Anche gli utenti di database indipendenti nel database master possono essere aggiunti a questi ruoli. Gli utenti di database indipendenti aggiunti al ruolo **dbmanager** non possono essere tuttavia usati per creare nuovi database.

|Nome del ruolo|Descrizione|  
|--------------------|-----------------|
|**dbmanager** | Può creare ed eliminare database. Un membro del ruolo dbmanager che crea un database diventa il proprietario del database e questo permette all'utente di connettersi al database come utente dbo. L'utente dbo ha tutte le autorizzazioni database nel database. I membri del ruolo dbmanager non hanno necessariamente l'autorizzazione necessaria per accedere ai database di cui non sono proprietari.|
|**loginmanager** | Può creare ed eliminare account di accesso nel database master virtuale.|

> [!NOTE]
> L'entità di livello di server e l'amministratore di Azure Active Directory (se configurato) hanno tutte le autorizzazioni in [!INCLUDE[ssSDS_md](../../../includes/sssds-md.md)] e Azure Synapse senza dover essere membri di alcun ruolo. Per altre informazioni, vedere [Autenticazione e autorizzazione del database SQL: concessione dell'accesso](/azure/azure-sql/database/logins-create-manage). 

Alcuni ruoli del database non sono applicabili a SQL di Azure o a Synapse SQL:
- **db_backupoperator** non è applicabile nel database SQL di Azure (istanza non gestita) e nel pool serverless di Synapse SQL perché i comandi T-SQL di backup e ripristino non sono disponibili.
- **db_datawriter** e **db_denydatawriter** non sono applicabili a Synapse SQL serverless perché legge solo i dati esterni.
  
## <a name="msdb-roles"></a>Ruoli msdb  
 Il database msdb contiene ruoli specifici per uno scopo illustrati nella tabella seguente.  
  
|Nome del ruolo in msdb|Descrizione|  
|--------------------|-----------------|  
|**db_ssisadmin**<br /><br /> **db_ssisoperator**<br /><br /> **db_ssisltduser**|I membri di tali ruoli del database possono amministrare e utilizzare [!INCLUDE[ssIS](../../../includes/ssis-md.md)]. Le istanze di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] aggiornate da una versione precedente potrebbero contenere una versione precedente del ruolo che era stata denominata usando Data Transformation Services (DTS) anziché [!INCLUDE[ssIS](../../../includes/ssis-md.md)]. Per altre informazioni, vedere [Ruoli Integration Services &#40;servizio SSIS&#41;](../../../integration-services/security/integration-services-roles-ssis-service.md).|  
|**dc_admin**<br /><br /> **dc_operator**<br /><br /> **dc_proxy**|I membri di tali ruoli del database possono amministrare e utilizzare l'agente di raccolta dati. Per altre informazioni, vedere [Data Collection](../../../relational-databases/data-collection/data-collection.md).|  
|**PolicyAdministratorRole**|I membri del ruolo del database **db_ PolicyAdministratorRole** possono eseguire tutte le attività di configurazione e manutenzione su criteri e condizioni della gestione basata su criteri. Per altre informazioni, vedere [Amministrare server usando la gestione basata su criteri](../../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md).|  
|**ServerGroupAdministratorRole**<br /><br /> **ServerGroupReaderRole**|I membri di questi ruoli del database possono amministrare e utilizzare gruppi di server registrati.|  
|**dbm_monitor**|Creato nel database **msdb** quando il primo database viene registrato in Monitoraggio mirroring del database. Il ruolo **dbm_monitor** non include alcun membro fino a quando un amministratore di sistema non provvede all'assegnazione di utenti al ruolo stesso.|  
  
> [!IMPORTANT]  
>  I membri dei ruoli **db_ssisadmin** e **dc_admin** possono essere in grado di elevare i propri privilegi a sysadmin. Questa elevazione dei privilegi può verificarsi perché tali ruoli possono modificare i pacchetti [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] e i pacchetti [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] possono essere eseguiti da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilizzando il contesto di sicurezza sysadmin di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent. Per impedire questa elevazione dei privilegi durante l'esecuzione di piani di manutenzione, set di raccolta dati e altri pacchetti di [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] , configurare i processi di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent che eseguono pacchetti in modo che usino un account proxy con privilegi limitati o aggiungere solo i membri **sysadmin** ai ruoli **db_ssisadmin** e **dc_admin** .  

## <a name="working-with-database-level-roles"></a>Utilizzo di ruoli a livello di database  
 Nella tabella seguente vengono spiegati i comandi, le viste e le funzioni necessari per l'utilizzo dei ruoli a livello di database.  
  
|Funzionalità|Type|Descrizione|  
|-------------|----------|-----------------|  
|[sp_helpdbfixedrole &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpdbfixedrole-transact-sql.md)|Metadati|Restituisce un elenco dei ruoli predefiniti del database.|  
|[sp_dbfixedrolepermission &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-dbfixedrolepermission-transact-sql.md)|Metadati|Visualizza le autorizzazioni di un ruolo predefinito del database.|  
|[sp_helprole &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)|Metadati|Restituisce informazioni sui ruoli del database corrente.|  
|[sp_helprolemember &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helprolemember-transact-sql.md)|Metadati|Restituisce informazioni sui membri di un ruolo del database corrente.|  
|[sys.database_role_members &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)|Metadati|Restituisce una riga per ogni membro di ogni ruolo del database.|  
|[IS_MEMBER &#40;Transact-SQL&#41;](../../../t-sql/functions/is-member-transact-sql.md)|Metadati|Indica se l'utente corrente è membro del gruppo di Microsoft Windows o del ruolo di database di Microsoft SQL Server specificato.|  
|[CREATE ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-role-transact-sql.md)|Comando|Crea un nuovo ruolo di database nel database corrente.|  
|[ALTER ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-role-transact-sql.md)|Comando|Modifica il nome o l'appartenenza di un ruolo del database.|  
|[DROP ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-role-transact-sql.md)|Comando|Rimuove un ruolo dal database.|  
|[sp_addrole &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addrole-transact-sql.md)|Comando|Crea un nuovo ruolo di database nel database corrente.|  
|[sp_droprole &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-droprole-transact-sql.md)|Comando|Rimuove un ruolo del database dal database corrente.|  
|[sp_addrolemember &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)|Comando|Aggiunge un utente del database, un ruolo del database, un account di accesso di Windows o un gruppo di Windows a un ruolo del database nel database corrente. Tutte le piattaforme ad accezione di [!INCLUDE[ssPDW_md](../../../includes/sspdw-md.md)] e Azure Synapse devono usare invece `ALTER ROLE`.|  
|[sp_droprolemember &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)|Comando|Rimuove un account di sicurezza da un ruolo di SQL Server nel database corrente. Tutte le piattaforme ad accezione di [!INCLUDE[ssPDW_md](../../../includes/sspdw-md.md)] e Azure Synapse devono usare invece `ALTER ROLE`.|
|[GRANT](../../../t-sql/statements/grant-transact-sql.md)| Autorizzazioni | Aggiunge autorizzazioni a un ruolo.
|[DENY](../../../t-sql/statements/deny-transact-sql.md)| Autorizzazioni | Nega un'autorizzazione a un ruolo.
|[REVOKE](../../../t-sql/statements/revoke-transact-sql.md)| Autorizzazioni | Rimuove un'autorizzazione precedentemente concessa o negata.
  
  
## <a name="public-database-role"></a>Ruolo di database public  
 Ogni utente di database appartiene al ruolo di database **public** . Quando a un utente non sono state concesse o sono state negate autorizzazioni specifiche per un oggetto a protezione diretta, l'utente eredita le autorizzazioni concesse a **public** su tale oggetto. Gli utenti di database non possono essere rimossi dal ruolo **public** . 
  
## <a name="related-content"></a>Contenuto correlato  
 [Viste del catalogo relative alla sicurezza &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)  
  
 [Stored procedure di sicurezza &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)  
  
 [Funzioni di sicurezza &#40;Transact-SQL&#41;](../../../t-sql/functions/security-functions-transact-sql.md)  
  
 [Sicurezza di SQL Server](../../../relational-databases/security/securing-sql-server.md)  
  
 [sp_helprotect &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)  
  
