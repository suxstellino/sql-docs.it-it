---
description: Membri di SQLServerDatabaseMetaData
title: SQLServerDatabaseMetaData Members | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 327ba0bc-438a-494c-b119-1cd4a096bb58
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4fe11f6a3377d0a26941138b8a581f642a6b0755
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172788"
---
# <a name="sqlserverdatabasemetadata-members"></a>Membri di SQLServerDatabaseMetaData
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Nelle tabelle seguenti sono elencati i membri esposti dalla classe [SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md).  
  
## <a name="constructors"></a>Costruttori  
 Nessuno.  
  
## <a name="fields"></a>Campi  
 No.  
  
## <a name="inherited-fields"></a>Campi ereditati  
  
|Nome|Descrizione|  
|----------|-----------------|  
|java.sql.DatabaseMetaData|attributeNoNulls, attributeNullable, attributeNullableUnknown, bestRowNotPseudo, bestRowPseudo, bestRowSession, bestRowTemporary, bestRowTransaction, bestRowUnknown, columnNoNulls, columnNullable, columnNullableUnknown, importedKeyCascade, importedKeyInitiallyDeferred, importedKeyInitiallyImmediate, importedKeyNoAction, importedKeyNotDeferrable, importedKeyRestrict, importedKeySetDefault, importedKeySetNull, procedureColumnIn, procedureColumnInOut, procedureColumnOut, procedureColumnResult, procedureColumnReturn, procedureColumnUnknown, procedureNoNulls, procedureNoResult, procedureNullable, procedureNullableUnknown, procedureResultUnknown, procedureReturnsResult, sqlStateSQL, sqlStateSQL99, sqlStateXOpen, tableIndexClustered, tableIndexHashed, tableIndexOther, tableIndexStatistic, typeNoNulls, typeNullable, typeNullableUnknown, typePredBasic, typePredChar, typePredNone, typeSearchable, versionColumnNotPseudo, versionColumnPseudo, versionColumnUnknown|  
  
## <a name="methods"></a>Metodi  
  
|Nome|Descrizione|  
|----------|-----------------|  
|[allProceduresAreCallable](../../../connect/jdbc/reference/allproceduresarecallable-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se l'utente corrente ha le autorizzazioni necessarie per chiamare tutte le procedure restituite dal metodo [getProcedures](../../../connect/jdbc/reference/getprocedures-method-sqlserverdatabasemetadata.md).|  
|[allTablesAreSelectable](../../../connect/jdbc/reference/alltablesareselectable-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se l'utente corrente ha le autorizzazioni necessarie per usare tutte le tabelle restituite dal metodo [getTables](../../../connect/jdbc/reference/gettables-method-sqlserverdatabasemetadata.md) in un'istruzione SELECT.|  
|[autoCommitFailureClosesAllResultSets](../../../connect/jdbc/reference/autocommitfailureclosesallresultsets-method-sqlserverdatabasemetadata.md)|Indica se tramite il driver JDBC vengono chiusi tutti i set di risultati aperti, inclusi quelli trattenibili, quando è abilitata una modalità di commit automatico e viene generata un'eccezione.|  
|[dataDefinitionCausesTransactionCommit](../../../connect/jdbc/reference/datadefinitioncausestransactioncommit-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se un'istruzione di definizione dei dati all'interno di una transazione forza il commit della transazione.|  
|[dataDefinitionIgnoredInTransactions](../../../connect/jdbc/reference/datadefinitionignoredintransactions-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database ignora un'istruzione di definizione dei dati all'interno di una transazione.|  
|[deletesAreDetected](../../../connect/jdbc/reference/deletesaredetected-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se è possibile rilevare una riga visibile eliminata chiamando il metodo [rowDeleted](../../../connect/jdbc/reference/rowdeleted-method-sqlserverresultset.md) della classe [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[doesMaxRowSizeIncludeBlobs](../../../connect/jdbc/reference/doesmaxrowsizeincludeblobs-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il valore restituito per il metodo [getMaxRowSize](../../../connect/jdbc/reference/getmaxrowsize-method-sqlserverdatabasemetadata.md) include i tipi di dati SQL LONGVARCHAR e LONGVARBINARY.|  
|[getAttributes](../../../connect/jdbc/reference/getattributes-method-sqlserverdatabasemetadata.md)|Recupera una descrizione di un determinato attributo del tipo indicato per un tipo definito dall'utente disponibile nello schema e nel catalogo specificati.|  
|[getBestRowIdentifier](../../../connect/jdbc/reference/getbestrowidentifier-method-sqlserverdatabasemetadata.md)|Recupera una descrizione del set ottimale di colonne di una tabella che identifica una riga in modo univoco.|  
|[getCatalogs](../../../connect/jdbc/reference/getcatalogs-method-sqlserverdatabasemetadata.md)|Recupera i nomi di catalogo disponibili nel server connesso.|  
|[getCatalogSeparator](../../../connect/jdbc/reference/getcatalogseparator-method-sqlserverdatabasemetadata.md)|Recupera il valore **String** usato dal database come separatore tra un nome di catalogo e un nome di tabella.|  
|[getCatalogTerm](../../../connect/jdbc/reference/getcatalogterm-method-sqlserverdatabasemetadata.md)|Recupera il termine preferito del fornitore del database per indicare "catalogo".|  
|[getClientInfoProperties](../../../connect/jdbc/reference/getclientinfoproperties-method-sqlserverdatabasemetadata.md)|Recupera un elenco di proprietà delle informazioni client supportate dal driver.|  
|[getColumnPrivileges](../../../connect/jdbc/reference/getcolumnprivileges-method-sqlserverdatabasemetadata.md)|Recupera una descrizione dei diritti di accesso per le colonne di una tabella.|  
|[getColumns](../../../connect/jdbc/reference/getcolumns-method-sqlserverdatabasemetadata.md)|Recupera una descrizione delle colonne di una tabella disponibili nel catalogo specificato.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverdatabasemetadata.md)|Recupera la connessione che ha prodotto questo oggetto di metadati.|  
|[getCrossReference](../../../connect/jdbc/reference/getcrossreference-method-sqlserverdatabasemetadata.md)|Recupera una descrizione delle colonne di chiave esterna per la tabella di chiave esterna specificata che fa riferimento alle colonne di chiave primaria della tabella di chiave primaria specificata. |  
|[getDatabaseMajorVersion](../../../connect/jdbc/reference/getdatabasemajorversion-method-sqlserverdatabasemetadata.md)|Recupera il numero della versione principale del database sottostante.|  
|[getDatabaseMinorVersion](../../../connect/jdbc/reference/getdatabaseminorversion-method-sqlserverdatabasemetadata.md)|Recupera il numero della versione secondaria del database sottostante.|  
|[getDatabaseProductName](../../../connect/jdbc/reference/getdatabaseproductname-method-sqlserverdatabasemetadata.md)|Recupera il nome di questo prodotto di database.|  
|[getDatabaseProductVersion](../../../connect/jdbc/reference/getdatabaseproductversion-method-sqlserverdatabasemetadata.md)|Recupera il numero di versione di questo prodotto di database.|  
|[getDefaultTransactionIsolation](../../../connect/jdbc/reference/getdefaulttransactionisolation-method-sqlserverdatabasemetadata.md)|Recupera il livello di isolamento della transazione predefinito per il database.|  
|[getDriverMajorVersion](../../../connect/jdbc/reference/getdrivermajorversion-method-sqlserverdatabasemetadata.md)|Recupera il numero della versione principale di questo driver JDBC.|  
|[getDriverMinorVersion](../../../connect/jdbc/reference/getdriverminorversion-method-sqlserverdatabasemetadata.md)|Recupera il numero della versione secondaria di questo driver JDBC.|  
|[getDriverName](../../../connect/jdbc/reference/getdrivername-method-sqlserverdatabasemetadata.md)|Recupera il nome del driver JDBC.|  
|[getDriverVersion](../../../connect/jdbc/reference/getdriverversion-method-sqlserverdatabasemetadata.md)|Recupera il numero di versione di questo driver JDBC.|  
|[getExportedKeys](../../../connect/jdbc/reference/getexportedkeys-method-sqlserverdatabasemetadata.md)|Recupera una descrizione delle colonne di chiave esterna che fanno riferimento alle colonne di chiave primaria della tabella specificata.|  
|[getExtraNameCharacters](../../../connect/jdbc/reference/getextranamecharacters-method-sqlserverdatabasemetadata.md)|Recupera tutti i caratteri aggiuntivi che possono essere utilizzati nei nomi di identificatore non racchiusi tra virgolette, ad esempio quelli non inclusi in a-z, A-Z, 0-9 e _.|  
|[getFunctions](../../../connect/jdbc/reference/getfunctions-method-sqlserverdatabasemetadata.md)|Recupera una descrizione delle funzioni utente e di sistema.|  
|[getFunctionColumns](../../../connect/jdbc/reference/getfunctioncolumns-method-sqlserverdatabasemetadata.md)|Recupera una descrizione dei parametri e del tipo restituito delle funzioni utente o di sistema del catalogo specificato.|  
|[getIdentifierQuoteString](../../../connect/jdbc/reference/getidentifierquotestring-method-sqlserverdatabasemetadata.md)|Recupera il valore **String** usato per racchiudere tra virgolette gli identificatori SQL.|  
|[getImportedKeys](../../../connect/jdbc/reference/getimportedkeys-method-sqlserverdatabasemetadata.md)|Recupera una descrizione delle colonne di chiave primaria cui fanno riferimento le colonne di chiave esterna di una tabella.|  
|[getIndexInfo](../../../connect/jdbc/reference/getindexinfo-method-sqlserverdatabasemetadata.md)|Recupera una descrizione degli indici e delle statistiche della tabella specificata.|  
|[getJDBCMajorVersion](../../../connect/jdbc/reference/getjdbcmajorversion-method-sqlserverdatabasemetadata.md)|Recupera il numero della versione principale di JDBC per questo driver.|  
|[getJDBCMinorVersion](../../../connect/jdbc/reference/getjdbcminorversion-method-sqlserverdatabasemetadata.md)|Recupera il numero della versione secondaria di JDBC per questo driver.|  
|[getMaxBinaryLiteralLength](../../../connect/jdbc/reference/getmaxbinaryliterallength-method-sqlserverdatabasemetadata.md)|Recupera il numero massimo di caratteri esadecimali consentiti dal database in un valore letterale binario inline.|  
|[getMaxCatalogNameLength](../../../connect/jdbc/reference/getmaxcatalognamelength-method-sqlserverdatabasemetadata.md)|Recupera il numero massimo di caratteri consentiti dal database in un nome di catalogo.|  
|[getMaxCharLiteralLength](../../../connect/jdbc/reference/getmaxcharliterallength-method-sqlserverdatabasemetadata.md)|Recupera il numero massimo di caratteri consentito dal database in un valore letterale carattere.|  
|[getMaxColumnNameLength](../../../connect/jdbc/reference/getmaxcolumnnamelength-method-sqlserverdatabasemetadata.md)|Recupera il numero massimo di caratteri consentiti dal database in un nome di colonna.|  
|[getMaxColumnsInGroupBy](../../../connect/jdbc/reference/getmaxcolumnsingroupby-method-sqlserverdatabasemetadata.md)|Recupera il numero massimo di colonne consentite dal database in una clausola GROUP BY.|  
|[getMaxColumnsInIndex](../../../connect/jdbc/reference/getmaxcolumnsinindex-method-sqlserverdatabasemetadata.md)|Recupera il numero massimo di colonne consentite dal database in un indice.|  
|[getMaxColumnsInOrderBy](../../../connect/jdbc/reference/getmaxcolumnsinorderby-method-sqlserverdatabasemetadata.md)|Recupera il numero massimo di colonne consentite dal database in una clausola ORDER BY.|  
|[getMaxColumnsInSelect](../../../connect/jdbc/reference/getmaxcolumnsinselect-method-sqlserverdatabasemetadata.md)|Recupera il numero massimo di colonne consentite dal database in un elenco SELECT.|  
|[getMaxColumnsInTable](../../../connect/jdbc/reference/getmaxcolumnsintable-method-sqlserverdatabasemetadata.md)|Recupera il numero massimo di colonne consentite dal database in una tabella.|  
|[getMaxConnections](../../../connect/jdbc/reference/getmaxconnections-method-sqlserverdatabasemetadata.md)|Recupera il numero massimo consentito di connessioni simultanee al database.|  
|[getMaxCursorNameLength](../../../connect/jdbc/reference/getmaxcursornamelength-method-sqlserverdatabasemetadata.md)|Recupera il numero massimo di caratteri consentito dal database in un nome di cursore.|  
|[getMaxIndexLength](../../../connect/jdbc/reference/getmaxindexlength-method-sqlserverdatabasemetadata.md)|Recupera il numero massimo di byte consentito dal database in un indice, incluse tutte le parti che lo compongono.|  
|[getMaxProcedureNameLength](../../../connect/jdbc/reference/getmaxprocedurenamelength-method-sqlserverdatabasemetadata.md)|Recupera il numero massimo di caratteri consentiti dal database in un nome di procedura.|  
|[getMaxRowSize](../../../connect/jdbc/reference/getmaxrowsize-method-sqlserverdatabasemetadata.md)|Recupera il numero massimo di byte consentiti dal database in una singola riga.|  
|[getMaxSchemaNameLength](../../../connect/jdbc/reference/getmaxschemanamelength-method-sqlserverdatabasemetadata.md)|Recupera il numero massimo di caratteri consentiti dal database in un nome di schema.|  
|[getMaxStatementLength](../../../connect/jdbc/reference/getmaxstatementlength-method-sqlserverdatabasemetadata.md)|Recupera il numero massimo di caratteri consentiti dal database in un'istruzione SQL.|  
|[getMaxStatements](../../../connect/jdbc/reference/getmaxstatements-method-sqlserverdatabasemetadata.md)|Recupera il numero massimo di istruzioni attive che possono essere aperte contemporaneamente sul database.|  
|[getMaxTableNameLength](../../../connect/jdbc/reference/getmaxtablenamelength-method-sqlserverdatabasemetadata.md)|Recupera il numero massimo di caratteri consentito dal database in un nome di tabella.|  
|[getMaxTablesInSelect](../../../connect/jdbc/reference/getmaxtablesinselect-method-sqlserverdatabasemetadata.md)|Recupera il numero massimo di tabelle consentite dal database in un'istruzione SELECT.|  
|[getMaxUserNameLength](../../../connect/jdbc/reference/getmaxusernamelength-method-sqlserverdatabasemetadata.md)|Recupera il numero massimo di caratteri consentito dal database in un nome utente.|  
|[getNumericFunctions](../../../connect/jdbc/reference/getnumericfunctions-method-sqlserverdatabasemetadata.md)|Recupera un elenco delimitato da virgole delle funzioni matematiche disponibili con il database.|  
|[getPrimaryKeys](../../../connect/jdbc/reference/getprimarykeys-method-sqlserverdatabasemetadata.md)|Recupera una descrizione delle colonne di chiave primaria della tabella specificata.|  
|[getProcedureColumns](../../../connect/jdbc/reference/getprocedurecolumns-method-sqlserverdatabasemetadata.md)|Recupera una descrizione dei parametri delle stored procedure e delle colonne dei risultati.|  
|[getProcedures](../../../connect/jdbc/reference/getprocedures-method-sqlserverdatabasemetadata.md)|Recupera una descrizione delle stored procedure disponibili nel modello di nome di catalogo, di schema o di stored procedure specificato.|  
|[getProcedureTerm](../../../connect/jdbc/reference/getprocedureterm-method-sqlserverdatabasemetadata.md)|Recupera il termine preferito per indicare "procedura" nel database.|  
|[getResultSetHoldability](../../../connect/jdbc/reference/getresultsetholdability-method-sqlserverdatabasemetadata.md)|Recupera la trattenibilità predefinita dei set di risultati del database.|  
|[getRowIdLifetime](../../../connect/jdbc/reference/getrowidlifetime-method-sqlserverdatabasemetadata.md)|Restituisce uno stato che indica se è supportato il tipo di dati SQL RowId. Se il tipo è supportato, restituisce la durata di validità di un oggetto RowId.|  
|[getSchemas](../../../connect/jdbc/reference/getschemas-method.md)|Recupera i nomi di schema disponibili nel database corrente.|  
|[getSchemaTerm](../../../connect/jdbc/reference/getschematerm-method-sqlserverdatabasemetadata.md)|Recupera il termine preferito per indicare "schema" nel database.|  
|[getSearchStringEscape](../../../connect/jdbc/reference/getsearchstringescape-method-sqlserverdatabasemetadata.md)|Recupera il valore **String** che può essere usato come escape per i caratteri jolly.|  
|[getSQLKeywords](../../../connect/jdbc/reference/getsqlkeywords-method-sqlserverdatabasemetadata.md)|Recupera un elenco delimitato da virgole di tutte le parole chiave SQL del database che non sono anche parole chiave SQL92.|  
|[getSQLStateType](../../../connect/jdbc/reference/getsqlstatetype-method-sqlserverdatabasemetadata.md)|Indica se il valore SQLSTATE restituito dal metodo SQLException.getSQLState è X/Open (ora noto come Open Group), SQL CLI, SQL99 (JDBC 3.0) o SQL:2003 (JDBC 4.0).|  
|[getStringFunctions](../../../connect/jdbc/reference/getstringfunctions-method-sqlserverdatabasemetadata.md)|Recupera un elenco delimitato da virgole di funzioni **String** disponibili con il database.|  
|[getSuperTables](../../../connect/jdbc/reference/getsupertables-method-sqlserverdatabasemetadata.md)|Recupera una descrizione delle gerarchie di tabelle definite in un determinato schema del database.|  
|[getSuperTypes](../../../connect/jdbc/reference/getsupertypes-method-sqlserverdatabasemetadata.md)|Recupera una descrizione delle gerarchie di tipi definiti dall'utente configurate in un determinato schema del database.|  
|[getSystemFunctions](../../../connect/jdbc/reference/getsystemfunctions-method-sqlserverdatabasemetadata.md)|Recupera un elenco delimitato da virgole delle funzioni di sistema disponibili con il database.|  
|[getTablePrivileges](../../../connect/jdbc/reference/gettableprivileges-method-sqlserverdatabasemetadata.md)|Recupera una descrizione dei diritti di accesso di ogni tabella disponibile nel modello di nome di catalogo, di schema o di tabella specificato.|  
|[getTables](../../../connect/jdbc/reference/gettables-method-sqlserverdatabasemetadata.md)|Recupera una descrizione delle tabelle disponibili nel modello di nome di catalogo, di schema o di tabella specificato.|  
|[getTableTypes](../../../connect/jdbc/reference/gettabletypes-method-sqlserverdatabasemetadata.md)|Recupera i tipi di tabella disponibili nel database corrente.|  
|[getTimeDateFunctions](../../../connect/jdbc/reference/gettimedatefunctions-method-sqlserverdatabasemetadata.md)|Recupera un elenco delimitato da virgole delle funzioni data e ora disponibili con il database.|  
|[getTypeInfo](../../../connect/jdbc/reference/gettypeinfo-method-sqlserverdatabasemetadata.md)|Recupera una descrizione di tutti i tipi SQL standard supportati dal database corrente.|  
|[getUDTs](../../../connect/jdbc/reference/getudts-method-sqlserverdatabasemetadata.md)|Recupera una descrizione dei tipi definiti dall'utente configurati in un determinato schema.|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlserverdatabasemetadata.md)|Recupera l'URL del database.|  
|[getUserName](../../../connect/jdbc/reference/getusername-method-sqlserverdatabasemetadata.md)|Recupera il nome utente così noto nel database.|  
|[getVersionColumns](../../../connect/jdbc/reference/getversioncolumns-method-sqlserverdatabasemetadata.md)|Recupera una descrizione delle colonne di una tabella che viene aggiornata automaticamente all'aggiornamento di un valore di una riga.|  
|[insertsAreDetected](../../../connect/jdbc/reference/insertsaredetected-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se è possibile rilevare un inserimento di riga visibile chiamando il metodo [rowInserted](../../../connect/jdbc/reference/rowinserted-method-sqlserverresultset.md) della classe [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[isCatalogAtStart](../../../connect/jdbc/reference/iscatalogatstart-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se un catalogo viene visualizzato all'inizio di un nome di tabella completo.|  
|[isReadOnly](../../../connect/jdbc/reference/isreadonly-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database è in modalità di sola lettura.|  
|[locatorsUpdateCopy](../../../connect/jdbc/reference/locatorsupdatecopy-method-sqlserverdatabasemetadata.md)|Indica se gli aggiornamenti di un LOB vengono eseguiti su una copia o direttamente sul LOB.|  
|[nullPlusNonNullIsNull](../../../connect/jdbc/reference/nullplusnonnullisnull-method-sqlserverdatabasemetadata.md)|Indica se il database supporta concatenazioni tra valori NULL e non NULL che sono NULL.|  
|[nullsAreSortedAtEnd](../../../connect/jdbc/reference/nullsaresortedatend-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se i valori NULL vengono posizionati alla fine indipendentemente dal tipo di ordinamento.|  
|[nullsAreSortedAtStart](../../../connect/jdbc/reference/nullsaresortedatstart-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se i valori NULL vengono posizionati all'inizio indipendentemente dal tipo di ordinamento.|  
|[nullsAreSortedHigh](../../../connect/jdbc/reference/nullsaresortedhigh-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se i valori NULL vengono posizionati in alto.|  
|[nullsAreSortedLow](../../../connect/jdbc/reference/nullsaresortedlow-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se i valori NULL vengono posizionati in basso.|  
|[othersDeletesAreVisible](../../../connect/jdbc/reference/othersdeletesarevisible-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se sono visibili le operazioni di eliminazione eseguite da altri utenti.|  
|[othersInsertsAreVisible](../../../connect/jdbc/reference/othersinsertsarevisible-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se sono visibili le operazioni di inserimento eseguite da altri utenti.|  
|[othersUpdatesAreVisible](../../../connect/jdbc/reference/othersupdatesarevisible-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se sono visibili le operazioni di aggiornamento eseguite da altri utenti.|  
|[ownDeletesAreVisible](../../../connect/jdbc/reference/owndeletesarevisible-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se sono visibili le operazioni di eliminazione di un set di risultati eseguite dall'utente corrente.|  
|[ownInsertsAreVisible](../../../connect/jdbc/reference/owninsertsarevisible-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se sono visibili le operazioni di inserimento di un set di risultati eseguite dall'utente corrente.|  
|[ownUpdatesAreVisible](../../../connect/jdbc/reference/ownupdatesarevisible-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se sono visibili le operazioni di aggiornamento del set di risultati eseguite dall'utente corrente.|  
|[storesLowerCaseIdentifiers](../../../connect/jdbc/reference/storeslowercaseidentifiers-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database non tiene conto della distinzione tra maiuscole e minuscole negli identificatori SQL con maiuscole e minuscole non racchiusi tra virgolette e li archivia in minuscolo.|  
|[storesLowerCaseQuotedIdentifiers](../../../connect/jdbc/reference/storeslowercasequotedidentifiers-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database non tiene conto della distinzione tra maiuscole e minuscole negli identificatori SQL con maiuscole e minuscole racchiusi tra virgolette e li archivia in minuscolo.|  
|[storesMixedCaseIdentifiers](../../../connect/jdbc/reference/storesmixedcaseidentifiers-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database non tiene conto della distinzione tra maiuscole e minuscole negli identificatori SQL con maiuscole e minuscole non racchiusi tra virgolette e li archivia in minuscolo e maiuscolo.|  
|[storesMixedCaseQuotedIdentifiers](../../../connect/jdbc/reference/storesmixedcasequotedidentifiers-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database non tiene conto della distinzione tra maiuscole e minuscole negli identificatori SQL con maiuscole e minuscole racchiusi tra virgolette e li archivia in minuscolo e maiuscolo.|  
|[storesUpperCaseIdentifiers](../../../connect/jdbc/reference/storesuppercaseidentifiers-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database non tiene conto della distinzione tra maiuscole e minuscole negli identificatori SQL con maiuscole e minuscole non racchiusi tra virgolette e li archivia in maiuscolo.|  
|[storesUpperCaseQuotedIdentifiers](../../../connect/jdbc/reference/storesuppercasequotedidentifiers-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database non tiene conto della distinzione tra maiuscole e minuscole negli identificatori SQL con maiuscole e minuscole racchiusi tra virgolette e li archivia in maiuscolo.|  
|[supportsAlterTableWithAddColumn](../../../connect/jdbc/reference/supportsaltertablewithaddcolumn-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta ALTER TABLE con aggiunta di colonne.|  
|[supportsAlterTableWithDropColumn](../../../connect/jdbc/reference/supportsaltertablewithdropcolumn-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta ALTER TABLE con eliminazione di colonne.|  
|[supportsANSI92EntryLevelSQL](../../../connect/jdbc/reference/supportsansi92entrylevelsql-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta la grammatica SQL di base ANSI92.|  
|[supportsANSI92FullSQL](../../../connect/jdbc/reference/supportsansi92fullsql-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta la grammatica SQL completa ANSI92.|  
|[supportsANSI92IntermediateSQL](../../../connect/jdbc/reference/supportsansi92intermediatesql-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta la grammatica SQL intermedia ANSI92.|  
|[supportsBatchUpdates](../../../connect/jdbc/reference/supportsbatchupdates-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta aggiornamenti batch.|  
|[supportsCatalogsInDataManipulation](../../../connect/jdbc/reference/supportscatalogsindatamanipulation-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se un nome di catalogo può essere utilizzato in un'istruzione di manipolazione dei dati.|  
|[supportsCatalogsInIndexDefinitions](../../../connect/jdbc/reference/supportscatalogsinindexdefinitions-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se un nome di catalogo può essere utilizzato in un'istruzione di definizione degli indici.|  
|[supportsCatalogsInPrivilegeDefinitions](../../../connect/jdbc/reference/supportscatalogsinprivilegedefinitions-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se un nome di catalogo può essere utilizzato in un'istruzione di definizione dei privilegi.|  
|[supportsCatalogsInProcedureCalls](../../../connect/jdbc/reference/supportscatalogsinprocedurecalls-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se un nome di catalogo può essere utilizzato in un'istruzione di chiamata di procedura.|  
|[supportsCatalogsInTableDefinitions](../../../connect/jdbc/reference/supportscatalogsintabledefinitions-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se un nome di catalogo può essere utilizzato in un'istruzione di definizione delle tabelle.|  
|[supportsColumnAliasing](../../../connect/jdbc/reference/supportscolumnaliasing-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta l'aliasing delle colonne.|  
|[supportsConvert](../../../connect/jdbc/reference/supportsconvert-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta la funzione CONVERT tra i tipi SQL.|  
|[supportsCoreSQLGrammar](../../../connect/jdbc/reference/supportscoresqlgrammar-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta la grammatica SQL principale ODBC.|  
|[supportsCorrelatedSubqueries](../../../connect/jdbc/reference/supportscorrelatedsubqueries-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta le sottoquery correlate.|  
|[supportsDataDefinitionAndDataManipulationTransactions](../../../connect/jdbc/reference/supportsdatadefinitionanddatamanipulationtransactions-method.md)|Recupera un valore che indica se il database supporta sia l'istruzione di definizione dei dati sia quella di manipolazione dei dati all'interno di una transazione.|  
|[supportsDataManipulationTransactionsOnly](../../../connect/jdbc/reference/supportsdatamanipulationtransactionsonly-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta solo le istruzioni di manipolazione dei dati all'interno di una transazione.|  
|[supportsDifferentTableCorrelationNames](../../../connect/jdbc/reference/supportsdifferenttablecorrelationnames-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se, quando sono supportati, i nomi di correlazione delle tabelle sono impostati in modo da essere diversi da quelli delle tabelle.|  
|[supportsExpressionsInOrderBy](../../../connect/jdbc/reference/supportsexpressionsinorderby-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta espressioni negli elenchi ORDER BY.|  
|[supportsExtendedSQLGrammar](../../../connect/jdbc/reference/supportsextendedsqlgrammar-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta la grammatica SQL estesa ODBC.|  
|[supportsFullOuterJoins](../../../connect/jdbc/reference/supportsfullouterjoins-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta full outer join nidificati.|  
|[supportsGetGeneratedKeys](../../../connect/jdbc/reference/supportsgetgeneratedkeys-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se è possibile recuperare le chiavi generate automaticamente dopo che è stata eseguita un'istruzione.|  
|[supportsGroupBy](../../../connect/jdbc/reference/supportsgroupby-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta alcuni tipi di clausola GROUP BY.|  
|[supportsGroupByBeyondSelect](../../../connect/jdbc/reference/supportsgroupbybeyondselect-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta l'utilizzo di colonne non incluse nell'istruzione SELECT in una clausola GROUP BY, a condizione che tutte le colonne dell'istruzione SELECT siano incluse nella clausola GROUP BY.|  
|[supportsGroupByUnrelated](../../../connect/jdbc/reference/supportsgroupbyunrelated-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta l'utilizzo di una colonna non inclusa nell'istruzione SELECT in una clausola GROUP BY.|  
|[supportsIntegrityEnhancementFacility](../../../connect/jdbc/reference/supportsintegrityenhancementfacility-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta la funzionalità di miglioramento dell'integrità SQL.|  
|[supportsLikeEscapeClause](../../../connect/jdbc/reference/supportslikeescapeclause-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta la specifica di una clausola di escape LIKE.|  
|[supportsLimitedOuterJoins](../../../connect/jdbc/reference/supportslimitedouterjoins-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database offre supporto limitato per gli outer join.|  
|[supportsMinimumSQLGrammar](../../../connect/jdbc/reference/supportsminimumsqlgrammar-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta la grammatica SQL minima ODBC.|  
|[supportsMixedCaseIdentifiers](../../../connect/jdbc/reference/supportsmixedcaseidentifiers-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database non tiene conto della distinzione tra maiuscole e minuscole negli identificatori SQL con maiuscole e minuscole non racchiusi tra virgolette e li archivia in minuscolo e maiuscolo.|  
|[supportsMixedCaseQuotedIdentifiers](../../../connect/jdbc/reference/supportsmixedcasequotedidentifiers-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database non tiene conto della distinzione tra maiuscole e minuscole negli identificatori SQL con maiuscole e minuscole racchiusi tra virgolette e li archivia in minuscolo e maiuscolo.|  
|[supportsMultipleOpenResults](../../../connect/jdbc/reference/supportsmultipleopenresults-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se è possibile che vengano restituiti simultaneamente più oggetti [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) da un oggetto [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md).|  
|[supportsMultipleResultSets](../../../connect/jdbc/reference/supportsmultipleresultsets-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta il recupero di più oggetti [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) da una singola chiamata al metodo [execute](../../../connect/jdbc/reference/execute-method.md) della classe [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md).|  
|[supportsMultipleTransactions](../../../connect/jdbc/reference/supportsmultipletransactions-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database consente l'apertura contemporanea di più transazioni su connessioni diverse.|  
|[supportsNamedParameters](../../../connect/jdbc/reference/supportsnamedparameters-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta parametri denominati nelle istruzioni richiamabili.|  
|[supportsNonNullableColumns](../../../connect/jdbc/reference/supportsnonnullablecolumns-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se le colonne del database possono essere definite come colonne che non ammettono valori Null.|  
|[supportsOpenCursorsAcrossCommit](../../../connect/jdbc/reference/supportsopencursorsacrosscommit-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database consente di mantenere i cursori aperti tra i vari commit.|  
|[supportsOpenCursorsAcrossRollback](../../../connect/jdbc/reference/supportsopencursorsacrossrollback-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database consente di mantenere i cursori aperti tra i vari rollback.|  
|[supportsOpenStatementsAcrossCommit](../../../connect/jdbc/reference/supportsopenstatementsacrosscommit-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database consente di mantenere le istruzioni aperte tra i vari commit.|  
|[supportsOpenStatementsAcrossRollback](../../../connect/jdbc/reference/supportsopenstatementsacrossrollback-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database consente di mantenere le istruzioni aperte tra i vari rollback.|  
|[supportsOrderByUnrelated](../../../connect/jdbc/reference/supportsorderbyunrelated-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta l'utilizzo di una colonna non inclusa nell'istruzione SELECT in una clausola ORDER BY.|  
|[supportsOuterJoins](../../../connect/jdbc/reference/supportsouterjoins-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta alcuni tipi di outer join.|  
|[supportsPositionedDelete](../../../connect/jdbc/reference/supportspositioneddelete-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta le istruzioni DELETE posizionate.|  
|[supportsPositionedUpdate](../../../connect/jdbc/reference/supportspositionedupdate-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta le istruzioni UPDATE posizionate.|  
|[supportsResultSetConcurrency](../../../connect/jdbc/reference/supportsresultsetconcurrency-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta il tipo di concorrenza specificato in combinazione con un dato tipo di set di risultati.|  
|[supportsResultSetHoldability](../../../connect/jdbc/reference/supportsresultsetholdability-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta la trattenibilità del set di risultati specificata.|  
|[supportsResultSetType](../../../connect/jdbc/reference/supportsresultsettype-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta il tipo di set di risultati specificato.|  
|[supportsSavepoints](../../../connect/jdbc/reference/supportssavepoints-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta punti di salvataggio.|  
|[supportsSchemasInDataManipulation](../../../connect/jdbc/reference/supportsschemasindatamanipulation-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se un nome di schema può essere utilizzato in un'istruzione di manipolazione dei dati.|  
|[supportsSchemasInIndexDefinitions](../../../connect/jdbc/reference/supportsschemasinindexdefinitions-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se un nome di schema può essere utilizzato in un'istruzione di definizione degli indici.|  
|[supportsSchemasInPrivilegeDefinitions](../../../connect/jdbc/reference/supportsschemasinprivilegedefinitions-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se un nome di schema può essere utilizzato in un'istruzione di definizione dei privilegi.|  
|[supportsSchemasInProcedureCalls](../../../connect/jdbc/reference/supportsschemasinprocedurecalls-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se un nome di schema può essere utilizzato in un'istruzione di chiamata di procedura.|  
|[supportsSchemasInTableDefinitions](../../../connect/jdbc/reference/supportsschemasintabledefinitions-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se un nome di schema può essere utilizzato in un'istruzione di definizione delle tabelle.|  
|[supportsSelectForUpdate](../../../connect/jdbc/reference/supportsselectforupdate-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta istruzioni SELECT FOR UPDATE.|  
|[supportsStatementPooling](../../../connect/jdbc/reference/supportsstatementpooling-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta il pool di istruzioni.|  
|[supportsStoredFunctionsUsingCallSyntax](../../../connect/jdbc/reference/supportsstoredfunctionsusingcallsyntax-method-sqlserverdatabasemetadata.md)|Indica se nel database corrente è supportato il richiamo di funzioni definite dal fornitore o dall'utente tramite la sintassi di escape delle stored procedure.|  
|[supportsStoredProcedures](../../../connect/jdbc/reference/supportsstoredprocedures-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta le chiamate delle stored procedure che utilizzano la sintassi di escape delle stored procedure.|  
|[supportsSubqueriesInComparisons](../../../connect/jdbc/reference/supportssubqueriesincomparisons-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta le sottoquery nelle espressioni di confronto.|  
|[supportsSubqueriesInExists](../../../connect/jdbc/reference/supportssubqueriesinexists-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta le sottoquery nelle espressioni EXISTS.|  
|[supportsSubqueriesInIns](../../../connect/jdbc/reference/supportssubqueriesinins-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta le sottoquery nelle istruzioni IN.|  
|[supportsSubqueriesInQuantifieds](../../../connect/jdbc/reference/supportssubqueriesinquantifieds-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta le sottoquery nelle espressioni quantificate.|  
|[supportsTableCorrelationNames](../../../connect/jdbc/reference/supportstablecorrelationnames-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta i nomi di correlazione delle tabelle.|  
|[supportsTransactionIsolationLevel](../../../connect/jdbc/reference/supportstransactionisolationlevel-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta il livello di isolamento transazione specificato.|  
|[supportsTransactions](../../../connect/jdbc/reference/supportstransactions-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta transazioni.|  
|[supportsUnion](../../../connect/jdbc/reference/supportsunion-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta SQL UNION.|  
|[supportsUnionAll](../../../connect/jdbc/reference/supportsunionall-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database supporta SQL UNION ALL.|  
|[updatesAreDetected](../../../connect/jdbc/reference/updatesaredetected-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se è possibile rilevare un aggiornamento di riga visibile chiamando il metodo [rowUpdated](../../../connect/jdbc/reference/rowupdated-method-sqlserverresultset.md) della classe [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[usesLocalFilePerTable](../../../connect/jdbc/reference/useslocalfilepertable-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database utilizza un file per ogni tabella.|  
|[usesLocalFiles](../../../connect/jdbc/reference/useslocalfiles-method-sqlserverdatabasemetadata.md)|Recupera un valore che indica se il database archivia le tabelle in un file locale.|  
  
## <a name="inherited-methods"></a>Metodi ereditati  
  
|Classe ereditata da:|Metodi|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Vedere anche  
 [Classe SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
