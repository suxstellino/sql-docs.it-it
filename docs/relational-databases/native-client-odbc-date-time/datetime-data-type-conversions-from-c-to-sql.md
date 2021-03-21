---
description: Conversioni dei tipi di dati datetime da C a SQL
title: Conversioni da C a SQL | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- conversions [ODBC], C to SQL
ms.assetid: 7ac098db-9147-4883-8da9-a58ab24a0d31
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 89c0627ff26cd5d12aa60b2a25f4ab52d9646695
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756111"
---
# <a name="datetime-data-type-conversions-from-c-to-sql"></a>Conversioni dei tipi di dati datetime da C a SQL
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Questo argomento elenca i problemi da considerare quando si esegue la conversione da tipi C a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipi di data/ora.  
  
 Le conversioni descritte nella tabella seguente sono valide se effettuate sul client. Nei casi in cui il client specifica la precisione in secondi frazionari per un parametro che differisce da quello definito nel server, la conversione client potrebbe avere esito positivo, ma il server restituirà un errore quando viene chiamato **SQLExecute** o **SQLExecuteDirect** . In particolare, ODBC considera il troncamento dei secondi frazionari come un errore, mentre il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comportamento viene arrotondato. ad esempio, l'arrotondamento si verifica quando si passa da **datetime2 (6)** a **datetime2 (2)**. I valori della colonna di tipo datetime vengono arrotondati a 1/300° di un secondo e le colonne smalldatetime contengono secondi impostati su zero dal server.  
  
|   | SQL_TYPE_DATE | SQL_TYPE_TIME | SQL_SS_TIME2 | SQL_TYPE_TIMESTAMP | SQL_SS_TIMSTAMPOFFSET | SQL_CHAR | SQL_WCHAR |
| - | ------------- | ------------- | ------------ | ------------------ | --------------------- | -------- | --------- |
| **SQL_C_DATE** |1|-|-|1,6|1,5,6|1,13|1,13|  
| **SQL_C_TIME** |-|1|1|1,7|1, 5, 7|1,13|1,13|  
| **SQL_C_SS_TIME2** |-|1,3|1,10|1,7|1, 5, 7|1,13|1,13|  
| **SQL_C_BINARY(SQL_SS_TIME2_STRUCT)** |N/D|N/D|1,10,11|N/D|N/D|N/D|N/D|  
| **SQL_C_TYPE_TIMESTAMP** |1,2|1,3,4|1,4,10|1,10|1,5,10|1,13|1,13|  
| **SQL_C_SS_TIMESTAMPOFFSET** |1,2,8|1,3,4,8|1,4,8,10|1,8,10|1,10|1,13|1,13|  
| **SQL_C_BINARY(SQL_SS_TIMESTAMPOFFSET_STRUCT)** |N/D|N/D|N/D|N/D|1,10,11|N/D|N/D|  
| **SQL_C_CHAR/SQL_WCHAR (date)** |9|9|9|9,6|9,5,6|N/D|N/D|  
| **SQL_C_CHAR/SQL_WCHAR (time2)** |9|9, 3|9,10|9,7,10|9,5,7,10|N/D|N/D|  
| **SQL_C_CHAR/SQL_WCHAR (datetime)** |9,2|9, 3, 4|9,4,10|9,10|9,5,10|N/D|N/D|  
| **SQL_C_CHAR/SQL_WCHAR (datetimeoffset)** |9,2,8|9, 3, 4, 8|9,4,8,10|9,8,10|9,10|N/D|N/D|  
| **SQL_C_BINARY(SQL_DATE_STRUCT)** |1,11|N/D|N/D|N/D|N/D|N/D|N/D|  
| **SQL_C_BINARY(SQL_TIME_STRUCT)** |N/D|N/D|N/D|N/D|N/D|N/D|N/D|  
| **SQL_C_BINARY(SQL_TIMESTAMP_STRUCT)** |N/D|N/D|N/D|N/D|N/D|N/D|N/D|  
  
## <a name="key-to-symbols"></a>Descrizione dei simboli  
  
-   **-**: Nessuna conversione supportata. Viene generato un record di diagnostica con SQLSTATE 07006 e il messaggio "Violazione dell'attributo del tipo di dati".  
  
-   **1**: se i dati forniti non sono validi, viene generato un record di diagnostica con SQLSTATE 22007 e il messaggio "formato DateTime non valido".  
  
-   **2**: i campi temporali devono essere zero o viene generato un record di diagnostica con SQLSTATE 22008 e il messaggio "troncamento frazionario".  
  
-   **3**: i secondi frazionari devono essere zero o viene generato un record di diagnostica con SQLSTATE 22008 e il messaggio "troncamento frazionario".  
  
-   **4**: il componente relativo alla data viene ignorato.  
  
-   **5**: il fuso orario viene impostato sull'impostazione del fuso orario del client.  
  
-   **6**: l'ora è impostata su zero.  
  
-   **7**: la data è impostata sulla data corrente.  
  
-   **8**: l'ora viene convertita dal fuso orario del client in ora UTC. Se si verifica un errore durante questa conversione, viene generato un record di diagnostica con l'identificativo SQLSTATE 22008 e il messaggio "Overflow del campo Datetime".  
  
-   **9**: la stringa viene analizzata e convertita in un valore date, DateTime, DateTimeOffset o Time, a seconda del primo carattere di punteggiatura rilevato e della presenza di componenti rimanenti. La stringa viene quindi convertita nel tipo di destinazione, seguendo le regole nella tabella precedente per il tipo di origine individuato da questo processo. Se durante l'analisi dei dati viene rilevato un errore, viene generato un record di diagnostica con SQLSTATE 22018 e il messaggio "Carattere non valido per la specifica del cast". Per i parametri datetime e smalldatetime, se l'anno non è compreso nell'intervallo supportato da questi tipi, viene generato un record di diagnostica con SQLSTATE 22007 e il messaggio "Formato di datetime non valido".  
  
     Per datetimeoffset, il valore deve essere compreso nell'intervallo supportato in seguito alla conversione in UTC, anche se non è necessaria alcuna conversione in UTC. Ciò è dovuto al fatto che TDS e il server normalizzano sempre l'ora in valori datetimeoffset per UTC. Il client deve pertanto verificare che i componenti relativi all'ora siano compresi nell'intervallo supportato in seguito alla conversione in UTC. Se il valore non è compreso nell'intervallo UTC supportato, viene generato un record di diagnostica con SQLSTATE 22007 e il messaggio "Formato di datetime non valido".  
  
-   **10**: se si verifica un troncamento con perdita di dati, viene generato un record di diagnostica con SQLSTATE 22008 e il messaggio "formato ora non valido". Questo errore si verifica anche se il valore non è incluso nell'intervallo che può essere rappresentato dall'intervallo UTC utilizzato dal server.  
  
-   **11**: se la lunghezza in byte dei dati non è uguale alla dimensione dello struct richiesta dal tipo SQL, viene generato un record di diagnostica con SQLSTATE 22003 e il messaggio "valore numerico non compreso nell'intervallo".  
  
-   **12**: se la lunghezza in byte dei dati è 4 o 8, i dati vengono inviati al server in formato smalldatetime o DateTime non elaborato. Se la lunghezza in byte dei dati corrisponde esattamente alla dimensione di SQL_TIMESTAMP_STRUCT, i dati vengono convertiti nel formato TDS per datetime2.  
  
-   **13**: se si verifica un troncamento con perdita di dati, viene generato un record di diagnostica con SQLState 22001 e il messaggio "dati stringa, troncati a destra".  
  
     Il numero di cifre per i secondi frazionari (scala) è determinato dalla dimensione della colonna di destinazione in base alla tabella seguente:  
  
    |   | Scala implicita | Scala implicita |
    | - | ------------- | ------------- |
    | **Tipo** | 0 | 1.. 9 |  
    |**SQL_C_TYPE_TIMESTAMP** |19|21..29|  
  
     Per SQL_C_TYPE_TIMESTAMP, tuttavia, se i secondi frazionari possono essere rappresentati con tre cifre senza perdita di dati e la dimensione della colonna è almeno 23, vengono generate esattamente tre cifre di secondi frazionari. Questo comportamento assicura la compatibilità con le versioni precedenti per le applicazioni sviluppate utilizzando driver ODBC meno recenti.  
  
     Per dimensioni di colonna maggiori dell'intervallo specificato nella tabella, si presuppone una scala 9. Questa conversione deve consentire fino a nove cifre per i secondi frazionari, il massimo consentito da ODBC.  
  
     Una dimensione della colonna pari a zero implica una dimensione illimitata per i tipi di carattere a lunghezza variabile in ODBC (9 cifre, a meno che non si applichi la regola delle 3 cifre per SQL_C_TYPE_TIMESTAMP). La specifica di una dimensione della colonna pari a zero con un tipo di carattere a lunghezza fissa è un errore.  
  
-   **N/A**: [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] viene mantenuto il comportamento esistente e quello precedente.  
  
## <a name="see-also"></a>Vedere anche  
 [Miglioramenti di data e ora &#40;ODBC&#41;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)  
  
  
