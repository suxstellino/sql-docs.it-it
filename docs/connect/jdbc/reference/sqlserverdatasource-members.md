---
description: Membri di SQLServerDataSource
title: Membri di SQLServerDataSource | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 7e749bc5-d765-4864-be2b-7822d4c20c09
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2e16d07888c7a86ecd1b3296ee234794ae7aac00
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178314"
---
# <a name="sqlserverdatasource-members"></a>Membri di SQLServerDataSource
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Nelle tabelle seguenti sono elencati i membri esposti dalla classe [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).  
  
## <a name="constructors"></a>Costruttori  
  
|Nome|Descrizione|  
|----------|-----------------|  
|[SQLServerDataSource ()](../../../connect/jdbc/reference/sqlserverdatasource-constructor.md)|Inizializza una nuova istanza della classe [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).|  
  
## <a name="fields"></a>Campi  
 No.  
  
## <a name="inherited-fields"></a>Campi ereditati  
 No.  
  
## <a name="methods"></a>Metodi  
  
|Nome|Descrizione|  
|----------|-----------------|  
|[getApplicationIntent](../../../connect/jdbc/reference/getapplicationintent-method-sqlserverdatasource.md)|Restituisce il valore della proprietà di connessione **applicationIntent**.|  
|[getApplicationName](../../../connect/jdbc/reference/getapplicationname-method-sqlserverdatasource.md)|Restituisce il nome dell'applicazione.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverdatasource.md)|Tenta di stabilire una connessione con l'origine dati rappresentata da questo oggetto [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).|  
|[getDatabaseName](../../../connect/jdbc/reference/getdatabasename-method-sqlserverdatasource.md)|Restituisce il nome del database.|  
|[getDisableStatementPooling](../../../connect/jdbc/reference/getdisablestatementpooling-method-sqlserverdatasource.md)|Restituisce il valore della proprietà di connessione **disableStatementPooling**. Questa impostazione controlla se il pool di istruzioni è abilitato o meno per questa connessione.|  
|[getEnablePrepareOnFirstPreparedStatementCall](../../../connect/jdbc/reference/getenableprepareonfirstpreparedstatementcall-method-sqlserverdatasource.md)|Restituisce il valore della proprietà di connessione **enablePrepareOnFirstPreparedStatementCall**.|  
|[getEncrypt](../../../connect/jdbc/reference/getencrypt-method-sqlserverdatasource.md)|Restituisce un valore **booleano** che indica se la proprietà di crittografia è abilitata.|  
|[getDescription](../../../connect/jdbc/reference/getdescription-method-sqlserverdatasource.md)|Restituisce una descrizione dell'origine dati.|  
|[getFailoverPartner](../../../connect/jdbc/reference/getfailoverpartner-method-sqlserverdatasource.md)|Restituisce il nome del server di failover utilizzato nella configurazione del mirroring del database.|  
|[getHostNameInCertificate](../../../connect/jdbc/reference/gethostnameincertificate-method-sqlserverdatasource.md)|Restituisce il nome host usato per convalidare il certificato Transport Layer Security (TLS) di SQL Server, noto in precedenza come Secure Sockets Layer (SSL).|  
|[getInstanceName](../../../connect/jdbc/reference/getinstancename-method-sqlserverdatasource.md)|Restituisce il nome dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[getLastUpdateCount](../../../connect/jdbc/reference/getlastupdatecount-method-sqlserverdatasource.md)|Restituisce un valore **booleano** che indica se la proprietà lastUpdateCount è abilitata.|  
|[getLockTimeout](../../../connect/jdbc/reference/getlocktimeout-method-sqlserverdatasource.md)|Restituisce un valore **int** che indica il numero di millisecondi di attesa del database prima che venga segnalato un timeout blocchi.|  
|[getLoginTimeout](../../../connect/jdbc/reference/getlogintimeout-method-sqlserverdatasource.md)|Restituisce il numero di secondi di attesa di questo oggetto [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) durante il tentativo di stabilire una connessione.|  
|[getLogWriter](../../../connect/jdbc/reference/getlogwriter-method-sqlserverdatasource.md)|Restituisce un flusso di output dei caratteri da utilizzare per tutti i messaggi di registrazione e di traccia.|  
|[getMultiSubnetFailover](../../../connect/jdbc/reference/getmultisubnetfailover-method-sqlserverdatasource.md)|Restituisce il valore della proprietà di connessione **multiSubnetFailover**.|  
|[getPacketSize](../../../connect/jdbc/reference/getpacketsize-method-sqlserverdatasource.md)|Restituisce le dimensioni del pacchetto di rete corrente usato per comunicare con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], specificate in byte.|  
|[getPortNumber](../../../connect/jdbc/reference/getportnumber-method-sqlserverdatasource.md)|Restituisce il numero di porta corrente usato per comunicare con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[getReference](../../../connect/jdbc/reference/getreference-method-sqlserverdatasource.md)|Restituisce un riferimento a questo oggetto [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).|  
|[getResponseBuffering](../../../connect/jdbc/reference/getresponsebuffering-method-sqlserverdatasource.md)|Restituisce la modalità di memorizzazione delle risposte nel buffer per questo oggetto [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).|  
|[getSelectMethod](../../../connect/jdbc/reference/getselectmethod-method-sqlserverdatasource.md)|Restituisce il tipo di cursore predefinito usato per tutti i set di risultati creati tramite questo oggetto [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).|  
|[getSendStringParametersAsUnicode](../../../connect/jdbc/reference/getsendstringparametersasunicode-method-sqlserverdatasource.md)|Restituisce un valore **booleano** che indica se l'invio di parametri di stringa al server in formato UNICODE è abilitato.|  
|[getSendTimeAsDatetime](../../../connect/jdbc/reference/getsendtimeasdatetime-method-sqlserverdatasource.md)|Restituisce l'impostazione della proprietà di connessione **SendTimeAsDatetime**.|  
|[getServerName](../../../connect/jdbc/reference/getservername-method-sqlserverdatasource.md)|Restituisce il nome del computer su cui è in esecuzione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[getServerPreparedStatementDiscardThreshold](../../../connect/jdbc/reference/getserverpreparedstatementdiscardthreshold-method-sqlserverdatasource.md)|Restituisce il valore della proprietà di connessione **serverPreparedStatementDiscardThreshold**.|  
|[getStatementPoolingCacheSize](../../../connect/jdbc/reference/getstatementpoolingcachesize-method-sqlserverdatasource.md)|Restituisce le dimensioni della cache delle istruzioni preparate per questa connessione.|  
|[getTrustManagerClass](../../../connect/jdbc/reference/gettrustmanagerclass-method-sqlserverdatasource.md)|Restituisce il valore stringa della proprietà di connessione TrustManagerClass.|  
|[getTrustManagerConstructorArg](../../../connect/jdbc/reference/gettrustmanagerconstructorarg-method-sqlserverdatasource.md)|Restituisce il valore stringa della proprietà di connessione TrustManagerConstructorArg.|  
|[getTrustServerCertificate](../../../connect/jdbc/reference/gettrustservercertificate-method-sqlserverdatasource.md)|Restituisce un valore **booleano** che indica se la proprietà trustServerCertificate è abilitata.|  
|[getTrustStore](../../../connect/jdbc/reference/gettruststore-method-sqlserverdatasource.md)|Restituisce il percorso (incluso il nome file) del file trustStore del certificato.|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlserverdatasource.md)|Restituisce l'URL utilizzato per la connessione all'origine dati.|  
|[getUser](../../../connect/jdbc/reference/getuser-method-sqlserverdatasource.md)|Restituisce il nome utente utilizzato per la connessione all'origine dati.|  
|[getUseSQLServerBaseDate](../../../connect/jdbc/reference/getsendtimeasdatetime-method-sqlserverdatasource.md)|Restituisce l'impostazione della proprietà di connessione useSQLServerBaseDate.|  
|[getWorkstationID](../../../connect/jdbc/reference/getworkstationid-method-sqlserverdatasource.md)|Restituisce il nome del computer client utilizzato per la connessione all'origine dati.|  
|[getXopenStates](../../../connect/jdbc/reference/getxopenstates-method-sqlserverdatasource.md)|Restituisce un valore **booleano** che indica se la conversione di stati SQL in stati conformi a XOPEN è abilitata.|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverdatasource.md)|Indica se questo oggetto origine dati è un wrapper per l'interfaccia specificata.|  
|[setApplicationIntent](../../../connect/jdbc/reference/setapplicationintent-method-sqlserverdatasource.md)|Imposta il valore della proprietà di connessione **applicationIntent**.|  
|[setApplicationName](../../../connect/jdbc/reference/setapplicationname-method-sqlserverdatasource.md)|Imposta il nome dell'applicazione.|  
|[setAuthenticationScheme](../../../connect/jdbc/reference/setauthenticationscheme-sqlserverdatasource.md)|Indica il tipo di sicurezza integrata che si desidera venga utilizzata dall'applicazione.|  
|[setDatabaseName](../../../connect/jdbc/reference/setdatabasename-method-sqlserverdatasource.md)|Imposta il nome del database a cui connettersi.|  
|[setDescription](../../../connect/jdbc/reference/setdescription-method-sqlserverdatasource.md)|Imposta la descrizione dell'origine dati.|  
|[setDisableStatementPooling](../../../connect/jdbc/reference/setdisablestatementpooling-method-sqlserverdatasource.md)|Imposta il pooling dell'istruzione su true o false.|  
|[setEnablePrepareOnFirstPreparedStatementCall](../../../connect/jdbc/reference/setenableprepareonfirstpreparedstatementcall-method-sqlserverdatasource.md)|Specifica il nuovo valore della proprietà di connessione **enablePrepareOnFirstPreparedStatementCall**.|  
|[setEncrypt](../../../connect/jdbc/reference/setencrypt-method-sqlserverdatasource.md)|Imposta un valore **booleano** che indica se la proprietà di crittografia è abilitata.|  
|[setFailoverPartner](../../../connect/jdbc/reference/setfailoverpartner-method-sqlserverdatasource.md)|Imposta il nome del server di failover utilizzato nella configurazione del mirroring del database.|  
|[setHostNameInCertificate](../../../connect/jdbc/reference/sethostnameincertificate-method-sqlserverdatasource.md)|Imposta il nome host da usare per convalidare il certificato Transport Layer Security (TLS) di SQL Server, noto in precedenza come Secure Sockets Layer (SSL).|  
|[setInstanceName](../../../connect/jdbc/reference/setinstancename-method-sqlserverdatasource.md)|Imposta il nome dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setIntegratedSecurity](../../../connect/jdbc/reference/setintegratedsecurity-method-sqlserverdatasource.md)|Imposta un valore **booleano** che indica se la proprietà integratedSecurity è abilitata.|  
|[setLastUpdateCount](../../../connect/jdbc/reference/setlastupdatecount-method-sqlserverdatasource.md)|Imposta un valore **booleano** che indica se la proprietà lastUpdateCount è abilitata.|  
|[setLockTimeout](../../../connect/jdbc/reference/setlocktimeout-method-sqlserverdatasource.md)|Imposta un valore **int** che indica il numero di millisecondi di attesa del database prima che venga segnalato un timeout blocchi.|  
|[setLoginTimeout](../../../connect/jdbc/reference/setlogintimeout-method-sqlserverdatasource.md)|Imposta il numero di secondi di attesa dell'oggetto [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) durante il tentativo di stabilire una connessione.|  
|[setLogWriter](../../../connect/jdbc/reference/setlogwriter-method-sqlserverdatasource.md)|Imposta un flusso di output dei caratteri da utilizzare per tutti i messaggi di registrazione e di traccia.|  
|[setMultiSubnetFailover](../../../connect/jdbc/reference/setmultisubnetfailover-method-sqlserverdatasource.md)|Imposta il valore della proprietà di connessione **multiSubnetFailover**.|  
|[setPacketSize](../../../connect/jdbc/reference/setpacketsize-method-sqlserverdatasource.md)|Imposta le dimensioni, specificate in byte, del pacchetto di rete corrente usato per comunicare con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setPassword](../../../connect/jdbc/reference/setpassword-method-sqlserverdatasource.md)|Imposta la password usata per la connessione a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setPortNumber](../../../connect/jdbc/reference/setportnumber-method-sqlserverdatasource.md)|Imposta il numero di porta usato per comunicare con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverdatasource.md)|Imposta la modalità di memorizzazione delle risposte nel buffer per le connessioni create tramite questo oggetto [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).|  
|[setSelectMethod](../../../connect/jdbc/reference/setselectmethod-method-sqlserverdatasource.md)|Imposta il tipo di cursore predefinito usato per tutti i set di risultati creati tramite questo oggetto [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md).|  
|[setSendStringParametersAsUnicode](../../../connect/jdbc/reference/setsendstringparametersasunicode-method-sqlserverdatasource.md)|Imposta un valore **booleano** che indica se l'invio di parametri di stringa al server in formato UNICODE è abilitato.|  
|[setSendTimeAsDatetime](../../../connect/jdbc/reference/setsendtimeasdatetime-method-sqlserverdatasource.md)|Specifica la modalità di invio dei valori java.sql.Time al server.|  
|[setServerName](../../../connect/jdbc/reference/setservername-method-sqlserverdatasource.md)|Imposta il nome del computer su cui è in esecuzione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setServerPreparedStatementDiscardThreshold](../../../connect/jdbc/reference/setserverpreparedstatementdiscardthreshold-method-sqlserverdatasource.md)|Imposta il nuovo valore della proprietà di connessione **serverPreparedStatementDiscardThreshold**.|  
|[setStatementPoolingCacheSize](../../../connect/jdbc/reference/setstatementpoolingcachesize-method-sqlserverdatasource.md)|Imposta la dimensione della cache delle istruzioni preparate per questa connessione.|  
|[setTrustManagerClass](../../../connect/jdbc/reference/settrustmanagerclass-method-sqlserverdatasource.md)|Imposta il valore stringa della proprietà di connessione TrustManagerClass.|  
|[setTrustManagerConstructorArg](../../../connect/jdbc/reference/settrustmanagerconstructorarg-method-sqlserverdatasource.md)|Imposta il valore stringa della proprietà di connessione TrustManagerConstructorArg.|  
|[setTrustServerCertificate](../../../connect/jdbc/reference/settrustservercertificate-method-sqlserverdatasource.md)|Imposta un valore **booleano** che indica se la proprietà trustServerCertificate è abilitata.|  
|[setTrustStore](../../../connect/jdbc/reference/settruststore-method-sqlserverdatasource.md)|Imposta il percorso (incluso il nome file) del file trustStore del certificato.|  
|[setTrustStorePassword](../../../connect/jdbc/reference/settruststorepassword-method-sqlserverdatasource.md)|Imposta la password utilizzata per verificare l'integrità dei dati del file trustStore.|  
|[setURL](../../../connect/jdbc/reference/seturl-method-sqlserverdatasource.md)|Imposta l'URL utilizzato per la connessione all'origine dati.|  
|[setUser](../../../connect/jdbc/reference/setuser-method-sqlserverdatasource.md)|Imposta il nome utente utilizzato per la connessione all'origine dati.|  
|[setWorkstationID](../../../connect/jdbc/reference/setworkstationid-method-sqlserverdatasource.md)|Imposta il nome del computer client utilizzato per la connessione all'origine dati.|  
|[setXopenStates](../../../connect/jdbc/reference/setxopenstates-method-sqlserverdatasource.md)|Imposta un valore **booleano** che indica se la conversione di stati SQL in stati conformi a XOPEN è abilitata.|  
|[unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverdatasource.md)|Restituisce un oggetto che implementa l'interfaccia specificata per consentire l'accesso ai metodi specifici di [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].|  
  
## <a name="inherited-methods"></a>Metodi ereditati  
  
|Classe ereditata da:|Metodi|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Vedere anche  
 [Classe SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
