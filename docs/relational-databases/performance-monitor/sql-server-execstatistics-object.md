---
title: Oggetto ExecStatistics di SQL Server | Microsoft Docs
description: Informazioni sull'oggetto SQLServer:ExecStatistics, che include contatori che consentono di monitorare diversi tipi di esecuzioni.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:ExecStatistics
- ExecStatistics object
ms.assetid: 4f8557a8-345f-4622-a8a5-763a0388ad94
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 67ef3667057a2cb5e2507d89a515814d2ea427e6
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505736"
---
# <a name="sql-server-execstatistics-object"></a>Oggetto ExecStatistics di SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  L'oggetto **SQLServer:ExecStatistics** di Microsoft SQL Server include contatori che consentono di monitorare diversi tipi di esecuzioni.  
  
 Nella tabella seguente vengono descritti i contatori **Exec Statistics** di SQL Server.  
  
|Contatori Exec Statistics di SQL Server|Descrizione|  
|-----------------------------------------|-----------------|  
|**Query distribuite**|Statistiche relative all'esecuzione di query distribuite.|  
|**Chiamate DTC**|Statistiche relative all'esecuzione di chiamate DTC.|  
|**Procedure estese**|Statistiche relative all'esecuzione di procedure estese.|  
|**Chiamate OLEDB**|Statistiche relative all'esecuzione di chiamate OLEDB.|  
  
 Per ogni contatore nell'oggetto sono disponibili le istanze seguenti:  
  
|Elemento|Descrizione|  
|----------|-----------------|  
|**Tempo di esecuzione (ms) medio**|Durata media per il tipo di esecuzione selezionato.|  
|**Tempo di esecuzione (ms) cumulativo al secondo**|Tempo di esecuzione complessivo al secondo per il tipo di esecuzione selezionato.|  
|**Esecuzioni in corso**|Numero di esecuzioni in corso per il tipo di esecuzione selezionato.|  
|**Esecuzioni avviate al secondo**|Numero di esecuzioni avviate al secondo per il tipo di esecuzione selezionato.|  
  
## <a name="see-also"></a>Vedere anche  
 [Monitorare l'utilizzo delle risorse &#40;Monitor di sistema&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
