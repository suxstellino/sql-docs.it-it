---
description: Convalida dei dati replicati
title: Convalidare i dati replicati | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Subscribers [SQL Server replication], data validation
- replication [SQL Server], validating data
- transactional replication, validating data
- validating data
- merge replication data validation [SQL Server replication], SQL Server Management Studio
ms.assetid: 215b4c9a-0ce9-4c00-ac0b-43b54151dfa3
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 1b5109c2bf93a49370f005e94a824fd5bfb70921
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250605"
---
# <a name="validate-replicated-data"></a>Convalida dei dati replicati
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  In questo argomento viene descritto come convalidare i dati nel Sottoscrittore in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o RMO (Replication Management Objects).  
  
La replica transazionale e la replica di tipo merge consentono di verificare che i dati presenti nel Sottoscrittore corrispondano ai dati nel server di pubblicazione. La convalida può essere eseguita per sottoscrizioni specifiche o per tutte le sottoscrizioni di una pubblicazione. Specificare uno dei tipi seguenti di convalida dei dati, che verrà eseguita dall'agente di distribuzione o dall'agente di merge alla successiva sincronizzazione:  
  
-   **Conteggio delle sole righe.** Questo tipo di convalida verifica se la tabella nel Sottoscrittore ha lo stesso numero di righe della tabella nel server di pubblicazione, ma non verifica che il contenuto delle righe corrisponda. La convalida del conteggio delle righe è un controllo non approfondito che consente comunque di evidenziare eventuali problemi dei dati.   
-   **Conteggio delle righe e checksum binario.** Oltre al conteggio delle righe nel server di pubblicazione e nel Sottoscrittore, viene calcolato il checksum di tutti i dati tramite lo specifico algoritmo di checksum. In caso di esito negativo durante il conteggio delle righe il checksum non viene eseguito.  
  
 Oltre a verificare che i dati nel Sottoscrittore corrispondano ai dati nel server di pubblicazione, la replica di tipo merge consente di verificare che i dati vengano partizionati correttamente per ogni Sottoscrittore. Per altre informazioni, vedere [Convalidare le informazioni sulle partizioni per un Sottoscrittore di tipo merge](../../relational-databases/replication/validate-partition-information-for-a-merge-subscriber.md).  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]
   
## <a name="how-data-validation-works"></a>Funzionamento della convalida dei dati  
 In[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] i dati vengono convalidati tramite il conteggio delle righe o il calcolo del checksum nel server di pubblicazione e nel Sottoscrittore e il successivo confronto dei risultati. Viene calcolato un solo valore per l'intera tabella di pubblicazione e un solo valore per l'intera tabella di sottoscrizione. I dati delle colonne di tipo **text**, **ntext** o **image** non vengono inclusi nei calcoli.  
  
 Durante l'esecuzione del calcolo vengono attivati temporaneamente i blocchi condivisi sulle tabelle per le quali viene eseguito il conteggio delle righe o il calcolo del checksum. Il calcolo viene comunque completato e i blocchi condivisi vengono subito rimossi.  
  
 Quando si utilizzano valori checksum binari, il controllo di ridondanza ciclico (CRC) a 32 bit viene eseguito su ogni colonna anziché sulla riga fisica di una pagina di dati. Le colonne possono essere pertanto ordinate fisicamente in qualsiasi modo nella pagina di dati e al medesimo tempo ottenere lo stesso CRC per la riga. La convalida mediante checksum binario può essere utilizzata quando alla pubblicazione sono applicati filtri di riga o di colonna.  

 La convalida dei dati è un processo suddiviso in tre parti:  
  
1.  Una sottoscrizione o tutte le sottoscrizioni di una pubblicazione vengono *contrassegnate* per la convalida. \Contrassegnare le sottoscrizioni per la convalida nelle finestre di dialogo **Convalida sottoscrizione**, **Convalida sottoscrizioni** e **Convalida tutte le sottoscrizioni** , disponibili dalle cartelle **Pubblicazioni locali** e **Sottoscrizioni locali** in [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. È inoltre possibile contrassegnare le sottoscrizioni nella scheda **Tutte le sottoscrizioni** , nella scheda **Elenco verifica sottoscrizioni** e nel nodo delle pubblicazioni in Monitoraggio replica. Per informazioni sull'avvio di Monitoraggio replica, vedere [Avviare Monitoraggio replica](../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
2.  La sottoscrizione viene convalidata alla successiva sincronizzazione eseguita dall'agente di distribuzione, nel caso della replica transazionale, o dall'agente di merge nella replica di tipo merge. L'agente di distribuzione in genere viene eseguito in modo continuativo, pertanto la convalida viene eseguita immediatamente, mentre l'agente di merge in genere viene eseguito su richiesta, pertanto la convalida viene eseguita solo dopo l'esecuzione dell'agente.  
  
3.  I risultati della convalida possono essere visualizzati nelle finestre seguenti:   
    -   Nella finestra dei dettagli in Monitoraggio replica, all'interno della scheda **Cronologia database di distribuzione - Sottoscrittore** per la replica transazionale e nella scheda **Cronologia sincronizzazione** per la replica di tipo merge.    
    -   Nella finestra di dialogo **Visualizza stato sincronizzazione** in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
 
## <a name="considerations-and-restrictions"></a>Considerazioni e restrizioni  
 Durante la convalida dei dati è opportuno considerare gli aspetti seguenti:  
  
-   Prima di eseguire la convalida dei dati è necessario arrestare tutte le attività di aggiornamento nei Sottoscrittori. Non è necessario arrestare l'eventuale aggiornamento nel server di pubblicazione durante la convalida.  
-   Poiché la convalida di set di dati estesi mediante checksum e checksum binari può richiedere quantità elevate di risorse di elaborazione, è opportuno pianificare la convalida in modo che venga eseguita nei periodi di attività minore nei server utilizzati per la replica.   
-   La replica convalida soltanto le tabelle e non verifica che gli articoli solo schema, ad esempio le stored procedure, presenti nel server di pubblicazione e nel Sottoscrittore corrispondano.   
-   Il checksum binario può essere utilizzato per qualsiasi tabella pubblicata. Non è possibile convalidare mediante il calcolo del checksum tabelle con filtri colonne o strutture di tabelle logiche con offset di colonna diversi, a causa di istruzioni di tipo ALTER TABLE tramite cui vengono eliminate o aggiunte colonne.   
-   Per la convalida della replica vengono utilizzate le funzioni **checksum** e **binary_checksum** . Per informazioni sul comportamento, vedere [CHECKSUM &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-transact-sql.md) e [BINARY_CHECKSUM  &#40;Transact-SQL&#41;](../../t-sql/functions/binary-checksum-transact-sql.md).   
-   La convalida eseguita mediante checksum o checksum binario può segnalare erroneamente un problema se nel Sottoscrittore vi sono tipi di dati diversi rispetto al server di pubblicazione. Ciò può verificarsi se si effettua una delle operazioni indicate di seguito.    
    -   Impostazione esplicita delle opzioni per lo schema per l'esecuzione del mapping dei tipi di dati per versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
    -   Impostazione del livello di compatibilità della pubblicazione per una pubblicazione di tipo merge su una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], qualora le tabelle pubblicate contengano uno o più tipi di dati di cui è necessario eseguire il mapping per questa versione.    
    -   Inizializzazione manuale di una sottoscrizione, in caso di utilizzo di tipi di dati diversi nel Sottoscrittore.   
-   Le convalide mediante calcoli del checksum e checksum binario non sono supportate per la replica transazionale di sottoscrizioni trasformabili.   
-   La convalida non è supportata per i dati replicati in Sottoscrittori non[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .    
-   Le procedure relative a Monitoraggio replica riguardano solo le sottoscrizioni push in quanto le sottoscrizioni pull non possono essere sincronizzate in Monitoraggio replica. È tuttavia possibile contrassegnare una sottoscrizione per la convalida e visualizzare i risultati della convalida per le sottoscrizioni pull in Monitoraggio replica.    
-   I risultati della convalida consentono di stabilire se la convalida è stata completata correttamente o meno, ma in caso di errore non indicano quali righe non hanno superato la convalida. Per confrontare i dati nel server di pubblicazione e nel Sottoscrittore, utilizzare la [tablediff Utility](../../tools/tablediff-utility.md). Per ulteriori informazioni sull'uso di questa utilità con i dati replicati, vedere [Confrontare tabelle replicate al fine di individuare le differenze &#40;programmazione della replica&#41;](../../relational-databases/replication/administration/compare-replicated-tables-for-differences-replication-programming.md).  


## <a name="data-validation-results"></a>Risultati della convalida dei dati  
 Al termine della convalida nell'agente di distribuzione o nell'agente di merge vengono registrati messaggi relativi all'esito positivo o negativo dell'operazione. Non vengono specificate le righe con esito negativo. È possibile visualizzare tali messaggi in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], Monitoraggio replica e nelle tabelle di sistema di replica. Nell'argomento elencato sopra vengono illustrate le procedure di esecuzione della convalida e di visualizzazione dei risultati.  
  
 Per gestire gli errori di convalida, considerare quanto segue:  
  
-   Configurare l'avviso di replica **Replica: la convalida dei dati nel Sottoscrittore non è riuscita.** , in modo che l'utente venga avvisato dell'errore. Per altre informazioni, vedere [Configurare gli avvisi di replica predefiniti &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/administration/configure-predefined-replication-alerts-sql-server-management-studio.md).  
  
-   L'errore di convalida rappresenta un problema per l'applicazione utilizzata? Se sì, aggiornare manualmente i dati in modo che siano sincronizzati oppure reinizializzare la sottoscrizione:  
  
    -   È possibile aggiornare i dati tramite l' [utilità tablediff](../../tools/tablediff-utility.md). Per ulteriori informazioni sull'uso di questa utilità, vedere [Confrontare tabelle replicate al fine di individuare le differenze &#40;programmazione della replica&#41;](../../relational-databases/replication/administration/compare-replicated-tables-for-differences-replication-programming.md).  
  
    -   Per altre informazioni sulla reinizializzazione, vedere [Reinizializzare le sottoscrizioni](../../relational-databases/replication/reinitialize-subscriptions.md).  

  
  
## <a name="articles-in-transactional-replication"></a>Articoli nella replica transazionale 

### <a name="using-sql-server-management-studio"></a>Utilizzare SQL Server Management Studio
  
1.  Connettersi al server di pubblicazione in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]e quindi espandere il nodo del server.    
2.  Espandere la cartella **Replica** e quindi la cartella **Pubblicazioni locali** .    
3.  Fare clic con il pulsante destro del mouse sulla pubblicazione di cui si desidera convalidare le sottoscrizioni e quindi scegliere **Convalida sottoscrizioni**.    
4.  Nella finestra di dialogo **Convalida sottoscrizioni** selezionare le sottoscrizioni da convalidare:   
    -   Selezionare **Convalida tutte le sottoscrizioni SQL Server**.    
    -   Selezionare **Convalida le sottoscrizioni seguenti** e quindi scegliere una o più sottoscrizioni.    
5.  Per specificare il tipo di convalida da eseguire (solo conteggio delle righe o conteggio delle righe e valori di checksum), fare clic su **Opzioni di convalida** e quindi specificare le opzioni nella finestra di dialogo **Opzioni di convalida delle sottoscrizioni** .  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]   
7.  I risultati della convalida possono essere visualizzati in Monitoraggio replica o nella finestra di dialogo **Visualizza stato sincronizzazione** . Eseguire la procedura seguente per ogni sottoscrizione:   
    1.  Espandere la pubblicazione, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Visualizza stato sincronizzazione**.    
    2.  Se l'agente non è in esecuzione, fare clic su **Avvia** nella finestra di dialogo **Visualizza stato sincronizzazione** . Nella finestra di dialogo verranno visualizzati messaggi informativi relativi alla convalida.    
     Se non viene visualizzato alcun messaggio attinente, l'agente deve aver già registrato un messaggio successivo. In questo caso, visualizzare i risultati della convalida in Monitoraggio replica. Per ulteriori informazioni, vedere le procedure per Monitoraggio replica in questo argomento.  

### <a name="using-transact-sql"></a>Uso di Transact-SQL

#### <a name="all-articles"></a>Tutti gli articoli 
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_publication_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md). Specificare `@publication` e uno dei valori seguenti per `@rowcount_only`:  
  
    -   **1** : convalida solo mediante conteggio delle righe (impostazione predefinita).    
    -   **2** : convalida mediante conteggio delle righe e checksum binario.  
  
    > [!NOTE]  
    >  Quando si esegue [sp_publication_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md), [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md) viene eseguito per ogni articolo nella pubblicazione. Per eseguire correttamente [sp_publication_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md) è necessario disporre delle autorizzazioni SELECT per tutte le colonne nelle tabelle di base pubblicate.    
2.  (Facoltativo) Avviare l'agente di distribuzione per ogni sottoscrizione, se non è già in esecuzione. Per ulteriori informazioni, vedere [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) e [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).    
3.  Verificare l'output dell'agente per il risultato della convalida. 
  
#### <a name="single-article"></a>Singolo articolo  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md). Specificare `@publication`, il nome dell'articolo per `@article` e uno dei valori seguenti per `@rowcount_only`:  
  
    -   **1** : convalida solo mediante conteggio delle righe (impostazione predefinita).    
    -   **2** : convalida mediante conteggio delle righe e checksum binario.  
  
    > [!NOTE]  
    >  Per eseguire correttamente [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md), è necessario disporre delle autorizzazioni SELECT per tutte le colonne nella tabella di base pubblicata.  
  
2.  (Facoltativo) Avviare l'agente di distribuzione per ogni sottoscrizione, se non è già in esecuzione. Per ulteriori informazioni, vedere [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) e [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).    
3.  Verificare l'output dell'agente per il risultato della convalida.
  
#### <a name="single-subscriber"></a>Singolo sottoscrittore 
  
1.  Nel database di pubblicazione del server di pubblicazione aprire una transazione esplicita usando [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md).    
2.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_marksubscriptionvalidation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-marksubscriptionvalidation-transact-sql.md). Specificare la pubblicazione per `@publication`, il nome del Sottoscrittore per `@subscriber` e il nome del database di sottoscrizione per `@destination_db`.    
3.  (Facoltativo) Ripetere il passaggio 2 per ciascuna sottoscrizione da convalidare.    
4.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md). Specificare `@publication`, il nome dell'articolo per `@article` e uno dei valori seguenti per `@rowcount_only`:    
    -   **1** : convalida solo mediante conteggio delle righe (impostazione predefinita).    
    -   **2** : convalida mediante conteggio delle righe e checksum binario.  
  
    > [!NOTE]  
    >  Per eseguire correttamente [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md), è necessario disporre delle autorizzazioni SELECT per tutte le colonne nella tabella di base pubblicata.  
  
5.  Nel database di pubblicazione del server di pubblicazione eseguire il commit della transazione usando [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md).    
6.  (Facoltativo) Ripetere i passaggi da 1 a 5 per ciascun articolo da convalidare.   
7.  (Facoltativo) Avviare l'agente di distribuzione, se non è già in esecuzione. Per ulteriori informazioni, vedere [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) e [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).    
8.  Verificare l'output dell'agente per il risultato della convalida. Per altre informazioni, vedere [Validate Data at the Subscriber](../../relational-databases/replication/validate-data-at-the-subscriber.md).  

## <a name="all-push-subscriptions-to-a-transactional-publication"></a>Tutte le sottoscrizioni push di una pubblicazione transazionale 

### <a name="using-replication-monitor"></a>Uso di Monitoraggio replica
  
1.  In Monitoraggio replica espandere un gruppo di server di pubblicazione nel riquadro di sinistra e quindi espandere un server di pubblicazione.   
2.  Fare clic con il pulsante destro del mouse sulla pubblicazione di cui si desidera convalidare le sottoscrizioni e quindi scegliere **Convalida sottoscrizioni**.   
3.  Nella finestra di dialogo **Convalida sottoscrizioni** selezionare le sottoscrizioni da convalidare:  
  
    -   Selezionare **Convalida tutte le sottoscrizioni SQL Server**.    
    -   Selezionare **Convalida le sottoscrizioni seguenti** e quindi scegliere una o più sottoscrizioni.    
4.  Per specificare il tipo di convalida da eseguire (solo conteggio delle righe o conteggio delle righe e valori di checksum), fare clic su **Opzioni di convalida** e quindi specificare le opzioni nella finestra di dialogo **Opzioni di convalida delle sottoscrizioni** .    
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
6.  Fare clic sulla scheda **Tutte le sottoscrizioni** .  
7.  Visualizzare i risultati della convalida. Eseguire la procedura seguente per ogni sottoscrizione push:    
    1.  Se l'agente non è in esecuzione, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Avvia sincronizzazione**.    
    2.  Fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Visualizza dettagli**.   
    3.  Visualizzare le informazioni nella scheda **Cronologia server di distribuzione - Sottoscrittore** all'interno dell'area di testo **Azioni nella sessione selezionata** .  
  
## <a name="for-a-single-subscription-to-a-merge-publication"></a>Per una singola sottoscrizione di una pubblicazione di tipo merge

### <a name="using-sql-server-management-studio"></a>Utilizzare SQL Server Management Studio
  
1.  Connettersi al server di pubblicazione in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]e quindi espandere il nodo del server.    
2.  Espandere la cartella **Replica** e quindi la cartella **Pubblicazioni locali** .   
3.  Espandere la pubblicazione di cui si desidera convalidare le sottoscrizioni, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Convalida sottoscrizione**.    
4.  Nella finestra di dialogo **Convalida sottoscrizione** selezionare **Convalida la sottoscrizione**.    
5.  Per specificare il tipo di convalida da eseguire (solo conteggio delle righe o conteggio delle righe e valori di checksum), fare clic su **Opzioni** e quindi specificare le opzioni nella finestra di dialogo **Opzioni di convalida delle sottoscrizioni** .    
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
7.  I risultati della convalida possono essere visualizzati in Monitoraggio replica o nella finestra di dialogo **Visualizza stato sincronizzazione** .  
  
    1.  Espandere la pubblicazione, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Visualizza stato sincronizzazione**.    
    2.  Se l'agente non è in esecuzione, fare clic su **Avvia** nella finestra di dialogo **Visualizza stato sincronizzazione** . Nella finestra di dialogo verranno visualizzati messaggi informativi relativi alla convalida.  
  
     Se non viene visualizzato alcun messaggio attinente, l'agente deve aver già registrato un messaggio successivo. In questo caso, visualizzare i risultati della convalida in Monitoraggio replica. Per ulteriori informazioni, vedere le procedure per Monitoraggio replica in questo argomento.  
  
## <a name="for-all-subscriptions-to-a-merge-publication"></a>Per tutte le sottoscrizioni di una pubblicazione di tipo merge

### <a name="using-sql-server-management-studio"></a>Utilizzare SQL Server Management Studio  
1.  Connettersi al server di pubblicazione in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]e quindi espandere il nodo del server.    
2.  Espandere la cartella **Replica** e quindi la cartella **Pubblicazioni locali** .   
3.  Fare clic con il pulsante destro del mouse sulla pubblicazione di cui si desidera convalidare le sottoscrizioni e quindi scegliere **Convalida tutte le sottoscrizioni**.    
4.  Nella finestra di dialogo **Convalida tutte le sottoscrizioni** specificare il tipo di convalida da eseguire (solo conteggio delle righe o conteggio delle righe e valori di checksum).   
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
6.  I risultati della convalida possono essere visualizzati in Monitoraggio replica o nella finestra di dialogo **Visualizza stato sincronizzazione** . Eseguire la procedura seguente per ogni sottoscrizione:    
    1.  Espandere la pubblicazione, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Visualizza stato sincronizzazione**.    
    2.  Se l'agente non è in esecuzione, fare clic su **Avvia** nella finestra di dialogo **Visualizza stato sincronizzazione** . Nella finestra di dialogo verranno visualizzati messaggi informativi relativi alla convalida.  
  
     Se non viene visualizzato alcun messaggio attinente, l'agente deve aver già registrato un messaggio successivo. In questo caso, visualizzare i risultati della convalida in Monitoraggio replica. Per ulteriori informazioni, vedere le procedure per Monitoraggio replica in questo argomento.  
  
 
## <a name="for-a-single-push-subscription-to-a-merge-publication"></a>Per una singola sottoscrizione push di una pubblicazione di tipo merge 

### <a name="using-replication-monitor"></a>Uso di Monitoraggio replica  
1.  In Monitoraggio replica espandere un gruppo di server di pubblicazione nel riquadro di sinistra, espandere un server di pubblicazione e quindi fare clic su una pubblicazione.    
2.  Fare clic sulla scheda **Tutte le sottoscrizioni** .    
3.  Fare clic con il pulsante destro del mouse sulla sottoscrizione che si desidera convalidare e quindi scegliere **Convalida sottoscrizione**.    
4.  Nella finestra di dialogo **Convalida sottoscrizione** selezionare **Convalida la sottoscrizione**.    
5.  Per specificare il tipo di convalida da eseguire (solo conteggio delle righe o conteggio delle righe e valori di checksum), fare clic su **Opzioni** e quindi specificare le opzioni nella finestra di dialogo **Opzioni di convalida delle sottoscrizioni** .    
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
7.  Fare clic sulla scheda **Tutte le sottoscrizioni** .    
8.  Visualizzare i risultati della convalida:    
    1.  Se l'agente non è in esecuzione, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Avvia sincronizzazione**.    
    2.  Fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Visualizza dettagli**.    
    3.  Visualizzare le informazioni nella scheda **Cronologia sincronizzazione** all'interno dell'area di testo **Ultimo messaggio della sessione selezionata** .  

### <a name="using-transact-sql"></a>Uso di Transact-SQL
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_validatemergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validatemergesubscription-transact-sql.md). Specificare `@publication`, il nome del Sottoscrittore per `@subscriber`, il nome del database di sottoscrizione per `@subscriber_db` e uno dei valori seguenti per `@level`:   
    -   **1** : convalida solo mediante conteggio delle righe.    
    -   **3** : convalida mediante conteggio delle righe e checksum binario.  
  
     La sottoscrizione selezionata viene contrassegnata per la convalida.  
  
2.  Avviare l'agente di merge per ciascuna sottoscrizione. Per ulteriori informazioni, vedere [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) e [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).  
3.  Verificare l'output dell'agente per il risultato della convalida.   
4.  Ripetere i passaggi da 1 a 3 per ciascuna sottoscrizione da convalidare.  
  
> [!NOTE]  
>  È inoltre possibile convalidare una sottoscrizione di una pubblicazione di tipo merge alla fine di una sincronizzazione, specificando il parametro **-Validate** al momento dell'esecuzione di [Replication Merge Agent](../../relational-databases/replication/agents/replication-merge-agent.md).  
  
## <a name="for-all-push-subscriptions-to-a-merge-publication"></a>Per tutte le sottoscrizioni push di una pubblicazione di tipo merge
  
### <a name="using-replication-monitor"></a>Uso di Monitoraggio replica    
1.  In Monitoraggio replica espandere un gruppo di server di pubblicazione nel riquadro di sinistra e quindi espandere un server di pubblicazione.    
2.  Fare clic con il pulsante destro del mouse sulla pubblicazione di cui si desidera convalidare le sottoscrizioni e quindi scegliere **Convalida tutte le sottoscrizioni**.    
3.  Nella finestra di dialogo **Convalida tutte le sottoscrizioni** specificare il tipo di convalida da eseguire (solo conteggio delle righe o conteggio delle righe e valori di checksum).    
4.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
5.  Fare clic sulla scheda **Tutte le sottoscrizioni** .    
6.  Visualizzare i risultati della convalida. Eseguire la procedura seguente per ogni sottoscrizione push:    
    1.  Se l'agente non è in esecuzione, fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Avvia sincronizzazione**.    
    2.  Fare clic con il pulsante destro del mouse sulla sottoscrizione e quindi scegliere **Visualizza dettagli**.    
    3.  Visualizzare le informazioni nella scheda **Cronologia sincronizzazione** all'interno dell'area di testo **Ultimo messaggio della sessione selezionata** . 
  
### <a name="using-transact-sql"></a>Uso di Transact-SQL
1.  (Facoltativo) Nel database di pubblicazione del server di pubblicazione eseguire [sp_validatemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validatemergepublication-transact-sql.md). Specificare `@publication` e uno dei valori seguenti per `@level`:    
    -   **1** : convalida solo mediante conteggio delle righe.   
    -   **3** : convalida mediante conteggio delle righe e checksum binario.  
  
     Tutte le sottoscrizioni vengono contrassegnate per la convalida.   
2.  Avviare l'agente di merge per ciascuna sottoscrizione. Per ulteriori informazioni, vedere [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) e [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
3.  Verificare l'output dell'agente per il risultato della convalida. Per altre informazioni, vedere [Validate Data at the Subscriber](../../relational-databases/replication/validate-data-at-the-subscriber.md).  

  
## <a name="validate-data-using-merge-agent-parameters"></a>Convalidare i dati usando i parametri dell'agente di merge  
  
1.  Avviare l'agente di merge nel Sottoscrittore (sottoscrizione pull) o nel server di distribuzione (sottoscrizione push) dal prompt dei comandi, mediante una delle modalità indicate di seguito.   
    -   Specificare il valore **1** (conteggio delle righe) o **3** (conteggio delle righe e checksum binario) per il parametro **-Validate** .    
    -   Specificare la **convalida mediante conteggio delle righe** o la **convalida mediante conteggio delle righe e checksum** per il parametro **-ProfileName** .  
  
     Per ulteriori informazioni, vedere [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) o [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilizzo di RMO (Replication Management Objects)  
 La replica consente di utilizzare gli oggetti RMO (Replication Management Objects) per convalidare a livello di programmazione la corrispondenza tra i dati nel Sottoscrittore e quelli nel server di pubblicazione. Gli oggetti utilizzati variano in base al tipo di topologia di replica. La replica transazionale richiede la convalida di tutte le sottoscrizioni di una pubblicazione.  
  
> [!NOTE]  
>  Ad esempio, vedere [Esempio (RMO)](#RMOExample)più avanti in questa sezione.  
  
#### <a name="to-validate-data-for-all-articles-in-a-transactional-publication"></a>Per convalidare i dati per tutti gli articoli in una pubblicazione transazionale  
  
1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.TransPublication>. Impostare le proprietà <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> e <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> per la pubblicazione. Impostare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sulla connessione creata nel passaggio 1.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per recuperare le proprietà rimanenti dell'oggetto. Se questo metodo restituisce **false**, le proprietà della pubblicazione sono state definite in modo non corretto nel passaggio 2 oppure la pubblicazione non esiste.  
  
4.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.TransPublication.ValidatePublication%2A> . Passare quanto segue:  
  
    -   <xref:Microsoft.SqlServer.Replication.ValidationOption>  
  
    -   <xref:Microsoft.SqlServer.Replication.ValidationMethod>  
  
    -   Un valore booleano che indica se l'agente di distribuzione deve essere arrestato al termine della convalida.  
  
     Gli articoli vengono contrassegnati per la convalida.  
  
5.  Se non è già in esecuzione, avviare l'agente di distribuzione per sincronizzare ciascuna sottoscrizione. Per ulteriori informazioni, vedere [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md) o [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md). Il risultato dell'operazione di convalida viene scritto nella cronologia dell'agente. Per altre informazioni, vedere [Monitoring Replication](../../relational-databases/replication/monitor/monitoring-replication.md).  
  
#### <a name="to-validate-data-in-all-subscriptions-to-a-merge-publication"></a>Per convalidare i dati in tutte le sottoscrizioni di una pubblicazione di tipo merge  
  
1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergePublication>. Impostare le proprietà <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> e <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> per la pubblicazione. Impostare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sulla connessione creata nel passaggio 1.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per recuperare le proprietà rimanenti dell'oggetto. Se questo metodo restituisce **false**, le proprietà della pubblicazione sono state definite in modo non corretto nel passaggio 2 oppure la pubblicazione non esiste.  
  
4.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.MergePublication.ValidatePublication%2A> . Passare l'oggetto <xref:Microsoft.SqlServer.Replication.ValidationOption>desiderato.  
  
5.  Eseguire l'agente di merge per avviare la convalida in ciascuna sottoscrizione oppure attendere la successiva esecuzione pianificata dell'agente. Per ulteriori informazioni, vedere [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) e [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md). Il risultato dell'operazione di convalida viene scritto nella cronologia dell'agente, visualizzabile tramite Monitoraggio replica. Per altre informazioni, vedere [Monitoring Replication](../../relational-databases/replication/monitor/monitoring-replication.md).  
  
#### <a name="to-validate-data-in-a-single-subscription-to-a-merge-publication"></a>Per convalidare i dati in una singola sottoscrizione di una pubblicazione di tipo merge  
  
1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergePublication>. Impostare le proprietà <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> e <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> per la pubblicazione. Impostare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sulla connessione creata nel passaggio 1.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per recuperare le proprietà rimanenti dell'oggetto. Se questo metodo restituisce **false**, le proprietà della pubblicazione sono state definite in modo non corretto nel passaggio 2 oppure la pubblicazione non esiste.  
  
4.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.MergePublication.ValidateSubscription%2A> . Passare il nome del Sottoscrittore e il database di sottoscrizione da convalidare e l'oggetto <xref:Microsoft.SqlServer.Replication.ValidationOption>desiderato.  
  
5.  Eseguire l'agente di merge per avviare la convalida nella sottoscrizione oppure attendere la successiva esecuzione pianificata dell'agente. Per ulteriori informazioni, vedere [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) e [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md). Il risultato dell'operazione di convalida viene scritto nella cronologia dell'agente, visualizzabile tramite Monitoraggio replica. Per altre informazioni, vedere [Monitoring Replication](../../relational-databases/replication/monitor/monitoring-replication.md).  
  
###  <a name="example-rmo"></a><a name="RMOExample"></a> Esempio (RMO)  
 In questo esempio tutte le sottoscrizioni di una pubblicazione transazionale vengono contrassegnate per la convalida mediante il conteggio delle righe.  
  
 [!code-cs[HowTo#rmo_ValidateTranPub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_validatetranpub)]  
  
 [!code-vb[HowTo#rmo_vb_ValidateTranPub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_validatetranpub)]  
  
 In questo esempio una sottoscrizione specifica di una pubblicazione di tipo merge viene contrassegnata per la convalida mediante il conteggio delle righe.  
  
 [!code-cs[HowTo#rmo_ValidateMergeSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_validatemergesub)]  
  
 [!code-vb[HowTo#rmo_vb_ValidateMergeSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_validatemergesub)]  
  
 ## <a name="see-also"></a>Vedere anche  
[Best Practices for Replication Administration](../../relational-databases/replication/administration/best-practices-for-replication-administration.md)  
  
  
