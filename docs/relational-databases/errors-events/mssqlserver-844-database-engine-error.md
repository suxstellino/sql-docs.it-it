---
description: MSSQLSERVER_844
title: MSSQLSERVER_844 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 844 (Database Engine error)
ms.assetid: 2060c886-1226-4066-bc0c-de90a1cfb82b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 0ae116b0ef06d82111e8ca356ae3f9c9a272f729
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186796"
---
# <a name="mssqlserver_844"></a>MSSQLSERVER_844
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|844|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|BUFLATCH_TIMEOUT_CONTINUE|  
|Testo del messaggio|Timeout durante l'attesa del latch del buffer: tipo %d, bp %p, pagina %d:%d, stat %#x, ID database: %d, ID unità di allocazione:%I64d%ls, attività 0x%p: %d, attesa %d, flag 0x%I64x, attività proprietaria 0x%p.  L'attesa verrà protratta.|  
  
## <a name="explanation"></a>Spiegazione
Un processo SQL è in attesa di acquisire un latch. Il problema può essere causato da un'operazione di I/O il cui completamento richiede troppo tempo. Questo tipo di errore in genere è dovuto ad altre attività che bloccano i processi di sistema. In alcuni casi, questo errore può essere dovuto a un problema hardware.  Quando viene visualizzato questo messaggio di errore, è possibile notare che il computer e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] smettono di rispondere.

## <a name="cause"></a>Causa
Questo messaggio di errore dipende dall'ambiente complessivo del sistema. Le circostanze seguenti potrebbero comportare un sovraccarico del sistema:

- Hardware che non soddisfa le esigenze di input/output (I/O) e memoria
- Impostazioni configurate e testate in modo non corretto
- Progettazione inefficiente

 È possibile rilevare l'errore 844 quando il sistema è sottoposto a un carico elevato e non è in grado di soddisfare le richieste del carico di lavoro. Alcune delle cause più comuni di un ambiente sovraccaricato sono:

- Problemi hardware
- Volumi compressi
- Impostazioni di configurazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non predefinite
- Progettazione inefficiente di query o indice
- Operazioni frequenti di aumento automatico o compattazione automatica del database

## <a name="user-action"></a>Azione dell'utente  
Per evitare che si verifichi questo errore, provare le soluzioni seguenti:  
  
- Verificare se sono presenti colli di bottiglia dell'hardware. Per iniziare, vedere [Individuazione dei colli di bottiglia](../performance/identify-bottlenecks.md). Se necessario, aggiornare l'hardware in modo che sia in grado di soddisfare le esigenze di configurazione, query e carico dell'ambiente.

- Verificare che tutto l'hardware funzioni correttamente. Controllare gli errori registrati ed eseguire tutti gli strumenti di diagnostica offerti dal fornitore dell'hardware. Verificare la presenza di errori di I/O associati nel log degli errori o nel log eventi. Gli errori di I/O in genere sono dovuti a un malfunzionamento del disco.  
- Assicurarsi che i volumi dei dischi non siano compressi. L'archiviazione di dati e file di log nelle unità compresse non è supportata. Vedere [Filegroup e file di database](../databases/database-files-and-filegroups.md). Per altre informazioni sul supporto di unità compresse, vedere l'articolo seguente: [Database di SQL Server non supportati in volumi compressi](https://support.microsoft.com/EN-US/help/231347)

- Verificare se i messaggi di errore scompaiono quando si disattivano tutte le opzioni di configurazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] seguenti:
   - L'[opzione priority boost](../../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md)
   - L'[opzione lightweight pooling (modalità fiber)](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md)
   - L'[opzione set working set size](../../database-engine/configure-windows/set-working-set-size-server-configuration-option.md)

    Per altre informazioni, vedere [Procedura: Determinare le impostazioni di configurazione corrette di SQL Server](https://support.microsoft.com/EN-US/help/319942)

- Ottimizzare le query per ridurre le risorse utilizzate nel sistema. L'ottimizzazione delle prestazioni consente di ridurre il sovraccarico del sistema e di migliorare il tempo di risposta per le singole query
- Impostare la proprietà AutoShrink su OFF per ridurre il sovraccarico delle modifiche apportate alle dimensioni del database
- Assicurarsi di impostare la proprietà AutoGrow su incrementi sufficientemente grandi da ridurne la frequenza. Pianificare un processo per controllare lo spazio disponibile nei database e quindi aumentare le dimensioni dei database durante i periodi di attività ridotta.
- Controllare nel log degli errori la presenza di attività che non cedono la precedenza e di altri errori critici. Risolvere prima questi errori, perché potrebbero puntare alla causa radice del problema sottostante.
- Se si verificano spesso errori critici, ad esempio di asserzione, risolvere questi problemi
- Se i messaggi di errore 844 non sono frequenti, è possibile ignorarli