---
description: Membri di SQLServerPreparedStatement
title: Membri di SQLServerPreparedStatement | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 2363902f-d4c6-4cd4-a5fc-86079eb9e418
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 74ae525bdd1c05eb01f9bce98cba9ef987087a30
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167994"
---
# <a name="sqlserverpreparedstatement-members"></a>Membri di SQLServerPreparedStatement
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Nelle tabelle seguenti sono elencati i membri esposti dalla classe [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).  
  
## <a name="constructors"></a>Costruttori  
 Nessuno.  
  
## <a name="fields"></a>Campi  
 No.  
  
## <a name="inherited-fields"></a>Campi ereditati  
  
|Classe ereditata da:|Metodi|  
|---------------------------|-------------|  
|java.sql.Statement|CLOSE_ALL_RESULTS, CLOSE_CURRENT_RESULT, EXECUTE_FAILED, KEEP_CURRENT_RESULT, NO_GENERATED_KEYS, RETURN_GENERATED_KEYS, SUCCESS_NO_INFO|  
  
## <a name="methods"></a>Metodi  
  
|Nome|Descrizione|  
|----------|-----------------|  
|[addBatch](../../../connect/jdbc/reference/addbatch-method-sqlserverpreparedstatement.md)|Aggiunge un set di parametri al batch di comandi per questo oggetto Statement.|  
|[cancel](../../../connect/jdbc/reference/cancel-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Annulla l'istruzione SQL attualmente in esecuzione tramite questo oggetto Statement.|  
|[clearBatch](../../../connect/jdbc/reference/clearbatch-method-sqlserverpreparedstatement.md)|Svuota l'elenco corrente di comandi SQL per questo oggetto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[clearParameters](../../../connect/jdbc/reference/clearparameters-method-sqlserverpreparedstatement.md)|Cancella immediatamente i valori di parametro correnti.|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Cancella tutti gli avvisi segnalati in questo oggetto Statement.|  
|[close](../../../connect/jdbc/reference/close-method-sqlserverpreparedstatement.md)|Rilascia immediatamente le risorse JDBC e di database di questo oggetto Statement invece di attendere il rilascio automatico.|  
|[execute](../../../connect/jdbc/reference/execute-method-sqlserverpreparedstatement.md)|Esegue l'istruzione SQL in questo oggetto Statement, che può essere qualsiasi tipo di istruzione SQL.|  
|[executeBatch](../../../connect/jdbc/reference/executebatch-method-sqlserverpreparedstatement.md)|Invia al database un batch di comandi da eseguire. Se tutti i comandi vengono eseguiti correttamente, restituisce una matrice di conteggi aggiornamenti.|  
|[executeQuery](../../../connect/jdbc/reference/executequery-method-sqlserverpreparedstatement.md)|Esegue la query SQL in questo oggetto Statement e restituisce l'oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) generato dalla query.|  
|[executeUpdate](../../../connect/jdbc/reference/executeupdate-method-sqlserverpreparedstatement.md)|Esegue l'istruzione SQL in questo oggetto Statement, che deve essere un'istruzione SQL INSERT, UPDATE, MERGE o DELETE oppure un'istruzione SQL che non restituisce valori, quale un'istruzione DDL.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera l'oggetto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) che ha generato questo oggetto Statement.|  
|[getFetchDirection](../../../connect/jdbc/reference/getfetchdirection-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera la direzione per il recupero di righe da tabelle di database che rappresenta l'impostazione predefinita per i set di risultati generati da questo oggetto Statement.|  
|[getFetchSize](../../../connect/jdbc/reference/getfetchsize-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera il numero di righe del set di risultati che rappresenta la dimensione di recupero predefinita per gli oggetti dei set di risultati generati da questo oggetto Statement.|  
|[getGeneratedKeys](../../../connect/jdbc/reference/getgeneratedkeys-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera le chiavi generate automaticamente create come risultato dell'esecuzione di questo oggetto Statement.|  
|[getMaxFieldSize](../../../connect/jdbc/reference/getmaxfieldsize-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera il numero massimo di byte che possono essere restituiti per i valori delle colonne binarie o di tipo carattere in un oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) generato da questo oggetto Statement.|  
|[getMaxRows](../../../connect/jdbc/reference/getmaxrows-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera il numero massimo di righe che un oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) generato da questo oggetto Statement può contenere.|  
|[getMetaData](../../../connect/jdbc/reference/getmetadata-method-sqlserverpreparedstatement.md)|Recupera una [classe SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md) che contiene informazioni sulle colonne dell'oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) che saranno restituite quando viene eseguito questo oggetto Statement.|  
|[getMoreResults](../../../connect/jdbc/reference/getmoreresults-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Passa al risultato successivo di questo oggetto Statement.|  
|[getParameterMetaData](../../../connect/jdbc/reference/getparametermetadata-method-sqlserverpreparedstatement.md)|Recupera il numero, i tipi e le proprietà dei parametri di questo oggetto Statement.|  
|[getResponseBuffering](../../../connect/jdbc/reference/getresponsebuffering-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera la modalità di memorizzazione delle risposte nel buffer per questo oggetto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getQueryTimeout](../../../connect/jdbc/reference/getquerytimeout-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera il numero di secondi di attesa di [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] prima dell'esecuzione di questo oggetto Statement.|  
|[getResultSet](../../../connect/jdbc/reference/getresultset-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera il risultato corrente come oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getResultSetConcurrency](../../../connect/jdbc/reference/getresultsetconcurrency-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera la concorrenza dei set di risultati per gli oggetti [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) generati da questo oggetto Statement.|  
|[getResultSetHoldability](../../../connect/jdbc/reference/getresultsetholdability-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera la trattenibilità dei set di risultati per gli oggetti [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) generati da questo oggetto Statement.|  
|[getResultSetType](../../../connect/jdbc/reference/getresultsettype-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera il tipo dei set di risultati per gli oggetti [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) generati da questo oggetto Statement.|  
|[getUpdateCount](../../../connect/jdbc/reference/getupdatecount-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera il risultato corrente come conteggio aggiornamenti.|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Recupera il primo avviso segnalato dalle chiamate a questo oggetto Statement.|  
|[isClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Indica se questo oggetto Statement è stato chiuso.|  
|[isPoolable](../../../connect/jdbc/reference/ispoolable-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Restituisce un valore che indica se è possibile aggiungere un'istruzione al pool di istruzioni fornito dall'utente.|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverpreparedstatement.md)|Indica se questo oggetto istruzione è un wrapper per l'interfaccia specificata.|  
|[setArray](../../../connect/jdbc/reference/setarray-method-sqlserverpreparedstatement.md)|Imposta il numero di parametro designato sull'oggetto Array specificato.|  
|[setAsciiStream](../../../connect/jdbc/reference/setasciistream-method-sqlserverpreparedstatement.md)|Imposta il numero di parametro designato sull'oggetto InputStream specificato.|  
|[setBigDecimal](../../../connect/jdbc/reference/setbigdecimal-method-sqlserverpreparedstatement.md)|Imposta il numero di parametro designato sull'oggetto BigDecimal specificato.|  
|[setBinaryStream](../../../connect/jdbc/reference/setbinarystream-method-sqlserverpreparedstatement.md)|Imposta il parametro designato sul flusso di input specificato.|  
|[setBlob](../../../connect/jdbc/reference/setblob-method-sqlserverpreparedstatement.md)|Imposta il parametro designato sull'oggetto Blob specificato.|  
|[setboolean](../../../connect/jdbc/reference/setboolean-method-sqlserverpreparedstatement.md)|Imposta il parametro designato sul valore **Boolean** specificato.|  
|[setByte](../../../connect/jdbc/reference/setbyte-method-sqlserverpreparedstatement.md)|Imposta il parametro designato sul valore **byte** specificato.|  
|[setBytes](../../../connect/jdbc/reference/setbytes-method-sqlserverpreparedstatement.md)|Imposta il parametro designato sulla matrice di byte specificata.|  
|[setCharacterStream](../../../connect/jdbc/reference/setcharacterstream-method-sqlserverpreparedstatement.md)|Imposta il parametro designato sull'oggetto Reader specificato.|  
|[setClob](../../../connect/jdbc/reference/setclob-method-sqlserverpreparedstatement.md)|Imposta il parametro designato sull'oggetto Clob specificato.|  
|[setCursorName](../../../connect/jdbc/reference/setcursorname-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Imposta il nome del cursore SQL sulla stringa specificata, che sarà utilizzata dai metodi Execute successivi.|  
|[setDate](../../../connect/jdbc/reference/setdate-method-sqlserverpreparedstatement.md)|Imposta il parametro designato sul valore di data specificato.|  
|[setDateTimeOffset](../../../connect/jdbc/reference/setdatetimeoffset-method-sqlserverpreparedstatement.md)|Imposta il valore della colonna specificata sul valore della [classe DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md).|  
|[setDouble](../../../connect/jdbc/reference/setdouble-method-sqlserverpreparedstatement.md)|Imposta il parametro designato sul valore **double** specificato.|  
|[setEscapeProcessing](../../../connect/jdbc/reference/setescapeprocessing-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Imposta la modalità di elaborazione di escape.|  
|[setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Fornisce al driver JDBC un hint per la direzione da utilizzare per l'elaborazione delle righe del set di risultati.|  
|[setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Fornisce al driver JDBC un hint per il numero di righe che devono essere recuperate dal database, qualora siano necessarie più righe.|  
|[setFloat](../../../connect/jdbc/reference/setfloat-method-sqlserverpreparedstatement.md)|Imposta il parametro designato sul valore **float** specificato.|  
|[setInt](../../../connect/jdbc/reference/setint-method-sqlserverpreparedstatement.md)|Imposta il parametro designato sul valore **int** specificato.|  
|[setLong](../../../connect/jdbc/reference/setlong-method-sqlserverpreparedstatement.md)|Imposta il parametro designato sul valore **long** specificato.|  
|[setMaxFieldSize](../../../connect/jdbc/reference/setmaxfieldsize-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Imposta sul numero di byte specificato il limite per il numero massimo di byte in una colonna [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) che archivia valori di tipo carattere o binari.|  
|[setMaxRows](../../../connect/jdbc/reference/setmaxrows-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Imposta sul numero specificato il numero massimo di righe che un oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) può contenere.|  
|[setNCharacterStream](../../../connect/jdbc/reference/setncharacterstream-method-sqlserverpreparedstatement.md)|Imposta il parametro designato sull'oggetto Reader specificato.|  
|[setNClob](../../../connect/jdbc/reference/setnclob-method-sqlserverpreparedstatement.md)|Imposta il parametro designato sull'oggetto specificato.|  
|[setNull](../../../connect/jdbc/reference/setnull-method-sqlserverpreparedstatement.md)|Imposta il parametro designato su un valore Null, in base al tipo di parametro da impostare.|  
|[setNString](../../../connect/jdbc/reference/setnstring-method-int-java-lang-string.md)|Imposta il parametro designato sull'oggetto **String** specificato.|  
|[setObject](../../../connect/jdbc/reference/setobject-method-sqlserverpreparedstatement.md)|Imposta il valore del parametro designato tramite l'oggetto specificato.|  
|[setPoolable](../../../connect/jdbc/reference/setpoolable-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Richiede che un'istruzione sia o meno inserita in un pool. Per impostazione predefinita, un oggetto SQLServerPreparedStatement può essere inserito in un pool al momento della creazione.|  
|[setQueryTimeout](../../../connect/jdbc/reference/setquerytimeout-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Imposta il numero di secondi durante i quali il driver rimarrà in attesa dell'esecuzione di un oggetto Statement sul numero di secondi specificato.|  
|[setRef](../../../connect/jdbc/reference/setref-method-sqlserverpreparedstatement.md)|Imposta il parametro designato sull'oggetto Ref specificato.|  
|[setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md)|Ereditato da [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). Imposta la modalità di memorizzazione delle risposte nel buffer per questo oggetto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) su un valore String **full** o **adaptive** senza distinzione tra maiuscole e minuscole.|  
|[setShort](../../../connect/jdbc/reference/setshort-method-sqlserverpreparedstatement.md)|Imposta il parametro designato sul valore **short** specificato.|  
|[setString](../../../connect/jdbc/reference/setstring-method-sqlserverpreparedstatement.md)|Imposta il parametro designato sul valore **String** specificato.|  
|[setSQLXML](../../../connect/jdbc/reference/setsqlxml-method-sqlserverpreparedstatement.md)|Imposta il parametro designato sull'oggetto **SQLXML** specificato.|  
|[setTime](../../../connect/jdbc/reference/settime-method-sqlserverpreparedstatement.md)|Imposta il parametro designato sul valore di ora specificato.|  
|[setTimestamp](../../../connect/jdbc/reference/settimestamp-method-sqlserverpreparedstatement.md)|Imposta il parametro designato sul valore timestamp specificato.|  
|[setUnicodeStream](../../../connect/jdbc/reference/setunicodestream-method-sqlserverpreparedstatement.md)|Imposta il numero del parametro designato sul flusso di input specificato, che disporrà del numero specificato di byte.|  
|[setURL](../../../connect/jdbc/reference/seturl-method-sqlserverpreparedstatement.md)|Imposta il parametro designato sul valore di URL specificato.|  
|[unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverpreparedstatement.md)|Restituisce un oggetto che implementa l'interfaccia specificata per consentire l'accesso ai metodi specifici di [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].|  
  
## <a name="inherited-methods"></a>Metodi ereditati  
  
|Classe ereditata da:|Metodi|  
|---------------------------|-------------|  
|com.microsoft.sqlserver.jdbc.SQLServerStatement|cancel, clearWarnings, execute, executeUpdate, getConnection, getFetchDirection, getFetchSize, getGeneratedKeys, getMaxFieldSize, getMaxRows, getMoreResults, getQueryTimeout, getResultSet, getResultSetConcurrency, getResultSetHoldability, getResultSetType, getUpdateCount, getWarnings, isPoolable, setCursorName, setEscapeProcessing, setFetchDirection, setFetchSize, setMaxFieldSize, setMaxRows, setPoolable, setQueryTimeout|  
|java.lang.Object|clone, equals, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Statement|cancel, clearWarnings, execute, executeUpdate, getConnection, getFetchDirection, getFetchSize, getGeneratedKeys, getMaxFieldSize, getMaxRows, getMoreResults, getQueryTimeout, getResultSet, getResultSetConcurrency, getResultSetHoldability, getResultSetType, getUpdateCount, getWarnings, setCursorName, setEscapeProcessing, setFetchDirection, setFetchSize, setMaxFieldSize, setMaxRows, setQueryTimeout|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Vedere anche  
 [Classe SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
