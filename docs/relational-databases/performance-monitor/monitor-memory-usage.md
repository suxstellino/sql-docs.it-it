---
title: Monitorare l'uso della memoria | Microsoft Docs
description: "Monitorare un'istanza di SQL Server per verificare che l'utilizzo della memoria rientri negli intervalli standard. Usare i contatori Memoria: Byte disponibili e Memoria: Pagine/sec."
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- tuning databases [SQL Server], memory
- monitoring server performance [SQL Server], memory usage
- isolating memory [SQL Server]
- paging rate [SQL Server]
- memory [SQL Server], monitoring usage
- monitoring [SQL Server], memory usage
- low-memory conditions
- database monitoring [SQL Server], memory usage
- available memory [SQL Server]
- page faults [SQL Server]
- monitoring performance [SQL Server], memory usage
- server performance [SQL Server], memory
ms.assetid: 1aee3933-a11c-4b87-91b7-32f5ea38c87f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 93e2780c3e51ce46e0687864896c36b7d3166917
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/02/2020
ms.locfileid: "96506006"
---
# <a name="monitor-memory-usage"></a>Monitoraggio dell'utilizzo della memoria
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Monitorare periodicamente un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per verificare che l'utilizzo della memoria rientri negli intervalli standard.  
  
 Per monitorare la quantità di memoria disponibile, usare i seguenti contatori:  
  
-   **Memoria: Byte disponibili**  
  
-   **Memoria: Pagine/sec**  
  
 Il contatore **Byte disponibili** indica il numero di byte di memoria disponibili per i processi. Il contatore **Pagine/sec** indica il numero di pagine richiamate dal disco o scritte su disco per liberare spazio nel set di lavoro in seguito a errori di pagina.  
  
 Valori bassi del contatore **Byte disponibili** possono indicare una quantità di memoria insufficiente nel computer o la presenza di un'applicazione che non rilascia la memoria. Un valore elevato del contatore **Pagine/sec** può indicare un paging eccessivo. Monitorare il contatore **Memoria: Errori di pagina/sec** per assicurarsi che l'attività del disco non sia dovuta al paging.  
  
 Anche nei computer con una notevole quantità di memoria disponibile, la frequenza di paging, e quindi di errori di pagina, dovrebbe essere bassa. Microsoft Windows Virtual Memory Manager (VMM) sottrae pagine a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e agli altri processi in quanto riduce le dimensioni dei set di lavoro di tali processi. L'attività di VMM tende quindi a causare errori di pagina. Per determinare se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o un altro processo provoca un paging eccessivo, monitorare il contatore **Processo: Errori di pagina/sec** alla ricerca dell'istanza del processo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Per altre informazioni su come evitare il paging eccessivo, vedere la documentazione del sistema operativo Windows.  
  
## <a name="isolating-memory-used-by-sql-server"></a>Memoria usata da SQL Server  
 Per impostazione predefinita, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] i requisiti di memoria vengono modificati in modo dinamico in base alle risorse di sistema disponibili. Se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] necessita di una maggior quantità di memoria, richiede al sistema operativo di determinare se è disponibile memoria fisica e utilizza la memoria disponibile. Se la memoria disponibile per il sistema operativo non è sufficiente, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libererà memoria per il sistema operativo finché la condizione di memoria insufficiente non sarà stata risolta o finché [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non avrà raggiunto il limite minservermemory. È tuttavia possibile ignorare l'opzione per l'uso dinamico della memoria specificando le opzioni di configurazione del server **minservermemory** e **maxservermemory** . Per altre informazioni, vedere [Opzioni per la memoria server](../../database-engine/configure-windows/server-memory-server-configuration-options.md).  
  
 Per monitorare la quantità di memoria usata da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , esaminare i contatori delle prestazioni seguenti:  
  
-   **Processo: Working set**  
  
-   **SQL Server: Gestione buffer: Percentuale riscontri cache del buffer**  
  
-   **SQL Server: Gestione buffer: Pagine di database**  
  
-   **SQL Server: Gestione memoria: Memoria totale server (KB)**  
  
 Il contatore **Working set** indica la quantità di memoria usata da un processo. Se questo valore è costantemente inferiore alla quantità di memoria impostata dalle opzioni server **min server memory** e **max server memory** , [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è stato configurato per usare una quantità eccessiva di memoria.  
  
 Il contatore **Percentuale riscontri cache del buffer** è specifico per un'applicazione. È tuttavia preferibile un livello pari o superiore a 90%. Aggiungere memoria fino a raggiungere un valore costantemente superiore a 90%. Tale percentuale indica che è stato soddisfatto oltre il 90% di tutte le richieste di dati dalla cache dei dati.  
  
 Se il valore del contatore **Memoria totale server (KB)Memoria** è costantemente elevato rispetto alla quantità di memoria fisica del computer, può essere necessario aggiungere ulteriore memoria.  
  
## <a name="determining-current-memory-allocation"></a>Determinazione dell'allocazione di memoria corrente  
 La query seguente restituisce le informazioni sulla memoria attualmente allocata.  
  
```  
SELECT  
(physical_memory_in_use_kb/1024) AS Memory_usedby_Sqlserver_MB,  
(locked_page_allocations_kb/1024) AS Locked_pages_used_Sqlserver_MB,  
(total_virtual_address_space_kb/1024) AS Total_VAS_in_MB,  
process_physical_memory_low,  
process_virtual_memory_low  
FROM sys.dm_os_process_memory;  
```  
  
  
