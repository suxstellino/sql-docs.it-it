---
title: Determinare le autorizzazioni valide per il motore di database | Microsoft Docs
description: Informazioni su come individuare gli utenti che hanno le autorizzazioni per i vari oggetti nel motore di database di SQL Server, inclusi i sistemi delle autorizzazioni correnti e precedenti.
ms.custom: ''
ms.date: 01/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- permissions, effective
- effective permissions
ms.assetid: 273ea09d-60ee-47f5-8828-8bdc7a3c3529
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 61378302f1ce2b49487cabb63f9580cd82f51409
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755671"
---
# <a name="determining-effective-database-engine-permissions"></a>Determinare le autorizzazioni valide per il motore di database
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Questo articolo descrive come determinare chi ha le autorizzazioni per i vari oggetti nel motore di database di SQL Server. SQL Server implementa due sistemi di autorizzazione per il motore di database. Nel sistema precedente basato sui ruoli predefiniti esistono autorizzazioni preconfigurate. A partire da SQL Server 2005 è disponibile un sistema più flessibile e preciso. Le informazioni in questo articolo si applicano a SQL Server a partire dalla versione 2005. Alcuni tipi di autorizzazioni non sono disponibili in alcune versioni di SQL Server.

> [!IMPORTANT]
>  * Le autorizzazioni valide sono il risultato dell'aggregazione di entrambi i sistemi di autorizzazione. 
>  * La negazione di autorizzazioni è prioritaria rispetto alla concessione di autorizzazioni. 
>  * Se un utente è un membro del ruolo predefinito del server sysadmin, è possibile che le autorizzazioni non vengono controllate e quindi che le negazioni non vengano applicate. 
>  * Il sistema precedente e quello nuovo hanno alcune analogie. Ad esempio, l'appartenenza al ruolo predefinito del server `sysadmin` è simile alla disponibilità dell'autorizzazione `CONTROL SERVER`. Ma i sistemi non sono identici. Ad esempio, se un account di accesso ha solo l'autorizzazione `CONTROL SERVER` e una stored procedure controlla l'appartenenza al ruolo predefinito del server `sysadmin`, il controllo delle autorizzazioni avrà esito negativo. È anche vero il contrario. 


## <a name="summary"></a>Summary   
* Le autorizzazioni a livello di server possono derivare dall'appartenenza ai ruoli predefiniti del server o ai ruoli del server definiti dall'utente. Tutti gli utenti appartengono al ruolo predefinito del server `public` e ricevono tutte le autorizzazioni assegnate a tale ruolo.   
* Le autorizzazioni a livello di server possono derivare dalle autorizzazioni concesse agli account di accesso o ai ruoli del server definiti dall'utente.   
* Le autorizzazioni a livello di database possono derivare dall'appartenenza ai ruoli predefiniti del database o ai ruoli del database definiti dall'utente. Tutti gli utenti appartengono al ruolo predefinito del database `public` e ricevono tutte le autorizzazioni assegnate a tale ruolo.   
* Le autorizzazioni a livello di database possono derivare dalle autorizzazioni concesse agli utenti o ai ruoli del database definiti dagli utenti in ogni database.   
* Le autorizzazioni possono essere ricevute dall'account di accesso `guest` o dall'utente di database `guest`, se abilitato. L'account di accesso `guest` e gli utenti sono disabilitati per impostazione predefinita.   
* Gli utenti di Windows possono essere membri dei gruppi di Windows che possono avere account di accesso. SQL Server viene a conoscenza dell'appartenenza ai gruppi di Windows quando un utente di Windows si connette e presenta un token di Windows con l'identificatore di sicurezza di un gruppo di Windows. Dato che SQL Server non gestisce o riceve gli aggiornamenti automatici sulle appartenenze ai gruppi di Windows, SQL Server non può fare riferimento in modo affidabile alle autorizzazioni degli utenti di Windows ricevute tramite l'appartenenza a gruppi di Windows.   
* Le autorizzazioni possono essere acquisite passando a un ruolo applicazione e fornendo la password.   
* Le autorizzazioni possono essere acquisite eseguendo una stored procedure che include la clausola `EXECUTE AS`.   
* Le autorizzazioni possono essere acquisite da account di accesso o utenti con l'autorizzazione `IMPERSONATE`.   
* I membri del gruppo di amministratori locali del computer possono sempre elevare i propri privilegi a `sysadmin`. (Non si applica a database SQL.)  
* I membri del ruolo predefinito del server `securityadmin` possono elevare molti dei propri privilegi e in alcuni casi possono elevare i privilegi a `sysadmin`. (Non si applica a database SQL.)   
* Gli amministratori di SQL Server possono visualizzare informazioni su tutti gli account di accesso e gli utenti. Gli utenti con privilegi inferiori in genere possono visualizzare informazioni solo sulle proprie identità.

## <a name="older-fixed-role-permission-system"></a>Sistema di autorizzazione precedente con ruoli predefiniti

I ruoli predefiniti del server e del database hanno autorizzazioni preconfigurate non modificabili. Per determinare i membri di un ruolo predefinito del server, eseguire la query seguente:    
> [!NOTE]
>  Non si applica al database SQL o ad Azure Synapse Analytics per cui non è disponibile l'autorizzazione a livello di server. La colonna `is_fixed_role` di `sys.server_principals` è stata aggiunta in SQL Server 2012. Non è necessaria per le versioni precedenti di SQL Server.  
> ```sql
> SELECT SP1.name AS ServerRoleName, 
>  isnull (SP2.name, 'No members') AS LoginName   
>  FROM sys.server_role_members AS SRM
>  RIGHT OUTER JOIN sys.server_principals AS SP1
>    ON SRM.role_principal_id = SP1.principal_id
>  LEFT OUTER JOIN sys.server_principals AS SP2
>    ON SRM.member_principal_id = SP2.principal_id
>  WHERE SP1.is_fixed_role = 1 -- Remove for SQL Server 2008
>  ORDER BY SP1.name;
> ```
> [!NOTE]
>  * Tutti gli account di accesso sono membri del ruolo public e non possono essere rimossi. 
>  * Questa query controlla le tabelle nel database master, ma può essere eseguita in qualsiasi database per il prodotto locale. 

Per determinare i membri di un ruolo predefinito del database, eseguire la query seguente in ogni database.
```sql
SELECT DP1.name AS DatabaseRoleName, 
   isnull (DP2.name, 'No members') AS DatabaseUserName 
 FROM sys.database_role_members AS DRM
 RIGHT OUTER JOIN sys.database_principals AS DP1
   ON DRM.role_principal_id = DP1.principal_id
 LEFT OUTER JOIN sys.database_principals AS DP2
   ON DRM.member_principal_id = DP2.principal_id
 WHERE DP1.is_fixed_role = 1
 ORDER BY DP1.name;
```
Per informazioni sulle autorizzazioni concesse a ogni ruolo, vedere le descrizioni dei ruoli nelle illustrazioni nella documentazione in linea ([Ruoli a livello di server](../../../relational-databases/security/authentication-access/server-level-roles.md) e [Ruoli a livello di database](../../../relational-databases/security/authentication-access/database-level-roles.md)).

## <a name="newer-granular-permission-system"></a>Nuovo sistema di autorizzazioni granulari

Questo sistema è flessibile e ciò significa che può diventare complicato se gli utenti che lo configurano vogliono essere precisi. Per semplificare la configurazione può essere utile creare ruoli, assegnare autorizzazioni ai ruoli e quindi aggiungere gruppi di utenti ai ruoli. Si può semplificare ulteriormente il sistema se il team di sviluppo del database separa le attività in base allo schema e quindi concede le autorizzazioni di ruolo per un intero schema anziché per singole tabelle o stored procedure. Gli scenari reali sono complessi e, per esigenze aziendali, si possono creare requisiti di sicurezza imprevisti.   

[!INCLUDE[database-engine-permissions](../../../includes/paragraph-content/database-engine-permissions.md)]


### <a name="security-classes"></a>Classi di sicurezza

Le autorizzazioni possono essere concesse a livello di server, a livello di database, a livello di schema, a livello di oggetto e così via. Esistono 26 livelli (denominati classi). L'elenco completo delle classi in ordine alfabetico è: `APPLICATION ROLE`, `ASSEMBLY`, `ASYMMETRIC KEY`, `AVAILABILITY GROUP`, `CERTIFICATE`, `CONTRACT`, `DATABASE`, `DATABASE` `SCOPED CREDENTIAL`, `ENDPOINT`, `FULLTEXT CATALOG`, `FULLTEXT STOPLIST`, `LOGIN`, `MESSAGE TYPE`, `OBJECT`, `REMOTE SERVICE BINDING`, `ROLE`, `ROUTE`, `SCHEMA`, `SEARCH PROPERTY LIST`, `SERVER`, `SERVER ROLE`, `SERVICE`, `SYMMETRIC KEY`, `TYPE`, `USER`, `XML SCHEMA COLLECTION`. (Alcune classi non sono disponibili per alcuni tipi di SQL Server.) Per ottenere informazioni complete su ogni classe, è necessaria una query diversa.

### <a name="principals"></a>Principals

Le autorizzazioni vengono concesse alle entità. Le entità possono essere ruoli del server, account di accesso, ruoli del database o utenti. Gli account di accesso possono rappresentare gruppi di Windows che includono molti utenti di Windows. Dato che i gruppi di Windows non vengono gestiti in SQL Server, SQL Server non sempre conosce i membri di un gruppo di Windows. Quando un utente di Windows si connette a SQL Server, il pacchetto di accesso contiene i token di appartenenza ai gruppi di Windows per l'utente.

Quando un utente di Windows si connette usando un account di accesso basato su un gruppo di Windows, alcune attività potrebbero richiedere la creazione di un account accesso in SQL Server o la rappresentazione dell'utente di Windows. Si supponga che un gruppo di Windows (Tecnici) contenga gli utenti Maria, Luca ed Enzo e che il gruppo Tecnici abbia un account utente del database. Se Maria è autorizzata e crea una tabella, potrebbe essere creato un utente (Maria) come proprietario della tabella. Se a Luca viene negata un'autorizzazione che invece ha il resto del gruppo Tecnici, è necessario creare l'utente Luca per tenere traccia della negazione dell'autorizzazione.

Tenere presente che un utente di Windows potrebbe essere un membro di più di un gruppo di Windows, ad esempio sia Tecnici che Manager. Per determinare le autorizzazioni valide, verranno aggregate e valutate tutte le autorizzazioni concesse o negate all'account di accesso Tecnici, all'account di accesso Manager, agli utenti singolarmente e ai ruoli di cui sono membri degli utenti. La funzione `HAS_PERMS_BY_NAME` consente di scoprire se un utente o un account di accesso ha una particolare autorizzazione. Non esiste tuttavia un modo ovvio per determinare l'origine della concessione o negazione dell'autorizzazione. Studiare l'elenco di autorizzazioni e sperimentare.

## <a name="useful-queries"></a>Query utili

### <a name="server-permissions"></a>Autorizzazioni per il server

La query seguente restituisce un elenco delle autorizzazioni concesse o negate a livello di server. Questa query deve essere eseguita nel database master.   
> [!NOTE]
>  Non è possibile concedere autorizzazioni a livello di server o eseguire query per recuperare tali autorizzazioni nel database SQL o in Azure Synapse Analytics.   
> ```sql
> SELECT pr.type_desc, pr.name, 
>  isnull (pe.state_desc, 'No permission statements') AS state_desc, 
>  isnull (pe.permission_name, 'No permission statements') AS permission_name 
>  FROM sys.server_principals AS pr
>  LEFT OUTER JOIN sys.server_permissions AS pe
>    ON pr.principal_id = pe.grantee_principal_id
>  WHERE is_fixed_role = 0 -- Remove for SQL Server 2008
>  ORDER BY pr.name, type_desc;
> ```

### <a name="database-permissions"></a>Autorizzazioni per il database

La query seguente restituisce un elenco delle autorizzazioni concesse o negate a livello di database. Questa query deve essere eseguita in ogni database.   
```sql
SELECT pr.type_desc, pr.name, 
 isnull (pe.state_desc, 'No permission statements') AS state_desc, 
 isnull (pe.permission_name, 'No permission statements') AS permission_name 
FROM sys.database_principals AS pr
LEFT OUTER JOIN sys.database_permissions AS pe
    ON pr.principal_id = pe.grantee_principal_id
WHERE pr.is_fixed_role = 0 
ORDER BY pr.name, type_desc;
```

Ogni classe di autorizzazione per la tabella delle autorizzazioni può essere unita in join ad altre viste di sistema che forniscono informazioni correlate su tale classe di entità a protezione diretta. Ad esempio, la query seguente fornisce il nome dell'oggetto di database interessato dall'autorizzazione.    
```sql
SELECT pr.type_desc, pr.name, pe.state_desc, 
 pe.permission_name, s.name + '.' + oj.name AS Object, major_id
 FROM sys.database_principals AS pr
 JOIN sys.database_permissions AS pe
   ON pr.principal_id = pe.grantee_principal_id
 JOIN sys.objects AS oj
   ON oj.object_id = pe.major_id
 JOIN sys.schemas AS s
   ON oj.schema_id = s.schema_id
 WHERE class_desc = 'OBJECT_OR_COLUMN';
```
Usare la funzione `HAS_PERMS_BY_NAME` per determinare se un utente specifico (in questo caso `TestUser`) ha un'autorizzazione. Ad esempio:   
```sql
EXECUTE AS USER = 'TestUser';
SELECT HAS_PERMS_BY_NAME ('dbo.T1', 'OBJECT', 'SELECT');
REVERT;
```
Per informazioni dettagliate sulla sintassi, vedere [HAS_PERMS_BY_NAME](../../../t-sql/functions/has-perms-by-name-transact-sql.md).

## <a name="see-also"></a>Vedere anche:

[Introduzione alle autorizzazioni del motore di database](../../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)    
[Esercitazione: Introduzione al motore di database](../../../relational-databases/tutorial-getting-started-with-the-database-engine.md) 

