---
title: 'Esercitazione: Preparare per la replica'
description: In questa esercitazione si apprenderà come preparare il server di pubblicazione, il server di distribuzione e il sottoscrittore per la replica creando account di Windows, preparando la cartella snapshot e configurando la distribuzione.
ms.custom: seo-lt-2019
ms.date: 04/02/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: quickstart
helpviewer_keywords:
- replication [SQL Server], tutorials
ms.assetid: ce30a095-2975-4387-9377-94a461ac78ee
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7506b3b57d81187269c26fae3cafc2d3cdbc6628
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348625"
---
# <a name="tutorial-prepare-sql-server-for-replication-publisher-distributor-subscriber"></a>Esercitazione: Preparare SQL Server per la replica: server di pubblicazione, server di distribuzione, sottoscrittore
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
È importante pianificare la sicurezza prima di configurare la topologia di replica. In questa esercitazione viene illustrato come proteggere in modo più sicuro una topologia di replica. Viene anche illustrato come configurare la distribuzione, il primo passaggio della replica dei dati. È necessario completare questa esercitazione prima delle altre esercitazioni sulla replica.  
  
> [!NOTE]  
> Per eseguire la replica dei dati tra server in modo protetto, è consigliabile seguire tutte le indicazioni riportate in [Procedure consigliate per la sicurezza della replica](../../relational-databases/replication/security/replication-security-best-practices.md).  
  
## <a name="what-you-will-learn"></a>Lezioni dell'esercitazione  
Questa esercitazione illustra come preparare un server in modo che la replica possa essere eseguita in modo protetto con privilegi minimi.  

In questa esercitazione verranno illustrate le procedure per:
> [!div class="checklist"]
> * Creare account di Windows per la replica.
> * Preparare la cartella per lo snapshot.
> * Configurare la distribuzione.

## <a name="prerequisites"></a>Prerequisiti
Questa esercitazione è destinata agli utenti che hanno familiarità con le operazioni fondamentali relative ai database ma con un'esperienza limitata delle operazioni di replica. 

Per completare questa esercitazione, sono necessari SQL Server, SQL Server Management Studio (SSMS) e un database AdventureWorks:  
  
- Nel server di pubblicazione (origine) installare:  
  
   - Una qualsiasi edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad eccezione di SQL Server Express e di SQL Server Compact. Questa edizioni non possono fungere da server di pubblicazione per la replica.   
   - Database di esempio [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] . Per una maggiore sicurezza, i database di esempio non vengono installati per impostazione predefinita.  
  
- Nel server sottoscrittore (destinazione) installare qualsiasi edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ad eccezione di [!INCLUDE[ssEW](../../includes/ssew-md.md)]. [!INCLUDE[ssEW](../../includes/ssew-md.md)] non può essere un sottoscrittore nella replica transazionale.  
  
- Installare [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md).
- Installare [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Scaricare il [database di esempio AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases). Per istruzioni sul ripristino di un database in SSMS, vedere [Ripristino di un database](../backup-restore/restore-a-database-backup-using-ssms.md). 
    
>[!NOTE]
> - La replica non è supportata nelle istanze di SQL Server con versioni la cui distanza sia maggiore di 2. Per altre informazioni, vedere [Supported SQL Server Versions in Replication Topology](replication-backward-compatibility.md) (Versioni di SQL Server supportate nella topologia di replica).
> - In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] è necessario connettersi al server di pubblicazione e al sottoscrittore usando un account di accesso che sia membro del ruolo predefinito del server **sysadmin**. Per altre informazioni su questo ruolo, vedere [Ruoli a livello di server](../security/authentication-access/server-level-roles.md).  


**Tempo stimato per il completamento dell'esercitazione: 30 minuti**
  
## <a name="create-windows-accounts-for-replication"></a>Creare account di Windows per la replica
In questa sezione vengono creati gli account di Windows per l'esecuzione degli agenti di replica. Verrà creato un account di Windows separato nel server locale per gli agenti seguenti:  
  
|Agente|Location|Nome account|  
|---------|------------|----------------|  
|agente snapshot|Editore|<*nome_computer*>\repl_snapshot|  
|Agente di lettura log|Editore|<*nome_computer*>\repl_logreader|  
|Agente di distribuzione|Server di pubblicazione e sottoscrittore|<*nome_computer*>\repl_distribution|  
|Agente di merge|Server di pubblicazione e sottoscrittore|<*nome_computer*>\repl_merge|  
  
> [!NOTE]  
> Nelle esercitazioni sulla replica il server di pubblicazione e il server di distribuzione condividono la stessa istanza (NODE1\SQL2016) di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'istanza del sottoscrittore (NODE2\SQL2016) è remota. Il server di pubblicazione e il sottoscrittore possono usare la stessa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ma questo non è un requisito. Se il server di pubblicazione e il sottoscrittore condividono la stessa istanza, i passaggi seguiti per creare gli account nel sottoscrittore non sono obbligatori.  

### <a name="create-local-windows-accounts-for-replication-agents-at-the-publisher"></a>Creare account di Windows locali per gli agenti di replica nel server di pubblicazione
  
1. Nel server di pubblicazione fare clic su **Gestione computer** in **Strumenti di amministrazione** nel Pannello di controllo.  
  
2. In **Utilità di sistema** espandere **Utenti e gruppi locali**.  
  
3. Fare clic con il pulsante destro del mouse su **Utenti** e quindi selezionare **Nuovo utente**.  
     
4. Immettere **repl_snapshot** nella casella **Nome utente**, specificare la password e altre informazioni pertinenti e quindi selezionare **Crea** per creare l'account repl_snapshot: 

   ![Finestra di dialogo "Nuovo utente"](media/tutorial-preparing-the-server-for-replication/newuser.png)
  
5. Ripetere il passaggio precedente per creare gli account repl_logreader, repl_distribution e repl_merge:  
 
   ![Elenco degli utenti di replica](media/tutorial-preparing-the-server-for-replication/replusers.png)
  
6. Selezionare **Chiudi**.  
  
### <a name="create-local-windows-accounts-for-replication-agents-at-the-subscriber"></a>Creare account di Windows locali per gli agenti di replica nel sottoscrittore  
  
1. Nel sottoscrittore aprire **Gestione computer** in **Strumenti di amministrazione** nel Pannello di controllo.  
  
2. In **Utilità di sistema** espandere **Utenti e gruppi locali**.  
  
3. Fare clic con il pulsante destro del mouse su **Utenti** e quindi selezionare **Nuovo utente**.  
  
4. Immettere **repl_distribution** nella casella **Nome utente**, specificare la password e altre informazioni pertinenti e quindi selezionare **Crea** per creare l'account repl_distribution.  
  
5. Ripetere il passaggio precedente per creare l'account repl_merge.  
  
6. Selezionare **Chiudi**.  

Per altre informazioni, vedere [Panoramica degli agenti di replica](../../relational-databases/replication/agents/replication-agents-overview.md).  

## <a name="prepare-the-snapshot-folder"></a>Preparare la cartella per lo snapshot
In questa sezione viene configurata la cartella usata per la creazione e l'archiviazione dello snapshot di pubblicazione. 

### <a name="create-a-share-for-the-snapshot-folder-and-assign-permissions"></a>Creare una condivisione per la cartella per lo snapshot e assegnare le autorizzazioni  
  
1. In Esplora file individuare la cartella Data di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il percorso predefinito è C:\Programmi\Microsoft SQL Server\MSSQL.X\MSSQL\Data.  
  
2. Creare una nuova cartella denominata **repldata**.  
  
3. Fare clic con il pulsante destro del mouse su questa cartella e selezionare **Proprietà**.  
  
   a. Nella scheda **Condivisione** della finestra di dialogo **Proprietà di repldata** selezionare **Condivisione avanzata**.  
  
   b. Nella finestra di dialogo **Condivisione avanzata** selezionare **Condividi cartella** e quindi selezionare **Autorizzazioni**.  

   ![Selezioni per la condivisione della cartella repldata](media/tutorial-preparing-the-server-for-replication/repldata.png)

6. Nella finestra di dialogo **Autorizzazioni per repldata** selezionare **Aggiungi**. Nella casella **Select User, Computers, Service Account, or Groups** (Seleziona utenti, computer, account del servizio o gruppi) digitare il nome dell'account dell'agente di snapshot creato in precedenza nel formato <*Nome_server_pubblicazione*> **\repl_snapshot**. Selezionare **Verifica nomi** e quindi selezionare **OK**.  

   ![Selezioni per aggiungere autorizzazioni di condivisione](media/tutorial-preparing-the-server-for-replication/addshareperms.png)

7. Ripetere il passaggio 6 per aggiungere gli altri due account creati in precedenza: <*Nome_server_pubblicazione*> **\repl_merge** e <*Nome_server_pubblicazione*> **\repl_distribution**.

8. Dopo aver aggiunto i tre account, assegnare le autorizzazioni seguenti:      
   - repl_distribution: **Lettura**  
   - repl_merge: **Lettura**  
   - repl_snapshot: **Controllo completo**    

   ![Autorizzazioni condivise per ogni account](media/tutorial-preparing-the-server-for-replication/sharedpermissions.png)

9. Dopo aver configurato correttamente le autorizzazioni di condivisione, selezionare **OK** per chiudere la finestra di dialogo **Autorizzazioni per repldata**. Selezionare **OK** per chiudere la finestra di dialogo **Condivisione avanzata**. 

10. Nella finestra di dialogo **Proprietà repldata** selezionare la scheda **Sicurezza** e selezionare **Modifica**:  

    ![Pulsante "Modifica" nella scheda "Sicurezza"](media/tutorial-preparing-the-server-for-replication/editsecurity.png)   

11. Nella finestra di dialogo **Autorizzazioni per repldata** selezionare **Aggiungi**. Nella casella **Select Users, Computers, Service Accounts, or Groups** (Seleziona utenti, computer, account del servizio o gruppi) digitare il nome dell'account dell'agente di snapshot creato in precedenza nel formato <*Nome_server_pubblicazione*> **\repl_snapshot**. Selezionare **Verifica nomi** e quindi selezionare **OK**.  

    ![Selezioni per aggiungere autorizzazioni di sicurezza](media/tutorial-preparing-the-server-for-replication/addsecuritypermissions.png)

  
12. Ripetere il passaggio precedente per aggiungere le autorizzazioni per l'agente di distribuzione nel formato <*Nome_server_pubblicazione*> **\repl_distribution** e per l'agente di merge nel formato <*Nome_server_pubblicazione*> **\repl_merge**.  
    
  
13. Verificare che siano state concesse le autorizzazioni seguenti:  
  
    - repl_distribution: **Lettura**
    - repl_merge: **Lettura**
    - repl_snapshot: **Controllo completo**   
 
    ![Autorizzazioni utente per i dati di replica](media/tutorial-preparing-the-server-for-replication/replpermissions.png) 

14. Selezionare di nuovo la scheda **Condivisione** e prendere nota del **percorso di rete** per la condivisione. Questo percorso sarà necessario in seguito, al momento di configurare la cartella per lo snapshot.  

    ![Percorso di rete nella scheda "Condivisione"](media/tutorial-replicating-data-between-continuously-connected-servers/networkpath.png)

15. Selezionare **OK** per chiudere la finestra di dialogo **Proprietà di repldata**. 
 
Per altre informazioni, vedere [Proteggere la cartella snapshot](../../relational-databases/replication/security/secure-the-snapshot-folder.md).  
  

## <a name="configure-distribution"></a>Configurare la distribuzione
In questa sezione viene configurata la distribuzione nel server di pubblicazione e vengono impostate le autorizzazioni necessarie per i database di pubblicazione e di distribuzione. Se il server di distribuzione è già stato configurato, è necessario disabilitare la pubblicazione e la distribuzione prima di iniziare questa sezione. Non eseguire questa operazione se è necessario mantenere la topologia di replica esistente, soprattutto in un ambiente di produzione.   
  
In questa esercitazione non è prevista la configurazione del server di pubblicazione con un server di distribuzione remoto.  

### <a name="configure-distribution-at-the-publisher"></a>Configurare la distribuzione nel server di pubblicazione  
  
1. Connettersi al server di pubblicazione in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] e quindi espandere il nodo server.  
  
2. Fare clic con il pulsante destro del mouse sulla cartella **Replica** e selezionare **Configura distribuzione**:  

   ![Comando "Configura distribuzione" nel menu di scelta rapida](media/tutorial-preparing-the-server-for-replication/configuredistribution.png)
  
   > [!NOTE]  
   > - Se è stata effettuata la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando **localhost** anziché il nome effettivo del server, verrà visualizzato un avviso che indica l'impossibilità di stabilire una connessione tra [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e **localhost o indirizzo IP**. Selezionare **OK** nella finestra di dialogo dell'avviso. Nella finestra di dialogo **Connetti al server** modificare **Nome server** sostituendo **localhost o indirizzo IP** con il nome del server. Selezionare **Connetti**.  
   > - Attualmente esiste un problema noto relativo a SQL Server Management Studio (SSMS) 18.0 (e versioni successive) per cui _non_ viene visualizzato un messaggio di avviso quando ci si connette al server di distribuzione con l'indirizzo IP, ma questo non è ancora valido. Per la connessione al database di distribuzione deve essere usato il nome effettivo del server. 
   
   Verrà avviata la Configurazione guidata distribuzione.  
  
3. Nella pagina **Server di distribuzione** selezionare < *'NomeServer'* >  **fungerà da database di distribuzione per se stesso. Verranno creati un database di distribuzione e un log**. Fare quindi clic su **Avanti**.  

   ![Opzione perché il server funga da server di distribuzione per se stesso](media/tutorial-preparing-the-server-for-replication/serverdistributor.png)
  
4. Se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent non è in esecuzione, nella pagina **Avvio Agent** di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] selezionare **Configura il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent per l'avvio automatico**. Selezionare **Avanti**.  

     
5. Immettere il percorso \\\\<*Nome_server_pubblicazione*> **\repldata** nella casella di testo **Cartella snapshot** e quindi selezionare **Avanti**. Questo percorso deve corrispondere a quanto visto in precedenza in **Percorso di rete** nella cartella delle proprietà di repldata dopo la configurazione delle proprietà della condivisione. 

   ![Confronto tra i percorsi di rete nella finestra di dialogo "Proprietà repldata" e nella Configurazione guidata distribuzione](media/tutorial-preparing-the-server-for-replication/repldatasnapshot.png)
  
6. Accettare i valori predefiniti nella pagine seguenti della procedura guidata.  
    
   ![Ultima pagina della procedura guidata](media/tutorial-preparing-the-server-for-replication/distributionwizarddefaults.png)
  
7. Selezionare **Fine** per abilitare la distribuzione. 

Durante la configurazione del server di distribuzione può essere visualizzato l'errore seguente, che indica che l'account usato per avviare l'account di SQL Server Agent non è un account amministratore del sistema. È necessario avviare SQL Server Agent manualmente, concedere le autorizzazioni necessarie all'account esistente o cambiare l'account usato da SQL Server Agent. 

![Messaggio di errore per la configurazione di SQL Server Agent](media/tutorial-preparing-the-server-for-replication/startingagenterror.png)

Se l'istanza di SQL Server Management Studio è in esecuzione con diritti amministrativi, è possibile avviare SQL Server Agent manualmente dall'interno di SSMS:  
    
![Selezione di "Avvia" nel menu di scelta rapida per l'agente in SSMS](media/tutorial-preparing-the-server-for-replication/ssmsstartagent.png) 

>[!NOTE]
> Se SQL Server Agent non sembra avviarsi, fare clic con il pulsante destro del mouse su SQL Server Agent in SSMS e quindi scegliere **Aggiorna**. Se è ancora arrestato, avviarlo manualmente da Gestione configurazione SQL Server.    
  
## <a name="set-database-permissions"></a>Impostare le autorizzazioni per il database  
  
1. In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] espandere la cartella **Sicurezza**, fare clic con il pulsante destro del mouse su **Account di accesso** e quindi scegliere **Nuovo account accesso**:  

   ![Comando "Nuovo account accesso" nel menu di scelta rapida](media/tutorial-preparing-the-server-for-replication/newlogin.png)
  
2. Nella pagina **Generale** selezionare **Cerca**. Immettere <*Nome_server_pubblicazione*> **\repl_snapshot** nella casella **Immettere il nome dell'oggetto da selezionare**, selezionare **Verifica nomi** e quindi selezionare **OK**.  

   ![Selezioni per l'immissione del nome dell'oggetto](media/tutorial-preparing-the-server-for-replication/addsnapshotlogin.png)
  
3. Nell'elenco **Utenti con mapping a questo account di accesso** nella pagina **Mapping utente** selezionare sia il database **di distribuzione** che il database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
   Nell'elenco delle appartenenze a ruoli del database selezionare il ruolo **db_owner** per l'accesso a entrambi i database.  

   ![Selezione dei database e del loro ruolo](media/tutorial-preparing-the-server-for-replication/dbowner.png)
  
4. Selezionare **OK** per creare l'account di accesso.  
  
5. Ripetere i passaggi 1-4 per creare un account di accesso per gli altri account locali (repl_distribution, repl_logreader e repl_merge). È necessario eseguire il mapping di questi account anche agli utenti membri del ruolo predefinito del database **db_owner** nel database di **distribuzione** e nel database **AdventureWorks**.  

   ![Visualizzazione di tutti i quattro account in Esplora oggetti](media/tutorial-preparing-the-server-for-replication/usersinssms.png)
   
  
Per altre informazioni, vedere:
- [Configurare la distribuzione](../../relational-databases/replication/configure-distribution.md) 
- [Modello di sicurezza dell'agente di replica](../../relational-databases/replication/security/replication-agent-security-model.md)  

## <a name="next-steps"></a>Passaggi successivi
La preparazione del server per la replica è stata completata. Il prossimo articolo insegna come configurare la replica transazionale: 

> [!div class="nextstepaction"]
> [Esercitazione: Configurare la replica tra due server sempre connessi (replica transazionale)](tutorial-replicating-data-between-continuously-connected-servers.md)

  
