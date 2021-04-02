---
title: Attività Backup database (piano di manutenzione) | Microsoft Docs
description: Informazioni su come aggiungere un'attività di backup a un piano di manutenzione in SQL Server usando l'attività Backup database.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- sql13.swb.maint.maintplanproperties.logbackup.f1
- sql13.swb.maint.backup.f1
helpviewer_keywords:
- Back Up Database Task dialog box
ms.assetid: ed1ef012-fa14-4ba5-bafe-d1527ba065b3
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 1b68c2b2f86633db9cbc5c35746c8113530491f3
ms.sourcegitcommit: 295b9dfc758471ef7d238a2b0f92f93e34acbb1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106054646"
---
# <a name="options-in-the-back-up-database-task-for-maintenance-plan"></a>Opzioni dell'attività Backup database per il piano di manutenzione
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Usare la finestra di dialogo **Attività Backup database** per aggiungere un'attività di backup al piano di manutenzione. L'esecuzione di backup del database è importante in caso di guasti al sistema o all'hardware, nonché di errori degli utenti, che possono danneggiare il database e rendere pertanto necessaria la disponibilità di una copia di backup per il ripristino. Questa attività consente di eseguire backup completi e differenziali, di file e filegroup, nonché del log delle transazioni.  
  
 **Per creare un'attività Backup database**  
  
-   [Creare un piano di manutenzione](../../relational-databases/maintenance-plans/create-a-maintenance-plan.md)  
  
## <a name="options"></a>Opzioni  
 **Connection**  
 Consente di selezionare la connessione server da utilizzare per l'esecuzione dell'attività.  
  
 **Nuovo**  
 Consente di creare una nuova connessione server da utilizzare per l'esecuzione dell'attività. La finestra di dialogo **Nuova connessione** è descritta di seguito.  
  
 **Database**  
 Consente di specificare i database su cui verrà eseguita l'attività. Quando questa opzione è selezionata, nell'elenco a discesa sono disponibili le opzioni seguenti: **Tutti i database**, **Tutti i database di sistema**, **Tutti i database utente**, **Database specifici**.  
  
 **Tutti i database**  
 Consente di generare un piano per l'esecuzione di attività di manutenzione su tutti i database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **Tutti i database di sistema (master, model e msdb)**  
 Consente di generare un piano per l'esecuzione di attività di manutenzione su tutti i database di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Non vengono eseguite attività di manutenzione sui database creati dall'utente.  
  
 **Tutti i database utente (diversi da master, model, msdb e tempdb)**  
 Consente di generare un piano per l'esecuzione di attività di manutenzione su tutti i database creati dall'utente. Nessuna attività di manutenzione viene eseguita sui database di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **I database seguenti**  
 Consente di generare un piano per l'esecuzione di attività di manutenzione solo sui database selezionati. Se si sceglie questa opzione, è necessario selezionare almeno un database nell'elenco.  
  
 **Tipo di backup**  
 Consente di visualizzare il tipo di backup da eseguire.  
  
 **Componente di cui eseguire il backup**  
 Selezionare **Database** per eseguire il backup dell'intero database. Selezionare **File e filegroup** per eseguire il backup solo di una parte del database. Quando si seleziona questa opzione, è necessario specificare il nome del file o del filegroup. Se nella casella **Database** sono selezionati più database, è necessario specificare **Database** solo per **Componente di cui eseguire il backup**. Per eseguire i backup di file o filegroup, creare un'attività per ogni database.  
  
 **Scadenza set di backup**  
 Consente di specificare la data a partire dalla quale il set di backup può essere sovrascritto da un altro set di backup.  
  
 **Backup su**  
 Consente di scegliere se eseguire il backup del database su file o su nastro. Sono disponibili solo i dispositivi nastro collegati al computer in cui è archiviato il database.  
  
 **Backup database in uno o più file**  
 Fare clic su **Aggiungi** per aprire la finestra di dialogo **Selezione destinazione di backup** e specificare una o più posizioni su disco o dispositivi nastro.  
  
 **Azione per file di backup esistenti**  
 Selezionare **Accoda** per aggiungere questo backup alla fine del file. Selezionare **Sovrascrivi** per rimuovere i backup esistenti dal file e sostituirli con il nuovo.  
  
 **Crea un file di backup per ogni database**  
 Creare un file di backup nel percorso specificato nella casella della cartella. Viene creato un file per ogni database selezionato.  
  
 **Crea una sottodirectory per ogni database**  
 Selezionare questa opzione per inserire ogni file di backup di database in una sottocartella.  
  
> [!IMPORTANT]  
>  Sebbene i piani di manutenzione possano creare sottodirectory, le attività di manutenzione non possono eliminare le sottodirectory. Questa caratteristica riduce la possibilità di un attacco dannoso che utilizzi l'attività Pulizia file manutenzione per eliminare i file.  
  
> [!IMPORTANT]  
>  La sottodirectory eredita le autorizzazioni dalla directory padre. Limitare le autorizzazioni per impedire l'accesso non autorizzato.  
  
 **Cartella**  
 Specificare la cartella in cui inserire i file di database creati automaticamente.  
  
 **Estensione file di backup**  
 Specificare l'estensione da utilizzare per i file di backup. L'estensione predefinita è **bak**.  
  
 **Verifica integrità backup**  
 Consente di verificare che il set di backup sia completo e che tutti i volumi siano leggibili.  
  
 **Esegui backup della parte finale del log e lascia il database in stato di ripristino**  
 Consente di eseguire un backup del log come ultimo passaggio prima di ripristinare un database. Per altre informazioni, vedere [Backup della parte finale del log &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).  
  
 **Imposta compressione backup**  
 In [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] o versioni successive, selezionare uno dei seguenti valori della [compressione dei backup](../../relational-databases/backup-restore/backup-compression-sql-server.md) :  
  
|Valore|Descrizione|  
|-|-|  
|**Utilizza l'impostazione predefinita del server**|Fare clic su questa opzione per utilizzare l'impostazione predefinita a livello di server.<br /><br /> Questa impostazione predefinita è specificata dall'opzione di configurazione del server **Valore predefinito di compressione backup** . Per informazioni su come visualizzare l'impostazione corrente di questa opzione, vedere [Visualizzare o configurare l'opzione di configurazione del server backup compression default](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md).|  
|**Comprimi backup**|Fare clic su questa opzione per comprimere il backup, indipendentemente dall'impostazione predefinita a livello di server.<br /><br /> **\*\* Importante \*\*** Per impostazione predefinita, la compressione aumenta significativamente l'uso della CPU e la CPU aggiuntiva usata dal processo di compressione può avere un impatto negativo sulle operazioni simultanee. Potrebbe pertanto essere necessario creare backup compressi con priorità bassa in una sessione in cui l'utilizzo della CPU è limitato da [Resource Governor](../../relational-databases/resource-governor/resource-governor.md). Per ulteriori informazioni, vedere [Utilizzo di Resource Governor per limitare l'utilizzo della CPU da parte della compressione dei backup &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md).|  
|**Non comprimere il backup**|Fare clic su questa opzione per creare un backup non compresso, indipendentemente dall'impostazione predefinita a livello di server.|  
  
 **Visualizza codice T-SQL**  
 Consente di visualizzare le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] eseguite sul server per questa attività, in base alle opzioni selezionate.  
  
> [!NOTE]  
>  Se il numero di oggetti interessato dall'attività è elevato, la visualizzazione del codice potrebbe richiedere una considerevole quantità di tempo.  
  
## <a name="new-connection-dialog-box"></a>Finestra di dialogo Nuova connessione  
 **Nome connessione**  
 Consente di immettere un nome per la nuova connessione.  
  
 **Selezionare o immettere il nome di un server**  
 Consente di selezionare il server a cui connettersi per l'esecuzione dell'attività.  
  
 **Aggiorna**  
 Consente di aggiornare l'elenco dei server disponibili.  
  
 **Immettere le informazioni per l'accesso al server**  
 Consente di specificare le opzioni di autenticazione per l'accesso al server.  
  
 **Usa la sicurezza integrata di Windows NT**  
 Consente di connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando l'autenticazione di Windows.  
  
 **Usa nome utente e password specifici**  
 Consente di connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando l'autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa opzione non è disponibile.  
  
 **Nome utente**  
 Consente di specificare un account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da utilizzare per l'autenticazione. Questa opzione non è disponibile.  
  
 **Password**  
 Consente di specificare una password da utilizzare per l'autenticazione. Questa opzione non è disponibile.  
  
## <a name="see-also"></a>Vedere anche  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)  
  
  
