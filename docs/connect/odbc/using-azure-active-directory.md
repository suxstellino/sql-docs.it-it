---
title: Uso di Azure Active Directory con il driver ODBC
description: Microsoft ODBC Driver for SQL Server consente alle applicazioni ODBC di connettersi a un'istanza del database SQL di Azure usando Azure Active Directory.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 52205f03-ff29-4254-bfa8-07cced155c86
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2a6b27c8c14a92ed9f17c56cab1cf2285b2cfec9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198859"
---
# <a name="using-azure-active-directory-with-the-odbc-driver"></a>Uso di Azure Active Directory con il driver ODBC
[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

## <a name="purpose"></a>Scopo

Microsoft ODBC Driver for SQL Server versione 13.1 o successiva consente alle applicazioni ODBC di connettersi a un'istanza del database SQL di Azure usando un'identità federata in Azure Active Directory con nome utente e password, un token di accesso di Azure Active Directory, un'identità gestita di Azure Active Directory (17.3+) o l'autenticazione integrata di Windows (17.6+ in Linux/macOS). Per la versione 13.1 del driver ODBC, l'autenticazione del token di accesso di Azure Active Directory è _solo Windows_. Il driver ODBC versione 17 e successive supporta questa autenticazione in tutte le piattaforme (Windows, Linux e macOS). Una nuova autenticazione interattiva di Azure Active Directory con ID di accesso è stato introdotta nella versione 17.1 per Windows del driver ODBC. Un nuovo metodo di autenticazione dell'identità gestita di Azure Active Directory è stato aggiunto nella versione del driver ODBC 17.3.1.1 per le identità assegnate dall'utente e dal sistema. Tutte queste operazioni sono eseguite tramite l'utilizzo di nuove parole chiave per la stringa di connessione e DSN e di attributi di connessione.

> [!NOTE]
> Il driver ODBC in Linux e macOS prima della versione 17.6 supporta solo l'autenticazione di Azure Active Directory direttamente da Azure Active Directory. Se si usa l'autenticazione con nome utente/password di Azure Active Directory da un client Linux o macOS e la configurazione di Active Directory richiede che il client esegua l'autenticazione in un endpoint Active Directory Federation Services, l'autenticazione potrebbe non riuscire. A partire dalla versione 17.6 del driver, questa restrizione è stata eliminata.

## <a name="new-andor-modified-dsn-and-connection-string-keywords"></a>Parole chiave della stringa di connessione e DSN nuovi e/o modificati

È possibile usare la parola chiave `Authentication` quando ci si connette con un DSN o una stringa di connessione per controllare la modalità di autenticazione. Il valore impostato nella stringa di connessione sostituisce il valore nel DSN, se specificato. Il _valore pre-attributo_ dell'impostazione `Authentication` è il valore calcolato in base ai valori della stringa di connessione e del DSN.

|Nome|Valori|Predefinito|Descrizione|
|-|-|-|-|
|`Authentication`|(non impostato), (stringa vuota),,, `SqlPassword` `ActiveDirectoryPassword` ,, `ActiveDirectoryIntegrated` `ActiveDirectoryInteractive` `ActiveDirectoryMsi` , `ActiveDirectoryServicePrincipal` |(non impostato)|Controlla la modalità di autenticazione.<table><tr><th>valore<th>Descrizione<tr><td>(non impostato)<td>Modalità di autenticazione determinata da altre parole chiave (opzioni di connessione legacy esistenti).<tr><td>(stringa vuota)<td>(Solo stringa di connessione). Eseguire l'override e annullare l'impostazione di un valore `Authentication` specificato nel DSN.<tr><td>`SqlPassword`<td>Eseguire l'autenticazione direttamente in un'istanza di SQL Server usando un nome utente e una password.<tr><td>`ActiveDirectoryPassword`<td>Eseguire l'autenticazione con un'identità di Azure Active Directory usando un nome utente e una password.<tr><td>`ActiveDirectoryIntegrated`<td>_Solo driver Windows e Linux/Mac 17.6+_ . Eseguire l'autenticazione con un'identità di Azure Active Directory usando l'autenticazione integrata.<tr><td>`ActiveDirectoryInteractive`<td>_Solo driver Windows_. Eseguire l'autenticazione con un'identità di Azure Active Directory usando l'autenticazione interattiva.<tr><td>`ActiveDirectoryMsi`<td>Eseguire l'autenticazione con un'identità di Azure Active Directory tramite l'autenticazione dell'identità gestita. Per l'identità assegnata dall'utente, UID è impostato sull'ID oggetto dell'identità utente.<tr><td>`ActiveDirectoryServicePrincipal`<td>(17,7 +) Eseguire l'autenticazione con Azure Active Directory identità usando l'autenticazione basata su entità servizio.</table>|
|`Encrypt`|(non impostato), `Yes`, `No`|(vedere la descrizione)|Controlla la crittografia per una connessione. Se il valore del pre-attributo dell'impostazione `Authentication` è diverso da _nessuno_ nel DSN o nella stringa di connessione, il valore predefinito è `Yes`. Diversamente, il valore predefinito è `No`. Se l'attributo `SQL_COPT_SS_AUTHENTICATION` sostituisce il valore del pre-attributo di `Authentication`, impostare in modo esplicito il valore di crittografia nel DSN, nella stringa di connessione o nell'attributo di connessione. Il valore del pre-attributo della crittografia è `Yes` se il valore è impostato su `Yes` nella stringa di connessione o nel DSN.|

## <a name="new-andor-modified-connection-attributes"></a>Attributi di connessione nuovi e/o modificati

Per supportare l'autenticazione di Azure Active Directory sono stati introdotti o modificati gli attributi di preconnessione seguenti. Quando un attributo di connessione ha una parola chiave della stringa di connessione o del DSN corrispondente e viene impostato, l'attributo di connessione ha la precedenza.

|Attributo|Type|Valori|Predefinito|Descrizione|
|-|-|-|-|-|
|`SQL_COPT_SS_AUTHENTICATION`|`SQL_IS_INTEGER`|`SQL_AU_NONE`, `SQL_AU_PASSWORD`, `SQL_AU_AD_INTEGRATED`, `SQL_AU_AD_PASSWORD`, `SQL_AU_AD_INTERACTIVE`, `SQL_AU_AD_MSI`, `SQL_AU_AD_SPA`, `SQL_AU_RESET`|(non impostato)|Vedere la descrizione della parola chiave `Authentication` fornita in precedenza. `SQL_AU_NONE` viene specificato per sostituire esplicitamente un valore `Authentication` impostato nella stringa di connessione e/o nel DSN, mentre `SQL_AU_RESET` rimuove l'impostazione se è stato impostato, consentendo al valore del DSN o della stringa di connessione di avere la precedenza.|
|`SQL_COPT_SS_ACCESS_TOKEN`|`SQL_IS_POINTER`|Puntatore a `ACCESSTOKEN` o NULL|NULL|Se diverso da Null, specifica il token di accesso di Azure AD da usare. È un errore specificare un token di accesso e le parole chiave della stringa di connessione `UID`, `PWD`, `Trusted_Connection` o `Authentication` o gli attributi equivalenti. <br> **NOTA** La versione 13.1 del driver ODBC supporta questa operazione solo in _Windows_.|
|`SQL_COPT_SS_ENCRYPT`|`SQL_IS_INTEGER`|`SQL_EN_OFF`, `SQL_EN_ON`|(vedere la descrizione)|Controlla la crittografia per una connessione. `SQL_EN_OFF` e `SQL_EN_ON` rispettivamente disabilitano e abilitano la crittografia. Se il valore del pre-attributo dell'impostazione `Authentication` è diverso da _nessuno_ o è impostato `SQL_COPT_SS_ACCESS_TOKEN` e `Encrypt` non è stato specificato nel DSN o nella stringa di connessione, il valore predefinito è `SQL_EN_ON`. Diversamente, il valore predefinito è `SQL_EN_OFF`. Se l'attributo di connessione `SQL_COPT_SS_AUTHENTICATION` è impostato su un valore diverso da _nessuno_, impostare in modo esplicito `SQL_COPT_SS_ENCRYPT` sul valore desiderato se `Encrypt` non è stato specificato nella stringa di connessione o nel DSN. Il valore effettivo di questo attributo determina [se verrà usata la crittografia per la connessione.](../../relational-databases/native-client/features/using-encryption-without-validation.md)|
|`SQL_COPT_SS_OLDPWD`|\-|\-|\-|Non supportato con Azure Active Directory, poiché le modifiche delle password nelle entità di sicurezza di Azure AD non possono essere eseguite tramite una connessione ODBC. <br><br>In SQL Server 2005 è stata introdotta la scadenza della password per l'autenticazione di SQL Server. L'attributo `SQL_COPT_SS_OLDPWD` è stato aggiunto per consentire al client di specificare sia la vecchia password che la nuova per la connessione. Quando questa proprietà è impostata, il provider non userà il pool di connessioni per la prima connessione o per le connessioni successive, in quanto la stringa di connessione conterrà la "vecchia password" che è stata modificata.|
|`SQL_COPT_SS_INTEGRATED_SECURITY`|`SQL_IS_INTEGER`|`SQL_IS_OFF`,`SQL_IS_ON`|`SQL_IS_OFF`|_Deprecato_; in alternativa, usare `SQL_COPT_SS_AUTHENTICATION` impostato su `SQL_AU_AD_INTEGRATED`. <br><br>Forza l'utilizzo dell'autenticazione di Windows (Kerberos in Linux e macOS) per la convalida dell'accesso in fase di connessione al server. Quando viene usata l'autenticazione di Windows, il driver ignora i valori di ID utente e password specificati come parte dell'elaborazione di `SQLConnect`, `SQLDriverConnect` o `SQLBrowseConnect`.|

## <a name="ui-additions-for-azure-active-directory-windows-driver-only"></a>Aggiunte all'interfaccia utente per Azure Active Directory (solo driver Windows)

La configurazione DSN e le interfacce utente di connessione del driver sono state migliorate con le opzioni aggiuntive necessarie per l'utilizzo dell'autenticazione con Azure AD.

### <a name="creating-and-editing-dsns-in-the-ui"></a>Creazione e modifica dei DSN nell'interfaccia utente

È possibile usare le nuove opzioni di autenticazione di Azure AD durante la creazione o la modifica di un DSN esistente usando l'interfaccia utente di installazione del driver:

`Authentication=ActiveDirectoryIntegrated` per l'autenticazione integrata di Azure Active Directory nel database SQL di Azure

![Schermata di creazione e modifica del DSN con autenticazione integrata di Azure Active Directory selezionata.](windows/create-dsn-ad-integrated.png)

`Authentication=ActiveDirectoryPassword` per l'autenticazione con nome utente/password di Azure Active Directory nel database SQL di Azure

![Schermata di creazione e modifica del DSN con autenticazione della password di Azure Active Directory selezionata.](windows/create-dsn-ad-password.png)

`Authentication=ActiveDirectoryInteractive` per l'autenticazione interattiva di Azure Active Directory nel database SQL di Azure

![Schermata di creazione e modifica del DSN con autenticazione interattiva di Azure Active Directory selezionata.](windows/create-dsn-ad-interactive.png)

`Authentication=SqlPassword` per l'autenticazione con nome utente/password in SQL Server (Azure o altro)

![Schermata di creazione e modifica del DSN con autenticazione di SQL Server selezionata.](windows/create-dsn-ad-sql-server.png)

`Trusted_Connection=Yes` per l'autenticazione integrata SSPI legacy di Windows

![Schermata di creazione e modifica del DSN con autenticazione integrata di Windows selezionata.](windows/create-dsn-win-sspi.png)

`Authentication=ActiveDirectoryMsi` per l'autenticazione tramite identità gestite di Azure Active Directory

![Schermata di creazione e modifica del DSN con autenticazione basata su identità del servizio gestita selezionata.](windows/create-dsn-ad-msi.png)

`Authentication=ActiveDirectoryServicePrincipal` per Azure Active Directory autenticazione basata su entità servizio

![Schermata di creazione e modifica DSN con Azure Active Directory autenticazione dell'entità servizio selezionata.](windows/create-dsn-ad-spa.png)

Le sette opzioni corrispondono a `Trusted_Connection=Yes` (autenticazione integrata esistente solo SSPI di Windows) e `Authentication=` `ActiveDirectoryIntegrated` rispettivamente,,, `SqlPassword` `ActiveDirectoryPassword` `ActiveDirectoryInteractive` , `ActiveDirectoryMsi` e `ActiveDirectoryServicePrincipal` .

### <a name="sqldriverconnect-prompt-windows-driver-only"></a>Prompt SQLDriverConnect (solo driver Windows)

La finestra di dialogo di richiesta visualizzata da SQLDriverConnect quando richiede le informazioni necessarie per completare la connessione contiene quattro nuove opzioni per l'autenticazione di Azure AD:

![Finestra di dialogo di accesso a SQL Server visualizzata da SQLDriverConnect.](windows/server-login.png)

Queste opzioni corrispondono alle sei opzioni disponibili nell'interfaccia utente di configurazione del DSN descritta in precedenza.

### <a name="example-connection-strings"></a>Esempi di stringhe di connessione
1. Autenticazione di SQL Server - sintassi legacy. Il certificato del server non viene convalidato e la crittografia viene usata solo se viene imposta dal server. Nome utente e password vengono passati nella stringa di connessione.
`server=Server;database=Database;UID=UserName;PWD=Password;`
2. Autenticazione SQL - nuova sintassi. Il client richiede la crittografia (il valore predefinito di `Encrypt` è `true`) e il certificato del server viene convalidato, indipendentemente dall'impostazione della crittografia (a meno che `TrustServerCertificate` non sia impostato su `true`). Nome utente e password vengono passati nella stringa di connessione.
 `server=Server;database=Database;UID=UserName;PWD=Password;Authentication=SqlPassword;`
3. Autenticazione integrata di Windows (Kerberos in Linux e macOS) tramite SSPI (in SQL Server o SQL IaaS) - sintassi corrente. Il certificato del server non viene convalidato, a meno che non venga usata la crittografia. 
`server=Server;database=Database;Trusted_Connection=yes;`
4. (_Solo driver Windows_). Autenticazione integrata di Windows tramite SSPI (se il database di destinazione è in SQL Server o SQL IaaS) - nuova sintassi. Il client richiede la crittografia (il valore predefinito di `Encrypt` è `true`) e il certificato del server viene convalidato, indipendentemente dall'impostazione della crittografia (a meno che `TrustServerCertificate` non sia impostato su `true`). 
`server=Server;database=Database;Authentication=ActiveDirectoryIntegrated;`
5. Autenticazione con nome utente/password di Azure Active Directory (se il database di destinazione si trova nel database SQL di Azure). Il certificato del server viene convalidato, indipendentemente dall'impostazione della crittografia (a meno che `TrustServerCertificate` non sia impostato su `true`). Nome utente e password vengono passati nella stringa di connessione. 
`server=Server;database=Database;UID=UserName;PWD=Password;Authentication=ActiveDirectoryPassword;`
6. (_Solo driver Windows e Linux/macOS 17.6+_ .) Autenticazione integrata di Windows tramite ADAL o Kerberos, che prevede il riscatto delle credenziali dell'account di Windows per un token di accesso emesso da Azure AD, presupponendo che il database di destinazione si trovi nel database SQL di Azure. Il certificato del server viene convalidato, indipendentemente dall'impostazione della crittografia (a meno che `TrustServerCertificate` non sia impostato su `true`). In Linux/macOS è necessario che sia disponibile un ticket Kerberos appropriato; per altre informazioni, vedere la sezione seguente sugli account federati e sull'[uso dell'autenticazione integrata](linux-mac/using-integrated-authentication.md).
`server=Server;database=Database;Authentication=ActiveDirectoryIntegrated;`
7. (_Solo driver Windows_). Azure AD autenticazione interattiva usa la tecnologia di Multi-Factor Authentication Azure Active Directory per configurare la connessione. In questa modalità, specificando l'ID di accesso, viene attivata una finestra di dialogo di autenticazione di Azure che consente all'utente di immettere la password per completare la connessione. Il nome utente viene passato nella stringa di connessione.
`server=Server;database=Database;UID=UserName;Authentication=ActiveDirectoryInteractive;`

![Interfaccia utente di autenticazione di Windows Azure durante l'utilizzo dell'autenticazione interattiva di Active Directory.](windows/WindowsAzureAuth.png)

8. L'autenticazione identità gestita di Azure Active Directory usa l'identità assegnata dal sistema o assegnata dall'utente per l'autenticazione allo scopo di configurare la connessione. Per l'identità assegnata dall'utente, UID è impostato sull'ID oggetto dell'identità utente.<br>
Per l'identità assegnata dal sistema,<br>
`server=Server;database=Database;Authentication=ActiveDirectoryMsi;`<br>
Per l'identità assegnata dall'utente con ID oggetto uguale a myObjectId,<br>
`server=Server;database=Database;UID=myObjectId;Authentication=ActiveDirectoryMsi;`

9. Autenticazione dell'entità servizio Azure Active Directory `server=Server;databse=Database;UID=clientId;PWD=clientSecret;Authentication=ActiveDirectoryServicePrincipal;`

> [!NOTE]
>- Quando si usano le opzioni di Active Directory con il driver ODBC *_di Windows * precedente alla_* versione 17.4.2, verificare che il [Active Directory Authentication Library per SQL Server](https://go.microsoft.com/fwlink/?LinkID=513072) sia stato installato. Quando si usano i driver Linux e macOS, assicurarsi che `libcurl` sia stato installato. Per la versione del driver 17.2 e versioni successive, questa non è una dipendenza esplicita poiché non è necessaria per gli altri metodi di autenticazione o per le operazioni ODBC.
>- Quando la configurazione di Azure Active Directory include criteri di accesso condizionale e il client è Windows 10 o Server 2016 o versione successiva, l'autenticazione integrata o tramite nome utente/password potrebbe non riuscire. I criteri di accesso condizionale richiedono l'uso di Windows Account Manager (WAM), supportato nella versione del driver 17.6 o successiva per Windows. Per usare WAM, creare una nuova stringa o un valore DWORD denominato `ADALuseWAM` in `HKLM\Software\ODBC\ODBCINST.INI\ODBC Driver 17 for SQL Server`, `HKCU\Software\ODBC\ODBC.INI\<your-user-DSN-name>` o `HKLM\Software\ODBC\ODBC.INI\<your-system-DSN-name>` per la configurazione globale, DSN utente o con ambito DSN di sistema rispettivamente e impostarlo su un valore pari a 1. Si noti che l'autenticazione con WAM non supporta l'esecuzione dell'applicazione come utente diverso con `runas`. Gli scenari che richiedono criteri di accesso condizionale non sono supportati per Linux o macOS.
>- Per connettersi usando il nome utente e la password di un account SQL Server, è ora possibile usare la nuova opzione `SqlPassword`, che è consigliata in particolare per Azure SQL poiché abilita impostazioni predefinite della connessione più sicure.
>- Per connettersi usando il nome utente e la password di un account Azure Active Directory, specificare `Authentication=ActiveDirectoryPassword` nella stringa di connessione e le parole chiave `UID` e `PWD` rispettivamente con nome utente e password.
>- Per connettersi usando l'autenticazione integrata di Windows o di Active Directory (solo driver Windows e Linux/macOS 17.6+), specificare `Authentication=ActiveDirectoryIntegrated` nella stringa di connessione. Il driver sceglierà automaticamente la modalità di autenticazione corretta. `UID` e `PWD` non devono essere specificati.
>- Per connettersi usando l'autenticazione interattiva di Active Directory (solo driver Windows), è necessario specificare `UID`.

## <a name="authenticating-with-an-access-token"></a>Autenticazione con un token di accesso

L'attributo di preconnessione `SQL_COPT_SS_ACCESS_TOKEN` consente l'utilizzo di un token di accesso ottenuto da Azure AD per l'autenticazione anziché di nome utente e password e ignora anche la negoziazione e il recupero di un token di accesso da parte del driver. Per usare un token di accesso, impostare l'attributo di connessione `SQL_COPT_SS_ACCESS_TOKEN` su un puntatore a una struttura `ACCESSTOKEN`:

~~~
typedef struct AccessToken
{
    DWORD dataSize;
    BYTE data[];
} ACCESSTOKEN;
~~~

`ACCESSTOKEN` è una struttura a lunghezza variabile costituita da una _lunghezza_ di 4 byte seguita da byte _lunghezza_ di dati opachi che formano il token di accesso. A causa del modo in cui SQL Server gestisce i token di accesso, un token ottenuto tramite una risposta JSON [OAuth 2.0](/azure/active-directory/develop/active-directory-authentication-scenarios) deve essere espanso in modo che ogni byte sia seguito da un byte di riempimento 0, simile a una stringa UCS-2 contenente solo caratteri ASCII. Il token tuttavia è un valore opaco e la lunghezza specificata, in byte, NON deve includere alcun carattere di terminazione Null. A causa dei vincoli di lunghezza e di formato considerevoli, questo metodo di autenticazione è disponibile solo a livello di codice tramite l'attributo di connessione `SQL_COPT_SS_ACCESS_TOKEN`. Non è disponibile alcun DSN o alcuna parola chiave della stringa di connessione corrispondente. La stringa di connessione non deve contenere le parole chiave `UID`, `PWD`, `Authentication` o `Trusted_Connection`.

> [!NOTE]
> La versione 13.1 del driver ODBC supporta questa autenticazione solo in _Windows_.

## <a name="azure-active-directory-authentication-sample-code"></a>Codice di esempio dell'autenticazione di Azure Active Directory

L'esempio seguente mostra il codice necessario per connettersi a SQL Server usando Azure Active Directory con le parole chiave di connessione. Si noti che non è necessario modificare il codice dell'applicazione. La stringa di connessione o il DSN, se usato, è l'unica modifica necessaria per usare Azure AD per l'autenticazione:
~~~
    ...
    SQLCHAR connString[] = "Driver={ODBC Driver 17 for SQL Server};Server={server};UID=myuser;PWD=myPass;Authentication=ActiveDirectoryPassword"
    ...
    SQLDriverConnect(hDbc, NULL, connString, SQL_NTS, NULL, 0, NULL, SQL_DRIVER_NOPROMPT);    
    ...
~~~
L'esempio seguente mostra il codice necessario per connettersi a SQL Server usando Azure Active Directory con l'autenticazione con token di accesso. In questo caso, è necessario modificare il codice dell'applicazione per elaborare il token di accesso e impostare l'attributo di connessione associato.
~~~
    SQLCHAR connString[] = "Driver={ODBC Driver 17 for SQL Server};Server={server}"
    SQLCHAR accessToken[] = "eyJ0eXAiOi..."; // In the format extracted from an OAuth JSON response
    ...
    DWORD dataSize = 2 _ strlen(accessToken);
    ACCESSTOKEN *pAccToken = malloc(sizeof(ACCESSTOKEN) + dataSize);
    pAccToken->dataSize = dataSize;
    // Expand access token with padding bytes
    for(int i = 0, j = 0; i < dataSize; i += 2, j++) {
        pAccToken->data[i] = accessToken[j];
        pAccToken->data[i+1] = 0;
    }
    ...
    SQLSetConnectAttr(hDbc, SQL_COPT_SS_ACCESS_TOKEN, (SQLPOINTER)pAccToken, SQL_IS_POINTER);
    SQLDriverConnect(hDbc, NULL, connString, SQL_NTS, NULL, 0, NULL, SQL_DRIVER_NOPROMPT);        
    ...
    free(pAccToken);
~~~
Di seguito è riportata una stringa di connessione di esempio da usare con l'autenticazione interattiva di Azure Active Directory. Si noti che non contiene il campo PWD poiché la password viene inserita usando la schermata di autenticazione di Azure.
~~~
SQLCHAR connString[] = "Driver={ODBC Driver 17 for SQL Server};Server={server};UID=myuser;Authentication=ActiveDirectoryInteractive"
~~~
Di seguito è riportata una stringa di connessione di esempio da usare con l'autenticazione con identità gestita di Azure Active Directory. Si noti che l'UID è impostato sull'ID oggetto dell'identità utente per l'identità assegnata dall'utente.
~~~
// For system-assigned identity,
SQLCHAR connString[] = "Driver={ODBC Driver 17 for SQL Server};Server={server};Authentication=ActiveDirectoryMsi"
...
// For user-assigned identity with object ID equals to myObjectId
SQLCHAR connString[] = "Driver={ODBC Driver 17 for SQL Server};Server={server};UID=myObjectId;Authentication=ActiveDirectoryMsi"
~~~

## <a name="considerations-for-using-adfs-federated-accounts-on-linuxmacos"></a>Considerazioni sull'utilizzo di account federati ADFS in Linux/macOS

A partire dalla versione 17.6, i driver per Linux e macOS supportano l'autenticazione con account federati ADFS di Azure Active Directory tramite l'uso di nome utente/password (`ActiveDirectoryPassword`) o Kerberos (`ActiveDirectoryIntegrated`). Sono presenti alcune limitazioni che dipendono dalla piattaforma quando si usa la modalità integrata.

Quando si esegue l'autenticazione con un utente il cui suffisso UPN è diverso dall'area di autenticazione Kerberos, ovvero è in uso un suffisso UPN alternativo, è necessario usare l'opzione Enterprise Principal (usare l'opzione `-E` con `kinit` e specificare il nome dell'entità di sicurezza nel formato `user@federated-domain`) quando si ottengono i ticket Kerberos. Ciò consente al driver di determinare correttamente sia il dominio federato che l'area di autenticazione Kerberos.

È possibile verificare che sia disponibile un ticket Kerberos appropriato controllando l'output del comando `klist`. Se il dominio federato corrisponde all'area di autenticazione Kerberos e al suffisso UPN, il nome dell'entità di sicurezza avrà il formato `user@realm`. Se è diverso, il nome dell'entità deve essere nel formato `user@federated-domain@realm`.

### <a name="linux"></a>Linux

In SuSE 11 la versione predefinita della libreria Kerberos 1.6.x non supporta l'opzione Enterprise Principal necessaria per usare suffissi UPN alternativi. Per usare suffissi UPN alternativi con autenticazione integrata Azure AD, aggiornare la libreria Kerberos a 1.7 o versione successiva.

In Alpine Linux il `libcurl` predefinito non supporta l'autenticazione SPNEGO/Kerberos necessaria per l'autenticazione integrata Azure AD.

### <a name="macos"></a>macOS

La libreria Kerberos di sistema `kinit` supporta Enterprise Principal con l'opzione `--enterprise`, ma esegue anche in modo implicito la canonizzazione dei nomi, che impedisce l'utilizzo di suffissi UPN alternativi. Per usare suffissi UPN alternativi con l'autenticazione integrata Azure AD, installare una libreria Kerberos più recente tramite `brew install krb5` e usare il relativo `kinit` con l'opzione `-E` come descritto in precedenza.

## <a name="see-also"></a>Vedere anche

[Supporto dell'autenticazione basata su token per il database SQL di Azure tramite l'autenticazione di Azure AD](/archive/blogs/sqlsecurity/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth)

[Uso dell'autenticazione integrata](linux-mac/using-integrated-authentication.md)