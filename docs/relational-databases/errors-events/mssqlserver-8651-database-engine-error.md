---
description: MSSQLSERVER_8651
title: MSSQLSERVER_8651 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 8651 (Database Engine error)
ms.assetid: 4cc3498d-5449-4c4e-b1f9-3271831c725a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1892dbdba3b39f9b5d66837f58c596e2f7a3b759
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203984"
---
# <a name="mssqlserver_8651"></a>MSSQLSERVER_8651
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|8651|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|MEMGRANT_ERR|  
|Testo del messaggio|Impossibile eseguire l'operazione richiesta perché non è disponibile la quantità di memoria minima per la query. Diminuire il valore dell'opzione di configurazione del server 'min memory per query'.|  
  
## <a name="explanation"></a>Spiegazione  
Altri processi stanno utilizzando la memoria del server, ovvero stanno inviando un numero eccessivo di richieste di memoria al server.  
  
## <a name="user-action"></a>Azione dell'utente  
Diminuire il valore dell'opzione di configurazione del server 'min memory per query' oppure ridurre il carico di query inviate al server.  
  
Nell'elenco seguente viene illustrata la procedura generale per la risoluzione degli errori di memoria:  
  
1.  Verificare se altre applicazioni o servizi utilizzano la memoria nel server specificato. Riconfigurare le applicazioni o i servizi meno critici per utilizzare una quantità di memoria inferiore.  
  
2.  Iniziare a raccogliere i dati dei contatori di Performance Monitor per **SQL Server: Gestione buffer**, **SQL Server: Memory Manager**.  
  
3.  Verificare i seguenti parametri di configurazione della memoria di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
    -   **max server memory**  
  
    -   **min server memory**  
  
    -   **min memory per query**  
  
    Valutare eventuali impostazioni non comuni e, se necessario, correggerle. Le impostazioni predefinite sono elencate nell'argomento "Impostazione delle opzioni di configurazione del server" nella documentazione online di SQL Server.  
  
4.  Verificare il carico di lavoro (ad esempio, numero di sessioni simultanee, query attualmente in esecuzione).  

Per aumentare la quantità di memoria disponibile per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], effettuare le operazioni seguenti:  
  
-   Se le risorse vengono utilizzate da altre applicazioni oltre a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], provare ad arrestarne l'esecuzione oppure a eseguirle in un server distinto. In questo modo sarà possibile eliminare le richieste di memoria esterne.  
  
-   Se è stata configurata l'opzione **max server memory,** , aumentarne il valore impostato.  
  
Eseguire i comandi DBCC seguenti per liberare diverse cache in memoria di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   DBCC FREESYSTEMCACHE  
  
-   DBCC FREESESSIONCACHE  
  
-   DBCC FREEPROCCACHE  
  
Se il problema persiste, sarà necessario analizzarlo in modo più dettagliato e cercare di ridurre il carico di lavoro.  
  
## <a name="see-also"></a>Vedere anche  
[DBCC FREESYSTEMCACHE &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-freesystemcache-transact-sql.md)  
[DBCC FREESESSIONCACHE &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-freesessioncache-transact-sql.md)  
[DBCC FREEPROCCACHE &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md)  
[Opzioni di configurazione del server &#40;SQL Server&#41;](~/database-engine/configure-windows/server-configuration-options-sql-server.md)  
[Oggetto Buffer Manager di SQL Server](~/relational-databases/performance-monitor/sql-server-buffer-manager-object.md)  
[Oggetto Memory Manager di SQL Server](~/relational-databases/performance-monitor/sql-server-memory-manager-object.md)  
  
