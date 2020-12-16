---
title: Sincronizzare i dati | Microsoft Docs
description: Per sincronizzazione dei dati nella replica si intende la propagazione delle modifiche di dati e schema tra il server di pubblicazione e i Sottoscrittori in SQL Server.
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- synchronization [SQL Server replication], about synchronization
- merge replication synchronization [SQL Server replication]
- scripts [SQL Server replication], synchronization and
- synchronization [SQL Server replication]
- snapshot replication [SQL Server], synchronization
- transactional replication, synchronization
- subscriptions [SQL Server replication], synchronizing
- on demand script execution
- replication [SQL Server], synchronization
- scripts [SQL Server replication]
ms.assetid: 724802f7-7d69-46d3-a330-bd8aa7f53114
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: c75b15ddd296d85ba7df44e97aa33dfca33ae8f2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460144"
---
# <a name="synchronize-data"></a>Sincronizzare i dati
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  La sincronizzazione dei dati è il processo di propagazione dei dati e delle modifiche di schema tra il server di pubblicazione e i Sottoscrittori dopo l'applicazione dello snapshot iniziale al Sottoscrittore. La sincronizzazione può verificarsi:  
  
-   Continuamente, come avviene in genere per la replica transazionale.  
  
-   Su richiesta, come avviene in genere per la replica di tipo merge.  
  
-   In base a una pianificazione, come avviene in genere con la replica snapshot.  
  
 Quando una sottoscrizione viene sincronizzata, si verificano processi differenti in base al tipo di replica in uso:  
  
-   Replica snapshot. La sincronizzazione indica che l'agente di distribuzione riapplica lo snapshot al Sottoscrittore in modo che i dati e lo schema del database di sottoscrizione siano consistenti con il database di pubblicazione.  
  
     Se nel server di pubblicazione sono state apportate modifiche ai dati o allo schema, è necessario generare un nuovo snapshot per poter propagare le modifiche al Sottoscrittore.  
  
-   Replica transazionale. La sincronizzazione indica che l'agente di distribuzione trasferisce aggiornamenti, inserimenti, eliminazioni e qualsiasi altra modifica dal database di distribuzione al Sottoscrittore.  
  
-   Replica di tipo merge. La sincronizzazione indica che l'agente di tipo merge carica le modifiche dal Sottoscrittore nel server di pubblicazione e quindi le scarica dal server di pubblicazione nel Sottoscrittore. Gli eventuali conflitti vengono rilevati e risolti. Al termine viene eseguita la convergenza dei dati e nel server di pubblicazione e in tutti i Sottoscrittori saranno disponibili gli stessi valori di dati. Se sono stati rilevati e risolti dei conflitti, il commit del lavoro eseguito da alcuni utenti viene modificato allo scopo di risolvere il conflitto in base ai criteri definiti.  
  
 Le pubblicazioni snapshot aggiornano completamente lo schema nel Sottoscrittore a ogni sincronizzazione, in modo da applicare a quest'ultimo tutte le modifiche di schema. La replica transazionale e la replica di tipo merge supportano inoltre le modifiche di schema più comuni. Per altre informazioni, vedere [Apportare modifiche allo schema nei database di pubblicazione](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).  
  
 Per sincronizzazione di una sottoscrizione push, vedere [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
 Per sincronizzazione di una sottoscrizione pull, vedere [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
 Per impostare le pianificazioni della sincronizzazione, vedere [Specify Synchronization Schedules](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
 **Per visualizzare e risolvere i conflitti di sincronizzazione**  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]: [Visualizzare e risolvere i conflitti di dati per le pubblicazioni di tipo merge &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/view-and-resolve-data-conflicts-for-merge-publications.md)  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]: [Visualizzare i conflitti di dati per le pubblicazioni transazionali &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/view-data-conflicts-for-transactional-publications-sql-server-management-studio.md)  
  
## <a name="executing-code-during-synchronization"></a>Esecuzione di codice durante la sincronizzazione  
 La replica supporta due modalità di esecuzione del codice durante la sincronizzazione  
  
-   L'esecuzione degli script su richiesta è supportata per la replica transazionale e di tipo merge. L'utilizzo di questa modalità di esecuzione consente di specificare che uno script SQL venga eseguito durante la sincronizzazione. Lo script viene copiato nel Sottoscrittore ed eseguito utilizzando **sqlcmd** all'inizio del processo di sincronizzazione. Tale script non dispone di accesso alle modifiche replicate applicate al Sottoscrittore. Per altre informazioni, vedere [Eseguire script durante la sincronizzazione &#40;programmazione Transact-SQL della replica&#41;](../../relational-databases/replication/execute-scripts-during-synchronization-replication-transact-sql-programming.md).  
  
-   La replica di tipo merge supporta i gestori della logica di business. Grazie all'utilizzo del framework di gestione della logica di business è possibile scrivere un assembly del codice gestito che viene chiamato durante il processo di sincronizzazione di tipo merge. L'assembly include la logica di business che consente di rispondere a diverse situazioni durante la sincronizzazione, ad esempio modifiche ai dati, conflitti ed errori. Per altre informazioni, vedere [Eseguire logiche di business durante la sincronizzazione di tipo merge](../../relational-databases/replication/merge/execute-business-logic-during-merge-synchronization.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Rilevare e risolvere i conflitti tra repliche di tipo merge](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)  
  
  
