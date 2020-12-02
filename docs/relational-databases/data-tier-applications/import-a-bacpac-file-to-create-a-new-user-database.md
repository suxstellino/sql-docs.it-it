---
description: Importare un file BACPAC per creare un nuovo database utente
title: Importare un file BACPAC per creare un nuovo database utente
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb. importdac.results.f1
- sql13.swb.importdac.settings.f1
- sql13.swb.importdac.storagebrowser.f1
- sql13.swb.importdac.results.f1
- sql13.swb.importdac.progress.f1
- sql13.swb. importdac.summary.f1
- sql13.swb.importdac.summary.f1
- sql13.swb. importdac.progress.f1
- sql13.swb.importdac.welcome.f1
- sql13.swb. importdac.settings.f1
helpviewer_keywords:
- Data-tier application
- SQL Server DAC
- Migrate database
- DAC
ms.assetid: 736d8d9a-39f1-4bf8-b81f-2e56c134d12e
author: stevestein
ms.author: sstein
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 77209c7b5156956897c3c458e825fddeb7462e22
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2020
ms.locfileid: "92195593"
---
# <a name="import-a-bacpac-file-to-create-a-new-user-database"></a>Importare un file BACPAC per creare un nuovo database utente
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Importare un file dell'applicazione livello dati (DAC), con estensione bacpac, per creare una copia del database originale, completo dei dati, in una nuova istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] o in [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]. Le operazioni di importazione ed esportazione possono essere combinate per eseguire la migrazione di un'applicazione livello dati o database tra istanze o per creare un backup logico, quale la creazione di una copia locale di un database distribuito in [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
## <a name="before-you-begin"></a>Prima di iniziare  
 Il processo di importazione compila una nuova applicazione livello dati in due fasi.  
  
1.  L'importazione crea una nuova applicazione livello dati con database associato utilizzando la definizione dell'applicazione livello dati archiviata nel file di esportazione, con le stesse modalità con cui una distribuzione dell'applicazione livello dati crea una nuova applicazione livello dati dalla definizione in un file del pacchetto di applicazione livello dati.  
  
2.  Durante l'importazione viene eseguita la copia bulk di dati dal file di esportazione.  

## <a name="sql-server-utility"></a>Utilità SQL Server  
 Se si importa un'applicazione livello dati in un'istanza del motore di database, il pacchetto di applicazione livello dati importato viene incorporato in Utilità SQL Server al successivo invio del set di raccolta dell'utilità dall'istanza al punto di controllo dell'utilità. L'applicazione livello dati sarà quindi presente nel nodo **Deployed Data-tier Applications** (Applicazioni livello dati distribuite) in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] **Utility Explorer** e segnalata nella pagina dei dettagli **Deployed Data-tier Applications** (Applicazioni livello dati distribuite).  
  
## <a name="database-options-and-settings"></a>Opzioni e impostazioni del database  
 Per impostazione predefinita, il database creato durante l'importazione disporrà di tutte le impostazioni predefinite dall'istruzione CREATE DATABASE, con l'eccezione delle regole di confronto del database e del livello di compatibilità che vengono impostati sui valori definiti nel file di esportazione dell'applicazione livello dati. In un file di esportazione dell'applicazione livello dati vengono utilizzati i valori del database originale.  
  
 Alcune opzioni del database, ad esempio TRUSTWORTHY, DB_CHAINING e HONOR_BROKER_PRIORITY, non possono essere modificate durante il processo di importazione. Le proprietà fisiche, ad esempio il numero di filegroup o i numeri e le dimensioni dei file, non possono essere modificate durante il processo di importazione. Al termine dell'importazione, è possibile usare l'istruzione ALTER DATABASE, [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell per personalizzare il database. Per altre informazioni, vedere [Databases](../../relational-databases/databases/databases.md).  
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
 È possibile importare un'applicazione livello dati in [!INCLUDE[ssSDS](../../includes/sssds-md.md)]o in un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] in cui è in esecuzione [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 4 (SP4) o versioni successive. Se si esporta un'applicazione livello dati da una versione successiva, è possibile che nell'applicazione in questione siano contenuti oggetti non supportati da [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Non è possibile distribuire tali applicazioni livello dati a istanze di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
## <a name="prerequisites"></a>Prerequisiti  
 È consigliabile evitare di importare file di esportazione dell'applicazione livello dati provenienti da origini sconosciute o non attendibili. Tali file potrebbero contenere codice dannoso che potrebbe eseguire codice Transact-SQL indesiderato o causare errori modificando lo schema. Prima di usare un file di esportazione proveniente da un'origine sconosciuta o non attendibile, decomprimere l'applicazione livello dati e controllare il codice, ad esempio le stored procedure e altro codice definito dall'utente. Per altre informazioni su come eseguire questi controlli, vedere [Validate a DAC Package](validate-a-dac-package.md).  
  
## <a name="security"></a>Sicurezza  
 Per migliorare la sicurezza, gli account di accesso dell'autenticazione di SQL Server vengono archiviati in un file di esportazione dell'applicazione livello dati senza password. Quando il file viene importato, l'account di accesso viene creato come account disabilitato con una password generata. Per abilitare gli account di accesso è necessario accedere usando un account con l'autorizzazione ALTER ANY LOGIN e usare ALTER LOGIN per abilitare l'account di accesso e assegnare una nuova password che può essere comunicata all'utente. Questa operazione non è necessaria per gli account di accesso dell'autenticazione di Windows, in quanto le relative password non sono gestite da SQL Server.  
  
## <a name="permissions"></a>Autorizzazioni  
 Un'applicazione livello dati può essere importata unicamente da membri del ruolo predefinito del server **sysadmin** o **serveradmin** oppure tramite account di accesso nel ruolo predefinito del server **dbcreator** con autorizzazioni ALTER ANY LOGIN. È anche possibile importare un'applicazione livello dati con l'account dell'amministratore di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] predefinito denominato **sa** . L'importazione di un'applicazione livello dati con gli account di accesso in [!INCLUDE[ssSDS](../../includes/sssds-md.md)] richiede l'appartenenza al ruolo loginmanager o serveradmin. L'importazione di un'applicazione livello dati senza account di accesso in [!INCLUDE[ssSDS](../../includes/sssds-md.md)] richiede l'appartenenza al ruolo dbmanager o serveradmin.  
  
## <a name="using-the-import-data-tier-application-wizard"></a>Utilizzo della procedura guidata Importa applicazione livello dati  
 **Per avviare la procedura guidata, effettuare i passaggi seguenti:**  
  
1.  Connettersi all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], locale o in [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
2.  In **Esplora oggetti** fare clic con il pulsante destro del mouse su **Database** quindi scegliere la voce di menu **Importa applicazione livello dati** per avviare la procedura guidata.  
  
3.  Completare le finestre di dialogo della procedura guidata.  
  
    -   [Pagina Introduzione](#Introduction)  
  
    -   [Pagina Impostazioni di importazione](#Import_settings)  
  
    -   [Pagina Impostazioni database](#Database_settings)  
  
    -   [Pagina Riepilogo](#Summary)  
  
    -   [Pagina Stato](#Progress)  
  
    -   [Pagina Risultati](#Results)  
  
###  <a name="introduction-page"></a><a name="Introduction"></a> Pagina Introduzione  
 In questa pagina vengono descritti i passaggi per la procedura guidata Importa applicazione livello dati.  
  
 **Opzioni**  
  
-   **Non visualizzare più questa pagina** Selezionare la casella di controllo per evitare che la pagina Introduzione venga visualizzata nuovamente in futuro.  
  
-   **Avanti**: passa alla pagina **Impostazioni di importazione**.  
  
-   **Annulla**: annulla l'operazione e chiude la procedura guidata.  
  
###  <a name="import-settings-page"></a><a name="Import_settings"></a> Pagina Impostazioni di importazione  
 Utilizzare questa pagina per specificare il percorso del file con estensione bacpac da importare.  
  
-   **Importa da disco locale**: fare clic su **Sfoglia** per selezionare un percorso nel computer locale o specificare il percorso nell'apposito campo. Il nome del percorso deve includere un nome file e l'estensione .bacpac.  
  
-   **Importa da Azure**: importa un file BACPAC da un contenitore di Microsoft Azure. È necessario connettersi a un contenitore di Microsoft Azure per convalidare questa opzione. Si noti che l'opzione di importazione da Azure richiede anche che si specifichi una directory locale per il file temporaneo. Il file temporaneo verrà creato nel percorso specificato, dove rimarrà una volta completata l'operazione.  
  
     Quando si esplora Azure, sarà possibile passare tra contenitori all'interno di un solo account. È necessario specificare un solo file bacpac per continuare l'operazione di importazione. È possibile ordinare le colonne in base a **Nome**, **Dimensioni** o **Data modifica**.  
  
     Per continuare, specificare il file bacpac da importare, quindi fare clic su **Apri**.  
  
###  <a name="database-settings-page"></a><a name="Database_settings"></a> Pagina Impostazioni database  
 Usare questa pagina per specificare i dettagli del database che verrà creato.  
  
 **Per un'istanza locale di SQL Server:**  
  
-   **Nome nuovo database**: specificare un nome per il database importato.  
  
-   **Percorso file di dati**: fornire una directory locale per i file di dati. Fare clic su **Sfoglia** per selezionare un percorso nel computer locale oppure specificare il percorso nell'apposito campo.  
  
-   **Percorso file di log**: specificare una directory locale per i file di log. Fare clic su **Sfoglia** per selezionare un percorso nel computer locale oppure specificare il percorso nell'apposito campo.  
  
 Scegliere **Avanti** per continuare.  
  
 **Da un database SQL di Azure:**  
  
 - **[Importare un file BACPAC per creare un database SQL di Azure](/azure/azure-sql/database/database-import)** contiene istruzioni dettagliate per eseguire questa operazione usando il portale di Azure, PowerShell, SQL Server Management Studio o SqlPackage.  
 - Per un'analisi approfondita ai diversi livelli di servizio, vedere **[Opzioni e prestazioni disponibili in ogni livello di servizio del database SQL](/azure/azure-sql/database/purchasing-models)** .  

### <a name="validation-page"></a>Pagina Convalida  
 Usare questa pagina per esaminare gli eventuali problemi che impediscono l'operazione. Per continuare, risolvere i problemi che causano il blocco, quindi fare clic su **Ripeti convalida** per assicurarsi che la convalida venga completata correttamente.  
  
 Scegliere **Avanti** per continuare.  
  
###  <a name="summary-page"></a><a name="Summary"></a> Pagina Riepilogo  
 Utilizzare questa pagina per esaminare le impostazioni di origine e destinazione specificate per l'operazione. Per completare l'operazione di importazione utilizzando le impostazioni specificate, fare clic su **Fine**. Per annullare l'operazione di importazione e chiudere la procedura guidata, fare clic su **Annulla**.  
  
###  <a name="progress-page"></a><a name="Progress"></a> Pagina Stato  
 In questa pagina viene visualizzato un indicatore di stato che indica lo stato dell'operazione. Per visualizzare lo stato dettagliato, fare clic sull'opzione **Visualizza dettagli** .  
  
 Scegliere **Avanti** per continuare.  
  
###  <a name="results-page"></a><a name="Results"></a> Pagina Risultati  
 In questa pagina viene riportato l'esito positivo o negativo delle operazioni di impostazione e creazione del database, con l'indicazione dei risultati positivi o negativi di ogni azione. Ogni azione che ha rilevato un errore avrà un collegamento nella colonna **Risultato** . Fare clic sul collegamento per visualizzare un report dell'errore relativo all'azione.  
  
 Fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
## <a name="see-also"></a>Vedere anche  
[Importare un file BACPAC per creare un nuovo database SQL di Azure](/azure/azure-sql/database/database-import)  
 [Applicazioni livello dati](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [Esportazione di un'applicazione livello dati](../../relational-databases/data-tier-applications/export-a-data-tier-application.md)  
  
