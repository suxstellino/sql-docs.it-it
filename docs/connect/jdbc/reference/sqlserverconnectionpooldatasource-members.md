---
description: Membri di SQLServerConnectionPoolDataSource
title: Membri di SQLServerConnectionPoolDataSource | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: dac0337e-8088-488c-a25a-801a2190f6ca
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 614bcf5295b776cdd61fc9bb7b070fe1e26cbb69
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178344"
---
# <a name="sqlserverconnectionpooldatasource-members"></a>Membri di SQLServerConnectionPoolDataSource
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Nelle tabelle seguenti sono elencati i membri esposti dalla classe [SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md).  
  
## <a name="constructors"></a>Costruttori  
  
|Nome|Descrizione|  
|----------|-----------------|  
|[SQLServerConnectionPoolDataSource ()](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-constructor.md)|Inizializza una nuova istanza della classe [SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md).|  
  
## <a name="fields"></a>Campi  
 No.  
  
## <a name="inherited-fields"></a>Campi ereditati  
 No.  
  
## <a name="methods"></a>Metodi  
  
|Nome|Descrizione|  
|----------|-----------------|  
|[getApplicationIntent](../../../connect/jdbc/reference/getapplicationintent-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Restituisce il valore della proprietà di connessione **applicationIntent**.|  
|[getApplicationName](../../../connect/jdbc/reference/getapplicationname-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Restituisce il nome dell'applicazione.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Tenta di stabilire una connessione con l'origine dati rappresentata dall'oggetto DataSource.|  
|[getDatabaseName](../../../connect/jdbc/reference/getdatabasename-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Restituisce il nome del database.|  
|[getDescription](../../../connect/jdbc/reference/getdescription-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Restituisce una descrizione dell'origine dati.|  
|[getFailoverPartner](../../../connect/jdbc/reference/getfailoverpartner-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Restituisce il nome del server di failover usato in una configurazione per il mirroring del database.|  
|[getInstanceName](../../../connect/jdbc/reference/getinstancename-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Restituisce il nome dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[getLastUpdateCount](../../../connect/jdbc/reference/getlastupdatecount-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Restituisce un valore **booleano** che indica se la proprietà lastUpdateCount è abilitata.|  
|[getLockTimeout](../../../connect/jdbc/reference/getlocktimeout-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Restituisce un valore **int** che indica il numero di millisecondi di attesa del database prima che venga segnalato un timeout blocchi.|  
|[getLoginTimeout](../../../connect/jdbc/reference/getlogintimeout-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Restituisce il numero di secondi di attesa dell'oggetto DataSource durante il tentativo di stabilire una connessione.|  
|[getLogWriter](../../../connect/jdbc/reference/getlogwriter-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Restituisce un flusso di output dei caratteri da usare per tutti i messaggi di registrazione e di traccia.|  
|[getMultiSubnetFailover](../../../connect/jdbc/reference/getmultisubnetfailover-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Restituisce il valore della proprietà di connessione **multiSubnetFailover**.|  
|[getPooledConnection](../../../connect/jdbc/reference/getpooledconnection-method-sqlserverconnectionpooldatasource.md)|Tenta di stabilire una connessione di database fisica che possa essere utilizzata come connessione in pool.|  
|[getPortNumber](../../../connect/jdbc/reference/getportnumber-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Restituisce il numero di porta corrente usato per comunicare con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[getReference](../../../connect/jdbc/reference/getreference-method-sqlserverconnectionpooldatasource.md)|Restituisce un riferimento a questo oggetto DataSource.|  
|[getSelectMethod](../../../connect/jdbc/reference/getselectmethod-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Restituisce il tipo di cursore predefinito usato per tutti i set di risultati creati tramite questo oggetto DataSource.|  
|[getSendStringParametersAsUnicode](../../../connect/jdbc/reference/getsendstringparametersasunicode-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Restituisce un valore **booleano** che indica se l'invio di parametri di stringa al server in formato UNICODE è abilitato.|  
|[getServerName](../../../connect/jdbc/reference/getservername-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Restituisce il nome del computer in cui è in esecuzione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Restituisce l'URL usato per la connessione all'origine dati.|  
|[getUser](../../../connect/jdbc/reference/getuser-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Restituisce il nome utente usato per la connessione all'origine dati.|  
|[getWorkstationID](../../../connect/jdbc/reference/getworkstationid-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Restituisce il nome del computer client usato per la connessione all'origine dati.|  
|[getXopenStates](../../../connect/jdbc/reference/getxopenstates-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Restituisce un valore **booleano** che indica se la conversione di stati SQL in stati conformi a XOPEN è abilitata.|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverconnectionpooldatasource.md)|Indica se questo oggetto è un wrapper per l'interfaccia specificata.|  
|[setApplicationIntent](../../../connect/jdbc/reference/setapplicationintent-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Imposta il valore della proprietà di connessione **applicationIntent**.|  
|[setApplicationName](../../../connect/jdbc/reference/setapplicationname-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Imposta il nome dell'applicazione.|  
|[setAuthenticationSceme](../../../connect/jdbc/reference/setauthenticationscheme-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Indica il tipo di sicurezza integrata che si vuole venga usato dall'applicazione.|  
|[setDatabaseName](../../../connect/jdbc/reference/setdatabasename-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Imposta il nome del database a cui connettersi.|  
|[setDescription](../../../connect/jdbc/reference/setdescription-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Imposta la descrizione dell'origine dati.|  
|[setFailoverPartner](../../../connect/jdbc/reference/setfailoverpartner-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Imposta il nome del server di failover usato in una configurazione per il mirroring del database.|  
|[setInstanceName](../../../connect/jdbc/reference/setinstancename-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Imposta il nome dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setIntegratedSecurity](../../../connect/jdbc/reference/setintegratedsecurity-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Imposta un valore **booleano** che indica se la proprietà integratedSecurity è abilitata.|  
|[setLastUpdateCount](../../../connect/jdbc/reference/setlastupdatecount-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Imposta un valore **booleano** che indica se la proprietà lastUpdateCount è abilitata.|  
|[setLockTimeout](../../../connect/jdbc/reference/setlocktimeout-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Imposta un valore **int** che indica il numero di millisecondi di attesa del database prima che venga segnalato un timeout blocchi.|  
|[setLoginTimeout](../../../connect/jdbc/reference/setlogintimeout-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Imposta il numero di secondi di attesa dell'oggetto DataSource durante il tentativo di stabilire una connessione.|  
|[setLogWriter](../../../connect/jdbc/reference/setlogwriter-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Imposta un flusso di output dei caratteri da usare per tutti i messaggi di registrazione e di traccia.|  
|[setMultiSubnetFailover](../../../connect/jdbc/reference/setmultisubnetfailover-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Imposta il valore della proprietà di connessione **multiSubnetFailover**.|  
|[setPassword](../../../connect/jdbc/reference/setpassword-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Imposta la password che verrà usata per la connessione a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setPortNumber](../../../connect/jdbc/reference/setportnumber-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Imposta il numero di porta da usare per comunicare con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setSelectMethod](../../../connect/jdbc/reference/setselectmethod-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Imposta il tipo di cursore predefinito usato per tutti i set di risultati creati tramite questo oggetto DataSource.|  
|[setSendStringParametersAsUnicode](../../../connect/jdbc/reference/setsendstringparametersasunicode-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Imposta un valore **booleano** che indica se l'invio di parametri di stringa al server in formato UNICODE è abilitato.|  
|[setServerName](../../../connect/jdbc/reference/setservername-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Imposta il nome del computer in cui è in esecuzione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setURL](../../../connect/jdbc/reference/seturl-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Imposta l'URL usato per la connessione all'origine dati.|  
|[setUser](../../../connect/jdbc/reference/setuser-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Imposta il nome utente usato per la connessione all'origine dati.|  
|[setWorkstationID](../../../connect/jdbc/reference/setworkstationid-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Imposta il nome del computer client usato per la connessione all'origine dati.|  
|[setXopenStates](../../../connect/jdbc/reference/setxopenstates-method-sqlserverdatasource.md)|Ereditato da [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md). Imposta un valore **booleano** che indica se la conversione di stati SQL in stati conformi a XOPEN è abilitata.|  
|[unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverconnectionpooldatasource.md)|Restituisce un oggetto che implementa l'interfaccia specificata per consentire l'accesso ai metodi specifici di [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].|  
  
## <a name="inherited-methods"></a>Metodi ereditati  
  
|Classe ereditata da:|Metodi|  
|---------------------------|-------------|  
|com.microsoft.sqlserver.jdbc.SQLServerDataSource|getApplicationName, getConnection, getDatabaseName, getDescription, getFailoverPartner, getInstanceName, getLastUpdateCount, getLockTimeout, getLoginTimeout, getLogWriter, getPortNumber, getSelectMethod, getSendStringParametersAsUnicode, getServerName, getURL, getUser, getWorkstationID, getXopenStates, setApplicationName, setDatabaseName, setDescription, setFailoverPartner, setInstanceName, setIntegratedSecurity, setLastUpdateCount, setLockTimeout, setLoginTimeout, setLogWriter, setPassword, setPortNumber, setSelectMethod, setSendStringParametersAsUnicode, setServerName, setURL, setUser, setWorkstationID, setXopenStates|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
|javax.sql.ConnectionPoolDataSource|getLoginTimeout, getLogWriter, setLoginTimeout, setLogWriter, getPooledConnection|  
  
## <a name="see-also"></a>Vedere anche  
 [Classe SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md)  
  
  
