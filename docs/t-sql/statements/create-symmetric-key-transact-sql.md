---
description: CREATE SYMMETRIC KEY (Transact-SQL)
title: CREATE SYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE SYMMETRIC KEY
- SYMMETRIC KEP
- CREATE_SYMMETRIC_KEY_TSQL
- SYMMETRIC_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE SYMMETRIC KEY statement
- temporary symmetric keys [SQL Server]
- symmetric keys [SQL Server], creating
- symmetric keys [SQL Server]
ms.assetid: b5d23572-b79d-4cf1-9eef-d648fa3b1358
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c0bc4a97ceeea9defe84c637ae4827902956510b
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075963"
---
# <a name="create-symmetric-key-transact-sql"></a>CREATE SYMMETRIC KEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Genera una chiave simmetrica e ne specifica le proprietà in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Questa funzionalità non è compatibile con l'esportazione del database mediante Data Tier Application Framework (DACFx). Prima dell'esportazione, è necessario eliminare tutte le chiavi simmetriche.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CREATE SYMMETRIC KEY key_name   
    [ AUTHORIZATION owner_name ]  
    [ FROM PROVIDER provider_name ]  
    WITH 
      [
          <key_options> [ , ... n ]  
        | ENCRYPTION BY <encrypting_mechanism> [ , ... n ] 
      ]
  
<key_options> ::=  
      KEY_SOURCE = 'pass_phrase'  
    | ALGORITHM = <algorithm>  
    | IDENTITY_VALUE = 'identity_phrase'  
    | PROVIDER_KEY_NAME = 'key_name_in_provider'   
    | CREATION_DISPOSITION = {CREATE_NEW | OPEN_EXISTING }  
  
<algorithm> ::=  
    DES | TRIPLE_DES | TRIPLE_DES_3KEY | RC2 | RC4 | RC4_128  
    | DESX | AES_128 | AES_192 | AES_256   
  
<encrypting_mechanism> ::=  
      CERTIFICATE certificate_name   
    | PASSWORD = 'password'   
    | SYMMETRIC KEY symmetric_key_name   
    | ASYMMETRIC KEY asym_key_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *Key_name*  
 Specifica il nome univoco con il quale la chiave simmetrica è nota all'interno del database. Le chiavi temporanee vengono designate quando _key_name_ inizia con un cancelletto (#). ad esempio **#temporaryKey900007**. Non è possibile creare una chiave simmetrica con un nome che inizia con più di un simbolo di cancelletto (#). Non è possibile creare una chiave simmetrica temporanea utilizzando un provider EKM.  
  
 AUTHORIZATION *owner_name*  
 Specifica il nome dell'utente di database o del ruolo applicazione che sarà il proprietario della chiave.  
  
 FROM PROVIDER *provider_name*  
 Specifica un nome e un provider EKM. La chiave non viene esportata dal dispositivo EKM. È necessario innanzitutto definire il provider utilizzando l'istruzione CREATE PROVIDER. Per altre informazioni sulla creazione di provider di chiavi esterne, vedere [Extensible Key Management &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md).  
  
> [!NOTE]  
>  Questa opzione non è disponibile in un database indipendente.  
  
 KEY_SOURCE **='** _pass\_phrase_ **'**  
 Specifica una passphrase da cui derivare la chiave.  
  
 IDENTITY_VALUE **='** _identity\_phrase_ **'**  
 Specifica una frase identificativa da cui generare un GUID per contrassegnare i dati crittografati con una chiave temporanea.  
  
 PROVIDER_KEY_NAME **='** _key\_name\_in\_provider_ **'**  
 Specifica il nome a cui viene fatto riferimento nel provider EKM.  
  
> [!NOTE]  
>  Questa opzione non è disponibile in un database indipendente.  
  
 CREATION_DISPOSITION **=** CREATE_NEW  
 Crea una nuova chiave nel dispositivo EKM.  Se nel dispositivo esiste già una chiave, l'istruzione genererà un errore.  
  
 CREATION_DISPOSITION **=** OPEN_EXISTING  
 Definisce il mapping di una chiave simmetrica di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a una chiave EKM esistente. Se non si specifica CREATION_DISPOSITION = OPEN_EXISTING, l'impostazione predefinita sarà CREATE_NEW.  
  
 *certificate_name*  
 Specifica il nome del certificato che verrà utilizzato per crittografare la chiave simmetrica. Il certificato deve esistere nel database corrente.  
  
 **'** *password* **'**  
 Specifica una password dalla quale derivare una chiave TRIPLE_DES con cui proteggere la chiave simmetrica. *password* deve soddisfare i requisiti per i criteri password di Windows del computer che sta eseguendo l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Usare sempre password complesse.  
  
 *symmetric_key_name*  
 Specifica una chiave simmetrica usata per crittografare la chiave creata. La chiave specificata deve esistere nel database ed essere aperta.  
  
 *asym_key_name*  
 Specifica una chiave asimmetrica usata per crittografare la chiave creata. Tale chiave asimmetrica deve esistere nel database.  
  
 \<algorithm>  
Specifica l'algoritmo di crittografia.   
> [!WARNING]  
> A partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], tutti gli algoritmi diversi da AES_128, AES_192 e AES_256 sono deprecati. Per usare algoritmi meno recenti (sconsigliato), è necessario impostare il database sul livello di compatibilità del database 120 o su un livello inferiore.  
  
## <a name="remarks"></a>Osservazioni  
 Quando si crea una chiave simmetrica è necessario crittografarla con almeno uno degli elementi seguenti: certificato, password, chiave simmetrica, chiave asimmetrica o PROVIDER. Una chiave può essere crittografata con più elementi di ogni tipo, ovvero una singola chiave simmetrica può essere crittografata contemporaneamente con più certificati, password, chiavi simmetriche e chiavi asimmetriche.  
  
> [!CAUTION]  
>  Se si crittografa una chiave simmetrica con una password anziché con un certificato o un'altra chiave, la password viene crittografata con l'algoritmo di crittografia TRIPLE DES. Per questo motivo, le chiavi create con un algoritmo di crittografia avanzato, come AES, vengono a loro volta protette con un algoritmo meno avanzato.  
  
 È possibile utilizzare la password facoltativa per crittografare la chiave simmetrica prima di distribuirla a più utenti.  
  
 Le chiavi temporanee sono di proprietà dell'utente che le crea e sono valide solo per la sessione corrente.  
  
 La clausola IDENTITY_VALUE genera un GUID per contrassegnare i dati crittografati con la nuova chiave simmetrica. Tale contrassegno può essere utilizzato per eseguire il mapping delle chiavi ai dati crittografati. Il GUID generato da una frase specifica è sempre uguale. Dopo aver utilizzato una frase per generare un GUID, non è possibile utilizzare di nuovo la stessa frase finché viene utilizzata in modo attivo da almeno una sessione. IDENTITY_VALUE è una clausola facoltativa, ma è consigliabile utilizzarla per l'archiviazione di dati crittografati con una chiave temporanea.  
  
 Non è previsto un algoritmo di crittografia predefinito.  
  
> [!IMPORTANT]  
>  Non è consigliabile utilizzare crittografie a flussi RC4 e RC4_128 per proteggere dati riservati. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta ulteriori codifiche per la crittografia eseguita con tali chiavi.  
  
 Le informazioni sulle chiavi simmetriche sono visibili nella vista del catalogo [sys.symmetric_keys](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md).  
  
 Non è possibile crittografare delle chiavi simmetriche con chiavi simmetriche create dal provider di crittografia.  
  
 **Chiarimento relativo agli algoritmi DES:**  
  
-   La crittografia DESX è stata menzionata erroneamente. Le chiavi simmetriche create con ALGORITHM = DESX utilizzano in realtà la crittografia TRIPLE DES con una chiave a 192 bit. L'algoritmo DESX non è disponibile. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
-   Le chiavi simmetriche create con ALGORITHM = TRIPLE_DES_3KEY utilizzano TRIPLE DES con una chiave a 192 bit.  
-   Le chiavi simmetriche create con ALGORITHM = TRIPLE_DES utilizzano TRIPLE DES con una chiave a 128 bit.  
  
 **Rimozione dell'algoritmo RC4:**  
  
 L'uso ripetuto della stessa funzione KEY_GUID RC4 o RC4_128 su blocchi di dati diversi produce la stessa chiave RC4 perché [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non specifica automaticamente un valore salt. L'utilizzo ripetuto della stessa chiave RC4 è un errore noto che comporta una crittografia molto debole. Per questo motivo le parole chiave RC4 e RC4_128 sono deprecate. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
> [!WARNING]  
>  L'algoritmo RC4 è supportato solo per motivi di compatibilità con le versioni precedenti. È possibile crittografare il nuovo materiale usando RC4 o RC4_128 solo quando il livello di compatibilità del database è 90 o 100. (Non consigliato.) Usare un algoritmo più recente, ad esempio uno degli algoritmi AES. In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] il materiale crittografato utilizzando RC4 o RC4_128 può essere decrittografato in qualsiasi livello di compatibilità.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER ANY SYMMETRIC KEY per il database. Se si specifica AUTHORIZATION, è richiesta l'autorizzazione IMPERSONATE per l'utente di database o l'autorizzazione ALTER per il ruolo applicazione. Se la crittografia viene applicata con un certificato o una chiave asimmetrica, è richiesta l'autorizzazione VIEW DEFINITION per il certificato o la chiave asimmetrica. Solo gli account di accesso di Windows e di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e i ruoli applicazione possono disporre di chiavi simmetriche. I gruppi e i ruoli non possono disporre di chiavi simmetriche.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-symmetric-key"></a>R. Creazione di una chiave simmetrica  
 Nell'esempio seguente viene creata una chiave simmetrica denominata `JanainaKey09` con l'algoritmo `AES 256` e la nuova chiave viene quindi crittografata con il certificato `Shipping04`.  
  
```sql  
CREATE SYMMETRIC KEY JanainaKey09   
WITH ALGORITHM = AES_256  
ENCRYPTION BY CERTIFICATE Shipping04;  
GO  
```  
  
### <a name="b-creating-a-temporary-symmetric-key"></a>B. Creazione di una chiave simmetrica temporanea  
 Nell'esempio seguente viene creata una chiave simmetrica temporanea denominata `#MarketingXXV` dalla passphrase: `The square of the hypotenuse is equal to the sum of the squares of the sides`. Alla chiave viene associato un GUID generato dalla stringa `Pythagoras` e la chiave viene poi crittografata con il certificato `Marketing25`.  
  
```sql 
CREATE SYMMETRIC KEY #MarketingXXV   
WITH ALGORITHM = AES_128,  
KEY_SOURCE   
     = 'The square of the hypotenuse is equal to the sum of the squares of the sides',  
IDENTITY_VALUE = 'Pythagoras'  
ENCRYPTION BY CERTIFICATE Marketing25;  
GO  
```  
  
### <a name="c-creating-a-symmetric-key-using-an-extensible-key-management-ekm-device"></a>C. Creazione di una chiave simmetrica utilizzando un dispositivo EKM  
 Nell'esempio seguente viene creata la chiave simmetrica denominata `MySymKey` utilizzando un provider denominato `MyEKMProvider` e il nome chiave `KeyForSensitiveData`. Si assegna l'autorizzazione a `User1` presupponendo che l'amministratore di sistema abbia già registrato il provider denominato `MyEKMProvider` in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
CREATE SYMMETRIC KEY MySymKey  
AUTHORIZATION User1  
FROM PROVIDER EKMProvider  
WITH  
PROVIDER_KEY_NAME='KeyForSensitiveData',  
CREATION_DISPOSITION=OPEN_EXISTING;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Scelta di un algoritmo di crittografia](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [ALTER SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [sys.symmetric_keys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)   
 [Extensible Key Management &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [Extensible Key Management con l'insieme di credenziali delle chiavi di Azure &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
  
  
