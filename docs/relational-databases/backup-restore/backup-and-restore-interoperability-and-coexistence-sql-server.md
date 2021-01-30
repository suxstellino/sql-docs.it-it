---
title: 'Backup e ripristino: interoperabilità delle funzionalità'
description: Questo articolo descrive le funzionalità di backup e ripristino in SQL Server, tra cui l'avvio del database, il ripristino in linea, gli indici disabilitati e il mirroring del database.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- file restores [SQL Server], related features
- restoring [SQL Server], files
- restoring files [SQL Server], related features
- backups [SQL Server], files or filegroups
- file backups [SQL Server], related features
ms.assetid: 69f212b8-edcd-4c5d-8a8a-679ced33c128
author: cawrites
ms.author: chadam
ms.openlocfilehash: b8de8452d7f39b1450df8c280ed27a9ac12603db
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077069"
---
# <a name="backup-and-restore-interoperability-and-coexistence-sql-server"></a>Backup e ripristino: interoperabilità e coesistenza (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In questo argomento vengono fornite alcune considerazioni sul backup e il ripristino di alcune funzionalità di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], tra cui ripristino dei file e avvio del database, ripristino online e indici disabilitati, mirroring del database, ripristino a fasi e indici full-text.  
  
 **Contenuto dell'argomento**  
  
-   [Ripristino dei file e avvio del database](#FileRestoreAndDbStartup)  
  
-   [Ripristino online e indici disabilitati](#OnlineRestoreAndDisabledIndexes)  
  
-   [Mirroring del database e procedure di backup e ripristino](#DbMandBnR)  
  
-   [Ripristino a fasi e indici full-text](#PiecemealAndFTIndexes)  
  
-   [Backup e ripristino di file e compressione](#FileBnRandCompression)  
  
-   [Attività correlate](#RelatedTasks)  
  
##  <a name="file-restore-and-database-startup"></a><a name="FileRestoreAndDbStartup"></a> Ripristino dei file e avvio del database  
 Le informazioni contenute in questa sezione sono rilevanti solo per i database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che includono più filegroup.  
  
> [!NOTE]  
>  Quando un database viene avviato, viene eseguito un recupero e attivata la modalità online solo per i filegroup i cui file erano online al momento della chiusura del database.  
  
 Se si verifica un problema durante l'avvio del database, il recupero non riesce e il database viene contrassegnato come sospetto. Se è possibile isolare uno o più file in cui è presente il problema, l'amministratore del database può attivare la modalità offline per i file e tentare di riavviare il database. Per attivare la modalità offline per un file, è possibile utilizzare l'istruzione [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) seguente:  
  
 ALTER DATABASE *database_name* MODIFY FILE (NAME **='** _filename_*_'_*, OFFLINE)  
  
 Se l'avvio ha esito positivo, tutti i filegroup che includono un file offline rimangono in modalità offline.  
  
##  <a name="online-restore-and-disabled-indexes"></a><a name="OnlineRestoreAndDisabledIndexes"></a> Ripristino online e indici disabilitati  
 Le informazioni contenute in questa sezione sono rilevanti solo per i database che includono più filegroup e, in base al modello di recupero con registrazione minima, almeno un filegroup di sola lettura.  
  
 In questi casi, se un database è online, è possibile creare, eliminare, abilitare o disabilitare l'indice solo se tutti i filegroup che contengono una parte qualsiasi dell'indice sono online.  
  
 Per informazioni sul ripristino di filegroup offline, vedere [Ripristino in linea&#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md).  
  
##  <a name="database-mirroring-and-backup-and-restore"></a><a name="DbMandBnR"></a> Mirroring del database e procedure di backup e ripristino  
 Le informazioni contenute in questa sezione sono rilevanti solo per i database basati sul modello di recupero con registrazione completa che includono più filegroup.  
  
> [!NOTE]  
>  La funzionalità del mirroring di database verrà rimossa in una delle prossime versioni di Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata. Usare invece [!INCLUDE[ssHADR](../../includes/sshadr-md.md)].  
  
 Il mirroring del database è una soluzione per aumentare la disponibilità del database. Il mirroring viene implementato a livello di singolo database e funziona solo con database che utilizzano il modello di recupero con registrazione completa. Per altre informazioni, vedere [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md).  
  
> [!NOTE]  
>  Per distribuire le copie di un subset dei filegroup in un database, è necessario replicare solo gli oggetti dei filegroup che si desidera copiare in altri server. Per altre informazioni sulla replica, vedere [Replica di SQL Server](../../relational-databases/replication/sql-server-replication.md).  
  
### <a name="creating-the-mirror-database"></a>Creazione del database mirror  
 Il database mirror viene creato ripristinando i backup del database principale nel server mirror senza eseguirne il recupero (WITH NORECOVERY). Il ripristino deve mantenere lo stesso nome del database. Per altre informazioni, vedere [Preparazione di un database mirror per il mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
 È possibile creare il database mirror utilizzando una sequenza di ripristino a fasi, se supportata. Non è tuttavia possibile avviare l'esecuzione del mirroring fino a quando non sono stati ripristinati tutti i filegroup e, in genere, fino a quando non sono stati ripristinati i backup del log necessari per portare il database mirror a un punto nel tempo sufficientemente vicino al database principale. Per altre informazioni, vedere [Ripristini a fasi &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md).  
  
### <a name="restrictions-on-backup-and-restore-during-mirroring"></a>Restrizioni relative a operazioni di backup e ripristino durante il mirroring  
 Mentre è attiva una sessione di mirroring del database, vengono applicate le restrizioni seguenti:  
  
-   Non sono consentite operazioni di backup e ripristino del database mirror.  
  
-   È consentito il backup del database principale, ma non è consentito utilizzare BACKUP LOG WITH NORECOVERY.  
  
-   Il ripristino del database principale non è consentito.  
  
##  <a name="piecemeal-restore-and-full-text-indexes"></a><a name="PiecemealAndFTIndexes"></a> Ripristino a fasi e indici full-text  
 Le informazioni contenute in questa sezione sono rilevanti solo per i database che includono più filegroup e, in base al modello di recupero con registrazione minima, solo per i filegroup in sola lettura.  
  
 Gli indici full-text vengono archiviati nei filegroup del database e possono essere influenzati dall'esecuzione di un ripristino a fasi. Se l'indice full-text si trova nello stesso filegroup di uno o più dati della tabella associati, il ripristino a fasi verrà eseguito nel modo previsto.  
  
> [!NOTE]  
>  Per visualizzare l'ID del filegroup che contiene un indice full-text, selezionare la colonna data_space_id di [sys.fulltext_indexes](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md).  
  
### <a name="full-text-indexes-and-tables-in-separate-filegroups"></a>Indici full-text e tabelle in filegroup distinti  
 Se un indice full-text si trova in un filegroup distinto rispetto ai dati delle tabelle associati, il funzionamento del ripristino a fasi dipenderà dal primo filegroup di cui viene eseguito il ripristino e per cui viene attivata la modalità online.  
  
-   Se il ripristino e l'attivazione della modalità online vengono eseguiti prima per il filegroup contenente l'indice full-text e quindi per il filegroup contenente i dati delle tabelle associati, il funzionamento della ricerca full-text è quello previsto non appena l'indice full-text è online.  
  
-   Se il ripristino e l'attivazione della modalità online vengono eseguiti prima per il filegroup contenente i dati delle tabelle e poi per il filegroup contenente l'indice full-text, è possibile che il comportamento della funzionalità full-text risulti diverso. Questa situazione si verifica poiché le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] che attivano un popolamento, ricompilano il catalogo o riorganizzano il catalogo hanno esito negativo fino a quando non viene riattivata la modalità online per l'indice. Tali istruzioni includono CREATE FULLTEXT INDEX, ALTER FULLTEXT INDEX, DROP FULLTEXT INDEX e ALTER FULLTEXT CATALOG.  
  
     In questo caso, considerare in particolare i fattori seguenti:  
  
    -   Se all'indice full-text è associato il rilevamento delle modifiche, le istruzioni DML eseguite dall'utente avranno esito negativo fino a quando non viene attivata la modalità online per il filegroup. Anche l'operazione di eliminazione avrà esito negativo se il filegroup dell'indice non è online.  
  
    -   Indipendentemente dal rilevamento delle modifiche, le query full-text hanno esito negativo perché l'indice non è disponibile. Se si tenta di eseguire una query full-text quando il filegroup contenente l'indice full-text è offline, viene restituito un errore.  
  
    -   Le funzioni di stato, ad esempio FULLTEXTCATALOGPROPERTY, hanno esito positivo solo quando non devono accedere all'indice full-text. L'accesso ai metadati full-text online, ad esempio, ha esito positivo, mentre **uniquekeycount, itemcount** non riesce.  
  
     Dopo il ripristino e l'attivazione della modalità online per il filegroup dell'indice full-text, i dati dell'indice e delle tabelle risultano coerenti.  
  
 Non appena il filegroup della tabella di base e il filegroup dell'indice full-text sono online, qualsiasi popolamento full-text sospeso viene ripreso.  
  
##  <a name="file-backup-and-restore-and-compression"></a><a name="FileBnRandCompression"></a> Backup e ripristino di file e compressione  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta la compressione dei dati con il file system NTFS per i filegroup e i database in sola lettura.  
  
 Il ripristino dei file in un filegroup di sola lettura è supportato per i file NTFS compressi. Il backup e il ripristino di questi filegroup vengono eseguiti sostanzialmente come per qualsiasi filegroup di sola lettura, con le eccezioni seguenti:  
  
-   Il ripristino di un file di lettura/scrittura, inclusi i file primari o di log di un database di lettura/scrittura, su un volume compresso ha esito negativo e viene visualizzato un messaggio di errore.  
  
-   Il ripristino di un database di sola lettura su un volume compresso è consentito.  
  
> [!NOTE]  
>  È consigliabile non memorizzare i file di log di database di lettura/scrittura in file system compressi.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
  
-   [Preparazione di un database mirror per il mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
-   [Backup e ripristino di indici e cataloghi full-text](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Backup e ripristino di database SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [Backup e ripristino di database replicati](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)   
[Repliche secondarie attive: Backup in repliche secondarie \(Gruppi di disponibilità AlwaysOn\)](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)  
  
  
