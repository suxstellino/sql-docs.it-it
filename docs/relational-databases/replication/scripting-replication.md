---
title: Creazione di script di replica | Microsoft Docs
description: Uno script contiene le stored procedure di sistema Transact-SQL necessarie per l'implementazione dei componenti di replica di SQL Server, ad esempio una pubblicazione o una sottoscrizione.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- scripts [SQL Server replication], replication objects
- merge replication scripting [SQL Server replication]
- replication [SQL Server], scripting
- snapshot replication [SQL Server], scripting
- scripts [SQL Server replication]
- transactional replication, scripting
ms.assetid: e50fac44-54c0-470c-a4ea-9c111fa4322b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 160066798b04b16e82ed1903555b6981323a4dcc
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468792"
---
# <a name="scripting-replication"></a>Creazione di script di replica
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Gli script di tutti i componenti di replica inclusi in una topologia devono essere creati come parte di un piano di ripristino di emergenza. Gli script possono inoltre essere utilizzati per automatizzare attività ripetitive. Uno script contiene le stored procedure di sistema Transact-SQL necessarie per l'implementazione dei componenti di replica, ad esempio una pubblicazione o una sottoscrizione. È possibile creare gli script tramite una procedura guidata, come la Creazione guidata nuova pubblicazione, o in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] dopo aver creato un componente. È possibile visualizzare, modificare ed eseguire lo script utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o **sqlcmd**. Gli script possono essere memorizzati con file di backup da utilizzare nel caso in cui sia necessario riconfigurare una topologia di replica.  
  
 È necessario creare un nuovo script per un componente se vengono apportate modifiche alle proprietà. Se si utilizzano stored procedure personalizzate con la replica transazionale, è consigliabile archiviare una copia di ogni procedura con gli script, aggiornando la copia in caso di modifica della procedura. Le procedure vengono in genere aggiornate in seguito a modifiche dello schema o a nuove esigenze applicative. Per altre informazioni sulle procedure personalizzate, vedere [Specificare la modalità di propagazione delle modifiche per gli articoli transazionali](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).  
  
 Per le pubblicazioni di tipo merge che utilizzano filtri con parametri, gli script di pubblicazione contengono le chiamate alle stored procedure per la creazione di partizioni dei dati. Lo script offre un riferimento per le partizioni create e un modo per ricreare, se necessario, una o più partizioni.  
  
## <a name="example-of-automating-a-task-with-scripts"></a>Esempio di automazione di un'attività tramite script  
 Si consideri [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)], che implementa la replica di tipo merge per distribuire dati alla forza vendita remota. Una rappresentante scarica tutti i dati relativi ai clienti nella propria zona utilizzando sottoscrizioni pull. Lavorando offline, la rappresentante aggiorna i dati e immette nuovi clienti e ordini. Poiché [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] dispone di oltre cinquanta rappresentanti in zone diverse, la creazione delle diverse sottoscrizioni in ogni Sottoscrittore mediante la Creazione guidata nuova sottoscrizione richiederebbe una notevole quantità di tempo. L'amministratore della replica può invece eseguire la procedura seguente:  
  
1.  Impostare le pubblicazioni di tipo merge necessarie con partizioni basate sul rappresentante o sulla zona.  
  
2.  Creare una sottoscrizione pull per un Sottoscrittore.  
  
3.  Generare uno script basato su tale sottoscrizione pull.  
  
4.  Modificare lo script, modificando valori come il nome del Sottoscrittore.  
  
5.  Eseguire lo script in più Sottoscrittori per generare le sottoscrizioni pull necessarie.  
  
## <a name="script-replication-objects"></a>Creazione di script per gli oggetti di replica  
 Creare script per gli oggetti di replica tramite le procedure guidate per la replica o dalla cartella **Replica** in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Se si utilizzano le procedure guidate, è possibile scegliere di creare oggetti e includerli in script o solo di includerli in script.  
  
> [!IMPORTANT]  
>  Negli script tutte le password sono impostate come NULL. Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se si archiviano le credenziali in un file di script, è necessario proteggere il file per impedire l'accesso non autorizzato.  
  
 Per ulteriori informazioni sull'utilizzo delle procedure guidate di replica, vedere:  
  
-   [Configurare la pubblicazione e la distribuzione](../../relational-databases/replication/configure-publishing-and-distribution.md)  
  
-   [Creare una pubblicazione](../../relational-databases/replication/publish/create-a-publication.md)  
  
-   [Creare una sottoscrizione push](../../relational-databases/replication/create-a-push-subscription.md)  
  
-   [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)  
  
#### <a name="to-script-an-object-from-a-replication-wizard"></a>Per creare script per un oggetto da una procedura guidata di replica  
  
1.  Nella pagina **Azioni procedura guidata** della procedura guidata selezionare la casella di controllo appropriata:  
  
    -   **Genera un file script con i passaggi per la creazione della pubblicazione**  
  
    -   **Genera un file script con i passaggi per la creazione delle sottoscrizioni**  
  
    -   **Genera un file script con i passaggi per la configurazione della distribuzione**  
  
2.  Specificare le opzioni nella pagina **Proprietà file script** .  
  
3.  Completare la procedura guidata.  
  
#### <a name="to-script-an-object-from-management-studio"></a>Per creare script per un oggetto da Management Studio  
  
1.  Connettersi al server di distribuzione, al server di pubblicazione o al Sottoscrittore in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]e quindi espandere il nodo del server.  
  
2.  Espandere la cartella **Replica** e quindi la cartella **Pubblicazioni locali** o la cartella **Sottoscrizioni locali** .  
  
3.  Fare clic con il pulsante destro del mouse su una pubblicazione o una sottoscrizione e quindi scegliere **Genera script**.  
  
4.  Specificare le opzioni nella finestra di dialogo **Genera script SQL - \<ReplicationObject>** .  
  
5.  Fare clic su **Genera script nel file**.  
  
6.  Immettere un nome di file nella finestra di dialogo **Percorso file script** e quindi fare clic su **Salva**. Viene visualizzato un messaggio di stato.  
  
7.  Fare clic su **OK**, quindi su **Chiudi**.  
  
#### <a name="to-script-multiple-objects-from-management-studio"></a>Per creare script per più oggetti da Management Studio  
  
1.  Connettersi al server di distribuzione, al server di pubblicazione o al Sottoscrittore in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]e quindi espandere il nodo del server.  
  
2.  Fare clic con il pulsante destro del mouse sulla cartella **Replica** e quindi scegliere **Genera script**.  
  
3.  Specificare le opzioni nella finestra di dialogo **Genera script SQL** .  
  
4.  Fare clic su **Genera script nel file**.  
  
5.  Immettere un nome di file nella finestra di dialogo **Percorso file script** e quindi fare clic su **Salva**. Viene visualizzato un messaggio di stato.  
  
6.  Fare clic su **OK** e quindi su **Chiudi**.  
  
  
