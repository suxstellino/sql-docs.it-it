---
description: Specificare le proprietà della replica di tipo merge
title: Specificare le proprietà della replica di tipo merge| Microsoft Docs
ms.custom: ''
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- merge replication [SQL Server replication], download-only articles
- articles [SQL Server replication], download-only
- download-only articles
ms.assetid: 14839cec-6dbf-49c2-aa27-56847b09b4db
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f53a43fa102505121cd176eca6c94d4697a25372
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336189"
---
# <a name="specify-merge-replication-properties"></a>Specificare le proprietà della replica di tipo merge
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
Questo argomento illustra come specificare varie proprietà per la replica di tipo merge. 

## <a name="merge-article-is-download-only"></a>Articolo di merge di solo download
Gli articoli di solo download sono progettati per le applicazioni i cui dati non vengono aggiornati nei Sottoscrittori. Per altre informazioni, vedere [Ottimizzare le prestazioni della replica di tipo merge con gli articoli di solo download](../../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md).  
   
###  <a name="considerations"></a>Considerazioni   
-   Se si specifica che un articolo è di tipo solo download dopo l'inizializzazione delle sottoscrizioni, tutte le sottoscrizioni client a cui è stato inviato l'articolo devono essere reinizializzate. Non è necessario reinizializzare le sottoscrizioni server. Per altre informazioni sugli effetti delle modifiche delle proprietà, vedere [Modifica delle proprietà di pubblicazioni e articoli](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
### <a name="use-sql-server-management-studio"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="on-the-articles-page"></a>Nella pagina Articoli
Selezionare una tabella nella pagina **Articoli** di Creazione guidata nuova pubblicazione e quindi selezionare la casella di controllo **La tabella evidenziata è di tipo solo download**.  
  
#### <a name="on-the-properties-tab-of-the-article-properties"></a>Nella scheda Proprietà della finestra delle proprietà dell'articolo  
  
1.  Nella pagina **Articoli** della Creazione guidata nuova pubblicazione o nella finestra di dialogo **Proprietà pubblicazione - \<Publication>** selezionare una tabella e quindi fare clic su **Proprietà articolo**.    
2.  Fare clic su **Imposta proprietà dell'articolo di tabelle evidenziato** o su **Imposta proprietà di tutti gli articoli di tabelle**.  
  
3.  Nella sezione **Oggetto di destinazione** della scheda **Proprietà** della finestra di dialogo **Proprietà articolo - \<Article>** specificare uno dei valori seguenti in **Direzione sincronizzazione**:    
    -   **Solo download sul Sottoscrittore, non consentire modifiche del Sottoscrittore**    
    -   **Solo download sul Sottoscrittore, consenti modifiche del Sottoscrittore**    
4.  Se è visualizzata la finestra di dialogo **Proprietà pubblicazione - \<Publication>** fare clic su **OK** per salvare e chiudere la finestra di dialogo.  

###  <a name="use-transact-sql"></a>Usare Transact-SQL  
  
#### <a name="new-article"></a>Nuovo articolo  
  
1.  Eseguire [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md), specificando il valore **1** o **2** per il parametro `@subscriber_upload_options`. I numeri corrispondono al comportamento seguente:  
  
    -   **0** : nessuna restrizione (valore predefinito). Le modifiche eseguite nel Sottoscrittore vengono caricate nel server di pubblicazione.    
    -   **1** : le modifiche sono consentite nel Sottoscrittore, ma non vengono caricate nel server di pubblicazione.    
    -   **2** : non è consentito apportare modifiche nel Sottoscrittore.  
  
       > [!NOTE]  
       > Se la tabella di origine di un articolo è già inclusa in un'altra pubblicazione, il valore di `@subscriber_upload_options` deve coincidere per entrambi gli articoli.  
  
#### <a name="existing-article"></a>Articolo esistente
  
1.  Per determinare se un articolo è di solo download, eseguire [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md) e verificare il valore di **upload_options** per l'articolo nel set di risultati. 
  
2.  Se il valore restituito al passaggio 1 è **0**, eseguire [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md), specificando il valore **subscriber_upload_options** per `@property`, il valore **1** per `@force_invalidate_snapshot` e `@force_reinit_subscription` e il valore **1** o **2** per `@value`, che corrisponde al comportamento seguente:  
  
    -   **1** : le modifiche sono consentite nel Sottoscrittore, ma non vengono caricate nel server di pubblicazione.    
    -   **2** : non è consentito apportare modifiche nel Sottoscrittore.  
  
        > [!NOTE]  
        >  Se la tabella di origine di un articolo è già inclusa in un'altra pubblicazione, il comportamento del solo download deve coincidere per entrambi gli articoli.  
  
## <a name="interactive-conflict-resolution"></a>Interactive Conflict Resolution 
  
 La replica di Microsoft SQL Server offre un sistema di risoluzione interattivo che consente di risolvere i conflitti manualmente durante la sincronizzazione su richiesta in Gestione sincronizzazione di Microsoft Windows. Dopo l'abilitazione della risoluzione interattiva, risolvere interattivamente i conflitti durante la sincronizzazione utilizzando il sistema di risoluzione interattivo. Il sistema di risoluzione interattivo è disponibile tramite Gestione sincronizzazione di Microsoft Windows. Per altre informazioni, vedere [Sincronizzare una sottoscrizione mediante Gestione sincronizzazione Microsoft Windows &#40;Gestione sincronizzazione Microsoft Windows&#41;](../../../relational-databases/replication/synchronize-a-subscription-using-windows-synchronization-manager.md).  
  
  
### <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Se si esegue una sincronizzazione all'esterno di Gestione sincronizzazione Microsoft Windows, ad esempio una sincronizzazione pianificata o su richiesta in SQL Server Management Studio o Monitoraggio replica, i conflitti vengono risolti automaticamente senza l'intervento dell'utente, utilizzando la risoluzione dei conflitti predefinita specificata per l'articolo. Per altre informazioni, vedere [Interactive Conflict Resolution](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md).  
  
### <a name="use-sql-server-management-studio"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="enable-interactive-conflict-resolution-for-an-article"></a>Abilitare la risoluzione interattiva dei conflitti per un articolo  
  
1.  Selezionare una tabella nella pagina **Articoli** della Creazione guidata nuova pubblicazione o nella finestra di dialogo **Proprietà pubblicazione - \<Publication>** . Per altre informazioni sull'uso della creazione guidata e l'accesso alla finestra di dialogo, vedere [Creare una pubblicazione](../../../relational-databases/replication/publish/create-a-publication.md) e [Visualizzare e modificare le proprietà della pubblicazione](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).   
2.  Fare clic su **Proprietà articolo**, quindi su **Imposta proprietà dell'articolo di tabella evidenziato** o su **Imposta proprietà di tutti gli articoli di tabelle**.    
3.  Nella pagina **Proprietà articolo - \<Article>** o **Proprietà articolo - \<ArticleType>** fare clic sulla scheda **Sistema di risoluzione**.    
4.  Selezionare **Consenti la risoluzione interattiva dei conflitti nel Sottoscrittore durante la sincronizzazione su richiesta**.    
5.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]    
6.  Se è visualizzata la finestra di dialogo **Proprietà pubblicazione - \<Publication>** fare clic su **OK** per salvare e chiudere la finestra di dialogo.  
  
#### <a name="specify-that-a-subscription-should-use-interactive-conflict-resolution"></a>Specificare la risoluzione interattiva dei conflitti per una sottoscrizione  
  
1.  Nella finestra di dialogo **Proprietà sottoscrizione - \<Subscriber>: \<SubscriptionDatabase>** specificare il valore **Vero** per l'opzione **Risoluzione interattiva dei conflitti**. Per ulteriori informazioni sull'accesso a questa finestra di dialogo, vedere [View and Modify Push Subscription Properties](../../../relational-databases/replication/view-and-modify-push-subscription-properties.md) e [View and Modify Pull Subscription Properties](../../../relational-databases/replication/view-and-modify-pull-subscription-properties.md).   
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
###  <a name="use-transact-sql"></a>Usare Transact-SQL  
 È possibile impostare a livello di programmazione un Sottoscrittore in modo che utilizzi questa interfaccia grafica per risolvere conflitti relativi agli articoli quando viene creata una sottoscrizione pull di una pubblicazione di tipo merge. Nel sistema di risoluzione interattivo verranno visualizzati solo i conflitti relativi ad articoli che supportano questa opzione.  
  
#### <a name="create-a-merge-pull-subscription-that-uses-the-interactive-resolver"></a>Creare una sottoscrizione pull di tipo merge che usa il sistema di risoluzione interattivo  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md), specificando `@publication`. Notare il valore di **allow_interactive_resolver** relativo a ogni articolo nel set di risultati per il quale verrà utilizzato il sistema di risoluzione interattivo.   
    -   Se questo valore è **1**, il sistema di risoluzione interattivo verrà utilizzato.    
    -   Se questo valore è **0**, è prima necessario attivare il sistema di risoluzione interattivo per ogni articolo. A tale scopo, eseguire [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md), specificando `@publication`, `@article`, il valore **allow_interactive_resolver** per `@property` e il valore **true** per `@value`.    
2.  Nel database di sottoscrizione del Sottoscrittore eseguire [sp_addmergepullsubscription](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md). Per altre informazioni, vedere [Creazione di una sottoscrizione pull](../../../relational-databases/replication/create-a-pull-subscription.md).    
3.  Nel database di sottoscrizione del Sottoscrittore eseguire [sp_addmergepullsubscription_agent](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md)specificando i parametri seguenti:    
    -   `@publisher`, `@publisher_db` (database pubblicato) e `@publication`.    
    -   Il valore **true** per `@enabled_for_syncmgr`.    
    -   Il valore **true** per `@use_interactive_resolver`.    
    -   Le informazioni sull'account di sicurezza richieste dall'agente di merge. Per altre informazioni, vedere [Creazione di una sottoscrizione pull](../../../relational-databases/replication/create-a-pull-subscription.md).    
4.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergesubscription](../../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md).  
  
#### <a name="define-an-article-that-supports-the-interactive-resolver"></a>Definire un articolo che supporta il sistema di risoluzione interattivo  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Specificare il nome della pubblicazione cui appartiene l'articolo per `@publication`, il nome dell'articolo per `@article`, l'oggetto di database da pubblicare per `@source_object` e il valore **true** per `@allow_interactive_resolver`. Per altre informazioni, vedere [definire un articolo](../../../relational-databases/replication/publish/define-an-article.md).  
 
## <a name="conflict-tracking-and-resolution-level-for-merge-articles"></a>Livello di rilevamento e risoluzione dei conflitti per gli articoli di merge
In questo argomento viene descritto come specificare il livello di rilevamento e risoluzione dei conflitti per gli articoli di merge in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 Quando si sincronizza una sottoscrizione di una pubblicazione di tipo merge, la replica verifica la presenza di conflitti causati dalle modifiche apportate agli stessi dati nel server di pubblicazione e nel Sottoscrittore. È possibile specificare se rilevare i conflitti a livello di riga, ovvero considerare un conflitto qualsiasi modifica apportata alla riga, o a livello di colonna, ovvero considerare un conflitto solo le modifiche apportate alla stessa riga e colonna. La risoluzione dei conflitti relativi agli articoli viene eseguita a livello di riga. Per ulteriori informazioni sul rilevamento e sulla risoluzione dei conflitti in caso di utilizzo dei record logici, vedere [Detecting and Resolving Conflicts in Logical Records](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-resolving-in-logical-record.md).  
 
### <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
  
-   Se si modifica il livello di rilevamento in seguito all'inizializzazione delle sottoscrizioni, sarà necessario reinizializzarle. Per altre informazioni sugli effetti delle modifiche delle proprietà, vedere [Modificare le proprietà di pubblicazioni e articoli](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).   
-   Con il rilevamento a livello di riga e di colonna, la risoluzione dei conflitti viene sempre eseguita a livello di riga, ovvero la riga che prevale sovrascrive quella perdente. La replica di tipo merge consente inoltre di specificare che i conflitti vengano rilevati e risolti a livello di record logico. Queste opzioni tuttavia non sono disponibili in [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Per informazioni sulla relativa impostazione dalle stored procedure di replica, vedere [Definizione di una relazione tra record logici degli articoli di tabelle di merge](../../../relational-databases/replication/publish/define-a-logical-record-relationship-between-merge-table-articles.md).  
  
### <a name="use-sql-server-management-studio"></a>Utilizzo di SQL Server Management Studio  
 Specificare il rilevamento a livello di riga o colonna per gli articoli di merge nella scheda **Proprietà** della finestra di dialogo **Proprietà articolo**, disponibile nella Creazione guidata nuova pubblicazione e nella finestra di dialogo **Proprietà pubblicazione - \<Publication>one>** . Per altre informazioni sull'uso della creazione guidata e l'accesso alla finestra di dialogo, vedere [Creare una pubblicazione](../../../relational-databases/replication/publish/create-a-publication.md) e [Visualizzare e modificare le proprietà della pubblicazione](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="specify-row--or-column-level-tracking"></a>Specificare il rilevamento a livello di riga o di colonna  
  
1.  Selezionare una tabella nella pagina **Articoli** della Creazione guidata nuova pubblicazione o nella finestra di dialogo **Proprietà pubblicazione - \<Publication>** .  
2.  Fare clic su **Proprietà articolo**, quindi su **Imposta proprietà dell'articolo di tabella evidenziato** o su **Imposta proprietà di tutti gli articoli di tabelle**.   
3.  Nella scheda **Proprietà** della finestra di dialogo **Proprietà articolo \<Article>** selezionare uno dei valori seguenti per la proprietà **Livello rilevamento**: **Rilevamento a livello di riga** o **Rilevamento a livello di colonna**.   
4.  Se è visualizzata la finestra di dialogo **Proprietà pubblicazione - \<Publication>** fare clic su **OK** per salvare e chiudere la finestra di dialogo.  
  
### <a name="use-transact-sql"></a>Usare Transact-SQL  
  
#### <a name="to-specify-conflict-tracking-options-for-a-new-merge-article"></a>Per specificare le opzioni di rilevamento dei conflitti per un nuovo articolo di merge  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) e specificare uno dei valori seguenti per `@column_tracking`:  
  
    -   **true** : consente di utilizzare il rilevamento a livello di colonna per l'articolo.    
    -   **false** : consente di utilizzare il rilevamento a livello di riga, che corrisponde all'impostazione predefinita.  
  
#### <a name="change-conflict-tracking-options-for-a-merge-article"></a>Modificare le opzioni di rilevamento dei conflitti per un articolo di merge  
  
1.  Per determinare le opzioni di rilevamento dei conflitti per un articolo di merge, eseguire [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md). Notare il valore dell'opzione **column_tracking** nel set di risultati relativo l'articolo. Il valore **1** indica che viene utilizzato il rilevamento a livello di colonna, mentre il valore **0** indica che viene utilizzato il rilevamento a livello di riga.    
2.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Specificare il valore **column_tracking** per `@property` e uno dei valori seguenti per `@value`:  
  
    -   **true** : consente di utilizzare il rilevamento a livello di colonna per l'articolo.    
    -   **false** : consente di utilizzare il rilevamento a livello di riga, che corrisponde all'impostazione predefinita.  
  
     Specificare il valore **1** per `@force_invalidate_snapshot` e `@force_reinit_subscription`. 

## <a name="manage-tracking--deletes"></a>Gestire il rilevamento delle eliminazioni
    
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
  
 Per impostazione predefinita, la replica di tipo merge consente di sincronizzare i comandi DELETE tra server di pubblicazione e Sottoscrittore. Con la replica di tipo merge le righe vengono mantenute nel database di sottoscrizione anche se sono state eliminate dalla pubblicazione e viceversa. È possibile specificare a livello di programmazione che i comandi DELETE vengano ignorati durante la creazione di un nuovo articolo oppure attivare questa funzionalità in un secondo momento utilizzando le stored procedure di replica.  
  
> [!IMPORTANT]  
>  L'attivazione di questa funzionalità causa la non convergenza pertanto i dati presenti nel Sottoscrittore non rifletteranno accuratamente i dati presenti nel server di pubblicazione. È necessario implementare un meccanismo personalizzato per la rimozione manuale delle righe eliminate.  
  
### <a name="specify-that-deletes-be-ignored-for-a-new-merge-article"></a>Specificare di ignorare le eliminazioni per un nuovo articolo di merge  
  
Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Specificare il valore **false** per `@delete_tracking`. Per altre informazioni, vedere [definire un articolo](../../../relational-databases/replication/publish/define-an-article.md).
  
> [!NOTE]  
>  Se la tabella di origine di un articolo è già pubblicata in un'altra pubblicazione, il valore di **delete_tracking** deve essere uguale per entrambi gli articoli.  
  
### <a name="specify-that-deletes-be-ignored-for-an-existing-merge-article"></a>Specificare di ignorare le eliminazioni per un articolo di merge esistente  
  
1.  Per determinare se la compensazione errori è abilitata per un articolo, eseguire [sp_helpmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md) e prendere nota del valore di **delete_tracking** nel set di risultati. Se questo valore è **0**, le eliminazioni vengono già ignorate.  
  
2.  Se il valore ottenuto al passaggio 1 è **1**, eseguire [sp_changemergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) nel database di pubblicazione del server di pubblicazione. Specificare il valore `delete_tracking` per `@property` e il valore **false** per `@value`.  
  
    > [!NOTE]  
    >  Se la tabella di origine di un articolo è già pubblicata in un'altra pubblicazione, il valore di **delete_tracking** deve essere uguale per entrambi gli articoli.  
  

## <a name="processing-order"></a>Ordine di elaborazione
  La replica di tipo merge consente di specificare l'ordine in cui gli articoli vengono elaborati dall'agente di merge durante il processo di sincronizzazione. È possibile assegnare a livello di programmazione un ordine a ogni articolo creato utilizzando le stored procedure di replica. Gli articoli vengono elaborati in ordine crescente in base al valore. Se due articoli hanno lo stesso valore, essi vengono elaborati simultaneamente. 

### <a name="how-processing-order-is-determined"></a>Modalità di determinazione dell'ordine di elaborazione  
 Durante la sincronizzazione di tipo merge, gli articoli vengono elaborati per impostazione predefinita nell'ordine richiesto dalle dipendenze tra gli oggetti, inclusi i vincoli di integrità referenziale dichiarativa definiti nelle tabelle di base. L'elaborazione prevede l'enumerazione delle modifiche apportate a una tabella e quindi l'applicazione di tali modifiche. Se non è presente l'integrità referenziale dichiarativa ma esistono filtri join o record logici tra gli articoli di tabella, gli articoli vengono elaborati nell'ordine richiesto dai filtri e dai record logici. Gli articoli non correlati ad altri articoli tramite integrità referenziale dichiarativa, filtri join, record logici o altre dipendenze vengono elaborati in base al nome alternativo dell'articolo nella tabella di sistema [sysmergearticles &#40;Transact-SQL&#41;](../../../relational-databases/system-tables/sysmergearticles-transact-sql.md).  
  
 Si consideri una pubblicazione in cui sono incluse le tabelle **SalesOrderHeader** e **SalesOrderDetail** con una colonna chiave primaria **SalesOrderID** nella tabella **SalesOrderHeader** e una colonna chiave esterna **SalesOrderID** corrispondente nella tabella **SalesOrderDetail** . Durante la sincronizzazione, la replica di tipo merge impedisce le violazioni della chiave esterna inserendo nuove righe in **SalesOrderHeader** prima dell'inserimento delle righe associate in **SalesOrderDetail**. Analogamente, le righe vengono eliminate da **SalesOrderDetail** prima dell'eliminazione delle righe associate da **SalesOrderHeader**.  
  
 In alcune applicazioni, tuttavia, l'integrità referenziale viene applicata tramite trigger di database o a livello di applicazione, anziché tramite integrità referenziale dichiarativa. Nel caso della pubblicazione sopra descritta, anziché utilizzare l'integrità referenziale dichiarativa, la tabella **SalesOrderDetail** potrebbe disporre di un trigger di inserimento in grado di verificare l'esistenza della riga associata nella tabella **SalesOrderHeader** prima di consentire un inserimento. **SalesOrderHeader** potrebbe disporre di un trigger di eliminazione in grado di verificare l'assenza di righe associate in **SalesOrderDetail** prima di consentire un'eliminazione. I trigger non vengono presi in considerazione nella replica di tipo merge per determinare l'ordine di elaborazione degli articoli, poiché non è possibile determinare il risultato del trigger finché non viene attivato. Analogamente, nella replica non possono essere considerati i vincoli definiti a livello di applicazione.  
  
 In caso di mantenimento dell'integrità referenziale tramite trigger o a livello di applicazione, è consigliabile specificare l'ordine in cui gli articoli devono essere elaborati. Nell'esempio con i trigger, verrà specificato che la tabella **SalesOrderHeader** deve essere elaborata prima di **SalesOrderDetail**, poiché l'ordine degli articoli è basato sull'ordine di inserimento. Nella replica di tipo merge verrà automaticamente invertito l'ordine per le eliminazioni. La replica di tipo merge non avrà esito negativo se gli articoli non vengono ordinati, poiché l'agente di merge continua a elaborare gli articoli in caso di violazione di un vincolo e quindi ripete le operazioni non riuscite dopo l'elaborazione degli altri articoli. Specificando l'ordine degli articoli è semplicemente possibile evitare i successivi tentativi e l'elaborazione aggiuntiva associata. Se si specifica un ordine non corretto, ad esempio un ordine che determina l'elaborazione dei record di dettagli prima dei record di intestazione, la replica di tipo merge ripeterà l'elaborazione finché non avrà esito positivo.   
  
### <a name="new-article"></a>Nuovo articolo
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Specificare un valore integer che rappresenta l'ordine di elaborazione per l'articolo per `@processing_order`. Per altre informazioni, vedere [definire un articolo](../../../relational-databases/replication/publish/define-an-article.md).  
  
    > [!NOTE]  
    >  Quando si creano articoli ordinati, è necessario lasciare gap tra i valori relativi all'ordine degli articoli. In questo modo risulta più agevole impostare nuovi valori in futuro. Se, ad esempio, si dispone di tre articoli per cui è necessario specificare un ordine di elaborazione fisso, impostare il valore di `@processing_order` su 10, 20 e 30 anziché rispettivamente su 1, 2 e 3.  
  
### <a name="existing-article"></a>Articolo esistente
  
1.  Per determinare l'ordine di elaborazione di un articolo, eseguire [sp_helpmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md) e prendere nota del valore di **processing_order** nel set di risultati.   
2.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_changemergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Specificare il valore **processing_order** per `@property` e un valore integer che rappresenta l'ordine di elaborazione per `@value`.  


## <a name="see-also"></a>Vedere anche  
 [Ottimizzare le prestazioni della replica di tipo merge con gli articoli di solo download](../../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md)   
 [Define an Article](../../../relational-databases/replication/publish/define-an-article.md)   
 [Visualizzare e modificare le proprietà degli articoli](../../../relational-databases/replication/publish/view-and-modify-article-properties.md)  
  
  
