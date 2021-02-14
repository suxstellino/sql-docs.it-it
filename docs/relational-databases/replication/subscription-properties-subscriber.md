---
title: Finestra di dialogo Proprietà sottoscrizione
description: Viene descritta la finestra di dialogo "Proprietà sottoscrizione" in SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newsubwizard.subproperties.publisher.f1
- sql13.rep.newsubwizard.subproperties.subscriber.f1
ms.assetid: db2be511-c76e-4f21-8be4-6a8c60a50d30
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 1d0af8d38957286080b6e01c4ed73149ee43bc36
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272332"
---
# <a name="sql-server-replication-subscription-properties-dialog-box"></a>Finestra di dialogo Proprietà sottoscrizione di replica di SQL Server 
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]

### <a name="publisher-properties"></a>Proprietà server di pubblicazione
La finestra di dialogo **Proprietà sottoscrizione** nel server di pubblicazione consente di visualizzare e impostare proprietà per le sottoscrizioni push. Sebbene sia possibile visualizzare alcune proprietà per le sottoscrizioni pull, nella finestra di dialogo **Proprietà sottoscrizione** nel Sottoscrizione sono incluse proprietà aggiuntive ed è possibile modificarle.  
  
 Ogni proprietà nella finestra di dialogo **Proprietà sottoscrizione** include una descrizione. Fare clic su una proprietà per visualizzarne la descrizione nella parte inferiore della finestra di dialogo. In questo argomento vengono fornite informazioni aggiuntive su alcune proprietà che, nella maggior parte dei casi, sono visualizzate nel server di pubblicazione solo per le sottoscrizioni push. Le proprietà sono raggruppate nelle categorie seguenti:  
  
-   Proprietà che si applicano a tutte le sottoscrizioni.    
-   Proprietà che si applicano alle sottoscrizioni transazionali.  
-   Proprietà che si applicano alle sottoscrizioni di tipo merge.  
  
 Se un'opzione è visualizzata in modalità di sola lettura, può essere impostata solo al momento della creazione della sottoscrizione. Se si desidera impostare opzioni non disponibili nella Creazione guidata nuova sottoscrizione, creare la sottoscrizione tramite stored procedure. Per ulteriori informazioni, vedere [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md) e [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md).  

### <a name="subscriber-properties"></a>Proprietà sottoscrittore
La finestra di dialogo **Proprietà sottoscrizione** nel Sottoscrittore consente di visualizzare e impostare le proprietà per le sottoscrizioni pull.  
  
 Ogni proprietà nella finestra di dialogo **Proprietà sottoscrizione** include una descrizione. Fare clic su una proprietà per visualizzarne la descrizione nella parte inferiore della finestra di dialogo. In questo argomento sono specificate informazioni aggiuntive su diverse proprietà. Le proprietà sono raggruppate nelle categorie seguenti:    
-   Proprietà che si applicano a tutte le sottoscrizioni.    
-   Proprietà che si applicano alle sottoscrizioni transazionali.   
-   Proprietà che si applicano alle sottoscrizioni di tipo merge.  
  
 Se un'opzione è visualizzata in modalità di sola lettura, può essere impostata solo al momento della creazione della sottoscrizione. Se si desidera impostare opzioni non disponibili nella Creazione guidata nuova sottoscrizione, creare la sottoscrizione tramite stored procedure. Per ulteriori informazioni, vedere [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md) e [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md).  
  
> [!NOTE]  
>  - Se non è stato ancora creato un processo dell'agente di merge o di distribuzione per la sottoscrizione, numerose proprietà non verranno visualizzate. Per creare un processo dell'agente per una sottoscrizione pull, eseguire [sp_addpullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md) in caso di una sottoscrizione a una pubblicazione transazionale o snapshot o [sp_addmergepullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md) in caso di una sottoscrizione a una pubblicazione di tipo merge.  
> - I database di Istanza gestita di SQL di Azure possono essere un server di pubblicazione, un server di distribuzione e un Sottoscrittore per la replica snapshot e transazionale. I database nel database SQL di Azure possono essere solo sottoscrittori push per la replica snapshot e transazionale. Per altre informazioni, vedere [Replica transazionale con il database SQL di Azure](/azure/sql-database/sql-database-managed-instance-transactional-replication). 
  
## <a name="publisher-options-for-all-subscriptions"></a>Opzioni del server di pubblicazione per tutte le sottoscrizioni  
 **Sicurezza**  
 Fare clic sulla riga **Account processo agente** e quindi sul pulsante delle proprietà ( **...** ) per modificare l'account utilizzato per l'esecuzione dell'agente di distribuzione o dell'agente di merge nel server di distribuzione. Per modificare l'account utilizzato dall'agente di distribuzione o dall'agente di merge per creare connessioni al Sottoscrittore, fare clic su **Connessione al Sottoscrittore** e quindi sul pulsante delle proprietà ( **...** ).  
  
 Per ulteriori informazioni sulle autorizzazioni necessarie per ogni agente, vedere [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
## <a name="publisher-options-for-transactional-subscriptions"></a>Opzioni del server di pubblicazione per le sottoscrizioni transazionali  
 **Impedisci il loop delle transazioni**  
 Consente di stabilire se l'agente di distribuzione reinvia le transazioni originate nel Sottoscrittore al Sottoscrittore. Questa opzione viene utilizzata per la replica transazionale bidirezionale. Per altre informazioni, vedere [Bidirectional Transactional Replication](../../relational-databases/replication/transactional/bidirectional-transactional-replication.md).  
  
 **Sottoscrizione aggiornabile**  
 Consente di stabilire se le modifiche del Sottoscrittore vengono replicate sul server di pubblicazione. È possibile replicare le modifiche utilizzando l'aggiornamento in coda o immediato. L'opzione **Metodo di aggiornamento del Sottoscrittore** determina quale metodo utilizzare. Per altre informazioni, vedere [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md).  
  
## <a name="publisher-options-for-merge-subscriptions"></a>Opzioni del server di pubblicazione per le sottoscrizioni di tipo merge  
 **Definizione partizione (HOST_NAME)**  
 Nel caso di una pubblicazione che utilizza filtri con parametri, la replica di tipo merge valuta una delle due funzioni di sistema, o entrambe se il filtro fa riferimento sia all'una che all'altra, durante la sincronizzazione per determinare i dati che devono essere ricevuti dal Sottoscrittore, ovvero **SUSER_SNAME()** o **HOST_NAME()** . Per impostazione predefinita, **HOST_NAME()** restituisce il nome del computer in cui è in esecuzione l'agente di merge. È tuttavia possibile sostituire questo valore nella Creazione guidata nuova sottoscrizione. Per ulteriori sui filtri con parametri e la sostituzione di **HOST_NAME()** , vedere [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 **Tipo di sottoscrizione** e **Priorità**  
 Indica se la sottoscrizione è una sottoscrizione client o server. Questa impostazione non può essere modificata dopo la creazione della sottoscrizione. Le sottoscrizioni server possono ripubblicare i dati in altri Sottoscrittori. A tali sottoscrizioni è inoltre possibile assegnare una priorità per la risoluzione dei conflitti.  
  
 Se si seleziona un tipo di sottoscrizione server nella Creazione guidata nuova sottoscrizione, al Sottoscrittore viene assegnata una priorità che verrà utilizzata nella risoluzione dei conflitti.  
  
 **Risoluzione interattiva**  
 Consente di indicare di utilizzare l'interfaccia utente del sistema di risoluzione dei conflitti interattivo durante la sincronizzazione di tipo merge. A tale scopo, è necessario impostare **Usa Gestione sincronizzazione Microsoft Windows** su **Abilita**. Per altre informazioni, vedere [Interactive Conflict Resolution](../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md).  

  
## <a name="subscriber-options-for-all-subscriptions"></a>Opzioni del sottoscrittore per tutte le sottoscrizioni  
 **Inizializza dati pubblicati da uno snapshot**  
 Consente di stabilire se le sottoscrizioni vengono inizializzate tramite uno snapshot, impostazione predefinita, o un altro metodo. Per altre informazioni sull'inizializzazione delle sottoscrizioni, vedere [Inizializzare una sottoscrizione](../../relational-databases/replication/initialize-a-subscription.md).  
  
 **Posizione snapshot**  
 Consente di determinare la posizione da cui si accede ai file di snapshot durante l'inizializzazione o una nuova inizializzazione. La posizione può essere uno dei valori seguenti:  
  
-   **Posizione predefinita**. Si tratta della posizione predefinita, che viene stabilita durante la configurazione di un server di distribuzione. Per altre informazioni, vedere [Modificare le opzioni snapshot](../../relational-databases/replication/snapshot-options.md).  
  
-   **Cartella alternativa**. Si tratta di una posizione alternativa, che può essere specificata nella finestra di dialogo **Proprietà pubblicazione** . Per altre informazioni, vedere [Modificare le opzioni snapshot](../../relational-databases/replication/snapshot-options.md).  
  
-   **Cartella snapshot dinamici**. Si tratta di una posizione degli snapshot per pubblicazioni di tipo merge che utilizzano filtri di riga con parametri. Per altre informazioni, vedere [Snapshots for Merge Publications with Parameterized Filters](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
-   **Cartella FTP**. È una cartella accessibile a un server FTP (File Transfer Protocol). Per altre informazioni, vedere [Recapitare gli snapshot tramite FTP](../../relational-databases/replication/publish/deliver-a-snapshot-through-ftp.md).  
  
 **Cartella snapshot**  
 Se si seleziona un valore qualsiasi diverso da **Posizione predefinita** per l'opzione **Posizione snapshot** , sarà necessario specificare un percorso per la cartella snapshot.  
  
 **Usa Gestione sincronizzazione Microsoft Windows**  
 Consente di stabilire se è possibile sincronizzare la sottoscrizione tramite Gestione sincronizzazione [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 **Sicurezza**  
 Fare clic sulla riga **Account processo agente** , quindi sul pulsante delle proprietà ( **...** ) per modificare l'account con cui viene eseguito l'agente di merge o di distribuzione nel Sottoscrittore. Le opzioni di sicurezza correlate a connessioni dipendono dal tipo di sottoscrizione:  
  
-   In caso di sottoscrizioni a una pubblicazione transazionale, per modificare l'account con cui l'agente di distribuzione crea connessioni al server di distribuzione, fare clic su **Connessione server di distribuzione** e quindi sul pulsante delle proprietà **...** .  
  
-   In caso di sottoscrizioni ad aggiornamento immediato a una pubblicazione transazionale, oltre alla connessione al server di distribuzione descritta in precedenza, è possibile modificare il metodo utilizzato per la propagazione delle modifiche dal Sottoscrittore al server di pubblicazione facendo clic su **Connessione server di pubblicazione** e quindi sul pulsante delle proprietà **...** .  
  
-   In caso di sottoscrizioni a pubblicazioni di tipo merge, fare clic su **Connessione server di pubblicazione** e quindi sul pulsante delle proprietà **...** .  
  
 Per ulteriori informazioni sulle autorizzazioni necessarie per ogni agente, vedere [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
## <a name="subscriber-options-for-transactional-subscriptions"></a>Opzioni del sottoscrittore per le sottoscrizioni transazionali  
 **Sottoscrizione aggiornabile**  
 Consente di stabilire se le modifiche del Sottoscrittore vengono replicate sul server di pubblicazione. È possibile replicare le modifiche utilizzando l'aggiornamento in coda o immediato. L'opzione **Metodo di aggiornamento del Sottoscrittore** determina quale metodo utilizzare. Per altre informazioni, vedere [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md).  
  
## <a name="options-for-merge-subscriptions"></a>Opzioni per sottoscrizioni di tipo merge  
 **Definizione partizione (HOST_NAME)**  
 Nel caso di una pubblicazione che utilizza filtri con parametri, la replica di tipo merge valuta una delle due funzioni di sistema, o entrambe se il filtro fa riferimento sia all'una che all'altra, durante la sincronizzazione per determinare i dati che devono essere ricevuti dal Sottoscrittore, ovvero **SUSER_SNAME()** o **HOST_NAME()** . Per impostazione predefinita, **HOST_NAME()** restituisce il nome del computer in cui è in esecuzione l'agente di merge. È tuttavia possibile sostituire questo valore nella Creazione guidata nuova sottoscrizione. Per ulteriori sui filtri con parametri e la sostituzione di **HOST_NAME()** , vedere [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 **Tipo di sottoscrizione** e **Priorità**  
 Indica se la sottoscrizione è una sottoscrizione client o server. Questa impostazione non può essere modificata dopo la creazione della sottoscrizione. Le sottoscrizioni server possono ripubblicare i dati in altri Sottoscrittori. A tali sottoscrizioni è inoltre possibile assegnare una priorità per la risoluzione dei conflitti.  
  
 Se si seleziona un tipo di sottoscrizione server nella Creazione guidata nuova sottoscrizione, al Sottoscrittore viene assegnata una priorità che verrà utilizzata nella risoluzione dei conflitti  
  
 **Risoluzione interattiva**  
 Consente di indicare di utilizzare l'interfaccia utente del sistema di risoluzione dei conflitti interattivo durante la sincronizzazione di tipo merge. A tale scopo, è necessario impostare **Usa Gestione sincronizzazione Microsoft Windows** su **Abilita**. Per altre informazioni, vedere [Interactive Conflict Resolution](../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md).  
  
 **Sincronizzazione Web**  
 **Usa sincronizzazione Web** consente di indicare di sincronizzare la sottoscrizione tramite una connessione a un server [!INCLUDE[msCoName](../../includes/msconame-md.md)] Internet Information Services (IIS). Questa opzione è disponibile solo se la pubblicazione è abilitata per la sincronizzazione Web. Per altre informazioni, vedere [Web Synchronization for Merge Replication](../../relational-databases/replication/web-synchronization-for-merge-replication.md).  
  
 Se si seleziona **Vero** per **Usa sincronizzazione Web**:  
  
-   Immettere l'indirizzo completo del server IIS in **Indirizzo server Web**.    
-   Fare clic sulla riga **Connessione server Web** e quindi sul pulsante delle proprietà **...** per impostare o modificare l'account con cui il sottoscrittore si connette al server IIS.    
-   Se necessario, modificare **Timeout server Web** . Il timeout rappresenta il tempo di attesa, in secondi, prima della scadenza di una richiesta di sincronizzazione Web. 

 
Per ulteriori informazioni sulla configurazione, vedere [Configure Web Synchronization](../../relational-databases/replication/configure-web-synchronization.md). 
  
## <a name="see-also"></a>Vedere anche  
 [Visualizzare e modificare le proprietà delle sottoscrizioni pull](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)   
 [Visualizzare e modificare le proprietà delle sottoscrizioni push](../../relational-databases/replication/view-and-modify-push-subscription-properties.md)   
 [Sottoscrizione delle pubblicazioni](../../relational-databases/replication/subscribe-to-publications.md)   

  
  
