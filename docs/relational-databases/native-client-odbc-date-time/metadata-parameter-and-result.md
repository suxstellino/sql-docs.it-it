---
description: Metadati - Parametro e risultato
title: Metadati del parametro e del risultato | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- metadata [ODBC]
ms.assetid: 1518e6e5-a6a8-4489-b779-064c5624df53
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9824dbfcd31f4d18fc55e91016c604379d8569c9
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756021"
---
# <a name="metadata---parameter-and-result"></a>Metadati - Parametro e risultato
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  In questo argomento viene descritto ciò che viene restituito nel campo di descrizione del parametro di implementazione (IPD, Implementation Parameter Descriptor) e in quello di descrizione della riga di implementazione (IRD, Implementation Row Descriptor) per i tipi di dati date e time.  
  
## <a name="information-returned-in-ipd-fields"></a>Informazioni restituite nei campi IPD  
 Di seguito sono riportate le informazioni restituite nei campi IPD:  
  
|Tipo di parametro|Data|time|smalldatetime|Datetime|datetime2|datetimeoffset|  
|--------------------|----------|----------|-------------------|--------------|---------------|--------------------|  
|SQL_DESC_CASE_SENSITIVE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|  
|SQL_DESC_CONCISE_TYPE|SQL_TYPE_DATE|SQL_SS_TIME2|SQL_TYPE_TIMESTAMP|SQL_TYPE_TIMESTAMP|SQL_TYPE_TIMESTAMP|SQL_SS_TIMESTAMPOFFSET|  
|SQL_DESC_DATETIME_INTERVAL_CODE|SQL_CODE_DATE|0|SQL_CODE_TIMESTAMP|SQL_CODE_TIMESTAMP|SQL_CODE_TIMESTAMP|0|  
|SQL_DESC_DATETIME_INTERVAL_PRECISION|10|8, 10.. 16|16|23|19, 21..27|26, 28..34|  
|SQL_DESC_FIXED_PREC_SCALE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|  
|SQL_DESC_LENGTH|10|8, 10.. 16|16|23|19, 21..27|26, 28..34|  
|SQL_DESC_OCTET_LENGTH|6|12|4|8|16|20|  
|SQL_DESC_PRECISION|0|0..7|0|3|0..7|0..7|  
|SQL_DESC_SCALE|0|0..7|0|3|0..7|0..7|  
|SQL_DESC_TYPE|SQL_TYPE_DATE|SQL_SS_TYPE_TIME2|SQL_DATETIME|SQL_DATETIME|SQL_DATETIME|SQL_SS_TIMESTAMPOFFSET|  
|SQL_DESC_TYPE_NAME|**date**|**time**|**smalldatetime** in IRD, **datetime2** in dpi|**DateTime** in IRD, **datetime2** in dpi|**datetime2**|datetimeoffset|  
|SQL_CA_SS_VARIANT_TYPE|SQL_C_TYPE_DATE|SQL_C_TYPE_BINARY|SQL_C_TYPE_TIMESTAMP|SQL_C_TYPE_TIMESTAMP|SQL_C_TYPE_TIMESTAMP|SQL_C_TYPE_BINARY|  
|SQL_CA_SS_VARIANT_SQL_TYPE|SQL_TYPE_DATE|SQL_SS_TIME2|SQL_TYPE_TIMESTAMP|SQL_TYPE_TIMESTAMP|SQL_TYPE_TIMESTAMP|SQL_SS_TIMESTAMPOFFSET|  
|SQL_CA_SS_SERVER_TYPE|N/D|N/D|SQL_SS_TYPE_SMALLDATETIME|SQL_SS_TYPE_DATETIME|SQL_SS_TYPE_DEFAULT|N/D|  
  
 Negli intervalli di valori si verificano talvolta delle discontinuità. Nell'intervallo 8,10.. 16, ad esempio, manca il valore 9. Ciò è dovuto all'aggiunta di un separatore decimale quando la precisione frazionaria è maggiore di zero.  
  
 **datetime2** viene restituito come TypeName per **smalldatetime** e **DateTime** perché il driver lo utilizza come tipo comune per trasmettere tutti i valori **SQL_TYPE_TIMESTAMP** al server.  
  
 SQL_CA_SS_VARIANT_SQL_TYPE è un nuovo campo di descrizione. Questo campo è stato aggiunto a IRD e a dpi per consentire alle applicazioni di specificare il tipo di valore associato a colonne e parametri **SqlVariant** (SQL_SSVARIANT)  
  
 SQL_CA_SS_SERVER_TYPE è un nuovo campo solo IPD che consente alle applicazioni di controllare le modalità di invio al server dei valori per i parametri associati come SQL_TYPE_TYPETIMESTAMP (oppure come SQL_SS_VARIANT con un SQL_C_TYPE_TIMESTAMP di tipo C). Se SQL_DESC_CONCISE_TYPE è SQL_TYPE_TIMESTAMP (o è SQL_SS_VARIANT e il tipo C è SQL_C_TYPE_TIMESTAMP) quando viene chiamato SQLExecute o SQLExecDirect, il valore di SQL_CA_SS_SERVER_TYPE determina il tipo TDS (Tabular Data Stream) del valore del parametro, come indicato di seguito:  
  
|Valore di SQL_CA_SS_SERVER_TYPE|Valori validi per SQL_DESC_PRECISION|Valori validi per SQL_DESC_LENGTH|Tipo TDS|  
|----------------------------------------|-------------------------------------------|----------------------------------------|--------------|  
|SQL_SS_TYPE_DEFAULT|0..7|19, 21..27|**datetime2**|  
|SQL_SS_TYPE_SMALLDATETIME|0|19|**smalldatetime**|  
|SQL_SS_TYPE_DATETIME|3|23|**datetime**|  
  
 L'impostazione predefinita di SQL_CA_SS_SERVER_TYPE è SQL_SS_TYPE_DEFAULT. Le impostazioni di SQL_DESC_PRECISION e SQL_DESC_LENGTH vengono convalidate con l'impostazione di SQL_CA_SS_SERVER_TYPE, come descritto nella tabella precedente. Se questa convalida non riesce, viene restituito SQL_ERROR e viene registrato un record di diagnostica con SQLState 07006 e con il messaggio "Violazione dell'attributo del tipo di dati". Questo errore viene restituito anche se SQL_CA_SS_SERVER_TYPE è impostato su un valore diverso da SQL_SS_TYPE DEFAULT e DESC_CONCISE_TYPE non è SQL_TYPE_TIMESTAMP. Queste convalide vengono eseguite quando si verifica la convalida di consistenza per i descrittori, ad esempio:  
  
-   Quando SQL_DESC_DATA_PTR viene modificato.  
  
-   In fase di preparazione o di esecuzione (quando vengono chiamati i metodi SQLExecute, SQLExecDirect, SQLSetPos o SQLBulkOperations).  
  
-   Quando un'applicazione impone una preparazione non posticipata chiamando SQLPrepare con la preparazione posticipata disabilitata oppure chiamando SQLNumResultCols, SQLDescribeCol o SQLDescribeParam per un'istruzione preparata ma non eseguita.  
  
 Quando SQL_CA_SS_SERVER_TYPE viene impostato da una chiamata a SQLSetDescField, il relativo valore deve essere SQL_SS_TYPE_DEFAULT, SQL_SS_TYPE_SMALLDATETIME o SQL_SS_TYPE_DATETIME. In caso contrario, viene restituito SQL_ERROR e viene registrato un record di diagnostica con SQLState HY092 e con il messaggio "Identificatore di opzione o di attributo non valido".  
  
 L'attributo SQL_CA_SS_SERVER_TYPE può essere utilizzato da applicazioni che dipendono dalla funzionalità supportata da **DateTime** e **smalldatetime**, ma non da **datetime2**. Ad esempio, **datetime2** richiede l'uso delle funzioni **DateAdd** e **datediif** , mentre **DateTime** e **smalldatetime** consentono anche operatori aritmetici. Nella maggior parte delle applicazioni è consigliabile evitare l'uso di questo attributo.  
  
## <a name="information-returned-in-ird-fields"></a>Informazioni restituite nei campi IRD  
 Di seguito sono riportate le informazioni restituite nei campi IRD:  
  
|Tipo di colonna|Data|time|smalldatetime|Datetime|datetime2|datetimeoffset|  
|-----------------|----------|----------|-------------------|--------------|---------------|--------------------|  
|SQL_DESC_AUTO_UNIQUE_VALUE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|  
|SQL_DESC_CASE_SENSITIVE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|  
|SQL_DESC_CONCISE_TYPE|SQL_TYPE_DATE|SQL_SS_TIME2|SQL_TYPE_TIMESTAMP|SQL_TYPE_TIMESTAMP|SQL_TYPE_TIMESTAMP|SQL_SS_TIMESTAMPOFFSET|  
|SQL_DESC_DATETIME_INTERVAL_CODE|SQL_CODE_DATE|0|SQL_CODE_TIMESTAMP|SQL_CODE_TIMESTAMP|SQL_CODE_TIMESTAMP|0|  
|SQL_DESC_DATETIME_INTERVAL_PRECISION|10|8, 10.. 16|16|23|19, 21..27|26, 28..34|  
|SQL_DESC_DISPLAY_SIZE|10|8, 10.. 16|16|23|19, 21..27|26, 28..34|  
|SQL_DESC_FIXED_PREC_SCALE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|  
|SQL_DESC_LENGTH|10|8, 10.. 16|16|2|19, 21..27|26, 28..34|  
|SQL_DESC_LITERAL_PREFIX|'|'|'|'|'|'|  
|SQL_DESC_LITERAL_SUFFIX|'|'|'|'|'|'|  
|SQL_DESC_LOCAL_TYPE_NAME|**date**|**time**|**smalldatetime**|**datetime**|**datetime2**|datetimeoffset|  
|SQL_DESC_OCTET_LENGTH|6|12|4|8|16|20|  
|SQL_DESC_PRECISION|0|0..7|0|3|0..7|0..7|  
|SQL_DESC_SCALE|0|0..7|0|3|0..7|0..7|  
|SQL_DESC_SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|  
|SQL_DESC_TYPE|SQL_DATETIME|SQL_SS_TIME2|SQL_DATETIME|SQL_DATETIME|SQL_DATETIME|SQL_SS_TIMESTAMPOFFSET|  
|SQL_DESC_TYPE_NAME|**date**|**time**|**smalldatetime**|**datetime**|**datetime2**|datetimeoffset|  
|SQL_DESC_UNSIGNED|SQL_TRUE|SQL_TRUE|SQL_TRUE|SQL_TRUE|SQL_TRUE|SQL_TRUE|  
  
## <a name="see-also"></a>Vedere anche  
 [Metadati &#40;&#41;ODBC ](./date-and-time-improvements-odbc.md)  
  
