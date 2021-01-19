---
title: Azioni e gruppi di azioni di SQL Server Audit | Microsoft Docs
description: Informazioni sui gruppi di azioni a livello di server, a livello di database e a livello di controllo e sulle singole azioni in SQL Server Audit.
ms.custom: ''
ms.date: 07/13/2020
ms.prod: sql
ms.prod_service: security
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- audit
helpviewer_keywords:
- audit actions [SQL Server]
- audits [SQL Server], groups
- server-level audit actions [SQL Server]
- SQL Server Audit
- audit-level audit actions [SQL Server]
- database-level audit actions [SQL Server]
- audit action groups [SQL Server]
- audits [SQL Server], actions
ms.assetid: b7422911-7524-4bcd-9ab9-e460d5897b3d
author: DavidTrigano
ms.author: datrigan
ms.openlocfilehash: e435d8c94dfdfc8f989875d48440554e04405376
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172493"
---
# <a name="sql-server-audit-action-groups-and-actions"></a>Azioni e gruppi di azioni di SQL Server Audit
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  La funzionalità [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Audit consente di controllare gruppi di eventi ed eventi singoli a livello di server e di database. Per altre informazioni, vedere [SQL Server Audit &#40;Motore di database&#41;](../../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sono costituiti da zero o da più attività di controllo. Tali attività possono essere un gruppo di azioni, ad esempio Server_Object_Change_Group, oppure azioni singole, ad esempio operazioni SELECT da eseguire in una tabella.  
  
> [!NOTE]  
>  Server_Object_Change_Group include le operazioni CREATE, ALTER e DROP per qualsiasi oggetto server (database o endpoint).  
  
 Ai controlli possono essere associate le categorie di azioni seguenti:  
  
-   Azioni a livello di server, che includono operazioni server, ad esempio modifiche relative alla gestione e operazioni di accesso e di disconnessione.  
  
-   Azioni a livello di database, che includono operazioni DML (Data Manipulation Language) e DDL (Data Definition Language).  
  
-   Azioni a livello di controllo, ovvero le azioni del processo di controllo.  
  
 Alcune azioni eseguite sui componenti di controllo di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vengono verificate in modo intrinseco in un controllo specifico. In questi casi gli eventi di controllo si verificano automaticamente poiché l'evento si è verificato nell'oggetto padre.  
  
 Di seguito sono elencate le azioni controllate in modo intrinseco:  
  
-   Modifica stato del controllo del server (impostazione dello stato su ON oppure OFF)  
  
 Di seguito sono elencati gli eventi non controllati in modo intrinseco:  
  
-   CREATE SERVER AUDIT SPECIFICATION  
  
-   ALTER SERVER AUDIT SPECIFICATION  
  
-   DROP SERVER AUDIT SPECIFICATION  
  
-   CREATE DATABASE AUDIT SPECIFICATION  
  
-   ALTER DATABASE AUDIT SPECIFICATION  
  
-   DROP DATABASE AUDIT SPECIFICATION  
  
 Al momento della creazione, tutti i controlli sono disabilitati.  
  
## <a name="server-level-audit-action-groups"></a>Gruppi di azioni di controllo a livello di server  
 I gruppi di azioni di controllo a livello di server sono analoghi alle classi degli eventi di controllo della sicurezza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [SQL Server Event Class Reference](../../../relational-databases/event-classes/sql-server-event-class-reference.md).  
  
 Nella tabella seguente vengono descritti i gruppi di azioni di controllo a livello di server e, dove applicabile, viene specificata la classe di evento di SQL Server equivalente.  
  
|Nome gruppo di azione|Descrizione|  
|-----------------------|-----------------|  
|APPLICATION_ROLE_CHANGE_PASSWORD_GROUP|Questo evento viene generato ogni volta che una password viene modificata per un ruolo applicazione. Equivale a [Audit App Role Change Password Event Class](../../../relational-databases/event-classes/audit-app-role-change-password-event-class.md).|  
|AUDIT_CHANGE_GROUP|Questo evento viene generato ogni volta che un controllo viene creato, modificato o eliminato, nonché ogni volta che la specifica di un controllo viene creata, modificata o eliminata. Qualsiasi modifica al controllo viene verificata nel controllo stesso. Equivale a [Audit Change Audit Event Class](../../../relational-databases/event-classes/audit-change-audit-event-class.md).|  
|BACKUP_RESTORE_GROUP|Questo evento viene generato ogni volta che viene eseguito un comando per il backup o il ripristino. Equivalente alla [classe di evento Audit Backup/Restore](../../../relational-databases/event-classes/audit-backup-and-restore-event-class.md).|  
|BATCH_COMPLETED_GROUP|Questo evento viene generato ogni volta che viene completata l'esecuzione di qualsiasi operazione di gestione di testo, stored procedure o transazione in batch. Viene generato al termine del batch e controlla l'intero testo del batch o della stored procedure, come inviato dal client, incluso il risultato. **Aggiunto in SQL Server 2019.**|  
|BATCH_STARTED_GROUP|Questo evento viene generato ogni volta che viene avviata l'esecuzione di qualsiasi operazione di gestione di testo, stored procedure o transazione in batch. Viene generato prima dell'esecuzione e controlla l'intero testo del batch o della stored procedure, come inviato dal client. **Aggiunto in SQL Server 2019.**|  
|BROKER_LOGIN_GROUP|Questo evento viene generato per segnalare i messaggi di controllo correlati alla sicurezza del trasporto Service Broker. Equivale a [Audit Broker Login Event Class](../../../relational-databases/event-classes/audit-broker-login-event-class.md).|  
|DATABASE_CHANGE_GROUP|Questo evento viene generato quando un database viene creato, modificato o eliminato. viene creato, modificato o eliminato. Equivale a [Audit Database Management Event Class](../../../relational-databases/event-classes/audit-database-management-event-class.md).|  
|DATABASE_LOGOUT_GROUP|Questo evento viene generato quando un utente del database indipendente si disconnette da un database equivalente alla [classe di evento Audit Logout](../../../relational-databases/event-classes/audit-logout-event-class.md).|  
|DATABASE_MIRRORING_LOGIN_GROUP|Questo evento viene generato per segnalare i messaggi di controllo correlati alla sicurezza del trasporto del mirroring del database. Equivale a [Audit Database Mirroring Login Event Class](../../../relational-databases/event-classes/audit-database-mirroring-login-event-class.md).|  
|DATABASE_OBJECT_ACCESS_GROUP|Questo evento viene generato a ogni accesso a oggetti di database, ad esempio tipi di messaggio, assembly e contratti. Questo evento viene generato per qualsiasi accesso a qualsiasi database. Nota: questo può provocare la creazione di record di controllo di dimensioni elevate.<br /><br /> Equivale a [Audit Database Object Access Event Class](../../../relational-databases/event-classes/audit-database-object-access-event-class.md).|  
|DATABASE_OBJECT_CHANGE_GROUP|Questo evento viene generato quando si esegue un'istruzione CREATE, ALTER o DROP in oggetti di database, ad esempio schemi. L'evento viene generato ogni volta che un oggetto di database viene creato, modificato o eliminato. Nota: questo può provocare la creazione di un numero elevato di record di controllo.<br /><br /> Equivale a [Audit Database Object Management Event Class](../../../relational-databases/event-classes/audit-database-object-management-event-class.md).|  
|DATABASE_OBJECT_OWNERSHIP_CHANGE_GROUP|Questo evento viene generato in caso di modifica del proprietario di oggetti nell'ambito del database. Questo evento viene generato in caso di qualsiasi modifica del proprietario di oggetti per qualsiasi database del server. Equivale a [Audit Database Object Take Ownership Event Class](../../../relational-databases/event-classes/audit-database-object-take-ownership-event-class.md).|  
|DATABASE_OBJECT_PERMISSION_CHANGE_GROUP|Questo evento viene generato quando è stata eseguita un'istruzione GRANT, REVOKE o DENY per oggetti di database, ad esempio assembly o schemi. Questo evento viene generato per qualsiasi modifica alle autorizzazioni per gli oggetti per qualsiasi database del server. Equivale a [Audit Database Object GDR Event Class](../../../relational-databases/event-classes/audit-database-object-gdr-event-class.md).|  
|DATABASE_OPERATION_GROUP|Questo evento viene generato quando vengono effettuate operazioni in un database, ad esempio il checkpoint o la sottoscrizione di notifiche di query. nonché in caso di qualsiasi operazione effettuata su qualsiasi database. Equivale a [Audit Database Operation Event Class](../../../relational-databases/event-classes/audit-database-operation-event-class.md).|  
|DATABASE_OWNERSHIP_CHANGE_GROUP|Questo evento viene generato quando si utilizza l'istruzione ALTER AUTHORIZATION per modificare il proprietario di un database e quando vengono controllate le autorizzazioni necessarie a tale scopo. L'evento viene inoltre generato in caso di qualsiasi modifica del proprietario del database per qualsiasi database del server. Equivale a [Audit Change Database Owner Event Class](../../../relational-databases/event-classes/audit-change-database-owner-event-class.md).|  
|DATABASE_PERMISSION_CHANGE_GROUP|Questo evento viene generato ogni volta che una qualsiasi entità di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] emette GRANT, REVOKE o DENY per un'autorizzazione per le istruzioni. Questa situazione si applica ad eventi relativi esclusivamente a database, ad esempio la concessione di autorizzazioni in un database.<br /><br /> Questo evento viene generato per qualsiasi modifica alle autorizzazioni del database (evento GDR) per qualsiasi database del server. Equivale a [Audit Database Scope GDR Event Class](../../../relational-databases/event-classes/audit-database-scope-gdr-event-class.md).|  
|DATABASE_PRINCIPAL_CHANGE_GROUP|Questo evento viene generato quando in un database vengono create, modificate o eliminate entità, quali gli utenti. Equivale a [Audit Database Principal Management Event Class](../../../relational-databases/event-classes/audit-database-principal-management-event-class.md). L'evento equivale anche alla classe di evento Audit Add DB Principal, generata in relazione alle stored procedure deprecate sp_grantdbaccess, sp_revokedbaccess, sp_addPrincipal e sp_dropPrincipal.<br /><br /> Questo evento viene generato ogni volta che un ruolo del database viene aggiunto o rimosso utilizzando le stored procedure sp_addrole e sp_droprole. nonché ogni volta che una qualsiasi entità di database viene creata, modificata o eliminata da un database. Equivale a [Classe di evento Audit Add Role](../../../relational-databases/event-classes/audit-add-role-event-class.md).|  
|DATABASE_PRINCIPAL_IMPERSONATION_GROUP|Questo evento viene generato in presenza di un'operazione di rappresentazione nell'ambito del database, ad esempio EXECUTE AS \<principal> o SETPRINCIPAL. Questo evento viene generato per le rappresentazioni effettuate in qualsiasi database. Equivale a [Audit Database Principal Impersonation Event Class](../../../relational-databases/event-classes/audit-database-principal-impersonation-event-class.md).|  
|DATABASE_ROLE_MEMBER_CHANGE_GROUP|Questo evento viene generato ogni volta che un account di accesso viene aggiunto a un ruolo del database o rimosso. La classe di evento viene generata in relazione alle stored procedure sp_addrolemember, sp_changegroup e sp_droprolemember. Questo evento viene generato per qualsiasi modifica apportata ai membri del ruolo del database in qualsiasi database. Equivale a [Classe di evento Audit Add Member to DB Role](../../../relational-databases/event-classes/audit-add-member-to-db-role-event-class.md).|  
|DBCC_GROUP|Questo evento viene generato ogni volta che un'entità esegue un comando DBCC. Equivale a [Audit DBCC Event Class](../../../relational-databases/event-classes/audit-dbcc-event-class.md).|  
|FAILED_DATABASE_AUTHENTICATION_GROUP|Viene indicato il tentativo fallito di accesso a un database indipendente da parte di un'entità. Gli eventi di questa classe vengono generati da connessioni nuove o riutilizzate da un pool di connessioni. Equivale a [Audit Login Failed Event Class](../../../relational-databases/event-classes/audit-login-failed-event-class.md).|    
|FAILED_LOGIN_GROUP|Indica che un'entità ha tentato di accedere a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e l'operazione ha avuto esito negativo. Gli eventi di questa classe vengono generati da connessioni nuove o riutilizzate da un pool di connessioni. Equivale a [Audit Login Failed Event Class](../../../relational-databases/event-classes/audit-login-failed-event-class.md). Questo controllo non si applica al database SQL di Azure.| 
|FULLTEXT_GROUP|Viene indicata l'occorrenza di un evento full-text. Equivale a [Audit Fulltext Event Class](../../../relational-databases/event-classes/audit-fulltext-event-class.md).|  
|LOGIN_CHANGE_PASSWORD_GROUP|Questo evento viene generato ogni volta che una password di accesso viene modificata tramite l'istruzione ALTER LOGIN o la stored procedure sp_password. Equivale a [Audit Login Change Password Event Class](../../../relational-databases/event-classes/audit-login-change-password-event-class.md).|  
|LOGOUT_GROUP|Viene indicata la disconnessione da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]da parte di un'entità. Gli eventi di questa classe vengono generati da connessioni nuove o riutilizzate da un pool di connessioni. Equivale a [Audit Logout Event Class](../../../relational-databases/event-classes/audit-logout-event-class.md).|  
|SCHEMA_OBJECT_ACCESS_GROUP|Questo evento viene generato ogni volta che nello schema è stata utilizzata un'autorizzazione per gli oggetti. Equivale a [Audit Schema Object Access Event Class](../../../relational-databases/event-classes/audit-schema-object-access-event-class.md).|  
|SCHEMA_OBJECT_CHANGE_GROUP|Questo evento viene generato quando si effettua un'operazione CREATE, ALTER o DROP in uno schema. Equivale a [Audit Schema Object Management Event Class](../../../relational-databases/event-classes/audit-schema-object-management-event-class.md).<br /><br /> Questo evento viene generato negli oggetti dello schema. Equivale a [Audit Object Derived Permission Event Class](../../../relational-databases/event-classes/audit-object-derived-permission-event-class.md).<br /><br /> Questo evento viene generato ogni volta che uno schema di un database viene modificato. Equivale a [Audit Statement Permission Event Class](../../../relational-databases/event-classes/audit-statement-permission-event-class.md).|  
|SCHEMA_OBJECT_OWNERSHIP_CHANGE_GROUP|Questo evento viene generato quando vengono controllate le autorizzazioni per la modifica del proprietario dell'oggetto dello schema, ad esempio una tabella, una procedura o una funzione. Questa situazione si verifica quando si assegna un proprietario a un oggetto tramite l'istruzione ALTER AUTHORIZATION. Questo evento viene generato in caso di qualsiasi modifica del proprietario dello schema per qualsiasi database del server. Equivale a [Audit Schema Object Take Ownership Event Class](../../../relational-databases/event-classes/audit-schema-object-take-ownership-event-class.md).|  
|SCHEMA_OBJECT_PERMISSION_CHANGE_GROUP|Questo evento viene generato ogni volta che un'istruzione GRANT, DENY o REVOKE viene eseguita per un oggetto dello schema. Equivale a [Audit Schema Object GDR Event Class](../../../relational-databases/event-classes/audit-schema-object-gdr-event-class.md).|  
|SERVER_OBJECT_CHANGE_GROUP|Questo evento viene generato per operazioni CREATE, ALTER o DROP negli oggetti server. Equivale a [Audit Server Object Management Event Class](../../../relational-databases/event-classes/audit-server-object-management-event-class.md).|  
|SERVER_OBJECT_OWNERSHIP_CHANGE_GROUP|Questo evento viene generato in caso di modifica del proprietario degli oggetti nell'ambito del server. Equivale a [Audit Server Object Take Ownership Event Class](../../../relational-databases/event-classes/audit-server-object-take-ownership-event-class.md).|  
|SERVER_OBJECT_PERMISSION_CHANGE_GROUP|Questo evento viene generato ogni volta che una qualsiasi entità di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]consente di eseguire un'istruzione GRANT, REVOKE o DENY in relazione a un'autorizzazione per l'oggetto server. Equivale a [Audit Server Object GDR Event Class](../../../relational-databases/event-classes/audit-server-object-gdr-event-class.md).|  
|SERVER_OPERATION_GROUP|Questo evento viene generato quando si utilizzano operazioni di controllo della sicurezza, ad esempio la modifica di impostazioni, risorse, opzioni per l'accesso esterno o autorizzazioni. Equivale a [Audit Server Operation Event Class](../../../relational-databases/event-classes/audit-server-operation-event-class.md).|  
|SERVER_PERMISSION_CHANGE_GROUP|Questo evento viene generato quando viene eseguita un'istruzione GRANT, REVOKE o DENY in relazione alle autorizzazioni nell'ambito del server. Equivale a [Audit Server Scope GDR Event Class](../../../relational-databases/event-classes/audit-server-scope-gdr-event-class.md).|  
|SERVER_PRINCIPAL_CHANGE_GROUP|Questo evento viene generato in caso di creazione, modifica o eliminazione di entità del server. Equivale a [Audit Server Principal Management Event Class](../../../relational-databases/event-classes/audit-server-principal-management-event-class.md).<br /><br /> Questo evento viene generato quando un'entità esegue la stored procedure sp_defaultdb o sp_defaultlanguage o l'istruzione ALTER LOGIN. Equivale a [Audit Addlogin Event Class](../../../relational-databases/event-classes/audit-addlogin-event-class.md).<br /><br /> Questo evento viene generato in relazione alle stored procedure sp_addlogin e sp_droplogin. e a [Audit Login Change Property Event Class](../../../relational-databases/event-classes/audit-login-change-property-event-class.md).<br /><br /> Questo evento viene generato in relazione alla stored procedure sp_grantlogin o sp_revokelogin. Equivale a [Audit Login GDR Event Class](../../../relational-databases/event-classes/audit-login-gdr-event-class.md).|  
|SERVER_PRINCIPAL_IMPERSONATION_GROUP|Questo evento viene generato in presenza di una rappresentazione nell'ambito del server, ad esempio EXECUTE AS \<login>. Equivale a [Audit Server Principal Impersonation Event Class](../../../relational-databases/event-classes/audit-server-principal-impersonation-event-class.md).|  
|SERVER_ROLE_MEMBER_CHANGE_GROUP|Questo evento viene generato ogni volta che un account di accesso viene aggiunto a un ruolo predefinito del server o rimosso. L'evento viene generato in relazione alle stored procedure sp_addsrvrolemember e sp_dropsrvrolemember. Equivale a [Classe di evento Audit Add Login to Server Role](../../../relational-databases/event-classes/audit-add-login-to-server-role-event-class.md).|  
|SERVER_STATE_CHANGE_GROUP|Questo evento viene generato quando lo stato del servizio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] viene modificato. Equivale a [Audit Server Starts and Stops Event Class](../../../relational-databases/event-classes/audit-server-starts-and-stops-event-class.md).|  
|SUCCESSFUL_DATABASE_AUTHENTICATION_GROUP|Viene indicato il corretto accesso a un database indipendente da parte di un'entità.|  
|SUCCESSFUL_LOGIN_GROUP|Viene indicato il corretto accesso a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]da parte di un'entità. Gli eventi di questa classe vengono generati da connessioni nuove o riutilizzate da un pool di connessioni. Equivale a [Audit Login Event Class](../../../relational-databases/event-classes/audit-login-event-class.md).|  
|TRACE_CHANGE_GROUP|Questo evento viene generato per tutte le istruzioni che consentono di controllare l'autorizzazione ALTER TRACE. Equivale a [Audit Server Alter Trace Event Class](../../../relational-databases/event-classes/audit-server-alter-trace-event-class.md).|  
|TRANSACTION_GROUP|Questo evento viene generato per le operazioni BEGIN TRANSACTION, ROLLBACK TRANSACTION e COMMIT TRANSACTION, sia per le chiamate esplicite a tali istruzioni che per le operazioni di transazioni implicite. Questo evento viene generato anche per le operazioni UNDO per singole istruzioni dovute al rollback di una transazione.|  
|USER_CHANGE_PASSWORD_GROUP|Questo evento viene generato ogni volta che la password di un utente del database indipendente viene modificata tramite l'istruzione ALTER USER.|  
|USER_DEFINED_AUDIT_GROUP|Questo gruppo consente di monitorare gli eventi generati usando [sp_audit_write &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-audit-write-transact-sql.md). In genere, i trigger o le stored procedure includono chiamate a **sp_audit_write** per abilitare il controllo di eventi importanti.|  
  
### <a name="considerations"></a>Considerazioni  
 Nei gruppi di azioni a livello di server sono comprese azioni relative a un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Qualsiasi controllo dell'accesso agli oggetti dello schema in un database, ad esempio, viene registrato se il gruppo di azioni appropriato viene aggiunto a una specifica del controllo del server. In una specifica del controllo del database vengono registrati solo gli accessi agli oggetti dello schema del database specifico.  
  
 Le azioni a livello di server non consentono di applicare filtri dettagliati sulle azioni a livello di database. Per implementare l'applicazione di filtri dettagliati sull'azione, è necessario un controllo a livello di database, ad esempio un controllo di azioni SELECT nella tabella relativa ai clienti  per account di accesso nel gruppo relativo ai dipendenti. In una specifica del controllo di un database utente non includere oggetti con ambito server, ad esempio viste di sistema.  

 > [!NOTE]
 > A causa dell'overhead associato all'abilitazione del controllo a livello di transazione, a partire da [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)] SP2 CU3 e [!INCLUDE[ssSQL17](../../../includes/sssql17-md.md)] CU4, il controllo a livello di transazione è disabilitato per impostazione predefinita, a meno che la modalità di conformità ai criteri comuni non sia abilitata.  Se la modalità di conformità ai criteri comuni è disabilitata, sarà comunque possibile aggiungere un'azione da TRANSACTION_GROUP a una specifica di controllo, ma non verranno raccolte azioni di transazione.  A partire da [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)] SP2 CU3 e [!INCLUDE[ssSQL17](../../../includes/sssql17-md.md)] CU4 e versioni successive, se si intende configurare azioni di transazione da TRANSACTION_GROUP, assicurarsi che l'infrastruttura di controllo a livello di transazione sia abilitata procedendo all'abilitazione della modalità di conformità ai criteri comuni.  Tenere presente che in [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)] il controllo a livello di transazione può anche essere disabilitato con il flag di traccia 3427 a partire da SP1 CU2.
  
## <a name="database-level-audit-action-groups"></a>Gruppi di azioni di controllo a livello di database  
 I gruppi di azioni di controllo a livello di database sono analoghi alle classi di eventi di controllo della sicurezza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per ulteriori informazioni sulle classi degli eventi, vedere [SQL Server Event Class Reference](../../../relational-databases/event-classes/sql-server-event-class-reference.md).  
  
 Nella tabella seguente vengono descritti i gruppi di azioni di controllo a livello di database e, dove applicabile, viene specificata la classe di evento di SQL Server equivalente.  
  
|Nome gruppo di azione|Descrizione|  
|-----------------------|-----------------|  
|APPLICATION_ROLE_CHANGE_PASSWORD_GROUP|Questo evento viene generato ogni volta che una password viene modificata per un ruolo applicazione. Equivale a [Audit App Role Change Password Event Class](../../../relational-databases/event-classes/audit-app-role-change-password-event-class.md).|  
|AUDIT_CHANGE_GROUP|Questo evento viene generato ogni volta che un controllo viene creato, modificato o eliminato, nonché ogni volta che la specifica di un controllo viene creata, modificata o eliminata. Qualsiasi modifica al controllo viene verificata nel controllo stesso. Equivale a [Audit Change Audit Event Class](../../../relational-databases/event-classes/audit-change-audit-event-class.md).|  
|BACKUP_RESTORE_GROUP|Questo evento viene generato ogni volta che viene eseguito un comando per il backup o il ripristino. Equivalente alla [classe di evento Audit Backup/Restore](../../../relational-databases/event-classes/audit-backup-and-restore-event-class.md).|  
|BATCH_COMPLETED_GROUP|Questo evento viene generato ogni volta che viene completata l'esecuzione di qualsiasi operazione di gestione di testo, stored procedure o transazione in batch. Viene generato al termine del batch e controlla l'intero testo del batch o della stored procedure, come inviato dal client, incluso il risultato.|  
|BATCH_STARTED_GROUP|Questo evento viene generato ogni volta che viene avviata l'esecuzione di qualsiasi operazione di gestione di testo, stored procedure o transazione in batch. Viene generato prima dell'esecuzione e controlla l'intero testo del batch o della stored procedure, come inviato dal client.|  
|DATABASE_CHANGE_GROUP|Questo evento viene generato quando un database viene creato, modificato o eliminato. Equivale a [Audit Database Management Event Class](../../../relational-databases/event-classes/audit-database-management-event-class.md).|  
|DATABASE_LOGOUT_GROUP|Questo evento viene generato quando un utente del database indipendente si disconnette da un database.|  
|DATABASE_OBJECT_ACCESS_GROUP|Questo evento viene generato ogni volta che viene eseguito l'accesso a oggetti di database, ad esempio certificati e chiavi asimmetriche. Equivale a [Audit Database Object Access Event Class](../../../relational-databases/event-classes/audit-database-object-access-event-class.md).|  
|DATABASE_OBJECT_CHANGE_GROUP|Questo evento viene generato quando si esegue un'istruzione CREATE, ALTER o DROP in oggetti di database, ad esempio schemi. Equivale a [Audit Database Object Management Event Class](../../../relational-databases/event-classes/audit-database-object-management-event-class.md).|  
|DATABASE_OBJECT_OWNERSHIP_CHANGE_GROUP|Questo evento viene generato in caso di modifica del proprietario di oggetti nell'ambito del database. Equivale a [Audit Database Object Take Ownership Event Class](../../../relational-databases/event-classes/audit-database-object-take-ownership-event-class.md).|  
|DATABASE_OBJECT_PERMISSION_CHANGE_GROUP|Questo evento viene generato quando è stata eseguita un'istruzione GRANT, REVOKE o DENY per oggetti di database, ad esempio assembly o schemi. Equivale a [Audit Database Object GDR Event Class](../../../relational-databases/event-classes/audit-database-object-gdr-event-class.md).|  
|DATABASE_OPERATION_GROUP|Questo evento viene generato quando vengono effettuate operazioni in un database, ad esempio il checkpoint o la sottoscrizione di notifiche di query. Equivale a [Audit Database Operation Event Class](../../../relational-databases/event-classes/audit-database-operation-event-class.md).|  
|DATABASE_OWNERSHIP_CHANGE_GROUP|Questo evento viene generato quando si utilizza l'istruzione ALTER AUTHORIZATION per modificare il proprietario di un database e quando vengono controllate le autorizzazioni necessarie a tale scopo. Equivale a [Audit Change Database Owner Event Class](../../../relational-databases/event-classes/audit-change-database-owner-event-class.md).|  
|DATABASE_PERMISSION_CHANGE_GROUP|Questo evento viene generato ogni volta che un qualsiasi utente di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] esegue un'istruzione GRANT, REVOKE o DENY in relazione a un'autorizzazione per eventi relativi esclusivamente a database, ad esempio la concessione di autorizzazioni in un database. Equivale a [Audit Database Scope GDR Event Class](../../../relational-databases/event-classes/audit-database-scope-gdr-event-class.md).|  
|DATABASE_PRINCIPAL_CHANGE_GROUP|Questo evento viene generato quando in un database vengono create, modificate o eliminate entità, quali gli utenti. Equivale a [Audit Database Principal Management Event Class](../../../relational-databases/event-classes/audit-database-principal-management-event-class.md). Equivale anche alla [classe di evento Audit Add DB User](../../../relational-databases/event-classes/audit-add-db-user-event-class.md), generata in relazione alle stored procedure deprecate sp_grantdbaccess, sp_revokedbaccess, sp_adduser e sp_dropuser.<br /><br /> Questo evento viene generato ogni volta che un ruolo del database viene aggiunto o rimosso utilizzando le stored procedure deprecate sp_addrole e sp_droprole. Equivale a [Classe di evento Audit Add Role](../../../relational-databases/event-classes/audit-add-role-event-class.md).|  
|DATABASE_PRINCIPAL_IMPERSONATION_GROUP|Questo evento viene generato in presenza di una rappresentazione nell'ambito del database, ad esempio EXECUTE AS \<user>. Equivale a [Audit Database Principal Impersonation Event Class](../../../relational-databases/event-classes/audit-database-principal-impersonation-event-class.md).|  
|DATABASE_ROLE_MEMBER_CHANGE_GROUP|Questo evento viene generato ogni volta che un account di accesso viene aggiunto a un ruolo del database o rimosso. Questa classe di evento viene usata con le stored procedure sp_addrolemember, sp_changegroup e sp_droprolemember. Equivale alla [classe di evento Audit Add Member to DB Role](../../../relational-databases/event-classes/audit-add-member-to-db-role-event-class.md)|  
|DBCC_GROUP|Questo evento viene generato ogni volta che un'entità esegue un comando DBCC. Equivale a [Audit DBCC Event Class](../../../relational-databases/event-classes/audit-dbcc-event-class.md).|  
|FAILED_DATABASE_AUTHENTICATION_GROUP|Viene indicato il tentativo fallito di accesso a un database indipendente da parte di un'entità. Gli eventi di questa classe vengono generati da connessioni nuove o riutilizzate da un pool di connessioni. Viene generato questo evento.|  
|SCHEMA_OBJECT_ACCESS_GROUP|Questo evento viene generato ogni volta che nello schema è stata utilizzata un'autorizzazione per gli oggetti. Equivale a [Audit Schema Object Access Event Class](../../../relational-databases/event-classes/audit-schema-object-access-event-class.md).|  
|SCHEMA_OBJECT_CHANGE_GROUP|Questo evento viene generato quando si effettua un'operazione CREATE, ALTER o DROP in uno schema. Equivale a [Audit Schema Object Management Event Class](../../../relational-databases/event-classes/audit-schema-object-management-event-class.md).<br /><br /> Questo evento viene generato negli oggetti dello schema. Equivale a [Audit Object Derived Permission Event Class](../../../relational-databases/event-classes/audit-object-derived-permission-event-class.md). e a [Audit Statement Permission Event Class](../../../relational-databases/event-classes/audit-statement-permission-event-class.md).|  
|SCHEMA_OBJECT_OWNERSHIP_CHANGE_GROUP|Questo evento viene generato quando vengono controllate le autorizzazioni per la modifica del proprietario dell'oggetto dello schema, ad esempio una tabella, una procedura o una funzione. Questa situazione si verifica quando si assegna un proprietario a un oggetto tramite l'istruzione ALTER AUTHORIZATION. Equivale a [Audit Schema Object Take Ownership Event Class](../../../relational-databases/event-classes/audit-schema-object-take-ownership-event-class.md).|  
|SCHEMA_OBJECT_PERMISSION_CHANGE_GROUP|Questo evento viene generato ogni volta che viene eseguita un'istruzione GRANT, DENY o REVOKE per un oggetto dello schema. Equivale a [Audit Schema Object GDR Event Class](../../../relational-databases/event-classes/audit-schema-object-gdr-event-class.md).|  
|SUCCESSFUL_DATABASE_AUTHENTICATION_GROUP|Viene indicato il corretto accesso a un database indipendente da parte di un'entità.|  
|USER_CHANGE_PASSWORD_GROUP|Questo evento viene generato ogni volta che la password di un utente del database indipendente viene modificata tramite l'istruzione ALTER USER.|  
|USER_DEFINED_AUDIT_GROUP|Questo gruppo consente di monitorare gli eventi generati usando [sp_audit_write &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-audit-write-transact-sql.md).|  
  
## <a name="database-level-audit-actions"></a>Azioni di controllo a livello di database  
 Le azioni a livello di database supportano il controllo di azioni specifiche direttamente nello schema del database e negli oggetti dello schema, ad esempio tabelle, viste, stored procedure, funzioni, stored procedure estese, code e sinonimi. Tipi, raccolte di XML Schema, database e schemi non sono controllati. Il controllo di oggetti dello schema può essere configurato nello schema o nel database, ovvero tutti gli eventi sugli oggetti dello schema contenuti nello schema o nel database specificato verranno controllati. Nella tabella seguente vengono descritte le azioni di controllo a livello di database.  
  
|Azione|Descrizione|  
|------------|-----------------|  
|SELECT|Questo evento viene generato ogni volta che viene eseguita un'istruzione SELECT.|  
|UPDATE|Questo evento viene generato ogni volta che viene eseguita un'istruzione UPDATE.|  
|INSERT|Questo evento viene generato ogni volta che viene eseguita un'istruzione INSERT.|  
|DELETE|Questo evento viene generato ogni volta che viene eseguita un'istruzione DELETE.|  
|EXECUTE|Questo evento viene generato ogni volta che viene eseguita un'istruzione EXECUTE.|  
|RECEIVE|Questo evento viene generato ogni volta che viene eseguita un'istruzione RECEIVE.|  
|REFERENCES|Questo evento viene generato ogni volta che viene controllata un'autorizzazione REFERENCES.|  
  
### <a name="considerations"></a>Considerazioni  
*  Le azioni di controllo a livello di database non si applicano alle colonne.  
  
*  Quando Query Processor parametrizza la query, il parametro può apparire nel log degli eventi di controllo anziché nei valori di colonna della query. 
 
*  Le istruzioni RPC non vengono registrate. 
  
## <a name="audit-level-audit-action-groups"></a>Gruppi di azioni di controllo a livello di controllo  
 È possibile controllare le azioni anche durante il processo di controllo stesso. Questa operazione può essere eseguita sia nell'ambito del server che in quello del database. In quest'ultimo caso l'operazione può essere eseguita solo per le specifiche del controllo del database. Nella tabella seguente vengono descritti i gruppi di azioni di controllo a livello di controllo.  
  
|Nome gruppo di azione|Descrizione|  
|-----------------------|-----------------|  
|AUDIT_CHANGE_GROUP|Questo evento viene generato ogni volta che viene eseguito uno dei comandi seguenti:<br /><br /> CREATE SERVER AUDIT<br /><br /> ALTER SERVER AUDIT<br /><br /> DROP SERVER AUDIT<br /><br /> CREATE SERVER AUDIT SPECIFICATION<br /><br /> ALTER SERVER AUDIT SPECIFICATION<br /><br /> DROP SERVER AUDIT SPECIFICATION<br /><br /> CREATE DATABASE AUDIT SPECIFICATION<br /><br /> ALTER DATABASE AUDIT SPECIFICATION<br /><br /> DROP DATABASE AUDIT SPECIFICATION|  
  
## <a name="related-content"></a>Contenuto correlato  
 [Creazione di un controllo del server e di una specifica del controllo del server](../../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
 [Creazione di un controllo del server e di una specifica del controllo del database](../../../relational-databases/security/auditing/create-a-server-audit-and-database-audit-specification.md)  
  
 [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-server-audit-transact-sql.md)  
  
 [ALTER SERVER AUDIT &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-server-audit-transact-sql.md)  
  
 [DROP SERVER AUDIT &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-server-audit-transact-sql.md)  
  
 [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/create-server-audit-specification-transact-sql.md)  
  
 [ALTER SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-server-audit-specification-transact-sql.md)  
  
 [DROP SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-server-audit-specification-transact-sql.md)  
  
 [CREATE DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/create-database-audit-specification-transact-sql.md)  
  
 [ALTER DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-database-audit-specification-transact-sql.md)  
  
 [DROP DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-database-audit-specification-transact-sql.md)  
  
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-authorization-transact-sql.md)  
  
 [sys.fn_get_audit_file &#40;Transact-SQL&#41;](../../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)  
  
 [sys.server_audits &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)  
  
 [sys.server_file_audits &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)  
  
 [sys.server_audit_specifications &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)  
  
 [sys.server_audit_specification_details &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)  
  
 [sys.database_audit_specifications &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)  
  
 [sys.database_audit_specification_details &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)  
  
 [sys.dm_server_audit_status &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)  
  
 [sys.dm_audit_actions &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)  
  
 [sys.dm_audit_class_type_map &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)  
  
  
