---
description: SQLParamData
title: SQLParamData | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLParamData function
ms.assetid: 92349482-ea22-4a6a-8484-e9c6566794fa
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6fc37208fffff63736682414e4537769b588ecbc
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485033"
---
# <a name="sqlparamdata"></a>SQLParamData
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Quando SQLParamData restituisce il *ValuePtrPtr* associato a un parametro con valori di tabella, l'applicazione deve chiamare SQLPutData con *StrLen_Or_Ind*. Se *StrLen_Or_Ind* ha un valore maggiore di 0, significa che l'applicazione è pronta per la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] raccolta di dati di parametro da parte di Native Client per la successiva riga di parametri con valori di tabella. Se *StrLen_Or_Ind* ha un valore pari a 0, significa che non sono presenti più righe di dati per il parametro con valori di tabella. Per altre informazioni, vedere [Binding e trasferimento dati di Table-Valued parametri e valori di colonna](../../relational-databases/native-client-odbc-table-valued-parameters/binding-and-data-transfer-of-table-valued-parameters-and-column-values.md).  
  
 Per ulteriori informazioni sui parametri con valori di tabella, vedere [parametri con valori di tabella &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="see-also"></a>Vedere anche  
 [SQLParamData](../../odbc/reference/syntax/sqlparamdata-function.md)   
 [Dettagli di implementazione dell'API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
