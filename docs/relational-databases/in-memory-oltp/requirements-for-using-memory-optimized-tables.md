---
title: Requisiti per l'uso di tabelle con ottimizzazione per la memoria | Microsoft Docs
description: Informazioni sui requisiti per l'uso di OLTP in memoria, tra cui la versione del database SQL, le considerazioni sulla memoria e archiviazione e l'installazione.
ms.custom: ''
ms.date: 11/24/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 47d9a7e8-c597-4b95-a58a-dcf66df8e572
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0cbe2b75a46e63b5e388b91ace3d74c0db3be49b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170733"
---
# <a name="requirements-for-using-memory-optimized-tables"></a>Requisiti per l'utilizzo di tabelle con ottimizzazione per la memoria
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Per l'uso di OLTP in memoria nel database di Azure, vedere [Introduzione alle tecnologie in memoria (anteprima) in database SQL](/azure/azure-sql/in-memory-oltp-overview).  
  
 In aggiunta ai requisiti indicati in [Requisiti hardware e software per l'installazione di SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md), per usare OLTP in memoria sono necessari i requisiti seguenti:  
  
-   [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 o versione successiva, qualsiasi edizione. Per [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] e [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] RTM (pre-SP1) sono necessarie le edizioni Enterprise, Developer o Evaluation.
    
    > [!NOTE]
    > OLTP in memoria richiede la versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a 64 bit.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] richiede memoria sufficiente per contenere i dati in tabelle ottimizzate per la memoria e indici, nonché memoria aggiuntiva per supportare il carico di lavoro in linea. Per altre informazioni, vedere [Stimare i requisiti di memoria delle tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md) .  

-   Quando si esegue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in una macchina virtuale (VM), assicurarsi che la memoria allocata alla macchina virtuale sia sufficiente per supportare la memoria necessaria per gli indici e le tabelle ottimizzate per la memoria. A seconda dell'applicazione host della macchina virtuale, l'opzione di configurazione per garantire l'allocazione di memoria per la macchina virtuale può essere chiamata prenotazione di memoria oppure, se viene usata la memoria dinamica, RAM minima. Assicurarsi che queste impostazioni siano sufficienti per soddisfare le esigenze dei database in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
-   Liberare una quantità di spazio su disco corrispondente al doppio delle dimensioni delle tabelle ottimizzate per la memoria durevoli.  
  
-   Un processore deve supportare l'istruzione **cmpxchg16b** per l'uso di OLTP in memoria. Tutti i moderni processori a 64 bit supportano **cmpxchg16b**.  
  
     Se si usa una macchina virtuale e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] visualizza un errore causato da un processore di versione precedente, controllare se l'applicazione host della macchina virtuale include un'opzione di configurazione che consente l'uso di **cmpxchg16b**. In caso contrario, è possibile usare Hyper-, che supporta **cmpxchg16b** senza dover modificare l'opzione di configurazione.  
  
-   OLTP in memoria viene installato come parte dei **servizi del motore di database**.  
  
     Per installare la generazione del report ([Determinare se una tabella o una stored procedure deve essere trasferita a OLTP in memoria](../../relational-databases/in-memory-oltp/determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp.md)) e [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (per gestire OLTP In memoria tramite Esplora oggetti [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]), [scaricare SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).   
  
## <a name="important-notes-on-using-hek_2"></a>Note importanti sull'uso di [!INCLUDE[hek_2](../../includes/hek-2-md.md)]  
  
-   A partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] non esiste alcun limite alle dimensioni delle tabelle ottimizzate per la memoria, ad eccezione della memoria disponibile. 

-   In [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], le dimensioni totali in memoria di tutte le tabelle durevoli di un database non devono superare i 250 GB. Per altre informazioni, vedere [Stimare i requisiti di memoria delle tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md).  

> [!NOTE]
> A partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1, le edizioni Standard ed Express supportano OLTP in memoria, ma impongono quote sulla quantità di memoria che è possibile usare per le tabelle ottimizzate per la memoria in un database specifico. Nell'edizione Standard tale quota è di 32 GB per ogni database; nell'edizione Express è di 352 MB per ogni database. 
  
-   Se si creano uno o più database con tabelle ottimizzate per la memoria, è consigliabile abilitare l'inizializzazione immediata concedendo all'account di avvio del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] il diritto utente *SE_MANAGE_VOLUME_NAME*. Senza l'inizializzazione immediata dei file, i file di archiviazione ottimizzati per la memoria (file di dati e differenziali) vengono inizializzati al momento della creazione. Tale operazione può causare una riduzione delle prestazioni del carico di lavoro. Per altre informazioni sull'inizializzazione immediata dei file e su come abilitarla, vedere [Inizializzazione immediata dei file di database](../../relational-databases/databases/database-instant-file-initialization.md).
  
## <a name="see-also"></a>Vedere anche  
 [OLTP in memoria &#40;ottimizzazione per la memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
 [Inizializzazione immediata dei file di database](../../relational-databases/databases/database-instant-file-initialization.md)  
 [Guida all'architettura di gestione della memoria](../../relational-databases/memory-management-architecture-guide.md)
