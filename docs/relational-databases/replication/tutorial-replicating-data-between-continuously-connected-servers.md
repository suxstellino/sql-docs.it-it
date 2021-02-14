---
title: 'Esercitazione: Configurare la replica transazionale'
description: Questa esercitazione illustra come configurare la replica transazionale tra due server SQL completamente connessi.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- tutorials [SQL Server replication]
- replication [SQL Server], tutorials
- wizards [SQL Server replication]
ms.assetid: 7b18a04a-2c3d-4efe-a0bc-c3f92be72fd0
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e2a27d4fccfb24aff29668651ea914e11429910f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347127"
---
# <a name="tutorial-configure-replication-between-two-fully-connected-servers-transactional"></a>Esercitazione: Configurare la replica tra due server sempre connessi (replica transazionale)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
La replica transazionale è una buona soluzione al problema legato al trasferimento dei dati tra server con connessione continua. Con la procedura guidata di replica è possibile eseguire in modo semplificato i passaggi necessari per configurare e amministrare una topologia di replica. 

Questa esercitazione illustra come configurare una topologia di replica transazionale per server con connessione continua. Per altre informazioni sul funzionamento della replica transazionale, vedere la [panoramica della replica transazionale](./transactional/transactional-replication.md). 
  
## <a name="what-you-will-learn"></a>Lezioni dell'esercitazione  
Questa esercitazione insegna come pubblicare dati da un database all'altro tramite la replica transazionale.  

In questa esercitazione verranno illustrate le procedure per:
> [!div class="checklist"]
> * Creare un server di pubblicazione tramite la replica transazionale.
> * Creare un sottoscrittore per il server di pubblicazione transazionale.
> * Convalidare la sottoscrizione e misurare la latenza.
  
  
## <a name="prerequisites"></a>Prerequisiti  
Questa esercitazione è destinata agli utenti che hanno familiarità con le operazioni di base relative ai database ma con un'esperienza limitata delle operazioni di replica. Prima di iniziare questa esercitazione è necessario completare l'[Esercitazione: Preparare SQL Server per la replica](../../relational-databases/replication/tutorial-preparing-the-server-for-replication.md).  
  
Per completare questa esercitazione, sono necessari SQL Server, SQL Server Management Studio (SSMS) e un database AdventureWorks:  
  
- Nel server di pubblicazione (origine) installare:  
  
   - Una qualsiasi edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad eccezione di SQL Server Express e di SQL Server Compact. Questa edizioni non possono fungere da server di pubblicazione per la replica.   
   - Database di esempio [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] . Per una maggiore sicurezza, i database di esempio non vengono installati per impostazione predefinita.  
  
- Nel server sottoscrittore (destinazione) installare qualsiasi edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad eccezione di [!INCLUDE[ssEW](../../includes/ssew-md.md)]. [!INCLUDE[ssEW](../../includes/ssew-md.md)] non può essere un sottoscrittore nella replica transazionale.  
  
- Installare [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md).
- Installare [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Scaricare il [database di esempio AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases). Per istruzioni sul ripristino di un database in SSMS, vedere [Ripristino di un database](../backup-restore/restore-a-database-backup-using-ssms.md). 
 
>[!NOTE]
> - La replica non è supportata nelle istanze di SQL Server con versioni la cui distanza sia maggiore di 2. Per altre informazioni, vedere [Supported SQL Server Versions in Replication Topology](https://blogs.msdn.microsoft.com/repltalk/2016/08/12/suppported-sql-server-versions-in-replication-topology/) (Versioni di SQL Server supportate nella topologia di replica).
> - In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] è necessario connettersi al server di pubblicazione e al sottoscrittore usando un account di accesso che sia membro del ruolo predefinito del server **sysadmin**. Per altre informazioni su questo ruolo, vedere [Ruoli a livello di server](../security/authentication-access/server-level-roles.md).  
  
  
**Tempo stimato per il completamento dell'esercitazione: 60 minuti**  
  
## <a name="configure-the-publisher-for-transactional-replication"></a>Configurare il server di pubblicazione per la replica transazionale
In questa sezione viene creata una pubblicazione transazionale usando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per pubblicare un subset filtrato della tabella **Product** nel database di esempio [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Viene anche aggiunto l'account di accesso di SQL Server usato dall'agente di distribuzione all'elenco di accesso alla pubblicazione.


### <a name="create-a-publication-and-define-articles"></a>Creare una pubblicazione e definire articoli
1. Connettersi al server di pubblicazione in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] e quindi espandere il nodo server.  
  
2. Fare clic con il pulsante destro del mouse su **SQL Server Agent** e selezionare **Avvia**. SQL Server Agent deve essere in esecuzione prima della creazione della pubblicazione. Se con questa operazione l'agente non si avvia, è necessario avviarlo manualmente da Gestione configurazione SQL Server. 
3. Espandere la cartella **Replica**, fare clic con il pulsante destro del mouse sulla cartella **Pubblicazioni locali** e selezionare **Nuova Pubblicazione**. Questa operazione avvia la Creazione guidata nuova pubblicazione:  

   ![Selezioni per l'avvio della Creazione guidata nuova pubblicazione](media/tutorial-replicating-data-between-continuously-connected-servers/newpublication.png)
  
  
3. Nella pagina **Database di pubblicazione** selezionare [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] e quindi **Avanti**.  
  
4. Nella pagina **Tipo di pubblicazione** selezionare **Pubblicazione transazionale** e quindi **Avanti**:  

   ![Pagina "Tipo di pubblicazione" con il tipo di pubblicazione selezionato](media/tutorial-replicating-data-between-continuously-connected-servers/tranrepl.png)
  
5. Nella pagina **Articoli** espandere il nodo **Tabelle** e selezionare la casella di controllo **Product**. Espandere quindi **Product** e deselezionare le caselle di controllo accanto a **ListPrice** e a **StandardCost**. Selezionare **Avanti**.  

   ![Pagina "Articoli" con gli articoli selezionati per la pubblicazione](media/tutorial-replicating-data-between-continuously-connected-servers/replarticles.png)
  
6. Nella pagina **Filtro righe tabella** selezionare **Aggiungi**.   
  
7. Nella finestra di dialogo **Aggiungi filtro** selezionare la colonna **SafetyStockLevel**. Selezionare la freccia a destra per aggiungere la colonna alla clausola WHERE in Istruzione per il filtro nella query di filtro. Digitare quindi manualmente il modificatore della clausola WHERE come segue:  
  
   ```sql  
   WHERE [SafetyStockLevel] < 500  
   ```
  
   ![Pagina "Filtro righe tabella" e finestra di dialogo "Aggiungi filtro"](media/tutorial-replicating-data-between-continuously-connected-servers/filter.png)
  
8. Selezionare **OK**, quindi **Avanti**.  
  
9. Selezionare la casella di controllo **Crea uno snapshot immediatamente e mantieni lo snapshot disponibile per l'inizializzazione delle sottoscrizioni** e selezionare **Avanti**:  

   ![Pagina "Agente di snapshot" con la casella di controllo selezionata](media/tutorial-replicating-data-between-continuously-connected-servers/snapshot.png)
  
10. Nella pagina **Sicurezza agente** deselezionare la casella di controllo **Usa le impostazioni di sicurezza dell'agente snapshot**.   
  
    Selezionare **Impostazioni di sicurezza** per l'agente snapshot. Immettere <*Nome_server_pubblicazione*> **\repl_snapshot** nella casella **Account processo**, specificare la password per l'account e quindi selezionare **OK**.  

    ![Pagina "Sicurezza agente" e finestra di dialogo "Sicurezza agente snapshot"](media/tutorial-replicating-data-between-continuously-connected-servers/snapshotagentsecurity.png)
  
12. Ripetere il passaggio precedente per impostare <*Nome_server_pubblicazione*> **\repl_logreader** come account di processo per l'agente di lettura log Selezionare **OK**.  

    ![Finestra di dialogo "Sicurezza agente di lettura log" e pagina "Sicurezza agente"](media/tutorial-replicating-data-between-continuously-connected-servers/logreaderagentsecurity.png)   

  
13. Nella pagina **Completamento procedura guidata** digitare **AdvWorksProductTrans** nella casella **Nome pubblicazione** e selezionare **Fine**:  

    ![Pagina "Completamento procedura guidata" con il nome della pubblicazione](media/tutorial-replicating-data-between-continuously-connected-servers/advworksproducttrans.png)
  
14. Dopo la creazione della pubblicazione, selezionare **Chiudi** per completare la procedura guidata. 

Se quando si tenta di creare la pubblicazione SQL Server Agent non è in esecuzione, può essere visualizzato l'errore riportato di seguito, che indica che la pubblicazione è stata creata ma non è stato possibile avviare l'agente di snapshot. In questo caso, è necessario avviare SQL Server Agent e quindi avviare manualmente l'agente di snapshot. Le istruzioni sono descritte nella prossima sezione. 

![Avviso del mancato avvio dell'agente di snapshot](media/tutorial-replicating-data-between-continuously-connected-servers/snapshotagenterror.png)
    
  
### <a name="view-the-status-of-snapshot-generation"></a>Visualizzare lo stato della generazione dello snapshot  
  
1. Connettersi al server di pubblicazione in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], espandere il nodo del server e quindi la cartella **Replica**.  
  
2. Nella cartella **Pubblicazioni locali** fare clic con il pulsante destro del mouse su **AdvWorksProductTrans** e quindi selezionare **Visualizza stato agente snapshot**:  
   ![Comandi del menu di scelta rapida per la visualizzazione dello stato dell'agente di snapshot](media/tutorial-replicating-data-between-continuously-connected-servers/viewsnapshot.png)
  
3. Verrà visualizzato lo stato corrente del processo dell'agente di snapshot per la pubblicazione. Verificare che il processo snapshot abbia avuto esito positivo prima di passare alla sezione successiva.
          
Se al momento della creazione della pubblicazione SQL Server Agent non era in esecuzione, l'agente di snapshot risulta mai eseguito in Visualizza stato agente snapshot per la pubblicazione. In questo caso, selezionare **Avvia** per avviare l'agente di snapshot: 

![Pulsante "Avvia" e modifica nel messaggio di stato che indica che l'agente di snapshot è stato eseguito](media/tutorial-replicating-data-between-continuously-connected-servers/startsnapshotagent.png)
     
Se a questo punto viene visualizzato un errore, vedere [Troubleshooting Snapshot Agent error](troubleshoot-tran-repl-errors.md#find-errors-with-the-snapshot-agent) (Risoluzione dei problemi dell'agente di snapshot).


  
### <a name="add-the-distribution-agent-login-to-the-pal"></a>Aggiungere l'account di accesso dell'agente di distribuzione all'elenco di accesso alla pubblicazione  
  
1. Connettersi al server di pubblicazione in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], espandere il nodo del server e quindi la cartella **Replica**.  
  
2. Nella cartella **Pubblicazioni locali** fare clic con il pulsante destro del mouse su **AdvWorksProductTrans** e quindi selezionare **Proprietà**.  Verrà visualizzata la finestra di dialogo **Proprietà pubblicazione**.    
  
   a. Selezionare la pagina **Elenco di accesso alla pubblicazione** e selezionare **Aggiungi**.  
   b. Nella finestra di dialogo **Aggiungi accesso alla pubblicazione** selezionare <*Nome_server_pubblicazione*> **\repl_distribution** e selezionare **OK**.
   
   ![Selezioni per l'aggiunta di un account di accesso all'elenco di accesso alla pubblicazione](media/tutorial-replicating-data-between-continuously-connected-servers/tranreplproperties.png)

Per altre informazioni, vedere [Concetti di base relativi alla programmazione della replica](../../relational-databases/replication/concepts/replication-programming-concepts.md).  
  

## <a name="create-a-subscription-to-the-transactional-publication"></a>Creare una sottoscrizione per la pubblicazione transazionale
In questa sezione si aggiunge un sottoscrittore alla pubblicazione creata in precedenza. Questa esercitazione usa un sottoscrittore remoto (NODE2\SQL2016), ma è anche possibile aggiungere localmente una sottoscrizione al server di pubblicazione. 

### <a name="create-the-subscription"></a>Creare la sottoscrizione  
  
1. Connettersi al server di pubblicazione in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], espandere il nodo del server e quindi la cartella **Replica**.  
  
2. Nella cartella **Pubblicazioni locali** fare clic con il pulsante destro del mouse sulla pubblicazione **AdvWorksProductTrans** e quindi selezionare **Nuove sottoscrizioni**. Verrà avviata la Creazione guidata nuova sottoscrizione: 
 
   ![Selezioni per avviare la Creazione guidata nuova sottoscrizione](media/tutorial-replicating-data-between-continuously-connected-servers/newsub.png)     
  
3. Nella pagina **Pubblicazione** selezionare **AdvWorksProductTrans** e quindi selezionare **Avanti**:  

   ![Pagina "Pubblicazione" con la pubblicazione selezionata](media/tutorial-replicating-data-between-continuously-connected-servers/selectpub.png)
  
4. Nella pagina **Posizione in cui eseguire l'agente di distribuzione** selezionare **Esegui tutti gli agenti nel database di distribuzione** e quindi selezionare **Avanti**.  Per altre informazioni sulle sottoscrizioni pull e push, vedere [Sottoscrivere le pubblicazioni](./subscribe-to-publications.md).

   ![Pagina "Posizione in cui eseguire l'agente di distribuzione" con l'opzione "Esegui tutti gli agenti nel database di distribuzione" selezionata](media/tutorial-replicating-data-between-continuously-connected-servers/runagentsatdist.png)
  
5. Nella pagina **Sottoscrittori**, se il nome dell'istanza del sottoscrittore non è visualizzato, selezionare **Aggiungi sottoscrittore** e quindi selezionare **Aggiungi Sottoscrittore SQL Server** dall'elenco a discesa. Questa operazione consente di aprire la finestra di dialogo **Connetti al server**. Immettere il nome dell'istanza del sottoscrittore e quindi selezionare **Connetti**.  
    
   Dopo l'aggiunta del sottoscrittore, selezionare la casella accanto al nome dell'istanza di questo e quindi selezionare **Nuovo database** in **Database di sottoscrizione**.   

   ![Pagina "Sottoscrittori" con le selezioni per l'aggiunta di un server di sottoscrizione](media/tutorial-replicating-data-between-continuously-connected-servers/addsub.png)

7. Verrà visualizzata la finestra di dialogo **Nuovo database**. Nella casella **Nome database** immettere **ProductReplica**, selezionare **OK** e quindi selezionare **Avanti**: 
  
   ![Immissione di un nome per il database di sottoscrizione](media/tutorial-replicating-data-between-continuously-connected-servers/productreplica.png)
  
8. Nella pagina **Sicurezza agente di distribuzione** selezionare il pulsante con i puntini di sospensione ( **...** ). Immettere <*Nome_server_pubblicazione*> **\repl_distribution** nella casella **Account processo**, immettere la password per l'account, selezionare **OK**, e quindi selezionare **Avanti**.

   ![Informazioni dell'account di distribuzione nella finestra di dialogo "Sicurezza agente di distribuzione"](media/tutorial-replicating-data-between-continuously-connected-servers/adddistaccount.png)
  
9. Selezionare **Fine** per accettare i valori predefiniti nelle pagine rimanenti e completare la procedura guidata.  
  
### <a name="set-database-permissions-at-the-subscriber"></a>Impostare le autorizzazioni per il database nel sottoscrittore  
  
1. Connettersi al sottoscrittore in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Espandere la cartella **Sicurezza**, fare clic con il pulsante destro del mouse su **Account di accesso** e quindi scegliere **Nuovo account accesso**.     
  
   a. Nella pagina **Generale**, in **Nome account di accesso**, selezionare **Cerca** e aggiungere l'account di accesso per <*Nome_computer_sottoscrittore*> **\repl_distribution**.

   b. Nella pagina **Mapping utenti** concedere all'account di accesso **db_owner** l'appartenenza per il database **ProductReplica**. 

   ![Selezioni per la configurazione dell'accesso nel sottoscrittore](media/tutorial-replicating-data-between-continuously-connected-servers/loginforsub.png)

2. Selezionare **OK** per chiudere la finestra di dialogo **Nuovo account accesso**. 

  
### <a name="view-the-synchronization-status-of-the-subscription"></a>Visualizzare lo stato di sincronizzazione della sottoscrizione  
  
1. Connettersi al server di pubblicazione in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Espandere il nodo server e la cartella **Replica**.  
  
2. Nella cartella **Pubblicazioni locali** espandere la pubblicazione **AdvWorksProductTrans**, fare clic con il pulsante destro del mouse sulla sottoscrizione nel database **ProductReplica** e quindi selezionare **Visualizza stato sincronizzazione**. Verrà visualizzato lo stato corrente della sincronizzazione della sottoscrizione:

   ![Selezioni per l'apertura della finestra di dialogo "Visualizza stato sincronizzazione"](media/tutorial-replicating-data-between-continuously-connected-servers/viewsyncstatus.png)
3. Se la sottoscrizione non è visualizzata in **AdvWorksProductTrans**, premere F5 per aggiornare l'elenco.  
  
Per altre informazioni, vedere:  
- [Inizializzare una sottoscrizione con uno snapshot](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)  
- [Creare una sottoscrizione push](../../relational-databases/replication/create-a-push-subscription.md)  
- [Sottoscrivere le pubblicazioni](../../relational-databases/replication/subscribe-to-publications.md)  

## <a name="measure-replication-latency"></a>Misurare la latenza di replica
In questa sezione vengono usati token di traccia per verificare che le modifiche vengano replicate nel sottoscrittore e per determinare la latenza. La latenza è il tempo necessario perché una modifica effettuata nel server di pubblicazione sia visibile nel sottoscrittore.
  
1. Connettersi al server di pubblicazione in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Espandere il nodo server, fare clic con il pulsante destro del mouse sulla cartella **Replica** e quindi selezionare **Avvia Monitoraggio replica**:

   ![Comando "Avvia Monitoraggio replica" nel menu di scelta rapida](media/tutorial-replicating-data-between-continuously-connected-servers/launchreplmonitor.png)

2. Espandere un gruppo di server di pubblicazione nel riquadro sinistro, espandere l'istanza del server di pubblicazione e quindi selezionare la pubblicazione **AdvWorksProductTrans**.  
  
   a. Selezionare la scheda **Token di traccia**.  
   b. Selezionare **Inserisci utilità di traccia**.    
   c. Visualizzare il tempo trascorso per il token di traccia nelle colonne seguenti: **Dal server di pubblicazione al server di distribuzione**, **Dal server di distribuzione al Sottoscrittore**, **Latenza totale**. Il valore **In sospeso** indica che il token non ha raggiunto il punto specificato.

   ![Informazioni per il token di traccia](media/tutorial-replicating-data-between-continuously-connected-servers/tracertoken.png)


Per altre informazioni, vedere: 
- [Misurare la latenza e convalidare le connessioni per la replica transazionale](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)
- [Trovare gli errori con la replica transazionale di SQL Server](troubleshoot-tran-repl-errors.md)


## <a name="next-steps"></a>Passaggi successivi
È stata eseguita la configurazione sia del server di pubblicazione che del sottoscrittore per la replica transazionale. È ora possibile inserire, aggiornare ed eliminare dati nella tabella **Product** nel server di pubblicazione. È anche possibile eseguire una query nella tabella **Product** dal sottoscrittore per visualizzare le modifiche replicate. 

Il prossimo articolo illustrerà come configurare la replica di tipo merge:  

> [!div class="nextstepaction"]
> [Esercitazione: Configurare la replica tra un server e più client per dispositivi mobili (replica di tipo merge)](tutorial-replicating-data-with-mobile-clients.md)