---
title: Uso di Always Encrypted con il driver JDBC
description: Informazioni su come usare Always Encrypted nell'applicazione Java con il driver JDBC per SQL Server per crittografare i dati sensibili nel server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 271c0438-8af1-45e5-b96a-4b1cabe32707
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a89deba9ed124587899bc915fa86ef74074ed761
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195275"
---
# <a name="using-always-encrypted-with-the-jdbc-driver"></a>Uso di Always Encrypted con il driver JDBC

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Questa pagina include informazioni su come sviluppare applicazioni Java usando [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md) e Microsoft JDBC Driver 6.0 (o versioni successive) per SQL Server.

Always Encrypted consente ai client di eseguire la crittografia dei dati sensibili senza mai rivelare i dati o le chiavi di crittografia a SQL Server o al database SQL di Azure. Di tutto ciò si occupa un driver abilitato per Always Encrypted, come Microsoft JDBC Driver 6.0 (o versione successiva) per SQL Server, che esegue in modo trasparente la crittografia e la decrittografia dei dati sensibili nell'applicazione client. Il driver determina automaticamente i parametri di query corrispondenti alle colonne di database Always Encrypted e crittografa i valori di tali parametri prima di passarli a SQL Server o al database SQL di Azure. Analogamente, il driver esegue in modo trasparente la decrittografia dei dati, recuperati dalle colonne di database crittografate nei risultati delle query. Per altre informazioni, vedere [Always Encrypted (motore di database)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) e [Informazioni di riferimento sull'API Always Encrypted per il driver JDBC](always-encrypted-api-reference-for-the-jdbc-driver.md).

## <a name="prerequisites"></a>Prerequisiti
- Assicurarsi che nel computer di sviluppo sia installato Microsoft JDBC Driver 6.0 (o versione successiva) per SQL Server. 
- Scaricare e installare Java Cryptography Extension (JCE) Unlimited Strenght Jurisdiction Policy Files.  Leggere il file leggimi incluso nel file ZIP per istruzioni sull'installazione e dettagli rilevanti sui possibili problemi di importazione/esportazione.  

    - Se si usa mssql-jdbc-X.X.X.jre7.jar o sqljdbc41.jar, i file dei criteri possono essere scaricati da [Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files 7 Download](https://www.oracle.com/technetwork/java/javase/downloads/jce-7-download-432124.html)

    - Se si usa mssql-jdbc-X.X.X.jre8.jar o sqljdbc42.jar, i file dei criteri possono essere scaricati da [Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files 8 Download](https://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html)

    - Se si usa mssql-jdbc-X.X.X.jre9.jar, non è necessario scaricare alcun file di criteri. I criteri di giurisdizione predefiniti in Java 9 corrispondono alla crittografia di forza illimitata (Unlimited Strength Jurisdiction).

## <a name="working-with-column-master-key-stores"></a>Uso degli archivi delle chiavi master delle colonne
Per crittografare o decrittografare i dati delle colonne crittografate, SQL Server gestisce chiavi di crittografia di colonna. Le chiavi di crittografia di colonna vengono archiviate in forma crittografata nei metadati del database. Ogni chiave di crittografia di colonna ha una chiave master corrispondente che viene usata per crittografare la chiave di crittografia della colonna. I metadati del database non contengono le chiavi master della colonna. Queste chiavi vengono usate solo dal client. Tuttavia i metadati del database contengono informazioni sulla posizione di archiviazione delle chiavi master della colonna rispetto al client. Ad esempio i metadati del database possono indicare che l'archivio chiavi contenente una chiave master della colonna è l'archivio certificati di Windows e che il certificato specifico usato per crittografare e decrittografare si trova in un percorso specifico all'interno dell'archivio certificati di Windows. Se il client ha accesso a tale certificato nell'archivio certificati di Windows, può ottenere il certificato. Il certificato può quindi essere usato per decrittografare la chiave di crittografia della colonna. A sua volta la chiave di crittografia può essere usata per decrittografare o crittografare i dati delle colonne crittografate che usano la chiave di crittografia della colonna.

Microsoft JDBC Driver per SQL Server comunica con un archivio delle chiavi usando un provider dell'archivio chiavi master delle colonne, ossia un'istanza di una classe derivata da **SQLServerColumnEncryptionKeyStoreProvider**.

### <a name="using-built-in-column-master-key-store-providers"></a>Uso dei provider predefiniti di archivio delle chiavi master delle colonne
Microsoft JDBC Driver per SQL Server include i provider dell'archivio chiavi master delle colonne predefiniti seguenti. Alcuni di questi provider sono preregistrati con i nomi provider specifici (usati per cercare il provider) e altri richiedono credenziali aggiuntive o una registrazione esplicita.

| Classe                                                 | Descrizione                                        | Nome del provider (ricerca)  | È preregistrato? | Piattaforma |
| :---------------------------------------------------- | :------------------------------------------------- | :---------------------- | :----------------- | :------- |
| **SQLServerColumnEncryptionAzureKeyVaultProvider**    | Provider per un archivio chiavi per Azure Key Vault. | AZURE_KEY_VAULT         | _No_ prima della versione del driver JDBC 7.4.1, ma _sì_ a partire dalla versione del driver JDBC 7.4.1. | Windows, Linux, macOS |
| **SQLServerColumnEncryptionCertificateStoreProvider** | Un provider per l'archivio certificati Windows.      | MSSQL_CERTIFICATE_STORE | _Sì_                | Windows |
| **SQLServerColumnEncryptionJavaKeyStoreProvider**     | Provider per l'archivio chiavi Java.                  | MSSQL_JAVA_KEYSTORE     | _Sì_                | Windows, Linux, macOS |
|||||

Per i provider dell'archivio chiave preregistrati non è necessario apportare alcuna modifica del codice dell'applicazione, ma tenere presenti gli elementi che seguono:

- È necessario che l'utente (o l'amministratore del database) verifichi che il nome del provider, configurato nei metadati della chiave master della colonna, sia corretto e che il percorso della chiave master sia conforme al formato del percorso della chiave valido per un determinato provider. È consigliabile configurare le chiavi usando strumenti come SQL Server Management Studio, che genera automaticamente i nomi di provider e i percorsi di chiave validi quando viene eseguita l'istruzione CREATE COLUMN MASTER KEY (Transact-SQL).
- Verificare che l'applicazione possa accedere alla chiave nell'archivio chiavi. Questa attività potrebbe comportare l'accesso da parte dell'applicazione alla chiave e/o all'archivio chiavi, a seconda dell'archivio, o l'esecuzione di altri passaggi di configurazione specifici dell'archivio chiavi. Ad esempio, per usare SQLServerColumnEncryptionJavaKeyStoreProvider è necessario specificare il percorso e la password dell'archivio chiavi nelle proprietà di connessione. 

Tutti questi provider dell'archivio chiavi sono descritti in modo più dettagliato nelle sezioni seguenti. È sufficiente implementare un solo provider dell'archivio chiavi per usare Always Encrypted.

### <a name="using-azure-key-vault-provider"></a>Uso del provider dell'insieme di credenziali delle chiavi di Azure

Azure Key Vault rappresenta una scelta valida per archiviare e gestire le chiavi master delle colonne per Always Encrypted, soprattutto se l'applicazione è ospitata in Azure. Microsoft JDBC driver per SQL Server include un provider predefinito, SQLServerColumnEncryptionAzureKeyVaultProvider, per le applicazioni che hanno chiavi archiviate in Azure Key Vault. Il nome di questo provider è AZURE_KEY_VAULT. Per usare il provider dell'archivio Azure Key Vault lo sviluppatore di applicazioni deve creare l'insieme di credenziali e le chiavi in Azure Key Vault e creare una registrazione dell'app in Azure Active Directory. All'applicazione registrata devono essere concesse le autorizzazioni Ottieni, Decrittografa, Crittografa, Annulla il wrapping della chiave, Esegui il wrapping della chiave e Verifica nei criteri di accesso definiti per l'insieme di credenziali delle chiavi creato per l'uso con Always Encrypted. Per altre informazioni su come configurare l'insieme di credenziali delle chiavi e come creare una chiave master della colonna, vedere [Azure Key Vault - Passo a passo](/archive/blogs/kv/azure-key-vault-step-by-step) e [Creazione di chiavi master della colonna in Azure Key Vault](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md#creating-column-master-keys-in-azure-key-vault).

Quando si usa il provider Azure Key Vault, il driver JDBC convalida il percorso della chiave master della colonna rispetto all'elenco degli endpoint attendibili. A partire dalla versione del driver 8.2.2, questo elenco è configurabile: creare il file "mssql-jdbc.properties" nella directory di lavoro dell'applicazione, impostare la proprietà `AKVTrustedEndpoints` su un elenco delimitato da punti e virgola. Se il valore inizia con un punto e virgola, estende l'elenco predefinito. In caso contrario, sostituisce l'elenco predefinito.

Per gli esempi in questa pagina, se è stata creata una chiave master della colonna basata su Azure Key Vault e una chiave di crittografia della colonna usando SQL Server Management Studio, lo script T-SQL per ricrearle potrebbe essere simile a questo esempio con **KEY_PATH** e **ENCRYPTED_VALUE** specifici:

```sql
CREATE COLUMN MASTER KEY [MyCMK]
WITH
(
    KEY_STORE_PROVIDER_NAME = N'AZURE_KEY_VAULT',
    KEY_PATH = N'https://<MyKeyValutName>.vault.azure.net:443/keys/Always-Encrypted-Auto1/c61f01860f37302457fa512bb7e7f4e8'
);

CREATE COLUMN ENCRYPTION KEY [MyCEK]
WITH VALUES
(
    COLUMN_MASTER_KEY = [MyCMK],
    ALGORITHM = 'RSA_OAEP',
    ENCRYPTED_VALUE = 0x01BA000001680074507400700073003A002F002F006400610076006...
);
```

Un'applicazione che usa il driver JDBC può usare Azure Key Vault. La sintassi o le istruzioni per questo uso di Azure Key Vault sono cambiate a partire dalla versione 7.4.1 del driver JDBC.

#### <a name="jdbc-driver-741-or-later"></a>Driver JDBC 7.4.1 o versione successiva

Questa sezione riguarda il driver JDBC versione 7.4.1 o successiva.

Un'applicazione client che usa il driver JDBC può essere configurata per l'uso di Azure Key Vault citando `keyVaultProviderClientId=<ClientId>;keyVaultProviderClientKey=<ClientKey>` nella stringa di connessione JDBC.

L'esempio illustra come specificare queste informazioni di configurazione in una stringa di connessione JDBC.

```java
String connectionUrl = "jdbc:sqlserver://<server>:<port>;user=<user>;password=<password>;columnEncryptionSetting=Enabled;keyVaultProviderClientId=<ClientId>;keyVaultProviderClientKey=<ClientKey>";
```
Il driver JDBC crea automaticamente un'istanza di un oggetto **SQLServerColumnEncryptionAzureKeyVaultProvider** quando queste credenziali sono presenti tra le proprietà di connessione.

> [!IMPORTANT]
> Le proprietà di connessione **keyVaultProviderClientId** e **keyVaultProviderClientKey** sono state deprecate a partire da v 8.4.1. Gli utenti sono invitati a usare **keyStoreAuthentication**, **KeyStorePrincipalId** e **KeyStoreSecret**.

#### <a name="jdbc-driver-version-prior-to-741"></a>Driver JDBC con versione precedente a 7.4.1

Questa sezione include le versioni del driver JDBC precedenti a 7.4.1.

Un'applicazione client che usa il driver JDBC deve creare un'istanza di un oggetto **SQLServerColumnEncryptionAzureKeyVaultProvider** e quindi registrare l'oggetto con il driver.

```java
SQLServerColumnEncryptionAzureKeyVaultProvider akvProvider = new SQLServerColumnEncryptionAzureKeyVaultProvider(clientID, clientKey);
```

**clientID** è l'ID applicazione della registrazione di un'app in Azure Active Directory. **clientkey** è una password chiave registrata in tale applicazione, che garantisce all'API l'accesso ad Azure Key Vault.

Dopo che ha creato un'istanza di SQLServerColumnEncryptionAzureKeyVaultProvider, l'applicazione deve registrare l'istanza con il driver usando il metodo SQLServerConnection.registerColumnEncryptionKeyStoreProviders(). È vivamente consigliabile registrare l'istanza usando il nome di ricerca predefinito, AZURE_KEY_VAULT, che può essere ottenuto chiamando l'API SQLServerColumnEncryptionAzureKeyVaultProvider. getName(). L'uso del nome predefinito consente di usare strumenti come SQL Server Management Studio o PowerShell per il provisioning e la gestione delle chiavi Always Encrypted (gli strumenti usano il nome predefinito per generare l'oggetto di metadati per la chiave master della colonna). L'esempio seguente illustra la registrazione del provider Azure Key Vault. Per altre informazioni sul metodo SQLServerConnection.registerColumnEncryptionKeyStoreProviders(), vedere [Informazioni di riferimento sull'API Always Encrypted per il driver JDBC](always-encrypted-api-reference-for-the-jdbc-driver.md).

```java
Map<String, SQLServerColumnEncryptionKeyStoreProvider> keyStoreMap = new HashMap<String, SQLServerColumnEncryptionKeyStoreProvider>();
keyStoreMap.put(akvProvider.getName(), akvProvider);
SQLServerConnection.registerColumnEncryptionKeyStoreProviders(keyStoreMap);
```

> [!IMPORTANT]
>  Se si usa il provider dell'archivio chiavi Azure Key Vault, l'implementazione Azure Key Vault del driver JDBC presenta dipendenze dalle librerie seguenti (di GitHub) che devono essere incluse nell'applicazione:
>
>  [azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java)
>
>  [librerie Microsoft-Authentication-Library-for-Java](https://github.com/AzureAD/microsoft-authentication-library-for-java)
>
> Per un esempio di come includere queste dipendenze in un progetto Maven, vedere [scaricare le dipendenze MSAL4J e AKV con Apache Maven](https://github.com/microsoft/mssql-jdbc/wiki/Download-MSAL4J-And-AKV-Dependencies-with-Apache-Maven)

### <a name="using-azure-key-vault-authentication-with-managed-identities"></a>Uso dell'autenticazione di Azure Key Vault con le identità gestite

A partire dal driver JDBC **8.4.1** al driver è stato aggiunto il supporto per l'autenticazione ad Azure Key Vault usando le identità gestite.

Se l'applicazione è ospitata in Azure, l'utente può usare le [identità gestite](/azure/active-directory/managed-identities-azure-resources/overview) per eseguire l'autenticazione ad Azure Key Vault, eliminando così la necessità di specificare ed esporre credenziali nel codice. 

#### <a name="connection-properties-for-key-vault-authentication-with-managed-identities"></a>Proprietà di connessione per l'autenticazione di Azure Key Vault con le identità gestite

Per il driver JDBC 8.4.1 e versioni successive, sono state introdotte le seguenti proprietà di connessione:

| Proprietà di connessione    | Possibile associazione di valori 1 | Possibile associazione di valori 2 | Possibile associazione di valori 3 |
| ---|---|---|----|
| keyStoreAuthentication| KeyVaultClientSecret   |KeyVaultManagedIdentity |JavaKeyStorePassword |  
| keyStorePrincipalId   | \<Azure AD Application Client ID\>    | \<Azure AD Application object ID\> (facoltativo)| n/d |
| keyStoreSecret        | \<Azure AD Application Client Secret\>|n/d|\<secret/password for the Java Key Store\> |

Negli esempi seguenti viene illustrato come usare le proprietà di connessione in una stringa di connessione.

#### <a name="use-managed-identity-to-authenticate-to-akv"></a>Usare un'identità gestita per l'autenticazione in Azure Key Vault
```
"jdbc:sqlserver://<server>:<port>;columnEncryptionSetting=Enabled;keyStoreAuthentication=KeyVaultManagedIdentity;"
```
#### <a name="use-managed-identity-and-the-principal-id-to-authenticate-to-akv"></a>Usare un'identità gestita e l'ID entità di sicurezza per l'autenticazione in Azure Key Vault
```
"jdbc:sqlserver://<server>:<port>;columnEncryptionSetting=Enabled;keyStoreAuthentication=KeyVaultManagedIdentity;keyStorePrincipal=<principalId>"
```
#### <a name="use-clientid-and-clientsecret-to-authentication-to-akv"></a>Usare clientId e clientSecret per l'autenticazione in Azure Key Vault
```
"jdbc:sqlserver://<server>:<port>;columnEncryptionSetting=Enabled;keyStoreAuthentication=KeyVaultClientSecret;keyStorePrincipalId=<clientId>;keyStoreSecret=<clientSecret>"
```
Gli utenti sono invitati a usare queste proprietà di connessione per specificare il tipo di autenticazione usato per gli archivi delle chiavi anziché usare `SQLServerColumnEncryptionAzureKeyVaultProvider`.

Si noti che le proprietà di connessione `keyVaultProviderClientId` e `keyVaultProviderClientKey` aggiunte in precedenza sono deprecate e sostituite dalle proprietà di connessione descritte sopra.

Per informazioni su come configurare le identità gestite, vedere [Configurare identità gestite per risorse di Azure in una macchina virtuale tramite il portale di Azure](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm).

### <a name="using-windows-certificate-store-provider"></a>Uso del provider per l'archivio certificati Windows
SqlColumnEncryptionCertificateStoreProvider consente di archiviare le chiavi master della colonna nell'archivio certificati Windows. Usare la procedura guidata Always Encrypted di SQL Server Management Studio (SSMS) o altri strumenti supportati per creare le definizioni delle chiavi master della colonna e della chiave di crittografia della colonna nel database. È possibile usare la stessa procedura guidata per generare un certificato autofirmato nell'archivio certificati di Windows, che può essere usato come chiave master della colonna per i dati Always Encrypted. Per altre informazioni sulla sintassi T-SQL della chiave master della colonna e della chiave di crittografia della colonna, vedere le istruzioni [CREATE COLUMN MASTER KEY](../../t-sql/statements/create-column-master-key-transact-sql.md) e [CREATE COLUMN ENCRYPTION KEY](../../t-sql/statements/create-column-encryption-key-transact-sql.md) rispettivamente.

Il nome di SQLServerColumnEncryptionCertificateStoreProvider è MSSQL_CERTIFICATE_STORE e può essere sottoposto a query dall'API getName() dell'oggetto provider. Viene registrato automaticamente dal driver e può essere usato senza alcuna modifica dell'applicazione.

Per gli esempi in questa pagina, se è stata creata una chiave master della colonna basata sull'archivio certificati di Windows e una chiave di crittografia della colonna usando SQL Server Management Studio, lo script T-SQL per ricrearle potrebbe essere simile a questo esempio con **KEY_PATH** e **ENCRYPTED_VALUE** specifici:

```sql
CREATE COLUMN MASTER KEY [MyCMK]
WITH
(
    KEY_STORE_PROVIDER_NAME = N'MSSQL_CERTIFICATE_STORE',
    KEY_PATH = N'CurrentUser/My/A2A91F59C461B559E4D962DA9D2BC6131B32CB91'
);

CREATE COLUMN ENCRYPTION KEY [MyCEK]
WITH VALUES
(
    COLUMN_MASTER_KEY = [MyCMK],
    ALGORITHM = 'RSA_OAEP',
    ENCRYPTED_VALUE = 0x016E000001630075007200720065006E0074007500730065007200...
);
```

> [!IMPORTANT]
> Mentre gli altri provider dell'archivio chiavi in questo articolo sono disponibili su tutte le piattaforme supportate dal driver, l'implementazione di SQLServerColumnEncryptionCertificateStoreProvider del driver JDBC è disponibile solo nei sistemi operativi Windows. Ha una dipendenza da mssql-jdbc_auth-\<version>-\<arch>.dll che è disponibile nel pacchetto driver. Per usare questo provider, copiare il file mssql-jdbc_auth-\<version>-\<arch>.dll in una directory nel percorso di sistema Windows nel computer in cui è installato JDBC Driver. In alternativa, è possibile impostare la proprietà di sistema java.library.path per specificare la directory di mssql-jdbc_auth-\<version>-\<arch>.dll. Se si esegue Java Virtual Machine (JVM) a 32 bit, usare il file mssql-jdbc_auth-\<version>-x86.dll nella cartella x86, anche se la versione del sistema operativo è x64. Se si esegue JVM a 64 bit in un processore x64, usare il file mssql-jdbc_auth-\<version>-x64.dll nella cartella x64. Ad esempio, se si usa il driver JVM a 32 bit e il driver JDBC è installato nella directory predefinita, specificare il percorso della DLL tramite l'argomento seguente della VM (Virtual Machine) quando l'applicazione Java viene avviata: `-Djava.library.path=C:\Microsoft JDBC Driver <version> for SQL Server\sqljdbc_<version>\enu\auth\x86`

### <a name="using-java-key-store-provider"></a>Uso del provider dell'archivio chiavi Java
Il driver JDBC è disponibile in un’implementazione predefinita del provider dell’archivio chiavi per l’archivio chiavi Java. Se la proprietà della stringa di connessione **keyStoreAuthentication** è presente nella stringa di connessione ed è impostata su "JavaKeyStorePassword", il driver crea automaticamente un'istanza del provider e lo registra per l'archivio chiavi Java. Il nome del provider dell'archivio chiavi Java è MSSQL_JAVA_KEYSTORE. È anche possibile eseguire query su questo nome usando l'API SQLServerColumnEncryptionJavaKeyStoreProvider.getName(). 

Sono disponibili tre proprietà della stringa di connessione che consentono a un'applicazione client di specificare le credenziali necessarie al driver per l'autenticazione nell'archivio chiavi Java. Il driver Inizializza il provider in base ai valori di queste tre proprietà nella stringa di connessione.

**keyStoreAuthentication:** Identifica l'archivio chiavi Java da usare. Con Microsoft JDBC Driver 6.0 e versioni successive per SQL Server è possibile eseguire l'autenticazione all'archivio chiavi Java solo tramite questa proprietà. Per l'archivio chiavi Java il valore di questa proprietà deve essere `JavaKeyStorePassword`.

**keyStoreLocation:** Percorso del file dell'archivio chiavi Java in cui è archiviata la chiave master della colonna. Il percorso include il nome file dell'archivio chiavi.

**keyStoreSecret:** Password o segreto da usare per l'archivio chiavi e per la chiave. Per l'uso dell'archivio chiavi Java, la password dell'archivio chiavi e la password della chiave devono essere uguali.

L'esempio seguente illustra come specificare queste credenziali nella stringa di connessione:

```java
String connectionUrl = "jdbc:sqlserver://<server>:<port>;user=<user>;password=<password>;columnEncryptionSetting=Enabled;keyStoreAuthentication=JavaKeyStorePassword;keyStoreLocation=<path_to_the_keystore_file>;keyStoreSecret=<keystore_key_password>";
```

È anche possibile ottenere o definire queste impostazioni usando l'oggetto SQLServerDataSource. Per altre informazioni, vedere [Informazioni di riferimento sull'API Always Encrypted per il driver JDBC](always-encrypted-api-reference-for-the-jdbc-driver.md).

Il driver JDBC crea automaticamente un'istanza di SQLServerColumnEncryptionJavaKeyStoreProvider quando queste credenziali sono presenti nelle proprietà di connessione.

### <a name="creating-a-column-master-key-for-the-java-key-store"></a>Creazione di una chiave master della colonna (CMK) per l'archivio chiavi Java
SQLServerColumnEncryptionJavaKeyStoreProvider può essere usato con i tipi di archivio chiavi JKS o PKCS12. Per creare o importare una chiave da usare con questo provider, usare l'utilità [keytool](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html) Java. La chiave deve avere la stessa password dell'archivio chiavi. L'esempio seguente illustra come creare una chiave pubblica e la chiave privata associata usando l'utilità keytool:

```console
keytool -genkeypair -keyalg RSA -alias AlwaysEncryptedKey -keystore keystore.jks -storepass mypassword -validity 360 -keysize 2048 -storetype jks
```

Questo comando crea una chiave pubblica e ne esegue il wrapping in un certificato autofirmato X.509, archiviato nell'archivio chiavi `keystore.jks` insieme alla chiave privata associata. Questa voce nell'archivio chiavi è identificata dall'alias `AlwaysEncryptedKey`.

L'esempio seguente dello stesso comando usa un archivio di tipo PKCS12:

```console
keytool -genkeypair -keyalg RSA -alias AlwaysEncryptedKey -keystore keystore.pfx -storepass mypassword -validity 360 -keysize 2048 -storetype pkcs12 -keypass mypassword
```

Se l'archivio chiavi è di tipo PKCS12, l'utilità keytool non richiede una password della chiave e tale password deve essere specificata con l'opzione `-keypass`, perché **SQLServerColumnEncryptionJavaKeyStoreProvider** richiede che l'archivio chiavi e la chiave abbiano la stessa password.

È anche possibile esportare un certificato dall'archivio certificati di Windows in formato pfx e usarlo con **SQLServerColumnEncryptionJavaKeyStoreProvider**. Il certificato esportato può anche essere importato nell'archivio chiavi Java come tipo di archivio chiavi JKS.

Dopo aver creato la voce keytool creare i metadati della chiave master della colonna nel database, che richiede il nome del provider dell'archivio chiavi e il percorso della chiave. Per altre informazioni su come creare i metadati della chiave master della colonna, vedere l'istruzione [CREATE COLUMN MASTER KEY](../../t-sql/statements/create-column-master-key-transact-sql.md). Per SQLServerColumnEncryptionJavaKeyStoreProvider il percorso della chiave è solo l'alias della chiave e il nome di SQLServerColumnEncryptionJavaKeyStoreProvider è `MSSQL_JAVA_KEYSTORE`. È anche possibile eseguire una query su questo nome usando l'API pubblica getName() della classe SQLServerColumnEncryptionJavaKeyStoreProvider. 

La sintassi T-SQL per la creazione della chiave master della colonna è:

```sql
CREATE COLUMN MASTER KEY [<CMK_name>]
WITH
(
    KEY_STORE_PROVIDER_NAME = N'MSSQL_JAVA_KEYSTORE',
    KEY_PATH = N'<key_alias>'
);
```

Per 'AlwaysEncryptedKey' creato in precedenza, la definizione della chiave master della colonna sarà:

```sql
CREATE COLUMN MASTER KEY [MyCMK]
WITH
(
    KEY_STORE_PROVIDER_NAME = N'MSSQL_JAVA_KEYSTORE',
    KEY_PATH = N'AlwaysEncryptedKey'
);
```

> [!NOTE]
> La funzionalità incorporata di SQL Server Management Studio non è in grado di creare definizioni chiave master della colonna per l'archivio chiavi Java. È necessario usare i comandi T-SQL a livello di codice.

### <a name="creating-a-column-encryption-key-for-the-java-key-store"></a>Creazione di una chiave di crittografia della colonna (CEK) per l'archivio chiavi Java
Non è possibile usare SQL Server Management Studio o altri strumenti per creare chiavi di crittografia della colonna usando le chiavi master della colonna nell'archivio chiavi Java. L'applicazione client deve creare la chiave di crittografia della colonna a livello di codice usando la classe SQLServerColumnEncryptionJavaKeyStoreProvider. Per altre informazioni, vedere [Uso di provider di archivi chiavi master delle colonne per il provisioning di chiavi a livello di codice](#using-column-master-key-store-providers-for-programmatic-key-provisioning).

### <a name="implementing-a-custom-column-master-key-store-provider"></a>Implementazione di un provider personalizzato di archivio delle chiavi master delle colonne
Se si vogliono archiviare le chiavi master delle colonne in un archivio chiavi che non è supportato da un provider esistente, è possibile implementare un provider personalizzato estendendo la classe SQLServerColumnEncryptionKeyStoreProvider e registrando il provider con il metodo SQLServerConnection.registerColumnEncryptionKeyStoreProviders().

```java
public class MyCustomKeyStore extends SQLServerColumnEncryptionKeyStoreProvider{  
    private String name = "MY_CUSTOM_KEYSTORE";

    public void setName(String name)
    {
        this.name = name;
    }

    public String getName()
    {
        return name;
    }

    public byte[] encryptColumnEncryptionKey(String masterKeyPath, String encryptionAlgorithm, byte[] plainTextColumnEncryptionKey)
    {
        // Logic for encrypting the column encryption key
    }

    public byte[] decryptColumnEncryptionKey(String masterKeyPath, String encryptionAlgorithm, byte[] encryptedColumnEncryptionKey)
    {
        // Logic for decrypting the column encryption key
    }
}
```

Registrazione del provider:

```java
SQLServerColumnEncryptionKeyStoreProvider storeProvider = new MyCustomKeyStore();
Map<String, SQLServerColumnEncryptionKeyStoreProvider> keyStoreMap = new HashMap<String, SQLServerColumnEncryptionKeyStoreProvider>();
keyStoreMap.put(storeProvider.getName(), storeProvider);
SQLServerConnection.registerColumnEncryptionKeyStoreProviders(keyStoreMap);
```

## <a name="using-column-master-key-store-providers-for-programmatic-key-provisioning"></a>Uso di provider di archivio delle chiavi master delle colonne per il provisioning di chiavi a livello di codice
Quando si accede a colonne crittografate, Microsoft JDBC Driver per SQL Server trova in modo trasparente e chiama il provider corretto di archivio chiavi master delle colonne per decrittografare le chiavi di crittografia delle colonne. In genere, il codice dell'applicazione normale non chiama direttamente i provider di archivio delle chiavi master delle colonne. È tuttavia possibile creare un'istanza e chiamare un provider a livello di codice per il provisioning e la gestione delle chiavi Always Encrypted. Questo passaggio può essere eseguito per generare una chiave di crittografia della colonna crittografata e decrittografare una chiave di crittografia della colonna, ad esempio nell'ambito della rotazione della chiave master della colonna. Per altre informazioni, vedere [Panoramica della gestione delle chiavi per Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).

Se si usa un provider dell'archivio chiavi personalizzato, potrebbe essere necessario implementare gli strumenti di gestione delle chiavi. Quando si usano chiavi archiviate nell'archivio certificati Windows o in Azure Key Vault è possibile usare gli strumenti esistenti, ad esempio SQL Server Management Studio o PowerShell, per gestire le chiavi ed eseguirne il provisioning. Quando si usano chiavi archiviate nell'archivio chiavi Java è necessario eseguire il provisioning delle chiavi a livello di codice. L'esempio seguente illustra l'uso della classe SQLServerColumnEncryptionJavaKeyStoreProvider per crittografare la chiave con una chiave archiviata nell'archivio chiavi Java.

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerColumnEncryptionJavaKeyStoreProvider;
import com.microsoft.sqlserver.jdbc.SQLServerColumnEncryptionKeyStoreProvider;
import com.microsoft.sqlserver.jdbc.SQLServerException;

/**
 * This program demonstrates how to create a column encryption key programmatically for the Java Key Store.
 */
public class AlwaysEncrypted {
    // Alias of the key stored in the keystore.
    private static String keyAlias = "<provide key alias>";

    // Name by which the column master key will be known in the database.
    private static String columnMasterKeyName = "MyCMK";

    // Name by which the column encryption key will be known in the database.
    private static String columnEncryptionKey = "MyCEK";

    // The location of the keystore.
    private static String keyStoreLocation = "C:\\Dev\\Always Encrypted\\keystore.jks";

    // The password of the keystore and the key.
    private static char[] keyStoreSecret = "********".toCharArray();

    /**
     * Name of the encryption algorithm used to encrypt the value of the column encryption key. The algorithm for the system providers must be
     * RSA_OAEP.
     */
    private static String algorithm = "RSA_OAEP";

    public static void main(String[] args) {
        String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=<databaseName>;user=<user>;password=<password>;columnEncryptionSetting=Enabled;";

        try (Connection connection = DriverManager.getConnection(connectionUrl);
                Statement statement = connection.createStatement();) {

            // Instantiate the Java Key Store provider.
            SQLServerColumnEncryptionKeyStoreProvider storeProvider = new SQLServerColumnEncryptionJavaKeyStoreProvider(keyStoreLocation,
                    keyStoreSecret);

            byte[] encryptedCEK = getEncryptedCEK(storeProvider);

            /**
             * Create column encryption key For more details on the syntax, see:
             * https://docs.microsoft.com/sql/t-sql/statements/create-column-encryption-key-transact-sql Encrypted column encryption key first needs
             * to be converted into varbinary_literal from bytes, for which byteArrayToHex() is used.
             */
            String createCEKSQL = "CREATE COLUMN ENCRYPTION KEY "
                    + columnEncryptionKey
                    + " WITH VALUES ( "
                    + " COLUMN_MASTER_KEY = "
                    + columnMasterKeyName
                    + " , ALGORITHM =  '"
                    + algorithm
                    + "' , ENCRYPTED_VALUE =  0x"
                    + byteArrayToHex(encryptedCEK)
                    + " ) ";
            statement.executeUpdate(createCEKSQL);
            System.out.println("Column encryption key created with name : " + columnEncryptionKey);
        }
        // Handle any errors that may have occurred.
        catch (SQLException e) {
            e.printStackTrace();
        }
    }

    private static byte[] getEncryptedCEK(SQLServerColumnEncryptionKeyStoreProvider storeProvider) throws SQLServerException {
        String plainTextKey = "You need to give your plain text";

        // plainTextKey has to be 32 bytes with current algorithm supported
        byte[] plainCEK = plainTextKey.getBytes();

        // This will give us encrypted column encryption key in bytes
        byte[] encryptedCEK = storeProvider.encryptColumnEncryptionKey(keyAlias, algorithm, plainCEK);

        return encryptedCEK;
    }

    public static String byteArrayToHex(byte[] a) {
        StringBuilder sb = new StringBuilder(a.length * 2);
        for (byte b : a)
            sb.append(String.format("%02x", b).toUpperCase());
        return sb.toString();
    }
}
```

## <a name="enabling-always-encrypted-for-application-queries"></a>Abilitazione di Always Encrypted per le query dell'applicazione
Il modo più semplice per abilitare la crittografia dei parametri e la decrittografia dei risultati delle query per le colonne crittografate consiste nell'impostare il valore della parola chiave della stringa di connessione **columnEncryptionSetting** su **Enabled**.

Di seguito è riportato un esempio di stringa di connessione che abilita Always Encrypted nel driver JDBC:

```java
String connectionUrl = "jdbc:sqlserver://<server>:<port>;user=<user>;password=<password>;databaseName=<database>;columnEncryptionSetting=Enabled;";
SQLServerConnection connection = (SQLServerConnection) DriverManager.getConnection(connectionUrl);
```

Il codice seguente è un esempio equivalente che usa l'oggetto SQLServerDataSource:

```java
SQLServerDataSource ds = new SQLServerDataSource();
ds.setServerName("<server>");
ds.setPortNumber(<port>);
ds.setUser("<user>");
ds.setPassword("<password>");
ds.setDatabaseName("<database>");
ds.setColumnEncryptionSetting("Enabled");
SQLServerConnection con = (SQLServerConnection) ds.getConnection();
```

Always Encrypted può anche essere abilitato per le singole query. Per altre informazioni, vedere [Controllo dell'impatto di Always Encrypted sulle prestazioni](#controlling-the-performance-impact-of-always-encrypted). Perché la crittografia o la decrittografia abbiano esito positivo, non è sufficiente l'abilitazione di Always Encrypted. È necessario anche assicurarsi che:
- L'applicazione abbia le autorizzazioni di database *VIEW ANY COLUMN MASTER KEY DEFINITION* e *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION* , necessarie per accedere ai metadati sulle chiavi Always Encrypted nel database. Per informazioni dettagliate, vedere [ Autorizzazioni in Always Encrypted (motore di database)](../../relational-databases/security/encryption/always-encrypted-database-engine.md#database-permissions).
- L'applicazione può accedere alla chiave master della colonna che protegge le chiavi di crittografia di colonna, le quali crittografano le colonne di database sottoposte a query. Per usare il provider dell'archivio chiavi Java è necessario specificare credenziali aggiuntive nella stringa di connessione. Per altre informazioni, vedere [Uso del provider dell'archivio chiavi Java](#using-java-key-store-provider).

### <a name="configuring-how-javasqltime-values-are-sent-to-the-server"></a>Configurazione della modalità di invio dei valori java.sql.Time al server
La proprietà di connessione **sendTimeAsDatetime** viene usata per configurare la modalità di invio del valore java.sql.Time al server. Se è impostata su false, il valore dell'ora viene inviato come tipo time di SQL Server. Se è impostata su true, il valore dell'ora viene inviato come tipo datetime. Se una colonna time è crittografata, la proprietà **sendTimeAsDatetime** deve essere false, perché le colonne crittografate non supportano la conversione da time a datetime. Si noti anche che questa proprietà è true per impostazione predefinita, pertanto quando si usano le colonne time crittografate è necessario impostarla su false. In caso contrario il driver genererà un'eccezione. A partire dalla versione 6.0 del driver, la classe SQLServerConnection dispone di due metodi per configurare il valore di questa proprietà a livello di codice:
 
* public void setSendTimeAsDatetime(boolean sendTimeAsDateTimeValue)
* public boolean getSendTimeAsDatetime()

Per altre informazioni su questa proprietà, vedere [Configurazione della modalità di invio dei valori java.sql.Time al server](configuring-how-java-sql-time-values-are-sent-to-the-server.md).

### <a name="configuring-how-string-values-are-sent-to-the-server"></a>Configurazione della modalità di invio dei valori String al server
La proprietà di connessione **sendStringParametersAsUnicode** viene usata per configurare la modalità di invio dei valori String a SQL Server. Se è impostata su True, i parametri String vengono inviati al server in formato Unicode. Se è impostata su false, i parametri String vengono inviati in formato non Unicode, ad esempio come ASCII o MBCS anziché come Unicode. Il valore predefinito di questa proprietà è True. Quando Always Encrypted è abilitato e viene crittografata una colonna char/varchar/varchar(max), il valore di **sendStringParametersAsUnicode** deve essere impostato su false. Se questa proprietà è impostata su true, il driver genera un'eccezione durante la decrittografia dei dati da una colonna char/varchar/varchar(max) crittografata che include caratteri Unicode. Per altre informazioni su questa proprietà, vedere [Impostazione delle proprietà di connessione](setting-the-connection-properties.md).
  
## <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>Recupero e modifica dei dati nelle colonne crittografate
Dopo aver abilitato Always Encrypted per le query dell'applicazione, è possibile usare le API JDBC standard per recuperare o modificare i dati nelle colonne di database crittografate. Se l'applicazione ha le autorizzazioni necessarie per il database e può accedere alla chiave master della colonna, il driver crittograferà tutti i parametri di query che puntano a colonne crittografate e decrittograferà i dati recuperati dalle colonne crittografate.

Se Always Encrypted non è abilitato, le query con parametri destinati alle colonne crittografate avranno esito negativo. Le query possono comunque recuperare i dati dalle colonne crittografate, a condizione che non presentino parametri destinati alle colonne crittografate. Il driver non tenterà però di decrittografare i valori recuperati dalle colonne crittografate e l'applicazione riceverà dati crittografati binari, sotto forma di matrici di byte.

La tabella seguente riepiloga il comportamento delle query, a seconda che la funzionalità Always Encrypted sia abilitata o meno:

| Caratteristica della query                                                                           | Always Encrypted è abilitato e l'applicazione può accedere alle chiavi e ai metadati delle chiavi                                                                                                                        | Always Encrypted è abilitato e l'applicazione non può accedere alle chiavi o ai metadati delle chiavi | Always Encrypted è disabilitato                                                                                        |
| :--------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :-------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------ |
| Query con parametri destinati alle colonne crittografate.                                           | I valori dei parametri vengono crittografati in modo trasparente.                                                                                                                                                           | Errore                                                                             | Errore                                                                                                               |
| Query che recuperano dati dalle colonne crittografate, senza parametri destinati alle colonne crittografate. | I risultati delle colonne vengono decrittografati in modo trasparente. L'applicazione riceve valori di testo non crittografato dei tipi di dati JDBC corrispondenti ai tipi di SQL Server configurati per le colonne crittografate. | Errore                                                                             | I risultati delle colonne crittografate non vengono decrittografati. L'applicazione riceve i valori crittografati come matrici di byte (byte[]). |

### <a name="inserting-and-retrieving-encrypted-data-examples"></a>Esempi di inserimento e recupero di dati crittografati

Gli esempi seguenti illustrano il recupero e la modifica dei dati nelle colonne crittografate. Gli esempi presuppongono la presenza della tabella di destinazione con lo schema seguente e le colonne crittografate SSN e BirthDate. Se è stata configurata una chiave master della colonna denominata "MyCMK" e una chiave di crittografia della colonna denominata "MyCEK", come descritto nelle sezioni precedenti per i provider dell'archivio chiavi, è possibile creare la tabella usando questo script:

```sql
CREATE TABLE [dbo].[Patients]([PatientId] [int] IDENTITY(1,1),
 [SSN] [char](11) COLLATE Latin1_General_BIN2
 ENCRYPTED WITH (ENCRYPTION_TYPE = DETERMINISTIC,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = MyCEK) NOT NULL,
 [FirstName] [nvarchar](50) NULL,
 [LastName] [nvarchar](50) NULL,
 [BirthDate] [date]
 ENCRYPTED WITH (ENCRYPTION_TYPE = RANDOMIZED,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = MyCEK) NOT NULL
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY]);
 GO
```

Per ogni esempio di codice Java è necessario inserire il codice specifico dell'archivio chiavi nel percorso indicato.

Se si usa un provider archivio chiavi Azure Key Vault:

```java
    String clientID = "<Azure Application ID>";
    String clientKey = "<Azure Application API Key Password>";
    SQLServerColumnEncryptionAzureKeyVaultProvider akvProvider = new SQLServerColumnEncryptionAzureKeyVaultProvider(clientID, clientKey);
    Map<String, SQLServerColumnEncryptionKeyStoreProvider> keyStoreMap = new HashMap<String, SQLServerColumnEncryptionKeyStoreProvider>();
    keyStoreMap.put(akvProvider.getName(), akvProvider);
    SQLServerConnection.registerColumnEncryptionKeyStoreProviders(keyStoreMap);
    String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=<databaseName>;user=<user>;password=<password>;columnEncryptionSetting=Enabled;";
```

Se si usa un provider archivio chiavi Archivio certificati Windows:

```java
    String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=<databaseName>;user=<user>;password=<password>;columnEncryptionSetting=Enabled;";
```

Se si usa un provider archivio chiavi Java:

```java
    String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=<databaseName>;user=<user>;password=<password>;columnEncryptionSetting=Enabled;keyStoreAuthentication=JavaKeyStorePassword;keyStoreLocation=<path to jks or pfx file>;keyStoreSecret=<keystore secret/password>";
```

### <a name="inserting-data-example"></a>Esempio di inserimento dei dati

Questo esempio illustra come inserire una riga nella tabella Patients. Si noti quanto segue:

- Il codice di esempio non contiene alcun elemento specifico per la crittografia. Il provider di dati Microsoft JDBC Driver per SQL Server rileva automaticamente e crittografa i parametri destinati alle colonne crittografate. In questo modo la crittografia diventa trasparente per l'applicazione.
- I valori inseriti nelle colonne di database, incluse quelle crittografate, vengono passati come parametri tramite SQLServerPreparedStatement. Quando si inviano valori a colonne non crittografate, l'uso dei parametri è facoltativo, nonostante sia consigliabile per prevenire attacchi SQL injection. È invece necessario usare i parametri in presenza di valori destinati a colonne crittografate. Se i valori inseriti nelle colonne crittografate sono stati passati come valori letterali incorporati nell'istruzione della query, la query ha esito negativo, perché il driver non riesce a determinare i valori delle colonne di destinazione crittografate e quindi non crittograferà i valori. Di conseguenza, il server li rifiuterà come incompatibili con le colonne crittografate.
- Tutti i valori stampati dal programma saranno in testo non crittografato perché Microsoft JDBC Driver per SQL Server possa decrittografare in modo trasparente i dati recuperati dalle colonne crittografate.
- Se si esegue una ricerca tramite una clausola WHERE, il valore usato nella clausola WHERE deve essere passato come parametro, in modo che il driver possa codificarlo in modo trasparente prima dell'invio al database. Nell'esempio seguente SSN viene passato come parametro, ma LastName viene passato come valore letterale, perché LastName non è crittografato.
- Il metodo setter usato per il parametro destinato alla colonna SSN è setString(), che esegue il mapping al tipo di dati di SQL Server char/varchar. Se per questo parametro il metodo setter usato era setNString(), che esegue il mapping a nchar/nvarchar, la query avrà esito negativo perché Always Encrypted non supporta le conversioni da valori nchar/nvarchar crittografati a valori char/varchar crittografati.

```java
// <Insert keystore-specific code here>
try (Connection sourceConnection = DriverManager.getConnection(connectionUrl);
        PreparedStatement insertStatement = sourceConnection.prepareStatement("INSERT INTO [dbo].[Patients] VALUES (?, ?, ?, ?)")) {
    insertStatement.setString(1, "795-73-9838");
    insertStatement.setString(2, "Catherine");
    insertStatement.setString(3, "Abel");
    insertStatement.setDate(4, Date.valueOf("1996-09-10"));
    insertStatement.executeUpdate();
    System.out.println("1 record inserted.\n");
}
// Handle any errors that may have occurred.
catch (SQLException e) {
    e.printStackTrace();
}
```

### <a name="retrieving-plaintext-data-example"></a>Esempio di recupero di dati di testo non crittografato

L'esempio seguente illustra come filtrare i dati in base ai valori crittografati e recuperare i dati in testo non crittografato da colonne crittografate. Si noti quanto segue:

- Il valore usato nella clausola WHERE per filtrare la colonna SSN deve essere passato come parametro, in modo che Microsoft JDBC Driver per SQL Server possa codificarlo in modo trasparente prima dell'invio al database.
- Tutti i valori stampati dal programma saranno in testo non crittografato perché Microsoft JDBC Driver per SQL Server possa decrittografare in modo trasparente i dati recuperati dalle colonne SSN e BirthDate.

> [!NOTE]
> Se le colonne sono crittografate tramite crittografia deterministica, le query possono eseguire confronti di uguaglianza nelle colonne. Per altre informazioni, vedere [Selezione della crittografia deterministica o casuale in Always Encrypted (motore di database)](../../relational-databases/security/encryption/always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).

```java
// <Insert keystore-specific code here>
try (Connection connection = DriverManager.getConnection(connectionUrl);
        PreparedStatement selectStatement = connection
                .prepareStatement("\"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN = ?;\"");) {
    selectStatement.setString(1, "795-73-9838");
    ResultSet rs = selectStatement.executeQuery();
    while (rs.next()) {
        System.out.println("SSN: " + rs.getString("SSN") + ", FirstName: " + rs.getString("FirstName") + ", LastName:"
                + rs.getString("LastName") + ", Date of Birth: " + rs.getString("BirthDate"));
    }
}
// Handle any errors that may have occurred.
catch (SQLException e) {
    e.printStackTrace();
}
```

### <a name="retrieving-encrypted-data-example"></a>Esempio di recupero di dati crittografati

Se Always Encrypted non è abilitato, una query può comunque recuperare dati dalle colonne crittografate, a condizione che non presenti parametri destinati alle colonne crittografate.

L'esempio seguente illustra come recuperare dati crittografati binari da colonne crittografate. Si noti quanto segue:

- Considerato che Always Encrypted non è abilitato nella stringa di connessione, la query restituirà i valori crittografati di SSN e BirthDate come matrici di byte. Il programma converte i valori in stringhe.
- Una query che recupera dati dalle colonne crittografate con Always Encrypted disabilitato può avere parametri, a condizione che nessuno dei parametri sia destinato a una colonna crittografata. La query seguente filtra in base alla colonna LastName, che non è crittografata nel database. Se la query avesse filtrato per SSN o data di nascita, avrebbe avuto esito negativo.

```java
try (Connection sourceConnection = DriverManager.getConnection(connectionUrl);
        PreparedStatement selectStatement = sourceConnection
                .prepareStatement("SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE LastName = ?;");) {

    selectStatement.setString(1, "Abel");
    ResultSet rs = selectStatement.executeQuery();
    while (rs.next()) {
        System.out.println("SSN: " + rs.getString("SSN") + ", FirstName: " + rs.getString("FirstName") + ", LastName:"
                + rs.getString("LastName") + ", Date of Birth: " + rs.getString("BirthDate"));
    }
}
// Handle any errors that may have occurred.
catch (SQLException e) {
    e.printStackTrace();
}
```

### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>Come evitare i problemi comuni quando si eseguono query su colonne crittografate

Questa sezione descrive categorie di errori comuni che si verificano quando si eseguono query su colonne crittografate da applicazioni Java e contiene alcune linee guida su come correggere tali errori.

### <a name="unsupported-data-type-conversion-errors"></a>Errori di conversione dei tipi di dati non supportati

Always Encrypted supporta alcune conversioni per i tipi di dati crittografati. Per l'elenco dettagliato delle conversioni dei tipi supportati, vedere [Always Encrypted (motore di database)](../../relational-databases/security/encryption/always-encrypted-database-engine.md). Per evitare errori di conversione dei tipi di dati, procedere in questo modo. Assicurarsi che:

- Vengano usati i metodi setter appropriati quando si passano valori per i parametri destinati alle colonne crittografate. Verificare che il tipo di dati SQL Server del parametro corrisponda esattamente al tipo della colonna di destinazione oppure che sia supportata una conversione del tipo di dati di SQL Server del parametro al tipo di destinazione della colonna. Sono stati aggiunti metodi API alle classi SQLServerPreparedStatement, SQLServerCallableStatement e SQLServerResultSet per il passaggio di parametri corrispondenti a tipi di dati di SQL Server specifici. Se ad esempio una colonna non è crittografata, è possibile usare il metodo setTimestamp() per passare un parametro a una colonna datetime2 o a una colonna datetime. Tuttavia quando una colonna è crittografata è necessario usare il metodo esatto che rappresenta il tipo della colonna nel database. Ad esempio usare setTimestamp() per passare i valori a una colonna datetime2 crittografata e usare il valore setDateTime() per passare i valori a una colonna datetime crittografata. Per l'elenco completo delle nuove API, vedere [Informazioni di riferimento sull'API Always Encrypted per il driver JDBC](always-encrypted-api-reference-for-the-jdbc-driver.md).
- La precisione e la scala dei parametri destinati alle colonne dei tipi di dati di SQL Server decimali e numerici sono uguali a quelle configurate per la colonna di destinazione. Sono stati aggiunti metodi API alle classi SQLServerPreparedStatement, SQLServerCallableStatement e SQLServerResultSet che accettano precisione e scala insieme ai valori di dati per parametri/colonne che rappresentano i tipi di dati decimale e numerico. Per un elenco completo delle API nuove/con overload, vedere [Informazioni di riferimento sull'API Always Encrypted per il driver JDBC](always-encrypted-api-reference-for-the-jdbc-driver.md).  
- La precisione/scala in frazioni di secondo dei parametri destinati alle colonne dei tipi di dati di SQL Server datetime2, datetimeoffset o time non è maggiore della precisione/scala in frazioni di secondo per la colonna di destinazione nelle query che modificano i valori di tale colonna. Sono stati aggiunti metodi API alle classi SQLServerPreparedStatement, SQLServerCallableStatement e SQLServerResultSet che accettano precisione/scala in frazioni di secondo insieme ai valori di dati per parametri/colonne che rappresentano questi tipi di dati. Per un elenco completo delle API nuove/con overload, vedere [Informazioni di riferimento sull'API Always Encrypted per il driver JDBC](always-encrypted-api-reference-for-the-jdbc-driver.md).

### <a name="errors-due-to-incorrect-connection-properties"></a>Errori causati da proprietà di connessione non corrette

Questa sezione descrive come configurare correttamente le impostazioni di connessione per usare i dati Always Encrypted. Poiché i tipi di dati crittografati supportano le conversioni limitate, le impostazioni di connessione **sendTimeAsDatetime** e **sendStringParametersAsUnicode** richiedono una configurazione appropriata quando si usano colonne crittografate. Assicurarsi che:

- L'impostazione di connessione [sendTimeAsDatetime](setting-the-connection-properties.md) sia impostata su false quando si inseriscono dati in colonne crittografate di tipo time. Per altre informazioni, vedere [Configurazione della modalità di invio dei valori java.sql.Time al server](configuring-how-java-sql-time-values-are-sent-to-the-server.md).
- L'impostazione di connessione [sendStringParametersAsUnicode](setting-the-connection-properties.md) sia impostata su true (o mantenga il valore predefinito) quando si inseriscono dati in colonne crittografate di tipo char/varchar/varchar(max).

### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>Errori causati dal passaggio di testo non crittografato anziché di valori crittografati

Qualsiasi valore destinato a una colonna crittografata deve essere crittografato all'interno dell'applicazione. Il tentativo di inserire, modificare o filtrare in base a un valore di testo non crittografato in una colonna crittografata genererà un errore simile al seguente:

```java
com.microsoft.sqlserver.jdbc.SQLServerException: Operand type clash: varchar is incompatible with varchar(8000) encrypted with (encryption_type = 'DETERMINISTIC', encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'MyCEK', column_encryption_key_database_name = 'ae') collation_name = 'SQL_Latin1_General_CP1_CI_AS'
```

Per evitare tali errori, verificare che:

- Always Encrypted sia abilitato per le query dell'applicazione destinate alle colonne crittografate (per la stringa di connessione o per una query specifica).
- Per inviare i dati destinati alle colonne crittografate vengano usate istruzioni e parametri preparati. L'esempio seguente illustra una query che filtra in modo errato in base a un valore letterale o costante in una colonna crittografata (SSN), anziché passare il valore letterale all'interno sotto forma di parametro. Questa query avrà esito negativo:

```java
ResultSet rs = connection.createStatement().executeQuery("SELECT * FROM Customers WHERE SSN='795-73-9838'");
```

## <a name="force-encryption-on-input-parameters"></a>Forzare la crittografia sui parametri di input

La funzionalità Forza crittografia applica la crittografia di un parametro quando si usa Always Encrypted. Se si usa la crittografia forzata e SQL Server indica al driver che il parametro non deve essere crittografato, la query che usa il parametro avrà esito negativo. Questa proprietà fornisce protezione aggiuntiva contro attacchi alla sicurezza in cui un SQL Server compromesso fornisce al client metadati di crittografia non corretti, con conseguente rischio di divulgazione dei dati. I metodi set* nelle classi SQLServerPreparedStatement e SQLServerCallableStatement e i metodi update\* nella classe SQLServerResultSet sono sottoposti a overload per accettare un argomento booleano che consenta di specificare l'impostazione Forza crittografia. Se il valore di questo argomento è false, il driver non forza la crittografia sui parametri. Se Forza crittografia è true, il parametro di query viene inviato solo se la colonna di destinazione è crittografata e Always Encrypted è abilitato per la connessione o per l'istruzione. L'uso di questa proprietà offre un livello di sicurezza aggiuntivo, garantendo che il driver non invii erroneamente i dati SQL Server come testo non crittografato quando si prevede che sia crittografato.

Per altre informazioni sui metodi SQLServerPreparedStatement e SQLServerCallableStatement sottoposti a overload con l'impostazione Forza crittografia, vedere [Informazioni di riferimento sull'API Always Encrypted per il driver JDBC](always-encrypted-api-reference-for-the-jdbc-driver.md)  

## <a name="controlling-the-performance-impact-of-always-encrypted"></a>Controllo dell'impatto di Always Encrypted sulle prestazioni

Considerato che Always Encrypted è una tecnologia di crittografia lato client, la maggior parte del sovraccarico delle prestazioni si verifica sul lato client, non nel database. A parte il costo delle operazioni di crittografia e decrittografia, le altre origini di sovraccarico delle prestazioni sul lato client sono le seguenti:

- Round trip aggiuntivi al database per recuperare i metadati per i parametri di query.
- Chiamate a un archivio delle chiavi master delle colonne per accedere alla chiave master di una colonna.

Questa sezione descrive le ottimizzazioni delle prestazioni predefinite in Microsoft JDBC Driver per SQL Server e come controllare l'impatto sulle prestazioni esercitato dai due fattori descritti.

### <a name="controlling-round-trips-to-retrieve-metadata-for-query-parameters"></a>Controllo dei round trip per recuperare i metadati per i parametri di query

Se la funzionalità Always Encrypted è abilitata per una connessione, per impostazione predefinita il driver chiamerà [sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) per ogni query con parametri, passando l'istruzione di query (senza i valori dei parametri) a SQL Server. [sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) analizza l'istruzione di query per verificare se sono presenti parametri da crittografare e, in tal caso, restituisce le informazioni relative alla crittografia che consentiranno al driver di crittografare i valori dei parametri. Il comportamento descritto garantisce all'applicazione client un elevato livello di trasparenza. A condizione che i valori destinati alle colonne crittografate vengano passati al driver tramite parametri. non è necessario che l'applicazione (e lo sviluppatore dell'applicazione) sappia quali query accedono alle colonne crittografate.

### <a name="setting-always-encrypted-at-the-query-level"></a>Impostazione di Always Encrypted a livello di query

Per controllare l'impatto sulle prestazioni del recupero dei metadati di crittografia per le query con parametri, è possibile abilitare Always Encrypted per singole query, anziché l'impostare la funzionalità per la connessione. In questo modo, è possibile assicurarsi che sys.sp_describe_parameter_encryption venga richiamato solo per le query con parametri destinati a colonne crittografate. Si noti, tuttavia, che in tal modo si riduce la trasparenza della crittografia. Se si modificano le proprietà di crittografia delle colonne di database, potrebbe essere necessario modificare il codice dell'applicazione per allinearlo alle modifiche dello schema.

Per controllare il comportamento Always Encrypted delle singole query è necessario configurare singoli oggetti istruzione passando un'enumerazione SQLServerStatementColumnEncryptionSetting, che indica come verranno inviati e ricevuti i dati durante la lettura e la scrittura di colonne crittografate per l'istruzione specifica. Di seguito sono riportate alcune linee guida utili:

- Se la maggior parte delle query che un'applicazione client invia alla connessione del database accede alle colonne crittografate, seguire queste linee guida:

    - Impostare la parola chiave della stringa di connessione **columnEncryptionSetting** su **Enabled**.
    - Impostare SQLServerStatementColumnEncryptionSetting.Disabled per le singole query che non accedono a colonne crittografate. In questo modo verranno disabilitati sia la chiamata di sys.sp_describe_parameter_encryption chiamante che il tentativo di decrittografare i valori nel set di risultati.
    - Impostare SQLServerStatementColumnEncryptionSetting.ResultSet per le singole query che non presentano parametri da crittografare, ma che recuperano i dati dalle colonne crittografate. In questo modo verranno disabilitate la chiamata di sys.sp_describe_parameter_encryption e la crittografia dei parametri. La query potrà decrittografare i risultati delle colonne di crittografia.

- Se la maggior parte delle query che un'applicazione client invia alla connessione del database non accede alle colonne crittografate, seguire queste linee guida:

    - Impostare la parola chiave della stringa di connessione **columnEncryptionSetting** su **Disabled**.
    - Impostare SQLServerStatementColumnEncryptionSetting.Enabled per le singole query che presentano parametri da crittografare. In questo modo verranno abilitate sia la chiamata di sys.sp_describe_parameter_encryption che la decrittografia dei risultati di query recuperati da colonne crittografate.
    - Impostare SQLServerStatementColumnEncryptionSetting.ResultSet per le query che non presentano parametri da crittografare, ma che recuperano i dati dalle colonne crittografate. In questo modo verranno disabilitate la chiamata di sys.sp_describe_parameter_encryption e la crittografia dei parametri. La query potrà decrittografare i risultati delle colonne di crittografia.

Le impostazioni SQLServerStatementColumnEncryptionSetting non possono essere usate per ignorare la crittografia e ottenere l'accesso ai dati in testo non crittografato. Per altre informazioni sulla configurazione della crittografia di colonna in un'istruzione, vedere [Informazioni di riferimento sull'API Always Encrypted per il driver JDBC](always-encrypted-api-reference-for-the-jdbc-driver.md).  

Nell'esempio seguente la funzionalità Always Encrypted è disabilitata per la connessione di database. La query eseguita dall'applicazione ha un parametro la cui destinazione è la colonna LastName non crittografata. La query recupera i dati dalle colonne SSN e BirthDate, che sono entrambe crittografate. In tal caso, la chiamata di sys.sp_describe_parameter_encryption per recuperare i metadati di crittografia non è necessaria. È tuttavia necessario abilitare la decrittografia dei risultati della query in modo che l'applicazione possa ricevere i valori di testo non crittografato dalle due colonne crittografate. A tale scopo si usa l'impostazione SQLServerStatementColumnEncryptionSetting.ResultSet.

```java
// Assumes the same table definition as in Section "Retrieving and modifying data in encrypted columns"
// where only SSN and BirthDate columns are encrypted in the database.
String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=<database>;user=<user>;password=<password>;" 
        + "keyStoreAuthentication=JavaKeyStorePassword;"
        + "keyStoreLocation=<keyStoreLocation>" 
        + "keyStoreSecret=<keyStoreSecret>;";

String filterRecord = "SELECT FirstName, LastName, SSN, BirthDate FROM " + tableName + " WHERE LastName = ?";

try (SQLServerConnection connection = (SQLServerConnection) DriverManager.getConnection(connectionUrl);
        PreparedStatement selectStatement = connection.prepareStatement(filterRecord, ResultSet.TYPE_FORWARD_ONLY, ResultSet.CONCUR_READ_ONLY,
                connection.getHoldability(), SQLServerStatementColumnEncryptionSetting.ResultSetOnly);) {

    selectStatement.setString(1, "Abel");
    ResultSet rs = selectStatement.executeQuery();
    while (rs.next()) {
        System.out.println("First name: " + rs.getString("FirstName"));
        System.out.println("Last name: " + rs.getString("LastName"));
        System.out.println("SSN: " + rs.getString("SSN"));
        System.out.println("Date of Birth: " + rs.getDate("BirthDate"));
    }
}
// Handle any errors that may have occurred.
catch (SQLException e) {
    e.printStackTrace();
}
```

### <a name="column-encryption-key-caching"></a>Memorizzazione nella cache delle chiavi di crittografia della colonna

Per ridurre il numero di chiamate a un archivio chiavi master della colonna per decrittografare le chiavi di crittografia della colonna, Microsoft JDBC Driver per SQL Server memorizza nella cache le chiavi di crittografia della colonna di testo non crittografato. Dopo aver ricevuto il valore delle chiavi di crittografia della colonna crittografata dai metadati del database, il driver prova prima a trovare la chiave di crittografia della colonna di testo non crittografato corrispondente al valore della chiave crittografata. Il driver chiama l'archivio chiavi contenente la chiave master della colonna solo se non trova nella cache il valore della chiave di crittografia della colonna crittografata.

È possibile configurare un valore di durata (TTL) per le voci chiave di crittografia della colonna nella cache usando l'API setColumnEncryptionKeyCacheTtl() nella classe SQLServerConnection. Il valore di durata (TTL) predefinito per le voci della chiave di crittografia della colonna nella cache è pari a due ore. Per disattivare la memorizzazione nella cache, usare un valore pari a 0. Per impostare un valore di durata (TTL) usare l'API seguente:

```java
SQLServerConnection.setColumnEncryptionKeyCacheTtl (int columnEncryptionKeyCacheTTL, TimeUnit unit)
```

Ad esempio, per impostare un valore di durata (TTL) di 10 minuti, usare:

```java
SQLServerConnection.setColumnEncryptionKeyCacheTtl (10, TimeUnit.MINUTES)
```

Le unità di tempo supportate sono DAYS (Giorni), HOURS (Ore), MINUTES (Minuti) o SECONDS (Secondi).  

## <a name="copying-encrypted-data-using-sqlserverbulkcopy"></a>Copia dei dati crittografati tramite SqlBulkCopy

Con SQLServerBulkCopy, è possibile copiare dati già crittografati e archiviati in una tabella in un'altra tabella, senza eseguire la decrittografia dei dati. A tale scopo, eseguire queste operazioni:

- Assicurarsi che la configurazione di crittografia della tabella di destinazione sia identica alla configurazione della tabella di origine. In particolare, le due le tabelle devono avere le stesse colonne crittografate e le colonne devono essere crittografate usando gli stessi tipi di crittografia e le stesse chiavi di crittografia. Se una delle colonne di destinazione è crittografata in modo diverso dalla relativa colonna di origine corrispondente, non sarà possibile decrittografare i dati nella tabella di destinazione dopo l'operazione di copia. I dati risulteranno danneggiati.
- Configurare le due connessioni di database alla tabella di origine e la tabella di destinazione, senza abilitare Always Encrypted.
- Impostare l'opzione AllowEncryptedValueModifications. Per altre informazioni, vedere [Uso della copia bulk con il driver JDBC](using-bulk-copy-with-the-jdbc-driver.md).

> [!NOTE]
> Prestare attenzione quando si specifica AllowEncryptedValueModifications. Questa opzione può danneggiare il database perché Microsoft JDBC Driver per SQL Server non verifica se i dati vengono effettivamente crittografati o se vengono crittografati correttamente usando lo stesso tipo di crittografia, l'algoritmo e la chiave della colonna di destinazione.

## <a name="see-also"></a>Vedere anche

[Always Encrypted (Motore di database)](../../relational-databases/security/encryption/always-encrypted-database-engine.md)
