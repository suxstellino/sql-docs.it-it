---
description: MSSQL_REPL027056
title: MSSQL_REPL027056 | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_REPL027056 error
ms.assetid: 92d62f3c-b8ae-482e-a348-2e9a8ee9786e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 82c2afc30c295b390ba76ceed4c24c95b6b0d666
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206155"
---
# <a name="mssql_repl027056"></a>MSSQL_REPL027056
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Dettagli messaggio  
  
|Attributo|valore|  
|-|-|  
|Nome prodotto|SQL Server|  
|ID evento|27056|  
|Origine evento|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nome simbolico||  
|Testo del messaggio|Impossibile modificare la cronologia di generazione in '%1'. Per risolvere il problema, riavviare la sincronizzazione con la registrazione dettagliata della cronologia e specificare un file di output in cui registrare i dati.|  
  
## <a name="explanation"></a>Spiegazione  
 Questo errore viene solitamente generato come risultato della contesa in tabelle di sistema della replica di tipo merge che hanno raggiunto una dimensione eccessiva. L'eccessivo aumento delle dimensioni delle tabelle di sistema è in genere dovuto a un lungo periodo di memorizzazione della pubblicazione, in quanto i metadati devono essere archiviati in queste tabelle fino al raggiungimento del periodo di memorizzazione.  
  
## <a name="user-action"></a>Azione dell'utente  
 **Per risolvere il problema:**  
  
1.  Ridurre il valore dei parametri **DownloadGenerationsPerBatch** e **-UploadGenerationsPerBatch** per l'agente di merge in modo da consentire la continuazione dell'elaborazione mentre si risolve il problema sottostante che causa l'errore. I parametri degli agenti possono essere specificati nei profili agente e dalla riga di comando. Per altre informazioni, vedere:  
  
    -   [Usare i profili agenti di replica](../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)  
  
    -   [Visualizzare e modificare i parametri del prompt dei comandi dell'agente di replica &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md)  
  
    -   [Replication Agent Executables Concepts](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
2.  Specificare l'impostazione più bassa possibile per il periodo di memorizzazione della pubblicazione. Per altre informazioni, vedere [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
3.  Come parte della manutenzione per la replica di tipo merge, controllare occasionalmente l'aumento delle dimensioni delle tabelle di sistema associate alla replica di tipo merge: **MSmerge_contents**, **MSmerge_genhistory** e **MSmerge_tombstone**, **MSmerge_current_partition_mappings** e **MSmerge_past_partition_mappings**. Reindicizzare periodicamente queste tabelle. Per altre informazioni, vedere [Riorganizzare e ricompilare gli indici](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Guida di riferimento a errori ed eventi &#40;replica&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
