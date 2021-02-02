---
title: Visualizzare e modificare le proprietà del server di pubblicazione e del database di distribuzione
description: Informazioni su come modificare le proprietà del database di distribuzione e del server di pubblicazione tramite SQL Server Management Studio (SSMS), Transact-SQL (T-SQL) o Replication Management Objects (RMO).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- viewing replication properties
- Distributors [SQL Server replication], modifying
- modifying replication properties, Distributors
- Distributors [SQL Server replication], properties
ms.assetid: 5dae1d59-c377-4c6e-adc9-b68c5b328f79
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: d262e7e8d6599f149afd2ad2b378cfbf8556417a
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250594"
---
# <a name="view-and-modify-distributor-and-publisher-properties"></a>Visualizzazione e modifica delle proprietà del server di pubblicazione e del database di distribuzione
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  In questo argomento viene descritto come visualizzare e modificare le proprietà del database di distribuzione e del server di pubblicazione in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o RMO (Replication Management Objects).  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Indicazioni](#Recommendations)  
  
     [Sicurezza](#Security)  
  
-   **Per visualizzare e modificare le proprietà di server di pubblicazione e database di distribuzione, utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Oggetti RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Per i server di pubblicazione che eseguono versioni precedenti a [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], un utente nel ruolo predefinito del server **sysadmin** può registrare i Sottoscrittori nella pagina **Sottoscrittori**. A partire da [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]non è più necessario registrare esplicitamente i Sottoscrittori per la replica.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
 Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-view-and-modify-distributor-properties"></a>Per visualizzare e modificare le proprietà del database di distribuzione  
  
1.  Connettersi al database di distribuzione in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], quindi espandere il nodo del server.  
  
2.  Fare clic con il pulsante destro del mouse sulla cartella **Replica** e quindi scegliere **Proprietà server di distribuzione**.  
  
3.  Visualizzare e modificare le proprietà nella finestra di dialogo **Proprietà database di distribuzione - \<Distributor>** .  
  
    -   Per visualizzare e modificare le proprietà di un database di distribuzione, fare clic sul pulsante delle proprietà ( **...** ) relativo al database nella pagina **Generale** della finestra di dialogo.  
  
    -   Per visualizzare e modificare le proprietà del server di pubblicazione associato al database di distribuzione, fare clic sul pulsante delle proprietà ( **...** ) relativo al server di pubblicazione nella pagina **Server di pubblicazione** della finestra di dialogo.  
  
    -   Per accedere ai profili degli agenti di replica, fare clic sul pulsante **Impostazioni predefinite profili** nella pagina **Generale** della finestra di dialogo. Per altre informazioni, vedere [Replication Agent Profiles](../../relational-databases/replication/agents/replication-agent-profiles.md).  
  
    -   Per modificare la password dell'account utilizzato quando stored procedure di amministrazione vengono eseguite sul server di pubblicazione e aggiornano le informazioni sul server di distribuzione, immettere una nuova password nelle caselle **Password** e **Conferma password** della pagina **Server di pubblicazione** della finestra di dialogo. Per altre informazioni, vedere [Sicurezza del database di distribuzione](../../relational-databases/replication/security/secure-the-distributor.md).  
  
4.  Se necessario, modificare le proprietà e quindi fare clic su **OK**.  
  
#### <a name="to-view-and-modify-publisher-properties"></a>Per visualizzare e modificare le proprietà del server di pubblicazione  
  
1.  Connettersi al server di pubblicazione in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]e quindi espandere il nodo del server.  
  
2.  Fare clic con il pulsante destro del mouse sulla cartella **Replica** e quindi scegliere **Proprietà server di pubblicazione**.  
  
3.  Visualizzare e modificare le proprietà nella finestra di dialogo **Proprietà server di pubblicazione - <ServerPubblicazione>** .  
  
    -   Un utente nel ruolo predefinito del server **sysadmin** può abilitare i database per la replica nella pagina **Database di pubblicazione** . L'abilitazione di un database non ne comporta la pubblicazione, ma piuttosto consente a un utente nel ruolo predefinito del database **db_owner** per il database in questione di creare una o più pubblicazioni nel database.  
  
4.  Se necessario, modificare le proprietà e quindi fare clic su **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 È possibile visualizzare le proprietà del server di pubblicazione e del database di distribuzione a livello di programmazione utilizzando le stored procedure di replica.  
  
#### <a name="to-view-distributor-and-distribution-database-properties"></a>Per visualizzare le proprietà del database di distribuzione  
  
1.  Eseguire [sp_helpdistributor](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md) per restituire informazioni sul server di distribuzione, il database di distribuzione e la directory di lavoro.  
  
2.  Eseguire [sp_helpdistributiondb](../../relational-databases/system-stored-procedures/sp-helpdistributiondb-transact-sql.md) per restituire le proprietà di un database di distribuzione specificato.  
  
#### <a name="to-change-distributor-and-distribution-database-properties"></a>Per modificare le proprietà del server di distribuzione e del database di distribuzione  
  
1.  Nel server di distribuzione eseguire [sp_changedistributor_property](../../relational-databases/system-stored-procedures/sp-changedistributor-property-transact-sql.md) per modificare le proprietà del server di distribuzione.  
  
2.  Nel server di distribuzione eseguire [sp_changedistributiondb](../../relational-databases/system-stored-procedures/sp-changedistributiondb-transact-sql.md) per modificare le proprietà del database di distribuzione.  
  
3.  Nel database di distribuzione eseguire [sp_changedistributor_password](../../relational-databases/system-stored-procedures/sp-changedistributor-password-transact-sql.md) per modificare la password del database di distribuzione.  
  
    > [!IMPORTANT]  
    >  Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file di script, proteggere tale file per impedire l'accesso non autorizzato.  
  
4.  Nel database di distribuzione eseguire [sp_changedistpublisher](../../relational-databases/system-stored-procedures/sp-changedistpublisher-transact-sql.md) per modificare le proprietà di un server di pubblicazione utilizzando il database di distribuzione.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Esempi (Transact-SQL)  
 Lo script [!INCLUDE[tsql](../../includes/tsql-md.md)] di esempio riportato di seguito restituisce informazioni sul database di distribuzione e sul database di distribuzione.  
  
 [!code-sql[HowTo#sp_helpdistributor](../../relational-databases/replication/codesnippet/tsql/view-and-modify-distribu_1.sql)]  
  
 [!code-sql[HowTo#sp_helpdistributiondb](../../relational-databases/replication/codesnippet/tsql/view-and-modify-distribu_2.sql)]  
  
 In questo esempio vengono modificati i periodi di memorizzazione per il server di distribuzione, la password utilizzata per la connessione al server di distribuzione e l'intervallo con cui il server di distribuzione verifica lo stato di diversi agenti di replica, noto anche come intervallo di heartbeat.  
  
> [!IMPORTANT]  
>  Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali in un file di script, proteggere tale file per impedire l'accesso non autorizzato.  
  
 [!code-sql[HowTo#sp_changedistributor_property](../../relational-databases/replication/codesnippet/tsql/view-and-modify-distribu_3.sql)]  
  
 [!code-sql[HowTo#sp_changedistributiondb](../../relational-databases/replication/codesnippet/tsql/view-and-modify-distribu_4.sql)]  
  
 [!code-sql[HowTo#sp_changedistributor_password](../../relational-databases/replication/codesnippet/tsql/view-and-modify-distribu_5.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilizzo di RMO (Replication Management Objects)  
  
#### <a name="to-view-and-modify-distributor-properties"></a>Per visualizzare e modificare le proprietà del database di distribuzione  
  
1.  Creare una connessione al server di distribuzione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.ReplicationServer>. Passare l'oggetto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> indicato nel passaggio 1.  
  
3.  (Facoltativo) Controllare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationServer.IsDistributor%2A> per verificare che il server attualmente connesso sia un server di distribuzione.  
  
4.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.Load%2A> per ottenere le proprietà dal server.  
  
5.  (Facoltativo) Per modificare le proprietà, specificare un nuovo valore per una o più proprietà del server di distribuzione che è possibile impostare sull'oggetto <xref:Microsoft.SqlServer.Replication.ReplicationServer> .  
  
6.  (Facoltativo) Se la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> sull'oggetto <xref:Microsoft.SqlServer.Replication.ReplicationServer> è impostata su **true**, chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> per eseguire il commit delle modifiche nel server.  
  
#### <a name="to-view-and-modify-distribution-database-properties"></a>Per visualizzare e modificare le proprietà del database di distribuzione  
  
1.  Creare una connessione al server di distribuzione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.DistributionDatabase>. Specificare la proprietà relativa al nome e passare l'oggetto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> indicato nel passaggio 1.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per ottenere le proprietà dal server. Se il metodo restituisce **false**, il database con il nome specificato non esiste nel server.  
  
4.  (Facoltativo) Per modificare le proprietà, specificare un nuovo valore per una delle proprietà dell'oggetto <xref:Microsoft.SqlServer.Replication.DistributionDatabase> che è possibile impostare.  
  
5.  (Facoltativo) Se la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> sull'oggetto <xref:Microsoft.SqlServer.Replication.DistributionDatabase> è impostata su **true**, chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> per eseguire il commit delle modifiche nel server.  
  
#### <a name="to-view-and-modify-publisher-properties"></a>Per visualizzare e modificare le proprietà del server di pubblicazione  
  
1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.DistributionPublisher>. Specificare la proprietà <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Name%2A> e passare l'oggetto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> indicato nel passaggio 1.  
  
3.  (Facoltativo) Per modificare le proprietà, specificare un nuovo valore per una delle proprietà dell'oggetto <xref:Microsoft.SqlServer.Replication.DistributionPublisher> che è possibile impostare.  
  
4.  (Facoltativo) Se la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> sull'oggetto <xref:Microsoft.SqlServer.Replication.DistributionPublisher> è impostata su **true**, chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> per eseguire il commit delle modifiche nel server.  
  
#### <a name="to-change-the-password-for-the-administrative-connection-from-the-publisher-to-the-distributor"></a>Per modificare la password per la connessione amministrativa dal server di pubblicazione al database di distribuzione  
  
1.  Creare una connessione al server di distribuzione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.ReplicationServer>.  
  
3.  Impostare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sulla connessione creata nel passaggio 1.  
  
4.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.Load%2A> per recuperare le proprietà dell'oggetto.  
  
5.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationServer.ChangeDistributorPassword%2A> . Passare il nuovo valore della password per il parametro *password* .  
  
    > [!IMPORTANT]  
    >  Se possibile, richiedere agli utenti di immettere le credenziali di sicurezza in fase di esecuzione. Se è necessario archiviare le credenziali, utilizzare i [servizi di crittografia](/previous-versions/aa719848(v=vs.71)) offerti da [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows .NET Framework.  
  
6.  (Facoltativo) Eseguire i passaggi seguenti per modificare la password in ogni server di pubblicazione remoto che utilizza questo server di distribuzione:  
  
    1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
    2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.ReplicationServer>.  
  
    3.  Impostare la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sulla connessione creata nel passaggio 6a.  
  
    4.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.Load%2A> per recuperare le proprietà dell'oggetto.  
  
    5.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationServer.ChangeDistributorPassword%2A> . Passare il nuovo valore della password indicato nel passaggio 5 per il parametro *password* .  
  
###  <a name="example-rmo"></a><a name="PShellExample"></a> Esempio (RMO)  
 In questo esempio viene illustrato come modificare le proprietà del database di distribuzione e del database di distribuzione.  
  
> [!IMPORTANT]  
>  Per evitare di archiviare le credenziali del codice, la nuova password del server di distribuzione viene specificata in fase di esecuzione.  
  
 [!code-cs[HowTo#rmo_ChangeDistPub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_changedistpub)]  
  
 [!code-vb[HowTo#rmo_vb_ChangeDistPub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_changedistpub)]  
  
## <a name="see-also"></a>Vedere anche  
 [Replication Management Objects Concepts](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Disabilitare la pubblicazione e la distribuzione](../../relational-databases/replication/disable-publishing-and-distribution.md)   
 [Configura distribuzione](../../relational-databases/replication/configure-distribution.md)   
 [Replication Management Objects Concepts](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Script di informazioni su database di distribuzione e server di pubblicazione](../../relational-databases/replication/administration/distributor-and-publisher-information-script.md)   
 [Replication System Stored Procedures Concepts](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Visualizzare le informazioni ed eseguire attività relative a un server di pubblicazione &#40;Monitoraggio replica&#41;](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)  
  
