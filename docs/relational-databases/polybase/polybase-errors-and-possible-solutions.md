---
title: Errori di polibase e possibili soluzioni
description: Riferimento di base per gli errori e le soluzioni consigliate.
ms.date: 02/17/2021
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
dev_langs:
- TSQL
- XML
f1_keywords:
- PolyBase, monitoring
- PolyBase, performance monitoring
helpviewer_keywords:
- PolyBase, troubleshooting
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: ''
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-2016'
ms.openlocfilehash: 463b54aefd36e74318331c90cf2c944734f8a5cc
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839368"
---
# <a name="polybase-errors-and-possible-solutions"></a>Errori di polibase e possibili soluzioni

[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

Questo articolo contiene scenari comuni di errore e soluzioni per la polibase.

Per altre informazioni sul monitoraggio e la risoluzione dei problemi di base, vedere [monitorare e risolvere i problemi di base](polybase-troubleshooting.md).

Per i percorsi comuni dei file di log di base in Windows e Linux, vedere [monitorare e risolvere i problemi di base](polybase-troubleshooting.md#log-file-locations).


## <a name="error-messages-and-possible-solutions"></a>Messaggi di errore e possibili soluzioni


### <a name="service-account-change"></a>Modifica dell'account del servizio

Messaggio di errore di esempio:

> 107035; autorizzazione DMS non riuscita a causa di [Dominio\Utente] non è membro del gruppo [PdwDataMovementAccess] <BR>
> 107017; l'intestazione di controllo DMS non è valida:

Questo errore è probabilmente dovuto alla modifica dell'account del servizio di base. Per modificare gli account del servizio per il motore PolyBase e PolyBase Data Movement Service, disinstallare e reinstallare la funzionalità PolyBase.


### <a name="data-movement-service-permissions-errors"></a>Errori delle autorizzazioni del servizio di spostamento dati

Per altre informazioni sulla risoluzione dei problemi relativi alle autorizzazioni per il servizio di spostamento dei dati, vedere [autorizzazioni dell'account del servizio di base e errori comuni osservati quando mancanti](https://techcommunity.microsoft.com/t5/sql-server-support/polybase-service-account-permissions-and-common-errors-observed/ba-p/2112711).


### <a name="windows-authentication-failure"></a>Errore di autenticazione di Windows

Per ulteriori informazioni sulla risoluzione dei problemi relativi alle autorizzazioni relative a un errore nell'autenticazione di Windows, vedere l'articolo relativo alle [autorizzazioni dell'account del servizio di base e agli errori comuni osservati quando mancanti](https://techcommunity.microsoft.com/t5/sql-server-support/polybase-service-account-permissions-and-common-errors-observed/ba-p/2112711).


### <a name="cannot-execute-the-query-remote-query"></a>Impossibile eseguire la query "remote query" 

Messaggio di errore di esempio:

> Messaggio 7320, livello 16, stato 110, riga 14<BR>
> Impossibile eseguire la query "remote query" sul provider OLE DB "SQLNCLI11" per il server collegato "(null)". Query interrotta. è stata raggiunta la soglia massima di rifiuto (0 righe) durante la lettura da un'origine esterna: 1 righe rifiutate su un totale di 1 righe elaborate.
(/Nation/sensors.ldjson.txt) Numero ordinale di colonna: 0, tipo di dati previsto: INT, valore danneggiato: {"ID": "S2740036465E2B", "Time": "2016-02-26T16:59:02.9300000 Z", "Temp": 23,3, "Hum": 0.77, "Wind": 17, "Press": 1032, "loc": [-76.90914996169623, 38.8929314364726]} (errore di conversione della colonna), errore: conversione del tipo di dati NVARCHAR in INT

Tenere presente che è possibile che si verifichino derivazioni di questo errore. Il nome del primo file rifiutato viene visualizzato in SQL Server Management Studio (SSMS) con i tipi di dati o i valori che hanno causato il danneggiamento.

**Motivo possibile:**  
Il motivo per cui si verifica questo errore è dovuto al fatto che ogni file dispone di uno schema diverso. Il DDL della tabella esterna di base, quando si fa riferimento a una directory, legge in modo ricorsivo tutti i file in tale directory. Quando si verifica una mancata corrispondenza tra una colonna o un tipo di dati, questo errore può essere visualizzato in SSMS.

**Possibile soluzione:**  
Se i dati di ogni tabella sono costituiti da un file, usare il nome file nella sezione LOCATION preceduta dalla directory dei file esterni. Se sono presenti più file per ogni tabella, inserire ogni set di file in directory diverse nell'archivio BLOB di Azure. POSIZIONE del punto alla directory anziché un file specifico. Questa soluzione è consigliata.

**Esempio:**  

```sqlsyntax
Create External Table foo
(col1 int)WITH (LOCATION='/bar/foobar.txt',DATA_SOURCE…); OR
Create External Table foo
(col1 int) WITH (LOCATION = '/bar/', DATA_SOURCE…);
```


### <a name="kerberos-support"></a>Supporto per Kerberos

SQL Server è configurato per accedere a un cluster Hadoop supportato. La sicurezza Kerberos non viene applicata nel cluster Hadoop.

Se si seleziona dalla tabella esterna, viene restituito l'errore seguente:

> Messaggio 105019, livello 16, stato 1, riga 55<BR>
> L'accesso alla tabella esterna non è riuscito a causa di un errore interno:' eccezione Java generata alla chiamata a HdfsBridge_Connect: errore [non è possibile creare un'istanza di LoginClass] durante l'accesso al file esterno '.<BR>
> Messaggio 7320, livello 16, stato 110, riga 55<BR>
> Impossibile eseguire la query "remote query" sul provider OLE DB "SQLNCLI11" per il server collegato "(null)". L'accesso alla tabella esterna non è riuscito a causa di un errore interno:' eccezione Java generata alla chiamata a HdfsBridge_Connect: errore [non è possibile creare un'istanza di LoginClass] durante l'accesso al file esterno '.

Nell'interrogazione del log del server DWEngine viene visualizzato l'errore seguente:

> [Thread: 16432] [EngineInstrumentation:EngineQueryErrorEvent] (Errore, elevato):<BR>
> L'accesso alla tabella esterna non è riuscito a causa di un errore interno:' eccezione Java generata alla chiamata a HdfsBridge_Connect: errore [com. Microsoft. pobase. client. KerberosSecureLogin] durante l'accesso al file esterno '.
Microsoft. SqlServer. DataWarehouse. Common. ErrorHandling. MppSqlException: l'accesso alla tabella esterna non è riuscito a causa di un errore interno:' eccezione Java generata alla chiamata a HdfsBridge_Connect: errore [com. Microsoft. polibase. client. KerberosSecureLogin] durante l'accesso al file esterno '. ---> Microsoft. SqlServer. Data warehouse. datamovement. Common. ExternalAccess. HdfsAccessException: eccezione Java generata alla chiamata a HdfsBridge_Connect: errore [com. Microsoft. polibase. client. KerberosSecureLogin] durante l'accesso al file esterno.

**Motivo possibile:**  
Kerberos non è abilitato nel cluster Hadoop, ma la sicurezza Kerberos è abilitata in core-site.xml, yarn-site.xml o il hdfs-site.xml che si trova per impostazione predefinita in Programmi\Microsoft SQL Server\MSSQL13. MSSQLSERVER\MSSQL\Binn\Polybase\Hadoop\conf. In Linux i file sono disponibili per impostazione predefinita in/var/opt/MSSQL/Binn/polybase/Hadoop/conf/.

**Possibile soluzione:**  
Impostare come commento le informazioni di sicurezza Kerberos dei file menzionati in precedenza.

Per ulteriori informazioni sulla risoluzione dei problemi relativi a polibase e Kerberos, vedere [risolvere i problemi di connettività Kerberos di base](polybase-troubleshoot-connectivity.md).

### <a name="internal-query-processor-error"></a>Errore interno di query processor

Se si esegue una query su una tabella esterna, viene restituito l'errore seguente:

> Messaggio 8680, livello 17, stato 5, riga 118<BR>
> Errore interno di Query Processor: errore imprevisto durante l'elaborazione di una fase di query remota.

Il log del server DWEngine contiene i messaggi seguenti:

> [Thread: 5216] [ControlNodeMessenger: ErrorEvent] (Errore, alto): * * * * * il sistema DMS ha nodi disconnessi:<BR>
> [Thread: 5216] [ControlNodeMessenger: ErrorEvent] (Errore, alto): * * * * * il sistema DMS ha nodi disconnessi:<BR>
> [Thread: 5216] [ControlNodeMessenger: ErrorEvent] (Errore, alto): * * * * * il sistema DMS ha nodi disconnessi:<BR>

**Motivo possibile:**  
Il motivo di questo errore potrebbe essere che SQL Server non è stato riavviato dopo la configurazione di base.

**Possibile soluzione:**  
Riavviare SQL Server. Controllare il log del server DWEngine per verificare che non siano presenti disconnessioni DMS dopo il riavvio.


### <a name="user-needed-for-hdfs-access"></a>Utente necessario per l'accesso a HDFS

**Scenario:**  
SQL Server è connesso a un cluster Hadoop non protetto (Kerberos non è abilitato). La polibase è configurata per eseguire il push del calcolo nel cluster Hadoop.

**Query di esempio:**  

```sql
select count(*) from foo WITH (FORCE EXTERNALPUSHDOWN);
```

Viene restituito un messaggio di errore simile al seguente: 

> Messaggio 105019, livello 16, stato 1, riga 1<BR>
> L'accesso alla tabella esterna non è riuscito a causa di un errore interno:' eccezione Java generata alla chiamata a JobSubmitter_PollJobStatus: errore [java. NET. ConnectException: chiamata da big1506sql2016/172.16.1.4 a 0.0.0.0:10020 non riuscita durante la connessione: java.net ConnectException: connessione rifiutata: non sono disponibili altre informazioni; Per altri dettagli, vedere:  http://wiki.apache.org/hadoop/ConnectionRefused ] durante l'accesso al file esterno.<BR>
> Il provider di OLE DB "SQLNCLI11" per il server collegato "(null)" ha restituito il messaggio "errore non specificato".<BR>
> Messaggio 7421, livello 16, stato 2, riga 1<BR>
> Impossibile recuperare il set di righe dal provider OLE DB "SQLNCLI11" per il server collegato "(null)". .<BR>

Errore di log Yarn Hadoop:
> Installazione del processo non riuscita: organizzazione. Apache. Hadoop. Security. AccessControlException: autorizzazione negata: utente = pdw_user, Access = WRITE, inode = "/User": HDFS: HDFS: drwxr-xr-x in org. Apache. Hadoop. HDFS. Server. NameNode. FSPermissionChecker. checkFsPermission (FSPermissionChecker. Java: 265) in org. Apache. Hadoop. HDFS. Server. NameNode. FSPermissionChecker. check (FSPermissionChecker. Java: 251) in org. Apache. Hadoop. HDFS. Server. NameNode. FSPermissionChecker. check (FSPermissionChecker. Java: 232) org. Apache. Hadoop. HDFS. Server. NameNode. FSPermissionChecker. checkPermission (FSPermissionChecker. Java: 176) in org. Apache. Hadoop. HDFS. Server. NameNode. FSNamesystem. checkPermission (FSNamesystem. Java: 5525)

**Motivo possibile:**  
Con Kerberos disabilitato, polibase utilizzerà pdw_user come utente per accedere a HDFS e inviare processi MapReduce.

**Possibile soluzione:**  
Creare pdw_user su Hadoop e assegnare loro autorizzazioni sufficienti per le directory usate durante l'elaborazione di MapReduce. Assicurarsi inoltre che pdw_user sia il proprietario della directory/User/pdw_user HDFS.

Di seguito è riportato un esempio di come creare la home directory e assegnare le autorizzazioni per pdw_user:

```console
sudo -u hdfs hadoop fs -mkdir /user/pdw_user
sudo -u hdfs hadoop fs -chown pdw_user /user/pdw_user
```

Al termine di questa operazione, assicurarsi che pdw_user disponga di autorizzazioni di lettura, scrittura ed esecuzione per la directory di pdw_user/User/. Verificare che la directory tmp disponga di 777 autorizzazioni.

Per ulteriori informazioni sulla risoluzione dei problemi relativi a polibase e Kerberos, vedere [risolvere i problemi di connettività Kerberos di base](polybase-troubleshoot-connectivity.md).

### <a name="java-memory-error-due-to-utf-8"></a>Errore di memoria Java dovuto a UTF-8

**Scenario:**  
SQL Server polibase è configurato con cluster Hadoop o archiviazione BLOB di Azure. Qualsiasi query SELECT ha esito negativo con l'errore seguente:

> Messaggio 106000, livello 16, stato 1, riga 1<BR>
> Spazio dell'heap di Java<BR>

**Motivo possibile:**  
L'input non valido può causare un errore di memoria insufficiente di Java. Il file non può essere in formato UTF-8. DMS tenta di leggere l'intero file come una riga poiché non può decodificare il delimitatore di riga e viene eseguito in un errore di spazio heap Java.

**Possibile soluzione:**  
Convertire il file in formato UTF-8 Poiché la polibase richiede attualmente il formato UTF-8 per i file delimitati da testo.

### <a name="hadoop-connectivity-configuration"></a>Configurazione della connettività di Hadoop

La configurazione di SQL Server polibase per la connessione all'archiviazione BLOB di Azure restituisce il seguente messaggio di errore in SQL Server:

> Messaggio 105019, livello 16, stato 1, riga 74<BR>
> L'accesso alla tabella esterna non è riuscito a causa di un errore interno:' eccezione Java generata alla chiamata a HdfsBridge_Connect: errore [nessun FileSystem per Scheme: wasbs] si è verificato durante l'accesso al file esterno '.<BR>

**Motivo possibile:**  
La connettività Hadoop non è impostata sul valore di configurazione per l'accesso all'archiviazione BLOB di Azure.

**Possibile soluzione:**  
Impostare la connettività Hadoop su un valore (preferibilmente 7) che supporta l'archiviazione BLOB di Azure e riavviare SQL Server. Per un elenco di valori di connettività e di tipi supportati, vedere [configurazione della connettività di base](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md#arguments).


### <a name="create-table-as-select-error"></a>Errore Create Table As Select

**Scenario:**  
Il tentativo di esportare i dati nell'archivio BLOB di Azure o in Hadoop file system usando la sintassi di base di CREATE EXTERNAL TABLE AS SELECT (CETAS) da SQL Server ha esito negativo con il messaggio di errore seguente:

> Messaggio 156, livello 15, stato 1, riga 177<BR>
> Sintassi non corretta in prossimità della parola chiave ' WITH '.<BR>
> Messaggio 319, livello 15, stato 1, riga 177<BR>
> Sintassi non corretta in prossimità della parola chiave 'with'. Se l'istruzione è un'espressione di tabella comune, una clausola xmlnamespaces o una clausola context per il rilevamento delle modifiche, l'istruzione precedente deve terminare con un punto e virgola (;).<BR>

**Motivo possibile:**  
Quando si esportano dati in Hadoop o in Archiviazione BLOB di Azure tramite PolyBase, vengono esportati solo i dati e non i nomi di colonne (metadati) definiti nel comando CREATE EXTERNAL TABLE.

**Possibile soluzione:**  
Creare innanzitutto la tabella esterna e quindi usare l'istruzione INSERT INTO SELECT per esportare il percorso esterno. Per un esempio di codice, vedere la pagina relativa agli [scenari di query di base](polybase-queries.md#export-data).


### <a name="create-external-table-from-azure-blob-storage-fails"></a>La creazione di una tabella esterna dall'archiviazione BLOB di Azure non riesce

**Scenario:**  
SQL DW è configurato per l'importazione di dati dall'archiviazione BLOB di Azure. La creazione della tabella esterna non riesce con il messaggio seguente.

> Messaggio 105019, livello 16, stato 1, riga 34<BR>
> L'accesso alla tabella esterna non è riuscito a causa di un errore interno:' eccezione Java generata alla chiamata a HdfsBridge_IsDirExist. Messaggio di eccezione Java: com. Microsoft. Azure. storage. StorageException: il server non è stato in grado di autenticare la richiesta. Verificare che il valore dell'intestazione di autorizzazione sia formato correttamente, inclusa la firma.: errore [com. Microsoft. Azure. storage. StorageException: il server non è riuscito ad autenticare la richiesta. Verificare che il valore dell'intestazione di autorizzazione sia formato correttamente, inclusa la firma.] si è verificato durante l'accesso al file esterno.<BR>

**Motivo possibile:**  
Chiave di archiviazione di Azure errata utilizzata per creare le credenziali con ambito database.

**Possibile soluzione:**  
Eliminare tutti gli oggetti correlati, ad esempio l'origine dati, il formato di file, quindi eliminare e ricreare le credenziali con ambito database con la chiave di archiviazione corretta.


### <a name="kerberos-configuration-capitalization"></a>Capitalizzazione della configurazione Kerberos

**Scenario:**  
SQL Server viene configurato con il cluster Cloudera abilitato per Kerberos. SQL Server è stato riavviato dopo tutte le modifiche alla configurazione. Il motore di base e i servizi PolyBase Data Movement sono in esecuzione dopo il riavvio. Vengono restituiti i messaggi di errore seguenti:

Origine dati configurata senza percorso del rilevamento processi:  
> org. Apache. Hadoop. FS. FileSystem: non è stato possibile creare un'istanza del provider org. Apache. Hadoop. FS. viewfs. ViewFileSystem

Origine dati configurata con percorso del rilevamento processi:  
> Errore [non è possibile ottenere l'area di autenticazione Kerberos] durante l'accesso al file esterno

**Motivo possibile:**  
Il valore della proprietà "Hadoop. Security. Authentication" indica Kerberos nel Coresite.xml.

**Possibile soluzione:**  
Il valore della proprietà "Hadoop. Security. Authentication" di Coresite.xml deve essere KERBEROS (all maiuscole). 

Per ulteriori informazioni sulla risoluzione dei problemi relativi a polibase e Kerberos, vedere [risolvere i problemi di connettività Kerberos di base](polybase-troubleshoot-connectivity.md).

### <a name="mapred-sitexml-missing-needed-values"></a>Mapred-site.xml i valori necessari mancanti

**Scenario:**  
SQL Server o APS è configurato con il cluster HDP supportato. Le query che non richiedono il funzionamento di distribuzione, ma hanno esito negativo con il messaggio seguente quando viene usato l'hint ' FORCE distribuzione ' con i messaggi di errore seguenti:

> Messaggio 7320, livello 16, stato 110, riga 35<BR>
> Impossibile eseguire la query "remote query" sul provider OLE DB "SQLNCLI11" per il server collegato "(null)". L'accesso alla tabella esterna non è riuscito a causa di un errore interno:' eccezione Java generata alla chiamata a JobSubmitter_PollJobStatus: errore [org. Apache. Hadoop. IPC. RemoteException (Java. lang. NullPointerException): Java. lang. NullPointerException<BR>
> in org. Apache. Hadoop. MapReduce. v2. HS. HistoryClientService $ HSClientProtocolHandler. getTaskAttemptCompletionEvents (HistoryClientService. Java: 277)<BR>
> in org. Apache. Hadoop. MapReduce. v2. API. impl. PB. Service. MRClientProtocolPBServiceImpl. getTaskAttemptCompletionEvents (MRClientProtocolPBServiceImpl. Java: 173)<BR>
> all'indirizzo org. Apache. Hadoop. Yarn. proto. MRClientProtocol $ MRClientProtocolService $2. callBlockingMethod (MRClientProtocol. Java: 283)<BR>
> in org. Apache. Hadoop. IPC. ProtobufRpcEngine $ server $ ProtoBufRpcInvoker. Call (ProtobufRpcEngine. Java: 619)<BR>
> in org. Apache. Hadoop. IPC. RPC $ server. Call (RPC. Java: 962)<BR>
> in org. Apache. Hadoop. IPC. Server $ Handler $1. Run (server. Java: 2127)<BR>
> in org. Apache. Hadoop. IPC. Server $ Handler $1. Run (server. Java: 2123)<BR>
> in Java. Security. AccessController. doPrivileged (metodo nativo)<BR>
> in javax. Security. auth. Subject. doAs (Subject. Java: 415)<BR>
> in org. Apache. Hadoop. Security. UserGroupInformation. doAs (UserGroupInformation. Java: 1671)<BR>
> in org. Apache. Hadoop. IPC. Server $ Handler. Run (server. Java: 2121)<BR>
> si è verificato un errore durante l'accesso al file esterno.<BR>

**Motivo possibile:**  
In Mapred-site.xml mancano alcuni valori necessari che controllano i risultati intermedi e finali.

**Possibile soluzione:**  
Aggiungere le proprietà seguenti e associare i valori corretti come visualizzato in Ambari nel file di mapred-site.xml SQL Server. 

```xml
<property>
<name>yarn.app.mapreduce.am.staging-dir</name>
<value>/user</value>
</property>
<property>
<name>mapreduce.jobhistory.done-dir</name>
<value>/mr-history/done</value>
</property>
<property>
<name>mapreduce.jobhistory.intermediate-done-dir</name>
<value>/mr-history/tmp</value>
</property>
```

### <a name="configuring-access-by-hostname"></a>Configurazione dell'accesso per nome host 

**Scenario:**  
SQL Server viene configurato per accedere a un cluster Hadoop supportato. La creazione di una tabella esterna restituisce uno degli errori seguenti:

> Impossibile eseguire la query "remote query" sul provider OLE DB "SQLNCLI11" per il server collegato "(null)". 110802; Si è verificato un errore DMS interno che ha causato l'esito negativo dell'operazione. Dettagli: eccezione: Microsoft. SqlServer. DataWarehouse. datamovement. Workers. DmsSqlNativeException, Message: SqlNativeBufferReader. Run, Error in OdbcExecuteQuery: SqlState: 42000, NativeError: 8680,' Error calling: SQLExecDirect (this->GetHstmt (), (SQLWCHAR *) statementText, SQL_NTS), codice SQL return:-1 | Informazioni sull'errore SQL: SrvrMsgState: 26, SrvrSeverity: 17, errore <1>: ErrorMsg: [Microsoft] [ODBC driver 13 for SQL Server] [SQL Server] errore interno di query processor: si è verificato un errore imprevisto durante l'elaborazione di una fase di query remota. | Errore durante la chiamata a: pReadConn->ExecuteQuery (statementText, bufferFormat) | stato: FFFF, numero: 24, connessioni attive: 8', stringa di connessione: driver = {pdwodbc}; APP = RCSmall-DmsNativeReader: WAD1D16HD2001\mpdwsvc (3600)-ODBC-PoolId1433; Trusted_Connection = Sì; AutoTranslate = no; Server = \\ .\pipe\sql\query<BR>

> [Thread: 30544] [AbstractReaderWorker: ErrorEvent] (Errore, alto): QueryId QID2433 PlanId 6c3a4551-e54c-4c06-a5ed-a8733edac691 StepId 7:<BR>
> Non è stato possibile ottenere il blocco: BP-1726738607-192.168.225.121-1443123675290: file blk_1159687047_86196509 =/User/hive/warehouse/u_data/000000_0<BR>
> Microsoft. SqlServer. DataWarehouse. Common. ErrorHandling. MppSqlException: non è stato possibile ottenere il blocco: BP-1726738607-192.168.225.121-1443123675290: file blk_1159687047_86196509 =/User/hive/warehouse/u_data/000000_0<BR>
> in Microsoft. SqlServer. Data warehouse. datamovement. Common. ExternalAccess. HdfsBridgeReadAccess. Read (buffer MemoryBuffer, valore booleano&)<BR>
> in Microsoft. SqlServer. DataWarehouse. datamovement. Workers. DataReader. ExternalMoveBufferReader. Read ()<BR>
> in Microsoft. SqlServer. DataWarehouse. datamovement. Workers. ExternalMoveReaderWorker. ReadAndSendData ()<BR>
> al Microsoft.SqlServer.DataWarehouse.DataMovement.Workers.ExternalMoveReaderWorker.Execarino (stato oggetto)<BR>

**Motivo possibile:**  
Questo messaggio di errore può essere visualizzato quando il cluster Hadoop viene configurato in una configurazione in cui i nodi dati sono accessibili solo all'esterno del cluster usando il nome host e non l'indirizzo IP.

**Possibile soluzione:**  
Aggiungere quanto segue a hdfs-site.xml file sul lato client (SQL Server). Questa configurazione forza il nodo nome a restituire un URI per i nodi dati con il nome host anziché l'indirizzo IP interno.

```xml
<property>
<name>dfs.client.use.datanode.hostname</name>
<value>true</value>
</property>
```

### <a name="folder-organization-forces-excess-memory-overhead"></a>L'organizzazione di cartelle impone un sovraccarico eccessivo della memoria

**Scenario:**  
SQL Server esegue una query di base su una directory con un numero elevato di file (>30.000 file nel percorso della directory in modo ricorsivo) e viene restituito uno dei messaggi di errore seguenti:

> Messaggio 105019, livello 16, stato 1, riga 1<BR>
> L'accesso alla tabella esterna non è riuscito a causa di un errore interno:' eccezione Java generata alla chiamata a HdfsBridge_GetFileNameByIndex. Messaggio di eccezione Java: limite sovraccarico GC superato: errore [limite sovraccarico GC superato] durante l'accesso al file esterno.<BR>

> Messaggio 105019, livello 16, stato 1, riga 1<BR>
> L'accesso alla tabella esterna non è riuscito a causa di un errore interno:' eccezione Java generata alla chiamata a HdfsBridge_GetDirectoryFiles. Messaggio eccezione Java: spazio heap Java: errore [spazio heap Java] durante l'accesso al file esterno.

**Motivo possibile:**  
Quando si elabora un percorso, polibase enumera tutti i file in tale percorso e si verifica un sovraccarico di memoria fisso associato alla struttura dei dati utilizzata per rappresentare i file. Con un numero elevato di file, questo sovraccarico diventa evidente e può consumare tutta la memoria disponibile per JVM.

**Possibile soluzione:**  
Ridisporre i dati in più directory in modo che ogni directory contenga un subset di file e quindi suddividere la query in più directory che operano su una parte del percorso originale alla volta e materializzare le tabelle come SQL Server tabelle (prima di aggiungerle).

Esempio: si supponga che i dati della tabella esterna si trovino nel percorso seguente: Orders/file1.txt,..., file30K.txt.

Modificare il layout in modo che i dati siano disposti in una struttura di partizione di file convenzionale in Orders/*aaaa* / *mm* / *GG*/file1.txt.
Puntare la tabella esterna a un percorso di directory inferiore, ad esempio mese (mm) o giorno (DD), e importare i file in SQL Server tabelle in parti, quindi aggiungerli come parte di una tabella.
Anche se è stata avviata la struttura di directory corretta, seguire i passaggi #2 per poter usare molti file senza esaurire la memoria JVM.


### <a name="unexpected-characters-in-configuration-files"></a>Caratteri imprevisti nei file di configurazione

**Scenario:**  
Configurazione di SQL Server o APS con un cluster Hadoop che comporta la modifica di yarn-site.xml, hdfs-site.xml e altri file di configurazione. Viene osservato il seguente messaggio di errore SQL Server:

> Messaggio 105019, livello 16, stato 1, riga 1<BR>
> Microsoft. SqlServer. DataWarehouse. Common. ErrorHandling. MppSqlException: l'accesso alla tabella esterna non è riuscito a causa di un errore interno:' eccezione Java generata alla chiamata a HdfsBridge_Connect. Messaggio di eccezione Java:com. Sun. org. Apache. xerces. Internal. impl. io. MalformedByteSequenceException: byte 1 della sequenza UTF-8 a 1 byte non valido.: errore [com. Sun. org. Apache. xerces. Internal. impl. io. MalformedByteSequenceException: byte 1 della sequenza UTF-8 a 1 byte non valido.] durante l'accesso al file esterno. ---><BR>

**Motivo possibile:**  
Questo problema può verificarsi se è stato copiato e incollato testo in file di configurazione da un sito Web o da una finestra di chat. È possibile che nei file di configurazione siano presenti caratteri indesiderati o non stampabili.

**Possibile soluzione:**  
Aprire i file in un editor di testo diverso (ad eccezione del blocco note) e cercare questi caratteri ed eliminarli. Riavviare i servizi necessari. 


## <a name="see-also"></a>Vedi anche

[Monitorare e risolvere i problemi relativi a PolyBase](polybase-troubleshooting.md)  
[Risolvere i problemi di connettività di PolyBase Kerberos](polybase-troubleshoot-connectivity.md)  

