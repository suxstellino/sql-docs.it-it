---
title: File di dati di SQL Server in Microsoft Azure | Microsoft Docs
description: Concetti e considerazioni fondamentali per archiviare i file di dati di SQL Server nel servizio di archiviazione di Microsoft Azure e alcuni vantaggi dell'utilizzo di Archiviazione di Azure.
ms.custom: ''
ms.date: 12/04/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
ms.assetid: 38ffd9c2-18a5-43d2-b674-e425addec4e4
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 8e9138ff27909f9236a608fd0c66f9dca8d5addf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351449"
---
# <a name="sql-server-data-files-in-microsoft-azure"></a>File di dati di SQL Server in Microsoft Azure
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  ![File di dati in Azure](../../relational-databases/databases/media/data-files-on-azure.png "File di dati in Azure")  
  
Con i file di dati di SQL Server in Microsoft Azure viene abilitato il supporto nativo dei file di database di SQL Server archiviati come BLOB. È possibile creare un database di SQL Server in esecuzione in locale o in una macchina virtuale in Microsoft Azure con un percorso di archiviazione dedicato per i dati nel servizio di archiviazione BLOB di Microsoft Azure. Si semplifica inoltre il processo di trasferimento dei database tra computer. È possibile scollegare i database da un computer e collegarli a un altro computer. Rende inoltre disponibile un percorso di archiviazione alternativo per i file di backup del database consentendo di eseguire il ripristino da o nel servizio di archiviazione di Microsoft Azure. Pertanto, rende possibile l'utilizzo di diverse soluzioni ibride offrendo numerosi vantaggi per la virtualizzazione dei dati, lo spostamento dei dati, la sicurezza e la disponibilità nonché costi moderatamente bassi e manutenzione per una disponibilità elevata e una scalabilità elastica.
 
> [!IMPORTANT]  
>  L'archiviazione dei database di sistema nell'archiviazione BLOB di Azure non è consigliata e non è supportata. 

 Questo argomento illustra concetti e considerazioni fondamentali per archiviare i file di dati di SQL Server nel servizio di archiviazione di Microsoft Azure.  
  
 Per un'esperienza pratica diretta di come usare questa nuova funzionalità, vedere [Esercitazione: Uso del servizio di archiviazione BLOB di Microsoft Azure con i database di SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md).  
  
## <a name="why-use-sql-server-data-files-in-microsoft-azure"></a>Perché usare i file di dati di SQL Server in Microsoft Azure? 

- **Vantaggi di una migrazione semplice e rapida:** questa funzionalità semplifica il processo di migrazione spostando un database alla volta tra computer locali nonché tra l'ambiente locale e l'ambiente cloud senza alcuna modifica alle applicazioni. Pertanto, supporta una migrazione incrementale senza influire sull'infrastruttura locale esistente. Inoltre, l'accesso a un'archiviazione dati centralizzata semplifica la logica dell'applicazione quando un'applicazione deve essere eseguita in più posizioni in un ambiente locale. In alcuni casi, può essere necessario installare rapidamente centri di calcolo in località geografiche diverse per la raccolta di dati da molte origini diverse. Tramite questa nuova funzionalità avanzata, anziché spostare dati da una posizione all'altra, è possibile archiviare molti database come oggetti BLOB di Microsoft Azure e quindi eseguire script Transact-SQL per creare i database in computer locali o in macchine virtuali.

- **Vantaggi in termini di archiviazione senza limiti e costi:** questa funzionalità consente di usare spazio di archiviazione esterno illimitato in Microsoft Azure sfruttando al tempo stesso risorse di calcolo locali. Quando si usa Microsoft Azure come posizione di archiviazione, è possibile concentrarsi sulla logica dell'applicazione senza l'overhead della gestione dell'hardware. Se si perde un nodo di calcolo locale, è possibile configurarne uno nuovo senza alcuno spostamento dei dati.

- **Vantaggi a livello di disponibilità elevata e ripristino di emergenza:** l'uso di file di dati di SQL Server in Microsoft Azure potrebbe semplificare le soluzioni di disponibilità elevata e ripristino di emergenza. Se ad esempio una macchina virtuale in Microsoft Azure o un'istanza di SQL Server si arresta in modo anomalo, è possibile ricreare i database in una nuova istanza di SQL Server semplicemente ristabilendo i collegamenti agli oggetti BLOB.

- **Vantaggi a livello di sicurezza:** con questo nuovo miglioramento è possibile separare un'istanza di calcolo da un'istanza di archiviazione. È possibile disporre di un database completamente crittografato da decrittografare solo in un'istanza di calcolo e non in un'istanza di archiviazione. In altri termini, usando questa nuova funzionalità avanzata è possibile crittografare tutti i dati in un cloud pubblico tramite i certificati TDE (Transparent Data Encryption) che sono separati fisicamente dai dati. Le chiavi TDE possono essere archiviate nel database master, che viene archiviato in locale nel computer fisicamente protetto in locale e di cui viene eseguito un backup in locale. È possibile usare queste chiavi locali per crittografare i dati che risiedono nel servizio di archiviazione di Microsoft Azure. Se le credenziali dell'account di archiviazione del cloud vengono rubate, i dati rimangono protetti perché i certificati TDE risiedono sempre in locale.

- **Backup di snapshot:**  questa funzionalità consente di usare gli snapshot di Azure per backup quasi istantanei e ripristini più veloci dei file di database archiviati con il servizio di archiviazione BLOB di Azure. Questa funzionalità consente di semplificare i criteri di backup e ripristino. Per altre informazioni, vedere [Backup di snapshot di file per i file di database in Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  

## <a name="concepts-and-requirements"></a>Concetti e requisiti  
  
I dischi di Azure sono compatibili con soluzioni di continuità aziendale e ripristino di emergenza a livello aziendale. Se i database vengono archiviati direttamente nei BLOB o nei file Premium di Azure, i dati non vengono associati automaticamente alla macchina virtuale per l'infrastruttura, la gestione e il monitoraggio.

L'inserimento dei file di database nei BLOB di pagine è una funzionalità più avanzata rispetto all'uso dei dischi di Azure, che sono semplici e intuitivi.

L'indicazione di base è usare i dischi di Azure, a meno che non esista uno scenario in cui è necessario evitare di creare una copia dei dati per i backup o di effettuare il ripristino come operazione di dimensione dei dati. Per la disponibilità elevata e il ripristino di emergenza, anche l'uso di un backup regolare in un URL o un backup gestito nell'archivio BLOB è molto più utile dei backup di snapshot di file, in quanto si ottengono la gestione del ciclo di vita, il supporto per più aree, l'eliminazione temporanea e tutte le altre funzionalità di archiviazione BLOB dei backup.

### <a name="azure-storage-concepts"></a>Concetti relativi ad Archiviazione di Azure  
Quando si usa la funzionalità relativa ai file di dati SQL Server in Azure, è necessario creare un account di archiviazione e un contenitore in Azure. Quindi, è necessario creare credenziali di SQL Server in cui includere le informazioni sui criteri del contenitore nonché una firma di accesso condiviso, necessaria per accedere al contenitore.  

In [Microsoft Azure](https://azure.microsoft.com), un account di [archiviazione di Azure](https://azure.microsoft.com/services/storage/) rappresenta il livello più elevato dello spazio dei nomi per accedere agli oggetti BLOB. Un account di archiviazione può contenere un numero illimitato di contenitori, purché la dimensione totale sia minore dei limiti di archiviazione. Per le informazioni più recenti sui limiti di archiviazione, vedere [Sottoscrizione di Azure e limiti, quote e vincoli dei servizi](/azure/azure-subscription-service-limits). Un contenitore consente di raggruppare un set di [BLOB](/azure/storage/common/storage-introduction#blob-storage). Tutti i BLOB devono essere inclusi in un contenitore. Un account può contenere un numero illimitato di contenitori. Analogamente, in un contenitore è possibile archiviare un numero illimitato di BLOB. Esistono due tipi di oggetti BLOB che è possibile archiviare in Archiviazione di Azure: BLOB in blocchi e BLOB di pagine. Questa nuova funzionalità usa i BLOB di pagine, che sono più efficienti quando gli intervalli di byte in un file vengono modificati spesso. È possibile accedere agli oggetti BLOB usando il seguente formato di URL: `https://storageaccount.blob.core.windows.net/<container>/<blob>`.  

### <a name="azure-billing-considerations"></a>Considerazioni sulla fatturazione in Azure  

 La stima dei costi associati all'uso dei servizi Azure è un aspetto importante del processo decisionale e di pianificazione. Quando si archiviano file di dati di SQL Server in Archiviazione di Azure, è necessario pagare i costi associati all'archiviazione e alle transazioni. Inoltre, l'implementazione della funzionalità relativa ai file di dati di SQL Server in Archiviazione di Azure richiede il rinnovo del lease di un oggetto BLOB ogni 45-60 secondi in modo implicito. Questa operazione comporta dei costi di transazione per ogni file di database, ad esempio per i file con estensione MDF o LDF. Usare le informazioni disponibili nella pagina [Prezzi di Azure](https://azure.microsoft.com/pricing/) per calcolare i costi mensili associati all'uso di Archiviazione di Azure e Macchine virtuali di Azure.  
  
### <a name="sql-server-concepts"></a>Concetti relativi a SQL Server  

Quando si usa questa nuova funzionalità avanzata, è necessario eseguire le operazioni seguenti:

- Creare i criteri in un contenitore e generare una chiave di firma di accesso condiviso (SAS, Shared Access Signature).

- Per ogni contenitore usato da un file di dati o di log, creare credenziali di SQL Server il cui nome corrisponda al percorso del contenitore.

- Archiviare le informazioni relative al contenitore di Archiviazione di Azure, il nome dei criteri e la chiave SAS associati nell'archivio delle credenziali di SQL Server.

Nell'esempio seguente si presuppone che sia stato creato un contenitore di archiviazione di Azure e che i criteri siano stati creati con diritti di lettura, scrittura ed elenco. La creazione dei criteri in un contenitore genera una chiave SAS che può essere conservata non crittografata in memoria e che è richiesta da SQL Server per accedere ai file BLOB nel contenitore. 

Nel frammento di codice seguente sostituire `'<your SAS key>'` con la chiave SAS. La chiave SAS sarà simile a `'sr=c&si=<MYPOLICYNAME>&sig=<THESHAREDACCESSSIGNATURE>'`.

```sql
CREATE CREDENTIAL [https://testdb.blob.core.windows.net/data]  
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = '<your SAS key>'  
  
CREATE DATABASE testdb   
ON  
( NAME = testdb_dat,  
    FILENAME = 'https://testdb.blob.core.windows.net/data/TestData.mdf' )  
 LOG ON  
( NAME = testdb_log,  
    FILENAME =  'https://testdb.blob.core.windows.net/data/TestLog.ldf')  
```  

>[!IMPORTANT]
>se sono presenti riferimenti attivi ai file di dati in un contenitore, i tentativi di eliminare le credenziali di SQL Server corrispondenti non riescono.

Per altre informazioni, vedere [Gestire l'accesso in lettura anonimo a contenitori e BLOB](/azure/storage/blobs/storage-manage-access-to-resources).  

### <a name="security"></a>Sicurezza  
 Di seguito sono riportati requisiti e considerazioni sulla sicurezza a cui attenersi durante l'archiviazione di file di dati di SQL Server in Archiviazione di Azure.

- Quando si crea un contenitore per il servizio di archiviazione BLOB di Azure, è consigliabile impostare l'accesso su privato. Quando si imposta l'accesso su privato, i dati del contenitore e del BLOB possono essere letti solo dal proprietario dell'account Azure.

- Quando si archiviano i file di database di SQL Server in Archiviazione di Azure, è necessario usare una firma di accesso condiviso, un URI che concede diritti di accesso limitati a contenitori, BLOB, code e tabelle. Con la firma di accesso condiviso è possibile abilitare SQL Server all'accesso alle risorse nell'account di archiviazione senza condividere la chiave dell'account di archiviazione di Azure.

- Inoltre, è consigliabile continuare a implementare le procedure di sicurezza locali tradizionali dei database.  
  
### <a name="installation-prerequisites"></a>Prerequisiti di installazione  
 Di seguito sono riportati i prerequisiti di installazione a cui attenersi durante l'archiviazione di file di dati di SQL Server in Azure.

- **SQL Server in locale:** questa funzionalità è inclusa in SQL Server 2016 e versioni successive. Per informazioni su come scaricare la versione più recente di SQL Server, vedere [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads).

- SQL Server in esecuzione in una macchina virtuale di Azure: se si installa [SQL Server in una macchina virtuale di Azure](https://azuremarketplace.microsoft.com/marketplace/apps?search=sql%20server&page=1), installare SQL Server 2016 o aggiornare l'istanza esistente. Analogamente, è possibile creare una nuova macchina virtuale in Azure usando un'immagine della piattaforma di SQL Server 2016.

  
###  <a name="limitations"></a><a name="bkmk_Limitations"></a> Limitazioni  
  
- Nella versione corrente di questa funzionalità, l'archiviazione dei dati **FileStream** in Archiviazione di Azure non è supportata. È possibile archiviare dati **FileStream** in un database contenente anche file di dati archiviati in Archiviazione di Azure, ma tutti i file di dati FileStream devono essere archiviati in un archivio locale.  Poiché i dati FileStream devono risiedere in un archivio locale, non possono essere spostati tra computer usando Archiviazione di Azure. È quindi consigliabile continuare a usare le [tecniche tradizionali](../../relational-databases/blob/move-a-filestream-enabled-database.md) per spostare i dati associati a FileStream tra computer diversi.  
  
- Attualmente, questa nuova funzionalità avanzata non supporta più istanze di SQL Server che accedono contemporaneamente agli stessi file di database in Archiviazione di Azure. Se ServerA è online con un file di database attivo e se ServerB viene avviato per errore e include anch'esso un database che punta allo stesso file di dati, il secondo server non riuscirà ad avviare il database generando il codice errore **5120 Impossibile aprire il file fisico "%.\*ls". Errore del sistema operativo %d: "%ls"** .  
  
- Solo i file con estensione mdf, ldf e ndf possono essere archiviati in Archiviazione di Azure con la funzionalità relativa ai file di dati di SQL Server in Azure.  
  
- Quando si usa la funzionalità relativa ai file di dati di SQL Server in Azure, la replica geografica per l'account di archiviazione non è supportata. Se un account di archiviazione viene sottoposto alla replica a livello geografico e si verifica un failover a livello geografico, il database può danneggiarsi.  
  
- Per le limitazioni di capacità, vedere [Introduzione all'archiviazione BLOB](/azure/storage/blobs/storage-blobs-introduction).  
  
- Non è possibile archiviare i dati di OLTP in memoria nell'archiviazione BLOB usando la funzionalità relativa ai file di dati di SQL Server in Archiviazione di Azure. Ciò è dovuto al fatto che i dati di OLTP in memoria dipendono da **FileStream** e nella versione corrente di questa funzionalità l'archiviazione dei dati **FileStream** in Archiviazione di Azure non è supportata.  
  
- Quando si usa la funzionalità relativa ai file di dati di SQL Server in Azure, in SQL Server vengono eseguiti tutti i confronti tra URL e percorsi di file usando le regole di confronto impostate nel database **master**.  
  
- I **gruppi di disponibilità AlwaysOn** sono supportati solo se non si aggiungono nuovi file di database al database primario. Se un'operazione sul database richiede la creazione di un nuovo file nel database primario, disabilitare prima i gruppi di disponibilità nel nodo secondario. Eseguire quindi l'operazione sul database primario ed eseguire il backup del database nel nodo primario. In seguito ripristinare il database nel nodo secondario. Al termine, abilitare di nuovo i gruppi di disponibilità AlwaysOn nel nodo secondario. 

   >[!NOTE]
   >Le istanze del cluster di failover AlwaysOn non sono supportate quando si usa la funzionalità relativa ai file di dati di SQL Server in Azure.
  
- Durante il normale funzionamento, in SQL Server vengono usati lease temporanei per riservare gli oggetti BLOB per l'archiviazione con un rinnovo del lease di ogni oggetto BLOB ogni 45-60 secondi. Se un server viene arrestato in modo anomalo e viene avviata un'altra istanza di SQL Server configurata per usare gli stessi oggetti BLOB, la nuova istanza dovrà attendere fino a 60 secondi per la scadenza del lease esistente nell'oggetto BLOB. Se si vuole collegare il database a un'altra istanza e non è possibile attendere i 60 secondi per la scadenza del lease, è possibile interrompere in modo esplicito il lease nell'oggetto BLOB .
  
## <a name="tools-and-programming-reference-support"></a>Strumenti e supporto di riferimento per la programmazione  
 Questa sezione descrive gli strumenti e le librerie di riferimento per la programmazione che è possibile usare quando si archiviano file di dati di SQL Server in Archiviazione di Azure.  
  
### <a name="powershell-support"></a>Supporto PowerShell  
 Usare i cmdlet di PowerShell per archiviare file di dati di SQL Server nel servizio di archiviazione BLOB facendo riferimento a un percorso URL di archiviazione BLOB anziché a un percorso di file. Accedere agli oggetti BLOB usando il formato di URL seguente`https://storageaccount.blob.core.windows.net/<container>/<blob>`.  
  
### <a name="sql-server-object-and-performance-counters-support"></a>Supporto di oggetti di SQL Server e dei contatori delle prestazioni  
 A partire da SQL Server 2014 è stato aggiunto un nuovo oggetto di SQL Server da usare con la funzionalità relativa ai file di dati di SQL Server in Archiviazione di Azure. Il nuovo oggetto di SQL Server è denominato [SQL Server, HTTP_STORAGE_OBJECT](../../relational-databases/performance-monitor/sql-server-http-storage-object.md) e può essere usato da Monitoraggio di sistema per monitorare l'attività quando SQL Server viene eseguito con Archiviazione di Azure.  
  
### <a name="sql-server-management-studio-support"></a>Supporto di SQL Server Management Studio  
 SQL Server Management Studio consente di usare questa funzionalità tramite diverse finestre di dialogo. Ad esempio, `https://teststorageaccnt.blob.core.windows.net/testcontainer/` rappresenta il percorso URL di un contenitore di archiviazione.
 
 come un **percorso** in diverse finestre di dialogo, ad esempio **Nuovo database**, **Collega database** e **Ripristina database**. Per altre informazioni, vedere [Esercitazione: Uso del servizio di archiviazione BLOB di Microsoft Azure con i database di SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md).  
  
### <a name="sql-server-management-objects-smo-support"></a>Supporto di SQL Server Management Objects (SMO)  
 Quando si usa la funzionalità relativa ai file di dati di SQL Server in Azure, sono supportati tutti gli oggetti SMO (SQL Server Management Objects). Se un oggetto SMO richiede un percorso di file, usare il formato URL dell'oggetto BLOB anziché un percorso di file locale, ad esempio `https://teststorageaccnt.blob.core.windows.net/testcontainer/`. Per altre informazioni su SQL Server Management Objects (SMO), vedere [Guida alla programmazione di SMO &#40;SQL Server Management Objects&#41;](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md) nella documentazione online di SQL Server.  
  
### <a name="transact-sql-support"></a>Supporto di Transact-SQL  
 Questa nuova funzionalità ha introdotto la seguente modifica nella superficie di attacco di Transact-SQL:

- Una nuova colonna **int** , **credential_id**, nella vista di sistema **sys.master_files** . La colonna **credential_id** viene usata per abilitare i file di dati del servizio di archiviazione di Azure in modo che possano essere riassociati a `sys.credentials` con un riferimento incrociato per le credenziali create per i file stessi. È possibile usarla per la risoluzione dei problemi, ad esempio quando una credenziale non può essere eliminata se usata da un file di database.  
  
##  <a name="troubleshooting-for-sql-server-data-files-in-microsoft-azure"></a><a name="bkmk_Troubleshooting"></a> Risoluzione dei problemi della funzionalità relativa ai file di dati di SQL Server in Microsoft Azure  
 Per evitare errori a causa di limitazioni o funzionalità non supportate, rivedere innanzitutto [Limitazioni](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md#bkmk_Limitations).  
  
 Di seguito è riportato l'elenco di errori che possono verificarsi durante l'uso della funzionalità relativa ai file di dati di SQL Server in Archiviazione di Azure.  
  
 **Errori di autenticazione**  
  
- *Impossibile eliminare le credenziali '%.\*ls' perché usate da un file di database attivo.*    
    Risoluzione: questo errore può essere visualizzato quando si tenta di eliminare credenziali ancora usate da un file di database attivo in Archiviazione di Azure. Per eliminare le credenziali, è innanzitutto necessario eliminare l'oggetto BLOB associato a questo file di database. Per eliminare un BLOB con un lease attivo è innanzitutto necessario rilasciare il lease.  
  
- *La firma di accesso condiviso non è stata creata correttamente nel contenitore.*    
     Risoluzione: assicurarsi di avere creato correttamente una firma di accesso condiviso nel contenitore. Esaminare le istruzioni riportate nella lezione 2 di [Esercitazione: Uso del servizio di archiviazione BLOB di Microsoft Azure con i database di SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#2---create-a-sql-server-credential-using-a-shared-access-signature).  
  
- *Le credenziali di SQL Server non sono state create correttamente.*    
    Risoluzione: verificare di aver usato "Shared Access Signature" per il campo **Identity** e di aver creato correttamente un segreto. Esaminare le istruzioni riportate nella lezione 3 di [Esercitazione: Uso del servizio di archiviazione BLOB di Microsoft Azure con i database di SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#3---database-backup-to-url).  
  
 **Errori di lease di oggetti BLOB:**  
  
- Errore quando si tenta di avviare SQL Server dopo che un'altra istanza che usava gli stessi file BLOB si è arrestata in modo anomalo. Risoluzione: Durante il normale funzionamento, in SQL Server vengono usati lease temporanei per riservare gli oggetti BLOB per l'archiviazione con un rinnovo del lease di ogni oggetto BLOB ogni 45-60 secondi. Se un server viene arrestato in modo anomalo e viene avviata un'altra istanza di SQL Server configurata per usare gli stessi oggetti BLOB, la nuova istanza dovrà attendere fino a 60 secondi per la scadenza del lease esistente nell'oggetto BLOB. Se si vuole collegare il database a un'altra istanza e non è possibile attendere i 60 secondi per la scadenza del lease, è possibile rilasciare in modo esplicito il lease nell'oggetto BLOB per evitare eventuali errori nelle operazioni di collegamento.  
  
 **Errori di database**  
  
**Errori durante la creazione di un database** Soluzione: Esaminare le istruzioni riportate nella lezione 4 di [Esercitazione: Uso del servizio di archiviazione BLOB di Microsoft Azure con i database di SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#4----restore-database-to-virtual-machine-from-url).  
  
**Errori durante l'esecuzione dell'istruzione Alter** Soluzione: accertarsi di eseguire l'istruzione Alter Database quando il database è online. Quando si copiano i file di dati in Archiviazione di Azure, creare sempre un BLOB di pagine e non un BLOB in blocchi. In caso contrario, l'istruzione ALTER Database avrà esito negativo. Esaminare le istruzioni riportate nella lezione 7 di [Esercitazione: Uso del servizio di archiviazione BLOB di Microsoft Azure con i database di SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md).  
  
**Codice di errore 5120 Impossibile aprire il file fisico "%.\*ls". Errore %d: "%ls" del sistema operativo**   

Risoluzione: attualmente, questo nuovo miglioramento non supporta l'accesso simultaneo di più istanze di SQL Server agli stessi file di database in Archiviazione di Azure. Se ServerA è online con un file di database attivo e se ServerB viene avviato per errore e include anch'esso un database che punta allo stesso file di dati, il secondo server non riuscirà ad avviare il database generando il codice errore *5120 Impossibile aprire il file fisico "%.\*ls". Errore del sistema operativo %d: "%ls"* .  
  
Per risolvere questo problema, determinare innanzitutto se è necessario che ServerA acceda al file di database in Archiviazione di Azure. Se non è necessario, rimuovere qualsiasi connessione tra ServerA e i file di database in Archiviazione di Azure. A questo scopo, seguire questa procedura:  

1.  Impostare il percorso di file di ServerA su una cartella locale tramite l'istruzione ALTER Database.  

2.  Impostare il database come offline in ServerA.  

3.  Copiare quindi i file di database da Archiviazione di Azure nella cartella locale in ServerA, in modo da garantire che ServerA contenga una copia del database in locale.  

4.  Impostare il database come online.

**Codice di errore 833 - Il completamento della richiesta di I/O sta impiegando più di 15 secondi** 
   
   Questo errore indica che il sistema di archiviazione non è in grado di soddisfare le esigenze del carico di lavoro di SQL Server. Ridurre l'attività di I/O dal livello applicazione o aumentare la capacità di velocità effettiva al livello archiviazione. Per altre informazioni, vedere [Errore 833](../errors-events/mssqlserver-833-database-engine-error.md). Se i problemi di prestazioni persistono, valutare la possibilità di trasferire i file in un livello di archiviazione diverso, ad esempio Premium o UltraSSD. Per SQL Server in macchine virtuali di Azure, vedere [Ottimizzare le prestazioni di archiviazione](/azure/virtual-machines/premium-storage-performance).


## <a name="next-steps"></a>Passaggi successivi  
  
[Creare un database](create-a-database.md)
