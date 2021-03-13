---
description: CREATE ASYMMETRIC KEY (Transact-SQL)
title: CREATE ASYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ASYMMETRIC_KEY_TSQL
- CREATE ASYMMETRIC KEY
- CREATE_ASYMMETRIC_KEY_TSQL
- ASYMMETRIC KEY
dev_langs:
- TSQL
helpviewer_keywords:
- asymmetric keys [SQL Server], creating
- encryption [SQL Server], asymmetric keys
- CREATE ASYMMETRIC KEY statement
- asymmetric keys [SQL Server]
- cryptography [SQL Server], asymmetric keys
ms.assetid: 141bc976-7631-49f6-82bd-a235028645b1
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: 1d0f2513ff1d81f1611eac5616ab8f1dbacfe07e
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421949"
---
# <a name="create-asymmetric-key-transact-sql"></a>CREATE ASYMMETRIC KEY (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  Crea una chiave asimmetrica nel database.  
  
 Questa funzionalità non è compatibile con l'esportazione del database mediante Data Tier Application Framework (DACFx). Prima dell'esportazione, è necessario eliminare tutte le chiavi asimmetriche.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)]  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CREATE ASYMMETRIC KEY asym_key_name   
   [ AUTHORIZATION database_principal_name ]  
   [ FROM <asym_key_source> ]  
   [ WITH <key_option> ] 
   [ ENCRYPTION BY <encrypting_mechanism> ] 
   [ ; ]
  
<asym_key_source>::=  
     FILE = 'path_to_strong-name_file'  
   | EXECUTABLE FILE = 'path_to_executable_file'  
   | ASSEMBLY assembly_name  
   | PROVIDER provider_name  
  
<key_option> ::=  
   ALGORITHM = <algorithm>  
      |  
   PROVIDER_KEY_NAME = 'key_name_in_provider'  
      |  
      CREATION_DISPOSITION = { CREATE_NEW | OPEN_EXISTING }  
  
<algorithm> ::=  
      { RSA_4096 | RSA_3072 | RSA_2048 | RSA_1024 | RSA_512 }   
  
<encrypting_mechanism> ::=  
    PASSWORD = 'password'   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *asym_key_name*  
 Nome della chiave asimmetrica nel database. I nomi di chiave asimmetrica devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md) e devono essere univoci all'interno del database.  

 AUTHORIZATION *database_principal_name*  
 Specifica il proprietario della chiave asimmetrica. Il proprietario non può essere un ruolo o un gruppo. Se l'opzione viene omessa, il proprietario sarà l'utente corrente.  
  
 FROM *asym_key_source*  
 Specifica l'origine da cui caricare la coppia di chiavi asimmetriche.  
  
 FILE = '*path_to_strong-name_file*'  
 Specifica il percorso di un file con nome sicuro da cui caricare la coppia di chiavi. Limitato a 260 caratteri da MAX_PATH dall'API Windows.  
  
> [!NOTE]  
>  Questa opzione non è disponibile in un database indipendente.  
  
 EXECUTABLE FILE = '*path_to_executable_file*'  
 Specifica il percorso di un file di assembly da cui caricare la chiave pubblica. Limitato a 260 caratteri da MAX_PATH dall'API Windows.  
  
> [!NOTE]  
>  Questa opzione non è disponibile in un database indipendente.  
  
 ASSEMBLY *assembly_name*  
 Specifica il nome di un assembly firmato già caricato nel database da cui caricare la chiave pubblica.  
  
 PROVIDER *provider_name*  
 Specifica il nome di un provider EKM (Extensible Key Management). È necessario innanzitutto definire il provider utilizzando l'istruzione CREATE PROVIDER. Per altre informazioni sulla gestione delle chiavi esterne, vedere [Extensible Key Management &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md).  
  
 ALGORITHM = \<algorithm>  
 Possono essere specificati cinque algoritmi: RSA_4096, RSA_3072, RSA_2048, RSA_1024 e RSA_512.  
  
 RSA_1024 e RSA_512 sono deprecati. Per usare RSA_1024 o RSA_512 (sconsigliato), è necessario impostare il database sul livello di compatibilità del database 120 o su uno inferiore.  
  
 PROVIDER_KEY_NAME = '*key_name_in_provider*'  
 Specifica il nome della chiave dal provider esterno.  
  
 CREATION_DISPOSITION = CREATE_NEW  
 Crea una nuova chiave nel dispositivo EKM. È necessario usare PROV_KEY_NAME per specificare il nome della chiave nel dispositivo. Se nel dispositivo esiste già una chiave, l'istruzione genererà un errore.  
  
 CREATION_DISPOSITION = OPEN_EXISTING  
 Definisce il mapping di una chiave asimmetrica di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a una chiave EKM esistente. È necessario usare PROV_KEY_NAME per specificare il nome della chiave nel dispositivo. Se CREATION_DISPOSITION = OPEN_EXISTING non è specificato, l'impostazione predefinita è CREATE_NEW.  
  
 ENCRYPTION BY PASSWORD = '*password*'  
 Specifica la password con cui crittografare la chiave privata. Se questa clausola è assente, la chiave privata verrà crittografata con la chiave master del database. *password* ha un massimo di 128 caratteri. *password* deve soddisfare i requisiti per i criteri password di Windows del computer che sta eseguendo l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="remarks"></a>Osservazioni  
 Una *chiave asimmetrica* è un'entità a protezione diretta a livello del database. Nella forma predefinita, questa entità contiene sia una chiave pubblica che una chiave privata. Se eseguita senza la clausola FROM, l'istruzione CREATE ASYMMETRIC KEY genera una nuova coppia di chiavi. Se eseguita con la clausola FROM, l'istruzione CREATE ASYMMETRIC KEY importa una coppia di chiavi da un file o importa una chiave pubblica da un assembly o da un file di DLL.  
  
 Per impostazione predefinita, la chiave privata è protetta dalla chiave master del database. Se non esiste una chiave master del database, è necessario proteggere la chiave privata con una password.  
  
 La chiave privata può avere una lunghezza di 512, 1024 o 2048 bit.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CREATE ASYMMETRIC KEY per il database. Se si specifica la clausola AUTHORIZATION, è richiesta l'autorizzazione IMPERSONATE per l'entità di database o l'autorizzazione ALTER per il ruolo applicazione. Solo gli account di accesso di Windows e di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e i ruoli applicazione possono disporre di chiavi asimmetriche. I gruppi e i ruoli non possono disporre di chiavi asimmetriche.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-an-asymmetric-key"></a>R. Creazione di una chiave asimmetrica  
 Nell'esempio seguente viene creata una chiave asimmetrica denominata `PacificSales09` tramite l'algoritmo `RSA_2048` e la chiave privata viene protetta con una password.  
  
```sql  
CREATE ASYMMETRIC KEY PacificSales09   
    WITH ALGORITHM = RSA_2048   
    ENCRYPTION BY PASSWORD = '<enterStrongPasswordHere>';   
GO  
```  
  
### <a name="b-creating-an-asymmetric-key-from-a-file-giving-authorization-to-a-user"></a>B. Creazione di una chiave asimmetrica da un file e concessione dell'autorizzazione a un utente  
 L'esempio seguente crea la chiave asimmetrica `PacificSales19` da una coppia di chiavi archiviata in un file e assegna la proprietà della chiave asimmetrica all'utente `Christina`. La chiave privata è protetta dalla chiave master del database, che deve essere creata prima della chiave asimmetrica.  
  
```sql  
CREATE ASYMMETRIC KEY PacificSales19  
    AUTHORIZATION Christina  
    FROM FILE = 'c:\PacSales\Managers\ChristinaCerts.tmp';  
GO  
```  
  
### <a name="c-creating-an-asymmetric-key-from-an-ekm-provider"></a>C. Creazione di una chiave asimmetrica da un provider EKM  
 L'esempio seguente crea la chiave asimmetrica `EKM_askey1` da una coppia di chiavi archiviata in un provider EKM denominato `EKM_Provider1`. Crea anche una chiave in tale provider denominata `key10_user1`.  
  
```sql  
CREATE ASYMMETRIC KEY EKM_askey1   
    FROM PROVIDER EKM_Provider1  
    WITH   
        ALGORITHM = RSA_2048,   
        CREATION_DISPOSITION = CREATE_NEW  
        , PROVIDER_KEY_NAME  = 'key10_user1' ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-asymmetric-key-transact-sql.md)  
 [DROP ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-asymmetric-key-transact-sql.md)  
 [ASYMKEYPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/asymkeyproperty-transact-sql.md)  
 [ASYMKEY_ID &#40;Transact-SQL&#41;](../../t-sql/functions/asymkey-id-transact-sql.md)  
 [Scelta di un algoritmo di crittografia](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)  
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)  
 [Extensible Key Management con l'insieme di credenziali delle chiavi di Azure &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
  
  
