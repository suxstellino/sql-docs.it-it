---
title: Checkpoint di database (SQL Server) | Microsoft Docs
description: Informazioni sui checkpoint, punti validi noti da cui il motore di database di SQL Server può iniziare ad applicare le modifiche contenute nel log durante il ripristino.
ms.date: 04/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.custom: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- automatic checkpoints
- transaction logs [SQL Server], checkpoints
- logs [SQL Server], active
- pages [SQL Server], dirty
- MinLSN
- checkpoints [SQL Server]
- pages [SQL Server], flushing
- dirty pages
- transaction logs [SQL Server], active logs
- recovery interval option [SQL Server]
- buffer cache [SQL Server]
- logs [SQL Server], checkpoints
- Minimum Recovery LSN
- flushing pages
- active logs
ms.assetid: 98a80238-7409-4708-8a7d-5defd9957185
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9277d551762afcff062a6a1e1fc389009e5ea2ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351205"
---
# <a name="database-checkpoints-sql-server"></a>Checkpoint di database (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
 Un *checkpoint* crea un punto valido noto da cui [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] può iniziare ad applicare le modifiche contenute nel log durante il recupero successivo a un arresto anomalo del sistema.

##  <a name="overview"></a><a name="Overview"></a> Panoramica   
Per motivi correlati alle prestazioni, tramite il [!INCLUDE[ssDE](../../includes/ssde-md.md)] vengono apportate modifiche alle pagine del database in memoria, nella cache buffer, ma queste pagine non vengono scritte su disco dopo ogni modifica. Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] pubblica periodicamente un checkpoint su ogni database. Un *checkpoint* scrive le pagine modificate in memoria correnti, note come pagine *dirty*, e le informazioni sul log delle transazioni dalla memoria sul disco e registra le informazioni nel log delle transazioni.  
  
 Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] supporta molti tipi di checkpoint: automatici, indiretti, manuali e interni. Nella tabella seguente vengono riepilogati i tipi di **checkpoint**
  
|Nome|[!INCLUDE[tsql](../../includes/tsql-md.md)] Interfaccia|Descrizione|  
|----------|----------------------------------|-----------------|  
|Automatico|EXEC sp_configure **'** intervallo di recupero **','** _secondi_ **'**|Emesso automaticamente in background per rispettare il limite di tempo superiore suggerito dall'opzione di configurazione del server **intervallo di recupero** . I checkpoint automatici vengono eseguiti fino al completamento.  I checkpoint automatici sono limitati in base al numero di scritture in sospeso e al fatto che il [!INCLUDE[ssDE](../../includes/ssde-md.md)] rilevi o meno un aumento della latenza di scrittura superiore ai 50 millisecondi.<br /><br /> Per altre informazioni, vedere [Configurare l'opzione di configurazione del server recovery interval](../../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md).|  
|Indiretto|ALTER DATABASE ... SET TARGET_RECOVERY_TIME **=** _target\_recovery\_time_ { SECONDS &#124; MINUTES }|Emesso in background per rispettare un tempo di recupero di destinazione specificato dall'utente per un determinato database. A partire da [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)], il valore predefinito è 1 minuto. Il valore predefinito è 0 per le versioni precedenti, a indicare che il database userà checkpoint automatici la cui frequenza dipende dall'impostazione dell'intervallo di recupero dell'istanza del server.<br /><br /> Per altre informazioni, vedere [Modificare il tempo di recupero di riferimento di un database &#40;SQL Server&#41;](../../relational-databases/logs/change-the-target-recovery-time-of-a-database-sql-server.md).|  
|Manuale|CHECKPOINT [*checkpoint_duration*]|Emesso quando si esegue un comando CHECKPOINT [!INCLUDE[tsql](../../includes/tsql-md.md)] . Il checkpoint manuale si verifica nel database corrente per la connessione. Per impostazione predefinita, i checkpoint manuali vengono eseguiti fino al completamento. La limitazione funziona come per i checkpoint automatici.  Facoltativamente, il parametro *checkpoint_duration* specifica una quantità di tempo richiesta, in secondi, per il completamento del checkpoint.<br /><br /> Per altre informazioni, vedere [CHECKPOINT &#40;Transact-SQL&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md).|  
|Interno|No.|Emesso da varie operazioni del server quali backup e creazione dello snapshot del database per garantire che le immagini del disco corrispondano allo stato corrente del log.|  
  
> [!NOTE]
> L'opzione di impostazione avanzata **-k** di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente a un amministratore del database di limitare il comportamento di I/O del checkpoint in base alla velocità effettiva del sottosistema di I/O per alcuni tipi di checkpoint. L'opzione di impostazione **-k** riguarda i checkpoint automatici e i checkpoint interni e manuali senza limitazione.  
  
 Per i checkpoint automatici, manuali e interni, solo le modifiche apportate dopo l'ultimo checkpoint devono essere sottoposte a rollforward durante il recupero del database. Ne consegue una riduzione del tempo necessario per recuperare un database.  
  
> [!IMPORTANT]
> Le transazioni di cui non è stato eseguito il commit con esecuzione prolungata aumentano il tempo di recupero per tutti i tipi di checkpoint.   
  
##  <a name="interaction-of-the-target_recovery_time-and-recovery-interval-options"></a><a name="InteractionBwnSettings"></a> Interazione delle opzioni TARGET_RECOVERY_TIME e 'intervallo di recupero'  
 La tabella seguente riepiloga l'interazione tra l'impostazione **sp_configure '** intervallo di recupero **'** a livello di server e l'impostazione `ALTER DATABASE ... TARGET_RECOVERY_TIME` specifica del database.  
  
|target_recovery_time|'intervallo di recupero'|Tipo di checkpoint utilizzato|  
|----------------------------|-------------------------|-----------------------------|  
|0|0|checkpoint automatici il cui intervallo di recupero di destinazione è pari a 1 minuto.|  
|0|>0|Checkpoint automatici il cui intervallo di recupero di destinazione è specificato dall'impostazione definita dall'utente dell'opzione **sp_configure 'intervallo di recupero'** .|  
|>0|Non applicabile.|Checkpoint indiretti il cui tempo di recupero di destinazione è determinato dall'impostazione TARGET_RECOVERY_TIME, espresso in secondi.|  
  
##  <a name="automatic-checkpoints"></a><a name="AutomaticChkpt"></a> Checkpoint automatici  
Si verifica un checkpoint automatico ogni volta che il numero di record di log raggiunge il numero elaborabile dal [!INCLUDE[ssDE](../../includes/ssde-md.md)] nel tempo specificato nell'opzione di configurazione del server **intervallo di recupero** . Per altre informazioni, vedere [Configurare l'opzione di configurazione del server recovery interval](../../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md).
 
In ogni database senza un tempo di recupero di destinazione definito dall'utente, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera checkpoint automatici. La frequenza dipende dall'opzione di configurazione del server avanzata **intervallo di recupero** che specifica il tempo massimo che un'istanza del server deve usare per recuperare un database durante un riavvio del sistema. Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] valuta il numero massimo di record di log che può elaborare nell'intervallo di recupero. Quando un database che usa i checkpoint automatici raggiunge il numero massimo specificato di record di log, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] pubblica un checkpoint sul database. 
 
L'intervallo di tempo tra checkpoint automatici può essere **estremamente** variabile. In un database con un considerevole carico di lavoro di transazioni si verificheranno checkpoint più frequenti rispetto a un database usato principalmente per le operazioni in sola lettura. In base al modello di recupero con registrazione minima, viene messo in coda anche un checkpoint automatico se il log è pieno al 70%.  
  
In base al modello di recupero con registrazione minima, a meno che alcuni fattori non ritardino il troncamento del log, un checkpoint automatico tronca la sezione inutilizzata del log delle transazioni. Al contrario, in base ai modelli di recupero con registrazione minima delle operazioni bulk e con registrazione completa, dopo avere stabilito una catena di backup del log, i checkpoint automatici non causano il troncamento del log. Per altre informazioni, vedere [Log delle transazioni &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).  
  
Dopo un arresto anomalo del sistema, la quantità di tempo necessaria per recuperare un database dipende in larga misura dalla quantità di I/O casuale necessario per ripristinare le pagine dirty al momento dell'arresto anomalo del sistema. Ciò significa che l'impostazione **intervallo di recupero** non è affidabile. Non è in grado di determinare una durata accurata del recupero. Inoltre, quando è in corso un checkpoint automatico, l'attività di I/O generale per i dati aumenta in modo significativo e imprevedibile.  
   
###  <a name="impact-of-recovery-interval-on-recovery-performance"></a><a name="PerformanceImpact"></a> Impatto dell'intervallo di recupero sulle prestazioni di recupero  
In un sistema di elaborazione delle transazioni online (OLTP) che usa transazioni brevi, il tempo identificato da **intervallo di recupero** costituisce il fattore che influisce maggiormente sul tempo di recupero. L'opzione **intervallo di recupero** non influisce tuttavia sul tempo necessario per annullare una transazione con esecuzione prolungata. Il recupero di un database con una transazione con esecuzione prolungata può richiedere molto più tempo rispetto al valore indicato nell'impostazione **Intervallo di recupero**. 
 
Ad esempio, se prima dell'interruzione dell'istanza del server sono stati eseguiti aggiornamenti con una transazione con esecuzione prolungata che hanno richiesto due ore, l'operazione di recupero durerà molto più a lungo rispetto al periodo di tempo specificato in **intervallo di recupero** per il recupero della transazione. Per informazioni sull'impatto di una transazione con esecuzione prolungata sul tempo di recupero, vedere [Log delle transazioni &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md). Per altre informazioni sul processo di recupero, vedere [Panoramica del ripristino e del recupero (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery).
  
In genere, i valori predefiniti forniscono prestazioni di recupero ottimali. Tuttavia, la modifica dell'intervallo di recupero potrebbe migliorare le prestazioni nelle circostanze seguenti:  
  
-   Se il recupero richiede regolarmente più di 1 minuto quando non viene eseguito il rollback delle transazioni con esecuzione prolungata.  
  
-   Se si nota che checkpoint frequenti riducono le prestazioni su un database.  
  
Se si decide di aumentare l'impostazione **recovery interval** , è consigliabile aumentarla gradualmente di piccoli incrementi e valutare l'effetto di ogni aumento incrementale sulle prestazioni del recupero. Questo approccio è importante perché man mano che l'impostazione **intervallo di recupero** viene aumentata, il recupero del database richiederà una quantità di tempo equivalente a tale impostazione. Ad esempio, se si imposta l'**intervallo di recupero** su 10 minuti, la procedura di recupero richiederà un tempo 10 volte superiore rispetto a quello che richiederebbe se l'**intervallo di recupero** fosse impostato su 1 minuto.  
  
##  <a name="indirect-checkpoints"></a><a name="IndirectChkpt"></a> Checkpoint indiretti
I checkpoint indiretti, nuovi in [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], offrono un'alternativa a livello di database configurabile ai checkpoint automatici. Per questa configurazione specificare l'opzione di configurazione del database **target recovery time**. Per altre informazioni, vedere [Modificare il tempo di recupero di riferimento di un database &#40;SQL Server&#41;](../../relational-databases/logs/change-the-target-recovery-time-of-a-database-sql-server.md).
In caso di un arresto anomalo del sistema, i checkpoint indiretti consentono un tempo di recupero potenzialmente più veloce e più prevedibile rispetto ai checkpoint automatici. I checkpoint indiretti offrono i vantaggi riportati di seguito:  
  
-   Un carico di lavoro transazionale online su un database configurato per i checkpoint indiretti può subire un calo delle prestazioni. I checkpoint indiretti assicurano che il numero di pagine dirty sia inferiore a una determinata soglia, in modo che il recupero del database venga completato entro il tempo di recupero di riferimento. 

  A differenza dei **checkpoint indiretti**, che usano il numero di pagine dirty, l'opzione di configurazione **Intervallo di recupero** usa il numero di transazioni per determinare il tempo di recupero. Quando i checkpoint indiretti sono abilitati per un database che riceve un numero elevato di operazioni DML, il writer in background può iniziare a scaricare i buffer dirty su disco in modo intensivo per garantire che il tempo necessario per eseguire il recupero non superi il tempo di recupero di riferimento impostato per il database. Questo può causare in determinati sistemi ulteriore attività di I/O che può comportare un collo di bottiglia delle prestazioni se il sottosistema del disco opera al di sopra o in prossimità della soglia di I/O.  
  
-   I checkpoint indiretti consentono di controllare in modo affidabile il tempo di recupero del database tramite factoring del costo di operazioni di I/O casuali durante REDO. In questo modo si consente a un'istanza del server di rimanere entro il limite superiore per i tempi di recupero per un determinato database, tranne quando una transazione con esecuzione prolungata causa tempi UNDO eccessivi.  
  
-   I checkpoint indiretti riducono i picchi di I/O correlati ai checkpoint scrivendo continuamente pagine dirty su disco in background.  
  
Tuttavia, un carico di lavoro transazionale online su un database configurato per i checkpoint indiretti può subire un calo delle prestazioni. Questo perché il writer in background utilizzato dai checkpoint indiretti aumenta talvolta il carico di scrittura totale di un'istanza del server.  
 
> [!IMPORTANT]
> I checkpoint indiretti rappresentano il comportamento predefinito per i nuovi database creati in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], inclusi i database modello e i database TempDB.          
> I database aggiornati sul posto o ripristinati da una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] useranno il comportamento precedente basato sui checkpoint automatici, a meno che non vengano modificati in modo esplicito per l'uso dei checkpoint indiretti.       

### <a name="improved-indirect-checkpoint-scalability"></a><a name="ctp23"></a> Scalabilità migliorata per il checkpoint indiretto
Nelle versioni precedenti a [!INCLUDE[ssNoVersion](../../includes/sssql19-md.md)] è possibile riscontrare errori con l'utilità di pianificazione che non cede il controllo quando il database genera un numero elevato di pagine dirty, ad esempio `tempdb`. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] introduce una scalabilità migliorata per il checkpoint indiretto che consente di evitare questi errori nei database con un carico di lavoro eccessivo di `UPDATE`/`INSERT`.
  
##  <a name="internal-checkpoints"></a><a name="EventsCausingChkpt"></a> Checkpoint interni  
I checkpoint interni vengono generati da vari componenti server per garantire che le immagini sul disco corrispondano allo stato corrente del log. I checkpoint interni vengono generati in risposta agli eventi seguenti:  
  
-   Vengono aggiunti o rimossi file di database utilizzando l'istruzione ALTER DATABASE.  
  
-   Viene eseguito un backup del database.  
  
-   Viene creato uno snapshot del database, in modo esplicito o internamente per DBCC CHECKDB.  
  
-   Viene eseguita un'attività che richiede la chiusura di un database. Ad esempio, AUTO_CLOSE è impostata su ON e la connessione al database dell'ultimo utente viene chiusa, oppure viene eseguita una modifica a un'opzione di database che richiede un riavvio del database.  
  
-   Un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene arrestata in seguito all'arresto del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (MSSQLSERVER). Entrambe le azioni causano un checkpoint in ogni database dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Impostazione della modalità offline per un'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .      
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Related tasks  
 **Per modificare l'intervallo di recupero su un'istanza del server**  
  
-   [Configurare l'opzione di configurazione del server intervallo di recupero](../../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md)  
  
 **Per configurare checkpoint indiretti su un database**  
  
-   [Modificare il tempo di recupero di riferimento di un database &#40;SQL Server&#41;](../../relational-databases/logs/change-the-target-recovery-time-of-a-database-sql-server.md)  
  
 **Per emettere un checkpoint manuale su un database**  
  
-   [CHECKPOINT &#40;Transact-SQL&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md)  

## <a name="see-also"></a>Vedere anche  
[Log delle transazioni &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)            
[Architettura e gestione del log delle transazioni di SQL Server](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)      
 
