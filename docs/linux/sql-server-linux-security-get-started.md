---
title: Introduzione alla sicurezza di SQL Server in Linux
description: Per individuare le aree da esaminare in modo più dettagliato, esaminare le funzionalità di sicurezza di SQL Server in Linux.
author: VanMSFT
ms.author: vanto
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ecc72850-8b01-492e-9a27-ec817648f0e0
ms.openlocfilehash: f4201aa6c07d4ae96d44d8aa443b23e275e1f9f2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346410"
---
# <a name="walkthrough-for-the-security-features-of-sql-server-on-linux"></a>Procedura dettagliata per le funzionalità di sicurezza di SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Le attività seguenti illustrano alcune delle attività di sicurezza per gli utenti Linux che non hanno familiarità con SQL Server. Queste attività non sono univoche o specifiche di Linux, ma offrono un'idea delle aree da approfondire. In ogni esempio viene fornito un collegamento alla documentazione dettagliata dell'area.

> [!NOTE]
>  Gli esempi seguenti usano il database di esempio **AdventureWorks2014**. Per istruzioni su come ottenere e installare questo database di esempio, vedere [Ripristinare un database di SQL Server da Windows a Linux](sql-server-linux-migrate-restore-database.md).


## <a name="create-a-login-and-a-database-user"></a>Creare un account di accesso e un utente del database 

Per concedere ad altri l'accesso a SQL Server, creare un account di accesso nel database master usando l'istruzione [CREATE LOGIN](../t-sql/statements/create-login-transact-sql.md). Ad esempio:

```
CREATE LOGIN Larry WITH PASSWORD = '************';  
```

> [!NOTE]
>  Usare sempre una password complessa al posto degli asterischi del comando precedente.

Gli account di accesso possono connettersi a SQL Server e avere accesso (con autorizzazioni limitate) al database master. Per connettersi a un database utente, un account di accesso deve avere un'identità corrispondente a livello di database, denominata utente del database. Gli utenti sono specifici di ogni database e devono essere creati separatamente in ogni database per concedere loro l'accesso. L'esempio seguente consente di passare al database AdventureWorks2014 e quindi usa l'istruzione [CREATE USER](../t-sql/statements/create-user-transact-sql.md) per creare un utente denominato Larry associato all'account di accesso denominato Larry. Nonostante l'account di accesso e l'utente siano correlati (associati l'uno all'altro), sono oggetti diversi. L'account di accesso è un'entità a livello di server. L'utente è un'entità a livello di database.

```
USE AdventureWorks2014;
GO
CREATE USER Larry;
GO
```

- Un account amministratore di SQL Server può connettersi a qualsiasi database e può creare altri account di accesso e altri utenti in qualsiasi database.  
- Quando un utente crea un database, diventa il proprietario del database, che può connettersi a tale database. I proprietari dei database possono creare altri utenti.

In seguito è possibile autorizzare gli altri account di accesso a creare ulteriori account di accesso concedendo loro l'autorizzazione `ALTER ANY LOGIN`. In un database è possibile autorizzare gli altri utenti a creare ulteriori utenti concedendo loro l'autorizzazione `ALTER ANY USER`. Ad esempio:   

```
GRANT ALTER ANY LOGIN TO Larry;   
GO   
   
USE AdventureWorks2014;   
GO   
GRANT ALTER ANY USER TO Jerry;    
GO   
```

A questo punto, l'account di accesso Larry può creare altri account di accesso e l'utente Jerry può creare altri utenti.


## <a name="granting-access-with-least-privileges"></a>Concessione dell'accesso con privilegi minimi

Le prime persone a connettersi a un database utente saranno gli account dell'amministratore e del proprietario del database. Tuttavia, questi utenti hanno tutte le autorizzazioni disponibili per il database. Si tratta di un'autorizzazione più elevata di quella della maggior parte degli utenti. 

All'inizio, è possibile assegnare alcune categorie generali di autorizzazioni usando i *ruoli predefiniti del database*. Il ruolo predefinito del database `db_datareader`, ad esempio, può leggere tutte le tabelle del database, ma non apportare modifiche. Per concedere l'appartenenza a un ruolo predefinito del database, usare l'istruzione [ALTER ROLE](../t-sql/statements/alter-role-transact-sql.md). L'esempio seguente aggiunge l'utente `Jerry` al ruolo predefinito del database `db_datareader`.   
   
```   
USE AdventureWorks2014;   
GO   
   
ALTER ROLE db_datareader ADD MEMBER Jerry;   
```   

Per un elenco dei ruoli predefiniti del database, vedere [Ruoli a livello di database](../relational-databases/security/authentication-access/database-level-roles.md).

In seguito, quando si è pronti per configurare un accesso più preciso ai dati (scelta consigliata), creare ruoli del database definiti dall'utente usando l'istruzione [CREATE ROLE](../t-sql/statements/create-role-transact-sql.md). Assegnare quindi autorizzazioni granulari specifiche ai ruoli personalizzati.

Le istruzioni seguenti, ad esempio, creano un ruolo del database denominato `Sales`, concede al gruppo `Sales` la possibilità di visualizzare, aggiornare ed eliminare righe dalla tabella `Orders` e quindi aggiunge l'utente `Jerry` al ruolo `Sales`.   
   
```   
CREATE ROLE Sales;   
GRANT SELECT ON Object::Sales TO Orders;   
GRANT UPDATE ON Object::Sales TO Orders;   
GRANT DELETE ON Object::Sales TO Orders;   
ALTER ROLE Sales ADD MEMBER Jerry;   
```

Per altre informazioni sul sistema di autorizzazione, vedere [Introduzione alle autorizzazioni del motore di database](../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).


## <a name="configure-row-level-security"></a>Configurare la sicurezza a livello di riga  

La [sicurezza a livello di riga](../relational-databases/security/row-level-security.md) consente di limitare l'accesso alle righe di un database in base all'utente che esegue una query. Questa funzionalità è utile per situazioni in cui, ad esempio, si deve garantire che i clienti possano accedere solo ai propri dati o che i ruoli di lavoro possano accedere solo ai dati pertinenti al proprio reparto.   

I passaggi seguenti illustrano come configurare due utenti con accesso a livello di riga diverso alla tabella `Sales.SalesOrderHeader`. 

Creare due account utente per testare la sicurezza a livello di riga:    
   
```   
USE AdventureWorks2014;   
GO   
   
CREATE USER Manager WITHOUT LOGIN;     
   
CREATE USER SalesPerson280 WITHOUT LOGIN;    
```

Concedere l'accesso in lettura alla tabella `Sales.SalesOrderHeader` a entrambi gli utenti:    
   
```   
GRANT SELECT ON Sales.SalesOrderHeader TO Manager;      
GRANT SELECT ON Sales.SalesOrderHeader TO SalesPerson280;
```
   
Creare un nuovo schema e una funzione con valori di tabella inline. La funzione restituisce 1 quando una riga della colonna `SalesPersonID` corrisponde all'ID di un accesso `SalesPerson` o se l'utente che esegue la query o se l'utente che esegue la query è l'utente gestore.   
   
```     
CREATE SCHEMA Security;   
GO   
   
CREATE FUNCTION Security.fn_securitypredicate(@SalesPersonID AS int)     
    RETURNS TABLE   
WITH SCHEMABINDING   
AS     
   RETURN SELECT 1 AS fn_securitypredicate_result    
WHERE ('SalesPerson' + CAST(@SalesPersonId as VARCHAR(16)) = USER_NAME())     
    OR (USER_NAME() = 'Manager');    
```

Creare un criterio di sicurezza aggiungendo la funzione sia come predicato di filtro e predicato di blocco nella tabella:  

```
CREATE SECURITY POLICY SalesFilter   
ADD FILTER PREDICATE Security.fn_securitypredicate(SalesPersonID)    
  ON Sales.SalesOrderHeader,   
ADD BLOCK PREDICATE Security.fn_securitypredicate(SalesPersonID)    
  ON Sales.SalesOrderHeader   
WITH (STATE = ON);   
```

Eseguire una query sulla tabella `SalesOrderHeader` come ogni utente usando il comando riportato di seguito. Verificare che `SalesPerson280` veda solo le 95 righe relative alle proprie vendite e che `Manager` possa visualizzare tutte le righe della tabella.  

```    
EXECUTE AS USER = 'SalesPerson280';   
SELECT * FROM Sales.SalesOrderHeader;    
REVERT; 
 
EXECUTE AS USER = 'Manager';   
SELECT * FROM Sales.SalesOrderHeader;   
REVERT;   
```
 
Modificare i criteri di sicurezza per disabilitarli.  Ora entrambi gli utenti possono accedere a tutte le righe. 

```
ALTER SECURITY POLICY SalesFilter   
WITH (STATE = OFF);    
``` 


## <a name="enable-dynamic-data-masking"></a>Abilitare il mascheramento dinamico dei dati

[Dynamic Data Masking](../relational-databases/security/dynamic-data-masking.md) consente di limitare l'esposizione dei dati sensibili agli utenti di un'applicazione usando il mascheramento completo o parziale di determinate colonne. 

Usare un'istruzione `ALTER TABLE` per aggiungere una funzione di mascheramento alla colonna `EmailAddress` della tabella `Person.EmailAddress`: 
 
```
USE AdventureWorks2014;
GO
ALTER TABLE Person.EmailAddress    
ALTER COLUMN EmailAddress    
ADD MASKED WITH (FUNCTION = 'email()');
``` 
 
Creare un nuovo utente `TestUser` con autorizzazione `SELECT` per la tabella, quindi eseguire una query come `TestUser` per visualizzare i dati mascherati:   

```  
CREATE USER TestUser WITHOUT LOGIN;   
GRANT SELECT ON Person.EmailAddress TO TestUser;    
 
EXECUTE AS USER = 'TestUser';   
SELECT EmailAddressID, EmailAddress FROM Person.EmailAddress;       
REVERT;    
```
 
Verificare che la funzione di mascheramento cambi l'indirizzo di posta elettronica nel primo record da:
  
|EmailAddressID |EmailAddress |  
|----|---- |   
|1 |ken0@adventure-works.com |    
 
into 

|EmailAddressID |EmailAddress |  
|----|---- |   
|1 |kXXX@XXXX.com |   


## <a name="enable-transparent-data-encryption"></a>Abilitare Transparent Data Encryption

Una minaccia per il database è il rischio che qualcuno rubi i file di database dal disco rigido. Questo problema può verificarsi con un'intrusione che ottiene l'accesso con privilegi elevati al sistema, sfruttando le azioni di un dipendente problematico o con il furto del computer che contiene i file, ad esempio un portatile.

Transparent Data Encryption (TDE) crittografa i file di dati archiviati nel disco rigido. Il database master del motore di database di SQL Server ha la chiave di crittografia, in modo che il motore di database possa modificare i dati. Non è possibile leggere i file di database senza accedere alla chiave. Gli amministratori di livello elevato possono gestire la chiave, eseguirne il backup e ricrearla, in modo che il database possa essere spostato, ma solo da utenti selezionati. Quando si configura Transparent Data Encryption, anche il database `tempdb` viene crittografato automaticamente. 

Poiché il motore di database è in grado di leggere i dati, Transparent Data Encryption non protegge dall'accesso non autorizzato da parte degli amministratori del computer che possono leggere direttamente la memoria o accedere a SQL Server usando un account amministratore.

### <a name="configure-tde"></a>Configurare Transparent Data Encryption

- Creare una chiave master
- Creare o ottenere un certificato protetto dalla chiave master
- Creare una chiave di crittografia del database e proteggerla mediante il certificato
- Impostare il database per l'uso della crittografia

La configurazione di Transparent Data Encryption richiede l'autorizzazione `CONTROL` per il database master e l'autorizzazione `CONTROL` per il database utente. In genere, un amministratore configura Transparent Data Encryption. 

L'esempio seguente illustra come crittografare e decrittografare il database `AdventureWorks2014` usando un certificato installato nel server denominato `MyServerCert`.


```
USE master;  
GO  

CREATE MASTER KEY ENCRYPTION BY PASSWORD = '**********';  
GO  

CREATE CERTIFICATE MyServerCert WITH SUBJECT = 'My Database Encryption Key Certificate';  
GO  

USE AdventureWorks2014;  
GO
  
CREATE DATABASE ENCRYPTION KEY  
WITH ALGORITHM = AES_256  
ENCRYPTION BY SERVER CERTIFICATE MyServerCert;  
GO
  
ALTER DATABASE AdventureWorks2014  
SET ENCRYPTION ON;   
```

Per rimuovere Transparent Data Encryption, eseguire `ALTER DATABASE AdventureWorks2014 SET ENCRYPTION OFF;`   

Le operazioni di crittografia e decrittografia sono pianificate sui thread di background da SQL Server. Per visualizzare lo stato di queste operazioni, è possibile usare le viste del catalogo e le viste a gestione dinamica nell'elenco illustrato di seguito in questo argomento.   

> [!WARNING]
>  I file di backup dei database in cui è abilitata la funzionalità TDE vengono crittografati anche tramite la chiave di crittografia del database. Di conseguenza, quando questi backup vengono ripristinati, è necessario disporre del certificato che protegge la chiave di crittografia del database. Pertanto, oltre ad eseguire il backup del database, è necessario assicurarsi di conservare un backup dei certificati server per impedire la perdita di dati. Se il certificato non è più disponibile, si verificherà la perdita di dati. Per altre informazioni, vedere [SQL Server Certificates and Asymmetric Keys](../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  

Per altre informazioni su Transparent Data Encryption, vedere [Transparent Data Encryption (TDE)](../relational-databases/security/encryption/transparent-data-encryption.md).   


## <a name="configure-backup-encryption"></a>Configurare la crittografia di backup
SQL Server permette di crittografare i dati durante la creazione di un backup. Specificando l'algoritmo di crittografia e il componente di crittografia (certificato o chiave asimmetrica) durante la creazione di un backup, è possibile creare un file di backup crittografato.    
  
> [!WARNING]
> È molto importante eseguire il backup del certificato o della chiave asimmetrica e preferibilmente in un percorso diverso dal file di backup utilizzato per la crittografia. Senza il certificato o la chiave asimmetrica, non è possibile ripristinare il backup, rendendo il file di backup inutilizzabile. 
 
 
Nell'esempio seguente viene creato un certificato, quindi un backup protetto dal certificato.

```
USE master;  
GO  
CREATE CERTIFICATE BackupEncryptCert   
   WITH SUBJECT = 'Database backups';  
GO 
BACKUP DATABASE [AdventureWorks2014]  
TO DISK = N'/var/opt/mssql/backups/AdventureWorks2014.bak'  
WITH  
  COMPRESSION,  
  ENCRYPTION   
   (  
   ALGORITHM = AES_256,  
   SERVER CERTIFICATE = BackupEncryptCert  
   ),  
  STATS = 10  
GO  
```

Per altre informazioni, vedere [Crittografia del backup](../relational-databases/backup-restore/backup-encryption.md).


## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulle funzionalità di sicurezza di SQL Server, vedere il [Centro sicurezza per il motore di database di SQL Server e il database SQL di Azure](../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md).