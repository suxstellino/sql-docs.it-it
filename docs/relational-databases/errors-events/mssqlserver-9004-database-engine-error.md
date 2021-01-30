---
description: MSSQLSERVER_9004
title: MSSQLSERVER_9004 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 9004 (Database Engine error)
ms.assetid: b528bb49-340c-4a81-9c8d-cefce6562f16
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: b4706982db7c2c121af0658142344c0558321b52
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161971"
---
# <a name="mssqlserver_9004"></a>MSSQLSERVER_9004
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|9004|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|LOG_CORRUPT|  
|Testo del messaggio|Si è verificato un errore durante l'elaborazione del log del database '%.*ls'.  Se possibile, ripristinarlo da un backup. Se non è disponibile un backup, potrebbe essere necessario ricompilare il log.|  
  
## <a name="explanation"></a>Spiegazione  
Si è verificato un errore nell'elaborazione del log durante un'operazione di rollback, recupero o replica. Potrebbe trattarsi di un errore rilevato dal sistema operativo o di un errore di consistenza interno rilevato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] esegue verifiche logiche sulla coerenza del contenuto del log delle transazioni durante la lettura e l'elaborazione del log. Non vengono controllati tutti gli aspetti dell'intestazione, dei blocchi e dei record del log. Il numero State (Stato) offre altre informazioni sul tipo di errore:

 - **State 1**: l'intestazione file di log del file di log virtuale (VLF) è danneggiata.  Se viene rilevata un'intestazione del file di registro danneggiata durante l'avvio del database al momento dell'avvio del servizio, è possibile che in ERRORLOG (log degli errori) sia visualizzato solo l'errore 9004. L'intestazione del file di log è la prima parte di ogni VLF all'interno di un log delle transazioni. L'intestazione del file di log non corrisponde all'intestazione del file singolo, ovvero ai primi 8 kB del file di log. Se l'intestazione di file del file di log è danneggiata, è possibile che si riceva un messaggio 5172, simile a quello originato per il danneggiamento della pagina di intestazione di un file di database.
 - **State 2 e State 3**: un blocco del log non era valido al momento dell'esecuzione del ripristino durante un'operazione RESTORE.
 - **State da 4 a 12**: questi sono i vari controlli sui blocchi di log durante l'elaborazione dei record di log. Includono la parità, il settore e altri controlli logici sulla coerenza del log delle transazioni.

Nella maggior parte dei casi questo errore si verifica solo in ERRORLOG o nel log eventi dell'applicazione di Windows con EventId = 9004, perché l'operazione che elabora il log non è basata su un comando utente diretto (ad esempio il ripristino eseguito all'avvio del motore SQL Server). In queste situazioni l'errore 9004 viene spesso visualizzato insieme all'errore 3414. Tuttavia, alcune query come ALTER DATABASE potrebbero richiedere un'elaborazione del log e pertanto visualizzeranno questi errori. Poiché la gravità dell'errore è Severity=21, la sessione utente viene disconnessa.

## <a name="cause"></a>Causa
L'errore 9004 è un errore generale che indica che il contenuto del log delle transazioni è danneggiato. Il motivo per cui il log perde la coerenza è simile a qualsiasi problema di danneggiamento del database rilevato dal motore SQL Server. Per trovare il motivo del danneggiamento del log, è consigliabile adottare tecniche simili a quelle usate per il danneggiamento del database, inclusa l'analisi di possibili problemi a livello di hardware, file system e I/O. Si noti che l'esecuzione di DBCC CHECKDB non include il controllo del log delle transazioni e non è in grado di rilevare errori di danneggiamento del log. L'errore 9004 viene generato dal motore SQL Server stesso.

## <a name="user-action"></a>Azione dell'utente  
Per correggere l'errore, eseguire una delle operazioni seguenti:  
  
-   **Eseguire il ripristino da un backup**:  eseguire il ripristino da un backup valido noto per risolvere questo problema. Se la parte log di un backup del database o del log include contenuti danneggiati, è possibile che si registri un errore 9004 in RESTORE. In questa situazione, il log delle transazioni nel backup è danneggiato.
  
-   **Ricompilare il log**:  se non è possibile eseguire il ripristino da un backup, la ricompilazione del log delle transazioni potrebbe consentire di riportare online il database. È importante esaminare con cura le conseguenze della ricostruzione del log delle transazioni. Una di queste è la *possibile perdita di coerenza delle transazioni nel database*. Per altre informazioni su come ricostruire il log delle transazioni, vedere [Risoluzione degli errori in modalità di emergenza per il database](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md#resolving-errors-in-database-emergency-mode).
  
-   **Esaminare i log per rilevare problemi di sistema**: esaminare anche il registro eventi e i log degli errori di sistema per trovare all'interno del sistema le cause che possono aver generato il problema.  
  
