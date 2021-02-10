---
title: Ruoli a livello di server | Microsoft Docs
description: SQL Server specifica ruoli a livello di server. Queste entità di sicurezza raggruppano altre entità per gestire le autorizzazioni a livello di server.
ms.custom: ''
ms.date: 05/24/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.Security.NT_AUTHORITY.SYSTEM
- sql13.Security.BUILTIN.administrators
helpviewer_keywords:
- roles [SQL Server], server-level
- principals [SQL Server], server-level
- CONTROL SERVER permission
- fixed server roles [SQL Server]
- credentials [SQL Server], roles
- sysadmin fixed server role
- server-level roles [SQL Server]
- authentication [SQL Server], roles
ms.assetid: 7adf2ad7-015d-4cbe-9e29-abaefd779008
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2e7e36e4e229cea43c1a144d2bd0424657d2b164
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067572"
---
# <a name="server-level-roles"></a>Ruoli a livello di server
[!INCLUDE[appliesto-ss-xxxx-xxxx-pdw-md](../../../includes/appliesto-ss-xxxx-xxxx-pdw-md.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fornisce ruoli a livello di server per semplificare la gestione delle autorizzazioni in un server. Questi ruoli sono entità di sicurezza che raggruppano altre entità. L'ambito delle autorizzazioni dei ruoli a livello di server è l'intero server. I *ruoli* equivalgono ai *gruppi* nel sistema operativo Windows.  
  
 I ruoli predefiniti del server vengono forniti per motivi di praticità e compatibilità con le versioni precedenti. Laddove possibile, assegnare autorizzazioni più specifiche.  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fornisce nove ruoli predefiniti del server. Le autorizzazioni concesse ai ruoli predefiniti del server (ad eccezione di **pubblico**) non possono essere modificate. A partire da [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], è possibile creare ruoli del server definiti dall'utente e aggiungere autorizzazioni a livello di server a tali ruoli.  
  
 È possibile aggiungere entità di livello server, ad esempio account di accesso di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], account di Windows e gruppi di Windows, nei ruoli a livello di server. Tutti i membri di un ruolo predefinito del server possono aggiungere altri account di accesso allo stesso ruolo. I membri dei ruoli del server definiti dall'utente non possono aggiungere altre entità del server al ruolo.  
> [!NOTE]
>  Le autorizzazioni a livello di server non sono disponibili nel database SQL o in Azure Synapse Analytics. Per altre informazioni sul database SQL, vedere [Controllo e concessione dell'accesso al database](/azure/sql-database/sql-database-manage-logins).
  
## <a name="fixed-server-level-roles"></a>Ruoli predefiniti a livello di server  
 Nella tabella seguente vengono illustrati i ruoli predefiniti a livello di server e le relative funzionalità.  
  
|Ruolo predefinito a livello di server|Descrizione|  
|------------------------------|-----------------|  
|**sysadmin**|I membri del ruolo predefinito del server **sysadmin** possono eseguire qualsiasi attività nel server.|  
|**serveradmin**|I membri del ruolo predefinito del server **serveradmin** sono autorizzati a modificare le opzioni di configurazione a livello di server e ad arrestare il server.|  
|**securityadmin**|I membri del ruolo predefinito del server **securityadmin** gestiscono gli account di accesso e le relative proprietà. Possono `GRANT`, `DENY`, e `REVOKE` le autorizzazioni a livello di server. Inoltre, possono `GRANT`, `DENY`, e `REVOKE` le autorizzazioni a livello di database se hanno accesso a un database. Questi membri sono inoltre autorizzati a reimpostare le password per gli account di accesso di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .<br /><br /> **IMPORTANTE:** la possibilità di concedere l'accesso al [!INCLUDE[ssDE](../../../includes/ssde-md.md)] e di configurare autorizzazioni utente consente all'amministratore responsabile della sicurezza di assegnare la maggior parte delle autorizzazioni server. Il ruolo **securityadmin** deve essere considerato equivalente al ruolo **sysadmin** .|  
|**processadmin**|I membri del ruolo predefinito del server **processadmin** sono autorizzati a terminare processi in esecuzione in un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|**setupadmin**|I membri del ruolo predefinito del server **setupadmin** sono autorizzati ad aggiungere e rimuovere server collegati usando istruzioni [!INCLUDE[tsql](../../../includes/tsql-md.md)]. L'appartenenza a **sysadmin** è necessaria per l'uso di [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)].|  
|**bulkadmin**|I membri del ruolo predefinito del server **bulkadmin** sono autorizzati a eseguire l'istruzione `BULK INSERT`.|  
|**diskadmin**|Il ruolo predefinito del server **diskadmin** consente di gestire i file su disco.|  
|**dbcreator**|I membri del ruolo predefinito del server **dbcreator** sono autorizzati a creare, modificare, eliminare e ripristinare qualsiasi database.|  
|**pubblico**|Ogni accesso [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] appartiene al ruolo del server **pubblico**. Quando a un'entità del server non sono state concesse o sono state negate autorizzazioni specifiche per un oggetto a protezione diretta, l'utente eredita le autorizzazioni concesse a public su tale oggetto. Assegnare le autorizzazioni public per un oggetto solo quando si desidera che l'oggetto sia disponibile a tutti gli utenti. Non è possibile modificare l'appartenenza a public.<br /><br /> **Nota:** il ruolo **pubblico** viene implementato in modo diverso rispetto agli altri ruoli e le autorizzazioni possono essere concesse, negate o revocate dai ruoli predefiniti del server pubblico.|  
  
> [!IMPORTANT] 
> La maggior parte delle autorizzazioni fornite dai seguenti ruoli del server non è applicabile a Synapse SQL: **processadmin**, **serveradmin**, **setupadmin** e **diskadmin**.
  
## <a name="permissions-of-fixed-server-roles"></a>Autorizzazioni dei ruoli predefiniti del server  
 A ogni ruolo predefinito del server vengono assegnate autorizzazioni specifiche. La figura seguente mostra le autorizzazioni assegnate ai ruoli del server.   
![fixed_server_role_permissions](../../../relational-databases/security/authentication-access/media/permissions-of-server-roles.png)   
  
> [!IMPORTANT]  
>  L'autorizzazione **CONTROL SERVER** è simile ma non identica al ruolo predefinito del server **sysadmin** . Le autorizzazioni non implicano le appartenenze ai ruoli e le appartenenze ai ruoli non concedono autorizzazioni. Ad esempio, **CONTROL SERVER** non implica o l'appartenenza al ruolo predefinito del server **sysadmin**. Talvolta, tuttavia, è possibile equiparare ruoli e autorizzazioni equivalenti. La maggior parte dei comandi **DBCC** e molte stored procedure di sistema richiedono l'appartenenza al ruolo predefinito del server **sysadmin**. Per un elenco delle 171 stored procedure di sistema che richiedono l'appartenenza a **sysadmin** , vedere il post di blog di Andreas Wolter sulle [avvertenze relative ad autorizzazioni, procedure di sistema, DBCC, creazione automatica dello schema ed escalation dei privilegi di CONTROL SERVER e sysadmin/sa](http://andreas-wolter.com/en/control-server-vs-sysadmin-sa/).  
  
## <a name="server-level-permissions"></a>Autorizzazioni a livello di server  
 Ai ruoli del server definiti dall'utente è possibile aggiungere solo autorizzazioni a livello di server. Per elencare le autorizzazioni a livello di server, eseguire la seguente istruzione. Di seguito sono elencate le autorizzazioni a livello di server:  
  
```  
SELECT * FROM sys.fn_builtin_permissions('SERVER') ORDER BY permission_name;  
```  
  
 Per altre informazioni sulle autorizzazioni, vedere [Autorizzazioni &#40;Motore di database&#41;](../../../relational-databases/security/permissions-database-engine.md) e [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md).  
  
## <a name="working-with-server-level-roles"></a>Lavorare con i ruoli a livello di server.  
 Nella tabella seguente vengono illustrati i comandi, le viste e le funzioni che consentono di utilizzare ruoli a livello di server.  
  
|Funzionalità|Type|Descrizione|  
|-------------|----------|-----------------|  
|[sp_helpsrvrole &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpsrvrole-transact-sql.md)|Metadati|Restituisce un elenco di ruoli a livello di server.|  
|[sp_helpsrvrolemember &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpsrvrolemember-transact-sql.md)|Metadati|Restituisce informazioni sui membri di un ruolo a livello di server.|  
|[sp_srvrolepermission &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-srvrolepermission-transact-sql.md)|Metadati|Visualizza le autorizzazioni di un ruolo a livello di server.|  
|[IS_SRVROLEMEMBER &#40;Transact-SQL&#41;](../../../t-sql/functions/is-srvrolemember-transact-sql.md)|Metadati|Indica se un account di accesso di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] è un membro del ruolo a livello di server specificato.|  
|[sys.server_role_members &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)|Metadati|Restituisce una riga per ogni membro di ogni ruolo a livello di server.|  
|[sp_addsrvrolemember &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md)|Comando|Aggiunge un account di accesso come membro di un ruolo a livello di server. Operazione deprecata. Usare [ALTER SERVER ROLE](../../../t-sql/statements/alter-server-role-transact-sql.md) .|  
|[sp_dropsrvrolemember &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-dropsrvrolemember-transact-sql.md)|Comando|Rimuove un account di accesso di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o un utente o un gruppo di Windows da un ruolo a livello di server. Operazione deprecata. Usare [ALTER SERVER ROLE](../../../t-sql/statements/alter-server-role-transact-sql.md) .|  
|[CREATE SERVER ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-server-role-transact-sql.md)|Comando|Crea un ruolo del server definito dall'utente.|  
|[ALTER SERVER ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-server-role-transact-sql.md)|Comando|Modifica l'appartenenza di un ruolo del server o il nome di un ruolo del server definito dall'utente.|  
|[DROP SERVER ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-server-role-transact-sql.md)|Comando|Rimuove un ruolo del server definito dall'utente.|  
|[IS_SRVROLEMEMBER &#40;Transact-SQL&#41;](../../../t-sql/functions/is-srvrolemember-transact-sql.md)|Funzione|Determina l'appartenenza del ruolo del server.|  
  
## <a name="see-also"></a>Vedere anche  
 [Ruoli a livello di database](../../../relational-databases/security/authentication-access/database-level-roles.md)   
 [Viste del catalogo relative alla sicurezza &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Funzioni di sicurezza &#40;Transact-SQL&#41;](../../../t-sql/functions/security-functions-transact-sql.md)   
 [Sicurezza di SQL Server](../../../relational-databases/security/securing-sql-server.md)   
 [Autorizzazioni per entità server GRANT &#40;Transact-SQL&#41;](../../../t-sql/statements/grant-server-principal-permissions-transact-sql.md)   
 [Autorizzazioni per entità server REVOKE &#40;Transact-SQL&#41;](../../../t-sql/statements/revoke-server-principal-permissions-transact-sql.md)   
 [Autorizzazioni per entità server DENY &#40;Transact-SQL&#41;](../../../t-sql/statements/deny-server-principal-permissions-transact-sql.md)   
 [Creazione di un ruolo del server](../../../relational-databases/security/authentication-access/create-a-server-role.md)  
  
