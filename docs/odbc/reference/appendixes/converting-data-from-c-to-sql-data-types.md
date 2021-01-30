---
description: Conversione di dati da C ai tipi di dati SQL
title: Conversione di dati da C a tipi di dati SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from c to SQL types [ODBC], about converting
- converting data from c to SQL types [ODBC]
- data conversions from C to SQL types [ODBC], about converting
- data types [ODBC], C data types
- data types [ODBC], converting data
- SQL data types [ODBC], converting from C types
- data types [ODBC], SQL data types
- data conversions from C to SQL types [ODBC]
- C data types [ODBC], converting to SQL types
ms.assetid: ee0afe78-b58f-4d34-ad9b-616bb23653bd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a326f91f0a10e8731153817cd8c2eed7ff0656e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165391"
---
# <a name="converting-data-from-c-to-sql-data-types"></a>Conversione di dati da C ai tipi di dati SQL
Quando un'applicazione chiama **SQLExecute** o **SQLExecDirect**, il driver recupera i dati per qualsiasi parametro associato a **SQLBindParameter** da percorsi di archiviazione nell'applicazione. Quando un'applicazione chiama **SQLSetPos**, il driver recupera i dati per un'operazione di aggiornamento o aggiunta da colonne associate a **SQLBindCol**. Per i parametri data-at-execution, l'applicazione invia i dati dei parametri con **SQLPutData**. Se necessario, il driver converte i dati dal tipo di dati specificato dall'argomento *ValueType* in **SQLBindParameter** al tipo di dati specificato dall'argomento *ParameterType* in **SQLBindParameter** e quindi invia i dati all'origine dati.  
  
 Nella tabella seguente vengono illustrate le conversioni supportate dai tipi di dati ODBC C ai tipi di dati SQL ODBC. Un cerchio pieno indica la conversione predefinita per un tipo di dati SQL (il tipo di dati C da cui verranno convertiti i dati quando il valore di *ValueType* o il campo del descrittore SQL_DESC_CONCISE_TYPE è SQL_C_DEFAULT). Un cerchio vuoto indica una conversione supportata.  
  
 Il formato dei dati convertiti non è influenzato dall'impostazione di Windows® Country.  
  
 ![Conversioni supportate: dal tipo di dati C ODBC al tipo di dati SQL ODBC](../../../odbc/reference/appendixes/media/apd1b.gif "apd1b")  
  
 Le tabelle nelle sezioni seguenti descrivono in che modo il driver o l'origine dati converte i dati inviati all'origine dati. i driver sono necessari per supportare le conversioni da tutti i tipi di dati ODBC C ai tipi di dati SQL ODBC supportati. Per un determinato tipo di dati ODBC C, nella prima colonna della tabella sono elencati i valori di input validi dell'argomento *ParameterType* in **SQLBindParameter**. Nella seconda colonna sono elencati i risultati di un test eseguito dal driver per determinare se è possibile convertire i dati. La terza colonna elenca il SQLSTATE restituito per ogni risultato da **SQLExecDirect**, **SQLExecute**, **SQLBulkOperations**, **SQLSetPos** o **SQLPutData**. I dati vengono inviati all'origine dati solo se viene restituito SQL_SUCCESS.  
  
 Se l'argomento *ParameterType* in **SQLBindParameter** contiene l'identificatore di un tipo di dati SQL ODBC che non viene visualizzato nella tabella per un tipo di dati C specificato, **SQLBindParameter** restituisce SQLSTATE 07006 (violazione dell'attributo del tipo di dati limitato). Se l'argomento *ParameterType* contiene un identificatore specifico del driver e il driver non supporta la conversione dal tipo di dati C ODBC specifico al tipo di dati SQL specifico del driver, **SQLBINDPARAMETER** restituisce SQLSTATE HYC00 (funzionalità facoltativa non implementata).  
  
 Se gli argomenti *ParameterValuePtr* e *StrLen_or_IndPtr* specificati in **SQLBindParameter** sono entrambi puntatori null, la funzione restituisce SQLSTATE HY009 (utilizzo non valido del puntatore null). Sebbene non venga visualizzato nelle tabelle, un'applicazione imposta il valore del buffer di lunghezza/indicatore a cui fa riferimento l'argomento *StrLen_or_IndPtr* di **SQLBindParameter** o il valore dell'argomento *StrLen_or_IndPtr* di **SQLPutData** su SQL_NULL_DATA per specificare un valore di dati SQL NULL. (L'argomento *StrLen_or_IndPtr* corrisponde al campo SQL_DESC_OCTET_LENGTH_PTR di APD.) L'applicazione imposta questi valori su SQL_NTS per specificare che il valore in \* *ParameterValuePtr* in **SQLBindParameter** o \* *DataPtr* in **SQLPutData** (a cui fa riferimento il campo SQL_DESC_DATA_PTR di APD) è una stringa con terminazione null.  
  
 Nelle tabelle vengono utilizzati i termini seguenti:  
  
-   **Lunghezza in byte dei** dati: numero di byte di dati SQL disponibili da inviare all'origine dati, indipendentemente dal fatto che i dati vengano troncati prima dell'invio all'origine dati. Per i dati di tipo stringa non è incluso lo spazio per il carattere di terminazione null.  
  
-   **Lunghezza in byte colonna** : numero di byte necessari per archiviare i dati nell'origine dati.  
  
-   **Lunghezza byte caratteri** : numero massimo di byte necessari per visualizzare i dati in formato carattere. Questa operazione è definita per ogni tipo di dati SQL in [dimensioni di visualizzazione](../../../odbc/reference/appendixes/display-size.md), tranne che la lunghezza in byte di caratteri è in byte, mentre le dimensioni di visualizzazione sono in caratteri.  
  
-   **Numero di cifre** : numero di caratteri utilizzato per rappresentare un numero, inclusi il segno meno, il separatore decimale ed esponente (se necessario).  
  
-   **Parole in**   
     **_corsivo_**  -elementi della grammatica SQL. Per la sintassi degli elementi di grammatica, vedere [Appendice C: grammatica SQL](../../../odbc/reference/appendixes/appendix-c-sql-grammar.md).  
  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Da C a SQL: carattere](../../../odbc/reference/appendixes/c-to-sql-character.md)  
  
-   [Da C a SQL: dati numerici](../../../odbc/reference/appendixes/c-to-sql-numeric.md)  
  
-   [Da C a SQL: bit](../../../odbc/reference/appendixes/c-to-sql-bit.md)  
  
-   [Da C a SQL: dati binari](../../../odbc/reference/appendixes/c-to-sql-binary.md)  
  
-   [Da C a SQL: data](../../../odbc/reference/appendixes/c-to-sql-date.md)  
  
-   [Da C a SQL: GUID](../../../odbc/reference/appendixes/c-to-sql-guid.md)  
  
-   [Da C a SQL: ora](../../../odbc/reference/appendixes/c-to-sql-time.md)  
  
-   [Da C a SQL: timestamp](../../../odbc/reference/appendixes/c-to-sql-timestamp.md)  
  
-   [Da C a SQL: intervalli anno-mese](../../../odbc/reference/appendixes/c-to-sql-year-month-intervals.md)  
  
-   [Da C a SQL: intervalli di data/ora](../../../odbc/reference/appendixes/c-to-sql-day-time-intervals.md)  
  
-   [Esempi di conversione di dati da C a SQL](../../../odbc/reference/appendixes/c-to-sql-data-conversion-examples.md)
