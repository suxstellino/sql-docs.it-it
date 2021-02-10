---
description: Tipi di dati C
title: Tipi di dati C | Microsoft Docs
ms.custom: ''
ms.date: 07/12/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], C data types
- C data types [ODBC], about C data types
- C data types [ODBC]
- C buffers [ODBC]
ms.assetid: b681d260-3dbb-47df-a616-4910d727add7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 322aa55f007e4ebf1ba3fa2b3093bf750db7e678
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051892"
---
# <a name="c-data-types"></a>Tipi di dati C
I tipi di dati ODBC C indicano il tipo di dati dei buffer C utilizzati per archiviare i dati nell'applicazione.  
  
 Tutti i driver devono supportare tutti i tipi di dati C. Questa operazione è necessaria perché tutti i driver devono supportare tutti i tipi C ai quali possono essere convertiti i tipi SQL supportati e tutti i driver supportano almeno un tipo SQL di tipo carattere. Poiché il tipo di carattere SQL può essere convertito in e da tutti i tipi C, tutti i driver devono supportare tutti i tipi C.  
  
 Il tipo di dati C viene specificato nelle funzioni **SQLBindCol** e **SQLGetData** con l'argomento *targetType* e nella funzione **SQLBindParameter** con l'argomento *ValueType* . Può anche essere specificato chiamando **SQLSetDescField** per impostare il campo SQL_DESC_CONCISE_TYPE di un ARD o APD oppure chiamando **SQLSetDescRec** con l'argomento di *tipo* (e l'argomento del *sottotipo* , se necessario) e l'argomento *DESCRIPTORHANDLE* impostato sull'handle di un oggetto ARD o APD.  
  
 Nelle tabelle seguenti sono elencati gli identificatori di tipo validi per i tipi di dati C. La tabella elenca inoltre il tipo di dati C ODBC che corrisponde a ogni identificatore e la definizione di questo tipo di dati.  
  
|Identificatore di tipo C|Typedef ODBC C|Tipo C|  
|-----------------------|--------------------|------------|  
|SQL_C_CHAR|SQLCHAR|unsigned char *|  
|SQL_C_WCHAR|SQLWCHAR|wchar_t *|  
|SQL_C_SSHORT [j]|SQLSMALLINT|short int|  
|SQL_C_USHORT [j]|SQLUSMALLINT|unsigned short int|  
|SQL_C_SLONG [j]|SQLINTEGER|long int|  
|SQL_C_ULONG [j]|SQLUINTEGER|unsigned long int|  
|SQL_C_FLOAT|Sqlreal|float|  
|SQL_C_DOUBLE|SQLDOUBLE, SQLFLOAT|double|  
|SQL_C_BIT|SQLCHAR|unsigned char|  
|SQL_C_STINYINT [j]|SQLSCHAR|signed char|  
|SQL_C_UTINYINT [j]|SQLCHAR|unsigned char|  
|SQL_C_SBIGINT|SQLBIGINT|_int64 [h]|  
|SQL_C_UBIGINT|SQLUBIGINT|unsigned _int64 [h]|  
|SQL_C_BINARY|SQLCHAR|unsigned char *|  
|SQL_C_BOOKMARK [i]|Segnalibro|unsigned long int [d]|  
|SQL_C_VARBOOKMARK|SQLCHAR|unsigned char *|  
|Tutti i tipi di dati intervallo C|SQL_INTERVAL_STRUCT|Vedere la sezione [struttura intervallo C](../../../odbc/reference/appendixes/c-interval-structure.md) , più avanti in questa appendice.|  
  
 **Identificatore di tipo C** SQL_C_TYPE_DATE [C]  
  
 **Typedef ODBC C** SQL_DATE_STRUCT  
  
 **Tipo C**  
  
```  
struct tagDATE_STRUCT {  
   SQLSMALLINT year;  
   SQLUSMALLINT month;  
   SQLUSMALLINT day;    
} DATE_STRUCT;[a]  
```  
  
 **Identificatore di tipo C** SQL_C_TYPE_TIME [C]  
  
 **Typedef ODBC C** SQL_TIME_STRUCT  
  
 **Tipo C**  
  
```  
struct tagTIME_STRUCT {  
   SQLUSMALLINT hour;  
   SQLUSMALLINT minute;  
   SQLUSMALLINT second;  
} TIME_STRUCT;[a]  
```  
  
 **Identificatore di tipo C** SQL_C_TYPE_TIMESTAMP [C]  
  
 **Typedef ODBC C** SQL_TIMESTAMP_STRUCT  
  
 **Tipo C**  
  
```  
struct tagTIMESTAMP_STRUCT {  
   SQLSMALLINT year;  
   SQLUSMALLINT month;  
   SQLUSMALLINT day;  
   SQLUSMALLINT hour;  
   SQLUSMALLINT minute;  
   SQLUSMALLINT second;  
   SQLUINTEGER fraction;[b]   
} TIMESTAMP_STRUCT;[a]  
```  
  
 **Identificatore di tipo C** SQL_C_NUMERIC  
  
 **Typedef ODBC C** SQL_NUMERIC_STRUCT  
  
 **Tipo C**  
  
```  
struct tagSQL_NUMERIC_STRUCT {  
   SQLCHAR precision;  
   SQLSCHAR scale;  
   SQLCHAR sign[g];  
   SQLCHAR val[SQL_MAX_NUMERIC_LEN];[e], [f]   
} SQL_NUMERIC_STRUCT;  
```  
  
 **Identificatore di tipo C** SQL_C_GUID  
  
 **Typedef ODBC C** SQLGUID  
  
 **Tipo C**  
  
```  
struct tagSQLGUID {  
   DWORD Data1;  
   WORD Data2;  
   WORD Data3;  
   BYTE Data4[8];  
} SQLGUID;[k]  
```  
  
 [a] i valori dei campi anno, mese, giorno, ora, minuto e secondo nei tipi di dati DateTime C devono essere conformi ai vincoli del calendario gregoriano. Vedere [vincoli del calendario gregoriano](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md) più avanti in questa appendice.  
  
 [b] il valore del campo della frazione è il numero di miliardesimi di un secondo e viene compreso tra 0 e 999.999.999 (1 minore di 1 miliardo). Ad esempio, il valore del campo della frazione per un mezzo secondo è 500 milioni, per un millesimo di secondo (un millisecondo) è 1 milione, per un milionesimo di secondo (un microsecondo) è 1.000 e per un miliardesimo di secondo (un nanosecondo) è 1.  
  
 [c] in ODBC 2. *x*, i tipi di dati data, ora e timestamp C sono SQL_C_DATE, SQL_C_TIME e SQL_C_TIMESTAMP.  
  
 [d] le applicazioni ODBC 3 *. x* devono utilizzare SQL_C_VARBOOKMARK, non SQL_C_BOOKMARK. Quando un'applicazione ODBC 3 *. x* funziona con ODBC 2. driver *x* , gestione driver ODBC 3 *. x* eseguirà il mapping SQL_C_VARBOOKMARK SQL_C_BOOKMARK.  
  
 [e] un numero viene archiviato nel campo *Val* della struttura SQL_NUMERIC_STRUCT come intero scalato, in modalità little endian (il byte più a sinistra è il byte meno significativo). Ad esempio, il numero 10,001 base 10, con scala 4, viene ridimensionato a un numero intero di 100010. Poiché si tratta di 186AA in formato esadecimale, il valore in SQL_NUMERIC_STRUCT sarà "AA 86 01 00 00... 00 ", con il numero di byte definito dall'SQL_MAX_NUMERIC_LEN **#define**.  
  
 Per ulteriori informazioni su **SQL_NUMERIC_STRUCT**, vedere [HOWTO: recupero di dati numerici con SQL_NUMERIC_STRUCT](retrieve-numeric-data-sql-numeric-struct-kb222831.md).  
  
 [f] i campi di precisione e scala del SQL_C_NUMERIC tipo di dati areused per l'input da un'applicazione e per l'output dal driver all'applicazione. Quando il driver scrive un valore numerico nell'SQL_NUMERIC_STRUCT, utilizzerà il proprio valore predefinito specifico del driver come valore per il campo di *precisione* e utilizzerà il valore nel campo SQL_DESC_SCALE del descrittore dell'applicazione (per impostazione predefinita è 0) per il campo *scala* . Un'applicazione può fornire valori propri per la precisione e la scala impostando i campi SQL_DESC_PRECISION e SQL_DESC_SCALE del descrittore dell'applicazione.  
  
 [g] il campo del segno è 1 se positivo, 0 se negativo.  
  
 [h] _int64 potrebbe non essere fornito da alcuni compilatori.  
  
 [i] _SQL_C_BOOKMARK è stato deprecato in ODBC 3 *. x*.  
  
 [j] _SQL_C_SHORT, SQL_C_LONG e SQL_C_TINYINT sono stati sostituiti in ODBC da tipi firmati e non firmati: SQL_C_SSHORT e SQL_C_USHORT, SQL_C_SLONG e SQL_C_ULONG e SQL_C_STINYINT e SQL_C_UTINYINT. Driver ODBC 3 *. x* che dovrebbe funzionare con ODBC 2. le applicazioni *x* devono supportare SQL_C_SHORT, SQL_C_LONG e SQL_C_TINYINT, perché quando vengono chiamate, gestione driver li passa al driver.  
  
 [k] SQL_C_GUID può essere convertito solo in SQL_CHAR o SQL_WCHAR.  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Strutture di interi a 64 bit](../../../odbc/reference/appendixes/64-bit-integer-structures.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Tipi di dati C in ODBC](../../../odbc/reference/develop-app/c-data-types-in-odbc.md)
