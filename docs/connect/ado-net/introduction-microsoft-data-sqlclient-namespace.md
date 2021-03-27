---
title: Introduzione allo spazio dei nomi Microsoft.Data.SqlClient
description: Informazioni sullo spazio dei nomi Microsoft.Data.SqlClient e su come rappresenti il modo preferibile per connettersi ad applicazioni SQL per .NET.
ms.date: 11/19/2020
ms.assetid: c18b1fb1-2af1-4de7-80a4-95e56fd976cb
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: 8fdb8f726fc33ccb5acc6129c693a5233f87e313
ms.sourcegitcommit: 524a0f0cc9533188f4b14d2e78ba1cfe816b3b9a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/27/2021
ms.locfileid: "105633283"
---
# <a name="introduction-to-microsoftdatasqlclient-namespace"></a>Introduzione allo spazio dei nomi Microsoft.Data.SqlClient

[!INCLUDE [Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

## <a name="release-notes-for-microsoftdatasqlclient-21"></a>Note sulla versione per Microsoft.Data.SqlClient 2.1

Le note sulla versione sono disponibili anche nel repository GitHub: [Note sulla versione 2.1](https://github.com/dotnet/SqlClient/tree/master/release-notes/2.1).

### <a name="new-features-in-21"></a>Nuove funzionalità di 2,1

### <a name="cross-platform-support-for-always-encrypted"></a>Supporto multipiattaforma per Always Encrypted

Microsoft. Data. SqlClient v 2.1 estende il supporto per Always Encrypted alle piattaforme seguenti:

| Supporto per Always Encrypted | Supporto per Always Encrypted con enclave sicura  | Framework di destinazione | Versione di Microsoft.Data.SqlClient | Sistema operativo |
|:--|:--|:--|:--:|:--:|
| Sì | Sì | .NET Framework 4.6+ | 1.1.0+ | Windows |
| Sì | Sì | .NET Core 2.1+ | 2.1.0+<sup>1</sup> | Windows, Linux, macOS |
| Sì | No<sup>2</sup> | .NET Standard 2.0 | 2.1.0+ | Windows, Linux, macOS |
| Sì | Sì | .NET Standard 2.1+ | 2.1.0+ | Windows, Linux, macOS |

> [!NOTE]
> <sup>1</sup> Prima di Microsoft.Data.SqlClient versione v2.1, Always Encrypted era supportato solo in Windows.
> <sup>2</sup> Always Encrypted con enclave sicure non è supportato in .NET Standard 2.0.

### <a name="azure-active-directory-device-code-flow-authentication"></a>Autenticazione con flusso di codice del dispositivo di Active Directory

Microsoft. Data. SqlClient v 2.1 fornisce il supporto per l'autenticazione "Flusso di codice del dispositivo" con MSAL.NET.
Documentazione di riferimento: [Flusso di concessione dell'autorizzazione del dispositivo OAuth 2.0](/azure/active-directory/develop/v2-oauth2-device-code)

Esempio di stringa di connessione:

`Server=<server>.database.windows.net; Authentication=Active Directory Device Code Flow; Database=Northwind;`

L'API seguente consente la personalizzazione del meccanismo di callback del flusso di codice del dispositivo:

```csharp
public class ActiveDirectoryAuthenticationProvider
{
    // For .NET Framework, .NET Core and .NET Standard targeted applications
    public void SetDeviceCodeFlowCallback(Func<DeviceCodeResult, Task> deviceCodeFlowCallbackMethod)
}
```

### <a name="azure-active-directory-managed-identity-authentication"></a>Autenticazione tramite identità gestite di Azure Active Directory

Microsoft.Data.SqlClient v2.1 introduce il supporto per l'autenticazione di Azure Active Directory tramite [identità gestite](/azure/active-directory/managed-identities-azure-resources/overview).

Sono supportate le parole chiave dei tipi di autenticazione seguenti:

- Identità gestita di Active Directory
- Active Directory MSI (per compatibilità incrociata con i driver MS SQL)

Esempi di stringhe di connessione:

```cs
// For System Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory MSI; Initial Catalog={db};"

// For System Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory Managed Identity; Initial Catalog={db};"

// For User Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory MSI; User Id={ObjectIdOfManagedIdentity}; Initial Catalog={db};"

// For User Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory Managed Identity; User Id={ObjectIdOfManagedIdentity}; Initial Catalog={db};"
```

### <a name="azure-active-directory-interactive-authentication-enhancements"></a>Miglioramenti dell'autenticazione interattiva di Azure Active Directory

Microsoft.Data.SqlClient v2.1 aggiunge le API seguenti per personalizzare l'esperienza di "Autenticazione interattiva di Active Directory":

```csharp
public class ActiveDirectoryAuthenticationProvider
{
    // For .NET Framework targeted applications only
    public void SetIWin32WindowFunc(Func<IWin32Window> iWin32WindowFunc);

    // For .NET Standard targeted applications only
    public void SetParentActivityOrWindowFunc(Func<object> parentActivityOrWindowFunc);

    // For .NET Framework, .NET Core and .NET Standard targeted applications
    public void SetAcquireAuthorizationCodeAsyncCallback(Func<Uri, Uri, CancellationToken, Task<Uri>> acquireAuthorizationCodeAsyncCallback);

    // For .NET Framework, .NET Core and .NET Standard targeted applications
    public void ClearUserTokenCache();
}
```

### <a name="sqlclientauthenticationproviders-configuration-section"></a>`SqlClientAuthenticationProviders` Sezione Configurazione

Microsoft.Data.SqlClient v2.1 introduce una nuova sezione di configurazione, `SqlClientAuthenticationProviders` (un clone della `SqlAuthenticationProviders` esistente). La sezione di configurazione esistente, `SqlAuthenticationProviders`, è ancora supportata per la compatibilità con le versioni precedenti quando viene definito il tipo appropriato.

La nuova sezione consente ai file di configurazione dell'applicazione di contenere sia una sezione SqlAuthenticationProviders per System.Data.SqlClient che una sezione SqlClientAuthenticationProviders per Microsoft.Data.SqlClient.

### <a name="azure-active-directory-authentication-using-an-application-client-id"></a>Autenticazione di Azure Active Directory con un ID client dell'applicazione

Microsoft.Data.SqlClient v2.1 introduce il supporto per il passaggio di un ID client dell'applicazione definito dall'utente a Microsoft Authentication Library. L'ID client dell'applicazione viene usato per l'autenticazione con Azure Active Directory.

Sono state introdotte le nuove API seguenti:

1. Un nuovo costruttore è stato introdotto in ActiveDirectoryAuthenticationProvider: \
_[Si applica a tutte le piattaforme .NET (.NET Framework, .NET Core e .NET Standard)]_

    ```csharp
    public ActiveDirectoryAuthenticationProvider(string applicationClientId)
    ```

    Utilizzo:

    ```csharp
    string APP_CLIENT_ID = "<GUID>";
    SqlAuthenticationProvider customAuthProvider = new ActiveDirectoryAuthenticationProvider(APP_CLIENT_ID);
    SqlAuthenticationProvider.SetProvider(SqlAuthenticationMethod.ActiveDirectoryInteractive, customAuthProvider);
    
    using (SqlConnection sqlConnection = new SqlConnection("<connection_string>")
    {
        sqlConnection.Open();
    }
    ```

2. Una nuova proprietà di configurazione è stata introdotta in `SqlAuthenticationProviderConfigurationSection` e `SqlClientAuthenticationProviderConfigurationSection`: \
_[Si applica a .NET Framework e .NET Core]_

    ```csharp
    internal class SqlAuthenticationProviderConfigurationSection : ConfigurationSection
    {
        ...
        [ConfigurationProperty("applicationClientId", IsRequired = false)]
        public string ApplicationClientId => this["applicationClientId"] as string;
    }
    
    // Inheritance
    internal class SqlClientAuthenticationProviderConfigurationSection : SqlAuthenticationProviderConfigurationSection
    { ... }
    ```

    Utilizzo:

    ```xml
    <configuration>
        <configSections>
            <section name="SqlClientAuthenticationProviders"
                             type="Microsoft.Data.SqlClient.SqlClientAuthenticationProviderConfigurationSection, Microsoft.Data.SqlClient" />
        </configSections>
        <SqlClientAuthenticationProviders applicationClientId ="<GUID>" />
    </configuration>
    
    <!--or-->
    
    <configuration>
        <configSections>
            <section name="SqlAuthenticationProviders"
                             type="Microsoft.Data.SqlClient.SqlAuthenticationProviderConfigurationSection, Microsoft.Data.SqlClient" />
        </configSections>
        <SqlAuthenticationProviders applicationClientId ="<GUID>" />
    </configuration>
    ```

### <a name="data-classification-v2-support"></a>Supporto della classificazione dei dati v2

Microsoft.Data.SqlClient v2.1 introduce il supporto per le informazioni sul "livello di riservatezza" della classificazione dei dati. Sono ora disponibili le nuove API seguenti:

```csharp
public class SensitivityClassification
{
    public SensitivityRank SensitivityRank;
}

public class SensitivityProperty
{
    public SensitivityRank SensitivityRank;
}

public enum SensitivityRank
{
    NOT_DEFINED = -1,
    NONE = 0,
    LOW = 10,
    MEDIUM = 20,
    HIGH = 30,
    CRITICAL = 40
}
```

### <a name="server-process-id-for-an-active-sqlconnection"></a>ID del processo server per un `SqlConnection` attivo

Microsoft.Data.SqlClient v2.1 introduce una nuova proprietà `SqlConnection`, `ServerProcessId`, in una connessione attiva.

```csharp
public class SqlConnection
{
    // Returns the server process Id (SPID) of the active connection.
    public int ServerProcessId;
}
```

### <a name="trace-logging-support-in-native-sni"></a>Supporto della registrazione delle tracce in SNI nativo

Microsoft.Data.SqlClient v2.1 estende l'implementazione di `SqlClientEventSource` esistente per abilitare la traccia eventi in SNI.dll. Gli eventi devono essere acquisiti usando uno strumento come Xperf.

La traccia può essere abilitata inviando un comando a `SqlClientEventSource` come illustrato di seguito:

```csharp
// Enables trace events:
EventSource.SendCommand(eventSource, (EventCommand)8192, null);

// Enables flow events:
EventSource.SendCommand(eventSource, (EventCommand)16384, null);

// Enables both trace and flow events:
EventSource.SendCommand(eventSource, (EventCommand)(8192 | 16384), null);
```

### <a name="command-timeout-connection-string-property"></a>Proprietà della stringa di connessione "timeout comando"

Microsoft.Data.SqlClient v2.1 introduce la proprietà della stringa di connessione "timeout comando" per sovrascrivere il valore predefinito di 30 secondi. Il timeout per i singoli comandi può essere sovrascritto tramite la proprietà `CommandTimeout` in SqlCommand.

Esempi di stringhe di connessione:

`"Server={serverURL}; Initial Catalog={db}; Integrated Security=true; Command Timeout=60"`

### <a name="removal-of-symbols-from-native-sni"></a>Rimozione di simboli da SNI nativi

Con Microsoft.Data.SqlClient v2.1 sono stati rimossi i simboli introdotti nella versione [v2.0.0](https://www.nuget.org/packages/Microsoft.Data.SqlClient.SNI/2.0.0) dal NuGet [Microsoft.Data.SqlClient.SNI.runtime](https://www.nuget.org/packages/Microsoft.Data.SqlClient.SNI.runtime) a partire dalla versione [v 2.1.1](https://www.nuget.org/packages/Microsoft.Data.SqlClient.SNI.runtime/2.1.1). I simboli pubblici vengono ora pubblicati in Microsoft symbols server per strumenti come BinSkim che richiedono l'accesso ai simboli pubblici.

### <a name="source-linking-of-microsoftdatasqlclient-symbols"></a>Collegamento di origine dei simboli Microsoft.Data.SqlClient

A partire da Microsoft.Data.SqlClient v 2.1, i simboli Microsoft. Data. SqlClient sono collegati al codice sorgente e pubblicati nel server Microsoft symbols per un'esperienza di debug avanzata senza necessità di scaricare il codice sorgente.

## <a name="release-notes-for-microsoftdatasqlclient-20"></a>Note sulla versione per Microsoft.Data.SqlClient 2.0

Le note sulla versione sono disponibili anche nel repository GitHub: [Note sulla versione 2.0](https://github.com/dotnet/SqlClient/tree/master/release-notes/2.0).

### <a name="breaking-changes-in-20"></a>Modifiche di rilievo in 2,0

- Il modificatore di accesso per l'interfaccia del provider di enclave `SqlColumnEncryptionEnclaveProvider` è stato modificato da `public` a `internal`.
- Le costanti nella classe `SqlClientMetaDataCollectionNames` sono state aggiornate in modo da riflettere le modifiche apportate a SQL Server.
- Il driver eseguirà ora la convalida del certificato del server quando l'istanza di SQL Server di destinazione applica la crittografia TLS, che è l'impostazione predefinita per le connessioni di Azure.
- `SqlDataReader.GetSchemaTable()` restituisce ora una stringa `DataTable` vuota invece di `null`.
- Il driver esegue ora l'arrotondamento delle cifre decimali in modo che corrisponda al comportamento di SQL Server. Per la compatibilità con le versioni precedenti, è possibile abilitare il comportamento di troncamento precedente usando un'opzione di AppContext.
- Per .NET Framework applicazioni che utilizzano **Microsoft. Data. SqlClient**, i file di SNI.dll scaricati in precedenza `bin\x64` nelle `bin\x86` cartelle e sono ora denominati `Microsoft.Data.SqlClient.SNI.x64.dll` e `Microsoft.Data.SqlClient.SNI.x86.dll` e verranno scaricati nella `bin` Directory.
- Nuovi sinonimi delle proprietà della stringa di connessione sostituiranno le proprietà obsolete durante il recupero della stringa di connessione da `SqlConnectionStringBuilder` per coerenza. [Altre informazioni](#new-connection-string-property-synonyms)

### <a name="new-features-in-20"></a>Nuove funzionalità di 2,0

In Microsoft. Data. SqlClient 2,0 sono state introdotte le nuove funzionalità seguenti.

#### <a name="dns-failure-resiliency"></a>Resilienza agli errori DNS

Il driver memorizza ora nella cache gli indirizzi IP da ogni connessione riuscita a un endpoint SQL Server che supporta la funzionalità. Se si verifica un errore di risoluzione DNS durante un tentativo di connessione, il driver tenta di stabilire una connessione usando un indirizzo IP memorizzato nella cache per il server, se presente.

#### <a name="eventsource-tracing"></a>Traccia EventSource

Questa versione introduce il supporto per l'acquisizione dei log di traccia eventi per il debug delle applicazioni. Per acquisire questi eventi, le applicazioni client devono restare in ascolto degli eventi dall'implementazione EventSource di SqlClient:

```csharp
Microsoft.Data.SqlClient.EventSource
```

Per altre informazioni, vedere [Come abilitare la traccia eventi in SqlClient](enable-eventsource-tracing.md).

#### <a name="enabling-managed-networking-on-windows"></a>Abilitazione di reti gestite in Windows

Una nuova opzione di AppContext, **"Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows"** , consente l'uso di un'implementazione di SNI gestita in Windows per scopi di test e debug. Questa opzione attiva o disattiva il comportamento del driver in modo da usare uno SNI gestito in progetti .NET Core 2.1+ e .NET Standard 2.0+ in Windows, eliminando tutte le dipendenze dalle librerie native per la libreria Microsoft. Data.SqlClient.

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows", true);
```

Per un elenco completo delle opzioni disponibili nel driver, vedere [Opzioni di AppContext in SqlClient](appcontext-switches.md).

#### <a name="enabling-decimal-truncation-behavior"></a>Abilitazione del comportamento di troncamento decimale

Per impostazione predefinita, i dati decimali vengono arrotondati dal driver allo stesso modo che in SQL Server. Per la compatibilità con le versioni precedenti, è possibile impostare l'opzione di AppContext **"Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal"** su **true**.

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal", true);
```

#### <a name="new-connection-string-property-synonyms"></a>Nuovi sinonimi delle proprietà della stringa di connessione

Sono stati aggiunti nuovi sinonimi per le proprietà della stringa di connessione esistenti seguenti per evitare problemi di spaziatura tra le proprietà con più di una parola. I nomi di proprietà precedenti continueranno a essere supportati per la compatibilità con le versioni precedenti, ma le nuove proprietà della stringa di connessione verranno ora incluse durante il recupero della stringa di connessione da [SqlConnectionStringBuilder](/dotnet/api/microsoft.data.sqlclient.sqlconnectionstringbuilder).

|Proprietà della stringa di connessione esistente|Nuovo sinonimo|
|-----------------------------------|-----------|
| ApplicationIntent | Finalità dell'applicazione |
| ConnectRetryCount | Numero di tentativi di connessione |
| ConnectRetryInterval | Intervallo dei tentativi di connessione |
| PoolBlockingPeriod | Periodo di blocco del pool |
| MultipleActiveResultSets | Più set di risultati attivi |
| MultiSubnetFailover | Failover di più subnet |
| TransparentNetworkIPResolution | Risoluzione dell'IP di rete trasparente |
| TrustServerCertificate | TrustServerCertificate |

#### <a name="sqlbulkcopy-rowscopied-property"></a>Proprietà RowsCopied di SqlBulkCopy

La proprietà RowsCopied fornisce accesso in sola lettura al numero di righe elaborate durante l'operazione di copia bulk in corso. Questo valore non deve necessariamente essere uguale al numero finale di righe aggiunte alla tabella di destinazione.

#### <a name="connection-open-overrides"></a>Override dell'apertura della connessione

È possibile eseguire l'override del comportamento predefinito di SqlConnection.Open() per disabilitare il ritardo di dieci secondi e i tentativi di connessione automatici attivati da errori temporanei.

```csharp
using SqlConnection sqlConnection = new SqlConnection("Data Source=(local);Integrated Security=true;Initial Catalog=AdventureWorks;");
sqlConnection.Open(SqlConnectionOverrides.OpenWithoutRetry);
```

> [!NOTE]
> Si noti che questo override può essere applicato solo a SqlConnection.Open() e non a SqlConnection.OpenAsync().

#### <a name="username-support-for-active-directory-interactive-mode"></a>Supporto di nomi utente per la modalità interattiva di Azure Active Directory

È possibile specificare un nome utente nella stringa di connessione quando si usa la modalità di autenticazione interattiva di Azure Active Directory per .NET Framework e .NET Core

Impostare un nome utente usando la proprietà **User ID** o **UID** della stringa di connessione:

```csharp
"Server=<server name>; Database=<db name>; Authentication=Active Directory Interactive; User Id=<username>;"
```

#### <a name="order-hints-for-sqlbulkcopy"></a>Suggerimenti per l'ordinamento per SqlBulkCopy

È possibile fornire suggerimenti per l'ordinamento per migliorare le prestazioni delle operazioni di copia bulk sulle tabelle con indici cluster. Per altre informazioni, vedere la sezione [Operazioni di copia bulk](sql/bulk-copy-order-hints.md).

#### <a name="sni-dependency-changes"></a>Modifiche alle dipendenze di SNI

Microsoft.Data.SqlClient (.NET Core e .NET Standard) in Windows è ora dipendente da **Microsoft.Data.SqlClient.SNI.runtime**, in sostituzione alla precedente dipendenza da **runtime.native.System.Data.SqlClient.SNI**. La nuova dipendenza aggiunge il supporto per la piattaforma ARM insieme alle piattaforme già supportate ARM64, x64 e x86 in Windows.

## <a name="release-notes-for-microsoftdatasqlclient-110"></a>Note sulla versione per Microsoft.Data.SqlClient 1.1.0

Le note sulla versione sono disponibili anche nel repository GitHub: [Note sulla versione 1.1](https://github.com/dotnet/SqlClient/tree/master/release-notes/1.1).

### <a name="new-features-in-11"></a>Nuove funzionalità di 1,1

#### <a name="always-encrypted-with-secure-enclaves"></a>Always Encrypted con enclave sicuri

Always Encrypted è disponibile a partire da Microsoft SQL Server 2016. Gli enclave sicuri sono disponibili a partire da Microsoft SQL Server 2019. Per usare la funzionalità di enclave, le stringhe di connessione devono includere il protocollo di attestazione e l'URL di attestazione necessari. Ad esempio:

```csharp
"Attestation Protocol=HGS;Enclave Attestation Url=<attestation_url_for_HGS>"
```

Per altre informazioni, vedere:

- [Supporto di SqlClient per Always Encrypted](sql/sqlclient-support-always-encrypted.md)
- [Esercitazione: Sviluppare un'applicazione .NET usando Always Encrypted con enclave sicure](sql/tutorial-always-encrypted-enclaves-develop-net-apps.md)

## <a name="release-notes-for-microsoftdatasqlclient-10"></a>Note sulla versione per Microsoft.Data.SqlClient 1.0

La versione iniziale dello spazio dei nomi Microsoft. Data. SqlClient offre più funzionalità rispetto allo spazio dei nomi System. Data. SqlClient esistente.
Le note sulla versione sono disponibili anche nel repository GitHub: [Note sulla versione 1.0](https://github.com/dotnet/SqlClient/tree/master/release-notes/1.0).

### <a name="new-features-in-10"></a>Nuove funzionalità di 1,0

#### <a name="new-features-over-net-framework-472-systemdatasqlclient"></a>Nuove funzionalità rispetto a .NET Framework 4.7.2 System.Data.SqlClient

- **Classificazione dei dati**: disponibile nel database SQL di Azure e in Microsoft SQL Server 2019.

- **Supporto UTF-8**: disponibile in Microsoft SQL Server 2019.

#### <a name="new-features-over-net-core-22-systemdatasqlclient"></a>Nuove funzionalità rispetto a .NET Core 2.2 System.Data.SqlClient

- **Classificazione dei dati**: disponibile nel database SQL di Azure e in Microsoft SQL Server 2019.

- **Supporto UTF-8**: disponibile in Microsoft SQL Server 2019.

- **Autenticazione**: modalità di autenticazione con password di Active Directory.

### <a name="data-classification"></a>Classificazione dei dati

La classificazione dei dati offre un nuovo set di API che espongono informazioni di sola lettura sulla riservatezza e classificazione dei dati relative agli oggetti recuperati tramite SqlDataReader quando l'origine sottostante supporta la funzionalità e contiene metadati di [riservatezza e classificazione dei dati](../../relational-databases/security/sql-data-discovery-and-classification.md). Vedere l'applicazione di esempio in [Data Discovery and Classification in SqlClient](https://github.com/dotnet/SqlClient/tree/master/release-notes/1.1).

```csharp
public class SqlDataReader
{
    public Microsoft.Data.SqlClient.DataClassification.SensitivityClassification SensitivityClassification
}

namespace Microsoft.Data.SqlClient.DataClassification
{
    public class ColumnSensitivity
    {
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.SensitivityProperty> SensitivityProperties
    }
    public class InformationType
    {
        public string Id
        public string Name
    }
    public class Label
    {
        public string Id
        public string Name
    }
    public class SensitivityClassification
    {
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.ColumnSensitivity> ColumnSensitivities
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.InformationType> InformationTypes
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.Label> Labels
    }
    public class SensitivityProperty
    {
        public Microsoft.Data.SqlClient.DataClassification.InformationType InformationType
        public Microsoft.Data.SqlClient.DataClassification.Label Label
    }
}
```

### <a name="utf-8-support"></a>Supporto UTF-8

Il supporto UTF-8 non richiede modifiche al codice dell'applicazione. Queste modifiche di SqlClient consentono di ottimizzare la comunicazione tra il client e il server quando il server supporta UTF-8 e le regole di confronto delle colonne sottostanti usano UTF-8. Vedere la sezione UTF-8 in [Novità di SQL Server 2019](../../sql-server/what-s-new-in-sql-server-ver15.md).

### <a name="always-encrypted-with-secure-enclaves"></a>Crittografia sempre attiva con enclave sicure

In generale, la documentazione esistente che usa System.Data.SqlClient in .NET Framework **e i provider dell'archivio chiavi master delle colonne predefiniti** può ora essere usata anche con .NET Core.

 [Sviluppare con Always Encrypted e il provider di dati .NET Framework](../../relational-databases/security/encryption/develop-using-always-encrypted-with-net-framework-data-provider.md)

 [Always Encrypted: proteggere i dati sensibili e archiviare le chiavi di crittografia nell'archivio certificati di Windows](/azure/sql-database/sql-database-always-encrypted)

### <a name="authentication"></a>Authentication

È possibile specificare modalità di autenticazione diverse usando l'opzione della stringa di connessione _Authentication_. Per altre informazioni, vedere la [documentazione di SqlAuthenticationMethod](/dotnet/api/system.data.sqlclient.sqlauthenticationmethod?view=netframework-4.7.2&preserve-view=true).

> [!NOTE]
> I provider dell'archivio chiavi personalizzati, come il provider Azure Key Vault, dovranno essere aggiornati per supportare Microsoft.Data.SqlClient. In modo analogo, anche i provider di enclave dovranno essere aggiornati per supportare Microsoft.Data.SqlClient.
> Always Encrypted è supportato solo su destinazioni .NET Framework e .NET Core. Non è supportato su .NET Standard perché in .NET Standard mancano alcune dipendenze di crittografia.
