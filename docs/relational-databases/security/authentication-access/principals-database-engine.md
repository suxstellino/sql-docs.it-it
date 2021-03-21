---
title: Entità (motore di database) | Microsoft Docs
description: Informazioni sulle entità di sicurezza nel motore di database che possono richiedere risorse di SQL Server. Sono presenti entità a livello di SQL Server e a livello di database.
ms.custom: ''
ms.date: 01/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.roleproperties.selectroll.f1
- sql13.swb.databaseuser.permissions.user.f1--May use common.permissions
helpviewer_keywords:
- certificates [SQL Server], principals
- roles [SQL Server], principals
- permissions [SQL Server], principals
- '##MS_SQLAuthenticatorCertificate##'
- principals [SQL Server]
- '##MS_SQLResourceSigningCertificate##'
- groups [SQL Server], principals
- '##MS_AgentSigningCertificate##'
- authentication [SQL Server], principals
- schemas [SQL Server], principals
- principals [SQL Server], about principals
- security [SQL Server], principals
- users [SQL Server], principals
- '##MS_SQLReplicationSigningCertificate##'
ms.assetid: 3f7adbf7-6e40-4396-a8ca-71cbb843b5c2
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9dc2478bee6ef6eafe5e0fe5d830325311bbdd6a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750411"
---
# <a name="principals-database-engine"></a>Entità (Motore di database)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Le *entità* possono richiedere risorse di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Analogamente ad altri componenti del modello di autorizzazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , le entità possono essere organizzate in una gerarchia. L'ambito di influenza di un'entità dipende dall'ambito della definizione dell'entità: Windows, server, database e se l'entità è indivisibile o una raccolta. Un account di accesso di Windows è un esempio di entità indivisibile mentre un gruppo di Windows è un esempio di entità costituita da una raccolta. Ogni entità dispone di un ID di sicurezza (SID). Questo argomento si applica a tutte le versioni di SQL Server, ma esistono alcune restrizioni relative alle entità di livello di server nel database SQL o in Azure Synapse Analytics. 
  
## <a name="sql-server-level-principals"></a>Entità a livello di SQL Server  
  
- Accesso per l'autenticazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]   
- Account di accesso con autenticazione di Windows per un utente di Windows  
- Account di accesso con autenticazione di Windows per un gruppo di Windows   
- Account di accesso con autenticazione di Azure Active Directory per un utente AD
- Account di accesso con autenticazione di Azure Active Directory per un gruppo AD
- Ruolo del server  
  
## <a name="database-level-principals"></a>Entità a livello di database
  
- Utente del database (sono disponibili 12 tipi di utenti. Per altre informazioni, vedere [CREATE USER](../../../t-sql/statements/create-user-transact-sql.md)).
- Ruolo del database
- Ruolo applicazione
  
## <a name="sa-login"></a>sa Login  
 L'account di accesso `sa` di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] è un'entità a livello di server. Per impostazione predefinita, questo account viene creato durante l'installazione di un'istanza. A partire da [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], il database predefinito di sa è il database master. diversamente da quanto consentito nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. L'account di accesso `sa` è un membro del ruolo predefinito a livello di server `sysadmin`. L'account di accesso `sa` dispone di tutte le autorizzazioni per il server e non può essere limitato. L'account di accesso `sa` non può essere eliminato, ma può essere disabilitato in modo da impedirne l'uso.

## <a name="dbo-user-and-dbo-schema"></a>Utente dbo e schema dbo

L'utente `dbo` è un'entità utente speciale in ogni database. Tutti gli amministratori di SQL Server, i membri del ruolo predefinito del server `sysadmin`, l'account di accesso `sa` e i proprietari del database accedono al database come utenti `dbo`. L'utente `dbo` dispone di tutte le autorizzazioni nel database e non può essere limitato o eliminato. `dbo` rappresenta il proprietario del database, ma l'account utente `dbo` non equivale al ruolo predefinito del database `db_owner`, mentre il ruolo predefinito del database `db_owner` non equivale all'account utente registrato come proprietario del database.     
L'utente `dbo` è proprietario dello schema `dbo`. Lo schema `dbo` è lo schema predefinito per tutti gli utenti, a meno che non venga specificato uno schema diverso.  Lo schema `dbo` non può essere eliminato.
  
## <a name="public-server-role-and-database-role"></a>Ruolo del server public e ruolo del database  
Ogni account di accesso appartiene al ruolo predefinito del server `public` e ogni utente del database appartiene al ruolo del database `public`. Quando a un account di accesso o a un utente non sono state concesse o negate autorizzazioni specifiche per un'entità a protezione diretta, l'account di accesso o l'utente eredita le autorizzazioni concesse al ruolo public su tale entità a protezione diretta. Il ruolo predefinito del server `public` e il ruolo predefinito del database `public` non possono essere eliminati. Tuttavia è possibile revocare le autorizzazioni dei ruoli `public`. Esistono numerose autorizzazioni assegnate per impostazione predefinita ai ruoli `public`. La maggior parte di queste autorizzazioni è necessaria per le operazioni di routine nel database, ovvero quelle operazioni che tutti gli utenti devono essere in grado di eseguire. Prestare attenzione durante la revoca delle autorizzazioni dell'account di accesso public o dell'utente perché questa operazione ha ripercussioni su tutti gli account di accesso e su tutti gli utenti. In genere non è consigliabile negare le autorizzazioni all'account di accesso public perché l'istruzione deny esegue l'override di qualsiasi istruzione grant che è possibile eseguire sui singoli utenti. 
  
## <a name="information_schema-and-sys-users-and-schemas"></a>INFORMATION_SCHEMA e utenti e schemi sys 
 Ogni database include due entità visualizzate come utenti nelle viste del catalogo: `INFORMATION_SCHEMA` e `sys`. Queste entità sono richieste internamente dal motore di database. Pertanto non possono essere modificate o eliminate.  
  
## <a name="certificate-based-sql-server-logins"></a>Account di accesso basati su certificati di SQL Server  
 Le entità del server i cui nomi sono racchiusi tra due simboli di cancelletto (##) sono solo per uso interno di sistema. Al momento dell'installazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vengono create le seguenti entità che non devono essere eliminate.  
  
-   \##MS_SQLResourceSigningCertificate##    
-   \##MS_SQLReplicationSigningCertificate##    
-   \##MS_SQLAuthenticatorCertificate##    
-   \##MS_AgentSigningCertificate##   
-   \##MS_PolicyEventProcessingLogin##   
-   \##MS_PolicySigningCertificate##   
-   \##MS_PolicyTsqlExecutionLogin##   
 
 Questi account principali non hanno password che possono essere modificate dagli amministratori poiché si basano sui certificati rilasciati a Microsoft.
  
## <a name="the-guest-user"></a>L'utente Guest  
 Ogni database include un `guest`. Le autorizzazioni garantite all'utente `guest` sono ereditate dagli utenti che hanno accesso al database ma non dispongono di un account utente nel database. Non è possibile eliminare l'utente `guest`, ma è possibile disabilitarlo revocando la relativa autorizzazione CONNECT. L'autorizzazione CONNECT può essere revocata eseguendo `REVOKE CONNECT FROM GUEST;` all'interno di qualsiasi database diverso da `master` o `tempdb`.  
  
  
## <a name="related-tasks"></a>Attività correlate  
 Per informazioni sulla progettazione di un sistema di autorizzazioni, vedere [Introduzione alle autorizzazioni del motore di database](../../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).  
  
 In questa sezione della documentazione online di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , sono inclusi i seguenti argomenti:  
  
-   [Procedure per la gestione di account di accesso, utenti e schemi](./create-a-login.md)  
  
-   [Ruoli a livello di server](../../../relational-databases/security/authentication-access/server-level-roles.md)  
  
-   [Ruoli a livello di database](../../../relational-databases/security/authentication-access/database-level-roles.md)  
  
-   [Ruoli applicazione](../../../relational-databases/security/authentication-access/application-roles.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Sicurezza di SQL Server](../../../relational-databases/security/securing-sql-server.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sys.sql_logins &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-sql-logins-transact-sql.md)   
 [sys.database_role_members &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [Ruoli a livello di server](../../../relational-databases/security/authentication-access/server-level-roles.md)   
 [Ruoli a livello di database](../../../relational-databases/security/authentication-access/database-level-roles.md)  
  
