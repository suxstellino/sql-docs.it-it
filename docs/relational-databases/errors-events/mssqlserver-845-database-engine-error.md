---
description: MSSQLSERVER_845
title: MSSQLSERVER_845 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 845 (Database Engine error)
ms.assetid: 8fff6ad4-234c-44be-b123-e25d5e1cd63e
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: af7bef2a8888428e8d0b818278d712e73c3df513
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186768"
---
# <a name="mssqlserver_845"></a>MSSQLSERVER_845
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|845|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|BUFLATCH_TIMEOUT|  
|Testo del messaggio|Timeout durante l'attesa di un latch del buffer di tipo %d per la pagina %S_PGID, ID di database %d.|  
  
## <a name="explanation"></a>Spiegazione  
Un processo era in attesa di acquisire un latch, ma ha attesto fino al tempo limite e non ha potuto acquisirne uno. Ciò può verificarsi se il completamento di un'operazione di I/O richiede una quantità di tempo eccessiva, in genere a causa di un blocco dei processi di sistema determinato da altre attività. In alcuni casi, tuttavia, può essere dovuto a un errore hardware.  
  
## <a name="cause"></a>Causa
Questo messaggio di errore dipende dall'ambiente complessivo del sistema. Le circostanze seguenti potrebbero comportare un sovraccarico del sistema:

- Hardware che non soddisfa le esigenze di input/output (I/O) e memoria
- Impostazioni configurate e testate in modo non corretto
- Progettazione inefficiente

 È possibile rilevare l'errore 845 quando il sistema è sottoposto a un carico elevato e non è in grado di soddisfare le richieste del carico di lavoro. Alcune delle cause più comuni di un ambiente sovraccaricato sono:

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
- Se i messaggi di errore 845 non sono frequenti, è possibile ignorarli