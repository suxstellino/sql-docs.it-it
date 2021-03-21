---
description: SQLSetDescRec
title: SQLSetDescRec | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLSetDescRec function
ms.assetid: 203d02a2-aa09-462b-a489-a2cdd6f6023b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f4745fedbcba60d449d22c84e0e2a07b6728eeb2
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754881"
---
# <a name="sqlsetdescrec"></a>SQLSetDescRec
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  In questo argomento vengono illustrate le funzionalità di SQLSetDescRec specifiche di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
## <a name="sqlsetdescrec-and-table-valued-parameters"></a>SQLSetDescRec e parametri con valori di tabella  
 SQLSetDescRec può essere utilizzato per impostare i campi di descrizione per i parametri con valori di tabella e le colonne dei parametri con valori di tabella. Le colonne dei parametri con valori di tabella sono disponibili solo quando il campo di intestazione di descrizione SQL_SOPT_SS_PARAM_FOCUS è impostato sul numero ordinale di un record in cui SQL_DESC_TYPE è impostato su SQL_SS_TABLE. Per ulteriori informazioni su SQL_SOPT_SS_PARAM_FOCUS, vedere [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md).  
  
 Nella tabella seguente viene descritto il mapping tra parametri e campi di descrizione.  
  
|Parametro|Attributo correlato per i tipi di parametro non con valori di tabella, incluse le colonne di parametri con valori di tabella|Attributo correlato per i parametri con valori di tabella|  
|---------------|--------------------------------------------------------------------------------------------------------|----------------------------------------------------|  
|*Tipo*|SQL_DESC_TYPE|SQL_SS_TABLE|  
|*Sottotipo*|Ignorato|Per i record di tipo SQL_DATETIME o SQL_INTERVAL, impostare su SQL_DESC_DATETIME_INTERVAL_CODE.|  
|*Length*|SQL_DESC_OCTET_LENGTH|Lunghezza del nome del tipo di parametro con valori di tabella. Può essere SQL_NTS, se il nome del tipo è con terminazione Null oppure zero se il nome del tipo di parametro con valori di tabella non è obbligatorio.|  
|*Precisione*|SQL_DESC_PRECISION|SQL_DESC_ARRAY_SIZE|  
|*Ridimensiona*|SQL_DESC_SCALE|Non utilizzato. Questo parametro deve essere zero.|  
|*DataPtr*|SQL_DESC_DATA_PTR in APD|SQL_CA_SS_TYPE_NAME<br /><br /> Questo parametro è facoltativo per le chiamate di stored procedure ed è possibile specificare NULL se non è obbligatorio. È necessario specificarlo per istruzioni SQL che non sono chiamate di procedure.<br /><br /> *DataPtr* funge anche da valore univoco che l'applicazione può utilizzare per identificare questo parametro con valori di tabella quando viene utilizzata l'associazione di righe variabile.|  
|*StringLengthPtr*|SQL_DESC_OCTET_LENGTH_PTR|SQL_DESC_OCTET_LENGTH_PTR<br /><br /> Per un parametro con valori di tabella, è il numero di righe da trasferire o SQL_DATA_AT_EXEC.SQL_DATA_AT_EXEC. Si tratta di un puntatore a un valore che include il numero di righe da trasferire con SQLExecDirect.|  
|*IndicatorPtr*|SQL_DESC_INDICATOR_PTR|SQL_DESC_INDICATOR_PTR|  
  
 Per ulteriori informazioni sui parametri con valori di tabella, vedere [parametri con valori di tabella &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="sqlsetdescrec-support-for-enhanced-date-and-time-features"></a>Supporto di SQLSetDescRec per le caratteristiche avanzate di data e ora  
 I valori consentiti per i tipi di data/ora sono i seguenti:  
  
| Attributo | *Tipo* | *Sottotipo* | *Length* | *Precisione* | *Ridimensionamento* |
| --------- | ------ | --------- | -------- | ----------- | ------- |
|Datetime|SQL_DATETIME|SQL_CODE_TIMESTAMP|4|3|3|  
|smalldatetime|SQL_SQL_DATETIME|SQL_CODE_TIMESTAMP|8|0|0|  
|Data|SQL_DATETIME|SQL_CODE_DATE|6|0|0|  
|time|SQL_SS_TIME2|0|10|0..7|0..7|  
|datetime2|SQL_DATETIME|SQL_CODE_TIMESTAMP|16|0..7|0..7|  
|datetimeoffset|SQL_SS_TIMESTAMPOFFSET|0|20|0..7|0..7|  
  
 Per ulteriori informazioni, vedere [miglioramenti di data e ora &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqlsetdescrec-support-for-large-clr-udts"></a>Supporto di SQLSetDescRec per tipi definiti dall'utente CLR di grandi dimensioni  
 **SQLSetDescRec** supporta i tipi CLR definiti dall'utente di grandi dimensioni. Per ulteriori informazioni, vedere [tipi CLR User-Defined di grandi dimensioni &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="see-also"></a>Vedere anche  
 [SQLSetDescRec](../../odbc/reference/syntax/sqlsetdescrec-function.md)   
 [Dettagli di implementazione dell'API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
