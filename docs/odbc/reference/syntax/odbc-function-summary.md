---
description: Riepilogo delle funzioni ODBC
title: Riepilogo delle funzioni ODBC | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- functions [ODBC], listed by task
ms.assetid: 7aa635da-e6b7-439f-8e9b-c3860e24de5e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d5d27a6093947ba4e29e43f4970848780cbf4e8e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174651"
---
# <a name="odbc-function-summary"></a>Riepilogo delle funzioni ODBC
Nella tabella seguente sono elencate le funzioni ODBC raggruppate in base al tipo di attività e viene inclusa la designazione della conformità e una breve descrizione dello scopo di ogni funzione. Per altre informazioni sulle designazioni di conformità, vedere [ODBC e l'interfaccia della](../../../odbc/reference/odbc-and-the-standard-cli.md)riga di comando standard. Per ulteriori informazioni sulla sintassi e la semantica per ogni funzione, vedere [ODBC API Reference](../../../odbc/reference/syntax/odbc-api-reference.md).  
  
 Un'applicazione può chiamare la funzione **SQLGetInfo** per ottenere informazioni sulla conformità di un driver. Per ottenere informazioni sul supporto di una funzione specifica in un driver, un'applicazione può chiamare **SQLGetFunctions**.  
  
|Attività|Nome della funzione|Conformità|Scopo|  
|----------|-------------------|-----------------|-------------|  
|Connessione a un'origine dati|[SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|ISO 92|Ottiene un handle di ambiente, connessione, istruzione o descrittore.|  
||[SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)|ISO 92|Consente di connettersi a un driver specifico per nome dell'origine dati, ID utente e password.|  
||[SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)|ODBC|Stabilisce la connessione a un driver specifico per stringa di connessione o richiede che le finestre di dialogo per l'utente siano visualizzate da Gestione driver e dal driver.|  
||[SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)|ODBC|Restituisce i livelli successivi degli attributi di connessione e i valori di attributo validi. Quando si specifica un valore per ogni attributo di connessione, si connette all'origine dati.|  
|Recupero di informazioni su un driver e un'origine dati|[SQLDataSources](../../../odbc/reference/syntax/sqldatasources-function.md)<br /><br /> [SQLDrivers](../../../odbc/reference/syntax/sqldrivers-function.md)|ISO 92<br /><br /> ODBC|Restituisce l'elenco delle origini dati disponibili.<br /><br /> Restituisce l'elenco dei driver installati e i relativi attributi.|  
||[SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md)|ISO 92|Restituisce informazioni su un driver e un'origine dati specifici.|  
||[SQLGetFunctions](../../../odbc/reference/syntax/sqlgetfunctions-function.md)|ISO 92|Restituisce le funzioni driver supportate.|  
||[SQLGetTypeInfo](../../../odbc/reference/syntax/sqlgettypeinfo-function.md)|ISO 92|Restituisce informazioni sui tipi di dati supportati.|  
|Impostazione e recupero degli attributi del driver|[SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)<br /><br /> [SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|ISO 92<br /><br /> ISO 92|Imposta un attributo di connessione.<br /><br /> Restituisce il valore di un attributo di connessione.|  
||[SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md)|ISO 92|Imposta un attributo dell'ambiente.|  
||[SQLGetEnvAttr](../../../odbc/reference/syntax/sqlgetenvattr-function.md)|ISO 92|Restituisce il valore di un attributo dell'ambiente.|  
||[SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|ISO 92|Imposta un attributo di istruzione.|  
||[SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)|ISO 92|Restituisce il valore di un attributo di istruzione.|  
|Impostazione e recupero dei campi del descrittore|[SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md)<br /><br /> [SQLGetDescRec](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|ISO 92<br /><br /> ISO 92|Restituisce il valore di un singolo campo del descrittore.<br /><br /> Restituisce i valori di più campi del descrittore.|  
||[SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|ISO 92|Imposta un singolo campo del descrittore.|  
||[SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|ISO 92|Imposta più campi del descrittore.|  
||[SQLCopyDesc](../../../odbc/reference/syntax/sqlcopydesc-function.md)|ISO 92|Copia le informazioni sul descrittore da un handle descrittore a un altro.|  
|Preparazione delle richieste SQL|[SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|ISO 92|Prepara un'istruzione SQL per l'esecuzione successiva.|  
||[SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|ODBC|Assegna lo spazio di archiviazione per un parametro in un'istruzione SQL.|  
||[SQLGetCursorName](../../../odbc/reference/syntax/sqlgetcursorname-function.md)|ISO 92|Restituisce il nome del cursore associato a un handle di istruzione.|  
||[SQLSetCursorName](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|ISO 92|Specifica il nome di un cursore.|  
||[SQLSetScrollOptions](../../../odbc/reference/syntax/sqlsetscrolloptions-function.md)|ODBC|Imposta le opzioni che controllano il comportamento del cursore.|  
|Invio di richieste|[SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)<br /><br /> [SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|ISO 92<br /><br /> ISO 92|Esegue un'istruzione preparata.<br /><br /> Esegue un'istruzione.|  
||[SQLNativeSql](../../../odbc/reference/syntax/sqlnativesql-function.md)|ODBC|Restituisce il testo di un'istruzione SQL convertito dal driver.|  
||[SQLDescribeParam](../../../odbc/reference/syntax/sqldescribeparam-function.md)|ODBC|Restituisce la descrizione di un parametro specifico in un'istruzione.|  
||[SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md)|ISO 92|Restituisce il numero di parametri in un'istruzione.|  
||[SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md)|ISO 92|Utilizzato insieme a **SQLPutData** per fornire i dati dei parametri in fase di esecuzione. (Utile per i valori di dati Long).|  
||[SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)|ISO 92|Invia parte o tutto un valore di dati per un parametro. (Utile per i valori di dati Long).|  
|Recupero di risultati e informazioni sui risultati|[SQLRowCount](../../../odbc/reference/syntax/sqlrowcount-function.md)<br /><br /> [SQLNumResultCols](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|ISO 92<br /><br /> ISO 92|Restituisce il numero di righe interessate da una richiesta di inserimento, aggiornamento o eliminazione.<br /><br /> Restituisce il numero di colonne nel set di risultati.|  
||[SQLDescribeCol](../../../odbc/reference/syntax/sqldescribecol-function.md)|ISO 92|Descrive una colonna nel set di risultati.|  
||[SQLColAttribute](../../../odbc/reference/syntax/sqlcolattribute-function.md)|ISO 92|Descrive gli attributi di una colonna nel set di risultati.|  
||[SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|ISO 92|Assegna lo spazio di archiviazione per una colonna risultato e specifica il tipo di dati.|  
||[SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|ISO 92|Restituisce più righe di risultati.|  
||[SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|ISO 92|Restituisce le righe di risultati scorrevoli.|  
||[SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)|ISO 92|Restituisce parte o tutte le colonne di una riga di un set di risultati. (Utile per i valori di dati Long).|  
||[SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)|ODBC|Posiziona un cursore all'interno di un blocco di dati recuperato e consente a un'applicazione di aggiornare i dati nel set di righe o di aggiornare o eliminare i dati nel set di risultati.|  
||[SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|ODBC|Esegue inserimenti bulk e operazioni di segnalibro bulk, tra cui l'aggiornamento, l'eliminazione e il recupero tramite segnalibro.|  
||[SQLMoreResults](../../../odbc/reference/syntax/sqlmoreresults-function.md)|ODBC|Determina se sono disponibili più set di risultati e, in caso affermativo, Inizializza l'elaborazione per il set di risultati successivo.|  
||[SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)|ISO 92|Restituisce informazioni di diagnostica aggiuntive (un singolo campo della struttura dei dati di diagnostica).|  
||[SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md)|ISO 92|Restituisce informazioni di diagnostica aggiuntive (più campi della struttura dei dati di diagnostica).|  
|Recupero di informazioni sulle tabelle di sistema dell'origine dati (funzioni del catalogo)|[SQLColumnPrivileges](../../../odbc/reference/syntax/sqlcolumnprivileges-function.md)<br /><br /> [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md)|ODBC<br /><br /> Apri gruppo|Restituisce un elenco di colonne e i privilegi associati per una o più tabelle.<br /><br /> Restituisce l'elenco dei nomi di colonna nelle tabelle specificate.|  
||[SQLForeignKeys](../../../odbc/reference/syntax/sqlforeignkeys-function.md)|ODBC|Restituisce un elenco di nomi di colonna che costituiscono chiavi esterne, se esistenti per una tabella specificata.|  
||[SQLPrimaryKeys](../../../odbc/reference/syntax/sqlprimarykeys-function.md)|ODBC|Restituisce l'elenco dei nomi di colonna che costituiscono la chiave primaria per una tabella.|  
||[SQLProcedureColumns](../../../odbc/reference/syntax/sqlprocedurecolumns-function.md)|ODBC|Restituisce l'elenco dei parametri di input e output, nonché le colonne che costituiscono il set di risultati per le procedure specificate.|  
||[SQLProcedures](../../../odbc/reference/syntax/sqlprocedures-function.md)|ODBC|Restituisce l'elenco di nomi di stored procedure archiviati in un'origine dati specifica.|  
||[SQLSpecialColumns](../../../odbc/reference/syntax/sqlspecialcolumns-function.md)|Apri gruppo|Restituisce informazioni sul set ottimale di colonne che identifica in modo univoco una riga in una tabella specificata o le colonne che vengono aggiornate automaticamente quando un valore della riga viene aggiornato da una transazione.|  
||[SQLStatistics](../../../odbc/reference/syntax/sqlstatistics-function.md)|ISO 92|Restituisce le statistiche relative a una singola tabella e l'elenco degli indici associati alla tabella.|  
||[SQLTablePrivileges](../../../odbc/reference/syntax/sqltableprivileges-function.md)|ODBC|Restituisce un elenco di tabelle e i privilegi associati a ogni tabella.|  
||[SQLTables](../../../odbc/reference/syntax/sqltables-function.md)|Apri gruppo|Restituisce l'elenco dei nomi di tabella archiviati in un'origine dati specifica.|  
|Terminazione di un'istruzione|[SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md)|ISO 92|Termina l'elaborazione dell'istruzione, Elimina i risultati in sospeso e, facoltativamente, libera tutte le risorse associate all'handle di istruzione.|  
||[SQLCloseCursor](../../../odbc/reference/syntax/sqlclosecursor-function.md)|ISO 92|Chiude un cursore aperto in un handle di istruzione.|  
||[SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|ISO 92|Annulla l'elaborazione di un'istruzione.|  
||[SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md)|ODBC|Annulla l'elaborazione in un'istruzione o una connessione.|  
||[SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md)|ISO 92|Esegue il commit o il rollback di una transazione.|  
|Interruzione di una connessione|[SQLDisconnect](../../../odbc/reference/syntax/sqldisconnect-function.md)<br /><br /> [SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md)|ISO 92<br /><br /> ISO 92|Chiude la connessione.<br /><br /> Rilascia un handle di ambiente, connessione, istruzione o descrittore.|
