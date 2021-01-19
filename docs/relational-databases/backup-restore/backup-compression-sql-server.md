---
title: Compressione dei backup (SQL Server) | Microsoft Docs
description: Informazioni sulla compressione dei backup di SQL Server, tra cui le restrizioni, i compromessi a livello di prestazioni, la configurazione della compressione dei backup e il rapporto di compressione.
ms.custom: ''
ms.date: 07/08/2020
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], backup compression
- backup compression [SQL Server], about backup compression
- compression [SQL Server], backup compression
- backups [SQL Server], compression
- backing up [SQL Server], backup compression
- backup compression [SQL Server]
ms.assetid: 05bc9c4f-3947-4dd4-b823-db77519bd4d2
author: cawrites
ms.author: chadam
ms.openlocfilehash: a98aaaea7d243853f31d19c0e7c3aa824ee2cc4e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171473"
---
# <a name="backup-compression-sql-server"></a>Compressione backup (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritta la compressione dei backup di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , incluse le restrizioni, il compromesso di prestazioni previsto dalla compressione di backup, la configurazione della compressione di backup e il rapporto di compressione.  La compressione dei backup è supportata nelle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: Enterprise, Standard e Developer.  Ogni edizione di [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e successiva è in grado di ripristinare un backup compresso. 
 
  
##  <a name="benefits"></a><a name="Benefits"></a> Vantaggi  
  
-   Poiché le dimensioni di un backup compresso sono minori di quelle di un backup non compresso degli stessi dati, la compressione di un backup richiede una minore quantità di I/O del dispositivo e pertanto la velocità del backup aumenta in genere in modo significativo.  
  
     Per ulteriori informazioni, vedere [Impatto sulle prestazioni della compressione di backup](#PerfImpact), più avanti in questo argomento.  
  
  
##  <a name="restrictions"></a><a name="Restrictions"></a> Restrizioni  
 Ai backup compressi vengono applicate le restrizioni seguenti:  
  
-   I backup compressi e non compressi non possono coesistere in un set di supporti.  
  
-   Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è possibile leggere i backup compressi.  
  
-   NTbackups non può condividere un nastro con backup compressi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
  
##  <a name="performance-impact-of-compressing-backups"></a><a name="PerfImpact"></a> Impatto sulle prestazioni della compressione di backup  
 Per impostazione predefinita, la compressione aumenta significativamente l'utilizzo della CPU, il che può avere un impatto negativo sulle operazioni simultanee. Quindi potrebbe essere necessario creare backup compressi con priorità bassa in una sessione in cui l'utilizzo della CPU è limitato da[Resource Governor](../../relational-databases/resource-governor/resource-governor.md). Per ulteriori informazioni, vedere [Utilizzo di Resource Governor per limitare l'utilizzo della CPU da parte della compressione dei backup &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md).  
  
 Per conoscere le prestazioni di I/O del backup, è possibile isolare l'I/O del backup in/da dispositivi valutando i seguenti ordinamenti di contatori delle prestazioni:  
  
-   Contatori delle prestazioni di I/O di Windows, ad esempio i contatori del disco fisico  
  
-   Contatore **Byte/sec velocità effettiva dispositivo** dell'oggetto [SQLServer:Backup Device](../../relational-databases/performance-monitor/sql-server-backup-device-object.md)  
  
-   Contatore **Velocità effettiva backup o ripristino/sec** dell'oggetto [SQLServer:Databases](../../relational-databases/performance-monitor/sql-server-databases-object.md)  
  
 Per informazioni sui contatori di Windows, vedere la Guida di Windows. Per informazioni su come usare i contatori di SQL Server, vedere [Utilizzare oggetti di SQL Server](../../relational-databases/performance-monitor/use-sql-server-objects.md).  
  
   
##  <a name="calculate-the-compression-ratio-of-a-compressed-backup"></a><a name="CompressionRatio"></a> Calcolare il rapporto di compressione di un backup compresso  
 Per calcolare il rapporto di compressione di un backup, usare i valori per il backup nelle colonne **backup_size** e **compressed_backup_size** della tabella di cronologia [backupset](../../relational-databases/system-tables/backupset-transact-sql.md) , come segue:  
  
 **backup_size**:**compressed_backup_size**  
  
 Una compressione 3:1, ad esempio, indica un risparmio approssimativo del 66% di spazio su disco. Per eseguire una query su queste colonne, è possibile utilizzare la seguente istruzione Transact-SQL:  
  
```sql  
SELECT backup_size/compressed_backup_size FROM msdb..backupset;  
```  
  
 Il rapporto di compressione di un backup compresso dipende dai dati compressi. Vari fattori possono avere un impatto sul rapporto di compressione ottenuto. Di seguito vengono indicati alcuni dei fattori principali:  
  
-   Tipo di dati.  
  
     I dati di tipo carattere vengono compressi maggiormente rispetto ad altri tipi di dati.  
  
-   Coerenza dei dati tra le righe in una pagina.  
  
     In genere, se una pagina contiene molte righe in cui un campo contiene lo stesso valore, potrebbe verificarsi una significativa compressione per tale valore. Invece, per un database che contiene dati casuali o che contiene solo una grande riga per pagina, un backup compresso avrebbe pressoché le stesse dimensioni di un backup non compresso.  
  
-   Crittografia dei dati.  
  
     I dati crittografati vengono compressi molto meno rispetto ai dati equivalenti non crittografati. Ad esempio, se i dati sono crittografati a livello di colonna con Always Encrypted o con altra crittografia a livello di applicazione, la compressione dei backup potrebbe non ridurre significativamente le dimensioni.

     Per altre informazioni relative alla compressione di database crittografati con Transparent Data Encryption (TDE), vedere [Compressione dei backup con TDE](#backup-compression-with-tde).

-   Compressione del database,  
  
     Se il database è compresso, la compressione potrebbe ridurre di poco o non ridurre le dimensioni dei backup.  

## <a name="backup-compression-with-tde"></a>Compressione dei backup con TDE

A partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], impostando `MAXTRANSFERSIZE` su un valore **maggiore di 65536 (64 KB)** si abilita un algoritmo di compressione ottimizzato per i database con crittografia [Transparent Data Encryption (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md) che esegue innanzitutto la decrittografia di una pagina, la comprime e quindi ne esegue nuovamente la crittografia. Se il parametro `MAXTRANSFERSIZE` non viene specificato o se viene usato il valore `MAXTRANSFERSIZE = 65536` (64 KB), la compressione di backup con i database con crittografia TDE comprime direttamente le pagine crittografate e potrebbe non produrre rapporti di compressione validi. Per altre informazioni, vedere [Backup Compression for TDE-enabled Databases](/archive/blogs/sqlcat/sqlsweet16-episode-1-backup-compression-for-tde-enabled-databases) (Compressione dei backup per i database con TDE).

A partire da [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] CU5, non è più necessario impostare `MAXTRANSFERSIZE` per abilitare questo algoritmo di compressione ottimizzato con TDE. Se viene specificato il comando di backup `WITH COMPRESSION` o la configurazione server *Valore predefinito di compressione backup* è impostata su 1, il valore `MAXTRANSFERSIZE` verrà automaticamente aumentato a 128 KB per abilitare l'algoritmo ottimizzato. Se `MAXTRANSFERSIZE` viene specificato nel comando di backup con un valore > 64K, il valore definito verrà rispettato. In altre parole, SQL Server non ridurrà mai il valore in modo automatico, ma lo aumenterà soltanto. Se è necessario eseguire il backup di un database con crittografia TDE con `MAXTRANSFERSIZE = 65536`, è necessario specificare `WITH NO_COMPRESSION` o assicurarsi che la configurazione server *Valore predefinito di compressione backup* sia impostata su 0.

Per altre informazioni, vedere [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md).

##  <a name="allocation-of-space-for-the-backup-file"></a><a name="Allocation"></a> Allocazione di spazio per il file di backup  
 Per i backup compressi le dimensioni dei file di backup finale dipende dal livello di compressione dei dati e questo valore non è noto finché l'operazione di backup non viene completata.  Per impostazione predefinita, quando si esegue il backup di un database mediante compressione, il motore di database utilizza pertanto un algoritmo di preallocazione per il file di backup. Tramite questo algoritmo viene preallocata una percentuale predefinita delle dimensioni del database per il file di backup. Se durante un'operazione di backup è necessaria una quantità di spazio maggiore, il motore di database aumenta le dimensioni del file. Se le dimensioni finali sono inferiori allo spazio allocato, al termine dell'operazione di backup il motore di database compatta il file in base alle dimensioni finali effettive del backup.  
  
 Per consentire l'aumento del file di backup solo di quanto necessario per raggiungere le relative dimensioni finali, utilizzare il flag di traccia 3042. Il flag di traccia 3042 fa in modo che durante l'operazione di backup venga ignorato l'algoritmo di preallocazione di compressione di backup predefinito. Questo flag di traccia è utile se è necessario risparmiare spazio allocando solo le dimensioni effettive necessarie per il backup compresso. L'utilizzo di questo flag di traccia può tuttavia comportare un effetto leggermente negativo sulle prestazioni, vale a dire un possibile aumento della durata dell'operazione di backup.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Configura compressione backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/configure-backup-compression-sql-server.md)  
  
-   [Visualizzare o configurare l'opzione di configurazione del server backup compression default](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)  
  
-   [Utilizzo di Resource Governor per limitare l'utilizzo della CPU da parte della compressione dei backup &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md)  
  
-   [DBCC TRACEON &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-transact-sql.md)  
  
-   [DBCC TRACEOFF &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceoff-transact-sql.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Panoramica del backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [Flag di traccia &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)  
  
