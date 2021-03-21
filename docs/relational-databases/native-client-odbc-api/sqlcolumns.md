---
description: SQLColumns
title: SQLColumns | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLColumns function
ms.assetid: 69d3af44-8196-43ab-8037-cdd06207b171
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 086904d3456ba5160813c731ca84db249692b441
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755061"
---
# <a name="sqlcolumns"></a>SQLColumns
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  **SQLColumns** restituisce SQL_SUCCESS se sono presenti o meno valori per i parametri *CatalogName*, *TableName* o *ColumnName* . **SQLFetch** restituisce SQL_NO_DATA quando in questi parametri vengono utilizzati valori non validi.  
  
> [!NOTE]  
>  Per i tipi di valori di grandi dimensioni, tutti i parametri della lunghezza verranno restituiti con un valore di SQL_SS_LENGTH_UNLIMITED.  
  
 **SQLColumns** può essere eseguito su un cursore del server statico. Il tentativo di eseguire **SQLColumns** su un cursore aggiornabile (dinamico o keyset) restituirà SQL_SUCCESS_WITH_INFO indicante che il tipo di cursore è stato modificato.  
  
 The [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC driver supports reporting information for tables on linked servers by accepting a two-part name for the *CatalogName* parameter: *Linked_Server_Name.Catalog_Name*.  
  
 Per ODBC 2. *x* le applicazioni che non utilizzano caratteri jolly in *TableName*, **SQLColumns** restituisce informazioni su tutte le tabelle i cui nomi corrispondono a *TableName* e sono di proprietà dell'utente corrente. Se l'utente corrente non è proprietario di alcuna tabella il cui nome corrisponde al parametro *TableName* , **SQLColumns** restituisce informazioni su qualsiasi tabella di proprietà di altri utenti in cui il nome della tabella corrisponde al parametro *TableName* . Per ODBC 2. *x* applicazioni che utilizzano caratteri jolly, **SQLColumns** restituisce tutte le tabelle i cui nomi corrispondono a *TableName*. Per ODBC 3. *x* applicazioni **SQLColumns** restituisce tutte le tabelle i cui nomi corrispondono a *TableName* indipendentemente dal proprietario o dal fatto che vengano utilizzati caratteri jolly.  
  
 Nella tabella seguente vengono elencate le colonne restituite dal set di risultati:  
  
|Nome colonna|Descrizione|  
|-----------------|-----------------|  
|DATA_TYPE|Restituisce SQL_VARCHAR, SQL_VARBINARY o SQL_WVARCHAR per i tipi di dati **varchar (max)** .|  
|TYPE_NAME|Restituisce "varchar", "varbinary" o "nvarchar" per i tipi di dati **varchar (max)**, **varbinary (max)** e **nvarchar (max)** .|  
|COLUMN_SIZE|Restituisce SQL_SS_LENGTH_UNLIMITED per i tipi di dati **varchar (max)** che indicano che le dimensioni della colonna sono illimitate.|  
|BUFFER_LENGTH|Restituisce SQL_SS_LENGTH_UNLIMITED per i tipi di dati **varchar (max)** che indicano che le dimensioni del buffer sono illimitate.|  
|SQL_DATA_TYPE|Restituisce SQL_VARCHAR, SQL_VARBINARY o SQL_WVARCHAR per i tipi di dati **varchar (max)** .|  
|CHAR_OCTET_LENGTH|Restituisce la lunghezza massima di una colonna di tipo carattere o binario. Restituisce 0 per indicare che la dimensione è illimitata.|  
|SS_XML_SCHEMACOLLECTION_CATALOG_NAME|Restituisce il nome del catalogo nel quale è definito il nome di una raccolta di XML Schema. Se non è possibile trovare il nome del catalogo, questa variabile contiene una stringa vuota.|  
|SS_XML_SCHEMACOLLECTION_SCHEMA_NAME|Restituisce il nome dello schema nel quale è definito il nome di una raccolta di XML Schema. Se non è possibile trovare il nome dello schema, questa variabile contiene una stringa vuota.|  
|SS_XML_SCHEMACOLLECTION_NAME|Restituisce il nome di una raccolta di XML Schema. Se non è possibile trovare il nome, questa variabile contiene una stringa vuota.|  
|SS_UDT_CATALOG_NAME|Nome del catalogo contenente il tipo definito dall'utente.|  
|SS_UDT_SCHEMA_NAME|Nome dello schema contenente il tipo definito dall'utente.|  
|SS_UDT_ASSEMBLY_TYPE_NAME|Nome completo dell'assembly del tipo definito dall'utente.|  
  
 Per i tipi definiti dall'utente, la colonna TYPE_NAME esistente viene utilizzata per indicare il nome del tipo definito dall'utente. non è pertanto necessario aggiungere altre colonne al set di risultati di **SQLColumns** o [SQLProcedureColumns](../../relational-databases/native-client-odbc-api/sqlprocedurecolumns.md). DATA_TYPE per un parametro o una colonna con tipo definito dall'utente è SQL_SS_UDT.  
  
 Per il tipo definito dall'utente dei parametri, è possibile utilizzare i nuovi descrittori specifici del driver definiti precedentemente per ottenere o impostare le proprietà di metadati aggiuntive di un tipo definito dall'utente, nel caso in cui il server restituisca o richieda queste informazioni.  
  
 Quando un client si connette a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e chiama SQLColumns, l'utilizzo di valori null o Wild Card per il parametro di input del catalogo non restituirà informazioni da altri cataloghi. Verranno invece restituite solo informazioni relative al catalogo corrente. Il client può prima chiamare SQLTables per determinare in quale catalogo si trova la tabella desiderata. Il client può quindi utilizzare tale valore del catalogo per il parametro di input del catalogo nella chiamata a SQLColumns per recuperare informazioni sulle colonne della tabella.  
  
## <a name="sqlcolumns-and-table-valued-parameters"></a>SQLColumns e parametri con valori di tabella  
 Il set di risultati restituito da SQLColumns dipende dall'impostazione di SQL_SOPT_SS_NAME_SCOPE. Per ulteriori informazioni, vedere [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md). Di seguito sono indicate le colonne che sono state aggiunte per i parametri con valori di tabella:  
  
|Nome colonna|Tipo di dati|Contenuto|  
|-----------------|---------------|--------------|  
|SS_IS_COMPUTED|Smallint|Per una colonna in TABLE_TYPE, SQL_TRUE se la colonna è una colonna calcolata. In caso contrario, SQL_FALSE.|  
|SS_IS_IDENTITY|Smallint|SQL_TRUE se la colonna è una colonna Identity. In caso contrario, SQL_FALSE.|  
  
 Per ulteriori informazioni sui parametri con valori di tabella, vedere [parametri con valori di tabella &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="sqlcolumns-support-for-enhanced-date-and-time-features"></a>Supporto di SQLColumns per le caratteristiche avanzate di data e ora  
 Per informazioni sui valori restituiti per i tipi data/ora, vedere [metadati del catalogo](../../relational-databases/native-client-odbc-date-time/metadata-catalog.md).  
  
 Per ulteriori informazioni, vedere [miglioramenti di data e ora &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqlcolumns-support-for-large-clr-udts"></a>Supporto di SQLColumns per tipi CLR definiti dall'utente di grandi dimensioni  
 **SQLColumns** supporta i tipi CLR definiti dall'utente (UDT) di grandi dimensioni. Per ulteriori informazioni, vedere [tipi CLR User-Defined di grandi dimensioni &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="sqlcolumns-support-for-sparse-columns"></a>Supporto di SQLColumns per colonne di tipo sparse  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Al set di risultati per SQLColumns sono state aggiunte due colonne specifiche:  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|SS_IS_SPARSE|**Smallint**|SQL_TRUE se una colonna è di tipo sparse. In caso contrario, SQL_FALSE.|  
|SS_IS_COLUMN_SET|**Smallint**|Se la colonna è la colonna **column_set** , questo è SQL_TRUE; in caso contrario, SQL_FALSE.|  
  
 In conformità con la specifica ODBC, SS_IS_SPARSE e SS_IS_COLUMN_SET vengono visualizzati prima di tutte le colonne specifiche del driver che sono state aggiunte alle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] versioni precedenti di [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e dopo tutte le colonne richieste da ODBC stesso.  
  
 Il set di risultati restituito da SQLColumns dipende dall'impostazione di SQL_SOPT_SS_NAME_SCOPE. Per ulteriori informazioni, vedere [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md).  
  
 Per ulteriori informazioni sulle colonne di tipo sparse in ODBC, vedere [supporto di colonne di tipo sparse &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/sparse-columns-support-odbc.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Funzione SQLColumns](../../odbc/reference/syntax/sqlcolumns-function.md)   
 [Dettagli di implementazione dell'API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
