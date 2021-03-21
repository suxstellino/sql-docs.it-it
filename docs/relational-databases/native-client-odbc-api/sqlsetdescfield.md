---
description: SQLSetDescField
title: SQLSetDescField | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLSetDescField function
ms.assetid: de4bed15-15be-4825-994c-1046255e725a
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3751e811455a48b52692583ac42d6ac80a18247c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754851"
---
# <a name="sqlsetdescfield"></a>SQLSetDescField
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  SQLSetDescField può essere utilizzato per impostare i campi di descrizione per i parametri con valori di tabella e le colonne dei parametri con valori di tabella. Per informazioni sui campi disponibili, vedere [campi di descrizione dei parametri con valori di tabella](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameter-descriptor-fields.md) e [campi del descrittore per Table-Valued colonne costituenti dei parametri](../../relational-databases/native-client-odbc-table-valued-parameters/descriptor-fields-for-table-valued-parameter-constituent-columns.md).  
  
## <a name="remarks"></a>Commenti  
 Le colonne dei parametri con valori di tabella sono disponibili solo quando il campo di intestazione di descrizione SQL_SOPT_SS_PARAM_FOCUS è impostato sul numero ordinale di un record in cui SQL_DESC_TYPE è impostato su SQL_SS_TABLE. Per ulteriori informazioni su SQL_SOPT_SS_PARAM_FOCUS, vedere [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md).  
  
 Se viene effettuato un tentativo di impostare SQL_SOPT_SS_PARAM_FOCUS sull'ordinale di un parametro che non è un parametro con valori di tabella, SQLSetStmtAttr restituisce SQL_ERROR e viene creato un record di diagnostica con SQLSTATE = HY024 e il messaggio "valore attributo non valido". SQL_SOPT_SS_PARAM_FOCUS non viene modificato al momento della restituzione di SQL_ERROR.  
  
 L'impostazione di SQL_SOPT_SS_PARAM_FOCUS su 0 ripristina l'accesso ai record del descrittore per i parametri.  
  
 Per ulteriori informazioni sui parametri con valori di tabella, vedere [parametri con valori di tabella &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="sqlsetdescfield-support-for-enhanced-date-and-time-features"></a>Supporto di SQLSetDescField per le caratteristiche avanzate di data e ora  
 Le caratteristiche di data/ora sono state migliorate in ODBC. Per informazioni sul campo di descrizione fornito per i nuovi tipi di data/ora, vedere [Parameter and Result Metadata](../../relational-databases/native-client-odbc-date-time/metadata-parameter-and-result.md).  
  
 Per ulteriori informazioni, vedere [miglioramenti di data e ora &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqlsetdescfield-support-for-large-clr-udts"></a>Supporto di SQLSetDescField per tipi CLR definiti dall'utente di grandi dimensioni  
 SQLSetDescField supporta i tipi CLR definiti dall'utente di grandi dimensioni. Per ulteriori informazioni, vedere [tipi CLR User-Defined di grandi dimensioni &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="sqlsetdescfield-support-for-sparse-columns"></a>Supporto di SQLSetDescField per colonne di tipo sparse  
 SQLSetDecField può essere usato per impostare SQL_SOPT_SS_NAME_SCOPE nel descrittore del parametro dell'applicazione (APD) sui valori SQL_SS_NAME_SCOPE_EXTENDED e SQL_SS_NAME_SCOPE_SPARSE_COLUMN_SET.  
  
 Per ulteriori informazioni, vedere [supporto di colonne di tipo sparse &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/sparse-columns-support-odbc.md).  
  
## <a name="see-also"></a>Vedere anche  
 [SQLSetDescField](../../odbc/reference/syntax/sqlsetdescfield-function.md)   
 [Dettagli di implementazione dell'API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
