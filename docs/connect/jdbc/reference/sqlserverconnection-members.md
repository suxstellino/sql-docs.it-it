---
description: Membri di SQLServerConnection
title: Membri di SQLServerConnection | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 3115a533-756b-4c78-aee9-4ba7253c85e0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ed9783e0b91dd79fe7b52c80b05fa7693213759c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172842"
---
# <a name="sqlserverconnection-members"></a>Membri di SQLServerConnection
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Nelle tabelle seguenti sono elencati i membri esposti dalla classe [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).  
  
## <a name="constructors"></a>Costruttori  
 Nessuno.  
  
## <a name="fields"></a>Campi  
  
|Nome|Descrizione|  
|----------|-----------------|  
|[TRANSACTION_SNAPSHOT](../../../connect/jdbc/reference/transaction-snapshot-field-sqlserverconnection.md)|Consente di specificare il livello di isolamento delle transazioni snapshot.|  
  
## <a name="inherited-fields"></a>Campi ereditati  
  
|Classe ereditata da:|Descrizione|  
|---------------------------|-----------------|  
|java.sql.Connection|TRANSACTION_NONE, TRANSACTION_READ_COMMITTED, TRANSACTION_READ_UNCOMMITTED, TRANSACTION_REPEATABLE_READ, TRANSACTION_SERIALIZABLE|  
  
## <a name="methods"></a>Metodi  
  
|Nome|Descrizione|  
|----------|-----------------|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverconnection.md)|Cancella tutti gli avvisi segnalati per questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[close](../../../connect/jdbc/reference/close-method-sqlserverconnection.md)|Rilascia immediatamente il database per questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) e le risorse JDBC invece di attendere il rilascio automatico.|  
|[closeUnreferencedPreparedStatementHandles](../../../connect/jdbc/reference/closeunreferencedpreparedstatementhandles-method-sqlserverconnection.md)|Impone richieste di annullamento per eventuali istruzioni preparate per l'esecuzione e rimosse in sospeso.| 
|[commit](../../../connect/jdbc/reference/commit-method-sqlserverconnection.md)|Rende permanenti tutte le modifiche apportate dal commit o dal rollback precedente e rilascia eventuali blocchi a livello di database attualmente mantenuti da questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[createBlob](../../../connect/jdbc/reference/createblob-method-sqlserverconnection.md)|Crea un oggetto **java.sql.Blob** senza dati.|  
|[createClob](../../../connect/jdbc/reference/createclob-method-sqlserverconnection.md)|Crea un oggetto **java.sql.Clob** senza dati.|  
|[createNClob](../../../connect/jdbc/reference/createnclob-method-sqlserverconnection.md)|Crea un oggetto **java.sql.NClob** senza dati.|  
|[createStatement](../../../connect/jdbc/reference/createstatement-method-sqlserverconnection.md)|Crea un oggetto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) per l'invio di istruzioni SQL al database.|  
|[createSQLXML](../../../connect/jdbc/reference/createsqlxml-method-sqlserverconnection.md)|Crea un oggetto **java.sql.SQLXML** senza dati.|  
|[getAutoCommit](../../../connect/jdbc/reference/getautocommit-method-sqlserverconnection.md)|Recupera la modalità di commit automatico corrente per questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[getCatalog](../../../connect/jdbc/reference/getcatalog-method-sqlserverconnection.md)|Recupera il nome di catalogo corrente per questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[Metodo getClientConnectionID &#40;SQLServerConnection&#41;](../../../connect/jdbc/reference/getclientconnectionid-method-sqlserverconnection.md)|Ottiene l'ID connessione del tentativo di connessione più recente, indipendentemente dalla riuscita o meno del tentativo.|  
|[getClientInfo](../../../connect/jdbc/reference/getclientinfo-method-sqlserverconnection.md)|Recupera informazioni relative alle proprietà delle informazioni client supportate dal driver JDBC.|  
|[getDisableStatementPooling](../../../connect/jdbc/reference/getdisablestatementpooling-method-sqlserverconnection.md)|Restituisce il valore della proprietà di connessione **disableStatementPooling**. Questa impostazione controlla se il pool di istruzioni è abilitato o meno per questa connessione.|
|[getDiscardedServerPreparedStatementCount](../../../connect/jdbc/reference/getdiscardedserverpreparedstatementcount-method-sqlserverconnection.md)|Restituisce il numero di azioni di annullamento della preparazione per le istruzioni preparate attualmente in sospeso.|
|[getEnablePrepareOnFirstPreparedStatementCall](../../../connect/jdbc/reference/getenableprepareonfirstpreparedstatementcall-method-sqlserverconnection.md)|Restituisce il valore della proprietà di connessione **enablePrepareOnFirstPreparedStatementCall**.|
|[getHoldability](../../../connect/jdbc/reference/getholdability-method-sqlserverconnection.md)|Recupera la trattenibilità corrente per gli oggetti [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) creati usando l'oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[getMetaData](../../../connect/jdbc/reference/getmetadata-method-sqlserverconnection.md)|Recupera un oggetto [SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md) che contiene metadati sul database per cui questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) rappresenta una connessione.|  
|[getServerPreparedStatementDiscardThreshold](../../../connect/jdbc/reference/getserverpreparedstatementdiscardthreshold-method-sqlserverconnection.md)|Restituisce il valore della proprietà di connessione **serverPreparedStatementDiscardThreshold**.|  
|[getStatementHandleCacheEntryCount](../../../connect/jdbc/reference/getstatementhandlecacheentrycount-method-sqlserverconnection.md)|Restituisce il numero corrente di handle di istruzioni preparate in pool.|  
|[getStatementPoolingCacheSize](../../../connect/jdbc/reference/getstatementpoolingcachesize-method-sqlserverconnection.md)|Restituisce le dimensioni della cache delle istruzioni preparate per questa connessione.|  
|[getTransactionIsolation](../../../connect/jdbc/reference/gettransactionisolation-method-sqlserverconnection.md)|Recupera il livello di isolamento della transazione corrente per questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[getTypeMap](../../../connect/jdbc/reference/gettypemap-method-sqlserverconnection.md)|Recupera l'oggetto Map associato a questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverconnection.md)|Recupera il primo avviso segnalato dalle chiamate in questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[isClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverconnection.md)|Indica se questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) è stato chiuso.|  
|[isReadOnly](../../../connect/jdbc/reference/isreadonly-method-sqlserverconnection.md)|Indica se questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) è in modalità di sola lettura.|  
|[isStatementPoolingEnabled](../../../connect/jdbc/reference/isstatementpoolingenabled-method-sqlserverconnection.md)|Indica se il pooling dell'istruzione è abilitato o meno per questa connessione.|  
|[isValid](../../../connect/jdbc/reference/isvalid-method-sqlserverconnection.md)|Indica se questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) non è stato chiuso ed è ancora valido.|  
|[nativeSQL](../../../connect/jdbc/reference/nativesql-method-sqlserverconnection.md)|Converte l'istruzione SQL specificata nella grammatica SQL nativa del server di database.|  
|[prepareCall](../../../connect/jdbc/reference/preparecall-method-sqlserverconnection.md)|Crea un oggetto [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md) per la chiamata delle stored procedure del database.|  
|[prepareStatement](../../../connect/jdbc/reference/preparestatement-method-sqlserverconnection.md)|Crea un oggetto [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) per l'invio di istruzioni SQL con parametri al database.|  
|[releaseSavepoint](../../../connect/jdbc/reference/releasesavepoint-method-sqlserverconnection.md)|Rimuove l'oggetto [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md) specificato dalla transazione corrente.|  
|[rollback](../../../connect/jdbc/reference/rollback-method-sqlserverconnection.md)|Annulla tutte le modifiche apportate nella transazione corrente e rilascia eventuali blocchi a livello di database attualmente mantenuti da questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[setAutoCommit](../../../connect/jdbc/reference/setautocommit-method-sqlserverconnection.md)|Imposta la modalità di commit automatico per questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) sullo stato specificato.|  
|[setCatalog](../../../connect/jdbc/reference/setcatalog-method-sqlserverconnection.md)|Imposta il nome di catalogo specificato per selezionare uno spazio secondario del database di questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) nel quale lavorare.|  
|[setClientInfo](../../../connect/jdbc/reference/setclientinfo-method-sqlserverconnection.md)|Imposta il valore delle proprietà delle informazioni client.|  
|[setDisableStatementPooling](../../../connect/jdbc/reference/setdisablestatementpooling-method-sqlserverconnection.md)|Imposta il pooling dell'istruzione su true o false.|  
|[setEnablePrepareOnFirstPreparedStatementCall](../../../connect/jdbc/reference/setenableprepareonfirstpreparedstatementcall-method-sqlserverconnection.md)|Specifica il nuovo valore della proprietà di connessione **enablePrepareOnFirstPreparedStatementCall**.|  
|[setHoldability](../../../connect/jdbc/reference/setholdability-method-sqlserverconnection.md)|Imposta sul valore specificato la trattenibilità corrente per gli oggetti [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) creati usando l'oggetto [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md).|  
|[setReadOnly](../../../connect/jdbc/reference/setreadonly-method-sqlserverconnection.md)|Attiva la modalità di sola lettura per questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) come hint per il driver JDBC in modo da abilitare le ottimizzazioni del database.|  
|[setSavepoint](../../../connect/jdbc/reference/setsavepoint-method-sqlserverconnection.md)|Crea un punto di salvataggio senza nome nella transazione corrente e restituisce il nuovo oggetto [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md) che lo rappresenta.|  
|[setServerPreparedStatementDiscardThreshold](../../../connect/jdbc/reference/setserverpreparedstatementdiscardthreshold-method-sqlserverconnection.md)|Imposta il nuovo valore della proprietà di connessione **serverPreparedStatementDiscardThreshold**.|  
|[setStatementPoolingCacheSize](../../../connect/jdbc/reference/setstatementpoolingcachesize-method-sqlserverconnection.md)|Imposta la dimensione della cache delle istruzioni preparate per questa connessione.|  
|[setTransactionIsolation](../../../connect/jdbc/reference/settransactionisolation-method-sqlserverconnection.md)|Cerca di impostare sul valore specificato il livello di isolamento della transazione corrente per questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
|[setTypeMap](../../../connect/jdbc/reference/settypemap-method-sqlserverconnection.md)|Installa l'oggetto TypeMap specificato come mappa del tipo per questo oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).|  
  
## <a name="inherited-methods"></a>Metodi ereditati  
  
|Classe ereditata da:|Metodi|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.lang.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Vedere anche  
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
