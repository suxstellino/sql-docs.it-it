---
title: Elaborazione dei risultati delle stored procedure | Microsoft Docs
description: Vengono fornite informazioni sui meccanismi utilizzati SQL Server stored procedure per restituire dati alle applicazioni. Le applicazioni devono essere in grado di gestire tutti questi tipi.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC, stored procedures
- SQL Server Native Client ODBC driver, stored procedures
- stored procedures [ODBC], results
ms.assetid: 788ef2a4-17de-4526-960b-46bf29aafc9f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3914292a971bba800fabb4c1b94995466cf0bef8
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104740177"
---
# <a name="processing-stored-procedure-results"></a>Risultati dell'elaborazione delle stored procedure
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Per le stored procedure di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono disponibili quattro meccanismi di restituzione dei dati:  
  
-   Ogni istruzione SELECT di una stored procedure genera un set di risultati.  
  
-   La procedura può restituire dati tramite parametri di output.  
  
-   Un parametro di output del cursore può passare nuovamente un cursore del server [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   La procedura può avere un codice restituito di tipo integer.  
  
 Le applicazioni devono essere in grado di gestire tutti questi output dalle stored procedure. L'istruzione CALL o EXECUTE deve includere marcatori di parametro per il codice restituito e i parametri di output. Utilizzare [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md) per associarli tutti come parametri di output e il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] driver ODBC di Native Client trasferirà i valori di output alle variabili associate. I parametri di output e i codici restituiti sono gli ultimi elementi restituiti al client da e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non vengono restituiti all'applicazione fino a quando [SQLMoreResults](../../relational-databases/native-client-odbc-api/sqlmoreresults.md) non restituisce SQL_NO_DATA.  
  
 ODBC non supporta i parametri di cursore [!INCLUDE[tsql](../../includes/tsql-md.md)] di associazione. Dal momento che tutti i parametri di output devono essere associati prima di eseguire una stored procedure, le stored procedure [!INCLUDE[tsql](../../includes/tsql-md.md)] che contengono un parametro di cursore di output non possono essere chiamate dalle applicazioni ODBC.  
  
## <a name="see-also"></a>Vedere anche  
 [Esecuzione delle stored procedure](../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)  
  
  
