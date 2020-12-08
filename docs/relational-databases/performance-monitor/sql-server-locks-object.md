---
title: Oggetto Locks di SQL Server | Microsoft Docs
description: Informazioni sull'oggetto SQLServer:Locks, che offre informazioni sui blocchi di SQL Server per i singoli tipi di risorse.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Locks object
- SQLServer:Locks
ms.assetid: ace04f0d-3993-4444-8317-ca39d7087e49
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 94e726a108c78ffec8715e80c4b14f2274656be0
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505665"
---
# <a name="sql-server-locks-object"></a>Oggetto Locks di SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  L'oggetto **SQLServer:Locks** di Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] offre informazioni sui blocchi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per i singoli tipi di risorse. I blocchi sulle risorse di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ad esempio sulle righe lette o modificate durante una transazione, impediscono che le risorse vengano utilizzate contemporaneamente da transazioni diverse. Ad esempio, se una transazione mantiene attivo un blocco esclusivo (X) su una riga all'interno di una tabella, nessun'altra transazione potrà modificare la riga fino a quando il blocco non viene rilasciato. La riduzione dei blocchi aumenta la concorrenza e, di conseguenza, potrebbe migliorare le prestazioni. È possibile monitorare contemporaneamente più istanze dell'oggetto **Locks** , che rappresentano i singoli blocchi sui tipi di risorse.  
  
 Nella tabella seguente vengono descritti i contatori dell'oggetto **Locks** di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Contatori di SQLServer Locks|Descrizione|  
|-------------------------------|-----------------|  
|**Tempo medio di attesa (ms)**|Tempo medio di attesa (in millisecondi) per ogni richiesta di blocco che ha comportato un periodo di attesa.|  
|**Base tempo medio di attesa**|Solo per uso interno.|
|**Richieste di blocco/sec**|Numero di nuovi blocchi e conversioni di blocco al secondo richiesti da Gestione blocchi.|  
|**Timeout blocchi (timeout > 0)/sec**|Numero di richieste di blocco al secondo per le quali si è verificato un timeout, incluse le richieste interne di blocchi NOWAIT.|  
|**Timeout blocchi/sec**|Numero di richieste di blocco al secondo per le quali si è verificato un timeout, incluse le richieste interne di blocchi NOWAIT.|  
|**Tempo di attesa blocchi (ms)**|Tempo di attesa totale dei blocchi (in millisecondi) nell'ultimo secondo.|  
|**Attese di blocco/sec**|Numero di richieste di blocco al secondo che richiedono un periodo di attesa del chiamante.|  
|**Numero di deadlock/sec**|Numero di richieste di blocco al secondo che hanno generato un deadlock.|  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è possibile bloccare le risorse seguenti.  
  
|Elemento|Descrizione|  
|----------|-----------------|  
|**_Total**|Informazioni per tutti i blocchi.|  
|**AllocUnit**|Un blocco su un'unità di allocazione.|  
|**Applicazione**|Un blocco su una risorsa specificata dall'applicazione.|  
|**Database**|Un blocco su un database, che include tutti gli oggetti nel database.|  
|**Extent**|Un blocco su un gruppo contiguo di 8 pagine.|  
|**File**|Un blocco su un file di database.|  
|**Heap o albero B**|Heap o albero B (HOBT). Un blocco su un heap di pagine di dati, oppure sull'albero B di un indice.|  
|**Chiave**|Un blocco su una riga in un indice.|  
|**Metadata**|Un blocco su un'informazione di catalogo, detta anche metadato.|  
|**Object**|Un blocco su una tabella, stored procedure, vista e così via, che include tutti i dati e gli indici. L'oggetto può essere qualsiasi elemento per il quale esista una voce in **sys.all_objects**.|  
|**Page**|Un blocco su una pagina di 8 kilobyte (KB) in un database.|  
|**RID**|ID di riga. Un blocco su una singola riga all'interno di un heap.|  
  
## <a name="see-also"></a>Vedere anche  
 [Monitorare l'utilizzo delle risorse &#40;Monitor di sistema&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
