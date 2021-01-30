---
description: Creazione di una pubblicazione da un database Oracle
title: Creare una pubblicazione da un database Oracle | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- publications [SQL Server replication], Oracle databases
- Oracle publishing [SQL Server replication], configuring
ms.assetid: b3812746-14b0-4b22-809e-b4a95e1c8083
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: cc7660d1990f556ae3ccfb8d34d66519dd2ecd76
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076779"
---
# <a name="create-a-publication-from-an-oracle-database"></a>Creazione di una pubblicazione da un database Oracle
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come creare una pubblicazione da un database Oracle in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Prerequisiti](#Prerequisites)  
  
-   **Per creare una pubblicazione da un database Oracle, utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
  
-   Prima di creare una pubblicazione, è necessario installare il software Oracle nel server di distribuzione [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e configurare il database Oracle. Per altre informazioni, vedere [Configurare un server di pubblicazione Oracle](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 Creare una pubblicazione snapshot o transazionale da un database Oracle con la Creazione guidata nuova pubblicazione.  
  
 La prima volta che si crea una pubblicazione da un database Oracle, è necessario identificare il server di pubblicazione Oracle nel database di distribuzione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Non è necessario ripetere l'operazione per le pubblicazioni successive provenienti dallo stesso database. L'identificazione del server di pubblicazione Oracle può essere effettuata tramite la Creazione guidata nuova pubblicazione o nella finestra di dialogo **Proprietà database di distribuzione - \<Distributor>** . In questo argomento viene illustrata la finestra di dialogo **Proprietà database di distribuzione - \<Distributor>** .  
  
#### <a name="to-identify-the-oracle-publisher-at-the-sql-server-distributor"></a>Per identificare il server di pubblicazione Oracle nel server di distribuzione SQL Server  
  
1.  In [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]connettersi all'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che verrà utilizzata dal server di pubblicazione Oracle come server di distribuzione e quindi espandere il nodo del server.  
  
2.  Fare clic con il pulsante destro del mouse sulla cartella **Replica** e quindi scegliere **Proprietà server di distribuzione**.  
  
3.  Nella pagina **Server di pubblicazione** della finestra di dialogo **Proprietà database di distribuzione - \<Distributor>** fare clic su **Aggiungi** e quindi su **Aggiungi server di pubblicazione Oracle**.  
  
4.  Nella finestra di dialogo **Connetti al server** fare clic sul pulsante **Opzioni** .  
  
5.  Nella scheda **Account di accesso** :  
  
    1.  Immettere il nome dell'istanza del database Oracle oppure selezionare **Cerca** nella casella combinata relativa all'**istanza del server**.  
  
    2.  Consente di selezionare l' **Autenticazione standard Oracle** (scelta consigliata) oppure l' **Autenticazione di Windows**.  
  
         Se si seleziona **Autenticazione di Windows**, è necessario che il server Oracle sia configurato per consentire le connessioni utilizzando le credenziali di Windows (per ulteriori informazioni, vedere la documentazione Oracle) ed è inoltre necessario essere connessi con lo stesso account di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows specificato per lo schema utente di amministrazione della replica.  
  
    3.  Se si seleziona **Autenticazione standard Oracle**, immettere il nome dell'account di accesso e la password dello schema utente di amministrazione della replica creato nel server di pubblicazione Oracle durante la configurazione.  
  
6.  Nella scheda **Proprietà connessione** selezionare un tipo di server di pubblicazione scegliendo tra **Gateway** o **Complete**.  
  
     L'opzione **Completa** è progettata per offrire pubblicazioni snapshot e transazionali con il set completo di caratteristiche supportate per la pubblicazione Oracle. L'opzione **Gateway** consente l'ottimizzazione della progettazione specifica per migliorare le prestazioni per i casi in cui la replica funge da gateway tra i sistemi. Non è possibile utilizzare l'opzione **Gateway** se si intende pubblicare la stessa tabella in più pubblicazioni transazionali. Se si seleziona **Gateway**, una tabella può essere presente al massimo in una pubblicazione transazionale e in un numero qualsiasi di pubblicazioni snapshot.  
  
7.  Fare clic su **Connetti** per creare una connessione al server di pubblicazione Oracle e configurarla per la replica. La finestra di dialogo **Connetti al server** verrà chiusa e verrà visualizzata di nuovo la finestra di dialogo **Proprietà database di distribuzione - \<Distributor>** .  
  
    > [!NOTE]  
    >  Se si verificano problemi con la configurazione di rete, a questo punto verrà visualizzato un errore. Se si verificano problemi durante la connessione al database Oracle, vedere la sezione relativa all'impossibilità di connessione del server di distribuzione SQL Server all'istanza del database Oracle in [Troubleshooting Oracle Publishers](../../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md).  
  
8.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

#### <a name="to-create-a-publication-from-an-oracle-database"></a>Per creare una pubblicazione da un database Oracle  
  
1.  Connettersi all'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che verrà utilizzata dal server di pubblicazione Oracle come server di distribuzione e quindi espandere il nodo del server.  
  
2.  Espandere la cartella **Replica** .  
  
3.  Fare clic con il pulsante destro del mouse sulla cartella **Pubblicazioni locali** e scegliere **Nuova pubblicazione Oracle**.  
  
4.  Nella pagina **Server di pubblicazione Oracle** della Creazione guidata nuova pubblicazione selezionare il server di pubblicazione Oracle. Se il server di pubblicazione Oracle non è disponibile, fare clic su **Aggiungi server di pubblicazione Oracle** per visualizzare i passaggi della procedura precedente.  
  
5.  Nella pagina **Tipo di pubblicazione** selezionare **Pubblicazione snapshot** o **Pubblicazione transazionale**.  
  
6.  Nella pagina **Articoli** selezionare gli oggetti di database che si desidera pubblicare.  
  
     Facoltativamente, scegliere di filtrare le colonne della tabella espandendo una tabella e quindi deselezionando le caselle di controllo per una o più colonne. Fare clic su **Proprietà articolo** per visualizzare e modificare le proprietà dell'articolo e per specificare, se necessario, mapping dei tipi di dati alternativi. Per altre informazioni sui mapping di tipi di dati, vedere [Specificare i mapping tra i tipi di dati di un server di pubblicazione Oracle](../../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md).  
  
7.  Nella pagina **Filtro righe tabella** è possibile applicare i filtri per pubblicare un subset di dati da una o più tabelle.  
  
8.  Nella pagina **Agente snapshot** deselezionare la casella di controllo **Crea snapshot immediatamente** solo se nel database di sottoscrizione sono stati creati tutti gli oggetti e aggiunti tutti i dati necessari.  
  
9. Nella pagina **Sicurezza agente** specificare le credenziali per l'agente snapshot per tutte le pubblicazioni e per l'agente di lettura log per le pubblicazioni transazionali. Gli agenti vengono eseguiti e si connettono al server di distribuzione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilizzando il contesto dell'account di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows specificato. Gli agenti si connettono al database Oracle utilizzando il contesto dell'account specificato come schema utente di amministrazione della replica. Per altre informazioni, vedere [Configurare un server di pubblicazione Oracle](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md).  
  
     Per ulteriori informazioni sulle autorizzazioni richieste per ogni agente, vedere [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md) e [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md).  
  
10. Nella pagina **Azioni procedura guidata** creare, se lo si desidera, uno script per la pubblicazione. Per altre informazioni, vedere [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
11. Nella pagina **Completamento procedura guidata** specificare un nome per la pubblicazione.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 Dopo aver configurato il database Oracle come server di pubblicazione, è possibile creare una pubblicazione transazionale o snapshot in modo analogo a quanto possibile con un server di pubblicazione [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], usando stored procedure di sistema.  
  
#### <a name="to-create-an-oracle-publication"></a>Per creare una pubblicazione Oracle  
  
1.  Configurare il database Oracle come server di pubblicazione. Per altre informazioni, vedere [Configurare un server di pubblicazione Oracle](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md).  
  
2.  Se non esiste un server di distribuzione remoto, configurarlo. Per altre informazioni, vedere [Configure Publishing and Distribution](../../../relational-databases/replication/configure-publishing-and-distribution.md).  
  
3.  Nel server di distribuzione remoto che verrà usato dal server di pubblicazione Oracle, eseguire [sp_adddistpublisher &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md). Specificare il nome TNS (Transparent Network Substrate) dell'istanza del database Oracle per **\@publisher** e il valore **ORACLE** o **ORACLE GATEWAY** per **\@publisher_type**. `Specify` la modalità di sicurezza utilizzata per la connessione dal server di pubblicazione Oracle al database di distribuzione [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] remoto. I possibili valori sono i seguenti:  
  
    -   Per usare l'autenticazione standard di Oracle, che corrisponde all'impostazione predefinita, specificare il valore **0** per **\@security_mode**, l'account di accesso dello schema utente di amministrazione della replica creato nel server di pubblicazione Oracle durante la configurazione per **\@login** e la password per **\@password**.  
  
        > [!IMPORTANT]  
        >  Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se si archiviano le credenziali in un file di script, è necessario proteggere il file per impedire l'accesso non autorizzato.  
  
    -   Per usare l'autenticazione di Windows, specificare il valore **1** per **\@security_mode**.  
  
        > [!NOTE]  
        >  Per utilizzare l'autenticazione di Windows, è necessario che il server Oracle sia configurato per consentire le connessioni utilizzando le credenziali di Windows (per ulteriori informazioni, vedere la documentazione Oracle) ed è inoltre necessario essere connessi con lo stesso account di Microsoft Windows specificato per lo schema utente di amministrazione della replica.  
  
4.  Creare un processo dell'agente di lettura log per il database di pubblicazione.  
  
    -   Per sapere se un processo dell'agente di lettura log esiste per un database pubblicato, eseguire [sp_helplogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helplogreader-agent-transact-sql.md) nel database di distribuzione del server di distribuzione usato dal server di pubblicazione Oracle. Specificare il nome del server di pubblicazione Oracle per **\@publisher**. Se il set di risultati è vuoto, sarà necessario creare un processo dell'agente di lettura log.  
  
    -   Se per il database di pubblicazione esiste già un processo dell'agente di lettura log, procedere con il passaggio 5.  
  
    -   Nel database di distribuzione del server di distribuzione usato dal server di pubblicazione Oracle eseguire [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md). Specificare le credenziali di Windows per l'esecuzione dell'agente nei parametri **\@job_login** e **\@job_password**.  
  
        > [!NOTE]  
        >  Il parametro **\@job_login** deve corrispondere all'account di accesso specificato al passaggio 3. Non specificare informazioni sulla sicurezza del server di pubblicazione. L'agente di lettura log si connette al server di pubblicazione utilizzando le informazioni sulla sicurezza specificate al passaggio 3.  
  
5.  Nel database di distribuzione del server di distribuzione eseguire [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md) per creare la pubblicazione. Per altre informazioni, vedere [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md).  
  
6.  Nel database di distribuzione del server di distribuzione eseguire [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Specificare il nome della pubblicazione usato al passaggio 4 per **\@publication** e le credenziali di Windows usate per l'agente di snapshot per **\@job_name** e **\@password**. Per usare l'autenticazione standard di Oracle per la connessione al server di pubblicazione, è anche necessario specificare il valore **0** per **\@publisher_security_mode** e le informazioni sull'account di accesso di Oracle per **\@publisher_login** e **\@publisher_password**. Verrà creato un processo dell'agente snapshot per la pubblicazione.  
  
## <a name="see-also"></a>Vedere anche  
 [Configurare un server di pubblicazione Oracle](../../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md)   
 [Pubblicare dati e oggetti di database](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Configurare il processo del set di transazioni per un server di pubblicazione Oracle &#40;programmazione Transact-SQL della replica&#41;](../../../relational-databases/replication/administration/configure-the-transaction-set-job-for-an-oracle-publisher.md)   
 [Panoramica della pubblicazione Oracle](../../../relational-databases/replication/non-sql/oracle-publishing-overview.md)   
 [Script per la concessione di autorizzazioni per Oracle](../../../relational-databases/replication/non-sql/script-to-grant-oracle-permissions.md)  
  
  
