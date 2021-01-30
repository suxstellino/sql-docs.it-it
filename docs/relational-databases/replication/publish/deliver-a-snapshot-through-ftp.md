---
description: Recapito di snapshot tramite FTP
title: Recapitare uno snapshot tramite FTP | Microsoft Docs
ms.custom: ''
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- snapshots [SQL Server replication], FTP snapshots
- FTP snapshots [SQL Server replication]
- snapshot replication [SQL Server], FTP
ms.assetid: 99872c4f-40ce-4405-8fd4-44052d3bd827
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e2cd6bb1841279be3f699c5d2f59b09f6b9c9c27
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076719"
---
# <a name="deliver-a-snapshot-through-ftp"></a>Recapito di snapshot tramite FTP
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come recapitare uno snapshot utilizzando l'FTP in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)].  

Per impostazione predefinita, gli snapshot vengono archiviati in cartelle definite condivisioni UNC. La replica consente inoltre di specificare una condivisione FTP anziché una condivisione UNC. Per utilizzare l'FTP, è necessario configurare un server FTP e quindi configurare una pubblicazione e una o più sottoscrizioni che utilizzeranno l'FTP. Per informazioni sulla configurazione di un server FTP, vedere la documentazione di Internet Information Services (IIS). Se si specificano le informazioni FTP per una pubblicazione, le sottoscrizioni a quella pubblicazione per impostazione predefinita utilizzano l'FTP. FTP viene utilizzato con la sincronizzazione Web solo quando il computer con IIS è separato dal server di distribuzione da un firewall. In questo caso, è possibile utilizzare FTP per trasferire lo snapshot dal server di distribuzione e dal computer che esegue IIS. Per il trasferimento degli snapshot al Sottoscrittore viene sempre utilizzato HTTPS.  
  
> [!IMPORTANT]  
>  È consigliabile usare l'autenticazione di Microsoft Windows e una condivisione UNC, anziché una condivisione FTP, perché le password FTP devono essere archiviate e la password viene inviata al server FTP come testo normale dal Sottoscrittore o dal computer con IIS che usa la sincronizzazione Web. Inoltre, dato che un unico account controlla l'accesso alla condivisione snapshot, non è possibile garantire che un Sottoscrittore di una pubblicazione filtrata di tipo merge abbia accesso solo ai file snapshot della relativa partizione di dati.  
  

## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
  
-   L'agente snapshot deve disporre delle autorizzazioni di scrittura per la directory specificata, mentre l'agente di distribuzione o l'agente di merge deve disporre delle autorizzazioni di lettura. Se vengono usate sottoscrizioni pull, è necessario specificare una directory condivisa come percorso UNC (Universal Naming Convention), ad esempio \\\serverftp\home\snapshot. Per altre informazioni, vedere [Proteggere la cartella snapshot](../../../relational-databases/replication/security/secure-the-snapshot-folder.md).  
  
## <a name="prerequisites"></a>Prerequisiti  
  
-   Prima di trasferire file snapshot tramite il protocollo FTP (File Transfer Protocol), è necessario configurare un server FTP. Per ulteriori informazioni, vedere la documentazione di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Internet Information Services (IIS).  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
 Per migliorare la sicurezza, si consiglia di implementare una rete privata virtuale (VPN) quando si utilizza il recapito di snapshot tramite FTP in Internet. Per altre informazioni, vedere [Pubblicare i dati su Internet tramite VPN](../../../relational-databases/replication/publish-data-over-the-internet-using-vpn.md).  
  
 Ai fini della sicurezza è consigliabile non consentire accessi anonimi al server FTP. L'agente snapshot deve disporre delle autorizzazioni di scrittura per la directory specificata, mentre l'agente di distribuzione o l'agente di merge deve disporre delle autorizzazioni di lettura. Se vengono usate sottoscrizioni pull, è necessario specificare una directory condivisa come percorso UNC (Universal Naming Convention), ad esempio \\\serverftp\home\snapshot. Per altre informazioni, vedere [Proteggere la cartella snapshot](../../../relational-databases/replication/security/secure-the-snapshot-folder.md).  
  
 Se possibile, richiedere agli utenti di immettere le credenziali in fase di esecuzione. Se si archiviano le credenziali in un file script, è necessario proteggere il file.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 Dopo la configurazione del server FTP specificare la directory e le informazioni di sicurezza relative a questo server nella finestra di dialogo **Proprietà pubblicazione \<Publication>** . Per ulteriori informazioni sull'accesso a questa finestra di dialogo, vedere [View and Modify Publication Properties](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-specify-ftp-information"></a>Per specificare informazioni FTP  
  
1.  Nella finestra di dialogo **Proprietà pubblicazione - \<Publication>** selezionare **I Sottoscrittori possono scaricare i file di snapshot utilizzando FTP (File Transfer Protocol)** in una delle pagine seguenti:  
  
    -   La pagina **Snapshot FTP**, per le pubblicazioni di snapshot e transazionali e le pubblicazioni di tipo merge per i server di pubblicazione che eseguono versioni precedenti a [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  
  
    -   La pagina **Snapshot FTP e Internet** , per le pubblicazioni di tipo merge provenienti dai server di pubblicazione in cui è in esecuzione [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] o una versione successiva.  
  
2.  Specificare i valori per **Nome server FTP**, **Numero di porta**, **Percorso dalla cartella radice FTP**, **Nome account di accesso** e **Password**.  
  
     Ad esempio, se la radice del server FTP è \\\serverftp\home e si vogliono archiviare gli snapshot nel percorso \\\serverftp\home\snapshot, specificare \snapshot\ftp per la proprietà **Percorso dalla cartella radice FTP**. La replica aggiunge "ftp" al percorso della cartella snapshot durante la creazione dei file di snapshot.  
  
3.  Specificare che tramite l'agente snapshot i file di snapshot devono essere scritti nella directory specificata al passaggio 2. Ad esempio, per fare in modo che l'agente snapshot scriva i file di snapshot in \\\serverftp\home\snapshot\ftp, è necessario specificare il percorso \\\serverftp\home\snapshot in una delle due posizioni indicate di seguito:  
  
    -   Il percorso predefinito dello snapshot per il server di distribuzione associato a questa pubblicazione.    
    -   Un percorso alternativo per la cartella snapshot per questa pubblicazione. Se lo snapshot è compresso viene richiesto un percorso alternativo.  

Per altre informazioni su come modificare le proprietà di posizione della cartella snapshot, vedere [Modificare le opzioni snapshot](../snapshot-options.md).
  

4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
 È possibile impostare l'opzione per rendere i file snapshot disponibili in un server FTP e modificare le impostazioni FTP a livello di programmazione utilizzando stored procedure di replica. La stored procedure utilizzata varia a seconda del tipo di pubblicazione. Il recapito di snapshot tramite FTP viene utilizzato solo con le sottoscrizioni pull.  
  
#### <a name="to-enable-ftp-snapshot-delivery-for-a-snapshot-or-transactional-publication"></a>Per abilitare il recapito di snapshot tramite FTP per una pubblicazione snapshot o transazionale  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addpublication](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md). Specificare `@publication`, il valore **true** per `@enabled_for_internet` e valori appropriati per i parametri seguenti:  
  
    -   `@ftp_address`: indirizzo del server FTP utilizzato per il recapito dello snapshot.  
  
    -   (Facoltativo) `@ftp_port`: porta usata dal server FTP.  
  
    -   (Facoltativo) `@ftp_subdirectory`: sottodirectory della directory FTP predefinita assegnata a un account di accesso FTP. Se, ad esempio, la radice del server FTP è \\\serverftp\home e si vogliono archiviare gli snapshot in \\\serverftp\home\snapshot, specificare **\snapshots\ftp** per `@ftp_subdirectory`. La replica aggiungerà "ftp" al percorso della cartella snapshot durante la creazione di file di snapshot.  
  
    -   (Facoltativo) `@ftp_login`: account di accesso usato per la connessione al server FTP.  
  
    -   (Facoltativo) `@ftp_password`: password per l'accesso FTP.  
  
     Verrà creata una pubblicazione che utilizza FTP. Per altre informazioni, vedere [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md).  
  
#### <a name="to-enable-ftp-snapshot-delivery-for-a-merge-publication"></a>Per attivare il recapito di snapshot tramite FTP per una pubblicazione di tipo merge  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergepublication](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md). Specificare `@publication`, il valore **true** per `@enabled_for_internet` e valori appropriati per i parametri seguenti:  
  
    -   `@ftp_address`: indirizzo del server FTP utilizzato per il recapito dello snapshot.  
  
    -   (Facoltativo) `@ftp_port`: porta usata dal server FTP.  
  
    -   (Facoltativo) `@ftp_subdirectory`: sottodirectory della directory FTP predefinita assegnata a un account di accesso FTP. Se, ad esempio, la radice del server FTP è \\\serverftp\home e si vogliono archiviare gli snapshot in \\\serverftp\home\snapshot, specificare **\snapshots\ftp** per `@ftp_subdirectory`. La replica aggiungerà "ftp" al percorso della cartella snapshot durante la creazione di file di snapshot.  
  
    -   (Facoltativo) `@ftp_login`: account di accesso usato per la connessione al server FTP.  
  
    -   (Facoltativo) `@ftp_password`: password per l'accesso FTP.  
  
     Verrà creata una pubblicazione che utilizza FTP. Per altre informazioni, vedere [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md).  
  
#### <a name="to-create-a-pull-subscription-to-a-snapshot-or-transactional-publication-that-uses-ftp-snapshot-delivery"></a>Per creare una sottoscrizione pull in una pubblicazione snapshot o transazionale che utilizza il recapito di snapshot tramite FTP  
  
1.  Nel database di sottoscrizione del Sottoscrittore eseguire [sp_addpullsubscription](../../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md). Specificare `@publisher` e `@publication`.  
  
    -   Nel database di sottoscrizione del Sottoscrittore eseguire [sp_addpullsubscription_agent](../../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md). Specificare `@publisher`, `@publisher_db`, `@publication`, le credenziali di [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows usate per l'esecuzione dell'agente di distribuzione nel Sottoscrittore per `@job_login` e `@job_password` e il valore **true** per `@use_ftp`.  
  
2.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addsubscription](../../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) per registrare la sottoscrizione pull. Per altre informazioni, vedere [Creazione di una sottoscrizione pull](../../../relational-databases/replication/create-a-pull-subscription.md).  
  
#### <a name="to-create-a-pull-subscription-to-a-merge-publication-that-uses-ftp-snapshot-delivery"></a>Per creare una sottoscrizione pull in una pubblicazione di tipo merge che utilizza il recapito di snapshot tramite FTP  
  
1.  Nel database di sottoscrizione del Sottoscrittore eseguire [sp_addmergepullsubscription](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md). Specificare `@publisher` e `@publication`.  
  
2.  Nel database di sottoscrizione del Sottoscrittore eseguire [sp_addmergepullsubscription_agent](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md). Specificare `@publisher`, `@publisher_db`, `@publication`, le credenziali di Windows usate per l'esecuzione dell'agente di distribuzione nel Sottoscrittore per `@job_login` e `@job_password` e il valore `true` per `@use_ftp`.  
  
3.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_addmergesubscription](../../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md) per registrare la sottoscrizione pull. Per altre informazioni, vedere [Creazione di una sottoscrizione pull](../../../relational-databases/replication/create-a-pull-subscription.md).  
  
#### <a name="to-change-one-or-more-ftp-snapshot-delivery-settings-for-a-snapshot-or-transactional-publication"></a>Per modificare una o più impostazioni del recapito di snapshot tramite FTP per una pubblicazione snapshot o transazionale  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_changepublication](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md). Specificare uno dei valori seguenti per `@property` e un nuovo valore di questa impostazione per `@value`:  
  
    -   `ftp_address `: indirizzo del server FTP usato per il recapito dello snapshot.  
  
    -   `ftp_port`: porta utilizzata dal server FTP.  
  
    -   `ftp_subdirectory`: sottodirectory della directory FTP predefinita utilizzata per lo snapshot tramite FTP.  
  
    -   `ftp_login`: account di accesso utilizzato per la connessione al server FTP.  
  
    -   `ftp_password`: password per l'account di accesso FTP.  
  
2.  (Facoltativo) Ripetere il passaggio 1 per ogni impostazione FTP da modificare.  
  
3.  (Facoltativo) Per disabilitare il recapito di snapshot tramite FTP, eseguire [sp_changepublication](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) nel database di pubblicazione del server di pubblicazione. Specificare il valore `enabled_for_internet` per `@property` e il valore `false` per `@value`.  
  
#### <a name="to-change-ftp-snapshot-delivery-settings-for-a-merge-publication"></a>Per modificare le impostazioni del recapito di snapshot tramite FTP per una pubblicazione di tipo merge  
  
1.  Nel database di pubblicazione del server di pubblicazione eseguire [sp_changemergepublication](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md). Specificare uno dei valori seguenti per `@property` e un nuovo valore di questa impostazione per `@value`:  
  
    -   `ftp_address`: indirizzo del server FTP utilizzato per il recapito dello snapshot.  
  
    -   `ftp_port`: porta utilizzata dal server FTP.  
  
    -   `ftp_subdirectory`: sottodirectory della directory FTP predefinita utilizzata per lo snapshot tramite FTP.  
  
    -   `ftp_login`: account di accesso utilizzato per la connessione al server FTP.  
  
    -   `ftp_password`: password per l'account di accesso FTP.  
  
2.  (Facoltativo) Ripetere il passaggio 1 per ogni impostazione FTP da modificare.  
  
3.  (Facoltativo) Per disabilitare il recapito di snapshot tramite FTP, eseguire [sp_changemergepublication](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md) nel database di pubblicazione del server di pubblicazione. Specificare il valore `enabled_for_internet` per `@property` e il valore `false` per `@value`.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Esempi (Transact-SQL)  
 Nell'esempio seguente viene creata una pubblicazione di tipo merge che consente ai Sottoscrittori di accedere ai dati dello snapshot tramite FTP. Il Sottoscrittore deve utilizzare una connessione VPN sicura per l'accesso alla condivisione FTP. I valori dell'account di accesso e della password vengono forniti tramite le variabili di scripting **sqlcmd** . Per altre informazioni, vedere [Usare sqlcmd con variabili di scripting](../../../ssms/scripting/sqlcmd-use-with-scripting-variables.md).  
  
 [!code-sql[HowTo#sp_createmergepub_ftp](../../../relational-databases/replication/codesnippet/tsql/deliver-a-snapshot-throu_1.sql)]  
  
 Nell'esempio seguente viene creata una sottoscrizione in una pubblicazione di tipo merge in cui il Sottoscrittore ottiene lo snapshot tramite FTP. Il Sottoscrittore deve utilizzare una connessione VPN sicura per l'accesso alla condivisione FTP. I valori dell'account di accesso e della password vengono forniti tramite le variabili di scripting **sqlcmd** . Per altre informazioni, vedere [Usare sqlcmd con variabili di scripting](../../../ssms/scripting/sqlcmd-use-with-scripting-variables.md).  
  
 [!code-sql[HowTo#sp_createmergepullsub_ftp](../../../relational-databases/replication/codesnippet/tsql/deliver-a-snapshot-throu_2.sql)]  
  
 [!code-sql[HowTo#sp_createmergepullsubagent_ftp](../../../relational-databases/replication/codesnippet/tsql/deliver-a-snapshot-throu_3.sql)]  
  
## <a name="see-also"></a>Vedere anche  
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Modificare le proprietà di pubblicazioni e articoli](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [Inizializzare una sottoscrizione con uno snapshot](../../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)  
  
