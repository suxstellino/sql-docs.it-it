---
title: Uso di Azure Active Directory
description: Informazioni sui metodi di autenticazione Azure Active Directory disponibili in Microsoft OLE DB Driver per SQL Server che consentono la connessione ai database SQL di Azure.
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
author: bazizi
ms.author: v-beaziz
ms.openlocfilehash: f75cfc74616a58975bedf2dc7b2de342c0c57066
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837370"
---
# <a name="using-azure-active-directory"></a>Uso di Azure Active Directory
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

## <a name="purpose"></a>Scopo


A partire dalla versione [18.2.1](../release-notes-for-oledb-driver-for-sql-server.md#1821), Microsoft OLE DB Driver per SQL Server consente alle applicazioni OLE DB di connettersi a un'istanza del database SQL di Azure usando un'identità federata. I nuovi metodi di autenticazione includono:
- ID di accesso e password di Azure Active Directory
- Token di accesso di Azure Active Directory
- Autenticazione integrata di Azure Active Directory
- ID di accesso e password di SQL


La versione [18.3.0](../release-notes-for-oledb-driver-for-sql-server.md#1830) aggiunge il supporto dei metodi di autenticazione seguenti:
- Autenticazione interattiva di Azure Active Directory
- Autenticazione tramite identità gestite di Azure Active Directory

La versione [18.5.0](../release-notes-for-oledb-driver-for-sql-server.md#1850) aggiunge il supporto dei metodi di autenticazione seguenti:
- Autenticazione tramite entità servizio di Azure Active Directory

> [!NOTE]
> L'uso delle modalità di autenticazione seguenti con `DataTypeCompatibility` (o la proprietà corrispondente) impostata su `80` **non** è supportato:
> - Autenticazione di Azure Active Directory con ID di accesso e password
> - Autenticazione di Azure Active Directory con token di accesso
> - Autenticazione integrata di Azure Active Directory
> - Autenticazione interattiva di Azure Active Directory
> - Autenticazione tramite identità gestite di Azure Active Directory
> - Autenticazione tramite entità servizio di Azure Active Directory

## <a name="connection-string-keywords-and-properties"></a>Parole chiave e proprietà delle stringhe di connessione
Per supportare l'autenticazione di Azure Active Directory sono state introdotte le parole chiave della stringa di connessione seguenti:

|Parola chiave stringa di connessione|Proprietà di connessione|Descrizione|
|---               |---                |---        |
|Token di accesso|SSPROP_AUTH_ACCESS_TOKEN|Specifica un token di accesso per eseguire l'autenticazione in Azure Active Directory. |
|Authentication|SSPROP_AUTH_MODE|Specifica il metodo di autenticazione da usare.|

Per altre informazioni sulle nuove parole chiave/proprietà, vedere le pagine seguenti:
- [Utilizzo delle parole chiave delle stringhe di connessione con driver OLE DB per SQL Server](../applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md)
- [Proprietà di inizializzazione e di autorizzazione](../ole-db-data-source-objects/initialization-and-authorization-properties.md)

## <a name="encryption-and-certificate-validation"></a>Crittografia e convalida di certificati
Questa sezione descrive le modifiche apportate al comportamento di crittografia e convalida dei certificati. Queste modifiche sono rese **effettive** solo quando vengono usate le nuove parole chiave della stringa di connessione per l'autenticazione o il token di accesso (o le proprietà corrispondenti).

### <a name="encryption"></a>Crittografia
Per migliorare la sicurezza, quando vengono usate le nuove parole chiave/proprietà di connessione, il driver sostituisce il valore di crittografia predefinito impostandolo su `yes`. La sostituzione viene eseguita al momento dell'inizializzazione dell'oggetto di origine dati. Se la crittografia viene impostata prima dell'inizializzazione, il valore viene rispettato e non sovrascritto.

> [!NOTE]   
> Nelle applicazioni ADO e nelle applicazioni che ottengono l'interfaccia `IDBInitialize` tramite `IDataInitialize::GetDataSource`, il componente di base che implementa l'interfaccia imposta in modo esplicito la crittografia sul valore predefinito di `no`. Di conseguenza, le nuove parole chiave/proprietà di autenticazione rispettano questa impostazione e il valore di crittografia **non** viene sovrascritto. Pertanto, è **consigliabile** che queste applicazioni impostino in modo esplicito `Use Encryption for Data=true` per la sostituzione del valore predefinito.

### <a name="certificate-validation"></a>Convalida del certificato
Per migliorare la sicurezza, le nuove parole chiave/proprietà di connessione rispettano l'impostazione di `TrustServerCertificate` (e le parole chiave/proprietà della stringa di connessione corrispondenti) **indipendentemente dall'impostazione della crittografia client**. Di conseguenza, il certificato del server viene convalidato per impostazione predefinita.

> [!NOTE]   
> La convalida del certificato può anche essere controllata tramite il campo `Value` della voce del Registro di sistema `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSSQLServer\Client\SNI18.0\GeneralFlags\Flag2`. I valori validi sono `0` o `1`. Il driver OLE DB sceglie l'opzione più sicura tra le impostazioni del Registro di sistema e le impostazioni delle parole chiave/proprietà di connessione. Ovvero, il driver convaliderà il certificato del server a condizione che almeno una delle impostazioni del Registro di sistema o di connessione abiliti la convalida del certificato del server.

## <a name="gui-additions"></a>Aggiunte all'interfaccia utente grafica
L'interfaccia utente grafica del driver è stata migliorata per consentire l'autenticazione di Azure Active Directory. Per altre informazioni, vedere:
- [Finestra di dialogo di accesso a SQL Server](../help-topics/sql-server-login-dialog.md)
- [Configurazione di UDL (Universal Data Link)](../help-topics/data-link-pages.md)

## <a name="example-connection-strings"></a>Esempi di stringhe di connessione
In questa sezione vengono illustrati esempi di parole chiave della stringa di connessione nuove ed esistenti da usare con la proprietà `IDataInitialize::GetDataSource` e `DBPROP_INIT_PROVIDERSTRING`.

### <a name="sql-authentication"></a>Autenticazione SQL
- Utilizzo di `IDataInitialize::GetDataSource`:
    - Nuovo:
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=SqlPassword**;User ID=[username];Password=[password];Use Encryption for Data=true
    - Deprecato:
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];User ID=[username];Password=[password];Use Encryption for Data=true
- Utilizzo di `DBPROP_INIT_PROVIDERSTRING`:
    - Nuovo:
        > Server=[server];Database=[database];**Authentication=SqlPassword**;UID=[username];PWD=[password];Encrypt=yes
    - Deprecato:
        > Server=[server];Database=[database];UID=[username];PWD=[password];Encrypt=yes

### <a name="integrated-windows-authentication-using-security-support-provider-interface-sspi"></a>Autenticazione integrata di Windows tramite Security Support Provider Interface (SSPI)

- Utilizzo di `IDataInitialize::GetDataSource`:
    - Nuovo:
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryIntegrated**;Use Encryption for Data=true
    - Deprecato:
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Integrated Security=SSPI**;Use Encryption for Data=true
- Utilizzo di `DBPROP_INIT_PROVIDERSTRING`:
    - Nuovo:
        > Server=[server];Database=[database];**Authentication=ActiveDirectoryIntegrated**;Encrypt=yes
    - Deprecato:
        > Server=[server];Database=[database];**Trusted_Connection=yes**;Encrypt=yes

### <a name="azure-active-directory-username-and-password-authentication"></a>Autenticazione di Azure Active Directory con nome utente e password

- Utilizzo di `IDataInitialize::GetDataSource`:
    > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryPassword**;User ID=[username];Password=[password];Use Encryption for Data=true
- Utilizzo di `DBPROP_INIT_PROVIDERSTRING`:
    > Server=[server];Database=[database];**Authentication=ActiveDirectoryPassword**;UID=[username];PWD=[password];Encrypt=yes

### <a name="azure-active-directory-integrated-authentication"></a>Autenticazione integrata di Azure Active Directory

- Utilizzo di `IDataInitialize::GetDataSource`:
    > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryIntegrated**;Use Encryption for Data=true
- Utilizzo di `DBPROP_INIT_PROVIDERSTRING`:
    > Server=[server];Database=[database];**Authentication=ActiveDirectoryIntegrated**;Encrypt=yes

### <a name="azure-active-directory-authentication-using-an-access-token"></a>Autenticazione di Azure Active Directory con token di accesso

- Utilizzo di `IDataInitialize::GetDataSource`:
    > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Access Token=[access token]** ;Use Encryption for Data=true
- Utilizzo di `DBPROP_INIT_PROVIDERSTRING`:
    > L'invio del token di accesso tramite `DBPROP_INIT_PROVIDERSTRING` non è supportato

### <a name="azure-active-directory-interactive-authentication"></a>Autenticazione interattiva di Azure Active Directory

- Utilizzo di `IDataInitialize::GetDataSource`:
    > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryInteractive**;User ID=[username];Use Encryption for Data=true
- Utilizzo di `DBPROP_INIT_PROVIDERSTRING`:
    > Server=[server];Database=[database];**Authentication=ActiveDirectoryInteractive**;UID=[username];Encrypt=yes

### <a name="azure-active-directory-managed-identity-authentication"></a>Autenticazione tramite identità gestite di Azure Active Directory

- Utilizzo di `IDataInitialize::GetDataSource`:
    - Identità gestita assegnata dall'utente:
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryMSI**;User ID=[Object ID];Use Encryption for Data=true
    - Identità gestita assegnata dal sistema:
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryMSI**;Use Encryption for Data=true
- Utilizzo di `DBPROP_INIT_PROVIDERSTRING`:
    - Identità gestita assegnata dall'utente:
        > Server=[server];Database=[database];**Authentication=ActiveDirectoryMSI**;UID=[Object ID];Encrypt=yes
    - Identità gestita assegnata dal sistema:
        > Server=[server];Database=[database];**Authentication=ActiveDirectoryMSI**;Encrypt=yes

### <a name="azure-active-directory-service-principal-authentication"></a>Autenticazione tramite entità servizio di Azure Active Directory

- Utilizzo di `IDataInitialize::GetDataSource`:
    > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryServicePrincipal**;User ID=[Application (client) ID];Password=[Application (client) secret];Use Encryption for Data=true
- Utilizzo di `DBPROP_INIT_PROVIDERSTRING`:
    > Server=[server];Database=[database];**Authentication=ActiveDirectoryServicePrincipal**;UID=[Application (client) ID];PWD=[Application (client) secret];Encrypt=yes

## <a name="code-samples"></a>Esempi di codice

Gli esempi seguenti mostrano il codice necessario per connettersi ad Azure Active Directory con le parole chiave di connessione. 

### <a name="access-token"></a>Token di accesso
```cpp
#include <string>
#include <iostream>
#include <msdasc.h>

int main()
{
    wchar_t azureServer[] = L"server";
    wchar_t azureDatabase[] = L"mydatabase";
    wchar_t accessToken[] = L"eyJ0eXAiOi...";
    IDBInitialize *pIDBInitialize = nullptr;
    IDataInitialize* pIDataInitialize = nullptr;
    HRESULT hr = S_OK;

    CoInitialize(nullptr);

    // Construct the connection string.
    std::wstring connString = L"Provider=MSOLEDBSQL;Data Source=" + std::wstring(azureServer) + L";Initial Catalog=" + 
                              std::wstring(azureDatabase) + L";Access Token=" + accessToken + L";Use Encryption for Data=true;";
    hr = CoCreateInstance(CLSID_MSDAINITIALIZE, nullptr, CLSCTX_INPROC_SERVER, 
                          IID_IDataInitialize, reinterpret_cast<LPVOID*>(&pIDataInitialize));
    if (FAILED(hr))
    {
        std::cout << "Failed to create an IDataInitialize instance." << std::endl;
        goto Cleanup;
    }
    hr = pIDataInitialize->GetDataSource(nullptr, CLSCTX_INPROC_SERVER, connString.c_str(), 
                                         IID_IDBInitialize, reinterpret_cast<IUnknown**>(&pIDBInitialize));
    if (FAILED(hr))
    {
        std::cout << "Failed to get data source object." << std::endl;
        goto Cleanup;
    }
    hr = pIDBInitialize->Initialize();
    if (FAILED(hr))
    {
        std::cout << "Failed to establish connection." << std::endl;
        goto Cleanup;
    }

Cleanup:
    if (pIDBInitialize)
    {
        pIDBInitialize->Uninitialize();
        pIDBInitialize->Release();
    }
    if (pIDataInitialize)
    {
        pIDataInitialize->Release();
    }

    CoUninitialize();
}
```
### <a name="active-directory-integrated"></a>Integrato in Active Directory
```cpp
#include <string>
#include <iostream>
#include <msdasc.h>

int main()
{
    wchar_t azureServer[] = L"server";
    wchar_t azureDatabase[] = L"mydatabase";
    IDBInitialize *pIDBInitialize = nullptr;
    IDataInitialize* pIDataInitialize = nullptr;
    HRESULT hr = S_OK;

    CoInitialize(nullptr);

    // Construct the connection string.
    std::wstring connString = L"Provider=MSOLEDBSQL;Data Source=" + std::wstring(azureServer) + L";Initial Catalog=" + 
                              std::wstring(azureDatabase) + L";Authentication=ActiveDirectoryIntegrated;Use Encryption for Data=true;";

    hr = CoCreateInstance(CLSID_MSDAINITIALIZE, nullptr, CLSCTX_INPROC_SERVER, 
                          IID_IDataInitialize, reinterpret_cast<LPVOID*>(&pIDataInitialize));
    if (FAILED(hr)) 
    {
        std::cout << "Failed to create an IDataInitialize instance." << std::endl;
        goto Cleanup;
    }
    hr = pIDataInitialize->GetDataSource(nullptr, CLSCTX_INPROC_SERVER, connString.c_str(), 
                                         IID_IDBInitialize, reinterpret_cast<IUnknown**>(&pIDBInitialize));
    if (FAILED(hr))
    {
        std::cout << "Failed to get data source object." << std::endl;
        goto Cleanup;
    }
    hr = pIDBInitialize->Initialize();
    if (FAILED(hr))
    {
        std::cout << "Failed to establish connection." << std::endl;
        goto Cleanup;
    }

Cleanup:
    if (pIDBInitialize)
    {
        pIDBInitialize->Uninitialize();
        pIDBInitialize->Release();
    }
    if (pIDataInitialize)
    {
        pIDataInitialize->Release();
    }

    CoUninitialize();
}
```

## <a name="next-steps"></a>Passaggi successivi
- [Autorizzare l'accesso ad applicazioni Web di Azure Active Directory tramite il flusso di concessione del codice di OAuth 2.0](/azure/active-directory/azuread-dev/v1-protocols-oauth-code)

- Informazioni sull'[autenticazione di Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview) in SQL Server.

- Configurare le connessioni driver usando le [parole chiave della stringa di connessione](../applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md) supportate dal driver OLE DB.