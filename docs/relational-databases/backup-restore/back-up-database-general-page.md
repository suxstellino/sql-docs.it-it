---
title: Backup database (pagina Generale) | Microsoft Docs
description: In SQL Server usare la pagina Generale della finestra di dialogo Backup database per visualizzare o modificare le impostazioni di origine e destinazione per un'operazione di backup del database.
ms.custom: ''
ms.date: 07/01/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.backupdatabase.general.f1
ms.assetid: 5c344dfd-1ad3-41cc-98cd-732973b4a162
author: cawrites
ms.author: chadam
ms.openlocfilehash: d6303aa2a0ee3165ca3a90f466fbb001f6f7dc83
ms.sourcegitcommit: 0b37eb7aef2f358f80867cd13830dd6683da8d85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2021
ms.locfileid: "105981012"
---
# <a name="back-up-database-general-page"></a>Backup database (pagina Generale)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Utilizzare la pagina **Generale** della finestra di dialogo **Backup database** per visualizzare o modificare le impostazioni per un'operazione di backup del database.  
  
 Per altre informazioni di base sui backup di database, vedere [Panoramica del backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md).  
  
> [!NOTE]  
>  Quando si specifica un'attività di backup usando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], è possibile generare lo script [BACKUP](../../t-sql/statements/backup-transact-sql.md) corrispondente di [!INCLUDE[tsql](../../includes/tsql-md.md)] facendo clic sul pulsante **Script** e selezionando una destinazione per lo script.  
  
 **Per utilizzare SQL Server Management Studio per la creazione di un backup**  
  
-   [Creazione di un backup completo del database &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Creare un backup differenziale del database &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
    > [!IMPORTANT]  
    >  È possibile definire un piano di manutenzione database per creare backup di database. Per ulteriori informazioni, vedere [Piani di manutenzione dei database](../maintenance-plans/maintenance-plans.md) nella documentazione online di [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .  
  
 **Per creare un backup parziale**  
  
-   Per un backup parziale, è necessario usare l'istruzione [BACKUP](../../t-sql/statements/backup-transact-sql.md) di [!INCLUDE[tsql](../../includes/tsql-md.md)] con l'opzione PARTIAL.  
  
## <a name="options"></a>Opzioni  
  
### <a name="source"></a>Source (Sorgente)  
 Le opzioni del pannello **Origine** identificano il database e specificano il tipo di backup e il componente per l'operazione di backup.  
  
 **Database**  
 È possibile selezionare il database di cui eseguire il backup.  
  
 **Modello di recupero**  
 È possibile visualizzare il modello di recupero, ovvero SIMPLE, FULL o BULK_LOGGED, per il database selezionato.  
  
 **Tipo di backup**  
 È possibile selezionare il tipo di backup che si desidera eseguire per il database specificato.  
  
|Tipo di backup|Disponibile per|Restrizioni|  
|-----------------|-------------------|------------------|  
|Full|Database, file e filegroup|Per il database **master** è possibile eseguire solo backup completi.<br /><br /> Quando si utilizza il modello di recupero con registrazione minima, i backup di file e filegroup sono disponibili solo per filegroup di sola lettura.|  
|Differenziale|Database, file e filegroup|Quando si utilizza il modello di recupero con registrazione minima, i backup di file e filegroup sono disponibili solo per filegroup di sola lettura.|  
|Log delle transazioni|Log delle transazioni|I backup dei log delle transazioni non sono disponibili per il modello di recupero con registrazione minima.|  
  
 **Backup di sola copia**  
 Selezionare questa opzione per creare un backup di sola copia. Un *backup di sola copia* è un backup di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] indipendente dalla sequenza di backup convenzionali di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Backup di sola copia &#40;SQL Server&#41;](../../relational-databases/backup-restore/copy-only-backups-sql-server.md).  
  
> [!NOTE]  
>  Quando si seleziona l'opzione **Differenziale** , non è possibile creare un backup di sola copia.  
  
 **Componente di cui eseguire il backup**  
 Con questa opzione è possibile selezionare il componente del database di cui eseguire il backup. Se nell'elenco **Tipo backup** è selezionato **Log delle transazioni** , questa opzione non è attivata.  
  
 Selezionare uno dei pulsanti di opzione seguenti:  
  
|Opzione|Descrizione|  
|-|-|  
|**Database**|È possibile specificare di eseguire il backup dell'intero database.|  
|**File e filegroup**|È possibile specificare di eseguire il backup dei file e/o dei filegroup specificati.<br /><br /> Selezionando questa opzione, viene visualizzata la finestra di dialogo **Seleziona file e filegroup** . Dopo avere selezionato i filegroup o i file di cui eseguire il backup e avere scelto **OK**, le opzioni selezionate verranno visualizzate nella casella **File e filegroup** .|  
  
### <a name="destination"></a>Destination  
 Con le opzioni del pannello **Destinazione** è possibile specificare il tipo di dispositivo di backup per l'operazione di backup e di trovare un dispositivo di backup logico o fisico esistente.  
  
> [!NOTE]  
>  Per informazioni sui dispositivi di backup di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Dispositivi di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).  
  
 **Backup su**  
 Selezionare uno dei tipi di supporti seguenti su cui eseguire il backup. Le destinazioni selezionate vengono visualizzate nell'elenco **Backup su** .  
  
|Tipo di supporto|Descrizione|  
|-|-|  
|**Disco**|Indica di eseguire il backup su disco. Può trattarsi di un file di sistema o di un dispositivo di backup logico su disco creato per il database. I dischi attualmente selezionati vengono visualizzati nell'elenco **Backup su** . È possibile selezionare fino a 64 dispositivi disco per l'operazione di backup.|  
|**Nastro**|Indica di eseguire il backup su nastro. Può trattarsi di un'unità nastro locale o di un dispositivo di backup logico su nastro creato per il database. I nastri attualmente selezionati vengono visualizzati nell'elenco **Backup su** . Il numero massimo è 64. Se al server non è collegato alcun dispositivo nastro, questa opzione è disattivata. I nastri selezionati vengono visualizzati nell'elenco **Backup su** .<br /><br /> Nota: Il supporto per i dispositivi di backup su nastro verrà rimosso in una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata.|  
|**URL**|Indica di eseguire il backup in Archiviazione BLOB di Microsoft Azure.|  
  
 Il set successivo di opzioni visualizzate dipende dal tipo di destinazione selezionata. Se si seleziona Disco o Nastro, vengono visualizzate le opzioni riportate di seguito.  
  
 **Aggiungere**  
 È possibile aggiungere un file o un dispositivo all'elenco **Backup su** . È possibile eseguire il backup su 64 dispositivi contemporaneamente su un disco locale o remoto. Per specificare un file su un disco remoto, utilizzare il nome completo in formato UNC (Universal Naming Convention). Per altre informazioni, vedere [Dispositivi di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).  
 
 
  
 **Rimuovi**  
 È possibile rimuovere uno o più dispositivi attualmente selezionati dall'elenco **Backup su** .  
  
 **Contents**  
È possibile visualizzare il contenuto dei supporti per il dispositivo selezionato, se esistente.  Il pulsante non esegue una funzione se viene specificato un **URL** . 
   
Finestra di dialogo **Seleziona destinazione di backup** La finestra di dialogo **Seleziona destinazione di backup** viene visualizzata dopo aver selezionato **Aggiungi**.   Il set di opzioni visualizzate dipende dal tipo di destinazione selezionata. 

Se si seleziona **Disco** o **Nastro** come destinazione di backup, verranno visualizzate le opzioni seguenti.  

*
  **Nome file**  
    Specificare il nome del file di backup.

Se si seleziona **URL** come destinazione di backup, verranno visualizzate le opzioni seguenti:
*
  **Contenitore di archiviazione di Azure**  
  Nome del contenitore di archiviazione di Microsoft Azure in cui archiviare i file di backup. 
   
*
  **Criteri di accesso condiviso:**  
  Immettere la firma di accesso condiviso per un contenitore immesso manualmente.  Questo campo non è disponibile se è stato scelto un contenitore esistente.
  
*
  **File di backup:**  
  nome del file di backup.

*
  **Nuovo contenitore:**  
Usato per registrare un contenitore esistente per il quale non si ha una firma di accesso condiviso.  Vedere [Connettersi a una sottoscrizione di Microsoft Azure](../../relational-databases/backup-restore/connect-to-a-microsoft-azure-subscription.md).
  
## <a name="see-also"></a>Vedere anche  
 [Backup di un log delle transazioni &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)   
 [Backup di file e filegroup &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)   
 [Definire un dispositivo di backup logico per un file su disco &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-disk-file-sql-server.md)   
 [Definizione di un dispositivo di backup logico per un'unità nastro &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)   
 [Modelli di recupero &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md)  
  
  
