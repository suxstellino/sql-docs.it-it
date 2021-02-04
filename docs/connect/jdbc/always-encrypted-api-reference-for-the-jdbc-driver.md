---
description: Informazioni di riferimento sull'API Always Encrypted per il driver JDBC
title: Informazioni di riferimento sull'API Always Encrypted per il driver JDBC | Microsoft Docs
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 6962a2aa-9508-4d4f-a78c-905e2bc68615
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2be51afae231cd31903861e7168d55f1b08c0371
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163650"
---
# <a name="always-encrypted-api-reference-for-the-jdbc-driver"></a>Informazioni di riferimento sull'API Always Encrypted per il driver JDBC
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Always Encrypted consente ai client di eseguire la crittografia dei dati sensibili all'interno delle applicazioni client e non rivelare le chiavi di crittografia di SQL Server. Un driver abilitato Always Encrypted installato nel computer client usa questa funzionalità eseguendo automaticamente la crittografia e la decrittografia dei dati sensibili nell'applicazione client di SQL Server. Il driver esegue la crittografia dei dati in colonne riservate prima di passare i dati a SQL Server e riscrive automaticamente le query in modo da mantenere la semantica per l'applicazione. Analogamente, il driver esegue in modo trasparente la decrittografia dei dati archiviati in colonne crittografate del database presenti nei risultati della query. Per altre informazioni, vedere [Always Encrypted (motore di database)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) e [Uso di Always Encrypted con il driver JDBC](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md).  
  
> [!NOTE]  
>  Always Encrypted è supportato solo da Microsoft JDBC Driver 6.0 o versione successiva per SQL Server con SQL Server 2016.  
  
 ## <a name="always-encrypted-api-references"></a>Riferimento all'API di Always Encrypted
 
 Esistono diverse nuove funzionalità e modifiche all'API del driver JDBC da utilizzare in applicazioni client che utilizzano Always Encrypted.  
  
 **Classe SQLServerConnection**  
  
|Nome|Descrizione|  
|----------|-----------------|  
|Nuova parola chiave della stringa di connessione:<br /><br /> columnEncryptionSetting|columnEncryptionSetting=Enabled abilita la funzionalità Always Encrypted per la connessione e columnEncryptionSetting=Disabled la disabilita. I valori accettati sono Enabled/Disabled. Il valore predefinito è Disabled.|  
|Nuova parola chiave della stringa di connessione: (MS JDBC 7.4 e versioni successive)<br /><br /> keyVaultProviderClientId <br /><br /> keyVaultProviderClientKey |keyVaultProviderClientId=\<ClientID>;keyVaultProviderClientKey=\<ClientKey> <br/><br/> Registra SQLServerColumnEncryptionAzureKeyVaultProvider e usa i valori ClientID e ClientKey per recuperare la chiave master della colonna da Azure Key Vault|  
|Nuovi metodi:<br /><br />`public static void setColumnEncryptionTrustedMasterKeyPaths(Map<String, List\<String>> trustedKeyPaths)`<br /><br />`public static void updateColumnEncryptionTrustedMasterKeyPaths(String server, List\<String> trustedKeyPaths)`<br /><br />`public static void removeColumnEncryptionTrustedMasterKeyPaths(String server)`|Consente di impostare, aggiornare o rimuovere un elenco di percorsi principali attendibili per un server di database. Se durante l'elaborazione di una query dell'applicazione, il driver riceve il percorso di una chiave che non è presente nell'elenco, la query ha esito negativo. Questa proprietà garantisce una maggiore protezione da attacchi alla sicurezza in cui un'istanza compromessa di SQL Server invia falsi percorsi delle chiavi, causando potenzialmente la perdita delle credenziali dell'archivio di chiavi.|  
|Nuovo metodo:<br /><br />`public static Map<String, List\<String>> getColumnEncryptionTrustedMasterKeyPaths()`|Restituisce un elenco di percorsi attendibili delle chiavi per un server di database.|  
|Nuovo metodo:<br /><br />`public static void registerColumnEncryptionKeyStoreProviders (Map\<String, SQLServerColumnEncryptionKeyStoreProvider> clientKeyStoreProviders)`|Consente di registrare provider personalizzati per gli archivi delle chiavi. È un dizionario che esegue il mapping dei nomi dei provider degli archivi delle chiavi per le implementazioni del provider dell'archivio delle chiavi.<br /><br /> Per usare l'archivio chiavi JVM è necessario creare un'istanza di un oggetto SQLServerColumnEncryptionJVMKeyStoreProvider con le credenziali keystore di JVM e registrarla con il driver. Il nome per questo provider deve essere 'MSSQL_JVM_KEYSTORE'.<br /><br /> Per usare l'archivio Azure Key Vault è necessario creare un'istanza di un oggetto SQLServerColumnEncryptionJVMKeyStoreProvider e registrarla con il driver. Il nome di questo provider deve essere 'AZURE_KEY_VAULT'.|
|Nuovo metodo:<br /><br />`public static void unregisterColumnEncryptionKeyStoreProviders (Map\<String, SQLServerColumnEncryptionKeyStoreProvider> clientKeyStoreProviders)`|Consente di annullare la registrazione di tutti i provider dell'archivio chiavi personalizzati deselezionando il dizionario che esegue il mapping dei nomi dei provider dell'archivio chiavi alle implementazioni del provider dell'archivio chiavi.|
|`public final boolean getSendTimeAsDatetime()`|Restituisce l'impostazione della proprietà di connessione sendTimeAsDatetime.|
|`public void setSendTimeAsDatetime(boolean sendTimeAsDateTimeValue)`|Modifica l'impostazione della proprietà di connessione sendTimeAsDatetime.|

 **Classe SQLServerConnectionPoolProxy**
 
|Nome|Descrizione|  
|----------|-----------------|  
|`public final boolean getSendTimeAsDatetime()` | Restituisce l'impostazione della proprietà di connessione sendTimeAsDatetime.|
|`public void setSendTimeAsDatetime(boolean sendTimeAsDateTimeValue)` | Modifica l'impostazione della proprietà di connessione sendTimeAsDatetime.|
     
  
 **Classe SQLServerDataSource**  
  
|Nome|Descrizione|  
|----------|-----------------|  
|`public void setColumnEncryptionSetting(String columnEncryptionSetting)`|Abilita/disabilita la funzionalità Always Encrypted per l'oggetto di origine dati.<br /><br /> Il valore predefinito è Disabled.|  
|`public String getColumnEncryptionSetting()`|Recupera l'impostazione della funzionalità Always Encrypted per l'oggetto di origine dati.|
|`public void setKeyStoreAuthentication(String keyStoreAuthentication)`|Imposta il nome che identifica un archivio chiavi. L'unico valore supportato è **JavaKeyStorePassword** per l'identificazione dell'archivio chiavi Java.<br/><br/>Il valore predefinito è Null.|
|`public String getKeyStoreAuthentication()`|Ottiene il valore dell'impostazione keyStoreAuthentication per l'oggetto origine dati.|
|`public void setKeyStoreSecret(String keyStoreSecret)`|Imposta la password per l'archivio chiavi Java. La password per l'archivio chiavi e la password per la chiave devono coincidere. keyStoreAuthentication deve essere impostato con **JavaKeyStorePassword**.|
|`public void setKeyStoreLocation(String keyStoreLocation)`|Imposta il percorso che include il nome file dell'archivio chiavi Java. keyStoreAuthentication deve essere impostato con **JavaKeyStorePassword**.|
|`public String getKeyStoreLocation()`|Recupera keyStoreLocation per l'archivio chiavi Java.|
  
 **Classe SQLServerColumnEncryptionJavaKeyStoreProvider**  
  
 Implementazione del provider dell'archivio chiavi per l'archivio chiavi Java. Questa classe consente l'utilizzo di certificati archiviati nell'archivio chiavi Java come chiavi master della colonna.  
  
 Costruttori  
  
|Nome|Descrizione|  
|----------|-----------------|  
|`public SQLServerColumnEncryptionJavaKeyStoreProvider (String keyStoreLocation, char[] keyStoreSecret)`|Provider dell'archivio chiavi per l'archivio chiavi Java.|  
  
 Metodi  
  
|Nome|Descrizione|  
|----------|-----------------|  
|`public byte[] decryptColumnEncryptionKey (String masterKeyPath, String encryptionAlgorithm, byte[] encryptedColumnEncryptionKey)`|Esegue la decrittografia del valore crittografato specificato di una chiave di crittografia di colonna. È previsto che il valore sia crittografato usando il certificato con il percorso di chiave specificato e con l'algoritmo specificato.<br /><br /> **Il percorso della chiave deve essere in uno dei formati seguenti:**<br /><br /> Identificazione personale:<certificate_thumbprint><br /><br /> Alias:<certificate_alias><br /><br /> (Sostituisce SQLServerColumnEncryptionKeyStoreProvider. decryptColumnEncryptionKey(String, String, Byte[]).)|  
|`public byte[] encryptColumnEncryptionKey (String masterKeyPath, String encryptionAlgorithm, byte[] plainTextColumnEncryptionKey)`|Crittografa una chiave di crittografia di colonna usando il certificato con il percorso di chiave specificato e usando l'algoritmo specificato.<br /><br /> **Il percorso della chiave deve essere in uno dei formati seguenti:**<br /><br /> Identificazione personale:<certificate_thumbprint><br /><br /> Alias:<certificate_alias><br /><br /> (Sostituisce SQLServerColumnEncryptionKeyStoreProvider. encryptColumnEncryptionKey(String, String, Byte[]).)|
|`public boolean verifyColumnEncryptionKey (String masterKeyPath, boolean allowEnclaveComputations, byte[] signature)`|Verifica la firma della chiave di crittografia della colonna utilizzando il certificato.<br /><br /> **Il percorso della chiave deve essere in uno dei formati seguenti:**<br /><br /> Identificazione personale:<certificate_thumbprint><br /><br /> Alias:<certificate_alias><br /><br /> (Sostituisce SQLServerColumnEncryptionKeyStoreProvider. verifyColumnEncryptionKey (String, Boolean, byte [])).|  
|`public void setName (String name)`|Imposta il nome di questo provider dell'archivio chiavi.|
|`public String getName ()`|Ottiene il nome di questo provider dell'archivio chiavi.|
  
 **Classe SQLServerColumnEncryptionAzureKeyVaultProvider**  
  
 Implementazione del provider dell'archivio chiavi per Azure Key Vault. Questa classe consente l'uso di chiavi archiviate in Azure Key Vault come chiavi master della colonna.  
  
 Costruttori  
  
|Nome|Descrizione|  
|----------|-----------------|  
|`public SQLServerColumnEncryptionAzureKeyVaultProvider ()`|Costruisce un SQLServerColumnEncryptionAzureKeyVaultProvider per l'autenticazione Azure Key Vault.|
|`public SQLServerColumnEncryptionAzureKeyVaultProvider (String clientId)`|Costruisce un SQLServerColumnEncryptionAzureKeyVaultProvider per l'autenticazione Azure Key Vault usando l'identificatore del client che richiede il token.|
|`public SQLServerColumnEncryptionAzureKeyVaultProvider (String clientId, String clientKey)`|Costruisce un SQLServerColumnEncryptionAzureKeyVaultProvider per l'autenticazione Azure Key Vault usando l'identificatore e la chiave del client che richiede il token.|  
|`public SQLServerColumnEncryptionAzureKeyVaultProvider (TokenCredential tokenCredential)`|Costruisce un SQLServerColumnEncryptionAzureKeyVaultProvider per l'autenticazione Azure Key Vault usando TokenCredential fornito.| 
  
 Metodi  
  
|Nome|Descrizione|  
|----------|-----------------|  
|`public byte[] decryptColumnEncryptionKey (String masterKeyPath, String encryptionAlgorithm, byte[] encryptedColumnEncryptionKey)` | Decrittografa una chiave di crittografia della colonna crittografata (CEK). Questa decrittografia viene eseguita con un algoritmo di crittografia RSA che usa la chiave asimmetrica specificata dal percorso della chiave master.<br />(Sostituisce SQLServerColumnEncryptionKeyStoreProvider. decryptColumnEncryptionKey(String, String, Byte[]).) |  
| `public byte[] encryptColumnEncryptionKey (String masterKeyPath, String encryptionAlgorithm, byte[] columnEncryptionKey)` | Crittografa una chiave di crittografia della passando la chiave master di colonna specificata all'algoritmo specificato.<br />(Sostituisce SQLServerColumnEncryptionKeyStoreProvider. encryptColumnEncryptionKey(String, String, Byte[]).) |  
|`public void setName (String name)`|Imposta il nome di questo provider dell'archivio chiavi.|
|`public String getName ()`|Ottiene il nome di questo provider dell'archivio chiavi.|  
  
  
 **Interfaccia SQLServerKeyVaultAuthenticationCallback**  
  
 Questa interfaccia contiene un metodo per l'autenticazione Azure Key Vault, che deve essere implementato dall'utente.  
  
 Metodi  
  
|Nome|Descrizione|  
|----------|-----------------|  
|`public String getAccessToken(String authority, String resource, String scope);`|È necessario eseguire l'override del metodo. Il metodo viene usato per ottenere il token di accesso per Azure Key Vault.|  
  
 **Classe SQLServerColumnEncryptionKeyStoreProvider**  
  
 Estendere questa classe per implementare un provider personalizzato dell’archivio delle chiavi.  
  
|Nome|Descrizione|  
|----------|-----------------|  
|SQLServerColumnEncryptionKeyStoreProvider|Classe di base per tutti i provider dell'archivio chiavi. Un provider personalizzato deve derivare da questa classe ed eseguire l'override delle relative funzioni membro e quindi registrarlo usando SQLServerConnection. registerColumnEncryptionKeyStoreProviders().|  
  
 Metodi  
  
|Nome|Descrizione|  
|----------|-----------------|  
|`public abstract byte[] decryptColumnEncryptionKey (String masterKeyPath, String encryptionAlgorithm, byte [] encryptedColumnEncryptionKey)`|Metodo della classe base per eseguire la decrittografia del valore crittografato specificato di una column encryption key. Il valore crittografato dovrebbe essere crittografato utilizzando la chiave master della colonna con il percorso della chiave specificato e l'algoritmo specificato.|  
|`public abstract byte[] encryptColumnEncryptionKey (String masterKeyPath, String encryptionAlgorithm, byte[]  columnEncryptionKey)`|Metodo della classe base per eseguire la crittografia di una column encryption key utilizzando la column master key con il percorso della chiave specificato e l'algoritmo specificato.|
|`public abstract void setName(String name)`|Imposta il nome di questo provider dell'archivio chiavi.|
|`public abstract String getName()`|Ottiene il nome di questo provider dell'archivio chiavi.|  
  
 Metodi nuovi o in overload nella classe **SQLServerPreparedStatement**.  
  
|Nome|Descrizione|  
|----------|-----------------|  
|`public void setBigDecimal(int parameterIndex, BigDecimal x, int precision, int scale)`<br /><br /> `public void setObject(int parameterIndex, Object x, int targetSqlType, Integer precision, int scale)`<br /><br /> `public void setObject(int parameterIndex, Object x, SQLType targetSqlType, Integer precision, Integer scale)`<br /><br /> `public void setTime(int parameterIndex, java.sql.Time x, int scale)`<br /><br /> `public void setTimestamp(int parameterIndex, java.sql.Timestamp x, int scale)` <br />`public void setDateTimeOffset(int parameterIndex, microsoft.sql.DateTimeOffset x, int scale)`|Questi metodi sono sottoposti a overload con una precisione o un argomento di scala (oppure entrambi) per supportare Always Encrypted per tipi di dati specifici che richiedono informazioni sulla precisione e sulla scala.|  
|`public void setMoney(int parameterIndex, BigDecimal x)`<br /><br /> `public void setSmallMoney(int parameterIndex, BigDecimal x)`<br /><br /> `public void setUniqueIdentifier(int parameterIndex, String guid)`<br /><br /> `public void setDateTime(int parameterIndex, java.sql.Timestamp x)`<br /><br /> `public void setSmallDateTime(int parameterIndex, java.sql.Timestamp x)`|Questi metodi aggiungono il supporto per Always Encrypted per i tipi di dati Money, smallmoney, uniqueidentifier, DateTime e smalldatetime. <br/><br/>Il `setTimestamp()` metodo esistente viene utilizzato per l'invio di valori di parametro alla colonna datetime2 crittografata. Per le colonne DateTime e smalldatetime crittografate, utilizzare rispettivamente i nuovi metodi `setDateTime()` e `setSmallDateTime()` .|  
|`public final void setBigDecimal(int parameterIndex, BigDecimal x, int precision, int scale, boolean forceEncrypt)`<br /><br /> `public final void setMoney(int parameterIndex, BigDecimal x, boolean forceEncrypt)`<br /><br /> `public final void setSmallMoney(int parameterIndex, BigDecimal x, boolean forceEncrypt)`<br /><br /> `public final void setBoolean(int parameterIndex, boolean x, boolean forceEncrypt)`<br /><br /> `public final void setByte(int parameterIndex, byte x, boolean forceEncrypt)`<br /><br /> `public final void setBytes(int parameterIndex, byte x[], boolean forceEncrypt)`<br /><br /> `public final void setUniqueIdentifier(int parameterIndex, String guid, boolean forceEncrypt)`<br /><br /> `public final void setDouble(int parameterIndex, double x, boolean forceEncrypt)`<br /><br /> `public final void setFloat(int parameterIndex, float x, boolean forceEncrypt)`<br /><br /> `public final void setInt(int parameterIndex, int value, boolean forceEncrypt)`<br /><br /> `public final void setLong(int parameterIndex, long x, boolean forceEncrypt)`<br /><br /> `public final setObject(int parameterIndex, Object x, int targetSqlType, Integer precision, int scale, boolean forceEncrypt)`<br /><br /> `public final void setObject(int parameterIndex, Object x, SQLType targetSqlType, Integer precision, Integer scale, boolean forceEncrypt)`<br /><br /> `public final void setShort(int parameterIndex, short x, boolean forceEncrypt)`<br /><br /> `public final void setString(int parameterIndex, String str, boolean forceEncrypt)`<br /><br /> `public final void setNString(int parameterIndex, String value, boolean forceEncrypt)`<br /><br /> `public final void setTime(int parameterIndex, java.sql.Time x, int scale, boolean forceEncrypt)`<br /><br /> `public final void setTimestamp(int parameterIndex, java.sql.Timestamp x, int scale, boolean forceEncrypt)`<br /><br /> `public final void setDateTimeOffset(int parameterIndex, microsoft.sql.DateTimeOffset x, int scale, boolean forceEncrypt)`<br /><br /> `public final void setDateTime(int parameterIndex, java.sql.Timestamp x, boolean forceEncrypt)`<br /><br /> `public final void setSmallDateTime(int parameterIndex, java.sql.Timestamp x, boolean forceEncrypt)`<br /><br /> `public final void setDate(int parameterIndex, java.sql.Date x, java.util.Calendar cal, boolean forceEncrypt)`<br /><br /> `public final void setTime(int parameterIndex, java.sql.Time x, java.util.Calendar cal, boolean forceEncrypt)`<br /><br /> `public final void setTimestamp(int parameterIndex, java.sql.Timestamp x, java.util.Calendar cal, boolean forceEncrypt)`|Imposta il parametro denominato sul valore Java specificato.<br /><br /> Se il valore booleano forceEncrypt è true, il parametro di query viene inviato solo se la colonna di destinazione è crittografata e Always Encrypted è abilitato per la connessione o per l'istruzione.<br /><br /> Se il valore booleano forceEncrypt è false, il driver non forza la crittografia sui parametri.|  
  
 Metodi nuovi o in overload nella classe **SQLServerCallableStatement**  
  
|Nome|Descrizione|  
|----------|-----------------|  
|`public void registerOutParameter(int parameterIndex, int sqlType, int precision, int scale)`<br /><br /> `public void registerOutParameter(int parameterIndex, SQLType sqlType, int precision, int scale)`<br /><br /> `public void registerOutParameter(String parameterName, int sqlType, int precision, int scale)`<br /><br /> `public void registerOutParameter(String parameterName, SQLType sqlType, int precision, int scale)`<br />`public void setBigDecimal(String parameterName, BigDecimal bd, int precision, int scale)`<br /><br /> `public void setTime(String parameterName, java.sql.Time t, int scale)`<br /><br /> `public void setTimestamp(String parameterName, java.sql.Timestamp t, int scale)`<br /><br /> `public void setDateTimeOffset(String parameterName, microsoft.sql.DateTimeOffset t, int scale)`<br/><br/>`public final void setObject(String sCol, Object x, int targetSqlType, Integer precision, int scale)`|Questi metodi sono sottoposti a overload con una precisione o un argomento di scala (oppure entrambi) per supportare Always Encrypted per tipi di dati specifici che richiedono informazioni sulla precisione e sulla scala.|  
|`public void setDateTime(String parameterName, java.sql.Timestamp x)`<br /><br /> `public void setSmallDateTime(String parameterName, java.sql.Timestamp x)`<br /><br /> `public void setUniqueIdentifier(String parameterName, String guid)`<br /><br /> `public void setMoney(String parameterName, BigDecimal bd)`<br /><br /> `public void setSmallMoney(String parameterName, BigDecimal bd)`<br/><br/>`public Timestamp getDateTime(int index)`<br/><br/>`public Timestamp getDateTime(String sCol)`<br/><br/>`public Timestamp getDateTime(int index, Calendar cal)`<br/><br/>`public Timestamp getSmallDateTime(int index)`<br/><br/>`public Timestamp getSmallDateTime(String sCol)`<br/><br/>`public Timestamp getSmallDateTime(int index, Calendar cal)`<br/><br/>`public Timestamp getSmallDateTime(String name, Calendar cal)`<br/><br/>`public BigDecimal getMoney(int index)`<br/><br/>`public BigDecimal getMoney(String sCol)`<br/><br/>`public BigDecimal getSmallMoney(int index)`<br/><br/>`public BigDecimal getSmallMoney(String sCol)`|Questi metodi aggiungono il supporto per Always Encrypted per i tipi di dati Money, smallmoney, uniqueidentifier, DateTime e smalldatetime. <br/><br/>Il `setTimestamp()` metodo esistente viene utilizzato per l'invio di valori di parametro alla colonna datetime2 crittografata. Per le colonne crittografate datetime e smalldatetime usare i nuovi metodi `setDateTime()` e `setSmallDateTime()` rispettivamente.|  
|`public void setObject(String parameterName, Object o, int n, int m, boolean forceEncrypt)`<br /><br /> `public void setObject(String parameterName, Object obj, SQLType jdbcType, int scale, boolean forceEncrypt)`<br /><br /> `public void setDate(String parameterName, java.sql.Date x, Calendar c, boolean forceEncrypt)`<br /><br /> `public void setTime(String parameterName, java.sql.Time t, int scale, boolean forceEncrypt)`<br /><br /> `public void setTime(String parameterName, java.sql.Time x, Calendar c, boolean forceEncrypt)`<br /><br /> `public void setDateTime(String parameterName, java.sql.Timestamp x, boolean forceEncrypt)`<br /><br /> `public void setDateTimeOffset(String parameterName, microsoft.sql.DateTimeOffset t, int scale, boolean forceEncrypt)`<br /><br /> `public void setSmallDateTime(String parameterName, java.sql.Timestamp x, boolean forceEncrypt)`<br /><br /> `public void setTimestamp(String parameterName, java.sql.Timestamp t, int scale, boolean forceEncrypt)`<br /><br /> `public void setTimestamp(String parameterName, java.sql.Timestamp x, boolean forceEncrypt)`<br /><br /> `public void setUniqueIdentifier(String parameterName, String guid, boolean forceEncrypt)`<br /><br /> `public void setBytes(String parameterName, byte[] b, boolean forceEncrypt)`<br /><br /> `public void setByte(String parameterName, byte b, boolean forceEncrypt)`<br /><br /> `public void setString(String parameterName, String s, boolean forceEncrypt)`<br /><br /> `public final void setNString(String parameterName, String value, boolean forceEncrypt)<br /><br /> public void setMoney(String parameterName, BigDecimal bd, boolean forceEncrypt)`<br /><br /> `public void setSmallMoney(String parameterName, BigDecimal bd, boolean forceEncrypt)`<br /><br /> `public void setBigDecimal(String parameterName, BigDecimal bd, int precision, int scale, boolean forceEncrypt)`<br /><br /> `public void setDouble(String parameterName, double d, boolean forceEncrypt)`<br /><br /> `public void setFloat(String parameterName, float f, boolean forceEncrypt)`<br /><br /> `public void setInt(String parameterName, int i, boolean forceEncrypt)`<br /><br /> `public void setLong(String parameterName, long l, boolean forceEncrypt)`<br /><br /> `public void setShort(String parameterName, short s, boolean forceEncrypt)`<br /><br /> `public void setBoolean(String parameterNames, boolean b, boolean forceEncrypt)`<br/><br/>`public void setTimeStamp(String sCol, java.sql.Timestamp x, Calendar c, Boolean forceEncrypt)`|Imposta il parametro denominato sul valore Java specificato.<br /><br /> Se il valore booleano forceEncrypt è true, il parametro di query viene inviato solo se la colonna di destinazione è crittografata e Always Encrypted è abilitato per la connessione o per l'istruzione.<br /><br /> Se il valore booleano forceEncrypt è false, il driver non forza la crittografia sui parametri.|
 

 Metodi nuovi o in overload nella classe **SQLServerResultSet**  
  
|Nome|Descrizione|  
|----------|-----------------|  
|`public String getUniqueIdentifier(int columnIndex)`<br/><br/>`public String getUniqueIdentifier(String columnLabel)`<br/><br/>   `public java.sql.Timestamp getDateTime(int columnIndex)` <br/><br/> `public java.sql.Timestamp getDateTime(String columnName)`   <br/><br/> `public java.sql.Timestamp getDateTime(int columnIndex, Calendar cal)`   <br/><br/>`public java.sql.Timestamp getDateTime(String colName, Calendar cal)`    <br/><br/>`public java.sql.Timestamp getSmallDateTime(int columnIndex)`    <br/><br/> `public java.sql.Timestamp getSmallDateTime(String columnName)`   <br/><br/> `public java.sql.Timestamp getSmallDateTime(int columnIndex, Calendar cal)`   <br/><br/> `public java.sql.Timestamp getSmallDateTime(String colName, Calendar cal)`   <br/><br/>  `public BigDecimal getMoney(int columnIndex)`  <br/><br/> `public BigDecimal getMoney(String columnName)`   <br/><br/> `public BigDecimal getSmallMoney(int columnIndex)`   <br/><br/>  `public BigDecimal getSmallMoney(String columnName)`  <br/><br/>`public void updateMoney(String columnName, BigDecimal x)`    <br/><br/>  `public void updateSmallMoney(String columnName, BigDecimal x)`  <br/><br/>     `public void updateDateTime(int index, java.sql.Timestamp x)` <br/><br/> `public void updateSmallDateTime(int index, java.sql.Timestamp x)` |Questi metodi aggiungono il supporto per Always Encrypted per i tipi di dati Money, smallmoney, uniqueidentifier, DateTime e smalldatetime. <br/><br/>Il `updateTimestamp()` metodo esistente viene utilizzato per aggiornare le colonne datetime2 crittografate. Per le colonne crittografate datetime e smalldatetime usare i nuovi metodi `updateDateTime()` e `updateSmallDateTime()` rispettivamente.|
|`public void updateBoolean(int index, boolean x, boolean forceEncrypt)`  <br/><br/>  `public void updateByte(int index, byte x, boolean forceEncrypt)`  <br/><br/>  `public void updateShort(int index, short x, boolean forceEncrypt)`  <br/><br/> `public void updateInt(int index, int x, boolean forceEncrypt)`   <br/><br/>  `public void updateLong(int index, long x, boolean forceEncrypt)`  <br/><br/> `public void updateFloat(int index, float x, boolean forceEncrypt)`   <br/><br/> `public void updateDouble(int index, double x, boolean forceEncrypt)`   <br/><br/> `public void updateMoney(int index, BigDecimal x, boolean forceEncrypt)`   <br/><br/>  `public void updateMoney(String columnName, BigDecimal x, boolean forceEncrypt)`  <br/><br/> `public void updateSmallMoney(int index, BigDecimal x, boolean forceEncrypt)`   <br/><br/>  `public void updateSmallMoney(String columnName, BigDecimal x, boolean forceEncrypt)`  <br/><br/> `public void updateBigDecimal(int index, BigDecimal x, Integer precision, Integer scale, boolean forceEncrypt)`   <br/><br/>  `public void updateString(int columnIndex, String stringValue, boolean forceEncrypt)`  <br/><br/>  `public void updateNString(int columnIndex, String nString, boolean forceEncrypt)`  <br/><br/>  `public void updateNString(String columnLabel, String nString, boolean forceEncrypt)`  <br/><br/> `public void updateBytes(int index, byte x[], boolean forceEncrypt)   <br/><br/>  public void updateDate(int index, java.sql.Date x, boolean forceEncrypt)`  <br/><br/> `public void updateTime(int index, java.sql.Time x, Integer scale, boolean forceEncrypt)`   <br/><br/> `public void updateTimestamp(int index, java.sql.Timestamp x, int scale, boolean forceEncrypt)`   <br/><br/> `public void updateDateTime(int index, java.sql.Timestamp x, Integer scale, boolean forceEncrypt)`   <br/><br/> `public void updateSmallDateTime(int index, java.sql.Timestamp x, Integer scale, boolean forceEncrypt)`   <br/><br/>  `public void updateDateTimeOffset(int index, microsoft.sql.DateTimeOffset x, Integer scale, boolean forceEncrypt)`  <br/><br/> `public void updateUniqueIdentifier(int index, String x, boolean forceEncrypt)`    <br/><br/>  `public void updateObject(int index, Object x, int precision, int scale, boolean forceEncrypt)`  <br/><br/>  `public void updateObject(int index, Object obj, SQLType targetSqlType, int scale, boolean forceEncrypt)`  <br/><br/> `public void updateBoolean(String columnName, boolean x, boolean forceEncrypt)`    <br/><br/>  `public void updateByte(String columnName, byte x, boolean forceEncrypt)`  <br/><br/>  `public void updateShort(String columnName, short x, boolean forceEncrypt)`  <br/><br/> `public void updateInt(String columnName, int x, boolean forceEncrypt)`   <br/><br/>   `public void updateLong(String columnName, long x, boolean forceEncrypt)` <br/><br/>  `public void updateFloat(String columnName, float x, boolean forceEncrypt)`  <br/><br/>  `public void updateDouble(String columnName, double x, boolean forceEncrypt)  <br/><br/> public void updateBigDecimal(String columnName, BigDecimal x, boolean forceEncrypt)`   <br/><br/>  `public void updateBigDecimal(String columnName, BigDecimal x, Integer precision, Integer scale, boolean forceEncrypt)`  <br/><br/> `public void updateString(String columnName, String x, boolean forceEncrypt)`   <br/><br/>  `public void updateBytes(String columnName, byte x[], boolean forceEncrypt)`  <br/><br/> `public void updateDate(String columnName, java.sql.Date x, boolean forceEncrypt)`   <br/><br/>  `public void updateTime(String columnName, java.sql.Time x, int scale, boolean forceEncrypt)`  <br/><br/>  `public void updateTimestamp(String columnName, java.sql.Timestamp x, int scale, boolean forceEncrypt)`  <br/><br/> `public void updateDateTime(String columnName, java.sql.Timestamp x, int scale, boolean forceEncrypt)`   <br/><br/>  `public void updateSmallDateTime(String columnName, java.sql.Timestamp x, int scale, boolean forceEncrypt)`  <br/><br/>  `public void updateDateTimeOffset(String columnName, microsoft.sql.DateTimeOffset x, int scale, boolean forceEncrypt)`  <br/><br/>  `public void updateUniqueIdentifier(String columnName, String x, boolean forceEncrypt)`<br/><br/>`public void updateObject(String columnName, Object x, int precision, int scale, boolean forceEncrypt)`<br/><br/>`public void updateObject(String columnName, Object obj, SQLType targetSqlType, int scale, boolean forceEncrypt)`|Aggiorna la colonna denominata al valore Java specificato.<br/><br/>Se il valore booleano forceEncrypt è true, la colonna viene impostata solo se è crittografata e Always Encrypted è abilitato per la connessione o per l'istruzione.<br/><br/>Se il valore booleano forceEncrypt è false, il driver non forza la crittografia sui parametri.|

  
Nuovi tipi nella classe **microsoft.sql.Types**

|Nome|Descrizione|  
|----------|-----------------|  
|DATETIME, SMALLDATETIME, MONEY, SMALLMONEY, GUID|Usare questi tipi come tipi SQL di destinazione quando si inviano valori di parametro alle colonne **crittografate** DateTime, smalldatetime, Money, `setObject()/updateObject()` smallmoney e uniqueidentifier usando metodi API.|  
  
  
 **Enumerazione SQLServerStatementColumnEncryptionSetting**  
  
 Specifica come verranno inviati e ricevuti i dati durante la lettura e scrittura delle colonne crittografate. A seconda della query specifica, l'impatto sulle prestazioni può essere ridotto ignorando l'elaborazione del driver Always Encrypted quando vengono usate le colonne non crittografate. Queste impostazioni non possono essere usate per ignorare la crittografia e ottenere l'accesso ai dati in testo non crittografato.  
  
 **Sintassi**  
  
```java
Public enum  SQLServerStatementColumnEncryptionSetting  
```  
  
 **Members**  
  
|Nome|Descrizione|  
|----------|-----------------|  
|UseConnectionSetting|Specifica che il comando deve essere predefinito per l'impostazione Sempre crittografato nella stringa di connessione.|  
|Attivato|Abilita Sempre crittografato per la query.|  
|ResultSetOnly|Specifica che solo i risultati del comando devono essere elaborati in base alla routine Sempre crittografato nel driver. Usare questo valore quando il comando non contiene parametri che richiedono la crittografia.|  
|Disabled|Disabilita Sempre crittografato per la query.|  
  
 L'impostazione del livello istruzione per AE viene aggiunta alla classe SQLServerConnection e alla classe SQLServerConnectionPoolProxy. I metodi seguenti in queste classi sono sottoposti a overload con la nuova impostazione.  
  
|Nome|Descrizione|  
|----------|-----------------|  
|`public Statement createStatement(int nType, int nConcur, int statementHoldability, SQLServerStatementColumnEncryptionSetting stmtColEncSetting)`|Crea un oggetto Statement che genererà oggetti ResultSet con le impostazioni di tipo, concorrenza, trattenibilità e crittografia di colonna specificati.|  
|`public CallableStatement prepareCall(String sql, int nType, int nConcur, int statementHoldability, SQLServerStatementColumnEncryptionSetting stmtColEncSetiing)`|Crea un oggetto CallableStatement con l'impostazione di crittografia della colonna specificata che genererà oggetti ResultSet con il tipo, la concorrenza e la trattenibilità specificati.|  
|`public PreparedStatement prepareStatement(String sql, int autogeneratedKeys, SQLServerStatementColumnEncryptionSetting stmtColEncSetting)`|Crea un oggetto PreparedStatement con l'impostazione di crittografia della colonna specificata che è in grado di recuperare le chiavi generate automaticamente.|  
|`public PreparedStatement prepareStatement(String sql, String[] columnNames, SQLServerStatementColumnEncryptionSetting stmtColEncSetting)`|Crea un oggetto PreparedStatement con l'impostazione di crittografia della colonna specificata, che genererà oggetti ResultSet con i nomi colonna specificati.|  
|`public PreparedStatement prepareStatement(String sql, int[] columnIndexes, SQLServerStatementColumnEncryptionSetting stmtColEncSetting`|Crea un oggetto PreparedStatement con l'impostazione di crittografia della colonna specificata, che genererà oggetti ResultSet con gli indici colonna specificati.|  
|`public PreparedStatement prepareStatement(String sql, int nType, int nConcur, int nHold, SQLServerStatementColumnEncryptionSetting stmtColEncSetting)`|Crea un oggetto PreparedStatement con l'impostazione di crittografia della colonna specificata, che genererà oggetti ResultSet con il tipo, la concorrenza e la trattenibilità specificati.|  
  
> [!NOTE]  
>  Se Always Encrypted è disabilitato per una query e la query include parametri che devono essere crittografati (parametri corrispondenti a colonne crittografate), la query avrà esito negativo.  
>   
>  Se Always Encrypted è disabilitato per una query e la query restituisce risultati da colonne crittografate, i valori restituiti saranno crittografati. I valori crittografati avranno il tipo di dati varbinary.  
  
 ## <a name="see-also"></a>Vedere anche  
 [Uso di Always Encrypted con il driver JDBC](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md)  
  
