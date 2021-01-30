---
description: Funzioni di data, ora e intervallo
title: Funzioni di ora, data e intervallo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- functions [ODBC], time functions
- functions [ODBC], date functions
- interval functions [ODBC]
- functions [ODBC], interval functions
- time functions [ODBC]
- date functions [ODBC]
ms.assetid: bdf054a0-7aba-4e99-a34a-799917376fd5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e73d295613f3f11b72317d20b49a6c75be802cbd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202500"
---
# <a name="time-date-and-interval-functions"></a>Funzioni di data, ora e intervallo
Nella tabella seguente sono elencate le funzioni di data e ora incluse nel set di funzioni scalari ODBC. Un'applicazione può determinare quali funzioni di data e ora sono supportate da un driver chiamando **SQLGetInfo** con un *tipo di informazioni* SQL_TIMEDATE_FUNCTIONS.  
  
 Gli argomenti identificati come *timestamp_exp* possono essere il nome di una colonna, il risultato di un'altra funzione scalare o ODBC- *Time-* Escape, *ODBC-date-Escape* o *ODBC-timestamp-Escape*, in cui il tipo di dati sottostante potrebbe essere rappresentato come SQL_CHAR, SQL_VARCHAR, SQL_TYPE_TIME, SQL_TYPE_DATE o SQL_TYPE_TIMESTAMP.  
  
 Gli argomenti identificati come *date_exp* possono essere il nome di una colonna, il risultato di un'altra funzione scalare o ODBC *-date-Escape* o *ODBC-timestamp-Escape*, in cui il tipo di dati sottostante potrebbe essere rappresentato come SQL_CHAR, SQL_VARCHAR, SQL_TYPE_DATE o SQL_TYPE_TIMESTAMP.  
  
 Gli argomenti identificati come *time_exp* possono essere il nome di una colonna, il risultato di un'altra funzione scalare o ODBC *-Time-Escape* o *ODBC-timestamp-Escape*, in cui il tipo di dati sottostante potrebbe essere rappresentato come SQL_CHAR, SQL_VARCHAR, SQL_TYPE_TIME o SQL_TYPE_TIMESTAMP.  
  
 Le funzioni scalari CURRENT_DATE, CURRENT_TIME e CURRENT_TIMESTAMP sono state aggiunte in ODBC 3,0 per essere allineate con SQL-92.  
  
|Funzione|Descrizione|  
|--------------|-----------------|  
|**CURRENT_DATE ()** (ODBC 3,0)|Restituisce la data corrente.|  
|**CURRENT_TIME [(** *precisione temporale* **)]** (ODBC 3,0)|Restituisce l'ora locale corrente. L'argomento relativo alla *precisione temporale* determina la precisione dei secondi del valore restituito.|  
|**CURRENT_TIMESTAMP**<br /> **[(** *Precisione timestamp* **)]** (ODBC 3,0)|Restituisce la data locale corrente e l'ora locale come valore di timestamp. L'argomento *timestamp-Precision* determina la precisione dei secondi del timestamp restituito.|  
|**Cagliate ()** (ODBC 1,0)|Restituisce la data corrente.|  
|**CURTIME ()** (ODBC 1,0)|Restituisce l'ora locale corrente.|  
|**NomeGiorno (** *date_exp* **)** (ODBC 2,0)|Restituisce una stringa di caratteri contenente il nome specifico dell'origine dati del giorno, ad esempio da domenica a sabato o Dom. a Sat per un'origine dati che utilizza l'inglese o Sonntag tramite Samstag per un'origine dati che utilizza il tedesco) per la parte relativa al giorno di *date_exp*.|  
|**DAYOFMONTH (** *date_exp* **)** (ODBC 1,0)|Restituisce il giorno del mese in base al campo month in *date_exp* come valore intero nell'intervallo 1-31.|  
|**DayOfWeek (** *date_exp* **)** (ODBC 1,0)|Restituisce il giorno della settimana in base al campo della settimana in *date_exp* come valore intero nell'intervallo 1-7, dove 1 rappresenta domenica.|  
|**DayOfYear (** *date_exp* **)** (ODBC 1,0)|Restituisce il giorno dell'anno in base al campo Year nel *date_exp* come valore intero nell'intervallo 1-366.|  
|**Estrai (** *Estrai-campo da* *Extract-source* **)** (ODBC 3,0)|Restituisce la parte di *estrazione-campo* dell' *estrazione-origine*. L'argomento *Extract-source* è un'espressione datetime o Interval. L'argomento *Extract-Field* può essere una delle parole chiave seguenti:<br /><br /> ANNO MESE GIORNO ORA MINUTO SECONDO<br /><br /> La precisione del valore restituito è definita dall'implementazione. La scala è 0, a meno che non venga specificato il secondo, nel qual caso la scala non è inferiore alla precisione dei secondi frazionari del campo *Extract-source* .|  
|**Ora (** *time_exp* **)** (ODBC 1,0)|Restituisce l'ora in base al campo hour in *time_exp* come valore intero nell'intervallo 0-23.|  
|**Minuto (** *time_exp* **)** (ODBC 1,0)|Restituisce il minuto in base al campo dei minuti in *time_exp* come valore intero nell'intervallo 0-59.|  
|**Mese (** *date_exp* **)** (ODBC 1,0)|Restituisce il mese in base al campo month in *date_exp* come valore intero nell'intervallo 1-12.|  
|**MonthName (** *date_exp* **)** (ODBC 2,0)|Restituisce una stringa di caratteri contenente il nome specifico dell'origine dati del mese, ad esempio da gennaio a dicembre o da gennaio a dicembre. per un'origine dati che utilizza l'inglese o da gennaio tramite dicembre per un'origine dati che utilizza il tedesco, per la parte relativa al mese di *date_exp*.|  
|**Now ()** (ODBC 1,0)|Restituisce la data e l'ora correnti come valore di timestamp.|  
|**Trimestre (** *date_exp* **)** (ODBC 1,0)|Restituisce il trimestre *date_exp* come valore intero nell'intervallo 1-4, dove 1 corrisponde al 1 ° gennaio al 31 marzo.|  
|**Secondo (** *time_exp* **)** (ODBC 1,0)|Restituisce il secondo in base al secondo campo in *time_exp* come valore intero nell'intervallo 0-59.|
|**TIMESTAMPADD (** *intervallo*, *integer_exp*, *timestamp_exp* **)** (ODBC 2,0)|Restituisce il timestamp calcolato aggiungendo *integer_exp* intervalli di tipo *intervallo* a *timestamp_exp*. I valori validi di *Interval* sono le parole chiave seguenti:<br /><br /> SQL_TSI_FRAC_SECOND<br /><br /> SQL_TSI_SECOND<br /><br /> SQL_TSI_MINUTE<br /><br /> SQL_TSI_HOUR<br /><br /> SQL_TSI_DAY<br /><br /> SQL_TSI_WEEK<br /><br /> SQL_TSI_MONTH<br /><br /> SQL_TSI_QUARTER<br /><br /> SQL_TSI_YEAR<br /><br /> dove i secondi frazionari sono espressi in miliardesimi di secondo. Ad esempio, l'istruzione SQL seguente restituisce il nome di ogni dipendente e la relativa data di anniversario di un anno:<br /><br /> `SELECT NAME, {fn  TIMESTAMPADD(SQL_TSI_YEAR, 1, HIRE_DATE)} FROM  EMPLOYEES`<br /><br /> Se *timestamp_exp* è un valore di ora e l' *intervallo* specifica giorni, settimane, mesi, trimestri o anni, la parte relativa alla data di *timestamp_exp* viene impostata sulla data corrente prima di calcolare il timestamp risultante.<br /><br /> Se *timestamp_exp* è un valore di data e l' *intervallo* specifica i secondi frazionari, i secondi, i minuti o le ore, la parte relativa all'ora di *timestamp_exp* è impostata su 0 prima di calcolare il timestamp risultante.<br /><br /> Un'applicazione determina gli intervalli supportati da un'origine dati chiamando **SQLGetInfo** con l'opzione SQL_TIMEDATE_ADD_INTERVALS.|  
|**TIMESTAMPDIFF (** *intervallo*, *timestamp_exp1*, *timestamp_exp2* **)** (ODBC 2,0)|Restituisce il numero intero di intervalli di *intervallo* di tipo in base al quale *timestamp_exp2* è maggiore di *timestamp_exp1*. I valori validi di *Interval* sono le parole chiave seguenti:<br /><br /> SQL_TSI_FRAC_SECOND<br /><br /> SQL_TSI_SECOND<br /><br /> SQL_TSI_MINUTE<br /><br /> SQL_TSI_HOUR<br /><br /> SQL_TSI_DAY<br /><br /> SQL_TSI_WEEK<br /><br /> SQL_TSI_MONTH<br /><br /> SQL_TSI_QUARTER<br /><br /> SQL_TSI_YEAR<br /><br /> dove i secondi frazionari sono espressi in miliardesimi di secondo. Ad esempio, l'istruzione SQL seguente restituisce il nome di ogni dipendente e il numero di anni in cui è stato utilizzato:<br /><br /> `SELECT NAME, {fn  TIMESTAMPDIFF(SQL_TSI_YEAR, {fn CURDATE()}, HIRE_DATE)} FROM EMPLOYEES`<br /><br /> Se un'espressione timestamp è un valore di ora e l' *intervallo* specifica giorni, settimane, mesi, trimestri o anni, la parte relativa alla data di tale timestamp viene impostata sulla data corrente prima di calcolare la differenza tra i timestamp.<br /><br /> Se un'espressione timestamp è un valore di data e l' *intervallo* specifica secondi frazionari, secondi, minuti o ore, la parte relativa all'ora di tale timestamp viene impostata su 0 prima di calcolare la differenza tra i timestamp.<br /><br /> Un'applicazione determina gli intervalli supportati da un'origine dati chiamando **SQLGetInfo** con l'opzione SQL_TIMEDATE_DIFF_INTERVALS.|  
|**Settimana (** *date_exp* **)** (ODBC 1,0)|Restituisce la settimana dell'anno in base al campo della settimana in *date_exp* come valore intero nell'intervallo 1-53.|  
|**Year (** *date_exp* **)** (ODBC 1,0)|Restituisce l'anno in base al campo Year nel *date_exp* come valore integer. L'intervallo è dipendente dall'origine dati.|
