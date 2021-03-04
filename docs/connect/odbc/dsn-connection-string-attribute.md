---
title: Parole chiave delle stringhe di connessione e del DNS ODBC
description: Questa pagina elenca le parole chiave per le stringhe di connessione e i DSN e gli attributi di connessione per SQLSetConnectAttr e SQLGetConnectAttr, disponibili nel driver ODBC per SQL Server.
ms.custom: ''
ms.date: 02/04/2019
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: v-chojas
ms.author: v-daenge
author: David-Engel
ms.openlocfilehash: d905ac9c7f2ea02bfac04b54e247d833ae337b96
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837386"
---
# <a name="dsn-and-connection-string-keywords-and-attributes"></a>Parole chiave e attributi per stringhe di connessione e DSN

Questa pagina elenca le parole chiave per le stringhe di connessione e i DSN e gli attributi di connessione per SQLSetConnectAttr e SQLGetConnectAttr, disponibili nel driver ODBC per SQL Server.

## <a name="supported-dsnconnection-string-keywords-and-connection-attributes"></a>Parole chiave per DSN/stringa di connessione e attributi di connessione supportati

La tabella seguente elenca le parole chiave disponibili e gli attributi per ogni piattaforma (L: Linux; M: macOS; W: Windows). Fare clic sulla parola chiave o sull'attributo per altri dettagli.

| Parola chiave DSN / stringa di connessione | Attributo di connessione | Piattaforma |
|-|-|-|
| [Addr](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [Indirizzo](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [AnsiNPW](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_ANSI_NPW](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssansinpw) | LMW |
| [APP](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [ApplicationIntent](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_APPLICATION_INTENT](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssapplicationintent) | LMW |
| [AttachDBFileName](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_ATTACHDBFILENAME](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssattachdbfilename) | LMW |
| [autenticazione](../../connect/odbc/dsn-connection-string-attribute.md#authentication---sql_copt_ss_authentication) | [SQL_COPT_SS_AUTHENTICATION](../../connect/odbc/dsn-connection-string-attribute.md#authentication---sql_copt_ss_authentication) | LMW |
| [AutoTranslate](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_TRANSLATE](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsstranslate) | LMW |
| [ColumnEncryption](../../connect/odbc/dsn-connection-string-attribute.md#columnencryption---sql_copt_ss_column_encryption) | [SQL_COPT_SS_COLUMN_ENCRYPTION](../../connect/odbc/dsn-connection-string-attribute.md#columnencryption---sql_copt_ss_column_encryption) | LMW |
| [ConnectRetryCount](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) | [SQL_COPT_SS_CONNECT_RETRY_COUNT](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) | W |
| [ConnectRetryInterval](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) | [SQL_COPT_SS_CONNECT_RETRY_INTERVAL](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) | W |
| [Database](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_ATTR_CURRENT_CATALOG](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| [Descrizione](../../connect/odbc/dsn-connection-string-attribute.md#description) | | LMW |
| [Driver](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [DSN](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [Encrypt](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_ENCRYPT](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssencrypt) | LMW |
| [Failover_Partner](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_FAILOVER_PARTNER](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssfailoverpartner) | W |
| [FailoverPartnerSPN](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_FAILOVER_PARTNER_SPN](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md) | W |
| [FileDSN](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [KeepAlive](../../connect/odbc/linux-mac/connection-string-keywords-and-data-source-names-dsns.md) (v17.4 e successive, solo DSN)| | LMW |
| [KeepAliveInterval](../../connect/odbc/linux-mac/connection-string-keywords-and-data-source-names-dsns.md) (v17.4 e successive, solo DSN) | | LMW |
| [KeystoreAuthentication](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#connection-string-keywords) | | LMW |
| [KeystorePrincipalId](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#connection-string-keywords) | | LMW |
| [KeystoreSecret](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#connection-string-keywords) | | LMW |
| [Lingua](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [MARS_Connection](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_MARS_ENABLED](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssmarsenabled) | LMW |
| [MultiSubnetFailover](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_MULTISUBNET_FAILOVER](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssmultisubnetfailover) | LMW |
| [Net](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [Network](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [PWD](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [QueryLog_On](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_QUERY](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfquery) | W |
| [QueryLogFile](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_QUERY_LOG](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfquerylog) | W |
| [QueryLogTIme](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_QUERY_INTERVAL](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfqueryinterval) | W |
| [QuotedId](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_QUOTED_IDENT](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssquotedident) | LMW |
| [Regional](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [SaveFile](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [Server](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [ServerSPN](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_SERVER_SPN](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md) | LMW |
| [StatsLog_On](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_DATA](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfdata) | W |
| [StatsLogFile](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_DATA_LOG](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfdatalog) | W |
| [TransparentNetworkIPResolution](../../connect/odbc/dsn-connection-string-attribute.md#transparentnetworkipresolution---sql_copt_ss_tnir) | [SQL_COPT_SS_TNIR](../../connect/odbc/dsn-connection-string-attribute.md#transparentnetworkipresolution---sql_copt_ss_tnir) | LMW |
| [Trusted_Connection](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_INTEGRATED_SECURITY](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssintegratedsecurity) | LMW |
| [TrustServerCertificate](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_TRUST_SERVER_CERTIFICATE](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsstrustservercertificate) | LMW |
| [UID](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [UseFMTONLY](../../connect/odbc/dsn-connection-string-attribute.md#usefmtonly) | | LMW |
| [WSID](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| | [SQL_ATTR_ACCESS_MODE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_ACCESS_MODE) | LMW |
| | [SQL_ATTR_ASYNC_DBC_EVENT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_ASYNC_DBC_PCALLBACK](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_ASYNC_DBC_PCONTEXT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_ASYNC_ENABLE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_AUTO_IPD](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_AUTOCOMMIT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_AUTOCOMMIT) | LMW |
| | [SQL_ATTR_CONNECTION_DEAD](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_CONNECTION_TIMEOUT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_DBC_INFO_TOKEN](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_LOGIN_TIMEOUT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_LOGIN_TIMEOUT) | LMW |
| | [SQL_ATTR_METADATA_ID](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_ODBC_CURSORS](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_ODBC_CURSORS) | LMW |
| | [SQL_ATTR_PACKET_SIZE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_PACKET_SIZE) | LMW |
| | [SQL_ATTR_QUIET_MODE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_QUIET_MODE) | LMW |
| | [SQL_ATTR_RESET_CONNECTION](../../odbc/reference/develop-driver/upgrading-a-3-5-driver-to-a-3-8-driver.md#connection-pooling) <br> (SQL_COPT_SS_RESET_CONNECTION) | LMW |  
| | [SQL_ATTR_TRACE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_OPT_TRACE) | LMW |
| | [SQL_ATTR_TRACEFILE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_OPT_TRACEFILE) | LMW |
| | [SQL_ATTR_TRANSLATE_LIB](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_TRANSLATE_DLL) | LMW |
| | [SQL_ATTR_TRANSLATE_OPTION](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_TRANSLATE_OPTION) | LMW |
| | [SQL_ATTR_TXN_ISOLATION](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_TXN_ISOLATION) | LMW |
| | [SQL_COPT_SS_ACCESS_TOKEN](dsn-connection-string-attribute.md#sql_copt_ss_access_token) | LMW |
| | [SQL_COPT_SS_ANSI_OEM](dsn-connection-string-attribute.md#sql_copt_ss_ansi_oem)| W |
| | [SQL_COPT_SS_AUTOBEGINTXN](dsn-connection-string-attribute.md#sql_copt_ss_autobegintxn)| LMW |
| | [SQL_COPT_SS_BCP](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssbcp) | LMW |
| | [SQL_COPT_SS_BROWSE_CACHE_DATA](../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md) | LMW |
| | [SQL_COPT_SS_BROWSE_CONNECT](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssbrowseconnect) | LMW |
| | [SQL_COPT_SS_BROWSE_SERVER](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssbrowseserver) | LMW |
| | [SQL_COPT_SS_CEKEYSTOREDATA](dsn-connection-string-attribute.md#sql_copt_ss_cekeystoredata) | LMW |
| | [SQL_COPT_SS_CEKEYSTOREPROVIDER](dsn-connection-string-attribute.md#sql_copt_ss_cekeystoreprovider) | LMW |
| | [SQL_COPT_SS_CLIENT_CONNECTION_ID](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md) | LMW |
| | [SQL_COPT_SS_CONCAT_NULL](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssconcatnull) | LMW |
| | [SQL_COPT_SS_CONNECTION_DEAD](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssconnectiondead) | LMW |
| | [SQL_COPT_SS_ENLIST_IN_DTC](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssenlistindtc) | W |
| | [SQL_COPT_SS_ENLIST_IN_XA](dsn-connection-string-attribute.md#sql_copt_ss_enlist_in_xa) | LMW |
| | [SQL_COPT_SS_FALLBACK_CONNECT](dsn-connection-string-attribute.md#sql_copt_ss_fallback_connect) | LMW |
| | [SQL_COPT_SS_INTEGRATED_AUTHENTICATION_METHOD](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md) | LMW |
| | [SQL_COPT_SS_MUTUALLY_AUTHENTICATED](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md) | LMW |
| | [SQL_COPT_SS_OLDPWD](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssoldpwd) | LMW |
| | [SQL_COPT_SS_PERF_DATA_LOG_NOW](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfdatalognow) | W |
| | [SQL_COPT_SS_PRESERVE_CURSORS](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsspreservecursors) | LMW |
| | [SQL_COPT_SS_SPID](../../connect/odbc/dsn-connection-string-attribute.md#sql_copt_ss_spid) (v 17.5 e successive) | LMW |
| | [SQL_COPT_SS_TXN_ISOLATION](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsstxnisolation) | LMW |
| | [SQL_COPT_SS_USER_DATA](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssuserdata) | LMW |
| | [SQL_COPT_SS_WARN_ON_CP_ERROR](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsswarnoncperror) | LMW |
| [ClientCertificate](../../connect/odbc/dsn-connection-string-attribute.md#clientcertificate) | | LMW | 
| [ClientKey](../../connect/odbc/dsn-connection-string-attribute.md#clientkey) | | LMW | 


Di seguito sono elencate alcune parole chiave delle stringhe di connessione e gli attributi di connessione che non sono documentati in [Uso delle parole chiave delle stringhe di connessione con SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md), [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) e [Funzione SQLSetConnectAttr](../../odbc/reference/syntax/sqlsetconnectattr-function.md).

### <a name="description"></a>Descrizione

Consentono di descrivere l'origine dati.

### <a name="sql_copt_ss_ansi_oem"></a>SQL_COPT_SS_ANSI_OEM

Controlla la conversione dei dati da ANSI a OEM. 

| Valore dell'attributo | Descrizione |
|-|-|
| SQL_AO_OFF | (Impostazione predefinita) La conversione non viene eseguita. |
| SQL_AO_ON | La conversione viene eseguita. |

### <a name="sql_copt_ss_autobegintxn"></a>SQL_COPT_SS_AUTOBEGINTXN

Versione 17.6+ Quando il commit automatico è disattivato, controlla l'istruzione BEGIN TRANSACTION automatica dopo ROLLBACK o COMMIT.

| Valore dell'attributo | Descrizione |
|-|-|
| SQL_AUTOBEGINTXN_ON | (Impostazione predefinita) Istruzione BEGIN TRANSACTION automatica dopo ROLLBACK o COMMIT. |
| SQL_AUTOBEGINTXN_OFF | Nessuna istruzione BEGIN TRANSACTION automatica dopo ROLLBACK o COMMIT. |

### <a name="sql_copt_ss_fallback_connect"></a>SQL_COPT_SS_FALLBACK_CONNECT

Controlla l'uso delle connessioni di fallback di SQL Server. Non è più supportata.

| Valore dell'attributo | Descrizione |
|-|-|
| SQL_FB_OFF | (Impostazione predefinita) Le connessioni di fallback sono disabilitate. |
| SQL_FB_ON | Le connessioni di fallback sono abilitate. |



## <a name="new-connection-string-keywords-and-connection-attributes"></a>Nuove parole chiave delle stringhe di connessione e nuovi attributi di connessione

###  <a name="authentication---sql_copt_ss_authentication"></a>Authentication - SQL_COPT_SS_AUTHENTICATION

Imposta la modalità di autenticazione da usare durante la connessione a SQL Server. Per altre informazioni vedere [Uso di Azure Active Directory](using-azure-active-directory.md).

| Valore della parola chiave | Valore dell'attributo | Descrizione |
|-|-|-|
| |SQL_AU_NONE|(Impostazione predefinita) Non impostata. La combinazione di altri attributi determina la modalità di autenticazione.|
|SqlPassword|SQL_AU_PASSWORD|Autenticazione di SQL Server con nome utente e password.|
|ActiveDirectoryIntegrated|SQL_AU_AD_INTEGRATED|Autenticazione integrata di Azure Active Directory.|
|ActiveDirectoryPassword|SQL_AU_AD_PASSWORD|Autenticazione della password di Azure Active Directory.|
|ActiveDirectoryInteractive|SQL_AU_AD_INTERACTIVE|Autenticazione interattiva di Azure Active Directory.|
|ActiveDirectoryMsi|SQL_AU_AD_MSI|Autenticazione tramite identità gestite di Azure Active Directory. Per l'identità assegnata dall'utente, UID è impostato sull'ID oggetto dell'identità utente. |
| |SQL_AU_RESET|Non impostata. Esegue l'override di qualsiasi impostazione del DSN o della stringa di connessione.|

> [!NOTE]
> Quando si usa la parola chiave `Authentication` o l'attributo, impostare in modo esplicito l'impostazione `Encrypt` sul valore desiderato nella stringa di connessione, nel DSN o nell'attributo di connessione. Per informazioni dettagliate, vedere [Uso delle parole chiave delle stringhe di connessione con SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).

### <a name="columnencryption---sql_copt_ss_column_encryption"></a>ColumnEncryption - SQL_COPT_SS_COLUMN_ENCRYPTION

Controlla la crittografia di colonna trasparente (Always Encrypted). Per altre informazioni vedere [Uso di Always Encrypted (ODBC)](using-always-encrypted-with-the-odbc-driver.md).

| Valore della parola chiave | Valore dell'attributo | Descrizione |
|-|-|-|
|Attivato|SQL_CE_ENABLED|Abilita Always Encrypted.|
|Disabled|SQL_CE_DISABLED|(Impostazione predefinita) Disabilita Always Encrypted.|
| |SQL_CE_RESULTSETONLY|Abilita solo la decrittografia (risultati e valori restituiti).|

### <a name="transparentnetworkipresolution---sql_copt_ss_tnir"></a>TransparentNetworkIPResolution - SQL_COPT_SS_TNIR

Controlla la funzionalità Risoluzione dell'IP di rete trasparente che interagisce con MultiSubnetFailover per consentire tentativi di riconnessione più rapidi. Per altre informazioni vedere [Uso della risoluzione dell'IP di rete trasparente](using-transparent-network-ip-resolution.md).

| Valore della parola chiave | Valore dell'attributo| Descrizione |
|-|-|-|
|Attivato|SQL_IS_ON|(Impostazione predefinita) Abilita la risoluzione dell'IP di rete trasparente.|
|Disabled|SQL_IS_OFF|Disabilita la risoluzione dell'IP di rete trasparente.|

### <a name="usefmtonly"></a>UseFMTONLY

Controlla l'uso di SET FMTONLY per i metadati durante la connessione a SQL Server 2012 e versioni successive.

| Valore della parola chiave | Descrizione |
|-|-|
|No|(Impostazione predefinita) Usare sp_describe_first_result_set per i metadati, se disponibili. |
|Sì| Usare SET FMTONLY per i metadati. |


## <a name="clientcertificate"></a>ClientCertificate

Specifica il certificato utente da usare per l'autenticazione. Le opzioni disponibili sono le seguenti: 

| Valore dell'opzione | Descrizione |
|-|-|
| sha1:`<hash_value>` | Il driver ODBC usa l'hash SHA1 per individuare un certificato nell'archivio certificati Windows |
| subject:`<subject>` | Il driver ODBC usa Subject per individuare un certificato nell'archivio certificati Windows |
| file:`<file_location>`[,password:`<password>`] | Il driver ODBC usa un file di certificato. |

Se il certificato è in formato PFX e la chiave privata all'interno del certificato PFX è protetta da password, è necessaria la parola chiave della password. Per i certificati nei formati PEM e DER è obbligatorio l'attributo ClientKey


## <a name="clientkey"></a>ClientKey

Specifica il percorso di un file della chiave privata per i certificati PEM o DER specificati dall'attributo ClientCertificate. Formato: 

| Valore dell'opzione | Descrizione |
|-|-|
| file:`<file_location>`[,password:`<password>`] | Specifica il percorso del file di chiave privata. |

Se il file di chiave privata è protetto da password, è necessario specificare la parola chiave della password. Se la password contiene caratteri ",", viene aggiunto un ulteriore carattere "," immediatamente dopo ognuno di essi. Se, ad esempio, la password è "a,b,c", la password con carattere di escape presente nella stringa di connessione è "a,,b,,c". 
    

### <a name="sql_copt_ss_access_token"></a>SQL_COPT_SS_ACCESS_TOKEN

Consente l'uso di un token di accesso di Azure Active Directory per l'autenticazione. Per altre informazioni vedere [Uso di Azure Active Directory](using-azure-active-directory.md).

| Valore dell'attributo | Descrizione |
|-|-|
| NULL | (Impostazione predefinita) Non viene fornito alcun token di accesso. |
| ACCESSTOKEN* | Puntatore a un token di accesso. |

### <a name="sql_copt_ss_cekeystoredata"></a>SQL_COPT_SS_CEKEYSTOREDATA

Comunica con una libreria del provider dell'archivio chiavi caricata. Controlla la crittografia di colonna trasparente (Always Encrypted). Questo attributo non ha un valore predefinito. Per altre informazioni vedere [Provider di archivi chiavi personalizzati](custom-keystore-providers.md).

| Valore dell'attributo | Descrizione |
|-|-|
| CEKEYSTOREDATA * | Struttura dei dati di comunicazione per la libreria del provider dell'archivio chiavi |

### <a name="sql_copt_ss_cekeystoreprovider"></a>SQL_COPT_SS_CEKEYSTOREPROVIDER

Carica una libreria del provider dell'archivio chiavi per Always Encrypted o recupera i nomi delle librerie del provider dell'archivio chiavi caricate. Per altre informazioni vedere [Provider di archivi chiavi personalizzati](custom-keystore-providers.md). Questo attributo non ha un valore predefinito.

| Valore dell'attributo | Descrizione |
|-|-|
| char * | Percorso di una libreria del provider dell'archivio chiavi |

### <a name="sql_copt_ss_enlist_in_xa"></a>SQL_COPT_SS_ENLIST_IN_XA

Per abilitare le transazioni XA con elaborazione delle transazioni (TP) conforme a XA, l'applicazione deve chiamare **SQLSetConnectAttr** con SQL_COPT_SS_ENLIST_IN_XA e un puntatore a un oggetto `XACALLPARAM`. Questa opzione è supportata in Windows, Linux (17.3 e versioni successive) e macOS.
```
SQLSetConnectAttr(hdbc, SQL_COPT_SS_ENLIST_IN_XA, param, SQL_IS_POINTER);  // XACALLPARAM *param
``` 
 Per associare una transazione XA con una sola connessione ODBC, specificare TRUE o FALSE con SQL_COPT_SS_ENLIST_IN_XA anziché con il puntatore nella chiamata a **SQLSetConnectAttr**. Questa procedura è valida solo in Windows e non può essere usata per specificare operazioni XA attraverso un'applicazione client. 
 ```
SQLSetConnectAttr(hdbc, SQL_COPT_SS_ENLIST_IN_XA, (SQLPOINTER)TRUE, 0);
``` 

|valore|Descrizione|Piattaforme|  
|-----------|-----------------|-----------------|  
|Oggetto XACALLPARAM*|Puntatore all'oggetto `XACALLPARAM`.|Windows, Linux e macOS|
|TRUE|Associa la transazione XA alla connessione ODBC. Tutte le attività del database correlate verranno eseguite sotto la protezione della transazione XA.|Windows|  
|FALSE|Annulla l'associazione della transazione XA alla connessione ODBC.|Windows|

 Per altre informazioni sulle transazioni XA, vedere [Uso delle transazioni XA](../../connect/odbc/use-xa-with-dtc.md).

### <a name="sql_copt_ss_spid"></a>SQL_COPT_SS_SPID

Recupera l'ID del processo server della connessione. Equivale alla variabile T-SQL [@@SPID](../../t-sql/functions/spid-transact-sql.md), tranne per il fatto che non comporta un round trip aggiuntivo al server.

| Valore dell'attributo | Descrizione |
|-|-|
| DWORD | SPID |
