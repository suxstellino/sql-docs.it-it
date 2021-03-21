---
description: Metadati aggiuntivi dei parametri con valori di tabella
title: Metadati aggiuntivi del parametro Table-Valued | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (ODBC), catalog functions to retrieve metadata
- table-valued parameters (ODBC), metadata
ms.assetid: 6c193188-5185-4373-9a0d-76cfc150c828
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0d9aece21d9b67140090312888ef7e1b1b9d327d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748125"
---
# <a name="additional-table-valued-parameter-metadata"></a>Metadati aggiuntivi dei parametri con valori di tabella
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Per recuperare i metadati per un parametro con valori di tabella, un'applicazione chiama SQLProcedureColumns. Per un parametro con valori di tabella, SQLProcedureColumns restituisce una singola riga. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Sono state aggiunte due colonne specifiche aggiuntive, SS_TYPE_CATALOG_NAME e SS_TYPE_SCHEMA_NAME, per fornire informazioni sullo schema e sul catalogo per i tipi di tabella associati ai parametri con valori di tabella. In conformità con la specifica ODBC, SS_TYPE_CATALOG_NAME e SS_TYPE_SCHEMA_NAME vengono visualizzati prima di tutte le colonne specifiche del driver che sono state aggiunte nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e dopo tutte le colonne richieste da ODBC stesso.  
  
 Nella tabella seguente sono elencate le colonne significative per i parametri con valori di tabella.  
  
|Nome colonna|Tipo di dati|Valore/commenti|  
|-----------------|---------------|---------------------|  
|DATA_TYPE|Smallint non NULL|SQL_SS_TABLE|  
|TYPE_NAME|WVarchar(128) non NULL|Nome del tipo del parametro con valori di tabella.|  
|COLUMN_SIZE|Integer|NULL|  
|BUFFER_LENGTH|Integer|0|  
|DECIMAL_DIGITS|Smallint|NULL|  
|NUM_PREC_RADIX|Smallint|NULL|  
|NULLABLE|Smallint non NULL|SQL_NULLABLE|  
|REMARKS|Varchar|NULL|  
|COLUMN_DEF|WVarchar(4000)|NULL|  
|SQL_DATA_TYPE|Smallint non NULL|SQL_SS_TABLE|  
|SQL_DATETIME_SUB|Smallint|NULL|  
|CHAR_OCTET_LENGTH|Integer|NULL|  
|ORDINAL_POSITION|Integer non NULL|Posizione ordinale del parametro.|  
|IS_NULLABLE|Varchar|"YES"|  
|SS_TYPE_CATALOG_NAME|WVarchar(128) non NULL|Catalogo contenente la definizione di tipo per il tipo di tabella del parametro con valori di tabella.|  
|SS_TYPE_SCHEMA_NAME|WVarchar(128) non NULL|Schema contenente la definizione di tipo per il tipo di tabella del parametro con valori di tabella.|  
  
 Le colonne WVarchar sono definite come Varchar nella specifica di ODBC ma, di fatto, vengono restituite come WVarchar in tutti i driver ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recenti. Questa modifica è stata apportata quando il supporto Unicode è stato aggiunto alla specifica di ODBC 3.5 ma non è stato esplicitamente menzionato.  
  
 Per ottenere metadati aggiuntivi per i parametri con valori di tabella, un'applicazione utilizza le funzioni di catalogo SQLColumns e SQLPrimaryKeys. Prima che queste funzioni vengano chiamate per i parametri con valori di tabella, è necessario che l'attributo dell'istruzione SQL_SOPT_SS_NAME_SCOPE venga impostato su SQL_SS_NAME_SCOPE_TABLE_TYPE. Questo valore indica che l'applicazione richiede metadati per un tipo di tabella piuttosto che una tabella effettiva. L'applicazione passa quindi il TYPE_NAME del parametro con valori di tabella come parametro *TableName* . Per identificare il catalogo e lo schema per il parametro con valori di tabella, SS_TYPE_CATALOG_NAME e SS_TYPE_SCHEMA_NAME vengono utilizzati rispettivamente con i parametri *CatalogName* e *SchemaName* . Quando un'applicazione ha completato il recupero dei metadati per i parametri con valori di tabella, deve impostare nuovamente SQL_SOPT_SS_NAME_SCOPE sul valore predefinito di SQL_SS_NAME_SCOPE_TABLE.  
  
 Quando SQL_SOPT_SS_NAME_SCOPE è impostato su SQL_SS_NAME_SCOPE_TABLE, le query ai server collegati non riescono. Le chiamate a SQLColumns o SQLPrimaryKeys con un catalogo contenente un componente server avranno esito negativo.  
  
## <a name="see-also"></a>Vedere anche  
 [Parametri con valori di tabella &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)  
  
  
