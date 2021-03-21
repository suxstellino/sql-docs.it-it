---
description: Utilizzare Microsoft Distributed Transaction Coordinator (ODBC).
title: Distributed Transaction Coordinator (ODBC)
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- MS DTC, using
ms.assetid: 12a275e1-8c7e-436d-8a4e-b7bee853b35c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a60437fe027f93bbadff502eb5bec63fdc5c16c8
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104733926"
---
# <a name="use-microsoft-distributed-transaction-coordinator-odbc"></a>Utilizzare Microsoft Distributed Transaction Coordinator (ODBC).
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

    
### <a name="to-update-two-or-more-sql-servers-by-using-ms-dtc"></a>Per aggiornare due o più computer SQL Server mediante MS DTC  
  
1.  Connettersi a MS DTC utilizzando la funzione MS DTC OLE DtcGetTransactionManager. Per informazioni su MS DTC, vedere Microsoft Distributed Transaction Coordinator.  
  
2.  Chiamare SQL DriverConnect una volta per ogni connessione SQL Server che si desidera stabilire.  
  
3.  Chiamare la funzione MS DTC OLE ITransactionDispenser::BeginTransaction per iniziare una transazione MS DTC e ottenere un oggetto Transaction che rappresenta la transazione.  
  
4.  Chiamare [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) una o più volte per ogni connessione ODBC che si desidera integrare nella transazione MS DTC. Il secondo parametro [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) deve essere SQL_ATTR_ENLIST_IN_DTC mentre il terzo parametro deve essere l'oggetto Transaction, ottenuto nel passaggio 3.  
  
5.  Chiamare [SQLExecDirect](../../odbc/reference/syntax/sqlexecdirect-function.md) una volta per ogni computer SQL Server da aggiornare.  
  
6.  Chiamare la funzione MS DTC OLE ITransaction::Commit per eseguire il commit della transazione MS DTC. L'oggetto Transaction non è più valido.  

 Per eseguire una serie di transazioni MS DTC, ripetere i passaggi da 3 a 6.  
  
 Per rilasciare il riferimento all'oggetto Transaction, chiamare la funzione MS DTC OLE ITransaction::Return.  
  
 Per usare una connessione ODBC con una transazione MS DTC e quindi usare la stessa connessione con una transazione di SQL Server locale, chiamare [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) con SQL_DTC_DONE.  
  
> [!NOTE]  
>  È inoltre possibile chiamare [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) e [SQLExecDirect](../../odbc/reference/syntax/sqlexecdirect-function.md) per ogni computer SQL Server anziché come suggerito nei passaggi 4 e 5.  
  
## <a name="see-also"></a>Vedere anche  
 [Esecuzione di transazioni &#40;ODBC&#41;](../native-client/odbc/performing-transactions-in-odbc.md)  
  
