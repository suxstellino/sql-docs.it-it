---
description: Membri di SQLServerCallableStatement
title: Membri di SQLServerCallableStatement | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apitype: Assembly
ms.assetid: 5ebdc186-e50f-4d14-bbf4-95af5051e4a4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f134ee20c60dc45b216122f76482f6a331fa9a35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172866"
---
# <a name="sqlservercallablestatement-members"></a>Membri di SQLServerCallableStatement
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Nelle tabelle seguenti sono elencati i membri esposti dalla classe [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md).  
  
## <a name="constructors"></a>Costruttori  
 Nessuno.  
  
## <a name="fields"></a>Campi  
  
|Classe ereditata da:|Metodi|  
|---------------------------|-------------|  
|java.sql.Statement|CLOSE_ALL_RESULTS, CLOSE_CURRENT_RESULT, EXECUTE_FAILED, KEEP_CURRENT_RESULT, NO_GENERATED_KEYS, RETURN_GENERATED_KEYS, SUCCESS_NO_INFO|  
  
## <a name="inherited-fields"></a>Campi ereditati  
 No.  
  
## <a name="methods"></a>Metodi  
  
|Nome|Descrizione|  
|----------|-----------------|  
|[addBatch](../../../connect/jdbc/reference/addbatch-method-sqlserverpreparedstatement.md)|Ereditato da [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md). Aggiunge un set di parametri al batch di comandi per questo oggetto CallableStatement.|  
|[cancel](../../../connect/jdbc/reference/cancel-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Annulla l'istruzione SQL attualmente in esecuzione tramite questo oggetto CallableStatement.|  
|[clearBatch](../../../connect/jdbc/reference/clearbatch-method-sqlserverpreparedstatement.md)|Ereditato da [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md). Svuota l'elenco corrente di comandi SQL per questo oggetto CallableStatement.|  
|[clearParameters](../../../connect/jdbc/reference/clearparameters-method-sqlserverpreparedstatement.md)|Ereditato da [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md). Cancella immediatamente i valori di parametro correnti.|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Cancella tutti gli avvisi segnalati in questo oggetto CallableStatement.|  
|[close](../../../connect/jdbc/reference/close-method-sqlserverpreparedstatement.md)|Ereditato da [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md). Rilascia immediatamente le risorse JDBC e di database di questo oggetto CallableStatement invece di attendere il rilascio automatico.|  
|[execute](../../../connect/jdbc/reference/execute-method-sqlserverpreparedstatement.md)|Ereditato da [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md). Esegue l'istruzione SQL in questo oggetto CallableStatement, che può essere qualsiasi tipo di istruzione SQL.|  
|[executeBatch](../../../connect/jdbc/reference/executebatch-method-sqlserverpreparedstatement.md)|Ereditato da [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md). Invia al database un batch di comandi da eseguire. Se tutti i comandi vengono eseguiti correttamente, restituisce una matrice di conteggi aggiornamenti.|  
|[executeQuery](../../../connect/jdbc/reference/executequery-method-sqlserverpreparedstatement.md)|Ereditato da [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md). Esegue la query SQL in questo oggetto CallableStatement e restituisce l'oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) generato dalla query.|  
|[executeUpdate](../../../connect/jdbc/reference/executeupdate-method-sqlserverpreparedstatement.md)|Ereditato da [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md). Esegue l'istruzione SQL in questo oggetto CallableStatement, che deve essere un'istruzione SQL INSERT, UPDATE, MERGE o DELETE oppure un'istruzione SQL che non restituisce valori, quale un'istruzione DDL.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera l'oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) che ha generato questo oggetto CallableStatement.|  
|[getDateTimeOffset](../../../connect/jdbc/reference/getdatetimeoffset-method-sqlservercallablestatement.md)|Recupera il valore della colonna specificata come oggetto della [classe DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md).|  
|[getFetchDirection](../../../connect/jdbc/reference/getfetchdirection-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera la direzione per il recupero di righe da tabelle di database che rappresenta l'impostazione predefinita per i set di risultati generati da questo oggetto CallableStatement.|  
|[getFetchSize](../../../connect/jdbc/reference/getfetchsize-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera il numero di righe del set di risultati che rappresenta la dimensione di recupero predefinita per gli oggetti dei set di risultati generati da questo oggetto CallableStatement.|  
|[getGeneratedKeys](../../../connect/jdbc/reference/getgeneratedkeys-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera le chiavi generate automaticamente create come risultato dell'esecuzione di questo oggetto CallableStatement.|  
|[getMaxFieldSize](../../../connect/jdbc/reference/getmaxfieldsize-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera il numero massimo di byte che possono essere restituiti per i valori delle colonne binarie o di tipo carattere in un oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) generato da questo oggetto CallableStatement.|  
|[getMaxRows](../../../connect/jdbc/reference/getmaxrows-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera il numero massimo di righe che un oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) generato da questo oggetto CallableStatement può contenere.|  
|[getMetaData](../../../connect/jdbc/reference/getmetadata-method-sqlserverpreparedstatement.md)|Ereditato da [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md). Recupera una [classe SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md) che contiene informazioni sulle colonne dell'oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) che saranno restituite quando viene eseguito questo oggetto CallableStatement.|  
|[getMoreResults](../../../connect/jdbc/reference/getmoreresults-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Passa al risultato successivo di questo oggetto CallableStatement.|  
|[getParameterMetaData](../../../connect/jdbc/reference/getparametermetadata-method-sqlserverpreparedstatement.md)|Ereditato da [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md). Recupera il numero, i tipi e le proprietà dei parametri di questo oggetto CallableStatement.|  
|[getArray](../../../connect/jdbc/reference/getarray-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come oggetto Array.|  
|[getAsciiStream](../../../connect/jdbc/reference/getasciistream-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come flusso di caratteri **ASCII**.|  
|[getBigDecimal](../../../connect/jdbc/reference/getbigdecimal-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come oggetto java.math.BigDecimal.|  
|[getBinaryStream](../../../connect/jdbc/reference/getbinarystream-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come flusso binario di byte senza interruzioni.|  
|[getBlob](../../../connect/jdbc/reference/getblob-method-sqlservercallablestatement.md)|Recupera il valore del parametro Blob JDBC designato come oggetto BLOB nel linguaggio di programmazione Java.|  
|[getboolean](../../../connect/jdbc/reference/getboolean-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come valore **booleano**.|  
|[getByte](../../../connect/jdbc/reference/getbyte-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come valore **byte**.|  
|[getBytes](../../../connect/jdbc/reference/getbytes-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come matrice di byte.|  
|[getCharacterStream](../../../connect/jdbc/reference/getcharacterstream-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come oggetto java.io.Reader.|  
|[getClob](../../../connect/jdbc/reference/getclob-method-sqlservercallablestatement.md)|Recupera il valore del parametro Blob JDBC designato come oggetto Clob nel linguaggio di programmazione Java.|  
|[getDate](../../../connect/jdbc/reference/getdate-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come oggetto java.sql.Date nel linguaggio di programmazione Java.|  
|[getDateTimeOffset](../../../connect/jdbc/reference/getdatetimeoffset-method-sqlservercallablestatement.md)|Recupera il valore della colonna specificata come oggetto della [classe DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md).|  
|[getDouble](../../../connect/jdbc/reference/getdouble-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come oggetto **double** nel linguaggio di programmazione Java.|  
|[getFloat](../../../connect/jdbc/reference/getfloat-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come oggetto **float** nel linguaggio di programmazione Java.|  
|[getInt](../../../connect/jdbc/reference/getint-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come oggetto **int** nel linguaggio di programmazione Java.|  
|[getLong](../../../connect/jdbc/reference/getlong-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come oggetto **long** nel linguaggio di programmazione Java.|  
|[getNCharacterStream](../../../connect/jdbc/reference/getncharacterstream-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come oggetto Reader.|  
|[getNClob](../../../connect/jdbc/reference/getnclob-method-sqlservercallablestatement.md)|Recupera il valore del parametro **NCLOB** JDBC designato come oggetto **NClob** nel linguaggio di programmazione Java.|  
|[getNString](../../../connect/jdbc/reference/getnstring-method-sqlservercallablestatement.md)|Recupera il valore del parametro **NCHAR**, **NVARCHAR** o **LONGNVARCHAR** designato come String nel linguaggio di programmazione Java.|  
|[getObject](../../../connect/jdbc/reference/getobject-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come oggetto nel linguaggio di programmazione Java.|  
|[getQueryTimeout](../../../connect/jdbc/reference/getquerytimeout-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera il numero di secondi di attesa di [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] prima dell'esecuzione di questo oggetto CallableStatement.|  
|[getRef](../../../connect/jdbc/reference/getref-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come oggetto Ref nel linguaggio di programmazione Java.|  
|[getResponseBuffering](../../../connect/jdbc/reference/getresponsebuffering-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera la modalità di memorizzazione delle risposte nel buffer per questo oggetto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getResultSet](../../../connect/jdbc/reference/getresultset-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera il risultato corrente come oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getResultSetConcurrency](../../../connect/jdbc/reference/getresultsetconcurrency-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera la concorrenza dei set di risultati per gli oggetti [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) generati da questo oggetto CallableStatement.|  
|[getResultSetHoldability](../../../connect/jdbc/reference/getresultsetholdability-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera la trattenibilità dei set di risultati per gli oggetti [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) generati da questo oggetto CallableStatement.|  
|[getResultSetType](../../../connect/jdbc/reference/getresultsettype-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera il tipo dei set di risultati per gli oggetti [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) generati da questo oggetto CallableStatement.|  
|[getShort](../../../connect/jdbc/reference/getshort-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come valore **short** nel linguaggio di programmazione Java.|  
|[getString](../../../connect/jdbc/reference/getstring-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come oggetto **String** nel linguaggio di programmazione Java.|  
|[getSQLXML](../../../connect/jdbc/reference/getsqlxml-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come oggetto java.sql.SQLXML.|  
|[getTime](../../../connect/jdbc/reference/gettime-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come oggetto java.sql.Time nel linguaggio di programmazione Java.|  
|[getTimestamp](../../../connect/jdbc/reference/gettimestamp-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come oggetto java.sql.Timestamp nel linguaggio di programmazione Java.|  
|[getUpdateCount](../../../connect/jdbc/reference/getupdatecount-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera il risultato corrente come conteggio aggiornamenti.|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlservercallablestatement.md)|Recupera il valore del parametro designato come oggetto URL nel linguaggio di programmazione Java.|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera il primo avviso segnalato dalle chiamate a questo oggetto CallableStatement.|  
|[isClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Indica se questo oggetto Statement è stato chiuso.|  
|[isPoolable](../../../connect/jdbc/reference/ispoolable-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Restituisce un valore che indica se è possibile aggiungere un'istruzione al pool di istruzioni fornito dall'utente.|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlservercallablestatement.md)|Indica se questo oggetto istruzione è un wrapper per l'interfaccia specificata.|  
|[registerOutParameter](../../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md)|Registra il parametro OUT.|  
|[setArray](../../../connect/jdbc/reference/setarray-method-sqlserverpreparedstatement.md)|Ereditato da [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md). Imposta il numero di parametro designato sull'oggetto Array specificato.|  
|[setAsciiStream](../../../connect/jdbc/reference/setasciistream-sqlservercallablestatement.md)|Imposta il parametro designato sul flusso di input specificato.|  
|[setBigDecimal](../../../connect/jdbc/reference/setbigdecimal-method-sqlservercallablestatement.md)|Imposta il numero di parametro designato sull'oggetto BigDecimal specificato.|  
|[setBinaryStream](../../../connect/jdbc/reference/setbinarystream-sqlservercallablestatement.md)|Imposta il parametro designato sul flusso di input specificato.|  
|[setBlob](../../../connect/jdbc/reference/setblob-method-sqlserverpreparedstatement.md)|Ereditato da [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md). Imposta il parametro designato sull'oggetto Blob specificato.|  
|[setboolean](../../../connect/jdbc/reference/setboolean-method-sqlservercallablestatement.md)|Imposta il parametro designato sul valore **Boolean** specificato.|  
|[setByte](../../../connect/jdbc/reference/setbyte-method-sqlservercallablestatement.md)|Imposta il parametro designato sul valore **byte** specificato.|  
|[setBytes](../../../connect/jdbc/reference/setbytes-method-sqlservercallablestatement.md)|Imposta il parametro designato sulla matrice di valori **byte** specificata.|  
|[setCharacterStream](../../../connect/jdbc/reference/setcharacterstream-method-sqlservercallablestatement.md)|Imposta il parametro designato sull'oggetto Reader specificato.|  
|[setClob](../../../connect/jdbc/reference/setclob-method-sqlservercallablestatement.md)|Ereditato da [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md). Imposta il parametro designato sull'oggetto specificato.|  
|[setCursorName](../../../connect/jdbc/reference/setcursorname-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Imposta il nome del cursore SQL sulla stringa specificata, che sarà utilizzata dai metodi Execute successivi.|  
|[setDate](../../../connect/jdbc/reference/setdate-method-sqlservercallablestatement.md)|Imposta il parametro designato sul valore di data specificato.|  
|[setDateTimeOffset](../../../connect/jdbc/reference/setdatetimeoffset-method-sqlservercallablestatement.md)|Imposta il valore della colonna specificata sul valore della [classe DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md).|  
|[setDouble](../../../connect/jdbc/reference/setdouble-method-sqlservercallablestatement.md)|Imposta il parametro designato sul valore **double** specificato.|  
|[setEscapeProcessing](../../../connect/jdbc/reference/setescapeprocessing-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Imposta la modalità di elaborazione di escape.|  
|[setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Fornisce al driver JDBC un hint per la direzione da utilizzare per l'elaborazione delle righe del set di risultati.|  
|[setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Fornisce al driver JDBC un hint per il numero di righe che devono essere recuperate dal database, qualora siano necessarie più righe.|  
|[setFloat](../../../connect/jdbc/reference/setfloat-method-sqlservercallablestatement.md)|Imposta il parametro designato sul valore **float** specificato.|  
|[setInt](../../../connect/jdbc/reference/setint-method-sqlservercallablestatement.md)|Imposta il parametro designato sul valore **int** specificato.|  
|[setLong](../../../connect/jdbc/reference/setlong-method-sqlservercallablestatement.md)|Imposta il parametro designato sul valore **long** specificato.|  
|[setMaxFieldSize](../../../connect/jdbc/reference/setmaxfieldsize-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Imposta sul numero di byte specificato il limite per il numero massimo di byte in una colonna [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) che archivia valori di tipo carattere o binari.|  
|[setMaxRows](../../../connect/jdbc/reference/setmaxrows-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Imposta sul numero specificato il numero massimo di righe che un oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) può contenere.|  
|[setNCharacterStream](../../../connect/jdbc/reference/setncharacterstream-method-sqlservercallablestatement.md)|Imposta il parametro designato sull'oggetto Reader specificato.|  
|[setNClob](../../../connect/jdbc/reference/setnclob-method-sqlservercallablestatement.md)|Imposta il parametro designato sull'oggetto specificato.|  
|[setNString](../../../connect/jdbc/reference/setnstring-method-sqlservercallablestatement.md)|Imposta il parametro designato sull'oggetto String specificato.|  
|[setNull](../../../connect/jdbc/reference/setnull-method-sqlservercallablestatement.md)|Imposta il parametro designato su un valore Null, in base al tipo di parametro da impostare.|  
|[setObject](../../../connect/jdbc/reference/setobject-method-sqlservercallablestatement.md)|Imposta il valore del parametro designato tramite l'oggetto specificato.|  
|[setPoolable](../../../connect/jdbc/reference/setpoolable-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Richiede che un'istruzione sia o meno inserita in un pool. Per impostazione predefinita, un oggetto SQLServerCallableStatement può essere inserito in un pool al momento della creazione.|  
|[setQueryTimeout](../../../connect/jdbc/reference/setquerytimeout-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Imposta il numero di secondi durante i quali il driver rimarrà in attesa dell'esecuzione di un oggetto CallableStatement sul numero di secondi specificato.|  
|[setRef](../../../connect/jdbc/reference/setref-method-sqlserverpreparedstatement.md)|Ereditato da [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md). Imposta il parametro designato sull'oggetto Ref specificato.|  
|[setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Imposta la modalità di memorizzazione delle risposte nel buffer per questo oggetto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) su un valore String **full** o **adaptive** senza distinzione tra maiuscole e minuscole.|  
|[setShort](../../../connect/jdbc/reference/setshort-method-sqlservercallablestatement.md)|Imposta il parametro designato sul valore **short** specificato.|  
|[setString](../../../connect/jdbc/reference/setstring-method-sqlservercallablestatement.md)|Imposta il parametro designato sul valore **String** Java specificato.|  
|[setSQLXML](../../../connect/jdbc/reference/setsqlxml-method-sqlservercallablestatement.md)|Imposta il parametro designato sull'oggetto **SQLXML** specificato.|  
|[setTime](../../../connect/jdbc/reference/settime-method-sqlservercallablestatement.md)|Imposta il parametro designato sul valore di ora specificato.|  
|[setTimestamp](../../../connect/jdbc/reference/settimestamp-method-sqlservercallablestatement.md)|Imposta il parametro designato sul valore timestamp specificato.|  
|[setUnicodeStream](../../../connect/jdbc/reference/setunicodestream-method-sqlserverpreparedstatement.md)|Ereditato da [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md). Imposta il numero di parametro designato sul flusso di input specificato, che disporrà del numero specificato di byte.|  
|[setURL](../../../connect/jdbc/reference/seturl-method-sqlservercallablestatement.md)|Imposta il parametro designato sul valore di URL specificato.|  
|[unwrap](../../../connect/jdbc/reference/unwrap-method-sqlservercallablestatement.md)|Restituisce un oggetto che implementa l'interfaccia specificata per consentire l'accesso ai metodi specifici di [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].|  
|[wasNull](../../../connect/jdbc/reference/wasnull-method-sqlservercallablestatement.md)|Recupera informazioni circa l'eventuale impostazione del valore SQL NULL per l'ultimo parametro OUT letto.|  
  
## <a name="inherited-methods"></a>Metodi ereditati  
  
|Classe ereditata da:|Metodi|  
|---------------------------|-------------|  
|com.microsoft.sqlserver.jdbc.SQLServerPreparedStatement|addBatch, clearBatch, clearParameters, close, execute, executeBatch, executeQuery, executeUpdate, getMetaData, getParameterMetaData, setArray, setAsciiStream, setBigDecimal, setBinaryStream, setBlob, setboolean, setByte, setBytes, setCharacterStream, setClob, setDate, setDouble, setFloat, setInt, setLong, setNull, setObject, setRef, setShort, setString, setTime, setTimestamp, setUnicodeStream, setURL|  
|com.microsoft.sqlserver.jdbc.SQLServerStatement|cancel, clearWarnings, execute, executeUpdate, getConnection, getFetchDirection, getFetchSize, getGeneratedKeys, getMaxFieldSize, getMaxRows, getMoreResults, getQueryTimeout, getResultSet, getResultSetConcurrency, getResultSetHoldability, getResultSetType, getUpdateCount, getWarnings, isPoolable, setCursorName, setEscapeProcessing, setFetchDirection, setFetchSize, setMaxFieldSize, setMaxRows, setPoolable, setQueryTimeout|  
|class java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait,|  
|java.sql.PreparedStatement|addBatch, clearParameters, execute, executeQuery, executeUpdate, getMetaData, getParameterMetaData, getSQLXML, setArray, setAsciiStream, setBigDecimal, setBinaryStream, setBlob, setboolean, setByte, setBytes, setCharacterStream, setClob, setDate, setDate, setDouble, setFloat, setInt, setLong, setNull, setObject, setRef, setShort, setString, setSQLXML, setTime, setTimestamp, setUnicodeStream, setURL|  
|java.sql.Statement|addBatch, cancel, clearBatch, clearWarnings, close, execute, executeBatch, executeQuery, executeUpdate, getConnection, getFetchDirection, getFetchSize, getGeneratedKeys, getMaxFieldSize, getMaxRows, getMoreResults, getQueryTimeout, getResultSet, getResultSetConcurrency, getResultSetHoldability, getResultSetType, getUpdateCount, getWarnings, setCursorName, setEscapeProcessing, setFetchDirection, setFetchSize, setMaxFieldSize, setMaxRows, setQueryTimeout|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Vedere anche  
 [Classe SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
