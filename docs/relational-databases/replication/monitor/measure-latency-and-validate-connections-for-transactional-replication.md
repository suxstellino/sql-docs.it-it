---
title: Misurare la latenza e convalidare le connessioni (transazionale)
description: Informazioni su come misurare la latenza e convalidare le connessioni per una pubblicazione transazionale in SQL Server tramite Monitoraggio replica in SQL Server Management Studio (SSMS), Transact-SQL (T-SQL) o Replication Management Objects (RMO).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Replication Monitor, performance
- tracer tokens [SQL Server replication]
- latency [SQL Server replication]
- transactional replication, tracer tokens
- monitoring performance [SQL Server replication], tracer tokens
ms.assetid: 4addd426-7523-4067-8d7d-ca6bae4c9e34
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 99e2551cef1208a2fe2f0f4c14280fdf2bfe83df
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077159"
---
# <a name="measure-latency-and-validate-connections-for-transactional-replication"></a>Misurazione della latenza e convalida delle connessioni per la replica transazionale
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  In questo argomento viene descritto come misurare le connessione di convalida e latenza per la replica transazionale in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite Monitoraggio replica, [!INCLUDE[tsql](../../../includes/tsql-md.md)]o RMO (Replication Management Objects). La replica transazionale offre la funzionalità token di traccia, che rappresenta una modalità pratica di misurazione della latenza nelle topologie di replica transazionale e di convalida delle connessioni tra server di pubblicazione, database di distribuzione e Sottoscrittori. Un token, ovvero una piccola quantità di dati, viene scritto nel log delle transazioni del database di pubblicazione, contrassegnato come se fosse una comune transazione replicata e inviato tramite il sistema in modo da consentire:  
  
-   Il calcolo del tempo che trascorre tra l'esecuzione del commit di una transazione nel server di pubblicazione e l'inserimento del comando corrispondente nel database di distribuzione del server di distribuzione.  
  
-   Il calcolo del tempo che trascorre tra l'inserimento di un comando nel database di distribuzione e l'esecuzione del commit della transazione corrispondente nel Sottoscrittore.  
  
 I risultati ottenuti da questi calcoli consentono all'utente di rispondere a una serie di domande, incluse le seguenti:  
  
-   Quali Sottoscrittori hanno richiesto più tempo per ricevere una modifica dal server di pubblicazione?  
  
-   Tra i Sottoscrittori destinati a ricevere il token di traccia, quali non lo hanno eventualmente ricevuto?  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
-   **Per misurare la latenza e convalidare le connessioni, utilizzando:**  
  
     [Monitoraggio replica per SQL Server](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [oggetti RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
 I token di traccia possono inoltre essere utili quando si mette un sistema in stato di inattività, il che richiede l'arresto di tutte le attività e la verifica dell'avvenuta ricezione di tutte le modifiche in attesa da parte di tutti i nodi. Per altre informazioni, vedere [Come mettere una topologia di replica in stato di inattività &#40;programmazione Transact-SQL della replica&#41;](../../../relational-databases/replication/administration/quiesce-a-replication-topology-replication-transact-sql-programming.md).  
  
 Per usare i token di traccia, sono necessarie alcune versioni di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]:  
  
-   Il server di distribuzione deve essere [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] o versione successiva.  
  
-   Il server di pubblicazione deve essere [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] o versione successiva, oppure deve essere un server di pubblicazione Oracle.  
  
-   Per le sottoscrizioni push, le statistiche dei token di traccia vengono raccolte dal server di pubblicazione, dal server di distribuzione e dai sottoscrittori se il Sottoscrittore è [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 7.0 o versione successiva.  
  
-   Per le sottoscrizioni pull, le statistiche dei token di traccia vengono raccolte dai soli Sottoscrittori se il Sottoscrittore è [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] o versione successiva. Se il Sottoscrittore è [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 7.0 o [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)], le statistiche vengono raccolte solo dal server di pubblicazione e dal server di distribuzione.  
  
 È inoltre necessario considerare altre problematiche e limitazioni, ovvero:  
  
-   Per poter ricevere un token di traccia, le sottoscrizioni devono essere attive. Una sottoscrizione è attiva se è stata inizializzata.  
  
-   La reinizializzazione elimina i token di traccia in sospeso per le relative sottoscrizioni.  
  
-   I Sottoscrittori ricevono solo i token di traccia creati dopo la sincronizzazione iniziale.  
  
-   I token di traccia non vengono inoltrati dai Sottoscrittori di ripubblicazione.  
  
-   Dopo il failover a un database secondario, Monitoraggio replica non è in grado di regolare il nome dell'istanza di pubblicazione di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] e continuerà a visualizzare informazioni sulla replica con il nome dell'istanza primaria originale di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Dopo il failover, non è possibile immettere un token di traccia tramite Monitoraggio replica, nondimeno un token di traccia immesso nel nuovo server di pubblicazione tramite [!INCLUDE[tsql](../../../includes/tsql-md.md)]è visibile in Monitoraggio replica.  
  
##  <a name="using-sql-server-replication-monitor"></a><a name="SSMSProcedure"></a> Utilizzo di Monitoraggio replica per SQL Server  
 Per informazioni sull'avvio di Monitoraggio replica, vedere [Avviare Monitoraggio replica](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
#### <a name="to-insert-a-tracer-token-and-view-information-on-the-token"></a>Per inserire un token di traccia e visualizzarne le informazioni  
  
1.  Espandere un gruppo di server di pubblicazione nel riquadro sinistro, espandere un server di pubblicazione e quindi fare clic su una pubblicazione.  
  
2.  Fare clic sulla scheda **Token di traccia** .  
  
3.  Fare clic su **Inserisci utilità di traccia**.  
  
4.  Visualizzare il tempo trascorso per il token di traccia nelle colonne **Dal server di pubblicazione al server di distribuzione**, **Dal server di distribuzione al Sottoscrittore** e **Latenza totale**. Il valore **In sospeso** indica che il token non ha raggiunto un determinato punto.  
  
#### <a name="to-view-information-on-a-tracer-token-inserted-previously"></a>Per visualizzare informazioni su un token di traccia inserito in precedenza  
  
1.  Espandere un gruppo di server di pubblicazione nel riquadro sinistro, espandere un server di pubblicazione e quindi fare clic su una pubblicazione.  
  
2.  Fare clic sulla scheda **Token di traccia** .  
  
3.  Selezionare un'ora dall'elenco a discesa **Ora di inserimento** .  
  
4.  Visualizzare il tempo trascorso per il token di traccia nelle colonne **Dal server di pubblicazione al server di distribuzione**, **Dal server di distribuzione al Sottoscrittore** e **Latenza totale**. Il valore **In sospeso** indica che il token non ha raggiunto un determinato punto.  
  
    > [!NOTE]  
    >  Le informazioni sul token di traccia vengono mantenute per lo stesso periodo di tempo degli altri dati cronologici, ovvero in base all'impostazione del periodo di memorizzazione della cronologia del database di distribuzione. Per informazioni sulla modifica delle proprietà del database di distribuzione, vedere [Visualizzare e modificare le proprietà del server di pubblicazione e del database di distribuzione](../../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL  
  
#### <a name="to-post-a-tracer-token-to-a-transactional-publication"></a>Per inviare un token di traccia a una pubblicazione transazionale  
  
1.  (Facoltativo) Nel database di pubblicazione del server di pubblicazione eseguire [sp_helppublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md). Verificare che la pubblicazione esista e che lo stato sia attivo.  
  
2.  (Facoltativo) Nel database di pubblicazione del server di pubblicazione eseguire [sp_helpsubscription &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md). Verificare che la sottoscrizione esista e che lo stato sia attivo.  
  
3.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_posttracertoken &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-posttracertoken-transact-sql.md) specificando **\@publication**. Tenere presente il valore del parametro di output **\@tracer_token_id**.  
  
#### <a name="to-determine-latency-and-validate-connections-for-a-transactional-publication"></a>Per determinare la latenza e convalidare le connessioni per una pubblicazione transazionale  
  
1.  Inviare un token di traccia alla pubblicazione utilizzando la procedura precedente.  
  
2.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_helptracertokens &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helptracertokens-transact-sql.md) specificando **\@publication**. Viene restituito un elenco di tutti i token di traccia inviati alla pubblicazione. Tenere presente il valore di **tracer_id** desiderato nel set di risultati.  
  
3.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_helptracertokenhistory &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helptracertokenhistory-transact-sql.md) specificando **\@publication** e l'ID del token di traccia indicato nel passaggio 2 per **\@tracer_id**. Vengono restituite le informazioni sulla latenza per il token di traccia selezionato.  
  
#### <a name="to-remove-tracer-tokens"></a>Per rimuovere token di traccia  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_helptracertokens &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helptracertokens-transact-sql.md) specificando **\@publication**. Viene restituito un elenco di tutti i token di traccia inviati alla pubblicazione. Tenere presente il valore di **tracer_id** del token di traccia da eliminare nel set di risultati.  
  
2.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_deletetracertokenhistory &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-deletetracertokenhistory-transact-sql.md) specificando **\@publication** e l'ID del token di traccia da eliminare indicato nel passaggio 2 per `@tracer_id`.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Esempio (Transact-SQL)  
 In questo esempio viene inserito un record di token di traccia e viene utilizzato l'ID restituito del token di traccia inviato per visualizzare le informazioni sulla latenza.  
  
 [!code-sql[HowTo#sp_tracertokens](../../../relational-databases/replication/codesnippet/tsql/measure-latency-and-vali_1.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilizzo di RMO (Replication Management Objects)  
  
#### <a name="to-post-a-tracer-token-to-a-transactional-publication"></a>Per inviare un token di traccia a una pubblicazione transazionale  
  
1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.TransPublication>.  
  
3.  Impostare le proprietà <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> e <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> per la pubblicazione, quindi impostare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sulla connessione creata nel passaggio 1.  
  
4.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per recuperare le proprietà dell'oggetto. Se questo metodo restituisce **false**, le proprietà della pubblicazione sono state definite in modo non corretto nel passaggio 3 oppure la pubblicazione non esiste.  
  
5.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.TransPublication.PostTracerToken%2A> . Questo metodo inserisce un token di traccia nel log delle transazioni della pubblicazione.  
  
#### <a name="to-determine-latency-and-validate-connections-for-a-transactional-publication"></a>Per determinare la latenza e convalidare le connessioni per una pubblicazione transazionale  
  
1.  Creare una connessione al server di distribuzione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.PublicationMonitor>.  
  
3.  Impostare le proprietà <xref:Microsoft.SqlServer.Replication.PublicationMonitor.Name%2A>, <xref:Microsoft.SqlServer.Replication.PublicationMonitor.DistributionDBName%2A>, <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublisherName%2A>e <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublicationDBName%2A> , quindi impostare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sulla connessione creata nel passaggio 1.  
  
4.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per recuperare le proprietà dell'oggetto. Se questo metodo restituisce **false**, le proprietà di monitoraggio della pubblicazione sono state definite in modo non corretto nel passaggio 3 oppure la pubblicazione non esiste.  
  
5.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.PublicationMonitor.EnumTracerTokens%2A> . Eseguire il cast dell'oggetto <xref:System.Collections.ArrayList> restituito in una matrice di oggetti <xref:Microsoft.SqlServer.Replication.TracerToken> .  
  
6.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.PublicationMonitor.EnumTracerTokenHistory%2A> . Passare un valore di <xref:Microsoft.SqlServer.Replication.TracerToken.TracerTokenId%2A> per un token di traccia indicato nel passaggio 5. Le informazioni sulla latenza per il token di traccia selezionato vengono restituite come oggetto <xref:System.Data.DataSet> . Se vengono restituite tutte le informazioni sul token di traccia, la connessione tra server di pubblicazione e server di distribuzione e la connessione tra server di distribuzione e Sottoscrittore esistono e la topologia di replica funziona correttamente.  
  
#### <a name="to-remove-tracer-tokens"></a>Per rimuovere token di traccia  
  
1.  Creare una connessione al server di distribuzione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.PublicationMonitor>.  
  
3.  Impostare le proprietà <xref:Microsoft.SqlServer.Replication.PublicationMonitor.Name%2A>, <xref:Microsoft.SqlServer.Replication.PublicationMonitor.DistributionDBName%2A>, <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublisherName%2A>e <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublicationDBName%2A> , quindi impostare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sulla connessione creata nel passaggio 1.  
  
4.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per recuperare le proprietà dell'oggetto. Se questo metodo restituisce **false**, le proprietà di monitoraggio della pubblicazione sono state definite in modo non corretto nel passaggio 3 oppure la pubblicazione non esiste.  
  
5.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.PublicationMonitor.EnumTracerTokens%2A> . Eseguire il cast dell'oggetto <xref:System.Collections.ArrayList> restituito in una matrice di oggetti <xref:Microsoft.SqlServer.Replication.TracerToken> .  
  
6.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.PublicationMonitor.CleanUpTracerTokenHistory%2A> . Passare uno dei valori seguenti:  
  
    -   La proprietà <xref:Microsoft.SqlServer.Replication.TracerToken.TracerTokenId%2A> per un token di traccia indicato al passaggio 5. Vengono eliminate le informazioni per un token selezionato.  
  
    -   Oggetto <xref:System.DateTime> . Vengono eliminate le informazioni per tutti i token con data e ora precedenti a quelle specificate.  
  
  
