---
title: Crittografia Always Encrypted | Microsoft Docs
description: Informazioni sugli algoritmi e i meccanismi di crittografia necessari per derivare il materiale crittografico usato dalla funzionalità Always Encrypted in SQL Server e nel database SQL di Azure.
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Always Encrypted, cryptography system
ms.assetid: ae8226ff-0853-4716-be7b-673ce77dd370
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0da1e6ba00c86bbc391d098ab7cf8b9b218d60e1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344385"
---
# <a name="always-encrypted-cryptography"></a>Crittografia Always Encrypted
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

  Il documento descrive gli algoritmi e i meccanismi di crittografia necessari per derivare il materiale crittografico usato dalla funzionalità [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)].  
  
## <a name="keys-key-stores-and-key-encryption-algorithms"></a>Chiavi, archivi di chiavi e algoritmi di crittografia delle chiavi
 Always Encrypted usa due tipi di chiavi: chiavi master di colonna e chiavi di crittografia di colonna.  
  
 La chiave CMK (Column Master Key, chiave master della colonna) è una chiave usata per crittografare altre chiavi. Resta sempre sotto il controllo del client ed è memorizzata in un archivio di chiavi esterno. Il driver client abilitato per Always Encrypted interagisce con l'archivio delle chiavi tramite un provider di archiviazione CMK, che può far parte sia della libreria dei driver (provider [!INCLUDE[msCoName](../../../includes/msconame-md.md)]/di sistema) che dell'applicazione client (provider personalizzato). Al momento, le librerie di driver client includono provider di archiviazione chiavi [!INCLUDE[msCoName](../../../includes/msconame-md.md)] per l' [archivio certificati Windows](/windows/desktop/SecCrypto/using-certificate-stores) e moduli di protezione hardware. Per l'elenco aggiornato dei provider, vedere [CREATE COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-column-master-key-transact-sql.md). Uno sviluppatore di applicazioni può realizzare un provider personalizzato per un archivio arbitrario.  
  
 La chiave CEK (Column Encryption Key, chiave di crittografia della colonna) è una chiave di crittografia del contenuto, ovvero una chiave usata per proteggere i dati, protetta da una chiave CMK.  
  
 Tutti i provider di archiviazione CMK di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] eseguono la crittografia delle chiavi CEK con riempimento RSA-OAEP. Il provider dell'archivio chiavi che supporta l'API Cryptography Next Generation (CNG) in .NET Framework ([classe SqlColumnEncryptionCngProvider](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncngprovider)) usa i parametri predefiniti specificati da RFC 8017 nella sezione A.2.1. Tali parametri predefiniti usano una funzione hash di SHA-1 e una funzione di generazione della maschera MGF1 con SHA-1. Tutti gli altri provider di archivi di chiavi usano SHA-256. 

Always Encrypted usa internamente i moduli di crittografia FIPS 140-2 convalidati. 

## <a name="data-encryption-algorithm"></a>Algoritmo di crittografia dei dati  
 La funzionalità Always Encrypted usa l'algoritmo **AEAD_AES_256_CBC_HMAC_SHA_256** per crittografare i dati nel database.  
  
 **AEAD_AES_256_CBC_HMAC_SHA_256** deriva dalla bozza della specifica illustrata qui [https://tools.ietf.org/html/draft-mcgrew-aead-aes-cbc-hmac-sha2-05](https://tools.ietf.org/html/draft-mcgrew-aead-aes-cbc-hmac-sha2-05). L’algoritmo usa uno schema di crittografia autenticata con dati associati che adotta l’approccio Encrypt-then-MAC. Tale approccio prevede prima la crittografia del testo non crittografato e quindi la generazione del MAC in base al testo crittografato risultante.  
  
 Per nascondere i modelli, l'algoritmo **AEAD_AES_256_CBC_HMAC_SHA_256** usa la modalità operativa CBC (Cipher Block Chaining), che prevede l'immissione nel sistema di un valore iniziale denominato IV (vettore di inizializzazione). La descrizione completa della modalità CBC è reperibile nella pagina [https://csrc.nist.gov/publications/nistpubs/800-38a/sp800-38a.pdf](https://csrc.nist.gov/publications/nistpubs/800-38a/sp800-38a.pdf).  
  
 L'algoritmo **AEAD_AES_256_CBC_HMAC_SHA_256** calcola il valore del testo crittografato per un determinato valore del testo non crittografato con la procedura seguente.  
  
### <a name="step-1-generating-the-initialization-vector-iv"></a>Passaggio 1: Generazione del vettore di inizializzazione (IV)  
 La funzionalità Always Encrypted supporta due varianti di **AEAD_AES_256_CBC_HMAC_SHA_256**:  
  
-   Casuale  
  
-   Deterministico  
  
 Nella crittografia casuale l’IV viene generato in modo casuale. Di conseguenza, quando viene crittografato lo stesso testo non crittografato, viene generato un testo crittografato diverso, impedendo così l'intercettazione delle informazioni.  
  
```  
When using randomized encryption: IV = Generate cryptographicaly random 128bits  
```  
  
 Nella crittografia deterministica l'IV non è generato casualmente ma viene derivato dal valore del testo non crittografato usando l'algoritmo seguente:  
  
```  
When using deterministic encryption: IV = HMAC-SHA-256( iv_key, cell_data ) truncated to 128 bits.  
```  
  
 Dove iv_key è derivato dalla chiave CEK come indicato di seguito:  
  
```  
iv_key = HMAC-SHA-256(CEK, "Microsoft SQL Server cell IV key" + algorithm + CEK_length)  
```  
  
 Il troncamento del valore HMAC viene eseguito per riempire un blocco di dati come necessario per l'IV.
Di conseguenza, la crittografia deterministica genera sempre lo stesso testo crittografato per un determinato testo non crittografato, consentendo di dedurre se due valori di testo non crittografato sono uguali confrontando i relativi valori del testo crittografato. Questa limitata intercettazione delle informazioni consente al sistema di database di supportare il confronto delle uguaglianze per i valori delle colonne crittografate.  
  
 La crittografia deterministica è più efficace nel nascondere i modelli rispetto ad alternative quali, ad esempio, l’uso di un valore IV predefinito.  
  
### <a name="step-2-computing-aes_256_cbc-ciphertext"></a>Passaggio 2: Calcolo del testo crittografato AES_256_CBC  
 Dopo avere calcolato l'IV viene generato il testo crittografato **AES_256_CBC** :  
  
```  
aes_256_cbc_ciphertext = AES-CBC-256(enc_key, IV, cell_data) with PKCS7 padding.  
```  
  
 Dove la chiave di crittografia (enc_key) è derivata dalla chiave CEK come segue.  
  
```  
enc_key = HMAC-SHA-256(CEK, "Microsoft SQL Server cell encryption key" + algorithm + CEK_length )  
```  
  
### <a name="step-3-computing-mac"></a>Passaggio 3: Calcolo del MAC  
 Successivamente, il MAC viene calcolato con l'algoritmo seguente:  
  
```  
MAC = HMAC-SHA-256(mac_key, versionbyte + IV + Ciphertext + versionbyte_length)  
```  
  
 Dove:  
  
```  
versionbyte = 0x01 and versionbyte_length = 1
mac_key = HMAC-SHA-256(CEK, "Microsoft SQL Server cell MAC key" + algorithm + CEK_length)  
```  
  
### <a name="step-4-concatenation"></a>Passaggio 4: Concatenazione  
 Infine, il valore crittografato è generato concatenando il byte della versione dell'algoritmo, il MAC, l'IV e il testo crittografato AES_256_CBC:  
  
```  
aead_aes_256_cbc_hmac_sha_256 = versionbyte + MAC + IV + aes_256_cbc_ciphertext  
```  
  
## <a name="ciphertext-length"></a>Lunghezza del testo crittografato  
 La lunghezza in byte dei singoli componenti del testo crittografato **AEAD_AES_256_CBC_HMAC_SHA_256** è:  
  
-   versionbyte: 1  
  
-   MAC: 32  
  
-   IV: 16  
  
-   aes_256_cbc_ciphertext: `(FLOOR (DATALENGTH(cell_data)/ block_size) + 1)* block_size`, dove:  
  
    -   block_size è pari a 16 byte  
  
    -   cell_data è un valore di testo non crittografato  
  
     Pertanto la dimensione minima di aes_256_cbc_ciphertext è 1 blocco, pari a 16 byte.  
  
 La lunghezza del testo crittografato, risultante dalla crittografia dei valori di un determinato testo non crittografato (cell_data), può essere calcolata con la formula seguente:  
  
```  
1 + 32 + 16 + (FLOOR(DATALENGTH(cell_data)/16) + 1) * 16  
```  
  
 Ad esempio:  
  
-   Dopo la crittografia, un valore di testo non crittografato **int** lungo 4 byte diventa un valore binario lungo 65 byte.  
  
-   Un valore di testo non crittografato **nchar(1000)** lungo 2.000 byte diventa un valore binario lungo 2.065 byte.  
  
 La tabella seguente contiene un elenco completo dei tipi di dati e della lunghezza del testo crittografato per ogni tipo.  
  
|Tipo di dati|Lunghezza del testo crittografato [byte]|  
|---------------|---------------------------------|  
|**bigint**|65|  
|**binary**|Variabile. Utilizzare la formula precedente.|  
|**bit**|65|  
|**char**|Variabile. Utilizzare la formula precedente.|  
|**date**|65|  
|**datetime**|65|  
|**datetime2**|65|  
|**datetimeoffset**|65|  
|**decimal**|81|  
|**float**|65|  
|**geography**|N/D (non supportato)|  
|**geometry**|N/D (non supportato)|  
|**hierarchyid**|N/D (non supportato)|  
|**image**|N/D (non supportato)|  
|**int**|65|  
|**money**|65|  
|**nchar**|Variabile. Utilizzare la formula precedente.|  
|**ntext**|N/D (non supportato)|  
|**numeric**|81|  
|**nvarchar**|Variabile. Utilizzare la formula precedente.|  
|**real**|65|  
|**smalldatetime**|65|  
|**smallint**|65|  
|**smallmoney**|65|  
|**sql_variant**|N/D (non supportato)|  
|**sysname**|N/D (non supportato)|  
|**text**|N/D (non supportato)|  
|**time**|65|  
|**timestamp**<br /><br /> (**rowversion**)|N/D (non supportato)|  
|**tinyint**|65|  
|**uniqueidentifier**|81|  
|**varbinary**|Variabile. Utilizzare la formula precedente.|  
|**varchar**|Variabile. Utilizzare la formula precedente.|  
|**xml**|N/D (non supportato)|  
  
## <a name="net-reference"></a>Guida di riferimento a .NET  
 Per i dettagli sugli algoritmi illustrati in questo documento, vedere i file **SqlAeadAes256CbcHmac256Algorithm.cs**, **SqlColumnEncryptionCertificateStoreProvider.cs** e **SqlColumnEncryptionCertificateStoreProvider.cs** nella [Guida di riferimento a .NET](https://referencesource.microsoft.com/).  
  
## <a name="see-also"></a>Vedere anche  
 - [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 - [Sviluppare applicazioni usando Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-client-development.md)  
  
