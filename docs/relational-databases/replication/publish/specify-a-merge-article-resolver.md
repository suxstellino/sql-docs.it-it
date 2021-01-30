---
description: Impostazione di un sistema di risoluzione dei conflitti dell'articolo di merge
title: Specificare un sistema di risoluzione dei conflitti dell'articolo di merge | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- articles [SQL Server replication], conflict resolution
- conflict resolution [SQL Server replication], merge replication
- merge replication conflict resolution [SQL Server replication], merge article resolvers
ms.assetid: a40083b3-4f7b-4a25-a5a3-6ef67bdff440
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 8299968a0c5176aae4cf00b5a69619ed96a56d80
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076639"
---
# <a name="specify-a-merge-article-resolver"></a>Impostazione di un sistema di risoluzione dei conflitti dell'articolo di merge
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come specificare un sistema di risoluzione dell'articolo di merge in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)].  

  
##  <a name="recommendations"></a>Consigli  
  
-   La replica di tipo merge consente i tipi di sistemi di risoluzione dei conflitti dell'articolo indicati di seguito:  
  
    -   Il sistema di risoluzione dei conflitti predefinito. Il comportamento del sistema di risoluzione dei conflitti predefinito dipende dal tipo di sottoscrizione, ovvero se si tratta di una sottoscrizione client o server. Per altre informazioni sulla specifica del tipo di sottoscrizione, vedere [Specificare una sottoscrizione di tipo merge e la priorità per la risoluzione dei conflitti &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/specify-a-merge-subscription-type-and-conflict-resolution-priority.md).  
  
    -   Un sistema di risoluzione dei conflitti personalizzato, che può essere un gestore della logica di business (scritto in codice gestito) oppure un sistema di risoluzione dei conflitti personalizzato basato su COM. Per altre informazioni, vedere [Rilevamento e risoluzione avanzati dei conflitti nella replica di tipo merge](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md). Se è necessario implementare logica personalizzata che viene eseguita per ogni riga replicata e non solo per quelle in conflitto, vedere [Implementare un gestore della logica di business per un articolo di merge](../../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md).  
  
    -   Un sistema di risoluzione dei conflitti standard basato su COM, incluso in [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   Per utilizzare un sistema di risoluzione dei conflitti diverso da quello predefinito, è necessario copiare il sistema desiderato nel computer in cui è in esecuzione l'agente di merge e registrarlo. Se si utilizza un gestore della logica di business, è necessario eseguire la registrazione anche nel server di pubblicazione. L'agente di merge viene eseguito nei sistemi seguenti:  
  
    -   Server di distribuzione per una sottoscrizione push  
  
    -   Sottoscrittore per una sottoscrizione pull  
  
    -   Server [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Internet Information Services (IIS) per una sottoscrizione pull che utilizza la sincronizzazione Web  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 In seguito alla registrazione del sistema di risoluzione dei conflitti, specificare che è necessario che un articolo usi il sistema nella scheda **Sistema di risoluzione** della finestra di dialogo **Proprietà articolo - \<Article>** , disponibile nella Creazione guidata nuova pubblicazione e nella finestra di dialogo **Proprietà pubblicazione - \<Publication>** . Per altre informazioni sull'uso della creazione guidata e l'accesso alla finestra di dialogo, vedere [Creare una pubblicazione](../../../relational-databases/replication/publish/create-a-publication.md) e [Visualizzare e modificare le proprietà della pubblicazione](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-specify-a-resolver"></a>Per specificare un sistema di risoluzione dei conflitti  
  
1.  Selezionare una tabella nella pagina **Articoli** della Creazione guidata nuova pubblicazione o nella finestra di dialogo **Proprietà pubblicazione - \<Publication>** .  
  
2.  Fare clic su **Proprietà articolo** e quindi su **Imposta proprietà dell'articolo di tabella evidenziato**.  
  
3.  Nella pagina **Proprietà articolo - \<Article>** fare clic sulla scheda **Sistema di risoluzione**.  
  
4.  Selezionare **Usa un sistema di risoluzione personalizzato (registrato nel server di distribuzione)** e quindi fare clic sul sistema di risoluzione nell'elenco.  
  
5.  Se il sistema di risoluzione dei conflitti richiede un input, ad esempio un nome di colonna, specificarlo nella casella di testo **Immettere le informazioni necessarie per il sistema di risoluzione** .  
  
6.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
7.  Ripetere questa procedura per ogni articolo che richiede un sistema di risoluzione dei conflitti.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-register-a-custom-conflict-resolver"></a>Per registrare un sistema di risoluzione dei conflitti personalizzato  
  
1.  Se si intende registrare un sistema di risoluzione dei conflitti personalizzato, creare uno dei tipi seguenti:  
  
    -   Sistema di risoluzione basato su codice gestito come gestore della logica di business. Per altre informazioni, vedere [Implementare un gestore della logica di business per un articolo di merge](../../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md).  
  
    -   Sistema di risoluzione basato sulla stored procedure e sistema di risoluzione basato su COM. Per altre informazioni, vedere [Implementare un sistema di risoluzione dei conflitti personalizzato per un articolo di tipo merge](../../../relational-databases/replication/implement-a-custom-conflict-resolver-for-a-merge-article.md)  
  
2.  Per determinare se il sistema di risoluzione desiderato è già registrato, eseguire [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md) in qualsiasi database nel server di pubblicazione. Verrà visualizzata una descrizione del sistema di risoluzione personalizzato, nonché l'identificatore di classe (CLSID) di ogni sistema di risoluzione basato su COM registrato nel server di distribuzione oppure informazioni sull'assembly gestito per ogni gestore della logica di business registrato nel server di distribuzione.  
  
3.  Se il sistema di risoluzione desiderato non è già registrato, eseguire [sp_registercustomresolver &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-registercustomresolver-transact-sql.md) nel database di distribuzione. Specificare un nome per il sistema di risoluzione per **\@article_resolver**. Nel caso di un gestore della logica di business, si tratta del nome descrittivo dell'assembly. Per i sistemi di risoluzione basati su COM, specificare il CLSID della DLL per **\@resolver_clsid**, mentre per un gestore della logica di business specificare il valore **true** per **\@is_dotnet_assembly**, il nome dell'assembly per **\@dotnet_assembly_name** e il nome completo della classe che sostituisce <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> per **\@dotnet_class_name**.  
  
    > [!NOTE]  
    >  Se un assembly del gestore della logica di business non viene distribuito nella stessa directory del file eseguibile dell'agente di merge, è necessario specificare il percorso completo indicando il nome dell'assembly per **\@dotnet_assembly_name** nella stessa directory dell'applicazione che avvia in modo sincrono l'agente di merge o nella Global Assembly Cache (GAC).  
  
4.  Se il sistema di risoluzione è basato su COM:  
  
    -   Copiare la DLL del sistema di risoluzione personalizzato nel server di distribuzione per le sottoscrizioni push o nel Sottoscrittore per le sottoscrizioni pull.  
  
        > [!NOTE]  
        >  I sistemi di risoluzione personalizzati[!INCLUDE[msCoName](../../../includes/msconame-md.md)] sono disponibili nella directory [!INCLUDE[ssInstallPath](../../../includes/ssinstallpath-md.md)]COM.  
  
    -   Utilizzare regsvr32.exe per registrare la DLL del sistema di risoluzione personalizzato con il sistema operativo. Ad esempio, eseguire il seguente comando dal prompt dei comandi per registrare il sistema di risoluzione dei conflitti aggiuntivi [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
        ```  
        regsvr32 ssradd.dll  
        ```  
  
5.  Se il sistema di risoluzione è un gestore della logica di business, distribuire l'assembly nella stessa cartella del file eseguibile dell'agente di merge (replmerg.exe), nella stessa cartella di un'applicazione che richiama l'agente di merge o nella cartella specificata per il parametro **\@dotnet_assembly_name** nel passaggio 3.  
  
    > [!NOTE]  
    >  Il percorso di installazione predefinito del file eseguibile dell'agente di merge è [!INCLUDE[ssInstallPath](../../../includes/ssinstallpath-md.md)]COM.  
  
## <a name="specify-a-custom-resolver-when-defining-a-merge-article"></a>Specificare un sistema di risoluzione personalizzato durante la definizione di un articolo di merge  
  
1.  Se si intende utilizzare un sistema di risoluzione dei conflitti personalizzato, crearlo e registrarlo utilizzando la procedura sopra riportata.  
  
2.  Nel server di pubblicazione eseguire [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md) e prendere nota del nome del sistema di risoluzione personalizzato desiderato nel campo **valore** del set di risultati.  
  
3.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Specificare il nome del sistema di risoluzione ottenuto nel passaggio 2 per **\@article_resolver** ed eventuale input obbligatorio per il sistema di risoluzione personalizzato usando il parametro **\@resolver_info**. Per sistemi di risoluzione personalizzati basati su stored procedure, **\@resolver_info** corrisponde al nome della stored procedure. Per altre informazioni sull'input richiesto per i sistemi di risoluzione dei conflitti forniti da [!INCLUDE[msCoName](../../../includes/msconame-md.md)], vedere [Sistemi di risoluzione dei conflitti basati su Microsoft COM](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-com-based-resolvers.md).  
  
## <a name="specify-or-change-a-custom-resolver-for-an-existing-merge-article"></a>Specificare o modificare un sistema di risoluzione personalizzato per un articolo di merge esistente  
  
1.  Per determinare se per un articolo è stato definito un sistema di risoluzione personalizzato oppure per ottenere il nome del sistema di risoluzione, eseguire [sp_helpmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md). Se per l'articolo è stato definito un sistema di risoluzione personalizzato, il relativo nome verrà visualizzato nel campo **article_resolver** . Eventuale input fornito al sistema di risoluzione verrà visualizzato nel campo **resolver_info** del set di risultati.  
  
2.  Nel server di pubblicazione eseguire [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md) e prendere nota del nome del sistema di risoluzione personalizzato desiderato nel campo **value** del set di risultati.  
  
3.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_changemergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Specificare il valore **article_resolver**, incluso il percorso completo dei gestori della logica di business, per **\@property**, nonché il nome del sistema di risoluzione personalizzato desiderato ottenuto nel passaggio 2 per **\@value**.  
  
4.  Per modificare l'eventuale input richiesto per il sistema di risoluzione personalizzato, eseguire nuovamente [sp_changemergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Specificare il valore **resolver_info** per **\@property** ed eventuale input obbligatorio per il sistema di risoluzione personalizzato per **\@value**. Per sistemi di risoluzione personalizzati basati su stored procedure, **\@resolver_info** corrisponde al nome della stored procedure. Per altre informazioni sull'input richiesto, vedere [Sistemi di risoluzione dei conflitti basati su Microsoft COM](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-com-based-resolvers.md).  
  
## <a name="unregister-a-custom-conflict-resolver"></a>Annullare la registrazione di un sistema di risoluzione dei conflitti personalizzato  
  
1.  Nel server di pubblicazione eseguire [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md) e prendere nota del nome del sistema di risoluzione personalizzato da rimuovere nel campo **value** del set di risultati.  
  
2.  Eseguire [sp_unregistercustomresolver &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-unregistercustomresolver-transact-sql.md) nel database di distribuzione. Specificare il nome completo del sistema di risoluzione personalizzato ottenuto nel passaggio 1 per **\@article_resolver**.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Esempi (Transact-SQL)  
 In questo esempio viene creato un nuovo articolo e viene impostato l'utilizzo del sistema di risoluzione dei conflitti medi [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per calcolare la media della colonna **UnitPrice** in caso di conflitti.  
  
 [!code-sql[HowTo#sp_addmerge_resolver](../../../relational-databases/replication/codesnippet/tsql/specify-a-merge-article-_1.sql)]  
  
 In questo esempio un articolo viene impostato in modo da utilizzare il sistema di risoluzione dei conflitti aggiuntivi [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per calcolare la somma della colonna **UnitsOnOrder** in caso di conflitti.  
  
 [!code-sql[HowTo#sp_changemerge_resolver](../../../relational-databases/replication/codesnippet/tsql/specify-a-merge-article-_2.sql)]  
  
## <a name="see-also"></a>Vedere anche  
 [Advanced Merge Replication Conflict Detection and Resolution](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)   
 [Implementare un gestore della logica di business per un articolo di merge](../../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md)  
  
  
