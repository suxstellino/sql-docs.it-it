---
description: MSSQLSERVER_846
title: MSSQLSERVER_846 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 846 (Database Engine error)
ms.assetid: ccf367eb-06b0-42b8-b4d6-2b88f4a502d3
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 94fc86afb22e8bcb3db6dea94b1f06b62bb43282
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186757"
---
# <a name="mssqlserver_846"></a>MSSQLSERVER_846
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|846|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|N/D|  
|Testo del messaggio|Timeout durante l'attesa del latch del buffer: tipo %d, bp %p, pagina %d:%d, stat %#x, ID database: %d, ID unità di allocazione:%I64d%ls, attività 0x%p : %d, attesa %d, flag 0x%I64x, attività proprietaria 0x%p. L'attesa verrà interrotta.|  
  
## <a name="explanation"></a>Spiegazione  
È possibile che un computer si arresti oppure che si verifichi un timeout o altre interruzioni delle operazioni regolari nel momento in cui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] scrive errori di latch del buffer nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
Se nel messaggio il campo stat ha il valore di 0x04 attivato, significa che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in attesa di un'operazione di I/O. Nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potrebbe inoltre essere presente il messaggio [MSSQLSERVER_833](~/relational-databases/errors-events/mssqlserver-833-database-engine-error.md).  
  
Se nel messaggio il campo stat ha il valore 0x04 disattivato, significa che si sta verificando un'intensa contesa per una pagina. Se l'oggetto è costituito da una pagina di dati, è possibile che il problema sia causato da una progettazione di codice non efficiente. Se la pagina non contiene dati, è possibile che l'errore si verifichi a causa di colli di bottiglia del server, ad esempio risorse hardware insufficienti.  
  
## <a name="user-action"></a>Azione dell'utente  
Per risolvere il problema, eseguire uno o più dei passaggi seguenti che, a seconda dell'ambiente in uso, potrebbero consentire di ridurre o eliminare i messaggi di errore:  
  
-   Determinare se è presente un collo di bottiglia dell'hardware. Se necessario, aggiornare l'hardware in modo che supporti i requisiti di configurazione, query e carico dell'ambiente in uso. Per altre informazioni sui colli di bottiglia, vedere [Individuare i colli di bottiglia](~/relational-databases/performance/identify-bottlenecks.md).  
  
-   Controllare gli errori registrati ed eseguire tutti gli strumenti di diagnostica offerti dal fornitore dell'hardware.  
  
-   Verificare che le unità disco non siano compresse. L'archiviazione di dati o file di log nelle unità compresse non è supportata. Per altre informazioni sui file fisici, vedere [Filegroup e file di database](~/relational-databases/databases/database-files-and-filegroups.md).  
  
-   Verificare se i messaggi di errore non vengono più visualizzati quando si disattivano le opzioni seguenti:  
  
    -   Opzione di configurazione priority boost di SQL Server  
  
    -   Opzione lightweight pooling (modalità fiber)  
  
    -   Opzione set working set size  
  
    > [!NOTE]  
    > La modifica dell'impostazione predefinita OFF delle opzioni precedenti può di frequente risultare controproducente. Per altre informazioni sulle impostazioni, vedere [Opzioni di configurazione del server &#40;SQL Server&#41;](~/database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
-   Ottimizzare le query per ridurre le risorse utilizzate nel sistema. L'ottimizzazione delle prestazioni consente di ridurre il sovraccarico del sistema e migliorare il tempo di risposta per le query individuali.  
  
-   Impostare l'opzione AUTO_SHRINK su OFF per ridurre l'overhead delle modifiche alle dimensioni del database.  
  
-   Verificare di aver impostato l'opzione FILEGROWTH su incrementi di dimensioni tali da risultare poco frequenti. Pianificare un processo che consenta di controllare lo spazio disponibile nei database e quindi aumentare le dimensioni dei database durante i periodi di attività ridotta.  
  
