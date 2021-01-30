---
title: Configurare un gestore della logica di business per un articolo di merge
description: Usare la programmazione della replica o Replication Management Objects per configurare un gestore della logica di business per la sincronizzazione della replica di tipo merge.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- merge replication conflict resolution [SQL Server replication], business logic handlers
- merge replication business logic handlers [SQL Server replication]
- conflict resolution [SQL Server replication], merge replication
- business logic handlers [SQL Server replication]
- BusinessLogicModule class
ms.assetid: ed477595-6d46-4fa2-b0d3-a5358903ec05
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 45ed99072fb5abad47b9f63a22f879c0f4e438eb
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076829"
---
# <a name="implement-a-business-logic-handler-for-a-merge-article"></a>Implementazione di un gestore della logica di business per un articolo di merge
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come implementare un gestore della logica di business per un articolo di tipo merge in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite la programmazione della replica o RMO (Replication Management Objects).  
  
 Lo spazio dei nomi <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport> implementa un'interfaccia che consente di scrivere logica di business complessa per gestire gli eventi che si verificano durante il processo di sincronizzazione della replica di tipo merge. I metodi del gestore della logica di business possono essere richiamati dal processo di replica per ogni riga modificata che viene replicata durante la sincronizzazione.  
  
 Il processo generale per l'implementazione di un gestore della logica di business è il seguente:  
  
1.  Creazione dell'assembly del gestore della logica di business.  
  
2.  Registrazione dell'assembly nel server di distribuzione.  
  
3.  Distribuzione dell'assembly nel server in cui viene eseguito l'agente di merge. Per una sottoscrizione pull, l'agente viene eseguito nel Sottoscrittore, mentre per una sottoscrizione push nel server di distribuzione. Quando si utilizza la sincronizzazione tramite il Web, l'agente viene eseguito nel server Web.  
  
4.  Creazione di un articolo che utilizza il gestore della logica di business o modifica di un articolo esistente per l'utilizzo del gestore della logica di business.  
  
 Il gestore della logica di business specificato viene eseguito per ogni riga sincronizzata. La complessità della logica e le chiamate ad altre applicazioni o servizi di rete possono ridurre le prestazioni. Per altre informazioni sui gestori della logica di business, vedere [Eseguire logiche di business durante la sincronizzazione di tipo merge](../../relational-databases/replication/merge/execute-business-logic-during-merge-synchronization.md).  
  
 **Contenuto dell'articolo**  
  
-   **Per implementare un gestore della logica di business per un articolo di merge, utilizzando:**  
  
     [Programmazione della replica](#ReplProg)  
  
     [Oggetti RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="using-replication-programming"></a><a name="ReplProg"></a> Utilizzo della programmazione della replica  
  
#### <a name="to-create-and-deploy-a-business-logic-handler"></a>Per creare e distribuire un gestore della logica di business  
  
1.  In [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Studio creare un nuovo progetto per l'assembly .NET contenente il codice che implementa il gestore della logica di business.  
  
2.  Aggiungere i riferimenti al progetto per gli spazi dei nomi seguenti.  
  
    |Riferimento all'assembly|Location|  
    |------------------------|--------------|  
    |<xref:Microsoft.SqlServer.Replication.BusinessLogicSupport>|[!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]COM (installazione predefinita)|  
    |<xref:System.Data>|GAC (componente di .NET Framework)|  
    |<xref:System.Data.Common>|GAC (componente di .NET Framework)|  
  
3.  Aggiungere una classe che esegue l'override della classe <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> .  
  
4.  Implementare la proprietà <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.HandledChangeStates%2A> per indicare i tipi di modifiche gestite.  
  
5.  Eseguire l'override di uno o più dei seguenti metodi della classe <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> :  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.CommitHandler%2A> : viene richiamato quando si esegue il commit di una modifica ai dati durante la sincronizzazione.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.DeleteErrorHandler%2A> : viene richiamato se si verifica un errore durante il caricamento o il download di un'istruzione DELETE.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.DeleteHandler%2A> : viene richiamato durante il caricamento o il download delle istruzioni DELETE.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.InsertErrorHandler%2A> : viene richiamato se si verifica un errore durante il caricamento o il download di un'istruzione INSERT.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.InsertHandler%2A> : viene richiamato durante il caricamento o il download delle istruzioni INSERT.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateConflictsHandler%2A> : viene richiamato quando si verificano conflitti di istruzioni UPDATE nel server di pubblicazione e nel Sottoscrittore.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateDeleteConflictHandler%2A> : viene richiamato quando si verificano conflitti tra istruzioni UPDATE e istruzioni DELETE nel server di pubblicazione e nel Sottoscrittore.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateErrorHandler%2A> : viene richiamato se si verifica un errore durante il caricamento o il download di un'istruzione UPDATE.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateHandler%2A> : viene richiamato durante il caricamento o il download delle istruzioni UPDATE.  
  
6.  Compilare il progetto per creare l'assembly del gestore della logica di business.  
  
7.  Distribuire l'assembly nella directory che contiene il file eseguibile dell'agente di merge (replmerg.exe), che per un'installazione predefinita è [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]COM, o installarlo nella Global Assembly Cache (GAC) .NET. È necessario installare l'assembly nella GAC solo se applicazioni diverse dall'agente di merge richiedono l'accesso all'assembly. L'assembly può essere installato nella GAC tramite lo strumento Global Assembly Cache (**Gacutil.exe)** disponibile in .NET Framework SDK.  
  
    > [!NOTE]  
    >  È necessario distribuire un gestore della logica di business su ogni server in cui viene eseguito l'agente di merge, incluso il server IIS che ospita il file replisapi.dll quando si utilizza la sincronizzazione tramite il Web.  
  
#### <a name="to-register-a-business-logic-handler"></a>Per registrare un gestore della logica di business  
  
1.  Nel server di pubblicazione eseguire [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md) per verificare che l'assembly non sia già stato registrato come gestore della logica di business.  
  
2.  Nel server di distribuzione eseguire [sp_registercustomresolver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-registercustomresolver-transact-sql.md), specificando un nome descrittivo per il gestore della logica di business per **\@article_resolver**, il valore **true** per **\@is_dotnet_assembly**, il nome dell'assembly per **\@dotnet_assembly_name** e il nome completo della classe che esegue l'override di <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> per **\@dotnet_class_name**.  
  
    > [!NOTE]  
    >  Se l'assembly non viene distribuito nella stessa directory dell'eseguibile dell'agente di merge, nella stessa directory dell'applicazione che avvia in modo sincrono l'agente di merge o nella Global Assembly Cache (GAC), è necessario specificare il percorso completo con il nome dell'assembly per **\@dotnet_assembly_name**. Quando si utilizza la sincronizzazione tramite il Web, è necessario specificare il percorso dell'assembly nel server Web.  
  
#### <a name="to-use-a-business-logic-handler-with-a-new-table-article"></a>Per utilizzare un gestore della logica di business con un nuovo articolo di tabella  
  
1.  Eseguire [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) per definire l'articolo, specificando il nome descrittivo del gestore della logica di business per **\@article_resolver**. Per altre informazioni, vedere [definire un articolo](../../relational-databases/replication/publish/define-an-article.md).  
  
#### <a name="to-use-a-business-logic-handler-with-an-existing-table-article"></a>Per utilizzare un gestore della logica di business con un articolo di tabella esistente  
  
1.  Eseguire [sp_changemergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md), specificando **\@publication**, **\@article**, il valore **article_resolver** per **\@property** e il nome descrittivo del gestore della logica di business per **\@value**.  
  
###  <a name="examples-replication-programming"></a><a name="TsqlExample"></a> Esempi (programmazione della replica)  
 In questo esempio è illustrato un gestore della logica di business che crea un log di controllo.  
  
 [!code-cs[HowTo#rmo_BusinessLogicCode](../../relational-databases/replication/codesnippet/csharp/rmohowto/businesslogic.cs#rmo_businesslogiccode)]  
  
 [!code-vb[HowTo#rmo_vb_BusinessLogicCode](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/businesslogic.vb#rmo_vb_businesslogiccode)]  
  
 Nell'esempio seguente viene registrato un assembly del gestore della logica di business nel database di distribuzione e viene modificato un articolo di merge esistente per l'utilizzo di questa logica di business personalizzata.  
  
 [!code-sql[HowTo#sp_RegisterBLH_10](../../relational-databases/replication/codesnippet/tsql/implement-a-business-log_3.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilizzo di RMO (Replication Management Objects)  
  
#### <a name="to-create-a-business-logic-handler"></a>Per creare un gestore della logica di business  
  
1.  In [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Studio creare un nuovo progetto per l'assembly .NET contenente il codice che implementa il gestore della logica di business.  
  
2.  Aggiungere i riferimenti al progetto per gli spazi dei nomi seguenti.  
  
    |Riferimento all'assembly|Location|  
    |------------------------|--------------|  
    |<xref:Microsoft.SqlServer.Replication.BusinessLogicSupport>|[!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]COM (installazione predefinita)|  
    |<xref:System.Data>|GAC (componente di .NET Framework)|  
    |<xref:System.Data.Common>|GAC (componente di .NET Framework)|  
  
3.  Aggiungere una classe che esegue l'override della classe <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> .  
  
4.  Implementare la proprietà <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.HandledChangeStates%2A> per indicare i tipi di modifiche gestite.  
  
5.  Eseguire l'override di uno o più dei seguenti metodi della classe <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> :  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.CommitHandler%2A> : viene richiamato quando si esegue il commit di una modifica ai dati durante la sincronizzazione.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.DeleteErrorHandler%2A> : viene richiamato se si verifica un errore durante il caricamento o il download di un'istruzione DELETE.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.DeleteHandler%2A> : viene richiamato durante il caricamento o il download delle istruzioni DELETE.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.InsertErrorHandler%2A> : viene richiamato se si verifica un errore durante il caricamento o il download di un'istruzione INSERT.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.InsertHandler%2A> : viene richiamato durante il caricamento o il download delle istruzioni INSERT.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateConflictsHandler%2A> : viene richiamato quando si verificano conflitti di istruzioni UPDATE nel server di pubblicazione e nel Sottoscrittore.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateDeleteConflictHandler%2A> : viene richiamato quando si verificano conflitti tra istruzioni UPDATE e istruzioni DELETE nel server di pubblicazione e nel Sottoscrittore.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateErrorHandler%2A> : viene richiamato se si verifica un errore durante il caricamento o il download di un'istruzione UPDATE.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateHandler%2A> : viene richiamato durante il caricamento o il download delle istruzioni UPDATE.  
  
    > [!NOTE]  
    >  I conflitti di articoli non gestiti in modo esplicito dalla logica di business personalizzata vengono gestiti dal sistema di risoluzione predefinito per l'articolo.  
  
6.  Compilare il progetto per creare l'assembly del gestore della logica di business.  
  
#### <a name="to-register-a-business-logic-handler"></a>Per registrare un gestore della logica di business  
  
1.  Creare una connessione al server di distribuzione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.ReplicationServer>. Passare il valore di <xref:Microsoft.SqlServer.Management.Common.ServerConnection> ottenuto al passaggio 1.  
  
3.  Chiamare <xref:Microsoft.SqlServer.Replication.ReplicationServer.EnumBusinessLogicHandlers%2A> e controllare l'oggetto <xref:System.Collections.ArrayList> restituito per verificare che l'assembly non sia già stato registrato come gestore della logica di business.  
  
4.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler>. Specificare le proprietà seguenti:  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.DotNetAssemblyName%2A> : nome dell'assembly .NET. Se l'assembly non viene distribuito nella stessa directory del file eseguibile dell'agente di merge, nella stessa directory dell'applicazione che avvia in modo sincrono l'agente di merge o nella GAC (Global Assembly Cache), è necessario includere il percorso completo con il nome dell'assembly. È necessario includere il percorso completo con il nome dell'assembly quando si utilizza un gestore della logica di business con la sincronizzazione tramite il Web.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.DotNetClassName%2A> : nome completo della classe che esegue l'override di <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> e implementa il gestore della logica di business.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.FriendlyName%2A> : nome descrittivo utilizzato per l'accesso al gestore della logica di business.  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.IsDotNetAssembly%2A> : valore **true**.  
  
#### <a name="to-deploy-a-business-logic-handler"></a>Per distribuire un gestore della logica di business  
  
1.  Distribuire l'assembly nel server in cui viene eseguito l'agente merge, nel percorso file specificato durante la registrazione del gestore della logica di business nel server di distribuzione. Per una sottoscrizione pull, l'agente viene eseguito nel Sottoscrittore, mentre per una sottoscrizione push nel server di distribuzione. Quando si utilizza la sincronizzazione tramite il Web, l'agente viene eseguito nel server Web. Se con il nome dell'assembly non è stato incluso il percorso completo durante la registrazione del gestore della logica di business, distribuire l'assembly nella stessa directory del file eseguibile dell'agente di merge o nella stessa directory dell'applicazione che avvia in modo sincrono l'agente di merge. È possibile installare l'assembly nella GAC se più applicazioni utilizzano lo stesso assembly.  
  
#### <a name="to-use-a-business-logic-handler-with-a-new-table-article"></a>Per utilizzare un gestore della logica di business con un nuovo articolo di tabella  
  
1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergeArticle>. Impostare le proprietà seguenti:  
  
    -   Nome dell'articolo per <xref:Microsoft.SqlServer.Replication.Article.Name%2A>.  
  
    -   Nome della pubblicazione per <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>.  
  
    -   Nome del database di pubblicazione per <xref:Microsoft.SqlServer.Replication.Article.DatabaseName%2A>.  
  
    -   Nome descrittivo del gestore della logica di business (<xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.FriendlyName%2A>) per <xref:Microsoft.SqlServer.Replication.MergeArticle.ArticleResolver%2A>.  
  
3.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.Article.Create%2A> . Per altre informazioni, vedere [definire un articolo](../../relational-databases/replication/publish/define-an-article.md).  
  
#### <a name="to-use-a-business-logic-handler-with-an-existing-table-article"></a>Per utilizzare un gestore della logica di business con un articolo di tabella esistente  
  
1.  Creare una connessione al server di pubblicazione tramite la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Creare un'istanza della classe <xref:Microsoft.SqlServer.Replication.MergeArticle>.  
  
3.  Impostare le proprietà <xref:Microsoft.SqlServer.Replication.Article.Name%2A>, <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>e <xref:Microsoft.SqlServer.Replication.Article.DatabaseName%2A> .  
  
4.  Impostare la connessione del passaggio 1 per la proprietà <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Chiamare il metodo <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> per recuperare le proprietà dell'oggetto. Se questo metodo restituisce **false**, le proprietà dell'articolo sono state definite in modo non corretto nel passaggio 3 oppure l'articolo non esiste. Per altre informazioni, vedere [View and Modify Article Properties](../../relational-databases/replication/publish/view-and-modify-article-properties.md).  
  
6.  Impostare il nome descrittivo del gestore della logica di business per <xref:Microsoft.SqlServer.Replication.MergeArticle.ArticleResolver%2A>. Si tratta del valore della proprietà <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.FriendlyName%2A> specificato durante la registrazione del gestore della logica di business.  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Esempi (RMO)  
 In questo esempio è illustrato un gestore della logica di business che registra informazioni sulle operazioni di inserimento, aggiornamento ed eliminazione nel Sottoscrittore.  
  
 [!code-cs[HowTo#rmo_BusinessLogicCode](../../relational-databases/replication/codesnippet/csharp/rmohowto/businesslogic.cs#rmo_businesslogiccode)]  
  
 [!code-vb[HowTo#rmo_vb_BusinessLogicCode](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/businesslogic.vb#rmo_vb_businesslogiccode)]  
  
 In questo esempio viene registrato un gestore della logica di business nel server di distribuzione.  
  
 [!code-cs[HowTo#rmo_RegisterBLH_10](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_registerblh_10)]  
  
 [!code-vb[HowTo#rmo_vb_RegisterBLH_10](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_registerblh_10)]  
  
 In questo esempio viene modificato un articolo esistente per l'utilizzo del gestore della logica di business.  
  
 [!code-cs[HowTo#rmo_ChangeMergeArticle_BLH](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_changemergearticle_blh)]  
  
 [!code-vb[HowTo#rmo_vb_ChangeMergeArticle_BLH](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_changemergearticle_blh)]  
  
## <a name="see-also"></a>Vedere anche  
 [Implementare un sistema di risoluzione dei conflitti personalizzato per un articolo di tipo merge](../../relational-databases/replication/implement-a-custom-conflict-resolver-for-a-merge-article.md)   
 [Eseguire il debug di un gestore della logica di business &#40;programmazione della replica&#41;](../../relational-databases/replication/debug-a-business-logic-handler-replication-programming.md)   
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Concetti di base relativi a RMO (Replication Management Objects)](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)  
  
  
