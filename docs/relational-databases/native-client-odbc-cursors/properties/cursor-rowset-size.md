---
description: Dimensione del set di righe del cursore
title: Dimensioni set di righe cursore | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- cursors [ODBC], rowset size
- ODBC cursors, rowset size
- rowsets [ODBC]
ms.assetid: 2febe2ae-fdc1-490e-a79f-c516bc8e7c3f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5d135dc225e33fa28680b0ff7e74cc81346f1642
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756181"
---
# <a name="cursor-rowset-size"></a>Dimensione del set di righe del cursore
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  I cursori ODBC non si limitano a recuperare una sola riga alla volta. Possono recuperare più righe in ogni chiamata a **SQLFetch** o [SQLFetchScroll](../../../relational-databases/native-client-odbc-api/sqlfetchscroll.md). Quando si utilizza un database client/server come Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], è consigliabile recuperare diverse righe contemporaneamente. Il numero di righe restituite in un recupero è denominato dimensione del set di righe e viene specificato tramite il SQL_ATTR_ROW_ARRAY_SIZE di [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md).  
  
```  
SQLUINTEGER uwRowsize;  
SQLSetStmtAttr(m_hstmt, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER)uwRowsetSize, SQL_IS_UINTEGER);  
```  
  
 I cursori con una dimensione del set di righe maggiore di 1 vengono definiti cursori a blocchi.  
  
 Sono disponibili due opzioni per associare le colonne dei set di risultati per i cursori a blocchi:  
  
-   Associazione per colonna  
  
     Ogni colonna viene associata a una matrice di variabili. Ogni matrice include lo stesso numero di elementi come dimensione del set di righe.  
  
-   Associazione per riga  
  
     Una matrice viene compilata utilizzando strutture che contengono i dati e gli indicatori per tutte le colonne di una riga. La matrice include lo stesso numero di strutture come dimensione del set di righe.  
  
 Quando si utilizza l'associazione per colonna o per riga, ogni chiamata a **SQLFetch** o **SQLFetchScroll** riempie le matrici associate con i dati del set di righe recuperato.  
  
 [SQLGetData](../../../relational-databases/native-client-odbc-api/sqlgetdata.md) può essere utilizzato anche per recuperare i dati delle colonne da un cursore a blocchi. Poiché **SQLGetData** funziona una riga alla volta, è necessario chiamare **SQLSetPos** per impostare una riga specifica nel set di righe come riga corrente prima di chiamare **SQLGetData**.  
  
 Il [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] driver ODBC di Native Client offre un'ottimizzazione utilizzando i set di righe per recuperare rapidamente un intero set di risultati. Per utilizzare questa ottimizzazione, impostare gli attributi del cursore sulle impostazioni predefinite (di sola lettura, di sola lettura, dimensioni del set di righe = 1) al momento della chiamata a **SQLExecDirect** o **SQLExecute** . Il [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] driver ODBC di Native Client configura un set di risultati predefinito. Questa soluzione risulta più efficiente dei cursori del server quando si trasferiscono risultati al client senza scorrimento. Al termine dell'esecuzione dell'istruzione, aumentare la dimensione del set di righe e utilizzare l'associazione per colonna o per riga. Questo consente di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilizzare un set di risultati predefinito per inviare le righe di risultati in modo efficiente al client, mentre il [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] driver ODBC di Native client estrae continuamente le righe dai buffer di rete nel client.  
  
## <a name="see-also"></a>Vedere anche  
 [Proprietà del cursore](../../../relational-databases/native-client-odbc-cursors/properties/cursor-properties.md)  
  
  
