---
description: SQLProcedures
title: SQLProcedures | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLProcedures function
ms.assetid: ec41f017-f5e0-40ef-913a-65d206068631
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5fc316c908b5de2e2fab906bbe80c1ed52caaec6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485003"
---
# <a name="sqlprocedures"></a>SQLProcedures
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Tutte le stored procedure di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituiscono un valore. **SQLProcedures** restituisce SQL_PT_FUNCTION per la colonna del set di risultati PROCEDURE_TYPE.  
  
 **SQLProcedures** restituisce SQL_SUCCESS se sono presenti o meno valori per i parametri *CatalogName, SchemaName* o *ProcName* . **SQLFetch** restituisce SQL_NO_DATA quando in questi parametri vengono utilizzati valori non validi.  
  
 **SQLProcedures** può essere eseguito su un cursore del server statico. Il tentativo di eseguire **SQLProcedures** su un cursore aggiornabile (dinamico o keyset) restituirà SQL_SUCCESS_WITH_INFO, a indicare che il tipo di cursore è stato modificato.  
  
 **SQLProcedures** restituisce informazioni su tutte le routine i cui nomi corrispondono a *ProcName* e possono essere eseguite dall'utente corrente o per cui all'utente corrente è stata concessa l'autorizzazione View definition.  
  
## <a name="see-also"></a>Vedere anche  
 [SQLProcedures (funzione)](../../odbc/reference/syntax/sqlprocedures-function.md)   
 [Dettagli di implementazione dell'API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
