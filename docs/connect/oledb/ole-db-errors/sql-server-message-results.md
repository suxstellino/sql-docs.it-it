---
title: Risultati dei messaggi di SQL Server (OLE DB Driver)
description: Informazioni sulle istruzioni Transact-SQL che non generano set di righe OLE DB Driver per SQL Server o un conteggio e sui valori restituiti previsti.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, errors
- errors [OLE DB], SQL Server message results
- OLE DB error handling, SQL Server message results
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dabbcef5f1d894f4d9a9c08230a13f60a269ce2f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104741721"
---
# <a name="sql-server-message-results"></a>Risultati dei messaggi di SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

Le istruzioni [!INCLUDE[tsql](../../../includes/tsql-md.md)] seguenti non generano set di righe di OLE DB Driver per SQL Server o un conteggio delle righe interessate durante l'esecuzione:  
  
-   PRINT  
  
-   RAISERROR con gravità minore o uguale a 10  
  
-   DBCC  
  
-   SET SHOWPLAN  
  
-   SET STATISTICS  
  
 Queste istruzioni restituiscono uno o più messaggi informativi o determinano la restituzione da parte di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] di messaggi informativi in sostituzione dei risultati del conteggio o del set di righe. Al completamento dell'esecuzione, OLE DB Driver per SQL Server restituisce S_OK e i messaggi sono disponibili per il consumer di OLE DB Driver per SQL Server.  
  
 OLE DB Driver per SQL Server restituisce S_OK e include uno o più messaggi informativi disponibili in seguito all'esecuzione di più istruzioni [!INCLUDE[tsql](../../../includes/tsql-md.md)] o all'esecuzione da parte del consumer di una funzione membro di OLE DB Driver per SQL Server.  
  
Il consumer OLE DB Driver per SQL Server è autorizzato alla specifica dinamica del testo della query. Il consumer deve controllare le interfacce di errore dopo _ogni_ esecuzione della funzione membro. Deve sempre eseguire questi controlli, quale che sia il valore del codice restituito, sia che venga restituito o meno un riferimento di interfaccia a un elemento `IRowset` o `IMultipleResults`, e indipendentemente dal numero di righe interessate.
  
## <a name="see-also"></a>Vedere anche  
 [Errori](../../oledb/ole-db-errors/errors.md)  
  
  
