---
description: Backup e ripristino di SQL Server con il servizio di archiviazione BLOB di Microsoft Azure
title: Backup e ripristino con Archiviazione BLOB di Azure
storage: Learn about SQL Server backup to and restore from Azure Blob storage, including the benefits of using Azure Blob storage to store SQL Server backups.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 6a0c9b6a-cf71-4311-82f2-12c445f63935
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2fb47efd89dfee8777d54b4cf2d1cf0ad2accec9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343651"
---
# <a name="sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service"></a>Backup e ripristino di SQL Server con il servizio di archiviazione BLOB di Microsoft Azure
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  ![Immagine del backup nel servizio BLOB di Azure](../../relational-databases/backup-restore/media/backup-to-azure-blob-graphic.png "Immagine del backup nel servizio BLOB di Azure")  
  
 Questo argomento illustra l'esecuzione di backup di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel [servizio di archiviazione BLOB di Microsoft Azure](https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/)e il ripristino dallo stesso servizio. È anche presente un riepilogo dei vantaggi dell'uso del servizio BLOB di Microsoft Azure per archiviare i backup di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 SQL Server supporta l'archiviazione di backup nel servizio di archiviazione BLOB di Microsoft Azure nei modi seguenti:  
  
-   **Gestione dei backup in Microsoft Azure:** usando gli stessi metodi usati per eseguire il backup su DISK e TAPE, è ora possibile eseguire il backup nel servizio di archiviazione Microsoft Azure specificando l'URL come destinazione di backup. È possibile utilizzare questa funzionalità per eseguire il backup manualmente o per configurare la propria strategia di backup allo stesso modo di una risorsa di archiviazione locale o di altre soluzioni esterne. Questa funzionalità viene definita anche **Backup di SQL Server nell'URL**. Per altre informazioni, vedere [SQL Server Backup to URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md). Questa funzionalità è disponibile in SQL Server 2012 SP1 CU2 o versione successiva. Questa funzionalità è stata migliorata in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] per garantire prestazioni e funzionalità superiori tramite l'uso di BLOB in blocchi, firme di accesso condiviso e striping.  
  
    > [!NOTE]  
    >  Per le versioni di SQL Server precedenti a SQL Server 2012 SP1 CU2, è possibile usare il componente aggiuntivo dello strumento per il backup di SQL Server in Microsoft Azure per creare i backup nel servizio di archiviazione Microsoft Azure in modo rapido e semplice. Per ulteriori informazioni, vedere l' [Area download](https://go.microsoft.com/fwlink/?LinkID=324399).  
  
-   **Backup di snapshot di file per i file di database in Archiviazione BLOB di Azure** : con l'uso degli snapshot di Azure, i backup degli snapshot di file di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] garantiscono backup e ripristini quasi istantanei per i file di database archiviati usando il servizio di archiviazione BLOB di Azure. Questa funzionalità consente di semplificare i criteri di backup e ripristino e supporta il ripristino temporizzato. Per altre informazioni, vedere [Backup di snapshot di file per i file di database in Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md). Questa funzionalità è disponibile in SQL Server 2016 o versione successiva.  
  
-   **Configurazione di SQL Server per la gestione dei backup in Microsoft Azure** : configurare SQL Server in modo da gestire la strategia di backup e pianificare i backup per un singolo database o più database oppure per impostare i valori predefiniti a livello di istanza. Questa funzionalità viene definita **[!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]** . Per altre informazioni, vedere [Backup gestito di SQL Server in Microsoft Azure](../../relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure.md). Questa funzionalità è disponibile in SQL Server 2014 o versione successiva.  
  
## <a name="benefits-of-using-the-microsoft-azure-blob-service-for-ssnoversion-backups"></a>Vantaggi dell'uso del servizio BLOB di Microsoft Azure per i backup di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   Archiviazione flessibile, affidabile e illimitata in una posizione esterna: l'archiviazione dei backup nel servizio BLOB di Microsoft Azure può essere una soluzione esterna utile, flessibile e di facile accesso. La creazione dell'archiviazione in una posizione esterna per i backup di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può essere semplice quanto la modifica degli script o dei processi esistenti. Generalmente, l'archiviazione in una posizione esterna deve essere sufficientemente lontana dalla posizione del database di produzione per impedire che una singola situazione di emergenza possa influire sia sulla posizione esterna sia su quella del database di produzione. Scegliendo la replica a livello geografico dell'archiviazione BLOB si dispone di un ulteriore livello di protezione in caso di evento di emergenza che potrebbe influire sull'intera area. Inoltre, i backup sono disponibili sempre e in qualsiasi punto e possono essere facilmente utilizzati per i ripristini.  
  
    > [!IMPORTANT]  
    >  Con l'uso di BLOB in blocchi in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], è possibile eseguire lo striping del set di backup per supportare file di backup con dimensioni fino a 12,8 TB.  
  
-   Archivio di backup: il servizio di archiviazione BLOB di Microsoft Azure offre un'alternativa migliore rispetto al più comune uso della soluzione su nastro adottata per archiviare i backup. L'archiviazione su nastro può richiedere il trasporto fisico alla struttura fuori sede e l'adozione di determinate misure per la protezione dei supporti. L'archiviazione dei backup nell'apposito servizio BLOB di Microsoft Azure garantisce una soluzione di archiviazione immediata, a elevata disponibilità e durevole.  
  
-   Nessun overhead di gestione dell'hardware: non è previsto alcun problema di questo tipo con i servizi Microsoft Azure. Tramite i servizi Microsoft Azure viene gestito l'hardware e viene fornita una replica a livello geografico per garantire ridondanza e protezione da eventuali errori dell'hardware.  
  
-   Attualmente, per le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in esecuzione in una macchina virtuale di Microsoft Azure, il backup nei servizi di archiviazione BLOB di Microsoft Azure può essere effettuato creando dischi collegati. Esiste tuttavia un limite al numero di dischi che è possibile collegare a una macchina virtuale di Microsoft Azure, vale a dire 16 per un'istanza di dimensioni elevate e un po' di meno per istanze di dimensioni inferiori. Abilitando un backup diretto nell'archiviazione BLOB di Microsoft Azure è possibile evitare il limite di 16 dischi.  
  
     Inoltre, il file di backup attualmente archiviato nel servizio di archiviazione BLOB di Microsoft Azure è direttamente disponibile in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locale o in un'altra di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in esecuzione in una macchina virtuale di Microsoft Azure, senza dover collegare/scollegare il database o scaricare e collegare il disco rigido virtuale.  
  
-   Vantaggi economici: si paga solo il servizio utilizzato. Può rivelarsi una soluzione economica per il backup e l'archiviazione fuori sede. Per altre informazioni e per i collegamenti, vedere la sezione [Considerazioni sui costi di Microsoft Azure](#Billing) .  
  
##  <a name="microsoft-azure-billing-considerations"></a><a name="Billing"></a> Considerazioni sui costi di Microsoft Azure:  
 Conoscendo i costi correlati all'archiviazione di Microsoft Azure è possibile prevedere il costo della creazione e dell'archiviazione dei backup in Microsoft Azure.  
  
 Per stimare i costi, è possibile usare il [calcolatore dei costi di Microsoft Azure](https://go.microsoft.com/fwlink/?LinkId=277060) .  
  
 **Archiviazione:** i costi dipendono dallo spazio utilizzato e vengono calcolati in base a una scala graduata e al livello di ridondanza. Per altri dettagli e informazioni aggiornate, vedere la sezione sulla **gestione dei dati** nell'articolo [Dettagli prezzi](https://go.microsoft.com/fwlink/?LinkId=277059) .  
  
 **Trasferimenti di dati:** i trasferimenti in entrata dei dati in Microsoft Azure sono gratuiti. Per i trasferimenti in uscita viene addebitato il costo relativo all'utilizzo della larghezza di banda e il calcolo viene effettuato in base a una scala graduata specifica per l'area. Per ulteriori informazioni, vedere la sezione [Trasferimenti di dati](https://go.microsoft.com/fwlink/?LinkId=277061) nell'articolo Dettagli prezzi.  
  
## <a name="see-also"></a>Vedere anche  

[Procedure consigliate e risoluzione dei problemi per il backup di SQL Server nell'URL](../../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md)   

[Backup e ripristino di database di sistema &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)   

[Esercitazione: Uso del servizio di archiviazione BLOB di Microsoft Azure con i database di SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)

[Backup di SQL Server nell'URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md)  
  
  
