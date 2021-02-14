---
title: 'Esercitazione: Configurare la replica di tipo merge'
description: Questa esercitazione illustra come configurare la replica di tipo merge tra un'istanza di SQL Server e un client per dispositivi mobili.
ms.custom: seo-lt-2019
ms.date: 04/03/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: quickstart
helpviewer_keywords:
- replication [SQL Server], tutorials
ms.assetid: af673514-30c7-403a-9d18-d01e1a095115
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3f29e39d8b66b5adafb4c84de6a8f1df99ce95bb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348616"
---
# <a name="tutorial-configure-replication-between-a-server-and-mobile-clients-merge"></a>Esercitazione: Configurare la replica tra un server e più client per dispositivi mobili (replica di tipo merge)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
La replica di tipo merge è una buona soluzione al problema legato al trasferimento dei dati tra un server centrale e client per dispositivi mobili connessi solo occasionalmente. Le procedure guidate relative alla replica consentono di eseguire in modo semplificato i passaggi necessari per configurare e amministrare una topologia di replica di tipo merge. 

In questa esercitazione viene illustrato come configurare una topologia di replica per client mobili. Per altre informazioni sulla replica di tipo merge, vedere [Panoramica della replica di tipo merge](./merge/merge-replication.md).
  
## <a name="what-you-will-learn"></a>Lezioni dell'esercitazione  
Questa esercitazione illustra come usare la replica di tipo merge per pubblicare dati da un database centrale per uno o più utenti di dispositivi mobili in modo che ogni utente riceva un subset dei dati filtrato in modo univoco. 

In questa esercitazione verranno illustrate le procedure per:
> [!div class="checklist"]
> * Configurare un server di pubblicazione per la replica di tipo merge.
> * Aggiungere un sottoscrittore per dispositivi mobili per la pubblicazione di tipo merge.
> * Sincronizzare la sottoscrizione con la pubblicazione di tipo merge.
  
## <a name="prerequisites"></a>Prerequisiti  
Questa esercitazione è destinata agli utenti che hanno familiarità con le operazioni fondamentali relative ai database ma con un'esperienza limitata delle operazioni di replica. Prima di iniziare questa esercitazione è necessario completare l'[Esercitazione: Preparare SQL Server per la replica](../../relational-databases/replication/tutorial-preparing-the-server-for-replication.md).  
  
Per completare questa esercitazione, sono necessari SQL Server, SQL Server Management Studio (SSMS) e un database AdventureWorks: 
  
- Nel server di pubblicazione (origine) installare:  
  
   - Una qualsiasi edizione di SQL Server, ad eccezione di SQL Server Express e SQL Server Compact. Queste edizioni non possono fungere da server di pubblicazione per la replica.   
   - Database di esempio [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . Per una maggiore sicurezza, i database di esempio non vengono installati per impostazione predefinita.  
  
- Nel server sottoscrittore (destinazione) installare qualsiasi edizione di SQL Server, ad eccezione di SQL Server Express o SQL Server Compact. La pubblicazione creata in questa esercitazione non supporta SQL Server Express o SQL Server Compact. 

- Installare [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md).
- Installare [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Scaricare il [database di esempio AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases). Per istruzioni sul ripristino di un database in SSMS, vedere [Ripristino di un database](../backup-restore/restore-a-database-backup-using-ssms.md).  
 
  
>[!NOTE]
> - La replica non è supportata nelle istanze di SQL Server con versioni la cui distanza sia maggiore di 2. Per altre informazioni, vedere [Supported SQL Server Versions in Replication Topology](https://blogs.msdn.microsoft.com/repltalk/2016/08/12/suppported-sql-server-versions-in-replication-topology/) (Versioni di SQL Server supportate nella topologia di replica).
> - In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] è necessario connettersi al server di pubblicazione e al sottoscrittore usando un account di accesso che sia membro del ruolo predefinito del server **sysadmin**. Per altre informazioni su questo ruolo, vedere [Ruoli a livello di server](../security/authentication-access/server-level-roles.md).  
  
  
**Tempo stimato per il completamento dell'esercitazione: 60 minuti**  
  
## <a name="configure-a-publisher-for-merge-replication"></a>Configurare un server di pubblicazione per la replica di tipo merge
In questa sezione viene creata una pubblicazione di tipo merge con [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per pubblicare un subset delle tabelle **Employee**, **SalesOrderHeader** e **SalesOrderDetail** nel database di esempio [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Queste tabelle vengono filtrate usando filtri di riga con parametri in modo che ogni sottoscrizione contenga una partizione univoca dei dati. Viene inoltre aggiunto l'account di accesso di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usato dall'agente di merge all'elenco di accesso alla pubblicazione.  
  
### <a name="create-merge-publication-and-define-articles"></a>Creare una pubblicazione di tipo merge e definire articoli  
  
1. Connettersi al server di pubblicazione in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] e quindi espandere il nodo server.  
  
2. Avviare SQL Server Agent facendo clic con il pulsante destro del mouse su di esso in Esplora oggetti e selezionando **Avvia**. Se con questa operazione l'agente non si avvia, è necessario avviarlo manualmente da Gestione configurazione SQL Server.  
3. Espandere la cartella **Replica**, fare clic con il pulsante destro del mouse su **Pubblicazioni locali** e quindi scegliere **Nuova Pubblicazione**. Verrà avviata la Creazione guidata nuova pubblicazione:  

   ![Selezioni per avviare la Creazione guidata nuova pubblicazione](media/tutorial-replicating-data-between-continuously-connected-servers/newpublication.png)
  
3. Nella pagina **Database di pubblicazione** selezionare [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] e quindi **Avanti**. 

      
4. Nella pagina **Tipo di pubblicazione** selezionare **Pubblicazione di tipo merge** e quindi **Avanti**.  
   
5. Nella pagina **Tipi di Sottoscrittore** verificare che sia selezionato solo [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o versione successiva e quindi selezionare **Avanti**: 

    ![Pagine "Tipo di pubblicazione" e "Tipi di Sottoscrittore"](media/tutorial-replicating-data-with-mobile-clients/mergerpl.png)
  
   
6. Nella pagina **Articoli** espandere il nodo **Tabelle**. Selezionare le tre tabelle seguenti: **Employee**, **SalesOrderHeader** e **SalesOrderDetail**. Selezionare **Avanti**.  

   ![Selezioni delle tabelle nella pagina "Articoli"](media/tutorial-replicating-data-with-mobile-clients/mergearticles.png)

   >[!NOTE]
   > La tabella **Employee** contiene una colonna (**OrganizationNode**) con tipo di dati **hierarchyid**. Questo tipo di dati è supportato per la replica solo in SQL Server 2017. 
   >
   > Se si usa una build precedente a SQL Server 2017, nella parte inferiore della schermata viene visualizzato un messaggio che informa della possibilità di perdere dati se si usa questa colonna nella replica bidirezionale. Ai fini di questa esercitazione, è possibile ignorare questo messaggio. Questo tipo di dati, tuttavia, deve essere replicato in un ambiente di produzione solo se si usa la build supportata.
   > 
   > Per altre informazioni sulla replica del tipo di dati **hierarchyid**, vedere [Uso di colonne hierarchyid nella replica](../../t-sql/data-types/hierarchyid-data-type-method-reference.md#using-hierarchyid-columns-in-replicated-tables).
    
  
7. Nella pagina **Filtro righe tabella** selezionare **Aggiungi** e quindi **Aggiungi filtro**.  
  
8. Nella finestra di dialogo **Aggiungi filtro** selezionare **Employee (HumanResources)** in **Selezionare la tabella da filtrare**. Selezionare la colonna **LoginID**, selezionare la freccia a destra per aggiungere la colonna alla clausola WHERE della query di filtro e modificare la clausola WHERE come segue:  
  
   ```sql 
    WHERE [LoginID] = HOST_NAME()  
   ```  
  
   Selezionare **Una riga di questa tabella verrà inviata a una sola sottoscrizione** e quindi **OK**.  
 
   ![Selezioni per aggiungere un filtro](media/tutorial-replicating-data-with-mobile-clients/mergeaddfilter.png)

    
  
10. Nella pagina **Filtro righe tabella** selezionare **Employee (Human Resources)** , selezionare **Aggiungi** e quindi **Aggiungi join per estendere il filtro selezionato**.  
  
    a. Nella finestra di dialogo **Aggiungi join** selezionare **Sales.SalesOrderHeader** in **Tabella unita in join**. Selezionare **L'istruzione per il join verrà scritta manualmente** e completare l'istruzione per il join come indicato di seguito:  
  
    ```sql  
    ON [Employee].[BusinessEntityID] =  [SalesOrderHeader].[SalesPersonID] 
    ```  
  
    b. In **Specificare le opzioni del join** selezionare **Chiave univoca** e quindi selezionare **OK**.

    ![Selezioni per aggiungere un join al filtro](media/tutorial-replicating-data-with-mobile-clients/mergeaddjoin.png)

  
13. Nella pagina **Filtro righe tabella** selezionare **SalesOrderHeader**, selezionare **Aggiungi** e quindi **Aggiungi join per estendere il filtro selezionato**.  
  
    a. Nella finestra di dialogo **Aggiungi join** selezionare **Sales.SalesOrderDetail** in **Tabella unita in join**.    
    b. Selezionare **Per creare l'istruzione verrà utilizzato il compilatore**.  
    c. Nella casella **Anteprima** verificare che l'istruzione join sia come segue:  
  
    ```sql  
    ON [SalesOrderHeader].[SalesOrderID] = [SalesOrderDetail].[SalesOrderID] 
    ```  
  
    d. In **Specificare le opzioni del join** selezionare **Chiave univoca** e quindi selezionare **OK**. Selezionare **Avanti**. 

    ![Selezioni per aggiungere un altro join per gli ordini di vendita](media/tutorial-replicating-data-with-mobile-clients/joinsalestables.png)
  
21. Selezionare **Crea snapshot immediatamente**, deselezionare **Usa la pianificazione seguente per l'esecuzione dell'agente snapshot** e quindi selezionare **Avanti**:  

    ![Selezione per creare uno snapshot immediatamente](media/tutorial-replicating-data-with-mobile-clients/snapshotagent.png)
  
22. Nella pagina **Sicurezza agente** selezionare **Impostazioni di sicurezza**. Immettere <*Nome_server_pubblicazione*> **\repl_snapshot** nella casella **Account processo**, specificare la password per l'account e quindi selezionare **OK**. Selezionare **Avanti**.  

    ![Selezioni per impostare la sicurezza dell'agente di snapshot](media/tutorial-replicating-data-with-mobile-clients/snapshotagentsecurity.png)
  
23. Nella pagina **Completamento procedura guidata** immettere **AdvWorksSalesOrdersMerge** nella casella **Nome pubblicazione** e selezionare **Fine**:  

    ![Pagina "Completamento procedura guidata" con il nome della pubblicazione](media/tutorial-replicating-data-with-mobile-clients/namemergerepl.png)
  
24. Dopo la creazione della pubblicazione, selezionare **Chiudi**. Nel nodo **Replica** in **Esplora oggetti** fare clic con il pulsante destro del mouse su **Pubblicazioni locali** e selezionare **Aggiorna** per visualizzare la nuova replica di tipo merge.  
  
### <a name="view-the-status-of-snapshot-generation"></a>Visualizzare lo stato della generazione dello snapshot  
  
1. Connettersi al server di pubblicazione in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], espandere il nodo del server e quindi la cartella **Replica**.  
  
2. Nella cartella **Pubblicazioni locali** fare clic con il pulsante destro del mouse su **AdvWorksSalesOrdersMerge** e quindi selezionare **Visualizza stato agente snapshot**:  

   ![Selezioni per visualizzare lo stato dell'agente di snapshot](media/tutorial-replicating-data-with-mobile-clients/viewsnapshotagentstatus.png)
  
3. Verrà visualizzato lo stato corrente del processo dell'agente di snapshot per la pubblicazione. Verificare che il processo snapshot abbia avuto esito positivo prima di passare alla lezione successiva.  
  
### <a name="add-the-merge-agent-login-to-the-pal"></a>Aggiungere l'account di accesso dell'agente di merge all'elenco di accesso alla pubblicazione  
  
1. Connettersi al server di pubblicazione in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], espandere il nodo del server e quindi la cartella **Replica**.  
  
2. Nella cartella **Pubblicazioni locali** fare clic con il pulsante destro del mouse su **AdvWorksSalesOrdersMerge** e quindi selezionare **Proprietà**.  
  
   a. Selezionare la pagina **Elenco di accesso alla pubblicazione** e selezionare **Aggiungi**. 
  
   b. Nella finestra di dialogo **Aggiungi accesso alla pubblicazione** selezionare <*Nome_server_pubblicazione*> **\repl_merge** e selezionare **OK**. Selezionare di nuovo **OK**. 

   ![Selezioni per aggiungere l'account di accesso dell'agente di merge](media/tutorial-replicating-data-with-mobile-clients/mergepal.png) 

  
Per altre informazioni, vedere:  
- [Filtrare i dati pubblicati](../../relational-databases/replication/publish/filter-published-data.md) 
- [Filtri di riga con parametri](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)  
- [Definire un articolo](../../relational-databases/replication/publish/define-an-article.md)  
  
  
## <a name="create-a-subscription-to-the-merge-publication"></a>Creare una sottoscrizione alla pubblicazione di tipo merge
In questa sezione si aggiungerà una sottoscrizione alla pubblicazione di tipo merge creata in precedenza. Questa esercitazione usa il sottoscrittore remoto (NODE2\SQL2016). Verranno quindi impostate le autorizzazioni per il database di sottoscrizione e verrà generato manualmente lo snapshot dei dati filtrati per la nuova sottoscrizione.   
  
### <a name="add-a-subscriber-for-merge-publication"></a>Aggiungere un sottoscrittore per la pubblicazione di tipo merge
  
1. Connettersi al sottoscrittore in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ed espandere il nodo server. Espandere la cartella **Replica**, fare clic con il pulsante destro del mouse sulla cartella **Sottoscrizioni locali** e quindi selezionare **Nuove Sottoscrizioni**. Verrà avviata la Creazione guidata nuova sottoscrizione:

   ![Selezioni per avviare la Creazione guidata nuova sottoscrizione](media/tutorial-replicating-data-with-mobile-clients/newsub.png)
  
2. Nella pagina **Pubblicazione** selezionare **Trova server di pubblicazione SQL Server** nell'elenco **Server di pubblicazione**.  
  
   Nella finestra di dialogo **Connetti al server** immettere il nome dell'istanza del server di pubblicazione nella casella **Nome server** e selezionare **Connetti**. 

   ![Selezioni per aggiungere un server di pubblicazione](media/tutorial-replicating-data-with-mobile-clients/publication.png)
  
4. Selezionare **AdvWorksSalesOrdersMerge** e quindi **Avanti**.  
  
5. Nella pagina **Posizione in cui eseguire l'agente di merge** selezionare **Esegui ogni agente nel relativo Sottoscrittore** e quindi selezionare **Avanti**:  

   ![Opzione "Esegui ogni agente nel relativo Sottoscrittore"](media/tutorial-replicating-data-with-mobile-clients/pullsub.png)
  
6. Nella pagina **Sottoscrittori** selezionare il nome di istanza del server sottoscrittore. In **Database di sottoscrizione** selezionare **Nuovo database** dall'elenco.  
  
   Nella finestra di dialogo **Nuovo database** immettere **SalesOrdersReplica** nella casella **Nome database**. Selezionare **OK**, quindi **Avanti**. 

   ![Selezioni per aggiungere un database al sottoscrittore](media/tutorial-replicating-data-with-mobile-clients/addsubdb.png)
  
8. Nella pagina **Sicurezza agente di merge** selezionare il pulsante con i tre puntini ( **…** ). Immettere <*Nome_computer_sottoscrittore*> **\repl_merge** nella casella **Account processo** e specificare la password per questo account. Selezionare **OK**, **Avanti** e quindi di nuovo **Avanti**.  

   ![Selezioni per la sicurezza dell'agente di merge](media/tutorial-replicating-data-with-mobile-clients/mergeagentsecurity.png)

9. Nella pagina **Pianificazione della sincronizzazione** impostare **Pianificazione agente** su **Esecuzione solo su richiesta**. Selezionare **Avanti**.  

   ![Selezione "Esecuzione solo su richiesta" per l'agente](media/tutorial-replicating-data-with-mobile-clients/mergesyncschedule.png)
  
9. Nella pagina **Inizializzazione sottoscrizioni** selezionare **Alla prima sincronizzazione** nell'elenco **Quando**. Selezionare **Avanti** per passare alla pagina **Tipo di sottoscrizione** e selezionare il tipo di sottoscrizione appropriato. In questa esercitazione viene usato **Client**. Dopo aver selezionato il tipo di sottoscrizione, selezionare di nuovo **Avanti**. 

   ![Selezioni per inizializzare le sottoscrizioni alla prima sincronizzazione](media/tutorial-replicating-data-with-mobile-clients/firstsync.png)

10. Nella pagina **Valori HOST_NAME** immettere il valore **adventure-works\pamela0** nella casella **Valore HOST_NAME**. Quindi selezionare **Fine**.  

    ![Pagina "Valori HOST_NAME"](media/tutorial-replicating-data-with-mobile-clients/hostname.png)
  
11. Selezionare di nuovo **Fine**. Dopo la creazione della sottoscrizione, selezionare **Chiudi**.  

### <a name="set-server-permissions-at-the-subscriber"></a>Impostare le autorizzazioni per il server nel sottoscrittore  
  
1. Connettersi al sottoscrittore in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Espandere la cartella **Sicurezza**, fare clic con il pulsante destro del mouse su **Account di accesso** e quindi scegliere **Nuovo account accesso**.  
  
   Nella pagina **Generale** selezionare **Cerca** e quindi immettere <*Nome_ computer_sottoscrittore*> **\repl_merge** nella casella **Immettere il nome dell'oggetto**. Selezionare **Verifica nomi** e quindi selezionare **OK**. 
    
   ![Selezioni per impostare l'account di accesso](media/tutorial-replicating-data-with-mobile-clients/sublogin.png)
  
1. Nella pagina **Mapping utenti** selezionare il database **SalesOrdersReplica** e selezionare il ruolo **db_owner**. Nella pagina **Entità a protezione diretta** concedere l'autorizzazione **Esplicita** a **Modifica traccia**. Selezionare **OK**.

   ![Pagine "Mapping utente" ed "Entità a protezione diretta"](media/tutorial-replicating-data-with-mobile-clients/setdbo.png)
  
### <a name="create-the-filtered-data-snapshot-for-the-subscription"></a>Creare lo snapshot dei dati filtrati per la sottoscrizione  
  
1. Connettersi al server di pubblicazione in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], espandere il nodo del server e quindi la cartella **Replica**.  
  
2. Nella cartella **Pubblicazioni locali** fare clic con il pulsante destro del mouse sulla pubblicazione **AdvWorksSalesOrdersMerge** e quindi selezionare **Proprietà**.  
   
   a. Selezionare la pagina **Partizioni dati** e selezionare **Aggiungi**.   
   b. Nella finestra di dialogo **Aggiungi partizione dati** immettere **adventure-works\pamela0** nella casella **Valore HOST_NAME** e quindi selezionare **OK**.  
   c. Selezionare la partizione appena aggiunta, selezionare **Genera gli snapshot selezionati adesso** e quindi **OK**. 

   ![Selezioni per aggiungere una partizione](media/tutorial-replicating-data-with-mobile-clients/partition.png)
  
  
Per altre informazioni, vedere:  
- [Sottoscrivere le pubblicazioni](../../relational-databases/replication/subscribe-to-publications.md)  
- [Creare una sottoscrizione pull](../../relational-databases/replication/create-a-pull-subscription.md)  
- [Snapshot per pubblicazioni di tipo merge con filtri con parametri](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)  

## <a name="synchronize-the-subscription-to-the-merge-publication"></a>Sincronizzare la sottoscrizione con la pubblicazione di tipo merge

In questa sezione verrà inizializzata la sottoscrizione avviando l'agente di merge tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. È inoltre necessario eseguire questa procedura per la sincronizzazione con il server di pubblicazione.   
  
### <a name="start-synchronization-and-initialize-the-subscription"></a>Avviare la sincronizzazione e inizializzare la sottoscrizione  
  
1. Connettersi al sottoscrittore in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
2. Verificare che SQL Server Agent sia in esecuzione. In caso negativo, fare clic con il pulsante destro del mouse su SQL Server Agent in Esplora oggetti e selezionare **Avvia**. Se con questa operazione l'agente non si avvia, è necessario avviarlo manualmente da Gestione configurazione SQL Server. 
  
2. Espandere il nodo **Replica**. Nella cartella **Sottoscrizioni locali** fare clic con il pulsante destro del mouse sulla sottoscrizione nel database **SalesOrdersReplica** e quindi selezionare **Visualizza stato sincronizzazione**.  
  
   Selezionare **Avvia** per inizializzare la sottoscrizione. 

   ![Stato di sincronizzazione con pulsante "Avvia"](media/tutorial-replicating-data-with-mobile-clients/mergesyncstatus.png)
    
  
  
## <a name="next-steps"></a>Passaggi successivi  
È stata eseguita la configurazione sia del server di pubblicazione che del sottoscrittore per la replica di tipo merge. È anche possibile:

1. Inserire, aggiornare o eliminare i dati nella tabella **SalesOrderHeader** o **SalesOrderDetail** nel server di pubblicazione o nel sottoscrittore.
2. Ripetere questa procedura quando è disponibile la connettività di rete per sincronizzare i dati tra il server di pubblicazione e il sottoscrittore.
3. Eseguire una query nella tabella **SalesOrderHeader** o **SalesOrderDetail** dall'altro server per visualizzare le modifiche replicate.  
  
Per altre informazioni, vedere:   
- [Inizializzare una sottoscrizione con uno snapshot](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)  
- [Sincronizzare i dati](../../relational-databases/replication/synchronize-data.md)  
- [Sincronizzare una sottoscrizione pull](../../relational-databases/replication/synchronize-a-pull-subscription.md)  
  
  
  
