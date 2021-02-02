---
description: Replica in sottoscrittori di tabelle con ottimizzazione per la memoria
title: Replica in sottoscrittori di tabelle con ottimizzazione per la memoria | Microsoft Docs
ms.custom: ''
ms.date: 11/21/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
ms.assetid: 1a8e6bc7-433e-471d-b646-092dc80a2d1a
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: a2dd5e112230540c519fad2b4926d70f374bbcfc
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236326"
---
# <a name="replication-to-memory-optimized-table-subscribers"></a>Replica in sottoscrittori di tabelle con ottimizzazione per la memoria
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Le tabelle con funzione di snapshot e sottoscrittori di replica transazionale, esclusa la replica transazionale peer-to-peer, possono essere configurate come tabelle ottimizzate per la memoria. Le altre configurazioni di replica non sono compatibili con le tabelle ottimizzate per la memoria. Questa funzionalità è disponibile a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)].  
  
## <a name="two-configurations-are-required"></a>Sono necessarie due configurazioni  
  
-   **Configurare il database sottoscrittore per supportare la replica per le tabelle ottimizzate per la memoria**  
  
     Impostare la proprietà **\@memory_optimized** su **true** con [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) o [sp_changesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md).  
  
-   **Configurare l'articolo in modo da supportare la replica per le tabelle ottimizzate per la memoria**  
  
     Impostare l'opzione `@schema_option = 0x40000000000` per l'articolo con [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) o [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md).  
  
#### <a name="to-configure-a-memory-optimized-table-as-a-subscriber"></a>Per configurare una tabella ottimizzata per la memoria come sottoscrittore  
  
1.  Creare una pubblicazione transazionale. Per altre informazioni, vedere [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md).  
  
2.  Aggiungere articoli alla pubblicazione. Per altre informazioni, vedere [definire un articolo](../../relational-databases/replication/publish/define-an-article.md).  
  
     Se si esegue la configurazione con [!INCLUDE[tsql](../../includes/tsql-md.md)] impostare il parametro **\@schema_option** della stored procedure **sp_addarticle** su   
    **0x40000000000**.  
  
3.  Nella finestra Proprietà articolo impostare **Abilita ottimizzazione per la memoria** su **true**.  
  
4.  Avviare il processo dell'agente snapshot per generare lo snapshot iniziale per la pubblicazione. Per altre informazioni, vedere [Creazione e applicazione dello snapshot iniziale](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
5.  Creare ora una nuova sottoscrizione. In **Creazione guidata nuova sottoscrizione** impostare **Sottoscrizione con ottimizzazione per la memoria** su **true**.  

 Le tabelle con ottimizzazione per la memoria dovrebbero ora iniziare a ricevere aggiornamenti dal server di pubblicazione.  
  
#### <a name="reconfigure-an-existing-transaction-replication"></a>Riconfigurare una replica transazionale esistente  
  
1.  Passare alle proprietà della sottoscrizione in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] e impostare **Sottoscrizione con ottimizzazione** per la memoria su **true**. Le modifiche non vengono applicate prima della reinizializzazione della sottoscrizione.  
  
     Se si esegue la configurazione con [!INCLUDE[tsql](../../includes/tsql-md.md)] impostare il nuovo parametro **\@memory_optimized** della stored procedure **sp_addsubscription** su true.  
  
2.  Passare alle proprietà articolo per una pubblicazione in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] e impostare **Abilita ottimizzazione per la memoria** su true.  
  
     Se si esegue la configurazione con [!INCLUDE[tsql](../../includes/tsql-md.md)] impostare il parametro **\@schema_option** della stored procedure **sp_addarticle** su   
    **0x40000000000**.  
  
3.  Le tabelle con ottimizzazione per la memoria non supportano gli indici cluster. Per far sì che la replica possa gestirlo convertendolo in un indice non cluster nella destinazione, impostare **Converti indice cluster in indice non cluster per un articolo con ottimizzazione per la memoria** su true.  
  
     Se si esegue la configurazione con [!INCLUDE[tsql](../../includes/tsql-md.md)] impostare il parametro **\@schema_option** della stored procedure **sp_addarticle** su **0x0000080000000000**.  
  
4.  Rigenerare lo snapshot.  
  
5.  Reinizializzare la sottoscrizione.  
  
## <a name="remarks-and-restrictions"></a>Osservazioni e restrizioni  
 È supportata una sola replica transazionale unidirezionale. La replica transazionale peer-to-peer non è supportata.  
  
 Non è possibile pubblicare le tabelle con ottimizzazione per la memoria.  
  
 Non è possibile configurare le tabelle di replica nel distributore come tabelle ottimizzate per la memoria.  
  
 La replica di tipo merge non può includere tabelle ottimizzate per la memoria.  
  
 Nel sottoscrittore le tabelle interessate dalla replica transazionale possono essere configurate come tabelle ottimizzate per la memoria, ma le tabelle del sottoscrittore devono soddisfare i requisiti delle tabelle ottimizzate per la memoria. Si applicano pertanto le restrizioni seguenti.  
 
-   Le tabelle replicate in tabelle ottimizzate per la memoria in un sottoscrittore sono limitate ai tipi di dati consentiti nelle tabelle ottimizzate per la memoria. Per altre informazioni, vedere [Tipi di dati supportati](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md).  
  
-   Non tutte le funzionalità di Transact-SQL sono supportate con le tabelle ottimizzate per la memoria. Per altri dettagli, vedere [Costrutti Transact-SQL non supportati da OLTP in memoria](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md).  
  
##  <a name="modifying-a-schema-file"></a><a name="Schema"></a> Modifica di un file dello schema  
  
-   Se si utilizza l'opzione della tabella ottimizzata per la memoria `DURABILITY = SCHEMA_AND_DATA`, la tabella deve disporre di un indice di chiave primaria non cluster.  
  
-   ANSI_PADDING deve essere ON.  
  
  
  
