---
description: SQLProcedureColumns
title: SQLProcedureColumns | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLProcedureColumns function
ms.assetid: 6671e180-0072-4de5-90f5-314306d2ba9c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8e6e8ba52edb7310112bfa3e4061d6654bab61bb
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104745471"
---
# <a name="sqlprocedurecolumns"></a>SQLProcedureColumns
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  **SQLProcedureColumns** restituisce una riga che riporta gli attributi del valore restituito di tutte le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stored procedure.  
  
 **SQLProcedureColumns** restituisce SQL_SUCCESS se sono presenti o meno valori per i parametri *CatalogName*, *SchemaName*, *ProcName* o *ColumnName* . **SQLFetch** restituisce SQL_NO_DATA quando in questi parametri vengono utilizzati valori non validi.  
  
 **SQLProcedureColumns** può essere eseguito su un cursore del server statico. Il tentativo di eseguire **SQLProcedureColumns** su un cursore aggiornabile (dinamico o keyset) restituirà SQL_SUCCESS_WITH_INFO indicante che il tipo di cursore è stato modificato.  
  
 Nella tabella seguente sono elencate le colonne restituite dal set di risultati e il modo in cui sono state estese per gestire i tipi di dati **UDT** e **XML** tramite il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native Client:  
  
|Nome colonna|Descrizione|  
|-----------------|-----------------|  
|SS_UDT_CATALOG_NAME|Restituisce il nome del catalogo contenente il tipo definito dall'utente.|  
|SS_UDT_SCHEMA_NAME|Restituisce il nome dello schema contenente il tipo definito dall'utente.|  
|SS_UDT_ASSEMBLY_TYPE_NAME|Restituisce il nome completo dell'assembly del tipo definito dall'utente.|  
|SS_XML_SCHEMACOLLECTION_CATALOG_NAME|Restituisce il nome del catalogo nel quale è definito il nome di una raccolta di XML Schema. Se non è possibile trovare il nome del catalogo, questa variabile contiene una stringa vuota.|  
|SS_XML_SCHEMACOLLECTION_SCHEMA_NAME|Restituisce il nome dello schema nel quale è definito il nome di una raccolta di XML Schema. Se non è possibile trovare il nome dello schema, questa variabile contiene una stringa vuota.|  
|SS_XML_SCHEMACOLLECTION_NAME|Restituisce il nome di una raccolta di XML Schema. Se non è possibile trovare il nome, questa variabile contiene una stringa vuota.|  
  
## <a name="sqlprocedurecolumns-and-table-valued-parameters"></a>SQLProcedureColumns e parametri con valori di tabella  
 SQLProcedureColumns gestisce i parametri con valori di tabella in modo analogo ai tipi CLR definiti dall'utente. Nelle righe restituite per i parametri con valori di tabella le colonne presentano i valori seguenti:  
  
|Nome colonna|Descrizione/valore|  
|-----------------|------------------------|  
|DATA_TYPE|SQL_SS_TABLE|  
|TYPE_NAME|Nome del tipo di tabella per il parametro con valori di tabella.|  
|COLUMN_SIZE|NULL|  
|BUFFER_LENGTH|0|  
|DECIMAL_DIGITS|Il numero delle colonne presenti nel parametro con valori di tabella.|  
|NUM_PREC_RADIX|NULL|  
|NULLABLE|SQL_NULLABLE|  
|REMARKS|NULL|  
|COLUMN_DEF|NULL I tipi di tabella potrebbero non avere valori predefiniti.|  
|SQL_DATA_TYPE|SQL_SS_TABLE|  
|SQL_DATEIME_SUB|NULL|  
|CHAR_OCTET_LENGTH|NULL|  
|IS_NULLABLE|"YES"|  
|SS_TYPE_CATALOG_NAME|Restituisce il nome del catalogo contenente la tabella o il tipo CLR definito dall'utente.|  
|SS_TYPE_SCHEMA_NAME|Restituisce il nome dello schema contenente la tabella o il tipo CLR definito dall'utente.|  
  
 In [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive sono disponibili le colonne SS_TYPE_CATALOG_NAME e SS_TYPE_SCHEMA_NAME per restituire rispettivamente il catalogo e lo schema per i parametri con valori di tabella. Tali colonne vengono popolate per i parametri con valori di tabella e anche per i parametri del tipo CLR definito dall'utente. Questa funzionalità aggiuntiva non influenza le colonne di catalogo e di schema esistenti per i parametri del tipo CLR definito dall'utente. Vengono popolate anche per mantenere la compatibilità con le versioni precedenti.  
  
 In conformità con la specifica ODBC, SS_TYPE_CATALOG_NAME e SS_TYPE_SCHEMA_NAME vengono visualizzati prima di tutte le colonne specifiche del driver che sono state aggiunte nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e dopo tutte le colonne richieste da ODBC stesso.  
  
 Per ulteriori informazioni sui parametri con valori di tabella, vedere [parametri con valori di tabella &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="sqlprocedurecolumns-support-for-enhanced-date-and-time-features"></a>Supporto di SQLProcedureColumns per le caratteristiche avanzate di data e ora  
 Per i valori restituiti per i tipi data/ora, vedere [metadati del catalogo](../../relational-databases/native-client-odbc-date-time/metadata-catalog.md).  
  
 Per informazioni più generali, vedere [miglioramenti di data e ora &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqlprocedurecolumns-support-for-large-clr-udts"></a>Supporto di SQLProcedureColumns per tipi CLR definiti dall'utente di grandi dimensioni  
 **SQLProcedureColumns** supporta i tipi CLR definiti dall'utente di grandi dimensioni. Per ulteriori informazioni, vedere [tipi CLR User-Defined di grandi dimensioni &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="see-also"></a>Vedere anche  
 [SQLProcedureColumns (funzione)](../../odbc/reference/syntax/sqlprimarykeys-function.md)   
 [Dettagli di implementazione dell'API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
