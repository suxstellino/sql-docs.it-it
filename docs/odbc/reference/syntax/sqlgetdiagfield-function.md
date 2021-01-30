---
description: Funzione SQLGetDiagField
title: Funzione SQLGetDiagField | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetDiagField
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetDiagField
helpviewer_keywords:
- SQLGetDiagField function [ODBC]
ms.assetid: 1dbc4398-97a8-4585-bb77-1f7ea75e24c4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 733933691297f22934fe59e0c337dfd6093ddd3b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194192"
---
# <a name="sqlgetdiagfield-function"></a>Funzione SQLGetDiagField

**Conformità**  
 Versione introdotta: ODBC 3,0 Standard Compliance: ISO 92  
  
 **Summary**  
 **SQLGetDiagField** restituisce il valore corrente di un campo di un record della struttura dei dati di diagnostica (associata a un handle specificato) che contiene informazioni su errori, avvisi e stato.  
  
## <a name="syntax"></a>Sintassi  
  
```cpp

SQLRETURN SQLGetDiagField(  
     SQLSMALLINT     HandleType,  
     SQLHANDLE       Handle,  
     SQLSMALLINT     RecNumber,  
     SQLSMALLINT     DiagIdentifier,  
     SQLPOINTER      DiagInfoPtr,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   StringLengthPtr);  
```  
  
## <a name="arguments"></a>Argomenti  
 *HandleType*  
 Input Identificatore del tipo di handle che descrive il tipo di handle per cui sono necessarie le diagnostica. I possibili valori sono i seguenti:  
  
-   SQL_HANDLE_DBC  
  
-   SQL_HANDLE_DBC_INFO_TOKEN  
  
-   SQL_HANDLE_DESC  
  
-   SQL_HANDLE_ENV  
  
-   SQL_HANDLE_STMT  
  
 SQL_HANDLE_DBC_INFO_TOKEN handle viene utilizzato solo dal driver e dalla gestione driver. Le applicazioni non devono usare questo tipo di handle. Per ulteriori informazioni su SQL_HANDLE_DBC_INFO_TOKEN, vedere [sviluppo di Connection-Pool awareness in un driver ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md).  
  
 *Handle*  
 Input Handle per la struttura dei dati di diagnostica, del tipo indicato da *HandleType*. Se *HandleType* è SQL_HANDLE_ENV, *handle* può essere un handle di ambiente condiviso o non condiviso.  
  
 *RecNumber*  
 Input Indica il record di stato da cui l'applicazione cerca le informazioni. I record di stato sono numerati da 1. Se l'argomento *DiagIdentifier* indica un campo dell'intestazione di diagnostica, *RecNumber* viene ignorato. In caso contrario, deve essere maggiore di 0.  
  
 *DiagIdentifier*  
 Input Indica il campo della diagnostica di cui deve essere restituito il valore. Per ulteriori informazioni, vedere la sezione "argomento *DiagIdentifier* " in "Commenti".  
  
 *DiagInfoPtr*  
 Output Puntatore a un buffer in cui restituire le informazioni di diagnostica. Il tipo di dati dipende dal valore di *DiagIdentifier*. Se *DiagInfoPtr* è un tipo Integer, le applicazioni devono usare un buffer di SQLULEN e inizializzare il valore su 0 prima di chiamare questa funzione, in quanto alcuni driver possono scrivere solo la versione precedente a 32 bit o a 16 bit di un buffer e lasciare invariato il bit di ordine superiore.  
  
 Se *DiagInfoPtr* è null, *StringLengthPtr* restituisce comunque il numero totale di byte (escluso il carattere di terminazione null per i dati di tipo carattere) disponibili per restituire nel buffer a cui punta *DiagInfoPtr*.  
  
 *BufferLength*  
 Input Se *DiagIdentifier* è una diagnostica definita da ODBC e *DiagInfoPtr* punta a una stringa di caratteri o a un buffer binario, questo argomento deve essere la lunghezza di \* *DiagInfoPtr*. Se *DiagIdentifier* è un campo definito da ODBC e \* *DiagInfoPtr* è un numero intero, *bufferLength* viene ignorato. Se il valore in *\* DiagInfoPtr* è una stringa Unicode (quando si chiama **SQLGetDiagFieldW**), l'argomento *bufferLength* deve essere un numero pari.  
  
 Se *DiagIdentifier* è un campo definito dal driver, l'applicazione indica la natura del campo per Gestione driver impostando l'argomento *bufferLength* . *BufferLength* può avere i valori seguenti:  
  
-   Se *DiagInfoPtr* è un puntatore a una stringa di caratteri, *bufferLength* è la lunghezza della stringa o del SQL_NTS.  
  
-   Se *DiagInfoPtr* è un puntatore a un buffer binario, l'applicazione inserisce il risultato della macro SQL_LEN_BINARY_ATTR (*length*) in *bufferLength*. Questo inserisce un valore negativo in *bufferLength*.  
  
-   Se *DiagInfoPtr* è un puntatore a un valore diverso da una stringa di caratteri o una stringa binaria, il valore di *BufferLength* deve essere SQL_IS_POINTER.  
  
-   Se *\* DiagInfoPtr* contiene un tipo di dati a lunghezza fissa, *BufferLength* è SQL_IS_INTEGER, SQL_IS_UINTEGER, SQL_IS_SMALLINT o SQL_IS_USMALLINT, a seconda dei casi.  
  
 *StringLengthPtr*  
 Output Puntatore a un buffer in cui restituire il numero totale di byte, escluso il numero di byte necessari per il carattere di terminazione null, disponibile per restituire in \* *DiagInfoPtr* per i dati di tipo carattere. Se il numero di byte disponibili per restituire è maggiore o uguale a *bufferLength*, il testo in \* *DiagInfoPtr* viene troncato in *bufferLength* meno la lunghezza di un carattere di terminazione null.  
  
## <a name="returns"></a>Restituisce  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR, SQL_INVALID_HANDLE o SQL_NO_DATA.  
  
## <a name="diagnostics"></a>Diagnostica  
 **SQLGetDiagField** non pubblica i record di diagnostica per se stesso. USA i valori restituiti seguenti per segnalare il risultato della propria esecuzione:  
  
-   SQL_SUCCESS: la funzione ha restituito correttamente le informazioni di diagnostica.  
  
-   SQL_SUCCESS_WITH_INFO: \* *DiagInfoPtr* troppo piccolo per conservare il campo di diagnostica richiesto. Pertanto, i dati nel campo di diagnostica sono stati troncati. Per determinare se si è verificato un troncamento, l'applicazione deve confrontare *bufferLength* con il numero effettivo di byte disponibili, scritto in **StringLengthPtr*.  
  
-   SQL_INVALID_HANDLE: l'handle indicato da *HandleType* e *handle* non è un handle valido.  
  
-   SQL_ERROR: si è verificata una delle condizioni seguenti:  
  
    -   *L'argomento DiagIdentifier* non è uno dei valori validi.  
  
    -   *L'argomento DiagIdentifier* è SQL_DIAG_CURSOR_ROW_COUNT, SQL_DIAG_DYNAMIC_FUNCTION, SQL_DIAG_DYNAMIC_FUNCTION_CODE o SQL_DIAG_ROW_COUNT, ma *handle* non è un handle di istruzione. (Gestione driver restituisce questa diagnostica).  
  
    -   *L'argomento RecNumber* è negativo o 0 quando *DiagIdentifier* indica un campo da un record di diagnostica. *RecNumber* viene ignorato per i campi di intestazione.  
  
    -   Il valore richiesto è una stringa di caratteri e *bufferLength* è minore di zero.  
  
    -   Quando si usa la notifica asincrona, l'operazione asincrona sull'handle non è stata completata.  
  
-   SQL_NO_DATA: *RecNumber* è maggiore del numero di record di diagnostica esistenti per l'handle specificato nell' *handle.* La funzione restituisce inoltre SQL_NO_DATA per qualsiasi *RecNumber* positivo se non sono presenti record di diagnostica per *handle*.  
  
## <a name="comments"></a>Commenti  
 Un'applicazione in genere chiama **SQLGetDiagField** per ottenere uno dei tre obiettivi seguenti:  
  
1.  Per ottenere informazioni specifiche sull'errore o sull'avviso quando una chiamata di funzione ha restituito SQL_ERROR o SQL_SUCCESS_WITH_INFO (o SQL_NEED_DATA per la funzione **SQLBrowseConnect** ).  
  
2.  Per determinare il numero di righe nell'origine dati interessate quando sono state eseguite operazioni di inserimento, eliminazione o aggiornamento con una chiamata a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** (dal campo di intestazione SQL_DIAG_ROW_COUNT) o per determinare il numero di righe presenti nel cursore aperto corrente, se il driver è in grado di fornire queste informazioni (dal campo di intestazione SQL_DIAG_CURSOR_ROW_COUNT).  
  
3.  Per determinare la funzione eseguita da una chiamata a **SQLExecDirect** o **SQLExecute** (dal SQL_DIAG_DYNAMIC_FUNCTION e SQL_DIAG_DYNAMIC_FUNCTION_CODE campi di intestazione).  
  
 Qualsiasi funzione ODBC può inviare zero o più record di diagnostica ogni volta che viene chiamata, quindi un'applicazione può chiamare **SQLGetDiagField** dopo qualsiasi chiamata di funzione ODBC. Non esiste un limite al numero di record di diagnostica che possono essere archiviati in qualsiasi momento. **SQLGetDiagField** recupera solo le informazioni di diagnostica associate più di recente alla struttura dei dati di diagnostica specificata nell'argomento *handle* . Se l'applicazione chiama una funzione ODBC diversa da **SQLGetDiagField** o **SQLGetDiagRec**, vengono perse tutte le informazioni di diagnostica di una chiamata precedente con lo stesso handle.  
  
 Un'applicazione può analizzare tutti i record di diagnostica incrementando *RecNumber*, purché **SQLGetDiagField** restituisca SQL_SUCCESS. Il numero di record di stato è indicato nel campo intestazione SQL_DIAG_NUMBER. Le chiamate a **SQLGetDiagField** non sono distruttive per i campi di intestazione e di record. L'applicazione può chiamare **SQLGetDiagField** in un secondo momento per recuperare un campo da un record, a condizione che una funzione diversa dalle funzioni di diagnostica non sia stata chiamata nel frattempo, in modo da inserire i record sullo stesso handle.  
  
 Un'applicazione può chiamare **SQLGetDiagField** per restituire qualsiasi campo di diagnostica in qualsiasi momento, ad eccezione di SQL_DIAG_CURSOR_ROW_COUNT o SQL_DIAG_ROW_COUNT, che restituirà SQL_ERROR se *handle* non è un handle di istruzione. Se un altro campo diagnostico non è definito, la chiamata a **SQLGetDiagField** restituirà SQL_SUCCESS (purché non venga rilevata nessun'altra diagnostica) e verrà restituito un valore non definito per il campo.  
  
 Per altre informazioni, vedere [uso di SQLGetDiagRec e SQLGetDiagField](../../../odbc/reference/develop-app/using-sqlgetdiagrec-and-sqlgetdiagfield.md) e [implementazione di SQLGetDiagRec e SQLGetDiagField](../../../odbc/reference/develop-app/implementing-sqlgetdiagrec-and-sqlgetdiagfield.md).  
  
 La chiamata di un'API diversa da quella che viene eseguita in modo asincrono genererà HY010 "errore della sequenza di funzioni". Non è tuttavia possibile recuperare il record di errore prima del completamento dell'operazione asincrona.  
  
## <a name="handletype-argument"></a>Argomento HandleType  
 A ogni tipo di handle possono essere associate informazioni di diagnostica. L'argomento *HandleType* indica il *tipo di handle*.  
  
 Non è possibile restituire alcuni campi di intestazione e record per gli handle di ambiente, connessione, istruzione e descrittore. Gli handle per i quali non è applicabile un campo sono indicati nelle sezioni "campi di intestazione" e "campi dei record" seguenti.  
  
 Se *HandleType* è SQL_HANDLE_ENV, *handle* può essere un handle di ambiente condiviso o non condiviso.  
  
 Nessun campo di diagnostica dell'intestazione specifico del driver deve essere associato a un handle di ambiente.  
  
 Gli unici campi di intestazione diagnostici definiti per un handle di descrittore sono SQL_DIAG_NUMBER e SQL_DIAG_RETURNCODE.  
  
## <a name="diagidentifier-argument"></a>Argomento DiagIdentifier  
 Questo argomento indica l'identificatore del campo richiesto dalla struttura dei dati di diagnostica. Se *RecNumber* è maggiore o uguale a 1, i dati nel campo descrivono le informazioni di diagnostica restituite da una funzione. Se *RecNumber* è 0, il campo si trova nell'intestazione della struttura dei dati di diagnostica e pertanto contiene i dati relativi alla chiamata di funzione che ha restituito le informazioni di diagnostica e non alle informazioni specifiche.  
  
 I driver possono definire i campi di intestazione e record specifici del driver nella struttura dei dati di diagnostica.  
  
 Un'applicazione ODBC 3 *. x* che utilizza un driver ODBC 2 *. x* sarà in grado di chiamare **SQLGetDiagField** solo con un argomento *DiagIdentifier* di SQL_DIAG_CLASS_ORIGIN, SQL_DIAG_CLASS_SUBCLASS_ORIGIN, SQL_DIAG_CONNECTION_NAME, SQL_DIAG_MESSAGE_TEXT, SQL_DIAG_NATIVE, SQL_DIAG_NUMBER, SQL_DIAG_RETURNCODE, SQL_DIAG_SERVER_NAME o SQL_DIAG_SQLSTATE. Tutti gli altri campi di diagnostica restituiranno SQL_ERROR.  
  
## <a name="header-fields"></a>Campi di intestazione  
 I campi di intestazione elencati nella tabella seguente possono essere inclusi nell'argomento *DiagIdentifier* .  
  
|DiagIdentifier|Tipo restituito|Restituisce|  
|--------------------|-----------------|-------------|  
|SQL_DIAG_CURSOR_ROW_COUNT|SQLLEN|Questo campo contiene il numero di righe nel cursore. La semantica dipende dai tipi di informazioni **SQLGetInfo** SQL_DYNAMIC_CURSOR_ATTRIBUTES2, SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES2, SQL_KEYSET_CURSOR_ATTRIBUTES2 e SQL_STATIC_CURSOR_ATTRIBUTES2, che indicano i conteggi delle righe disponibili per ogni tipo di cursore (nella SQL_CA2_CRC_EXACT e SQL_CA2_CRC_APPROXIMATE bit).<br /><br /> Il contenuto di questo campo viene definito solo per gli handle di istruzione e solo dopo la chiamata a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** . La chiamata di **SQLGetDiagField** con un *DiagIdentifier* di SQL_DIAG_CURSOR_ROW_COUNT su un handle di istruzione restituirà SQL_ERROR.|  
|SQL_DIAG_DYNAMIC_FUNCTION|SQLCHAR|Si tratta di una stringa che descrive l'istruzione SQL eseguita dalla funzione sottostante. (Vedere "valori dei campi della funzione dinamica", più avanti in questa sezione, per valori specifici). Il contenuto di questo campo viene definito solo per gli handle di istruzione e solo dopo una chiamata a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults**. La chiamata di **SQLGetDiagField** con un *DiagIdentifier* di SQL_DIAG_DYNAMIC_FUNCTION su un handle di istruzione restituirà SQL_ERROR. Il valore di questo campo non è definito prima di una chiamata a **SQLExecute** o **SQLExecDirect**.|  
|SQL_DIAG_DYNAMIC_FUNCTION_CODE|SQLINTEGER|Si tratta di un codice numerico che descrive l'istruzione SQL eseguita dalla funzione sottostante. (Vedere "valori dei campi della funzione dinamica", più avanti in questa sezione, per un valore specifico). Il contenuto di questo campo viene definito solo per gli handle di istruzione e solo dopo una chiamata a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults**. La chiamata di **SQLGetDiagField** con un *DiagIdentifier* di SQL_DIAG_DYNAMIC_FUNCTION_CODE su un handle di istruzione restituirà SQL_ERROR. Il valore di questo campo non è definito prima di una chiamata a **SQLExecute** o **SQLExecDirect**.|  
|SQL_DIAG_NUMBER|SQLINTEGER|Numero di record di stato disponibili per l'handle specificato.|  
|SQL_DIAG_RETURNCODE|SQLRETURN|Codice restituito dalla funzione. Per un elenco di codici restituiti, vedere [codici restituiti](../../../odbc/reference/develop-app/return-codes-odbc.md). Il driver non deve implementare SQL_DIAG_RETURNCODE; viene sempre implementato da Gestione driver. Se non è ancora stata chiamata alcuna funzione nell' *handle*, per SQL_DIAG_RETURNCODE verrà restituito SQL_SUCCESS.|  
|SQL_DIAG_ROW_COUNT|SQLLEN|Numero di righe interessate da un'istruzione INSERT, DELETE o UPDATE eseguita da **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos**. È definito dal driver dopo l'esecuzione di una *specifica del cursore* . Il contenuto di questo campo viene definito solo per gli handle di istruzione. La chiamata di **SQLGetDiagField** con un *DiagIdentifier* di SQL_DIAG_ROW_COUNT su un handle di istruzione restituirà SQL_ERROR. I dati in questo campo vengono restituiti anche nell'argomento *RowCountPtr* di **SQLRowCount**. I dati in questo campo vengono reimpostati dopo ogni chiamata di funzione non diagnostica, mentre il conteggio delle righe restituito da **SQLRowCount** rimane invariato fino a quando l'istruzione non viene reimpostata sullo stato preparato o allocato.|  
  
## <a name="record-fields"></a>Campi di record  
 I campi dei record elencati nella tabella seguente possono essere inclusi nell'argomento *DiagIdentifier* .  
  
|DiagIdentifier|Tipo restituito|Restituisce|  
|--------------------|-----------------|-------------|  
|SQL_DIAG_CLASS_ORIGIN|SQLCHAR|Stringa che indica il documento che definisce la parte di classe del valore SQLSTATE in questo record. Il valore è "ISO 9075" per tutti gli stati definiti dall'interfaccia Open Group e ISO a livello di chiamata. Per sqlstati specifici di ODBC (tutti quelli la cui classe SQLSTATE è "IM"), il relativo valore è "ODBC 3,0".|  
|SQL_DIAG_COLUMN_NUMBER|SQLINTEGER|Se il campo SQL_DIAG_ROW_NUMBER è un numero di riga valido in un set di righe o in un set di parametri, questo campo contiene il valore che rappresenta il numero di colonna nel set di risultati o il numero del parametro nel set di parametri. I numeri di colonna del set di risultati iniziano sempre da 1. Se questo record di stato è relativo a una colonna del segnalibro, il campo può essere zero. I numeri di parametro iniziano da 1. Ha il valore SQL_NO_COLUMN_NUMBER se il record di stato non è associato a un numero di colonna o a un numero di parametro. Se il driver non è in grado di determinare il numero di colonna o il numero di parametro a cui è associato questo record, questo campo ha il valore SQL_COLUMN_NUMBER_UNKNOWN.<br /><br /> Il contenuto di questo campo viene definito solo per gli handle di istruzione.|  
|SQL_DIAG_CONNECTION_NAME|SQLCHAR|Stringa che indica il nome della connessione a cui si riferisce il record di diagnostica. Questo campo è definito dal driver. Per le strutture di dati di diagnostica associate all'handle di ambiente e per la diagnostica che non si riferiscono a nessuna connessione, questo campo è una stringa di lunghezza zero.|  
|SQL_DIAG_MESSAGE_TEXT|SQLCHAR|Un messaggio informativo sull'errore o sull'avviso. Questo campo è formattato come descritto in [messaggi di diagnostica](../../../odbc/reference/develop-app/diagnostic-messages.md). Non esiste una lunghezza massima per il testo del messaggio di diagnostica.|  
|SQL_DIAG_NATIVE|SQLINTEGER|Codice di errore nativo specifico di un driver o di un'origine dati. Se non è presente codice di errore nativo, il driver restituisce 0.|  
|SQL_DIAG_ROW_NUMBER|SQLLEN|Questo campo contiene il numero di riga nel set di righe o il numero del parametro nel set di parametri a cui è associato il record di stato. I numeri di riga e i numeri di parametro iniziano con 1. Questo campo presenta il valore SQL_NO_ROW_NUMBER se il record di stato non è associato a un numero di riga o a un numero di parametro. Se il driver non è in grado di determinare il numero di riga o il numero di parametro a cui è associato questo record, questo campo ha il valore SQL_ROW_NUMBER_UNKNOWN.<br /><br /> Il contenuto di questo campo viene definito solo per gli handle di istruzione.|  
|SQL_DIAG_SERVER_NAME|SQLCHAR|Stringa che indica il nome del server a cui si riferisce il record di diagnostica. Corrisponde al valore restituito per una chiamata a **SQLGetInfo** con l'opzione SQL_DATA_SOURCE_NAME. Per le strutture di dati di diagnostica associate al punto di controllo dell'ambiente e per la diagnostica che non si riferiscono a nessun server, questo campo è una stringa di lunghezza zero.|  
|SQL_DIAG_SQLSTATE|SQLCHAR|Codice di diagnostica SQLSTATE di cinque caratteri. Per ulteriori informazioni, vedere [Sqlstates](../../../odbc/reference/develop-app/sqlstates.md).|  
|SQL_DIAG_SUBCLASS_ORIGIN|SQLCHAR|Stringa con lo stesso formato e i valori validi di SQL_DIAG_CLASS_ORIGIN, che identifica la parte di definizione della parte della sottoclasse del codice SQLSTATE. Gli Stati SQLSTATE specifici di ODBC per i quali viene restituito "ODBC 3,0" includono quanto segue:<br /><br /> 01S00, 01S01, 01S02, 01S06, 01S07, 07S01, PARI 08S01, 21S01, 21S02, 25S01, 25S02, 25S03, 42S01, 42S02, 42S11, 42S12, 42S21, 42S22, HY095, HY097, HY098, HY099, HY100, HY101, HY105, HY107, HY109, HY110, HY111, HYT00, HYT01, IM001, IM002, IM003, IM004, IM005, IM006, IM007, IM008, IM010, IM011, IM012.|  
  
## <a name="values-of-the-dynamic-function-fields"></a>Valori dei campi della funzione dinamica  
 Nella tabella seguente vengono descritti i valori di SQL_DIAG_DYNAMIC_FUNCTION e SQL_DIAG_DYNAMIC_FUNCTION_CODE applicabili a ogni tipo di istruzione SQL eseguita da una chiamata a **SQLExecute** o **SQLExecDirect**. Il driver può aggiungere valori definiti dal driver a quelli elencati.  
  
|Istruzione SQL<br /><br /> eseguito|Valore di <br /><br /> SQL_DIAG_DYNAMIC_FUNCTION|Valore di <br /><br /> SQL_DIAG_DYNAMIC_FUNCTION_CODE|  
|--------------------------------|-----------------------------------------------|-----------------------------------------------------|  
|*Alter-Domain-Statement*|"ALTER DOMAIN"|SQL_DIAG_ALTER_DOMAIN|  
|*ALTER-TABLE-Statement*|"ALTER TABLE"|SQL_DIAG_ALTER_TABLE|  
|*Assertion-definizione*|"CREA ASSERZIONE"|SQL_DIAG_CREATE_ASSERTION|  
|*set di caratteri-definizione*|"CREA SET DI CARATTERI"|SQL_DIAG_CREATE_CHARACTER_SET|  
|*regole di confronto-definizione*|"CREA REGOLE DI CONFRONTO"|SQL_DIAG_CREATE_COLLATION|  
|*dominio-definizione*|"CREA DOMINIO"|SQL_DIAG_CREATE_DOMAIN|
|*create-index-Statement*|"CREA INDICE"|SQL_DIAG_CREATE_INDEX|  
|*CREATE-TABLE-Statement*|"CREATE TABLE"|SQL_DIAG_CREATE_TABLE|  
|*Create-View-Statement*|"CREA VISTA"|SQL_DIAG_CREATE_VIEW|  
|*specifica del cursore*|"SELEZIONA CURSORE"|SQL_DIAG_SELECT_CURSOR|  
|*Delete-Statement-posizionato*|"CURSORE ELIMINAZIONE DINAMICA"|SQL_DIAG_DYNAMIC_DELETE_CURSOR|  
|*Delete-Statement-ricerca eseguita*|"ELIMINA DOVE"|SQL_DIAG_DELETE_WHERE|  
|*Drop-Assertion-Statement*|"ELIMINA ASSERZIONE"|SQL_DIAG_DROP_ASSERTION|  
|*Drop-character-set-stmt*|"DROP CHARACTER SET"|SQL_DIAG_DROP_CHARACTER_SET|  
|*Drop-Collation-Statement*|"ELIMINA REGOLE DI CONFRONTO"|SQL_DIAG_DROP_COLLATION|  
|*Drop-Domain-Statement*|"ELIMINA DOMINIO"|SQL_DIAG_DROP_DOMAIN|  
|*DROP-INDEX-statement*|"DROP INDEX"|SQL_DIAG_DROP_INDEX|  
|*Drop-schema-Statement*|"ELIMINA SCHEMA"|SQL_DIAG_DROP_SCHEMA|  
|*DROP-TABLE-Statement*|"ELIMINA TABELLA"|SQL_DIAG_DROP_TABLE|  
|*Drop-Translation-Statement*|"DROP TRANSLATION"|SQL_DIAG_DROP_TRANSLATION|  
|*Drop-View-Statement*|"DROP VIEW"|SQL_DIAG_DROP_VIEW|  
|*GrantStatement*|Grant|SQL_DIAG_GRANT|
|*INSERT-Statement*|INSERIRE|SQL_DIAG_INSERT|  
|*ODBC-procedura-estensione*|CHIAMARE|CHIAMATA SQL_DIAG_|  
|*Revoke-Statement*|REVOCARE|SQL_DIAG_REVOKE|  
|*schema-definizione*|"CREA SCHEMA"|SQL_DIAG_CREATE_SCHEMA|  
|*Traduzione-definizione*|"CREA TRADUZIONE"|SQL_DIAG_CREATE_TRANSLATION|  
|*UPDATE-statement-posizionato*|"CURSORE AGGIORNAMENTO DINAMICO"|SQL_DIAG_DYNAMIC_UPDATE_CURSOR|  
|*UPDATE-statement-ricerca eseguita*|"AGGIORNA DOVE"|SQL_DIAG_UPDATE_WHERE|  
|Sconosciuto|*stringa vuota*|SQL_DIAG_UNKNOWN_STATEMENT|  

<!--
These two malformed table rows were fixed by educated GUESS only.
Each pair starts with the original flawed row.
Flawed because treated as only two cells by HTML render,
and because missing info anyway.
Also, these flawed rows lacked '|' as their first nonWhitespace character (although markdown technically allows this omission, unfortunately).
Arguably the following SQL.H file shows the sequence of the flawed rows in the table was suboptimal also.

ftp://www.fpc.org/fpc32/VS6Disk1/VC98/INCLUDE/SQL.H

GeneMi , 2019/01/19
- - - - - - - - - - - - - -

n-definition*|"CREATE DOMAIN"|SQL_DIAG_CREATE_DOMAIN|  

|*domain-definition*|"CREATE DOMAIN"|SQL_DIAG_CREATE_DOMAIN|

-statement*|"GRANT"|SQL_DIAG_GRANT|  

|*grant-statement*|"GRANT"|SQL_DIAG_GRANT|

-->

## <a name="sequence-of-status-records"></a>Sequenza di record di stato

 I record di stato vengono posizionati in una sequenza in base al numero di riga e al tipo di diagnostica. Gestione driver determina l'ordine finale in cui restituire i record di stato generati. Il driver determina l'ordine finale in cui restituire i record di stato generati.  
  
 Se i record di diagnostica vengono pubblicati sia da Gestione driver che dal driver, gestione driver è responsabile dell'ordinamento.  
  
 Se sono presenti due o più record di stato, la sequenza dei record viene determinata prima in base al numero di riga. Per determinare la sequenza di record di diagnostica per riga, si applicano le regole seguenti:  
  
-   I record che non corrispondono a nessuna riga vengono visualizzati davanti ai record che corrispondono a una determinata riga, perché SQL_NO_ROW_NUMBER viene definito come-1.  
  
-   I record per i quali il numero di riga è sconosciuto vengono visualizzati davanti a tutti gli altri record, perché SQL_ROW_NUMBER_UNKNOWN viene definito come-2.  
  
-   Per tutti i record relativi a righe specifiche, i record vengono ordinati in base al valore nel campo SQL_DIAG_ROW_NUMBER. Vengono elencati tutti gli errori e gli avvisi della prima riga interessati, quindi tutti gli errori e gli avvisi della riga successiva interessati e così via.  
  
> [!NOTE]
>  Gestione driver ODBC 3 *. x* non Ordina i record di stato nella coda di diagnostica se SQLSTATE 01S01 (Error in Row) viene restituito da un driver ODBC *2. x* o se SQLSTATE 01S01 (Error in Row) viene restituito da un driver ODBC 3 *. x* quando viene chiamato **SQLExtendedFetch** o **SQLSetPos** su un cursore posizionato con **SQLExtendedFetch**.  
  
 All'interno di ogni riga o per tutti i record che non corrispondono a una riga o per i quali il numero di riga è sconosciuto o per tutti i record con un numero di riga uguale a SQL_NO_ROW_NUMBER, il primo record elencato viene determinato tramite un set di regole di ordinamento. Dopo il primo record, l'ordine degli altri record che interessano una riga non è definito. Un'applicazione non può presupporre che gli errori precedano gli avvisi dopo il primo record. Le applicazioni devono analizzare la struttura dei dati di diagnostica completa per ottenere informazioni complete su una chiamata non riuscita a una funzione.  
  
 Le regole seguenti vengono utilizzate per determinare il primo record all'interno di una riga. Il record con il rango più alto è il primo record. L'origine di un record (Gestione driver, driver, gateway e così via) non viene considerata quando i record di rango.  
  
-   **Errori** di I record di stato che descrivono errori hanno il rango più alto. Per gli errori di ordinamento vengono applicate le regole seguenti:  
  
    -   Record che indicano un errore di transazione o un possibile errore di transazione in ordine di priorità di tutti gli altri record.  
  
    -   Se due o più record descrivono la stessa condizione di errore, i SQLSTATE definiti dalla specifica dell'interfaccia della riga di comando per il gruppo aperto (classi da 03 a HZ) derangono ODBC e sqlstates definiti dal driver.  
  
-   **Implementazione-nessun valore di dati definito** I record di stato che descrivono i valori di dati non definiti dal driver (classe 02) hanno il secondo rango più alto.  
  
-   **Avvisi** di I record di stato che descrivono gli avvisi (classe 01) hanno il rango più basso. Se due o più record descrivono la stessa condizione di avviso, gli Stati di avviso SQLSTATE definiti dalla specifica CLI del gruppo aperto vengono classificati come SQLSTATE definito da ODBC e dal driver.  
  
## <a name="related-functions"></a>Funzioni correlate  
  
|Per informazioni su|Vedere|  
|---------------------------|---------|  
|Recupero di più campi di una struttura di dati di diagnostica|[Funzione SQLGetDiagRec](sqlgetdiagrec-function.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Informazioni di riferimento sulle API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [File di intestazione ODBC](../../../odbc/reference/install/odbc-header-files.md)
