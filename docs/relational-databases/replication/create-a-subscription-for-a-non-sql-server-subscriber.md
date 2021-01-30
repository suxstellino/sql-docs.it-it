---
title: Creare una sottoscrizione per un sottoscrittore non SQL
description: Informazioni su come creare una sottoscrizione per un sottoscrittore non SQL Server in SQL Server tramite SQL Server Management Studio (SSMS) o Transact-SQL (T-SQL).
ms.custom: seo-lt-2019
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- subscriptions [SQL Server replication], non-SQL Server Subscribers
- Subscribers [SQL Server replication], non-SQL Server Subscribers
- non-SQL Server Subscribers, subscriptions
ms.assetid: 5020ee68-b988-4d57-8066-67d183e61237
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 36c15a9d7de2c613b3a25b16827bfcc037393524
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076223"
---
# <a name="create-a-subscription-for-a-non-sql-server-subscriber"></a>Creazione di una sottoscrizione per un Sottoscrittore non SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come creare una sottoscrizione per un Sottoscrittore non SQL Server in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. La replica transazionale e la replica snapshot supportano la pubblicazione di dati su sottoscrittori non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per informazioni sulle piattaforme di Sottoscrittori supportate, vedere [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md).  
  
 **Contenuto dell'articolo**  
  
-   **Per creare una sottoscrizione per un Sottoscrittore non SQL Server, utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 Per creare una sottoscrizione per un sottoscrittore non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
1.  Installare e configurare il software client e il provider o i provider OLE DB appropriati sul database di distribuzione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per ulteriori informazioni, vedere [Oracle Subscribers](../../relational-databases/replication/non-sql/oracle-subscribers.md) e [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md).  
  
2.  Creare una pubblicazione utilizzando la Creazione guidata nuova pubblicazione. Per informazioni sulla creazione di pubblicazioni, vedere [Creare una pubblicazione](../../relational-databases/replication/publish/create-a-publication.md) e [Creare una pubblicazione da un database Oracle](../../relational-databases/replication/publish/create-a-publication-from-an-oracle-database.md). Specificare le opzioni seguenti nella Creazione guidata nuova pubblicazione.  
  
    -   Nella pagina **Tipo di pubblicazione** selezionare **Pubblicazione snapshot** o **Pubblicazione transazionale**.  
  
    -   Nella pagina **Agente snapshot** , deselezionare **Crea snapshot immediatamente**.  
  
         Lo snapshot viene creato dopo che la pubblicazione è stata attivata per i Sottoscrittori non[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , in modo da garantire che l'agente snapshot generi uno snapshot e script di inizializzazione che siano adatti ai Sottoscrittori non[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
3.  Abilitare la pubblicazione per i Sottoscrittori non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando la finestra di dialogo **Proprietà pubblicazione - \<PublicationName>** . Per ulteriori informazioni su questo passaggio, vedere [Publication Properties, Subscription Options](../../relational-databases/replication/publication-properties-subscription-options.md) .  
  
4.  Creare una sottoscrizione utilizzando la Creazione guidata nuova sottoscrizione. In questo argomento sono presenti ulteriori informazioni su questo passaggio.  
  
5.  (Facoltativo) Modificare la proprietà di articolo **pre_creation_cmd** affinché le tabelle vengano mantenute nel Sottoscrittore. In questo argomento sono presenti ulteriori informazioni su questo passaggio.  
  
6.  Generare uno snapshot per la pubblicazione. In questo argomento sono presenti ulteriori informazioni su questo passaggio.  
  
7.  Sincronizzare la sottoscrizione. Per altre informazioni, vedere [Sincronizzazione di una sottoscrizione push](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
#### <a name="to-enable-a-publication-for-non-sql-server-subscribers"></a>Per abilitare una pubblicazione per Sottoscrittori non SQL Server  
  
1.  Connettersi al server di pubblicazione in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]e quindi espandere il nodo del server.  
  
2.  Espandere la cartella **Replica** e quindi la cartella **Pubblicazioni locali** .  
  
3.  Fare clic con il pulsante destro del mouse sulla pubblicazione e quindi scegliere **Proprietà**.  
  
4.  Nella pagina **Opzioni sottoscrizione** , selezionare il valore **True** per l'opzione **Consenti Sottoscrittori non SQL Server**. Se si seleziona questa opzione vengono modificate alcune proprietà in modo che la pubblicazione sia compatibile con i Sottoscrittori non[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    > [!NOTE]  
    >  Selezionando **True** , il valore della proprietà dell'articolo **pre_creation_cmd** viene impostato su 'drop'. In base a tale impostazione, se una tabella nel Sottoscrittore corrisponde al nome della tabella nell'articolo, tale tabella verrà eliminata dalla replica. Se si dispone di tabelle esistenti che si desidera mantenere, utilizzare la stored procedure [sp_changearticle](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md) per ogni articolo e specificare il valore 'none' per **pre_creation_cmd**: `sp_changearticle @publication= 'MyPublication', @article= 'MyArticle', @property='pre_creation_cmd', @value='none'`.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)] Verrà richiesto di creare un nuovo snapshot per la pubblicazione. Se non si desidera crearlo in questo momento, utilizzare i passaggi descritti nella "Procedura" seguente in un momento successivo.  
  
#### <a name="to-create-a-subscription-for-a-non-sql-server-subscriber"></a>Per creare una sottoscrizione per un Sottoscrittore non SQL Server  
  
1.  Espandere la cartella **Replica** e quindi la cartella **Pubblicazioni locali** .  
  
2.  Fare clic con il pulsante destro del mouse sulla pubblicazione appropriata e quindi scegliere **Nuove sottoscrizioni**.  
  
3.  Verificare che l'opzione **Esegui tutti gli agenti nel server di distribuzione** nella pagina **Posizione in cui eseguire l'agente di distribuzione** sia selezionata. I Sottoscrittori non[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supportano l'esecuzione di agenti nel Sottoscrittore.  
  
4.  Nella pagina **Sottoscrittori** , fare clic su **Aggiungi Sottoscrittore** , quindi su **Aggiungi Sottoscrittore non SQL Server**.  
  
5.  Nella finestra di dialogo **Aggiungi Sottoscrittore non SQL Server** , selezionare il tipo di Sottoscrittore.  
  
6.  Inserire un valore nella casella **Nome origine dati**:  
  
    -   Per Oracle, si tratta del nome TNS che è stato configurato.  
  
    -   Per IBM, può essere qualsiasi nome. In genere viene utilizzato per specificare l'indirizzo di rete del Sottoscrittore.  
  
     Il nome origine dati immesso in questo passaggio e le credenziali specificate nel passaggio 9 non vengono convalidati da questa procedura guidata. Non vengono utilizzati dalla replica fino all'esecuzione dell'agente di distribuzione per la sottoscrizione. Accertarsi che tutti i valori siano stati verificati connettendosi al Sottoscrittore con uno strumento client come **sqlplus** per Oracle. Per ulteriori informazioni, vedere [Oracle Subscribers](../../relational-databases/replication/non-sql/oracle-subscribers.md) e [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md).  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)] Nella pagina **Sottoscrittori** della procedura guidata, il Sottoscrittore è ora visualizzato nella colonna **Sottoscrittore** con una **(destinazione predefinita)** di sola lettura nella colonna **Database di sottoscrizione** :  
  
    -   Per Oracle, un server include al massimo un database e pertanto non è necessario specificare il database.  
  
    -   Per IBM DB2, il database viene specificato nella proprietà **Catalogo iniziale** della stringa di connessione DB2, che può essere inserita nel campo **Opzioni di connessione aggiuntive** descritto più avanti in questo processo.  
  
8.  Nella pagina **Sicurezza agente di distribuzione** , fare clic sul pulsante delle proprietà ( **...** ) accanto al Sottoscrittore per accedere alla finestra di dialogo **Sicurezza agente di distribuzione** .  
  
9. Nella finestra di dialogo **Sicurezza agente di distribuzione** :  
  
    -   Nei campi **Account processo**, **Password** e **Conferma password** , inserire l'account e la password di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows con cui deve essere eseguito l'agente di distribuzione e stabilire le connessioni locali al server di distribuzione.  
  
         L'account richiede le autorizzazioni minime seguenti: membro del ruolo predefinito **db_owner** del database di distribuzione, membro dell'elenco di accesso alla pubblicazione, autorizzazione di lettura sulla condivisione snapshot e autorizzazione di lettura sulla directory di installazione del provider OLE DB. Per altre informazioni sull'elenco di acceso alla pubblicazione, vedere [Proteggere il server di pubblicazione](../../relational-databases/replication/security/secure-the-publisher.md).  
  
    -   Nei campi **Nome account di accesso**, **Password** e **Conferma password** di **Connessione al Sottoscrittore** , inserire il nome account di accesso e la password da utilizzare per la connessione al Sottoscrittore. Questo nome account di accesso dovrebbe essere già configurato ed avere autorizzazioni sufficienti per la creazione di oggetti nel database di sottoscrizione.  
  
    -   Nel campo **Opzioni di connessione aggiuntive** , specificare le eventuali opzioni di connessione per il Sottoscrittore nel formato di stringa di connessione (Oracle non richiede opzioni aggiuntive). Le opzioni devono essere separate dal punto e virgola. L'esempio seguente illustra una stringa di connessione DB2 (le interruzioni di riga sono inserite per agevolare la lettura):  
  
        ```  
        Provider=DB2OLEDB;Initial Catalog=MY_SUBSCRIBER_DB;Network Transport Library=TCP;Host CCSID=1252;  
        PC Code Page=1252;Network Address=MY_SUBSCRIBER;Network Port=50000;Package Collection=MY_PKGCOL;  
        Default Schema=MY_SCHEMA;Process Binary as Character=False;Units of Work=RUW;DBMS Platform=DB2/NT;  
        Persist Security Info=False;Connection Pooling=True;  
        ```  
  
         La maggior parte delle opzioni nella stringa è specifica per il server DB2 configurato, ma è necessario che l'opzione **Process Binary as Character** sia sempre impostata su **False**. È necessario specificare un valore per l'opzione **Initial Catalog** per identificare il database di sottoscrizione.  
  
10. Nella pagina **Pianificazione della sincronizzazione** , selezionare una pianificazione per l'agente di distribuzione dal menu **Pianificazione agente** . In genere è **Esecuzione continua**.  
  
11. Nella pagina **Inizializzazione sottoscrizioni** , specificare se la sottoscrizione deve essere inizializzata e quando:  
  
    -   Deselezionare **Inizializzazione di** solo se nel database di sottoscrizione sono stati creati tutti gli oggetti e sono stati aggiunti tutti i dati necessari.  
  
    -   Selezionare **Immediatamente** nell'elenco a discesa della colonna **Quando** per ottenere il trasferimento dei file di snapshot da parte dell'agente di distribuzione al Sottoscrittore al termine di questa procedura guidata. Selezionare **Alla prima sincronizzazione** per fare in modo che l'agente trasferisca i file alla successiva esecuzione pianificata dell'agente.  
  
12. Nella pagina **Azioni procedura guidata** , se si desidera, creare lo script della sottoscrizione. Per altre informazioni, vedere [Scripting Replication](../../relational-databases/replication/scripting-replication.md).  
  
#### <a name="to-retain-tables-at-the-subscriber"></a>Per mantenere le tabelle nel Sottoscrittore  
  
-   Per impostazione predefinita, se si attiva una pubblicazione per Sottoscrittori non[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , il valore della proprietà di articolo **pre_creation_cmd** verrà impostato su 'drop'. In base a tale impostazione, se una tabella nel Sottoscrittore corrisponde al nome della tabella nell'articolo, tale tabella verrà eliminata dalla replica. Se si dispone di tabelle esistenti nel Sottoscrittore che si desidera mantenere, utilizzare la stored procedure [sp_changearticle](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md) per ogni articolo e specificare il valore 'none' per **pre_creation_cmd**. `sp_changearticle @publication= 'MyPublication', @article= 'MyArticle', @property='pre_creation_cmd', @value='none'`.  
  
#### <a name="to-generate-a-snapshot-for-the-publication"></a>Per generare uno snapshot per la pubblicazione.  
  
1.  Espandere la cartella **Replica** e quindi la cartella **Pubblicazioni locali** .  
  
2.  Fare clic con il pulsante destro del mouse sulla pubblicazione e quindi scegliere **Visualizza stato agente snapshot**.  
  
3.  Nella finestra di dialogo **Visualizza stato agente snapshot - \<Publication>** fare clic su **Avvia**.  
  
 Al termine della generazione dello snapshot da parte dell'agente, viene visualizzato un messaggio come ""[100%] Generato uno snapshot di 17 articoli."  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 È possibile creare sottoscrizioni push a sottoscrittori non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a livello di programmazione usando le stored procedure di replica.  
  
> [!IMPORTANT]  
>  Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file script, è fondamentale proteggere il file per evitare accessi non autorizzati.  
  
#### <a name="to-create-a-push-subscription-for-a-transactional-or-snapshot-publication-to-a-non-sql-server-subscriber"></a>Per creare una sottoscrizione push per una pubblicazione transazionale o snapshot in un Sottoscrittore non SQL Server  
  
1.  Installare il provider OLE DB più recente per il Sottoscrittore non[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sia nel server di pubblicazione che nel server di distribuzione. Per i requisiti di replica relativi a un provider OLE DB, vedere [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md), [Oracle Subscribers](../../relational-databases/replication/non-sql/oracle-subscribers.md), [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md).  
  
2.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md) per verificare che la pubblicazione supporti Sottoscrittori non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
    -   Se il valore di **enabled_for_het_sub** è 1, i Sottoscrittori non[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono supportati.  
  
    -   Se il valore di **enabled_for_het_sub** è 0, eseguire [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md), specificando **enabled_for_het_sub** per `@property` e **true** per `@value`.  
  
        > [!NOTE]  
        >  Prima di impostare **enabled_for_het_sub** su **true**, è necessario eliminare eventuali sottoscrizioni esistenti nella pubblicazione. Non è possibile impostare **enabled_for_het_sub** su **true** se la pubblicazione supporta anche sottoscrizioni aggiornabili. La modifica dell'impostazione **enabled_for_het_sub** influirà su altre proprietà della pubblicazione. Per altre informazioni, vedere [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md).  
  
3.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Specificare `@publication`, `@subscriber`, il valore `(default destination)` per `@destination_db`, il valore **push** per `@subscription_type` e il valore 3 per `@subscriber_type` (per indicare un provider OLE DB).  
  
4.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addpushsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md). Specificare le opzioni seguenti:  
  
    -   I parametri `@subscriber` e `@publication`.  
  
    -   Il valore **(default destination)** per `@subscriber_db`.  
  
    -   Le proprietà dell'origine dati diversa da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per `@subscriber_provider`, `@subscriber_datasrc`, `@subscriber_location`, `@subscriber_provider_string` e `@subscriber_catalog`.  
  
    -   Le credenziali di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows usate per l'esecuzione dell'agente di distribuzione nel server di distribuzione per `@job_login` e `@job_password`.  
  
       > [!NOTE]  
       > Per le connessioni effettuate con l'autenticazione integrata di Windows vengono usate sempre le credenziali di Windows specificate da `@job_login` e `@job_password`. L'agente di distribuzione esegue sempre la connessione locale al server di distribuzione utilizzando l'autenticazione integrata di Windows. Per impostazione predefinita, l'agente si connette al Sottoscrittore utilizzando l'autenticazione integrata di Windows.  
  
    -   Il valore **0** per `@subscriber_security_mode` e le informazioni sull'account di accesso del provider OLE DB per `@subscriber_login` e `@subscriber_password`.  
  
    -   Specificare una pianificazione per il processo dell'agente di distribuzione da eseguire per la sottoscrizione. Per altre informazioni, vedere [Specify Synchronization Schedules](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
    > [!IMPORTANT]  
    >  Quando si crea una sottoscrizione push in un server di pubblicazione per un server di distribuzione remoto, i valori specificati per tutti i parametri, compresi *job_login* e *job_password*, vengono inviati al server di distribuzione come testo normale. È consigliabile crittografare la connessione tra il server di pubblicazione e il server di distribuzione remoto prima di eseguire questa stored procedure. Per altre informazioni, vedere [Abilitare le connessioni crittografate al motore di database &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
## <a name="see-also"></a>Vedere anche  
 [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md)   
 [Oracle Subscribers](../../relational-databases/replication/non-sql/oracle-subscribers.md)   
 [Altri Sottoscrittori non SQL Server](../../relational-databases/replication/non-sql/other-non-sql-server-subscribers.md)   
 [Replication System Stored Procedures Concepts](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Procedure consigliate per la sicurezza della replica](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
  
