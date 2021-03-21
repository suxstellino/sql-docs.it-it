---
title: Accesso utente indipendente ai database indipendenti
description: Informazioni su come configurare l'accesso utente indipendente per i database indipendenti e le differenze con il modello tradizionale basato su account di accesso/utente.
ms.custom: seo-lt-2019
ms.date: 01/28/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- contained database, users
- user [SQL Server], about contained database users
ms.assetid: e57519bb-e7f4-459b-ba2f-fd42865ca91d
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||=azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bfd77ab7a8f03431c103326793686eaa49a698b7
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754691"
---
# <a name="contained-database-users---making-your-database-portable"></a>Utenti di database indipendente: rendere portabile un database

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Usare gli utenti di database indipendente per autenticare le connessioni [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssSDS](../../includes/sssds-md.md)] a livello di database. Un database indipendente è un database isolato dagli altri database e dall'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/ [!INCLUDE[ssSDS](../../includes/sssds-md.md)] (e del database master) che ospita il database. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta gli utenti di database indipendente per l'autenticazione di Windows e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Quando si usa [!INCLUDE[ssSDS](../../includes/sssds-md.md)], combinare gli utenti di del database indipendente con le regole firewall a livello di database. Questo argomento illustra le differenze e i vantaggi correlati all'uso del modello di database indipendente rispetto al modello tradizionale basato su account di accesso/utente e alle regole firewall a livello di server o Windows. L'uso del modello tradizionale basato su account di accesso/utente e delle regole firewall a livello di server può essere ancora necessario in scenari specifici, per la gestibilità o per la logica di business dell'applicazione.  
  
> [!NOTE]  
> Quando [!INCLUDE[msCoName](../../includes/msconame-md.md)] evolverà il servizio [!INCLUDE[ssSDS](../../includes/sssds-md.md)] e offrirà a contratti di servizi maggiormente garantiti, potrebbe essere necessario passare al modello basato sull'utente di database indipendente e a regole del firewall con ambito database per usufruire di contratti di servizio con maggiore disponibilità e di un numero superiore di account di accesso consentiti per un determinato database. [!INCLUDE[msCoName](../../includes/msconame-md.md)] invita quindi ad adottare questo approccio già a partire da oggi.  
  
## <a name="traditional-login-and-user-model"></a>Modello tradizionale basato su account di accesso e utente

 Nel modello di connessione tradizionale gli utenti di Windows o i membri di gruppi di Windows si connettono al [!INCLUDE[ssDE](../../includes/ssde-md.md)] fornendo le credenziali utente o di gruppo autenticate da Windows. In alternativa si può specificare un nome e una password e connettersi usando l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . In entrambi i casi, il database master deve disporre di un account di accesso corrispondente alle credenziali di connessione. Dopo che il [!INCLUDE[ssDE](../../includes/ssde-md.md)] ha verificato le credenziali di autenticazione di Windows o ha autenticato le credenziali di autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la connessione in genere tenta di connettersi a un database utente. Per connettersi a un database utente, l'account di accesso deve poter essere sottoposto a mapping (ovvero associato) a un utente del database nel database utente. La stringa di connessione può inoltre specificare la connessione a un database specifico. Questo è facoltativo in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ma obbligatorio nel [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 L'aspetto importante è che sia l'account di accesso (nel database master) che l'utente (nel database utente) devono esistere ed essere correlati tra loro. Ciò significa che la connessione al database utente presenta una dipendenza dall'account di accesso nel database master e questo limita la capacità del database di essere spostato in un server [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)] di hosting diverso. Se inoltre per qualsiasi motivo non è disponibile una connessione al database master (ad esempio è in corso un failover), aumenterà il tempo complessivo di connessione oppure potrà verificarsi un timeout della connessione. Questo potrebbe quindi ridurre la scalabilità della connessione.  
  
## <a name="contained-database-user-model"></a>Modello di utente di database indipendente

 Nel modello di utente di database indipendente l'account di accesso nel database master non è presente. Al contrario, il processo di autenticazione si verifica nel database utente e l'utente del database nel database utente non dispone di un account di accesso associato nel database master. Il modello di utente di database indipendente supporta sia l'autenticazione di Windows che l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e può essere usato sia in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che nel [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Per connettersi come utente di database indipendente, la stringa di connessione deve sempre contenere un parametro per il database utente in modo che il [!INCLUDE[ssDE](../../includes/ssde-md.md)] sappia quale database è responsabile della gestione del processo di autenticazione. L'attività dell'utente di database indipendente è limitata al database di autenticazione, pertanto durante la connessione come utente di database indipendente, l'account utente del database deve essere creato in modo indipendente in ogni database richiesto dall'utente. Per modificare i database, gli utenti del [!INCLUDE[ssSDS](../../includes/sssds-md.md)] devono creare una nuova connessione. Gli utenti di database indipendente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possono modificare i database se è presente un utente identico in un altro database.  
  
**Azure:** [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] e [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] supportano le identità di Azure Active Directory come utenti di database indipendente. [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] supporta gli utenti di database indipendente che usano l'autenticazione di [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , mentre [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] non li supporta. Per altre informazioni, vedere [Connessione al database SQL tramite l'autenticazione di Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview). Quando si usa l'autenticazione di Azure Active Directory, è possibile stabilire una connessione da SQL Server Management Studio usando l'autenticazione universale di Active Directory.  Gli amministratori possono configurare l'autenticazione universale per richiedere l'autenticazione a più fattori, che consente di verificare l'identità con una telefonata, SMS, smart card con pin o la notifica dell'app mobile. Per altre informazioni, vedere [Supporto di SQL Server Management Studio (SSMS) per l'autenticazione MFA di Azure AD con il database SQL e Azure Synapse Analytics](/azure/azure-sql/database/authentication-mfa-ssms-overview).  
  
 Per [!INCLUDE[ssSDS](../../includes/sssds-md.md)] e [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)], dal momento che il nome del database è sempre richiesto nella stringa di connessione, non sono necessarie modifiche della stringa di connessione quando si passa dal modello tradizionale al modello di utente di database indipendente. Per le connessioni a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , il nome del database deve essere aggiunto alla stringa di connessione, se non è già presente.  
  
> [!IMPORTANT]  
> Quando si usa il modello tradizionale, i ruoli e le autorizzazioni a livello di server possono limitare l'accesso a tutti i database. Quando si usa il modello di database indipendente, i proprietari del database e gli utenti del database con autorizzazione ALTER ANY USER possono concedere l'accesso al database. In questo modo si riduce il controllo di accesso di account di accesso server con privilegi elevati e si espande il controllo di accesso per includere utenti di database con privilegi elevati.  
  
## <a name="firewalls"></a>Firewall  
  
### [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

 Le regole di Windows Firewall si applicano a tutte le connessioni e producono gli stessi effetti sugli account di accesso (connessioni con modello tradizionale) e sugli utenti di database indipendente. Per altre informazioni su Windows Firewall, vedere [Configurazione di Windows Firewall per l'accesso al Motore di database](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md).  
  
### <a name="sssds-firewalls"></a>[!INCLUDE[ssSDS](../../includes/sssds-md.md)] Firewall

 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] consente regole del firewall separate per connessioni a livello di server (account di accesso) e per connessioni a livello di database (utenti di database indipendente). Durante la connessione a un database utente vengono verificate per prime le regole del firewall a livello di database. Se non esistono regole che consentono l'accesso al database, vengono verificate le regole del firewall a livello di server, operazione che richiede l'accesso al database master del server di database SQL. L'uso combinato di regole del firewall a livello di database e utenti di database indipendente consente di non dover accedere al database master del server durante la connessione e implica di conseguenza un miglioramento della scalabilità della connessione.  
  
 Per altre informazioni sulle regole del firewall del [!INCLUDE[ssSDS](../../includes/sssds-md.md)] , vedere gli argomenti seguenti:  
  
- [Firewall del database SQL di Azure](/previous-versions/azure/ee621782(v=azure.100))  
- [Procedura: Configurare le impostazioni del firewall (database SQL di Azure)](/previous-versions/azure/jj553530(v=azure.100))  
- [sp_set_firewall_rule &#40;Database di SQL Azure&#41;](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)  
- [sp_set_database_firewall_rule &#40;Database di SQL Azure&#41;](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md)  
  
## <a name="syntax-differences"></a>Differenze di sintassi  
  
|Modello tradizionale|Modello di utente di database indipendente|  
|-----------------------|-----------------------------------|  
|Quando connesso al database master:<br /><br /> `CREATE LOGIN login_name  WITH PASSWORD = 'strong_password';`<br /><br /> Quindi, quando connesso a un database utente:<br /><br /> `CREATE USER 'user_name' FOR LOGIN 'login_name';`|Quando connesso a un database utente:<br /><br /> `CREATE USER user_name  WITH PASSWORD = 'strong_password';`|  
  
|Modello tradizionale|Modello di utente di database indipendente|  
|-----------------------|-----------------------------------|  
|Per cambiare la password, nel contesto del database master:<br /><br /> `ALTER LOGIN login_name  WITH PASSWORD = 'strong_password';`|Per cambiare la password, nel contesto del database utente:<br /><br /> `ALTER USER user_name  WITH PASSWORD = 'strong_password';`|  

### <a name="managed-instance"></a>database SQL

Istanza gestita di SQL di Azure si comporta come SQL Server in locale nel contesto dei database indipendenti. Assicurarsi di modificare il contesto del database dal database master al database utente durante la creazione dell'utente indipendente. Quando si imposta l'opzione di indipendenza, inoltre, non devono essere presenti connessioni attive al database utente. 


Ad esempio:

> [!WARNING]
> Prima di eseguire lo script seguente, assicurarsi che non siano presenti altre connessioni attive nel database di Istanza gestita. Lo script potrebbe interrompere altri processi in esecuzione nel database.

```sql
Use MASTER;
GO 

ALTER DATABASE Test
SET RESTRICTED_USER
WITH ROLLBACK IMMEDIATE;

ALTER DATABASE Test
SET containment=partial;

ALTER DATABASE Test
SET MULTI_USER;

USE Test;  
GO 

CREATE USER Carlo  
WITH PASSWORD='Enterpwdhere*'  

SELECT containment_desc FROM sys.databases
WHERE name='Test'
```

  
## <a name="remarks"></a>Osservazioni  
  
- In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]gli utenti di database indipendente devono essere abilitati per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Opzione di configurazione del server contained database authentication](../../database-engine/configure-windows/contained-database-authentication-server-configuration-option.md).  
- Gli account di accesso e gli utenti di database indipendente con nomi diversi possono coesistere nelle applicazioni.  
- Se nel database master è presente un account di accesso denominato **nome1** e si crea un utente di database indipendente denominato **nome1**, quando viene specificato un nome di database nella stringa di connessione il contesto dell'utente del database verrà preferito a quello dell'account di accesso durante la connessione al database, ovvero gli utenti di database indipendente avranno la precedenza rispetto agli account di accesso con lo stesso nome.  
- Nel [!INCLUDE[ssSDS](../../includes/sssds-md.md)] il nome dell'utente di database indipendente non può essere uguale a quello dell'account dell'amministratore del server.  
- L'account dell'amministratore del server [!INCLUDE[ssSDS](../../includes/sssds-md.md)] non può mai essere un utente di database indipendente. L'amministratore del server dispone di autorizzazioni sufficienti per creare e gestire utenti del database indipendente. L'amministratore del server può concedere a utenti di database indipendente autorizzazioni per i database utente.  
- Dal momento che gli utenti del database indipendente sono entità a livello di database, è necessario creare utenti del database indipendente in ogni database in cui verranno usati. L'identità è confinata al database ed è da ogni punto di vista indipendente rispetto a un utente con lo stesso nome e la stessa password in un altro database nello stesso server.  
- Usare le stesse password complesse usate in genere per gli account di accesso.  
  
## <a name="see-also"></a>Vedere anche

 [Database indipendenti](../../relational-databases/databases/contained-databases.md)   
 [Procedure consigliate per la sicurezza in database indipendenti](../../relational-databases/databases/security-best-practices-with-contained-databases.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [Connettersi al Database SQL utilizzando l’autenticazione di Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview)  
