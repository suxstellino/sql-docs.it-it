---
title: Seleziona destinazione di backup | Microsoft Docs
description: Per il ripristino di SQL Server, usare la finestra di dialogo Seleziona destinazione di backup per selezionare un disco o un dispositivo di backup logico come destinazione di backup.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.selectbackupdest.f1
ms.assetid: f79e824b-1525-45de-8ede-513563af41b6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4caae987574c6833530aef858ce57d55ccd7b3fc
ms.sourcegitcommit: 0b37eb7aef2f358f80867cd13830dd6683da8d85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105981107"
---
# <a name="select-backup-destination"></a>Seleziona destinazione di backup
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Usare la finestra di dialogo **Seleziona destinazione di backup** per selezionare un dispositivo come destinazione di backup. La destinazione di backup può essere un disco o un dispositivo di backup logico.  
  
 **Per utilizzare SQL Server Management Studio per l'esecuzione del backup di un database**  
  
-   [Creazione di un backup completo del database &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Creare un backup differenziale del database &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
-   [Backup di file e filegroup &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)  
  
-   [Eseguire il backup di un log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
## <a name="options"></a>Opzioni  
 Le opzioni disponibili in questa finestra di dialogo variano a seconda che si selezioni una destinazione su disco o su nastro.  
  
 **Destinazioni su disco**  
 Per specificare una destinazione di backup, scegliere una delle opzioni seguenti.  
  
|Opzione|Descrizione|  
|-|-|  
|**Nome file**|Scegliere questa opzione per immettere un file locale o remoto come destinazione di backup nella casella di testo per:<br /><br /> Specificare un file locale, facendo clic sul pulsante Sfoglia a destra della casella di testo e individuando il file nei dischi rigidi del computer che esegue il server oppure digitando direttamente il nome e il percorso completo del file, ad esempio `C:\Program Files\Microsoft SQL Server\MSSQL\Backup\AdventureWorksBackup.bak`.<br /><br /> Specificare un file remoto come destinazione di backup usando il nome completo in formato UNC (Universal Naming Convention). Per altre informazioni, vedere [Dispositivi di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).<br /><br /> <br /><br /> **\*\* Importante \*\*** L'esecuzione del backup dei dati in rete può essere soggetta a errori di rete, quindi è consigliabile verificare l'operazione di backup dopo il suo completamento. Per altre informazioni, vedere [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md).|  
|**Dispositivo di backup**|Scegliere questa opzione per selezionare un dispositivo di backup logico.<br /><br /> Nota: per informazioni su come creare un dispositivo di backup su disco, vedere [Definizione di un dispositivo di backup logico per un file su disco &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-disk-file-sql-server.md).|  
  
 **Destinazioni su nastro**  
 Consente di specificare una destinazione di backup su un'unità nastro fisicamente collegata al computer che esegue il server, ovvero l'istanza di [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Scegliere una delle opzioni seguenti:  
  
|Opzione|Descrizione|  
|-|-|  
|**Unità nastro**|Scegliere questa opzione per selezionare un'unità nastro come destinazione del backup dall'elenco delle unità nastro fisicamente collegate al computer che esegue l'istanza del server.<br /><br /> Nota: I dispositivi di backup su nastro collegati a computer remoti non rappresentano destinazioni di backup valide.|  
|**Dispositivo di backup**|Scegliere questa opzione per selezionare un dispositivo di backup logico esistente. Questi dispositivi di backup logici corrispondono a unità nastro fisicamente collegate al computer che esegue l'istanza del server.<br /><br /> Nota: per informazioni su come creare un dispositivo di backup su nastro, vedere [Definizione di un dispositivo di backup logico per un'unità nastro &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md).|  
  
## <a name="see-also"></a>Vedere anche  
 [Dispositivi di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [Set di supporti, gruppi di supporti e set di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)  
  
  
