---
description: SQLEndTran
title: SQLEndTran | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLEndTran function
ms.assetid: 95cff841-c2d5-4e1e-a18d-f3d4696a5b85
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ae1b157fd256ae9dd16a4c2b2dd8544fa472a500
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751391"
---
# <a name="sqlendtran"></a>SQLEndTran
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Per impostazione predefinita, il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native client chiude il cursore associato di un'istruzione quando **SQLEndTran** esegue il commit o il rollback di un'operazione. I cursori del server vengono chiusi a meno che non siano statici. Quando **SQLEndTran** esegue il commit o il rollback di un'operazione, il comportamento del cursore associato dell'istruzione è determinato dal valore dell'attributo di connessione ODBC specifico del driver SQL_COPT_SS_PRESERVE_CURSORS, impostato da [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Dettagli di implementazione dell'API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)   
 [SQLEndTran Function](../../odbc/reference/syntax/sqlendtran-function.md)  
  
