---
description: Tipi di dati SQL
title: Tipi di dati SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQL data types [ODBC]
- SQL data types [ODBC], about SQL data types
- data types [ODBC], SQL data types
ms.assetid: 1b22f985-f5e4-4779-87eb-e43329a442b1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e15771f52af0830f07f135453d08f4355a3fae8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199872"
---
# <a name="sql-data-types"></a>Tipi di dati SQL
Ogni DBMS definisce tipi SQL personalizzati. Ogni driver ODBC espone solo i tipi di dati SQL definiti dal DBMS associato. Informazioni sul modo in cui un driver esegue il mapping dei tipi SQL DBMS agli identificatori di tipo SQL definiti da ODBC e sul modo in cui un driver esegue il mapping dei tipi SQL DBMS ai propri identificatori di tipo SQL specifici del driver viene restituito tramite una chiamata a **SQLGetTypeInfo**. Un driver restituisce anche i tipi di dati SQL quando si descrivono i tipi di dati di colonne e parametri tramite chiamate a **SQLColAttribute**, **SQLColumns**, **SQLDescribeCol**, **SQLDescribeParam**, **SQLProcedureColumns** e **SQLSpecialColumns**.  
  
> [!NOTE]  
>  I tipi di dati SQL sono contenuti nei campi SQL_DESC_ CONCISE_TYPE, SQL_DESC_TYPE e SQL_DESC_DATETIME_INTERVAL_CODE dei descrittori di implementazione. Le caratteristiche dei tipi di dati SQL sono contenute nei campi SQL_DESC_PRECISION, SQL_DESC_SCALE, SQL_DESC_LENGTH e SQL_DESC_OCTET_LENGTH dei descrittori di implementazione. Per ulteriori informazioni, vedere [identificatori e descrittori di tipi di dati](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md) più avanti in questa appendice.  
  
 Un driver e un'origine dati specificati non supportano necessariamente tutti i tipi di dati SQL definiti in questa appendice. Il supporto di un driver per i tipi di dati SQL dipende dal livello di SQL-92 a cui è conforme il driver. Per determinare il livello di grammatica SQL-92 supportato dal driver, un'applicazione chiama **SQLGetInfo** con il tipo di informazioni SQL_SQL_CONFORMANCE. Inoltre, un determinato driver e un'origine dati possono supportare tipi di dati SQL aggiuntivi specifici del driver. Per determinare i tipi di dati supportati da un driver, un'applicazione chiama **SQLGetTypeInfo**. Per informazioni sui tipi di dati SQL specifici del driver, vedere la documentazione del driver. Per informazioni sui tipi di dati in un'origine dati specifica, vedere la documentazione relativa a tale origine dati.  
  
> [!IMPORTANT]  
>  Le tabelle in questa appendice sono solo linee guida e mostrano i nomi, gli intervalli e i limiti dei tipi di dati SQL utilizzati in genere. Una determinata origine dati potrebbe supportare solo alcuni dei tipi di dati elencati e le caratteristiche dei tipi di dati supportati possono essere diverse da quelle elencate.  
  
 Nella tabella seguente sono elencati gli identificatori di tipo SQL validi per tutti i tipi di dati SQL. Nella tabella sono inoltre elencati il nome e la descrizione del tipo di dati corrispondente da SQL-92 (se presente).  
  
|Identificatore di tipo SQL [1]|Dati SQL tipici<br /><br /> tipo [2]|Descrizione tipica del tipo|  
|------------------------------|------------------------------------|------------------------------|  
|SQL_CHAR|CHAR (*n*)|Stringa di caratteri di lunghezza stringa fissa *n*.|  
|SQL_VARCHAR|VARCHAR (*n*)|Stringa di caratteri a lunghezza variabile con lunghezza massima della stringa *n*.|  
|SQL_LONGVARCHAR|LONG VARCHAR|Dati di caratteri a lunghezza variabile. La lunghezza massima dipende dall'origine dati. 9|  
|SQL_WCHAR|WCHAR (*n*)|Stringa di caratteri Unicode con lunghezza stringa fissa *n*|  
|SQL_WVARCHAR|VARWCHAR (*n*)|Stringa di caratteri a lunghezza variabile Unicode con lunghezza massima della stringa *n*|  
|SQL_WLONGVARCHAR|LONGWVARCHAR|Dati di caratteri a lunghezza variabile Unicode. La lunghezza massima dipende dall'origine dati|  
|SQL_DECIMAL|DECIMAL (*p*,*s*)|Valore numerico esatto con segno, precisione almeno *p* e scala *s.* (La precisione massima è definita dal driver). (1 <= *p* <= 15; *s*  <=  *p*). 4|  
|SQL_NUMERIC|NUMERICO (*p*,*s*)|Valore numerico esatto con segno di precisione *p* e scala *s* (1 <= *p* <= 15; *s*  <=  *p*). 4|  
|SQL_SMALLINT|SMALLINT|Valore numerico esatto con precisione 5 e scala 0 (con segno:-32.768 <= *n* <= 32.767, senza segno: 0 <= *n* <= 65535) [3].|  
|SQL_INTEGER|INTEGER|Valore numerico esatto con precisione 10 e scala 0 (con segno:-2 [31] <= *n* <= 2 [31]-1, senza segno: 0 <= *n* <= 2 [32]-1) [3].|  
|SQL_REAL|REAL|Valore numerico con segno, approssimativo con una precisione binaria 24 (zero o valore assoluto 10 [-38] a 10 [38]).|  
|SQL_FLOAT|FLOAT (*p*)|Valore numerico con segno, approssimativo con una precisione binaria di almeno *p*. (La precisione massima è definita dal driver). 5|  
|SQL_DOUBLE|DOUBLE PRECISION|Valore numerico con segno, approssimativo con una precisione binaria 53 (zero o valore assoluto 10 [-308] a 10 [308]).|  
|SQL_BIT|BIT|Dati binari a bit singolo. 8|  
|SQL_TINYINT|TINYINT|Valore numerico esatto con precisione 3 e scala 0 (con segno:-128 <= *n* <= 127, senza segno: 0 <= *n* <= 255) [3].|  
|SQL_BIGINT|bigint|Valore numerico esatto con precisione 19 (se firmato) o 20 (se senza segno) e scala 0 (con segno:-2 [63] <= *n* <= 2 [63]-1, senza segno: 0 <= *n* <= 2 [64]-1) [3], [9].|  
|SQL_BINARY|BINARIO (*n*)|Dati binari di lunghezza fissa *n*. 9|  
|SQL_VARBINARY|VARBINARY (*n*)|Dati binari a lunghezza variabile con lunghezza massima *n*. Il valore massimo è impostato dall'utente. 9|  
|SQL_LONGVARBINARY|LONG (VARBINARY)|Dati binari a lunghezza variabile. La lunghezza massima dipende dall'origine dati. 9|  
|SQL_TYPE_DATE [6]|DATE|Campi anno, mese e giorno, conformi alle regole del calendario gregoriano. Vedere [vincoli del calendario gregoriano](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md), più avanti in questa appendice.|  
|SQL_TYPE_TIME [6]|ORA (*p*)|Campi di ora, minuti e secondi, con valori validi per le ore da 00 a 23, valori validi per i minuti da 00 a 59 e valori validi per i secondi da 00 a 61. La precisione *p* indica la precisione dei secondi.|  
|SQL_TYPE_TIMESTAMP [6]|TIMESTAMP (*p*)|Campi di anno, mese, giorno, ora, minuto e secondo con valori validi definiti per i tipi di dati di data e ora.|  
|SQL_TYPE_UTCDATETIME|UTCDATETIME|Campi anno, mese, giorno, ora, minuti, secondi, utchour e utcminute. I campi utchour e utcminute hanno una precisione di 1/10 microsecondi.|  
|SQL_TYPE_UTCTIME|UTCTIME|Campi hour, minute, Second, utchour e utcminute. I campi utchour e utcminute hanno una precisione di 1/10 microsecondi.|  
|SQL_INTERVAL_MONTH [7]|MESE intervallo (*p*)|Numero di mesi tra due date. *p* è la precisione principale dell'intervallo.|  
|SQL_INTERVAL_YEAR [7]|ANNO intervallo (*p*)|Numero di anni tra due date. *p* è la precisione principale dell'intervallo.|  
|SQL_INTERVAL_YEAR_TO_MONTH [7]|ANNO intervallo (*p*) al mese|Numero di anni e mesi tra due date. *p* è la precisione principale dell'intervallo.|  
|SQL_INTERVAL_DAY [7]|GIORNO intervallo (*p*)|Numero di giorni tra due date. *p* è la precisione principale dell'intervallo.|  
|SQL_INTERVAL_HOUR [7]|ORA intervallo (*p*)|Numero di ore tra due data/ora; *p* è la precisione principale dell'intervallo.|  
|SQL_INTERVAL_MINUTE [7]|MINUTI intervallo (*p*)|Numero di minuti tra due data/ora; *p* è la precisione principale dell'intervallo.|  
|SQL_INTERVAL_SECOND [7]|SECONDO intervallo (*p*,*q*)|Numero di secondi tra due data/ora; *p* è la precisione massima dell'intervallo e *q* è la precisione dell'intervallo di secondi.|  
|SQL_INTERVAL_DAY_TO_HOUR [7]|INTERVALLO giorno (*p*) all'ora|Numero di giorni/ore tra due data/ora; *p* è la precisione principale dell'intervallo.|  
|SQL_INTERVAL_DAY_TO_MINUTE [7]|INTERVALLO giorno (*p*) al minuto|Numero di giorni/ore/minuti tra due data/ora; *p* è la precisione principale dell'intervallo.|  
|SQL_INTERVAL_DAY_TO_SECOND [7]|INTERVALLO giorno (*p*) al secondo (*q*)|Numero di giorni/ore/minuti/secondi tra due data/ora; *p* è la precisione massima dell'intervallo e *q* è la precisione dell'intervallo di secondi.|  
|SQL_INTERVAL_HOUR_TO_MINUTE [7]|ORA intervallo (*p*) al minuto|Numero di ore/minuti tra due data/ora; *p* è la precisione principale dell'intervallo.|  
|SQL_INTERVAL_HOUR_TO_SECOND [7]|ORA intervallo (*p*) al secondo (*q*)|Numero di ore/minuti/secondi tra due data/ora; *p* è la precisione massima dell'intervallo e *q* è la precisione dell'intervallo di secondi.|  
|SQL_INTERVAL_MINUTE_TO_SECOND [7]|INTERVALLO minuti (*p*) al secondo (*q*)|Numero di minuti/secondi tra due data/ora; *p* è la precisione massima dell'intervallo e *q* è la precisione dell'intervallo di secondi.|  
|SQL_GUID|GUID|GUID a lunghezza fissa.|  
  
 [1] valore restituito nella colonna DATA_TYPE da una chiamata a **SQLGetTypeInfo**.  
  
 [2] valore restituito nella colonna nome e crea PARAMs mediante una chiamata a **SQLGetTypeInfo**. La colonna nome restituisce la designazione, ad esempio CHAR, mentre la colonna CREATE PARAMs restituisce un elenco delimitato da virgole di parametri di creazione quali precisione, scala e lunghezza.  
  
 [3] un'applicazione utilizza **SQLGetTypeInfo** o **SQLColAttribute** per determinare se un particolare tipo di dati o una particolare colonna in un set di risultati è senza segno.  
  
 [4] SQL_DECIMAL e i tipi di dati di SQL_NUMERIC differiscono solo per la precisione. La precisione di un numero decimale (*p*,*s*) è una precisione decimale definita dall'implementazione che non è minore di *p*, mentre la precisione di un valore numerico (*p*,*s*) è esattamente uguale a *p*.  
  
 [5] a seconda dell'implementazione, la precisione del SQL_FLOAT può essere 24 o 53: se è 24, il tipo di dati SQL_FLOAT è identico a quello SQL_REAL; Se è 53, il tipo di dati SQL_FLOAT corrisponde a quello SQL_DOUBLE.  
  
 [6] in ODBC *3. x*, i tipi di dati di data, ora e timestamp SQL sono rispettivamente SQL_TYPE_DATE, SQL_TYPE_TIME e SQL_TYPE_TIMESTAMP. in ODBC *2. x* i tipi di dati sono SQL_DATE, SQL_TIME e SQL_TIMESTAMP.  
  
 [7] per ulteriori informazioni sui tipi di dati interval SQL, vedere la sezione relativa ai [tipi di dati interval](../../../odbc/reference/appendixes/interval-data-types.md) , più avanti in questa appendice.  
  
 [8] il tipo di dati SQL_BIT ha caratteristiche diverse rispetto al tipo di BIT in SQL-92.  
  
 [9] questo tipo di dati non ha un tipo di dati corrispondente in SQL-92.  
  
 Questa sezione illustra l'esempio seguente.  
  
-   [Esempio di set di risultati SQLGetTypeInfo](../../../odbc/reference/appendixes/example-sqlgettypeinfo-result-set.md)
