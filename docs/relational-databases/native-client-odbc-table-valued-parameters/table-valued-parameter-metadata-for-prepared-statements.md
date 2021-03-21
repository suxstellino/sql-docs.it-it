---
description: Metadati del parametro con valori di tabella per le istruzioni preparate
title: Metadati TVP per le istruzioni preparate
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (ODBC), metadata for prepared statements
ms.assetid: fd2fc705-2e98-4011-9822-c7e6cca4a535
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 995655e5764c3aada26317a9ef2c3acf803dcaa4
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755071"
---
# <a name="table-valued-parameter-metadata-for-prepared-statements"></a>Metadati del parametro con valori di tabella per le istruzioni preparate
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Un'applicazione può ottenere i metadati per una chiamata di procedura preparata tramite SQLNumParams e SQLDescribeParam. Per i parametri con valori di tabella, *DataTypePTR* è impostato su SQL_SS_TABLE. I metadati aggiuntivi sono disponibili tramite SQLGetDescField per SQL_CA_SS_TYPE_NAME, SQL_CA_SS_CATALOG_NAME e SQL_CA_SS_SCHEMA_NAME.  
  
 È possibile utilizzare SQL_CA_SS_TYPE_NAME, SQL_CA_SS_CATALOG_NAME e SQL_CA_SS_SCHEMA_NAME con SQLColumns per ottenere i metadati della colonna per i tipi di tabella associati ai parametri con valori di tabella. In questo caso, SQL_SOPT_SS_NAME_SCOPE necessario impostare su SQL_SS_NAME_SCOPE_TABLE_TYPE prima di chiamare SQLColumns. Impostare nuovamente SQL_SOPT_SS_NAME_SCOPE sul valore predefinito SQL_SS_NAME_SCOPE_TABLE al termine del recupero dei metadati della colonna per i parametri con valori di tabella.  
  
 SQL_CA_SS_TYPE_NAME, SQL_CA_SS_CATALOG_NAME e SQL_CA_SS_SCHEMA_NAME possono inoltre essere utilizzati con i parametri dei tipi CLR definiti dall'utente.  
  
 Non è possibile ottenere i metadati del parametro con valori di tabella per istruzioni preparate che non siano chiamate a stored procedure. Se si tenta di eseguire questa operazione, l'applicazione restituisce SQL_ERROR con SQLSTATE 42000 e il messaggio indicante un errore di sintassi o una violazione dell'accesso.  
  
## <a name="see-also"></a>Vedere anche  
 [Parametri con valori di tabella &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)  
  
  
