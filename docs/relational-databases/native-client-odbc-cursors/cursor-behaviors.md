---
description: Comportamenti dei cursori
title: Comportamenti del cursore | Microsoft Docs
ms.custom: ''
ms.date: 10/24/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- scrollable cursors [SQL Server]
- SQL Server Native Client ODBC driver, cursors
- version-based optimistic concurrency
- cursors [ODBC], cursor behaviors
- ODBC applications, cursors
- SQL_ATTR_CURSOR_SENSITIVITY option
- SQL_ATTR_CURSOR_SCROLLABLE option
- sensitivity behavior of cursor
- ODBC cursors, cursor behaviors
ms.assetid: 742ddcd2-232b-4aa1-9212-027df120ad35
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 91bc7286beb8b03f6e76698e84a8fcf3d8d3f11c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438664"
---
# <a name="cursor-behaviors"></a>Comportamenti dei cursori
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  ODBC supporta le opzioni ISO per la specifica del comportamento dei cursori mediante lo scorrimento e la sensibilità. Questi comportamenti vengono specificati impostando le opzioni SQL_ATTR_CURSOR_SCROLLABLE e SQL_ATTR_CURSOR_SENSITIVITY in una chiamata a [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md). Il driver ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client implementa queste opzioni richiedendo cursori server con le caratteristiche seguenti.  
  
|Impostazioni del comportamento del cursore|Caratteristiche del cursore server richieste|  
|------------------------------|---------------------------------------------|  
|SQL_SCROLLABLE e SQL_SENSITIVE|Cursore gestito da keyset e concorrenza ottimistica basata sulla versione|  
|SQL_SCROLLABLE e SQL_INSENSITIVE|Cursore statico e concorrenza di sola lettura|  
|SQL_SCROLLABLE e SQL_UNSPECIFIED|Cursore statico e concorrenza di sola lettura|  
|SQL_NONSCROLLABLE e SQL_SENSITIVE|Cursore forward-only e concorrenza ottimistica basata sulla versione|  
|SQL_NONSCROLLABLE e SQL_INSENSITIVE|Set di risultati predefinito (forward only, di sola lettura)|  
|SQL_NONSCROLLABLE e SQL_UNSPECIFIED|Set di risultati predefinito (forward only, di sola lettura)|  
  
 Per la concorrenza ottimistica basata sulla versione è richiesta una colonna **timestamp** nella tabella sottostante. Se è richiesto il controllo della concorrenza ottimistica basata sulla versione in una tabella che non dispone di una colonna di tipo **timestamp** , il server utilizza la concorrenza ottimistica basata sui valori.  
  
## <a name="scrollability"></a>Scorrimento  
 Quando SQL_ATTR_CURSOR_SCROLLABLE è impostato su SQL_SCROLLABLE, il cursore supporta tutti i valori diversi per il parametro *FetchOrientation* di [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md). Quando SQL_ATTR_CURSOR_SCROLLABLE è impostato su SQL_NONSCROLLABLE, il cursore supporta solo un valore *FetchOrientation* di SQL_FETCH_NEXT.  
  
## <a name="sensitivity"></a>Sensibilità  
 Quando SQL_ATTR_CURSOR_SENSITIVITY è impostato su SQL_SENSITIVE, il cursore riflette le modifiche dei dati apportate dall'utente corrente di cui è stato eseguito il commit da altri utenti. Quando SQL_ATTR_CURSOR_SENSITIVITY è impostato su SQL_INSENSITIVE, il cursore non riflette le modifiche dei dati.  
  
## <a name="see-also"></a>Vedere anche  
 Utilizzo delle [proprietà del cursore](properties/cursor-properties.md) [cursori (ODBC)](../../relational-databases/native-client-odbc-cursors/using-cursors-odbc.md) 
  
  
