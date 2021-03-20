---
description: Recuperare e aggiornare set di righe (ODBC)
title: Recuperare e aggiornare set di righe (ODBC) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- rowsets [ODBC]
ms.assetid: cf0eb3b4-8b72-49fc-a845-95edc360cf93
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: adb000e81724279868713fb78d4550d68c308f9b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752511"
---
# <a name="fetch-and-update-rowsets-odbc"></a>Recuperare e aggiornare set di righe (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

    
### <a name="to-fetch-and-update-rowsets"></a>Per recuperare e aggiornare set di righe  
  
1.  Facoltativamente, chiamare [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) con SQL_ROW_ARRAY_SIZE per modificare il numero di righe (R) nel set di righe.  
  
2.  Chiamare [SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md) o [SQLFetchScroll](../../../relational-databases/native-client-odbc-api/sqlfetchscroll.md) per ottenere un set di righe.  
  
3.  Se si utilizzano colonne associate, utilizzare i valori dei dati e le lunghezze dei dati disponibili nei buffer delle colonne associate per il set di righe.  
  
     Se vengono utilizzate colonne non associate, per ogni riga chiamare [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md) con SQL_POSITION per impostare la posizione del cursore; quindi, per ogni colonna non associata:  
  
    -   Chiamare [SQLGetData](../../../relational-databases/native-client-odbc-api/sqlgetdata.md) una o più volte per ottenere i dati per le colonne non vincolate dopo l'ultima colonna associata del set di righe. Le chiamate a [SQLGetData](../../../relational-databases/native-client-odbc-api/sqlgetdata.md) devono essere in ordine di numero di colonna crescente.  
  
    -   Chiamare [SQLGetData](../../../relational-databases/native-client-odbc-api/sqlgetdata.md) più volte per ottenere dati da una colonna di tipo text o image.  
  
4.  Configurare tutte le colonne data-at-execution di tipo text o image.  
  
5.  Chiamare [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md) o [SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md) per impostare la posizione del cursore, aggiornare, aggiornare, eliminare o aggiungere righe all'interno del set di righe.  
  
     Se si utilizzano colonne data-at-execution di tipo text o image per un'operazione di aggiornamento o di aggiunta, è necessario gestirle.  
  
6.  Facoltativamente, eseguire un'istruzione UPDATE o DELETE posizionata, specificando il nome del cursore, disponibile da [SQLGetCursorName](../../../relational-databases/native-client-odbc-api/sqlgetcursorname.md), e utilizzando un handle di istruzione diverso nella stessa connessione.  
  
## <a name="see-also"></a>Vedere anche  
 [Procedure per l'utilizzo di cursori &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-how-to/cursors/using-cursors-how-to-topics-odbc.md)  
  
