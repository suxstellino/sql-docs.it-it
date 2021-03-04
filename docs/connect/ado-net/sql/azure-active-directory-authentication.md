---
title: Uso dell'autenticazione di Azure Active Directory con SqlClient
description: Viene descritto come usare le modalità di autenticazione di Azure Active Directory supportate per connettersi alle origini dati di Azure SQL con SqlClient
ms.date: 11/20/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: karinazhou
ms.author: v-jizho2
ms.reviewer: v-daenge
ms.openlocfilehash: c57c2d10854ed902a6230eafc3a912cd0508c989
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835998"
---
# <a name="using-azure-active-directory-authentication-with-sqlclient"></a>Uso dell'autenticazione di Azure Active Directory con SqlClient

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE [Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

Questo articolo descrive come connettersi alle origini dati di Azure SQL usando l'autenticazione di Azure Active Directory (Azure AD) da un'applicazione .NET con SqlClient.

L'autenticazione di Azure AD usa le identità in Azure AD per accedere alle origini dati di Azure SQL, ad esempio il database SQL di Azure, l'istanza gestita di SQL di Azure e Azure Synapse Analytics. Lo spazio dei nomi **Microsoft.Data.SqlClient** consente alle applicazioni client di specificare le credenziali di Azure AD in modalità di autenticazione diverse quando si connettono al database SQL di Azure.

Quando si imposta la proprietà di connessione `Authentication` nella stringa di connessione, il client può scegliere una modalità di autenticazione preferita per Azure AD in base al valore specificato:

- La versione meno recente di **Microsoft.Data.SqlClient** supporta `Active Directory Password` per .NET Framework, .NET Core e .NET Standard. Supporta inoltre l'autenticazione `Active Directory Integrated` e l'autenticazione `Active Directory Interactive` per .NET Framework. 

- A partire da **Microsoft.Data.SqlClient** 2.0.0 il supporto per l'autenticazione `Active Directory Integrated` e l'autenticazione `Active Directory Interactive` è stato esteso in .NET Framework, .NET Core e .NET Standard. 

  Viene inoltre aggiunta in SqlClient 2.0.0 una nuova modalità di autenticazione `Active Directory Service Principal` che consente di usare l'ID client e il segreto di un'identità dell'entità servizio per eseguire l'autenticazione. 

- Altre modalità di autenticazione sono state aggiunte in **Microsoft.Data.SqlClient** 2.1.0, tra cui `Active Directory Device Code Flow` e `Active Directory Managed Identity` (nota anche come `Active Directory MSI`). Queste nuove modalità consentono all'applicazione di acquisire un token di accesso per la connessione al server. 

Per informazioni sull'autenticazione di Azure AD oltre a quanto descritto nelle sezioni seguenti, vedere [Connessione al database SQL usando l'autenticazione di Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview).

## <a name="setting-azure-active-directory-authentication"></a>Impostazione dell'autenticazione di Azure Active Directory

Quando si connette alle origini dati di Azure SQL usando l'autenticazione di Azure AD, l'applicazione deve specificare una modalità di autenticazione valida. Nella tabella seguente sono elencate le modalità di autenticazione supportate. L'applicazione specifica una modalità utilizzando la proprietà di connessione `Authentication` nella stringa di connessione.

| Valore | Description  | Framework    | Versione di Microsoft.Data.SqlClient |
|:--|:--|:--|:--:|
| Password di Active Directory | Eseguire l'autenticazione con un'identità di Azure AD usando un nome utente e una password | .NET Framework 4.6+, .NET Core 2.1+, .NET Standard 2.0+  | 1.0+|
| Integrato in Active Directory |Eseguire l'autenticazione con un'identità di Azure AD usando l'autenticazione integrata | .NET Framework 4.6+, .NET Core 2.1+, .NET Standard 2.0+ | 2.0.0+<sup>1</sup> |
| Autenticazione interattiva di Active Directory | Eseguire l'autenticazione con un'identità di Azure AD usando l'autenticazione interattiva | .NET Framework 4.6+, .NET Core 2.1+, .NET Standard 2.0+ | 2.0.0+<sup>1</sup> |
| Entità servizio di Active Directory | Eseguire l'autenticazione con un'identità di Azure AD usando l'ID client e il segreto di un'identità dell'entità servizio | .NET Framework 4.6+, .NET Core 2.1+, .NET Standard 2.0+ | 2.0.0+ |
| Flusso di codice del dispositivo di Active Directory | Eseguire l'autenticazione con un'identità di Azure AD usando la modalità Flusso di codice del dispositivo | .NET Framework 4.6+, .NET Core 2.1+, .NET Standard 2.0+ | 2.1.0+ |
| Identità gestita di Active Directory, <br>MSI di Active Directory | Eseguire l'autenticazione con un'identità di Azure AD usando un'identità gestita assegnata dal sistema o assegnata dall'utente | .NET Framework 4.6+, .NET Core 2.1+, .NET Standard 2.0+ | 2.1.0+ |

<sup>1</sup> prima di **Microsoft. Data. SqlClient** 2.0.0 `Active Directory Integrated` e le `Active Directory Interactive` modalità di autenticazione sono supportate solo in .NET Framework 4.6 +.

## <a name="using-active-directory-password-authentication"></a>Uso dell'autenticazione della password di Active Directory

La modalità di autenticazione `Active Directory Password` supporta l'autenticazione per le origini dati di Azure con Azure AD per gli utenti di Azure AD nativi o federati. Quando si usa questa modalità, è necessario specificare le credenziali utente nella stringa di connessione. Nell'esempio seguente viene illustrato come usare l'autenticazione `Active Directory Password`.

```c#
// Use your own server, database, user ID, and password.
string ConnectionString = @"Server=demo.database.windows.net; Authentication=Active Directory Password; Database=testdb; User Id=user@domain.com; Password=***";

using (SqlConnection conn = new SqlConnection(ConnectionString)) {
    conn.Open();
}
```


## <a name="using-active-directory-integrated-authentication"></a>Uso dell'autenticazione integrata di Active Directory

Per usare la modalità di autenticazione `Active Directory Integrated`, è necessario eseguire la federazione dell'istanza locale di Active Directory con Azure AD nel cloud. La federazione può essere eseguita ad esempio con Active Directory Federation Services (AD FS).

Con questa modalità, quando si accede a un computer aggiunto a un dominio è possibile accedere alle origini dati di Azure SQL senza che vengano richieste le credenziali. Non è possibile specificare nome utente e password nella stringa di connessione per le applicazioni .NET Framework. Il nome utente è facoltativo nella stringa di connessione per le applicazioni .NET Core e .NET Standard. Non è possibile impostare la proprietà `Credential` di SqlConnection in questa modalità. 

Il frammento di codice seguente è un esempio di utilizzo dell'autenticazione `Active Directory Integrated`.

```c#
// Use your own server and database.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Integrated; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

// User ID is optional for .NET Core and .NET Standard.
string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory Integrated; Database=testdb; User Id=user@domain.com";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```


## <a name="using-active-directory-interactive-authentication"></a>Uso dell'autenticazione interattiva di Active Directory

L'autenticazione `Active Directory Interactive` supporta la tecnologia di autenticazione a più fattori per la connessione alle origini dati di Azure SQL. Se si specifica questa modalità di autenticazione nella stringa di connessione, viene visualizzata una schermata di autenticazione di Azure in cui viene chiesto all'utente di immettere credenziali valide. Non è possibile specificare la password nella stringa di connessione. 

Non è possibile impostare la proprietà `Credential` di SqlConnection in questa modalità. Con **Microsoft. Data. SqlClient** 2.0.0 e versioni successive, il nome utente è consentito nella stringa di connessione quando si è in modalità interattiva. 

Nell'esempio seguente viene illustrato come usare l'autenticazione `Active Directory Interactive`.

```c#
// Use your own server, database, and user ID.
// User ID is optional.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Interactive; Database=testdb; User Id=user@domain.com";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

// User ID is not provided.
string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory Interactive; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```


## <a name="using-active-directory-service-principal-authentication"></a>Uso dell'autenticazione con entità servizio di Active Directory

In modalità di autenticazione `Active Directory Service Principal` l'applicazione client può connettersi alle origini dati di Azure SQL specificando l'ID client e il segreto di un'identità dell'entità servizio. L'autenticazione con entità servizio prevede quanto segue:
1. Configurazione di una registrazione dell'app con un segreto.
1. Concessione delle autorizzazioni all'app nell'istanza del database SQL di Azure.
1. Connessione con le credenziali corrette. 

Nell'esempio seguente viene illustrato come usare l'autenticazione `Active Directory Service Principal`.

```c#
// Use your own server, database, app ID, and secret.
string ConnectionString = @"Server=demo.database.windows.net; Authentication=Active Directory Service Principal; Database=testdb; User Id=AppId; Password=secret";

using (SqlConnection conn = new SqlConnection(ConnectionString)) {
    conn.Open();
}
```


## <a name="using-active-directory-device-code-flow-authentication"></a>Uso dell'autenticazione con flusso di codice del dispositivo di Active Directory

Con [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) per .NET (MSAL.NET), l'autenticazione `Active Directory Device Code Flow` consente all'applicazione client di connettersi alle origini dati di Azure SQL da dispositivi e sistemi operativi che non hanno un Web browser interattivo. L'autenticazione interattiva verrà eseguita in un altro dispositivo. Per altre informazioni sull'autenticazione con flusso di codice del dispositivo, vedere [Flusso di codice del dispositivo OAuth 2.0](/azure/active-directory/develop/v2-oauth2-device-code). 

Non è possibile impostare la proprietà `Credential` di `SqlConnection` se questa modalità è in uso. Inoltre, il nome utente e la password non devono essere specificati nella stringa di connessione. 

Il frammento di codice seguente è un esempio di utilizzo dell'autenticazione `Active Directory Device Code Flow`.

```c#
// Use your own server and database.
string ConnectionString = @"Server=demo.database.windows.net; Authentication=Active Directory Device Code Flow; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString)) {
    conn.Open();
}
```


## <a name="using-active-directory-managed-identity-authentication"></a>Uso dell'autenticazione con Identità gestite di Active Directory

*Identità gestite* per le risorse di Azure è il nuovo nome del servizio precedentemente noto come identità del servizio gestita (MSI). Quando un'applicazione client usa una risorsa di Azure per accedere a un servizio di Azure che supporta l'autenticazione di Azure AD, è possibile usare le identità gestite per l'autenticazione specificando un'identità per la risorsa di Azure in Azure AD. È quindi possibile usare tale identità per ottenere i token di accesso. Questo metodo di autenticazione può eliminare la necessità di gestire le credenziali e i segreti.

Sono disponibili due tipi di identità gestite:

- _Identità gestita assegnata dal sistema_: viene creata in un'istanza del servizio in Azure AD. È associata al ciclo di vita di quell'istanza del servizio. 
- _Identità gestita assegnata dall'utente_: viene creata come risorsa di Azure autonoma. Può essere assegnata a una o più istanze di un servizio di Azure. 

Per altre informazioni sulle identità gestite, vedere [Informazioni sulle identità gestite per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/overview).

A partire da **Microsoft.Data.SqlClient** 2.1.0 il driver supporta l'autenticazione nel database SQL di Azure, in Azure Synapse Analytics e in Istanza gestita di SQL di Azure con l'acquisizione di token di accesso attraverso l'identità gestita. Per usare questa autenticazione, specificare `Active Directory Managed Identity` o `Active Directory MSI` nella stringa di connessione e non è richiesta alcuna password. 

Inoltre, non è possibile impostare la proprietà `Credential` di `SqlConnection` in questa modalità. È necessario specificare un nome utente per un'identità gestita assegnata dall'utente. 

Nell'esempio seguente viene illustrato l'uso dell'autenticazione `Active Directory Managed Identity` con un'identità gestita assegnata dal sistema.

```c#
// For system-assigned managed identity
// Use your own server and database.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Managed Identity; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory MSI; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```

L'esempio seguente illustra l'uso dell'autenticazione `Active Directory Managed Identity` con un'identità gestita assegnata dall'utente.

```c#
// For user-assigned managed identity
// Use your own server, database, and user ID.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Managed Identity; User Id=ObjectIdOfManagedIdentity; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory MSI; User Id=ObjectIdOfManagedIdentity; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```


## <a name="customizing-active-directory-authentication"></a>Personalizzazione dell'autenticazione di Active Directory

Oltre a usare l'autenticazione di Active Directory integrata nel driver, **Microsoft.Data.SqlClient** 2.1.0 e versioni successive offrono alle applicazioni la possibilità di personalizzare l'autenticazione di Active Directory. La personalizzazione è basata sulla classe `ActiveDirectoryAuthenticationProvider`, derivata dalla classe astratta [`SqlAuthenticationProvider`](/dotnet/api/system.data.sqlclient.sqlauthenticationprovider). 

Durante l'autenticazione di Active Directory l'applicazione client può definire la propria classe `ActiveDirectoryAuthencationProvider` nel modo seguente:

- Usando un metodo di callback personalizzato.
- Passando un ID client dell'applicazione alla libreria MSAL usando il driver SqlClient per recuperare i token di accesso.

Nell'esempio seguente viene illustrato come usare un callback personalizzato quando l'autenticazione `Active Directory Device Code Flow` è in uso. 

[!code-csharp [AADAuthenticationCustomDeviceFlowCallback#1](~/../sqlclient/doc/samples/AADAuthenticationCustomDeviceFlowCallback.cs#1)]

Con una classe `ActiveDirectoryAuthenticationProvider` personalizzata, un ID client dell'applicazione definito dall'utente può essere passato a SqlClient quando è in uso una modalità di autenticazione di Active Directory supportata. Le modalità di autenticazione di Active Directory supportate includono `Active Directory Password`, `Active Directory Integrated`, `Active Directory Interactive`, `Active Directory Service Principal` e `Active Directory Device Code Flow`.

L'ID client dell'applicazione può essere configurato anche usando `SqlAuthenticationProviderConfigurationSection` o `SqlClientAuthenticationProviderConfigurationSection`. La proprietà di configurazione `applicationClientId` si applica a .NET Framework 4.6 + e .NET Core 2.1 +.

Il frammento di codice seguente è un esempio di utilizzo di una classe `ActiveDirectoryAuthenticationProvider` personalizzata con un ID client dell'applicazione definito dall'utente quando è in uso l'autenticazione `Active Directory Interactive`.

[!code-csharp [ApplicationClientIdAzureAuthenticationProvider#1](~/../sqlclient/doc/samples/ApplicationClientIdAzureAuthenticationProvider.cs#1)]

L'esempio seguente illustra come configurare un ID client dell'applicazione in una sezione di configurazione.

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

## <a name="support-for-a-custom-sql-authentication-provider"></a>Supporto per un provider di autenticazione SQL personalizzato

Grazie a una maggiore flessibilità, l'applicazione client può usare anche il proprio provider per l'autenticazione di Active Directory anziché usare la classe `ActiveDirectoryAuthenticationProvider`. Il provider di autenticazione personalizzato deve essere una sottoclasse di `SqlAuthenticationProvider` con i metodi sottoposti a override. 

Nell'esempio seguente viene illustrato come usare un nuovo provider di autenticazione per l'autenticazione `Active Directory Device Code Flow`.

[!code-csharp [CustomDeviceCodeFlowAzureAuthenticationProvider#1](~/../sqlclient/doc/samples/CustomDeviceCodeFlowAzureAuthenticationProvider.cs#1)]

Oltre a migliorare l'esperienza di autenticazione `Active Directory Interactive`, **Microsoft.Data.SqlClient** 2.1.0 e versioni successive offrono le seguenti API per le applicazioni client per personalizzare l'autenticazione interattiva e l'autenticazione con flusso di codice del dispositivo.

```c#
public class ActiveDirectoryAuthenticationProvider
{
    // For .NET Framework targeted applications only
    // Sets a reference to the current System.Windows.Forms.IWin32Window that triggers the browser to be shown. 
    // Used to center the browser pop-up onto this window.
    public void SetIWin32WindowFunc(Func<IWin32Window> iWin32WindowFunc);

    // For .NET Standard targeted applications only
    // Sets a reference to the ViewController (if using Xamarin.iOS), Activity (if using Xamarin.Android) IWin32Window, or IntPtr (if using .NET Framework). 
    // Used for invoking the browser for Active Directory Interactive authentication.
    public void SetParentActivityOrWindowFunc(Func<object> parentActivityOrWindowFunc);

    // For .NET Framework, .NET Core, and .NET Standard targeted applications
    // Sets a callback method that's invoked with a custom web UI instance that will let the user sign in with Azure AD, present consent if needed, and get back the authorization code. 
    // Applicable when working with Active Directory Interactive authentication.
    public void SetAcquireAuthorizationCodeAsyncCallback(Func<Uri, Uri, CancellationToken, Task<Uri>> acquireAuthorizationCodeAsyncCallback);

    // For .NET Framework, .NET Core, and .NET Standard targeted applications
    // Clears cached user tokens from the token provider.
    public static void ClearUserTokenCache();
}
```


## <a name="see-also"></a>Vedere anche
- [Oggetti applicazione e oggetti entità servizio in Azure Active Directory](/azure/active-directory/develop/app-objects-and-service-principals)
- [Flussi di autenticazione](/azure/active-directory/develop/msal-authentication-flows)
