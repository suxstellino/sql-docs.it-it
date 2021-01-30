---
description: CREATE COLUMN MASTER KEY (Transact-SQL)
title: CREATE COLUMN MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/15/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SQL13.SWB.NEWCOLUMNMASTERKEYDEF.GENERAL.F1
- SQL13.SWB.COLUMNMASTERKEYDEF.GENERAL.F1
- CREATE COLUMN MASTER KEY
- COLUMN MASTER KEY
- CREATE_COLUMN_MASTER_KEY_TSQL
- COLUMN_MASTER_KEY_TSQL
- SQL13.SWB.NEWCOLUMNMASTERKEY.GENERAL.F1
- SQL13.SWB.COLUMNMASTERKEY.GENERAL.F1
dev_langs:
- TSQL
helpviewer_keywords:
- column master key definition
- column master key, create
- CREATE COLUMN MASTER KEY statement
- Always Encrypted, create column master key
ms.assetid: f8926b95-e146-4e3f-b56b-add0c0d0a30e
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 6e18a1e0721428dcbfdb649be46ca83fd0d42972
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204937"
---
# <a name="create-column-master-key-transact-sql"></a>CREATE COLUMN MASTER KEY (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


Crea un oggetto metadati chiave master della colonna nel database. Una voce di metadati della chiave master di colonna rappresenta una chiave archiviata in un archivio chiavi esterno. Questa chiave protegge (crittografa) le chiavi di crittografia della colonna quando si usa [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md) o [Always Encrypted con enclavi sicuri](../../relational-databases/security/encryption/always-encrypted-enclaves.md). Più chiavi master di colonna consentono la rotazione periodica delle chiavi per migliorare la sicurezza. Creare una chiave master di colonna in un archivio chiavi e l'oggetto metadati correlato nel database usando Esplora oggetti in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o PowerShell. Per i dettagli, vedere [Panoramica della gestione delle chiavi per Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
 

> [!IMPORTANT]
> La creazione di chiavi basate su enclave (con ENCLAVE_COMPUTATIONS) richiede [Always Encrypted con enclave sicuri](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

## <a name="syntax"></a>Sintassi  

```syntaxsql
CREATE COLUMN MASTER KEY key_name   
    WITH (  
        KEY_STORE_PROVIDER_NAME = 'key_store_provider_name',  
        KEY_PATH = 'key_path'   
        [,ENCLAVE_COMPUTATIONS (SIGNATURE = signature)]
         )   
[;]  
```  

## <a name="arguments"></a>Argomenti
*key_name*  
Nome della chiave master di colonna nel database.  
  
*key_store_provider_name*  
Specifica il nome di un provider dell'archivio chiavi. Un provider dell'archivio chiavi è un componente software lato client che contiene un archivio chiavi con la chiave master di colonna. 

Un driver client con la funzionalità Always Encrypted abilitata:

- Usa il nome di un provider dell'archivio chiavi 
- Cerca il provider dell'archivio chiavi nel proprio registro di provider dell'archivio chiavi 

Il driver usa quindi il provider per decrittografare le chiavi di crittografia di colonna. Le chiavi di crittografia di colonna sono protette da una chiave master di colonna, archiviata nell'archivio chiavi sottostante. Un valore di testo non crittografato della chiave di crittografia di colonna viene quindi usato per crittografare i parametri di query corrispondenti alle colonne del database crittografate oppure la chiave di crittografia di colonna decrittografa i risultati delle query delle colonne crittografate.  
  
Le librerie dei driver client abilitati per Always Encrypted includono provider dell'archivio chiavi per gli archivi chiavi più diffusi.   
  
Il set dei provider disponibili dipende dal tipo e dalla versione del driver client. Per informazioni su driver specifici, vedere la documentazione su Always Encrypted: [Sviluppare applicazioni usando Always Encrypted](../../relational-databases/security/encryption/always-encrypted-client-development.md).


La tabella seguente contiene i nomi dei provider di sistema:  
  
|Nome provider archivio chiavi|Archivio chiavi sottostante|  
    |-----------------------------|--------------------------|
    |'MSSQL_CERTIFICATE_STORE'|Archivio certificati Windows| 
    |'MSSQL_CSP_PROVIDER'|Archivio, ad esempio un modulo di protezione hardware, che supporta la CryptoAPI Microsoft.|
    |'MSSQL_CNG_STORE'|Archivio, ad esempio un modulo di protezione hardware, che supporta l'API Cryptography Next Generation di Microsoft.|  
    |'AZURE_KEY_VAULT'|[Introduzione a Azure Key Vault](/azure/key-vault/general/overview)|  
    |'MSSQL_JAVA_KEYSTORE'| Java Key Store.}
  

Nel driver client abilitato per Always Encrypted è possibile configurare un provider dell'archivio chiavi personalizzato per archiviare le chiavi master di colonna per cui non è disponibile un provider dell'archivio chiavi predefinito. I nomi dei provider dell'archivio chiavi personalizzati non possono iniziare con "MSSQL_", perché è un prefisso riservato per i provider dell'archivio chiavi [!INCLUDE[msCoName](../../includes/msconame-md.md)]. 

  
key_path  
Percorso della chiave nell'archivio chiavi master della colonna. Il percorso della chiave deve essere valido per ogni applicazione client che dovrà crittografare o decrittografare i dati. I dati vengono archiviati in una colonna protetta (indirettamente) dalla chiave master di colonna a cui viene fatto riferimento. L'applicazione client deve avere accesso alla chiave. Il formato del percorso della chiave è specifico del provider dell'archivio chiavi. L'elenco seguente descrive il formato dei percorsi delle chiavi per provider dell'archivio chiavi di sistema Microsoft specifici.  
  
-   **Nome provider:** MSSQL_CERTIFICATE_STORE  
  
    **Formato del percorso della chiave:** *CertificateStoreName*/*CertificateStoreLocation*/*CertificateThumbprint*  
  
     Dove:  
  
    *CertificateStoreLocation*  
    Posizione dell'archivio certificati, che deve essere Utente corrente o Computer locale. Per altre informazioni, vedere [Local Machine and Current User Certificate Stores](/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores) (Archivi certificati Computer locale e Utente corrente).  
  
    *CertificateStore*  
    Nome dell'archivio certificati, ad esempio 'My'.  
  
    *CertificateThumbprint*  
    Identificazione personale del certificato.  
  
    **Esempi:**  
  
    ```  
    N'CurrentUser/My/BBF037EC4A133ADCA89FFAEC16CA5BFA8878FB94'  
  
    N'LocalMachine/My/CA5BFA8878FB94BBF037EC4A133ADCA89FFAEC16'  
    ```  
  
-   **Nome del provider:** MSSQL_CSP_PROVIDER  
  
    **Formato del percorso della chiave:** *ProviderName*/*KeyIdentifier*  
  
    Dove:  
  
    *ProviderName*  
    Nome di un provider del servizio di crittografia (CSP) che implementa CAPI, per l'archivio chiavi master di colonna. Se si usa un modulo di protezione hardware come archivio chiavi, deve essere il nome del CSP offerto dal fornitore del modulo di protezione hardware. Il provider deve essere installato in un computer client.  
  
    *KeyIdentifier*  
    Identificatore della chiave, usato come chiave master della colonna, nell'archivio chiavi.  
  
    **Esempi:**  
  
    ```  
    N'My HSM CSP Provider/AlwaysEncryptedKey1'  
    ```  
  
-   **Nome del provider:** MSSQL_CNG_STORE  
  
    **Formato del percorso della chiave:** *ProviderName*/*KeyIdentifier*  
  
    Dove:  
  
    *ProviderName*  
    Nome del provider di archiviazione chiavi, che implementa l'API Cryptography Next Generation (CNG), per l'archivio chiavi master della colonna. Se si usa un modulo di protezione hardware come archivio chiavi, deve essere il nome del provider di archiviazione chiavi offerto dal fornitore del modulo di protezione hardware. Il provider deve essere installato in un computer client.  
  
    *KeyIdentifier*  
    Identificatore della chiave, usato come chiave master della colonna, nell'archivio chiavi.  
  
    **Esempi:**  
  
    ```  
    N'My HSM CNG Provider/AlwaysEncryptedKey1'  
    ```  

-   **Nome del provider:** AZURE_KEY_STORE  
  
    **Formato del percorso della chiave:** *KeyUrl*  
  
    Dove:  
  
    *KeyUrl*  
    URL della chiave in Azure Key Vault

ENCLAVE_COMPUTATIONS  
Specifica che la chiave master di colonna è abilitata per l'enclave. È possibile condividere tutte le chiavi di crittografia di colonna, crittografate con la chiave master di colonna, con un enclave sicuro sul lato server e usarle per calcoli all'interno dell'enclave. Per altre informazioni, vedere [Always Encrypted con enclave sicuri](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

*URL REST*  
Valore letterale binario risultante dalla firma digitale del *percorso della chiave* e dall'impostazione di ENCLAVE_COMPUTATIONS con la chiave master di colonna. La firma riflette se ENCLAVE_COMPUTATIONS è stato specificato o meno. La firma consente di proteggere i valori firmati dalla modifica da parte di utenti non autorizzati. Un driver client abilitato per Always Encrypted verifica la firma e restituisce un errore all'applicazione se la firma non è valida. La firma deve essere generata usando gli strumenti lato client. Per altre informazioni, vedere [Always Encrypted con enclave sicuri](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

## <a name="remarks"></a>Osservazioni

Creare una voce di metadati della chiave master di colonna prima di creare una voce di metadati della chiave di crittografia di colonna nel database e prima che qualsiasi colonna nel database possa essere crittografata con Always Encrypted. Una voce della chiave master di colonna nei metadati non contiene la chiave master di colonna effettiva. La chiave master di colonna deve essere archiviata in un archivio chiavi di colonna esterno, ossia all'esterno di SQL Server. Il nome del provider dell'archivio chiavi e il percorso della chiave master di colonna nei metadati devono essere validi per un'applicazione client. L'applicazione client deve usare la chiave master di colonna per decrittografare una chiave di crittografia di colonna. La chiave di crittografia di colonna viene crittografata con la chiave master di colonna. L'applicazione client deve anche eseguire query sulle colonne crittografate.

Per gestire le chiavi master della colonna, si consiglia di usare strumenti quali SQL Server Management Studio (SSMS) o PowerShell. Questi strumenti generano firme (se si usa Always Encrypted con enclave sicuri) ed emettono automaticamente istruzioni `CREATE COLUMN MASTER KEY` per creare oggetti metadati delle chiavi di crittografia della colonna. Vedere [Effettuare il provisioning di chiavi Always Encrypted usando SQL Server Management Studio](../../relational-databases/security/encryption/configure-always-encrypted-keys-using-ssms.md) ed [Effettuare il provisioning di chiavi Always Encrypted con PowerShell](../../relational-databases/security/encryption/configure-always-encrypted-keys-using-powershell.md). 

  
## <a name="permissions"></a>Autorizzazioni  
È necessaria l'autorizzazione **ALTER ANY COLUMN MASTER KEY**.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-column-master-key"></a>R. Creazione di una chiave master della colonna  
L'esempio seguente crea una voce di metadati per una chiave master di colonna. La chiave master di colonna viene archiviata nell'archivio certificati per consentire alle applicazioni client che usano il provider MSSQL_CERTIFICATE_STORE di accedere alla chiave master di colonna:  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
     KEY_STORE_PROVIDER_NAME = N'MSSQL_CERTIFICATE_STORE',   
     KEY_PATH = 'Current User/Personal/f2260f28d909d21c642a3d8e0b45a830e79a1420'  
   );  
```  
  
Creare una voce di metadati per una chiave master di colonna. Alla chiave master di colonna hanno accesso le applicazioni client che usano il provider MSSQL_CNG_STORE:  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = N'MSSQL_CNG_STORE',    
    KEY_PATH = N'My HSM CNG Provider/AlwaysEncryptedKey'  
);  
```  
  
Creare una voce di metadati per una chiave master di colonna. La chiave master di colonna viene archiviata in Azure Key Vault per consentire alle applicazioni client che usano il provider AZURE_KEY_VAULT di accedere alla chiave master di colonna.  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = N'AZURE_KEY_VAULT',  
    KEY_PATH = N'https://myvault.vault.azure.net:443/keys/  
        MyCMK/4c05f1a41b12488f9cba2ea964b6a700');  
```  
  
Creare una voce di metadati per una chiave master di colonna. La chiave master di colonna viene archiviata in un archivio chiavi master di colonna personalizzato:  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = 'CUSTOM_KEY_STORE',    
    KEY_PATH = 'https://contoso.vault/sales_db_tce_key'  
);  
```  
### <a name="b-creating-an-enclave-enabled-column-master-key"></a>B. Creazione di una chiave master della colonna abilitata da enclave  
L'esempio seguente crea una voce di metadati per una chiave master di colonna abilitata per l'enclave. La chiave master di colonna abilitata per l'enclave viene archiviata in un archivio certificati per consentire alle applicazioni client che usano il provider MSSQL_CERTIFICATE_STORE di accedere alla chiave master di colonna:  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
     KEY_STORE_PROVIDER_NAME = N'MSSQL_CERTIFICATE_STORE',   
     KEY_PATH = 'Current User/Personal/f2260f28d909d21c642a3d8e0b45a830e79a1420'  
     ENCLAVE_COMPUTATIONS (SIGNATURE = 0xA80F5B123F5E092FFBD6014FC2226D792746468C901D9404938E9F5A0972F38DADBC9FCBA94D9E740F3339754991B6CE26543DEB0D094D8A2FFE8B43F0C7061A1FFF65E30FDDF39A1B954F5BA206AAC3260B0657232020542419990261D878318CC38EF4E853970ED69A8D4A306693B8659AAC1C4E4109DE5EB148FD0E1FDBBC32F002C1D8199D313227AD689279D8DEEF91064DF122C19C3767C463723AB663A6F8412AE17E745922C0E3A257EAEF215532588ACCBD440A03C7BC100A38BD0609A119E1EF7C5C6F1B086C68AB8873DBC6487B270340E868F9203661AFF0492CEC436ABF7C4713CE64E38CF66C794B55636BFA55E5B6554AF570CF73F1BE1DBD)
  );  
```  
  
Creare una voce di metadati per una chiave master di colonna abilitata per l'enclave. La chiave master di colonna abilitata per l'enclave viene archiviata in Azure Key Vault per consentire alle applicazioni client che usano il provider AZURE_KEY_VAULT di accedere alla chiave master di colonna.  
  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = N'AZURE_KEY_VAULT',  
    KEY_PATH = N'https://myvault.vault.azure.net:443/keys/MyCMK/4c05f1a41b12488f9cba2ea964b6a700');
    ENCLAVE_COMPUTATIONS (SIGNATURE = 0xA80F5B123F5E092FFBD6014FC2226D792746468C901D9404938E9F5A0972F38DADBC9FCBA94D9E740F3339754991B6CE26543DEB0D094D8A2FFE8B43F0C7061A1FFF65E30FDDF39A1B954F5BA206AAC3260B0657232020582413990261D878318CC38EF4E853970ED69A8D4A306693B8659AAC1C4E4109DE5EB148FD0E1FDBBC32F002C1D8199D313227AD689279D8DEEF91064DF122C19C3767C463723AB663A6F8412AE17E745922C0E3A257EAEF215532588ACCBD440A03C7BC100A38BD0609A119E1EF7C5C6F1B086C68AB8873DBC6487B270340E868F9203661AFF0492CEC436ABF7C4713CE64E38CF66C794B55636BFA55E5B6554AF570CF73F1BE1DBD)
  );
```  
  
## <a name="see-also"></a>Vedere anche
 
* [DROP COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-column-master-key-transact-sql.md)   
* [CREATE COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-column-encryption-key-transact-sql.md)
* [sys.column_master_keys (Transact-SQL)](../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
* [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
* [Always Encrypted con enclave sicuri](../../relational-databases/security/encryption/always-encrypted-enclaves.md)   
* [Panoramica della gestione delle chiavi per Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)   
* [Gestire le chiavi per Always Encrypted con enclave sicuri](../../relational-databases/security/encryption/always-encrypted-enclaves-manage-keys.md)   
