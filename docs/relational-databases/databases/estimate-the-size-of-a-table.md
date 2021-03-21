---
title: Stimare le dimensioni di un tabella | Microsoft Docs
description: Usare questa procedura per valutare la quantità di spazio necessaria per l'archiviazione dei dati in una tabella in SQL Server.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- pages [SQL Server], space
- space [SQL Server], tables
- row size [SQL Server]
- size [SQL Server], tables
- column size [SQL Server]
- predicting table size [SQL Server]
- table size [SQL Server]
- estimating table size
- clustered indexes, table size
- disk space [SQL Server], tables
- space allocation [SQL Server], table size
- designing databases [SQL Server], estimating size
- reserved free rows per page [SQL Server]
- calculating table size
ms.assetid: 15c17c92-616f-402e-894b-907a296efe5f
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 07cd28395a2413bf4c0411a37a3d6d502a1aae3f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754901"
---
# <a name="estimate-the-size-of-a-table"></a>Stima delle dimensioni di una tabella
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Per stimare la quantità di spazio necessario per archiviare dati in una tabella, è possibile utilizzare la procedura seguente:  
  
1.  Calcolare lo spazio necessario per l'heap o indice cluster seguendo le istruzioni in [Stima delle dimensioni di un heap](../../relational-databases/databases/estimate-the-size-of-a-heap.md) o [Stima delle dimensioni di un indice cluster](../../relational-databases/databases/estimate-the-size-of-a-clustered-index.md).  
  
2.  Calcolare lo spazio necessario per ogni indice non cluster seguendo le istruzioni disponibili in [Stima delle dimensioni di un indice non cluster](../../relational-databases/databases/estimate-the-size-of-a-nonclustered-index.md).  
  
3.  Sommare i valori calcolati nei passaggi 1 e 2.  

## <a name="see-also"></a>Vedere anche  
 [Stima delle dimensioni di un database](../../relational-databases/databases/estimate-the-size-of-a-database.md)   
 [Stima delle dimensioni di un heap](../../relational-databases/databases/estimate-the-size-of-a-heap.md)   
 [Stima delle dimensioni di un indice cluster](../../relational-databases/databases/estimate-the-size-of-a-clustered-index.md)   
 [Stima delle dimensioni di un indice non cluster](../../relational-databases/databases/estimate-the-size-of-a-nonclustered-index.md)  
  
  
