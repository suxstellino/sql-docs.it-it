---
title: Disinstallare un'istanza esistente
description: Questo articolo descrive come disinstallare un'istanza autonoma di SQL Server, che prepara anche il sistema in modo da poter reinstallare SQL Server.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- removing instances of SQL Server
- uninstalling instances of SQL Server
- removing SQL Server
- instances of SQL Server, uninstalling
- uninstalling SQL Server
ms.assetid: 3c64b29d-61d7-4b86-961c-0de62261c6a1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6a5b77c8e67d6512e691f748265e7901319a5e4f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336083"
---
# <a name="uninstall-an-existing-instance-of-sql-server-setup"></a>Disinstallare un'istanza esistente di SQL Server (Programma di installazione)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In questo articolo viene descritto come disinstallare un'istanza autonoma di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. I passaggi inclusi in questo articolo consentono anche di preparare il sistema per la reinstallazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 > [!NOTE]
 > Per disinstallare un cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , utilizzare la funzionalità per la rimozione del nodo disponibile nel programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per rimuovere ogni nodo singolarmente. Per altre informazioni, vedere [Aggiungere o rimuovere nodi in un cluster di failover di SQL Server &#40;programma di installazione&#41;](../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)  

## <a name="considerations"></a>Considerazioni

- Per disinstallare un'istanza di SQL Server, è necessario essere un amministratore locale con le autorizzazioni per accedere come servizio. 
- Se il computer dispone della quantità di memoria fisica *minima* richiesta, aumentare le dimensioni del file di paging fino al doppio della quantità di memoria fisica. L'insufficienza di memoria virtuale può causare una rimozione incompleta di SQL Server. 
- In un sistema con più istanze di SQL Server, il servizio SQL Server Browser viene disinstallato solo dopo la rimozione dell'ultima istanza di SQL Server. Il servizio SQL Server Browser può essere rimosso manualmente da **Programmi e funzionalità** nel **Pannello di controllo**. 
- La disinstallazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comporta l'eliminazione dei file di dati tempdb aggiunti durante il processo di installazione. I file con modello di nome tempdb_mssql_*.ndf vengono eliminati, se sono presenti nella directory del database di sistema. 
  

  
## <a name="prepare"></a>Preparazione  
  
1.  **Eseguire il backup dei dati.** Creare [backup completi](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md) di tutti i database, inclusi i database di sistema, oppure copiare manualmente i file con estensione mdf e ldf in una posizione separata. Il database **master** contiene tutte le informazioni a livello di sistema per il server, ad esempio account di accesso e schemi. Il database **msdb** contiene informazioni sui processi, ad esempio processi di SQL Server Agent, cronologia dei backup e piani di manutenzione. Per altre informazioni sui database di sistema, vedere [Database di sistema](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md). 
  
    Nei file da salvare sono inclusi i file di database seguenti:  

    * master.mdf
    * msdbdata.mdf
    * Tempdb.mdf
    * mastlog.ldf
    * msdblog.ldf
    * Templog.ldf
    * model.mdf
    * Mssqlsystemresource.mdf
    * ReportServer[$InstanceName]
    * modellog.ldf
    * Mssqlsustemresource.ldf
    * ReportServer[$InstanceName]TempDB

    > [!NOTE]
    > I database ReportServer sono inclusi in SQL Server Reporting Services.   

 
1.  **Arrestare tutti** i **servizi** di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Prima di disinstallare i componenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è consigliabile arrestare tutti i servizi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le connessioni attive possono impedire la corretta esecuzione della disinstallazione.  
  
1.  **Utilizzare un account dotato di autorizzazioni appropriate.** Accedere al server utilizzando l'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o un account che dispone di autorizzazioni equivalenti. È possibile, ad esempio, accedere al server utilizzando un account membro del gruppo di amministratori locali.  
  
## <a name="uninstall"></a>Disinstallare 

# <a name="windows-10--2016-"></a>[Windows 10 / 2016 +](#tab/Windows10)

Per disinstallare SQL Server da Windows 10, Windows Server 2016, Windows Server 2019 e versioni successive, seguire questa procedura: 

1. Per iniziare il processo di rimozione, passare a **Impostazioni** dal menu Start e quindi scegliere **App**. 
1. Cercare `sql` nella casella di ricerca. 
1. Selezionare **Microsoft SQL Server (versione) (bit)** . Ad esempio: `Microsoft SQL Server 2017 (64-bit)`.
1. Selezionare **Disinstalla**.
 
    ![Disinstallare SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/uninstall-sql-server-windows-10.png)

1. Selezionare **Rimuovi** nella finestra di dialogo popup di SQL Server per avviare l'installazione guidata di Microsoft SQL Server. 

    ![Rimuovere SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/remove-sql-2017.png)
  
1.  Nella pagina **Seleziona istanza** usare la casella di riepilogo a discesa per specificare un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da rimuovere o indicare l'opzione per rimuovere solo gli strumenti di gestione e le funzionalità condivise di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per continuare, selezionare **Avanti**.  
  
1.  Nella pagina **Seleziona funzionalità** specificare le funzionalità da rimuovere dall'istanza specificata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
1.  Nella pagina **Inizio rimozione** esaminare l'elenco dei componenti e delle funzionalità che verranno disinstallate. Fare clic su **Rimuovi** per avviare la disinstallazione.  
 
1. Aggiornare la finestra **App e funzionalità** per verificare che l'istanza di SQL Server sia stata rimossa correttamente e determinare gli eventuali componenti di SQL Server ancora presenti. Rimuovere anche questi componenti da questa finestra, se lo si desidera. 

# <a name="windows-2008---2012-r2"></a>[Windows 2008 - 2012 R2](#tab/windows2012)

Per disinstallare SQL Server da Windows Server 2008, Windows Server 2012 e Windows 2012 R2, seguire questa procedura: 

1. Per avviare il processo di rimozione, passare al **Pannello di controllo** e quindi selezionare **Programmi e funzionalità**.
1. Fare clic con il pulsante destro del mouse su **Microsoft SQL Server (versione) (bit)**  e scegliere **Disinstalla**. Ad esempio: `Microsoft SQL Server 2012 (64-bit)`.  
  
    ![Disinstallare SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/uninstall-sql-server-windows-2012.png)

1. Selezionare **Rimuovi** nella finestra di dialogo popup di SQL Server per avviare l'installazione guidata di Microsoft SQL Server. 

    ![Rimuovere SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/remove-sql-2012.png)
  
1.  Nella pagina **Seleziona istanza** usare la casella di riepilogo a discesa per specificare un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da rimuovere o indicare l'opzione per rimuovere solo gli strumenti di gestione e le funzionalità condivise di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per continuare, selezionare **Avanti**.  
  
1.  Nella pagina **Seleziona funzionalità** specificare le funzionalità da rimuovere dall'istanza specificata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
1.  Nella pagina **Inizio rimozione** esaminare l'elenco dei componenti e delle funzionalità che verranno disinstallate. Fare clic su **Rimuovi** per avviare la disinstallazione.  
 
1. Aggiornare la finestra **Programmi e funzionalità** per verificare che l'istanza di SQL Server sia stata rimossa correttamente e determinare gli eventuali componenti di SQL Server ancora presenti. Rimuovere anche questi componenti da questa finestra, se lo si desidera. 

---

  
## <a name="in-the-event-of-failure"></a>In caso di errore  

Se il processo di rimozione non riesce, esaminare i [file di log del programma di installazione di SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md) per determinare la causa principale. 

L'articolo della Knowledge Base [Come identificare i problemi di installazione di SQL Server 2008 nei file di registro di installazione](https://support.microsoft.com/kb/955396/en-us) può risultare utile per le indagini. Sebbene sia scritto per SQL Server 2008, la metodologia descritta è applicabile a tutte le versioni di SQL Server. 

  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare e leggere i file di log del programma di installazione di SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
  
