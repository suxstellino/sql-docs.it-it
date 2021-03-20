---
description: Esecuzione di istruzioni (ODBC)
title: Esecuzione di istruzioni (ODBC) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, statements
- statements [ODBC]
- ODBC applications, statements
- statements [ODBC], executing
ms.assetid: 063fc40d-ff81-490d-9c9b-2faefb729f37
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5dde78cafece70444564286d12234acaf9d18038
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751301"
---
# <a name="executing-statements-odbc"></a>Esecuzione di istruzioni (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Il [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] driver ODBC di Native Client offre diversi modi per eseguire istruzioni SQL in un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] database:  
  
-   Esecuzione diretta  
  
-   Esecuzione preparata  
  
 L'esecuzione diretta implica la compilazione di una stringa di caratteri contenente un' [!INCLUDE[tsql](../../../includes/tsql-md.md)] istruzione e l'invio per l'esecuzione tramite la funzione **SQLExecDirect** . L'esecuzione preparata implica la compilazione di una stringa di caratteri contenente un'istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)] e la successiva esecuzione di questa in due fasi. Nella prima fase viene utilizzata la funzione [SQLPrepare function](../../../odbc/reference/syntax/sqlprepare-function.md) per analizzare e compilare il piano di esecuzione per l'istruzione in [!INCLUDE[ssDE](../../../includes/ssde-md.md)] . Nella seconda fase viene utilizzata la funzione **SQLExecute** per eseguire il piano di esecuzione preparato in precedenza. con conseguente risparmio dell'overhead correlato all'analisi e alla compilazione in ogni esecuzione. L'esecuzione preparata viene generalmente utilizzata dalle applicazioni per eseguire ripetutamente la stessa istruzione SQL con parametri.  
  
 Sia nel caso dell'esecuzione diretta che in quello dell'esecuzione preparata è possibile eseguire una singola istruzione [!INCLUDE[tsql](../../../includes/tsql-md.md)] o un batch di istruzioni SQL oppure è possibile chiamare una stored procedure.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Esecuzione diretta](../../../relational-databases/native-client-odbc-queries/executing-statements/direct-execution.md)  
  
-   [Esecuzione preparata](../../../relational-databases/native-client-odbc-queries/executing-statements/prepared-execution.md)  
  
-   [Procedure](../../../relational-databases/native-client-odbc-queries/executing-statements/procedures.md)  
  
-   [Batch di istruzioni](../../../relational-databases/native-client-odbc-queries/executing-statements/batches-of-statements.md)  
  
-   [Effetti delle opzioni ISO](../../../relational-databases/native-client-odbc-queries/executing-statements/effects-of-iso-options.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esecuzione di query &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-queries/executing-queries-odbc.md)  
  
