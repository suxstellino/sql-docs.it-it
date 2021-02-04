---
description: Membri di SQLServerResultSet
title: Membri di SQLServerResultSet | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 2a438d5d-2d6a-46a0-a2ae-f35fbae4a472
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bf8e9b71b638a3836ebefb71e3d574183372267e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180106"
---
# <a name="sqlserverresultset-members"></a>Membri di SQLServerResultSet
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Nelle tabelle seguenti vengono elencati i membri esposti dalla classe [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="constructors"></a>Costruttori  
 Nessuno.  
  
## <a name="fields"></a>Campi  
  
|Nome|Descrizione|  
|----------|-----------------|  
|[CONCUR_SS_OPTIMISTIC_CC](../../../connect/jdbc/reference/concur-ss-optimistic-cc-field-sqlserverresultset.md)|Usato per specificare un tipo di concorrenza ottimistica di lettura/scrittura di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] senza blocchi di riga.|  
|[CONCUR_SS_OPTIMISTIC_CCVAL](../../../connect/jdbc/reference/concur-ss-optimistic-ccval-field-sqlserverresultset.md)|Usato per specificare un tipo di concorrenza ottimistica di lettura/scrittura di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] senza blocchi di riga.|  
|[CONCUR_SS_SCROLL_LOCKS](../../../connect/jdbc/reference/concur-ss-scroll-locks-field-sqlserverresultset.md)|Usato per specificare un tipo di concorrenza ottimistica di lettura/scrittura di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] con blocchi di riga.|  
|[TYPE_SS_DIRECT_FORWARD_ONLY](../../../connect/jdbc/reference/type-ss-direct-forward-only-field-sqlserverresultset.md)|Usato per specificare un tipo di cursore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fast forward-only di sola lettura.|  
|[TYPE_SS_SCROLL_DYNAMIC](../../../connect/jdbc/reference/type-ss-scroll-dynamic-field-sqlserverresultset.md)|Usato per specificare un tipo di cursore dinamico di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[TYPE_SS_SCROLL_KEYSET](../../../connect/jdbc/reference/type-ss-scroll-keyset-field-sqlserverresultset.md)|Usato per specificare un tipo di cursore keyset [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[TYPE_SS_SCROLL_STATIC](../../../connect/jdbc/reference/type-ss-scroll-static-field-sqlserverresultset.md)|Usato per specificare un tipo di cursore statico [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[TYPE_SS_SERVER_CURSOR_FORWARD_ONLY](../../../connect/jdbc/reference/type-ss-server-cursor-forward-only-field-sqlserverresultset.md)|Usato per specificare un tipo di cursore [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fast forward-only di sola lettura.|  
  
## <a name="inherited-fields"></a>Campi ereditati  
  
|Classe ereditata da:|Descrizione|  
|---------------------------|-----------------|  
|java.sql.ResultSet|CLOSE_CURSORS_AT_COMMIT, CONCUR_READ_ONLY, CONCUR_UPDATABLE, FETCH_FORWARD, FETCH_REVERSE, FETCH_UNKNOWN, HOLD_CURSORS_OVER_COMMIT, TYPE_FORWARD_ONLY, TYPE_SCROLL_INSENSITIVE, TYPE_SCROLL_SENSITIVE|  
  
## <a name="methods"></a>Metodi  
  
|Nome|Descrizione|  
|----------|-----------------|  
|[absolute](../../../connect/jdbc/reference/absolute-method-sqlserverresultset.md)|Sposta il cursore nella riga specificata in questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[afterLast](../../../connect/jdbc/reference/afterlast-method-sqlserverresultset.md)|Sposta il cursore nella posizione successiva all'ultima riga di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[beforeFirst](../../../connect/jdbc/reference/beforefirst-method-sqlserverresultset.md)|Sposta il cursore nella posizione precedente alla prima riga di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[cancelRowUpdates](../../../connect/jdbc/reference/cancelrowupdates-method-sqlserverresultset.md)|Annulla gli aggiornamenti eseguiti alla riga corrente in questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverresultset.md)|Cancella tutti gli avvisi segnalati su questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[close](../../../connect/jdbc/reference/close-method-sqlserverresultset.md)|Rilascia immediatamente le risorse JDBC e di database di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) anziché attendere che tale operazione venga eseguita al momento della chiusura automatica.|  
|[deleteRow](../../../connect/jdbc/reference/deleterow-method-sqlserverresultset.md)|Elimina la riga corrente da questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) e dal database sottostante.|  
|[finalize](../../../connect/jdbc/reference/finalize-method-sqlserverresultset.md)|Chiude in modo esplicito questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[findColumn](../../../connect/jdbc/reference/findcolumn-method-sqlserverresultset.md)|Recupera l'indice della prima colonna corrispondente per il nome di colonna specificato in questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[first](../../../connect/jdbc/reference/first-method-sqlserverresultset.md)|Sposta il cursore alla prima riga di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getArray](../../../connect/jdbc/reference/getarray-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto Array.|  
|[getAsciiStream](../../../connect/jdbc/reference/getasciistream-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come flusso di caratteri ASCII.|  
|[getBigDecimal](../../../connect/jdbc/reference/getbigdecimal-method-sqlserverresultset.md)|Recupera il valore dell'indice della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto java.math.BigDecimal.|  
|[getBinaryStream](../../../connect/jdbc/reference/getbinarystream-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come flusso binario di byte non interpretati.|  
|[getBlob](../../../connect/jdbc/reference/getblob-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto BLOB nel linguaggio di programmazione Java.|  
|[getBoolean](../../../connect/jdbc/reference/getboolean-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto **booleano** nel linguaggio di programmazione Java.|  
|[getByte](../../../connect/jdbc/reference/getbyte-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto **byte** nel linguaggio di programmazione Java.|  
|[getBytes](../../../connect/jdbc/reference/getbytes-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come matrice **byte** nel linguaggio di programmazione Java.|  
|[getCharacterStream](../../../connect/jdbc/reference/getcharacterstream-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto java.io.Reader.|  
|[getClob](../../../connect/jdbc/reference/getclob-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto CLOB nel linguaggio di programmazione Java.|  
|[getConcurrency](../../../connect/jdbc/reference/getconcurrency-method-sqlserverresultset.md)|Recupera la modalità di concorrenza di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getCursorName](../../../connect/jdbc/reference/getcursorname-method-sqlserverresultset.md)|Recupera il nome del cursore SQL usato da questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getDate](../../../connect/jdbc/reference/getdate-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto java.sql.Date nel linguaggio di programmazione Java.|  
|[getDateTimeOffset](../../../connect/jdbc/reference/getdatetimeoffset-sqlserverresultset.md)|Recupera il valore della colonna specificata come oggetto della [classe DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md).|  
|[getDouble](../../../connect/jdbc/reference/getdouble-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto **double** nel linguaggio di programmazione Java.|  
|[getFetchDirection](../../../connect/jdbc/reference/getfetchdirection-method-sqlserverresultset.md)|Recupera la direzione di recupero di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getFetchSize](../../../connect/jdbc/reference/getfetchsize-method-sqlserverresultset.md)|Recupera le dimensioni del recupero di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getFloat](../../../connect/jdbc/reference/getfloat-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto **float** nel linguaggio di programmazione Java.|  
|[getHoldability](../../../connect/jdbc/reference/getholdability-method-sqlserverresultset.md)|Recupera la trattenibilità di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getInt](../../../connect/jdbc/reference/getint-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto **int** nel linguaggio di programmazione Java.|  
|[getLong](../../../connect/jdbc/reference/getlong-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto **long** nel linguaggio di programmazione Java.|  
|[getMetaData](../../../connect/jdbc/reference/getmetadata-method-sqlserverresultset.md)|Recupera il numero, i tipi e le proprietà delle colonne di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getNCharacterStream](../../../connect/jdbc/reference/getncharacterstream-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente dell'oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto Reader.|  
|[getNClob](../../../connect/jdbc/reference/getnclob-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente dell'oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto **NClob** nel linguaggio di programmazione Java.|  
|[getNString](../../../connect/jdbc/reference/getnstring-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente dell'oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto String nel linguaggio di programmazione Java.|  
|[getObject](../../../connect/jdbc/reference/getobject-method-sqlserverresultset.md)|Ottiene il valore della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto nel linguaggio di programmazione Java.|  
|[getRef](../../../connect/jdbc/reference/getref-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto Ref nel linguaggio di programmazione Java.|  
|[getRow](../../../connect/jdbc/reference/getrow-method-sqlserverresultset.md)|Recupera il numero di riga corrente.|  
|[getShort](../../../connect/jdbc/reference/getshort-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto **short** nel linguaggio di programmazione Java.|  
|[getStatement](../../../connect/jdbc/reference/getstatement-method-sqlserverresultset.md)|Recupera l'oggetto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) che ha generato questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getString](../../../connect/jdbc/reference/getstring-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente dell'oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto **String** nel linguaggio di programmazione Java.|  
|[getSQLXML](../../../connect/jdbc/reference/getsqlxml-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente dell'oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto **SQLXML**.|  
|[getTime](../../../connect/jdbc/reference/gettime-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto java.sql.Time nel linguaggio di programmazione Java.|  
|[getTimestamp](../../../connect/jdbc/reference/gettimestamp-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto java.sql.Timestamp nel linguaggio di programmazione Java.|  
|[getType](../../../connect/jdbc/reference/gettype-method-sqlserverresultset.md)|Recupera il tipo di cursore di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getUnicodeStream](../../../connect/jdbc/reference/getunicodestream-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come flusso di caratteri Unicode.|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlserverresultset.md)|Recupera il valore della colonna designata nella riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) come oggetto URL.|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverresultset.md)|Recupera il primo avviso segnalato dalle chiamate a questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[insertRow](../../../connect/jdbc/reference/insertrow-method-sqlserverresultset.md)|Inserisce il contenuto della riga di inserimento in questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) e nel database.|  
|[isAfterLast](../../../connect/jdbc/reference/isafterlast-method-sqlserverresultset.md)|Recupera informazioni sull'eventuale presenza del cursore nella posizione successiva all'ultima riga in questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[isBeforeFirst](../../../connect/jdbc/reference/isbeforefirst-method-sqlserverresultset.md)|Recupera informazioni sull'eventuale presenza del cursore nella posizione precedente alla prima riga in questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[isClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverresultset.md)|Indica se questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) è stato chiuso.|  
|[isFirst](../../../connect/jdbc/reference/isfirst-method-sqlserverresultset.md)|Recupera informazioni circa l'eventuale presenza del cursore nella prima riga di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[isLast](../../../connect/jdbc/reference/islast-method-sqlserverresultset.md)|Recupera informazioni circa l'eventuale presenza del cursore nell'ultima riga di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[last](../../../connect/jdbc/reference/last-method-sqlserverresultset.md)|Sposta il cursore nell'ultima riga in questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[moveToCurrentRow](../../../connect/jdbc/reference/movetocurrentrow-method-sqlserverresultset.md)|Sposta il cursore nella posizione memorizzata, generalmente la riga corrente.|  
|[moveToInsertRow](../../../connect/jdbc/reference/movetoinsertrow-method-sqlserverresultset.md)|Sposta il cursore nella riga di inserimento.|  
|[avanti](../../../connect/jdbc/reference/next-method-sqlserverresultset.md)|Sposta il cursore di una riga verso il basso dalla posizione corrente.|  
|[previous](../../../connect/jdbc/reference/previous-method-sqlserverresultset.md)|Sposta il cursore nella riga precedente in questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[refreshRow](../../../connect/jdbc/reference/refreshrow-method-sqlserverresultset.md)|Aggiorna la riga corrente con il relativo valore più recente nel database.|  
|[relative](../../../connect/jdbc/reference/relative-method-sqlserverresultset.md)|Sposta il cursore della quantità specificata di righe rispetto alla riga corrente, in direzione positiva o negativa.|  
|[rowDeleted](../../../connect/jdbc/reference/rowdeleted-method-sqlserverresultset.md)|Recupera informazioni circa l'eventuale eliminazione di una riga.|  
|[rowInserted](../../../connect/jdbc/reference/rowinserted-method-sqlserverresultset.md)|Recupera informazioni sull'eventuale presenza di un inserimento nella riga corrente.|  
|[rowUpdated](../../../connect/jdbc/reference/rowupdated-method-sqlserverresultset.md)|Recupera informazioni sull'eventuale aggiornamento della riga corrente.|  
|[setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverresultset.md)|Specifica un hint per la direzione di elaborazione delle righe in questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverresultset.md)|Offre al driver JDBC un hint per il numero di righe che devono essere recuperate dal database, qualora siano necessarie più righe per questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[updateArray](../../../connect/jdbc/reference/updatearray-method-sqlserverresultset.md)|Aggiorna la colonna designata con un oggetto Array.|  
|[updateAsciiStream](../../../connect/jdbc/reference/updateasciistream-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore del flusso ASCII.|  
|[updateBigDecimal](../../../connect/jdbc/reference/updatebigdecimal-method-sqlserverresultset.md)|Aggiorna la colonna designata con un oggetto BigDecimal.|  
|[updateBinaryStream](../../../connect/jdbc/reference/updatebinarystream-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore del flusso binario.|  
|[updateBlob](../../../connect/jdbc/reference/updateblob-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore java.sql.Blob.|  
|[updateBoolean](../../../connect/jdbc/reference/updateboolean-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore **booleano**.|  
|[updateByte](../../../connect/jdbc/reference/updatebyte-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore **byte**.|  
|[updateBytes](../../../connect/jdbc/reference/updatebytes-method-sqlserverresultset.md)|Aggiorna la colonna designata con una matrice di valori **byte**.|  
|[updateCharacterStream](../../../connect/jdbc/reference/updatecharacterstream-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore del flusso di caratteri.|  
|[updateClob](../../../connect/jdbc/reference/updateclob-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore java.sql.Clob.|  
|[updateDate](../../../connect/jdbc/reference/updatedate-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore di data.|  
|[updateDateTimeOffset](../../../connect/jdbc/reference/updatedatetimeoffset-sqlserverresultset.md)|Aggiorna una colonna della [classe DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md).|  
|[updateDouble](../../../connect/jdbc/reference/updatedouble-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore **double**.|  
|[updateFloat](../../../connect/jdbc/reference/updatefloat-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore **float**.|  
|[updateInt](../../../connect/jdbc/reference/updateint-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore **int**.|  
|[updateLong](../../../connect/jdbc/reference/updatelong-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore **long**.|  
|[updateNCharacterStream](../../../connect/jdbc/reference/updatencharacterstream-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore del flusso di caratteri.|  
|[updateNClob](../../../connect/jdbc/reference/updatenclob-method-sqlserverresultset.md)|Aggiorna la colonna designata con il valore di oggetto specificato.|  
|[updateNString](../../../connect/jdbc/reference/updatenstring-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore **String**.|  
|[updateNull](../../../connect/jdbc/reference/updatenull-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore Null.|  
|[updateObject](../../../connect/jdbc/reference/updateobject-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore **Object**.|  
|[updateRef](../../../connect/jdbc/reference/updateref-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore java.sql.Ref.|  
|[updateRow](../../../connect/jdbc/reference/updaterow-method-sqlserverresultset.md)|Aggiorna il database sottostante con il nuovo contenuto della riga corrente di questo oggetto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[updateShort](../../../connect/jdbc/reference/updateshort-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore **short**.|  
|[updateString](../../../connect/jdbc/reference/updatestring-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore **String**.|  
|[updateSQLXML](../../../connect/jdbc/reference/updatesqlxml-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore **SQLXML**.|  
|[updateTime](../../../connect/jdbc/reference/updatetime-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore di ora.|  
|[updateTimestamp](../../../connect/jdbc/reference/updatetimestamp-method-sqlserverresultset.md)|Aggiorna la colonna designata con un valore di timestamp.|  
|[wasNull](../../../connect/jdbc/reference/wasnull-method-sqlserverresultset.md)|Verifica se l'ultimo valore letto è un valore Null.|  
  
## <a name="inherited-methods"></a>Metodi ereditati  
  
|Classe ereditata da:|Metodi|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Vedere anche  
 [Classe SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
