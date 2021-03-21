---
description: SQLDescribeParam
title: SQLDescribeParam | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLDescribeParam function
ms.assetid: 396e74b1-5d08-46dc-b404-2ef2003e4689
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 06a173317a6539f7419976b4916effaaf93fca2c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753381"
---
# <a name="sqldescribeparam"></a>SQLDescribeParam
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Per descrivere i parametri di qualsiasi istruzione SQL, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native client compila ed esegue un' [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzione SELECT quando SQLDescribeParam viene chiamato su un handle di istruzione ODBC preparata. I metadati del set di risultati determinano le caratteristiche dei parametri dell'istruzione preparata. SQLDescribeParam può restituire qualsiasi codice di errore che SQLExecute o SQLExecDirect può restituire.  
  
 I miglioramenti apportati al motore di database a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] consentono a SQLDescribeParam di ottenere descrizioni più accurate dei risultati previsti. Questi risultati più accurati possono essere diversi dai valori restituiti da SQLDescribeParam nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Metadata Discovery](../../relational-databases/native-client/features/metadata-discovery.md).  
  
 Inoltre [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] , una novità di, *ParameterSizePtr* restituisce ora un valore che viene allineato alla definizione per la dimensione, in caratteri, della colonna o dell'espressione del marcatore di parametro corrispondente come definito nella [specifica ODBC](../../odbc/reference/appendixes/column-size.md). Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] native client, *ParameterSizePtr* potrebbe essere il valore corrispondente di **SQL_DESC_OCTET_LENGTH** per il tipo o un valore di dimensione della colonna irrilevante fornito a SQLBindParameter per un tipo, il cui valore deve essere ignorato (ad esempio **SQL_INTEGER**).  
  
 Il driver non supporta la chiamata a SQLDescribeParam nelle situazioni seguenti:  
  
-   Dopo SQLExecDirect per le [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzioni Update o DELETE contenenti la clausola from.  
  
-   Per qualsiasi istruzione ODBC o [!INCLUDE[tsql](../../includes/tsql-md.md)] contenente un parametro in una clausola HAVING o confrontata con il risultato di una funzione SUM.  
  
-   Per qualsiasi istruzione ODBC o [!INCLUDE[tsql](../../includes/tsql-md.md)] a seconda di una subquery che contiene parametri.  
  
-   Per le istruzioni ODBC SQL che contengono marcatori di parametro in entrambe le espressioni di un predicato di confronto, LIKE o quantificato.  
  
-   Per qualsiasi query in cui uno dei parametri è il parametro di una funzione.  
  
-   Quando sono presenti commenti (/* \* /) nel [!INCLUDE[tsql](../../includes/tsql-md.md)] comando.  
  
 Quando si elabora un batch di [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzioni, il driver non supporta inoltre la chiamata a SQLDescribeParam per i marcatori di parametro nelle istruzioni dopo la prima istruzione nel batch.  
  
 Quando si descrivono i parametri delle stored procedure preparate, SQLDescribeParam usa il [sp_sproc_columns](../../relational-databases/system-stored-procedures/sp-sproc-columns-transact-sql.md) di stored procedure di sistema per recuperare le caratteristiche dei parametri. sp_sproc_columns possibile segnalare i dati per le stored procedure all'interno del database utente corrente. La preparazione di un nome completo stored procedure consente l'esecuzione di SQLDescribeParam tra database. Ad esempio, il [sp_who](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md) di stored procedure di sistema può essere preparato ed eseguito in qualsiasi database come:  
  
```  
SQLPrepare(hstmt, "{call sp_who(?)}", SQL_NTS);  
```  
  
 L'esecuzione di SQLDescribeParam dopo il completamento della preparazione restituisce un set di righe vuoto in caso di connessione a un database **Master**. La stessa chiamata, preparata come indicato di seguito, fa sì che SQLDescribeParam abbia esito positivo indipendentemente dal database utente corrente:  
  
```  
SQLPrepare(hstmt, "{call master..sp_who(?)}", SQL_NTS);  
```  
  
 Per i tipi di dati con valori di grandi dimensioni, il valore restituito in *DataTypePTR* è SQL_VARCHAR, SQL_VARBINARY o SQL_NVARCHAR. Per indicare che la dimensione del parametro del tipo di dati per valori di grandi dimensioni è "illimitata", il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native Client imposta *ParameterSizePtr* su 0. I valori delle dimensioni effettive vengono restituiti per i parametri **varchar** standard.  
  
> [!NOTE]  
>  Se il parametro è già stato associato alle dimensioni massime del parametro SQL_VARCHAR, SQL_VARBINARY o SQL_WVARCHAR, vengono restituite le dimensioni associate limitate del parametro.  
  
 Per associare un parametro di input di dimensioni illimitate, è necessario utilizzare data-at-execution. Non è possibile associare un parametro di output di dimensioni illimitate. non esiste alcun metodo per lo streaming dei dati da un parametro di output, [ad esempio per](../../relational-databases/native-client-odbc-api/sqlgetdata.md) i set di risultati.  
  
 Per i parametri di output è necessario associare un buffer. Se il valore è troppo grande, il buffer viene riempito e vengono restituiti un messaggio SQL_SUCCESS_WITH_INFO e un avviso "Troncamento a destra della stringa di dati". I dati troncati verranno quindi eliminati.  
  
## <a name="sqldescribeparam-and-table-valued-parameters"></a>SQLDescribeParam e parametri con valori di tabella  
 Un'applicazione può recuperare informazioni sui parametri con valori di tabella per un'istruzione preparata con SQLDescribeParam. Per ulteriori informazioni, vedere [metadati dei parametri con valori di tabella per le istruzioni preparate](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameter-metadata-for-prepared-statements.md).  
  
 Per ulteriori informazioni sui parametri con valori di tabella in generale, vedere [parametri con valori di tabella &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="sqldescribeparam-support-for-enhanced-date-and-time-features"></a>Supporto di SQLDescribeParam per le caratteristiche avanzate di data e ora  
 I valori restituiti per i tipi di data/ora sono i seguenti:  
  
| Attributo | *DataTypePtr* | *ParameterSizePtr* | *DecimalDigitsPtr* |  
| --------- | ------------- | ------------------ | ------------------ |  
|Datetime|SQL_TYPE_TIMESTAMP|23|3|  
|smalldatetime|SQL_TYPE_TIMESTAMP|16|0|  
|Data|SQL_TYPE_DATE|10|0|  
|time|SQL_SS_TIME2|8, 10..16|0..7|  
|datetime2|SQL_TYPE_TIMESTAMP|19, 21..27|0..7|  
|datetimeoffset|SQL_SS_TIMESTAMPOFFSET|26, 28..34|0..7|  
  
 Per ulteriori informazioni, vedere [miglioramenti di data e ora &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqldescribeparam-support-for-large-clr-udts"></a>Supporto di SQLDescribeParam per i tipi CLR definiti dall'utente di grandi dimensioni  
 **SQLDescribeParam** supporta i tipi CLR definiti dall'utente di grandi dimensioni. Per ulteriori informazioni, vedere [tipi CLR User-Defined di grandi dimensioni &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="see-also"></a>Vedere anche  
 [SQLDescribeParam (funzione)](../../odbc/reference/syntax/sqldescribeparam-function.md)   
 [Dettagli di implementazione dell'API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
