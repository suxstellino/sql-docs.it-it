---
title: Gestire le dimensioni del file di log delle transazioni | Microsoft Docs
description: Informazioni su come monitorare le dimensioni di un log delle transazioni di SQL Server, compattare il log, aumentare le dimensioni di un file di log, ottimizzare la velocità di aumento del log delle transazioni tempdb e controllare l'aumento delle dimensioni di un log delle transazioni.
ms.custom: ''
ms.date: 01/05/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- transaction logs [SQL Server], size management
- manage log size
- log size, manage
ms.assetid: 3a70e606-303f-47a8-96d4-2456a18d4297
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 699052eedf2a32faac9f3bf849f5f4aaa4c43b4c
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237696"
---
# <a name="manage-the-size-of-the-transaction-log-file"></a>Gestire le dimensioni del file di log delle transazioni
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Questo argomento descrive come monitorare le dimensioni di un log delle transazioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], compattare il log delle transazioni, aumentare le dimensioni di un file di log delle transazioni, ottimizzare la velocità di aumento del log delle transazioni **tempdb** e controllare l'aumento delle dimensioni di un file di log delle transazioni.  

##  <a name="monitor-log-space-use"></a><a name="MonitorSpaceUse"></a>Monitorare l'uso dello spazio del log  
Monitorare l'uso dello spazio del log tramite [sys.dm_db_log_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md). Questo DMV restituisce informazioni sulla quantità di spazio del log attualmente usata e indica quando il log delle transazioni deve essere troncato. 

Per informazioni sulle dimensioni correnti di un file di log, sulle relative dimensioni massime e sull'opzione di aumento automatico delle dimensioni per il file, è anche possibile usare le colonne **size**, **max_size** e **growth** per il file di log in [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md).  
  
> [!IMPORTANT]
> Evitare l'overload del disco del log. Verificare che la risorsa di archiviazione log sia in grado di sostenere i requisiti [IOPS](https://wikipedia.org/wiki/IOPS) e di bassa latenza per il carico di lavoro transazionale. 
  
##  <a name="shrink-log-file-size"></a><a name="ShrinkSize"></a> Compattare il file di log  
 Per ridurre la dimensione fisica di un file di log fisico, è necessario ridurre il file di log. Ciò può risultare utile quando si sa che un file di log delle transazioni contiene spazio inutilizzato. È possibile compattare un file di log solo mentre il database è online ed è disponibile almeno un [file di log virtuale (VLF)](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch). In alcuni casi, la compattazione del log potrebbe non essere possibile finché il log non viene troncato.  
  
> [!NOTE]
> Fattori come una transazione con esecuzione prolungata, che mantiene i [VLF](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) virtuali attivi per un lungo periodo di tempo, possono limitare in tutto o in parte la compattazione del log. Per informazioni, vedere [Fattori che possono posticipare il troncamento del log](../../relational-databases/logs/the-transaction-log-sql-server.md#FactorsThatDelayTruncation).  
  
Il processo di compattazione di un file di log comporta la rimozione di uno o più [VLF](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) che non contengono alcuna parte del log logico, ovvero dei *VLF* inattivi. Quando si compatta un file di log delle transazioni vengono rimossi dalla fine del file di log alcuni file VLF inattivi, per ridurre il log approssimativamente alle dimensioni della destinazione. 

> [!IMPORTANT]
> Prima di ridurre le dimensioni del log delle transazioni, tenere presenti i [fattori che possono posticipare il troncamento del log](../../relational-databases/logs/the-transaction-log-sql-server.md#FactorsThatDelayTruncation). Se dopo una compattazione del log è di nuovo necessario lo spazio di archiviazione, il log delle transazioni torna a crescere e durante tale crescita origina un overhead delle prestazioni. Per altre informazioni, vedere [Indicazioni](#Recommendations) in questo argomento.
  
 **Compattare un file di log senza compattare i file di database**  
  
-   [DBCC SHRINKFILE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md)  
  
-   [Compattare un file](../../relational-databases/databases/shrink-a-file.md)  
  
 **Monitorare gli eventi di compattazione dei file di log**  
  
-   [Log File Auto Shrink Event Class](../../relational-databases/event-classes/log-file-auto-shrink-event-class.md).  
  
 **Monitorare lo spazio del log**  
  
-   [sys.dm_db_log_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql.md)  
  
-   [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md) (Vedere le colonne **size**, **max_size** e **growth** per il file o i file di log).  
  
##  <a name="add-or-enlarge-a-log-file"></a><a name="AddOrEnlarge"></a> Aggiungere o aumentare le dimensioni di un file di log  
È possibile guadagnare spazio aumentando le dimensioni del file di log esistente, se lo spazio è sufficiente, o aggiungendo un file di log al database, in genere in un disco diverso. Un file di log delle transazioni è sufficiente, a meno che lo spazio del log sia in esaurimento e anche lo spazio su disco sia in esaurimento nel volume che contiene il file di log.   
  
-   Per aggiungere un file di log al database, usare la clausola `ADD LOG FILE` dell'istruzione `ALTER DATABASE`. L'aggiunta di un file di log consente l'aumento delle dimensioni del log.  
-   Per espandere il file di log usare la clausola `MODIFY FILE` dell'istruzione `ALTER DATABASE` specificando la sintassi `SIZE` e `MAXSIZE`. Per altre informazioni, vedere [Opzioni per file e filegroup ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).  

Per altre informazioni, vedere [Indicazioni](#Recommendations) in questo argomento.
    
##  <a name="optimize-tempdb-transaction-log-size"></a><a name="tempdbOptimize"></a> Ottimizzare le dimensioni del log delle transazioni di tempdb  
 Il riavvio di un'istanza del server riporta il log delle transazioni del database **tempdb** alle dimensioni originali, antecedenti all'aumento automatico delle dimensioni. Questo può comportare una riduzione delle prestazioni del log delle transazioni di **tempdb** . 
 
 Per evitare tale overhead, è possibile incrementare le dimensioni del log delle transazioni di **tempdb** dopo l'avvio o il riavvio dell'istanza del server. Per altre informazioni, vedere [tempdb Database](../../relational-databases/databases/tempdb-database.md).  
  
##  <a name="control-transaction-log-file-growth"></a><a name="ControlGrowth"></a> Controllare l'aumento delle dimensioni di un file di log delle transazioni  
 Per gestire la crescita di un file di log delle transazioni, usare l'istruzione descritta in [Opzioni per file e filegroup ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md). Tenere presente quanto segue:  
  
-   Per modificare le dimensioni del file corrente in unità KB, MB, GB e TB usare l'opzione `SIZE`.  
-   Per modificare l'incremento di crescita, usare l'opzione `FILEGROWTH`. Il valore 0 indica che l'aumento automatico delle dimensioni è disattivato e non è consentita l'allocazione di spazio aggiuntivo.  
-   Per controllare le dimensioni massime di un file di log in unità KB, MB, GB e TB o per impostare la crescita su UNLIMITED, usare l'opzione `MAXSIZE`.  

Per altre informazioni, vedere [Indicazioni](#Recommendations) in questo argomento.

## <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni
Di seguito sono elencate alcune indicazioni di carattere generale relative all'uso dei file registro transazioni:

-   L'incremento automatico (autogrow) delle dimensioni del log delle transazioni, definito dall'opzione `FILEGROWTH`, deve essere sufficiente a soddisfare le esigenze delle transazioni del carico di lavoro. È consigliabile specificare un incremento di crescita per un file di log sufficientemente grande da consentire di evitare l'espansione frequente. Un buon indicatore per il dimensionamento corretto di un log delle transazioni è la quantità di spazio del log occupato durante:
    -  Il tempo necessario per l'esecuzione di un backup completo, perché i backup del log non possono verificarsi finché non termina il backup completo.
    -  Il tempo necessario per le operazioni di manutenzione dell'indice più grande.
    -  Il tempo necessario per eseguire il batch più grande in un database.

-   Quando si imposta **autogrow** per i file di dati e di log usando l'opzione `FILEGROWTH` può essere preferibile impostare **size** (dimensioni) anziché **percentage** (percentuale), per consentire un controllo migliore del rapporto di crescita, dato che percentage corrisponde a un valore in continua crescita.
    -  Tenere presente che i log delle transazioni non possono sfruttare [Inizializzazione immediata dei file](../../relational-databases/databases/database-instant-file-initialization.md), quindi i tempi di crescita dei file di log estesi sono particolarmente importanti. 
    -  Come procedura consigliata, evitare di impostare un valore dell'opzione `FILEGROWTH` superiore a 1024 MB per i log delle transazioni. I valori predefiniti dell'opzione `FILEGROWTH` sono:  
  
      |Versione|Valori predefiniti|  
      |-------------|--------------------|  
      |A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|Dati 64 MB. File di log 64 MB.|  
      |A partire da [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|Dati 1 MB. File di log 10%.|  
      |Prima di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|Dati 10%. File di log 10%.|  

-   Un incremento della crescita ridotto può generare molti file [VLF](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) piccoli e ridurre le prestazioni. Per determinare la distribuzione dei file di log virtuali ottimale per le dimensioni correnti del log delle transazioni di tutti i database in un'istanza specifica e gli incrementi della crescita necessari per ottenere le dimensioni richieste, vedere questo [script](https://github.com/Microsoft/tigertoolbox/tree/master/Fixing-VLFs).

-   Un incremento della crescita elevato può generare pochi file [VLF](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#physical_arch) di grandi dimensioni e ridurre a sua volta le prestazioni. Per determinare la distribuzione dei file di log virtuali ottimale per le dimensioni correnti del log delle transazioni di tutti i database in un'istanza specifica e gli incrementi della crescita necessari per ottenere le dimensioni richieste, vedere questo [script](https://github.com/Microsoft/tigertoolbox/tree/master/Fixing-VLFs). 

-   Anche con l'aumento automatico attivato è possibile che si riceva un messaggio indicante che il log delle transazioni è pieno, se questo non può crescere a sufficienza per soddisfare le esigenze della query. Per altre informazioni su come modificare l'incremento della crescita, vedere [Opzioni per file e filegroup ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)

-   La presenza di più file di log in un database non migliora le prestazioni, perché i file di log delle transazioni non usano il [riempimento proporzionale](../../relational-databases/pages-and-extents-architecture-guide.md#ProportionalFill) come i file di dati nello stesso gruppo di file.  

-   È possibile impostare i file di log in modo che vengano compattati automaticamente. Tuttavia questa impostazione **è sconsigliata** e la proprietà del database **auto_shrink** è FALSE per impostazione predefinita. Se **auto_shrink** è impostata su TRUE, la compattazione automatica riduce le dimensioni di un file solo quando più del 25% dello spazio del file risulta inutilizzato. 
    -   Il file viene compattato fino a quando la percentuale di spazio inutilizzato nel file non risulta pari al 25 percento oppure fino a quando il file non raggiunge le dimensioni originali, a seconda di quale tra questi due sia il valore maggiore. 
    -   Per informazioni sulla modifica dell'impostazione della proprietà **auto_shrink**, vedere [Visualizzare o modificare le proprietà di un database](../../relational-databases/databases/view-or-change-the-properties-of-a-database.md) e [Opzioni ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md). 
  
## <a name="see-also"></a>Vedere anche  
[BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
[Risolvere i problemi relativi a un log delle transazioni completo &#40;Errore di SQL Server 9002&#41;](../../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md)    
[Backup del log delle transazioni nella Guida sull'architettura e gestione del log delle transazioni in SQL Server](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#Backups)    
[Backup di log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)    
[Opzioni per file e filegroup ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)
