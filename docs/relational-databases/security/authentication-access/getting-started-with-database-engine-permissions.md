---
title: Introduzione alle autorizzazioni del motore di database | Microsoft Docs
description: Esaminare alcuni concetti di base sulla sicurezza in SQL Server e ottenere informazioni su un'implementazione tipica delle autorizzazioni del motore di database.
ms.custom: ''
ms.date: 01/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: quickstart
helpviewer_keywords:
- permissions [SQL Server], getting started
ms.assetid: 051af34e-bb5b-403e-bd33-007dc02eef7b
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 51e1253eb44d49285a30e8a0ec77ddf6caae7cba
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750431"
---
# <a name="getting-started-with-database-engine-permissions"></a>Introduzione alle autorizzazioni del motore di database
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Le autorizzazioni del [!INCLUDE[ssDE](../../../includes/ssde-md.md)] vengono gestite a livello di server tramite gli account di accesso e i ruoli del server e a livello di database tramite gli utenti e i ruoli del database. Il modello per il [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] espone lo stesso sistema all'interno di ogni database, ma le autorizzazioni a livello di server non saranno disponibili. Questo argomento illustra alcuni concetti di base sulla sicurezza e quindi descrive un'implementazione tipica delle autorizzazioni.  
  
## <a name="security-principals"></a>Entità di sicurezza  
 Con entità di sicurezza si definiscono le identità che usano [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e a cui è possibile assegnare delle autorizzazioni per eseguire varie azioni. Si tratta in genere di utenti o gruppi di utenti, ma possono essere altre entità che fingono di essere utenti. Le entità di sicurezza possono essere create e gestite con il linguaggio [!INCLUDE[tsql](../../../includes/tsql-md.md)] elencato o con [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
##### <a name="logins"></a>Logins  
 Gli account di accesso sono account utente singoli per l'accesso al [!INCLUDE[ssDEnoversion](../../../includes/ssdenoversion-md.md)]. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e il [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] supportano account di accesso basati sull'autenticazione di Windows e account di accesso basati sull'autenticazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per informazioni sui due tipi di account di accesso, vedere [Choose an Authentication Mode](../../../relational-databases/security/choose-an-authentication-mode.md).  
  
##### <a name="fixed-server-roles"></a>Ruoli predefiniti del server  
 In [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]i ruoli predefiniti del server sono costituiti da un set di ruoli preconfigurati che forniscono una serie appropriata di autorizzazioni a livello di server. Gli account di accesso possono essere aggiunti ai ruoli con l'istruzione `ALTER SERVER ROLE ... ADD MEMBER` . Per altre informazioni, vedere [ALTER SERVER ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-server-role-transact-sql.md). [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] non supporta i ruoli predefiniti del server, ma ha due ruoli nel database master (`dbmanager` e `loginmanager`) che fungono da ruoli del server.  
  
##### <a name="user-defined-server-roles"></a>Ruoli del server definiti dall'utente  
 In [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]è possibile creare ruoli del server personalizzati e assegnarvi autorizzazioni a livello di server. Gli account di accesso possono essere aggiunti ai ruoli del server con l'istruzione `ALTER SERVER ROLE ... ADD MEMBER` . Per altre informazioni, vedere [ALTER SERVER ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-server-role-transact-sql.md). [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] non supporta i ruoli del server definiti dall'utente.  
  
##### <a name="database-users"></a>Utenti di database  
 Agli account di accesso viene concesso l'accesso a un database creando un utente in un database ed eseguendo il mapping di tale utente di database all'account di accesso. In genere, il nome utente di database è identico al nome dell'account di accesso, anche se non è necessario. Ogni utente di database esegue il mapping a un singolo account di accesso. Il mapping di un account di accesso può essere eseguito a un solo utente in un database, ma può essere eseguito come utente di database in diversi database.  
  
 Gli utenti di database possono anche essere creati senza avere un account di accesso corrispondente e vengono denominati *utenti di database indipendente*. [!INCLUDE[msCoName](../../../includes/msconame-md.md)] promuove l'uso di utenti di database indipendente perché semplifica lo spostamento di un database in un altro server. Analogamente agli account di accesso, gli utenti di database indipendente possono usare l'autenticazione di Windows o l'autenticazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Utenti di database indipendente: rendere portabile un database](../../../relational-databases/security/contained-database-users-making-your-database-portable.md).  
  
 Esistono 12 tipi di utenti con piccole differenze per la modalità di autenticazione e la relativa rappresentazione. Per vedere un elenco di utenti, vedere [CREATE USER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-user-transact-sql.md).  
  
##### <a name="fixed-database-roles"></a>Ruoli predefiniti del database  
 I ruoli predefiniti del database sono costituiti da un set di ruoli preconfigurati che forniscono una serie appropriata di autorizzazioni a livello di database. Gli utenti del database e i ruoli del database definiti dall'utente possono essere aggiunti ai ruoli predefiniti del database con l'istruzione  `ALTER ROLE ... ADD MEMBER` . Per altre informazioni, vedere [ALTER ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-role-transact-sql.md).  
  
##### <a name="user-defined-database-roles"></a>Ruoli del database definiti dall'utente  
 Gli utenti con l'autorizzazione `CREATE ROLE` possono creare nuovi ruoli del database definiti dall'utente per rappresentare gruppi di utenti con autorizzazioni comuni. In genere, le autorizzazioni vengono concesse o negate per l'intero ruolo, semplificando la gestione e il monitoraggio delle autorizzazioni. Gli utenti di database possono essere aggiunti ai ruoli del database con l'istruzione `ALTER ROLE ... ADD MEMBER` . Per altre informazioni, vedere [ALTER ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-role-transact-sql.md).  
  
##### <a name="other-principals"></a>Altre entità  
 Altre entità di sicurezza non illustrate nel presente articolo includono ruoli applicazione nonché account di accesso e utenti basati su certificati o chiavi asimmetriche.  
  
 Per un grafico che mostra le relazioni tra utenti di Windows, gruppi di Windows, account di accesso e utenti di database, vedere [Create a Database User](../../../relational-databases/security/authentication-access/create-a-database-user.md).  
  
## <a name="typical-scenario"></a>Scenario tipico  
 L'esempio seguente rappresenta un metodo comune e consigliato di configurazione delle autorizzazioni.  
  
#### <a name="in-active-directory-or-azure-active-directory"></a>In Active Directory o Azure Active Directory:  
  
1.  Creare un utente di Windows per ogni utente.  
  
2.  Creare gruppi di Windows che rappresentano le unità di lavoro e le funzioni di lavoro.  
  
3.  Aggiungere gli utenti di Windows ai gruppi di Windows.  

#### <a name="if-the-person-connecting-will-be-connecting-to-many-databases"></a>Se l'utente che si connette verrà connesso a molti database  
  
1.  Creare un account di accesso per i gruppi di Windows. Se si usa l'autenticazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , ignorare i passaggi di Active Directory e creare qui gli account di accesso con autenticazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
2.  Nel database utente creare un utente di database per l'account di accesso che rappresenta i gruppi di Windows.  
  
3.  Nel database utente creare uno o più ruoli del database definiti dall'utente, ognuno dei quali rappresenta una funzione simile, ad esempio analista finanziario e analista vendite.  
  
4.  Aggiungere gli utenti di database a uno o più ruoli del database definiti dall'utente.  
  
5.  Concedere le autorizzazioni ai ruoli del database definiti dall'utente.  
  
#### <a name="if-the-person-connecting-will-be-connecting-to-only-one-database"></a>Se l'utente che si connette verrà connesso a un solo database  
  
1.  Nel database utente creare un utente di database indipendente per il gruppo di Windows. Se si usa l'autenticazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , ignorare i passaggi di Active Directory e creare qui l'autenticazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per gli utenti di database indipendente.  
  
1.  Nel database utente creare uno o più ruoli del database definiti dall'utente, ognuno dei quali rappresenta una funzione simile, ad esempio analista finanziario e analista vendite.  
  
1.  Aggiungere gli utenti di database a uno o più ruoli del database definiti dall'utente.  
  
1.  Concedere le autorizzazioni ai ruoli del database definiti dall'utente.  
  
 Il risultato tipico a questo punto è che un utente di Windows è un membro di un gruppo di Windows. Il gruppo di Windows ha un account di accesso in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o nel [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]. Viene eseguito il mapping dell'account di accesso a un'identità utente nel database utente. L'utente è un membro di un ruolo del database. È necessario ora aggiungere le autorizzazioni al ruolo.  
  
## <a name="assigning-permissions"></a>Assegnazione delle autorizzazioni  
 Di seguito è riportato il formato della maggior parte delle istruzioni di autorizzazione:  
  
```  
AUTHORIZATION  PERMISSION  ON  SECURABLE::NAME  TO  PRINCIPAL;  
```  
  
-   `AUTHORIZATION` deve essere `GRANT`, `REVOKE` o `DENY`.  
  
-   `PERMISSION` stabilisce l'azione consentita o quella non consentita. [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] può specificare 230 autorizzazioni. [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] contiene meno autorizzazioni perché alcune azioni non sono rilevanti in Azure. Le autorizzazioni sono elencate nell'argomento [Autorizzazioni &#40;motore di database&#41;](../../../relational-databases/security/permissions-database-engine.md) e nel grafico riportato più avanti.  
  
-   `ON SECURABLE::NAME` specifica il tipo di oggetto a protezione diretta (server, oggetto server, database o oggetto di database) e il nome corrispondente. Alcune autorizzazioni non richiedono `ON SECURABLE::NAME` perché non è ambiguo o non è appropriato nel contesto. Ad esempio, con l'autorizzazione `CREATE TABLE` non è richiesta la clausola `ON SECURABLE::NAME`. Ad esempio, l'istruzione `GRANT CREATE TABLE TO Mary;` consente a Mary di creare tabelle.  
  
-   `PRINCIPAL` è l'entità di sicurezza (account di accesso, utente o ruolo) che riceve o perde l'autorizzazione. Concedere le autorizzazioni ai ruoli quando possibile.  
  
 L'istruzione di concessione dell'esempio seguente concede l'autorizzazione `UPDATE` per la tabella o vista `Parts` contenuta nello schema `Production` al ruolo denominato `PartsTeam`:  
  
```  
GRANT UPDATE ON OBJECT::Production.Parts TO PartsTeam;  
```  
  
 Le autorizzazioni vengono concesse alle entità di sicurezza (account di accesso, utenti e ruoli) con l'istruzione `GRANT` . Le autorizzazioni vengono negate in modo esplicito con il comando  `DENY` . Un'autorizzazione concessa o negata in precedenza viene rimossa con l'istruzione `REVOKE` . Le autorizzazioni sono cumulative, con l'utente che riceve tutte le autorizzazioni concesse all'utente, all'account di accesso e a qualsiasi appartenenza a un gruppo. Tuttavia, la negazione di un'autorizzazione prevale su tutte le concessioni.  
  
> [!TIP]  
>  Un errore comune consiste nel provare a rimuovere l'autorizzazione `GRANT` con `DENY` anziché `REVOKE`. Ciò può causare problemi quando un utente riceve autorizzazioni da più origini, e tale circostanza è piuttosto comune. L'esempio seguente illustra l'uso delle entità.  
  
 Il gruppo Sales riceve le autorizzazioni `SELECT` per la tabella OrderStatus con l'istruzione `GRANT SELECT ON OBJECT::OrderStatus TO Sales;`. L'utente Ted è un membro del ruolo Sales. A Ted è stata concessa anche l'autorizzazione `SELECT` per la tabella OrderStatus con il proprio nome utente con l'istruzione  `GRANT SELECT ON OBJECT::OrderStatus TO Ted;`. Si supponga che l'amministratore voglia rimuovere l'autorizzazione `GRANT` al ruolo Sales.  
  
-   Se l'amministratore esegue `REVOKE SELECT ON OBJECT::OrderStatus TO Sales;`in modo corretto, Ted conserverà l'accesso `SELECT` alla tabella OrderStatus con l'istruzione `GRANT` personale.  
  
-   Se l'amministratore esegue `DENY SELECT ON OBJECT::OrderStatus TO Sales;` in modo non corretto, a Ted, come membro del ruolo Sales, verrà negata l'autorizzazione `SELECT` perché l'istruzione `DENY` per il gruppo Sales prevale sull'istruzione  `GRANT`personale.  
  
> [!NOTE]  
>  Le autorizzazioni possono essere configurate con [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]. Cercare l'oggetto a protezione diretta in Esplora oggetti, fare clic con il pulsante destro del mouse sull'oggetto e quindi scegliere **Proprietà**. Selezionare la pagina **Autorizzazioni** . Per informazioni sull'uso della pagina delle autorizzazioni, vedere [Permissions or Securables Page](../../../relational-databases/security/permissions-or-securables-page.md).  
  
## <a name="permission-hierarchy"></a>Gerarchia delle autorizzazioni  
 Le autorizzazioni hanno una gerarchia padre/figlio, ovvero se si concede l'autorizzazione `SELECT` per un database, tale autorizzazione include l'autorizzazione `SELECT` per tutti gli schemi (figlio) presenti nel database. Se si concede l'autorizzazione `SELECT` per uno schema, tale autorizzazione include l'autorizzazione `SELECT` per tutte le tabelle e le viste (figlio) presenti nello schema. Le autorizzazioni sono transitive, ovvero se si concede l'autorizzazione `SELECT` per un database, tale autorizzazione include l'autorizzazione `SELECT` per tutti gli schemi (figlio) e tutte le tabelle e le viste (nipote).  
  
 Le autorizzazioni contengono anche le autorizzazioni implicite. L'autorizzazione `CONTROL` per un oggetto concede in genere tutte le altre autorizzazioni per l'oggetto.  
  
 Poiché sia la gerarchia padre/figlio che la gerarchia implicita possono agire sulla stessa autorizzazione, il sistema di autorizzazioni può diventare complicato. Prendiamo ad esempio una tabella (Region) di uno schema (Customers) in un database (SalesDB).  
  
-   `CONTROL` per la tabella Region include tutte le altre autorizzazioni per la tabella Region, tra cui `ALTER`, `SELECT`, `INSERT`,  `UPDATE`, `DELETE`e altre autorizzazioni.  
  
-   `SELECT` per lo schema Customers a cui appartiene la tabella Region include l'autorizzazione `SELECT` per la tabella Region.  
  
 L'autorizzazione `SELECT` per la tabella Region può quindi essere ottenuta con una di queste sei istruzioni:  
  
```  
GRANT SELECT ON OBJECT::Region TO Ted;   
  
GRANT CONTROL ON OBJECT::Region TO Ted;   
  
GRANT SELECT ON SCHEMA::Customers TO Ted;   
  
GRANT CONTROL ON SCHEMA::Customers TO Ted;   
  
GRANT SELECT ON DATABASE::SalesDB TO Ted;   
  
GRANT CONTROL ON DATABASE::SalesDB TO Ted;  
```  
  
## <a name="grant-the-least-permission"></a>Concedere l'autorizzazione minima  
 La prima autorizzazione elencata in precedenza (`GRANT SELECT ON OBJECT::Region TO Ted;`) è la più granulare, ovvero tale istruzione è l'autorizzazione minima possibile che concede l'autorizzazione `SELECT`. Non sono incluse autorizzazioni per gli oggetti subordinati. È un ottimo principio concedere sempre l'autorizzazione minima possibile, ma (contraddicendo ciò) concederla a livelli superiori per semplificare il sistema di concessione. Se quindi Ted deve ottenere le autorizzazioni per l'intero schema, concedere `SELECT` una sola volta a livello di schema, anziché concedere `SELECT` molte volte a livello di tabella o di vista. La progettazione del database ha un notevole impatto sull'efficacia di questa strategia. Questa strategia funziona meglio quando il database viene progettato in modo tale che gli oggetti che devono ottenere autorizzazioni identiche vengano inclusi in un singolo schema.  
  
## <a name="list-of-permissions"></a>Elenco di autorizzazioni  
 [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] contiene 230 autorizzazioni. [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] contiene 219 autorizzazioni. [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] contiene 214 autorizzazioni. [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)] contiene 195 autorizzazioni. [!INCLUDE[ssSDS](../../../includes/sssds-md.md)], [!INCLUDE[ssDW](../../../includes/ssdw-md.md)]e la [!INCLUDE[ssAPS](../../../includes/ssaps-md.md)] contengono meno autorizzazioni perché espongono solo una parte del motore di database, anche se contengono alcune autorizzazioni che non si applicano a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. 
 
 [!INCLUDE[database-engine-permissions](../../../includes/paragraph-content/database-engine-permissions.md)]
 
 Per una rappresentazione grafica delle relazioni tra le entità del [!INCLUDE[ssDE](../../../includes/ssde-md.md)] e gli oggetti server e di database, vedere [Gerarchia delle autorizzazioni &#40;motore di database&#41;](../../../relational-databases/security/permissions-hierarchy-database-engine.md).  
  
## <a name="permissions-vs-fixed-server-and-fixed-database-roles"></a>Autorizzazioni e ruoli predefiniti del server e ruoli predefiniti del database  
 Le autorizzazioni dei ruoli predefiniti del server e dei ruoli predefiniti del database sono simili ma non esattamente uguali alle autorizzazioni granulari. Ad esempio, i membri del ruolo predefinito del server `sysadmin` hanno tutte le autorizzazioni per l'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], come gli account di accesso con l'autorizzazione `CONTROL SERVER` . Ma concedere l'autorizzazione `CONTROL SERVER` non rende un account di accesso membro del ruolo predefinito del server sysadmin e aggiungere un account di accesso al ruolo predefinito del server `sysadmin` non concede in modo esplicito l'autorizzazione `CONTROL SERVER` all'account di accesso. Una stored procedure potrebbe talvolta verificare le autorizzazioni controllando il ruolo predefinito e non controllando l'autorizzazione granulare. Ad esempio, per scollegare un database è necessaria l'appartenenza al ruolo predefinito del database `db_owner` . L'autorizzazione `CONTROL DATABASE` equivalente non è sufficiente. Questi due sistemi operano in parallelo ma raramente interagiscono tra loro. Se possibile, Microsoft consiglia di usare il sistema più recente di autorizzazioni granulari anziché i ruoli predefiniti.
  
## <a name="monitoring-permissions"></a>Monitoraggio delle autorizzazioni  
 Le viste seguenti restituiscono informazioni sulla sicurezza.  
  
-   Gli account di accesso e i ruoli del server definiti dall'utente in un server possono essere esaminati con la vista `sys.server_principals` . Questa vista non è disponibile nel [!INCLUDE[ssSDS](../../../includes/sssds-md.md)].  
  
-   Gli utenti e i ruoli definiti dall'utente in un database possono essere esaminati con la vista `sys.database_principals` .  
  
-   Le autorizzazioni concesse agli account di accesso e ai ruoli predefiniti del server definiti dall'utente possono essere esaminate con la vista `sys.server_permissions` . Questa vista non è disponibile nel [!INCLUDE[ssSDS](../../../includes/sssds-md.md)].  
  
-   Le autorizzazioni concesse agli utenti e ai ruoli predefiniti del database definiti dall'utente possono essere esaminate con la vista `sys.database_permissions` .  
  
-   L'appartenenza ai ruoli del database può essere esaminata con la vista `sys. sys.database_role_members` .  
  
-   L'appartenenza ai ruoli del server può essere esaminata con la vista `sys.server_role_members` . Questa vista non è disponibile nel [!INCLUDE[ssSDS](../../../includes/sssds-md.md)].  
  
-   Per altre viste correlate alla sicurezza, vedere [Viste del catalogo relative alla sicurezza &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md) .  
  
### <a name="useful-transact-sql-statements"></a>Istruzioni Transact-SQL utili  
 Le istruzioni seguenti restituiscono informazioni utili sulle autorizzazioni.  
  
 Per restituire le autorizzazioni esplicite concesse o negate in un database ([!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]), eseguire l'istruzione seguente nel database.  
  
```sql  
SELECT   
    perms.state_desc AS State,   
    permission_name AS [Permission],   
    obj.name AS [on Object],   
    dPrinc.name AS [to User Name]  
FROM sys.database_permissions AS perms  
JOIN sys.database_principals AS dPrinc  
    ON perms.grantee_principal_id = dPrinc.principal_id  
JOIN sys.objects AS obj  
    ON perms.major_id = obj.object_id;  
```  
  
 Per restituire i membri dei ruoli del server (solo [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]), eseguire l'istruzione seguente.  
  
```sql  
SELECT sRole.name AS [Server Role Name] , sPrinc.name AS [Members]  
FROM sys.server_role_members AS sRo  
JOIN sys.server_principals AS sPrinc  
    ON sRo.member_principal_id = sPrinc.principal_id  
JOIN sys.server_principals AS sRole  
    ON sRo.role_principal_id = sRole.principal_id;  
```  
  
 
 Per restituire i membri dei ruoli del database ([!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]), eseguire l'istruzione seguente nel database.  
  
```sql  
SELECT dRole.name AS [Database Role Name], dPrinc.name AS [Members]  
FROM sys.database_role_members AS dRo  
JOIN sys.database_principals AS dPrinc  
    ON dRo.member_principal_id = dPrinc.principal_id  
JOIN sys.database_principals AS dRole  
    ON dRo.role_principal_id = dRole.principal_id;  
```  
  
## <a name="next-steps"></a>Passaggi successivi  
 Per altri argomenti introduttivi, vedere:  
  
-   [Esercitazione: Introduzione al motore di database](../../../relational-databases/tutorial-getting-started-with-the-database-engine.md) 

-   [Creazione di un database &#40;esercitazione&#41;](../../../t-sql/lesson-1-creating-database-objects.md)  
  
-   [Esercitazione: SQL Server Management Studio](../../../ssms/quickstarts/ssms-connect-query-sql-server.md)  
  
-   [Esercitazione: Scrittura di istruzioni Transact-SQL](../../../t-sql/tutorial-writing-transact-sql-statements.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Centro di sicurezza per il motore di database di SQL Server e il database SQL di Azure](../../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)   
 [Funzioni di sicurezza &#40;Transact-SQL&#41;](../../../t-sql/functions/security-functions-transact-sql.md)   
 [Funzioni e viste a gestione dinamica relative alla sicurezza &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Viste del catalogo relative alla sicurezza &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [Determinare le autorizzazioni valide per il motore di database](../../../relational-databases/security/authentication-access/determining-effective-database-engine-permissions.md)
