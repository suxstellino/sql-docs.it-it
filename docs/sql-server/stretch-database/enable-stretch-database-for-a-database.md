---
description: Abilitare Stretch Database per un database
title: Abilitare Stretch Database per un database
ms.date: 08/05/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, enabling database
- enabling database for Stretch Database
ms.assetid: 37854256-8c99-4566-a552-432e3ea7c6da
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 0b93f34e5a5b716c649f0902dabefe651253c9ff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079982"
---
# <a name="enable-stretch-database-for-a-database"></a>Abilitare Stretch Database per un database
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Per configurare un database esistente per Stretch Database, selezionare **Attività | Stretch | Abilita** per un database in SQL Server Management Studio per aprire la procedura guidata **Abilitare il database per la funzionalità Stretch**. È possibile anche usare Transact-SQL per abilitare Stretch Database per un database.  
  
 Se si seleziona **Attività | Stretch | Abilita** per una tabella e non è stato ancora abilitato il database per Stretch Database, la procedura guidata consente di configurare il database per Stretch Database e le tabelle come parte del processo. Seguire la procedura illustrata in questo articolo anziché i passaggi descritti in [Abilitare Stretch Database per una tabella](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md).  
  
 L'abilitazione di Stretch Database in un database o una tabella richiede autorizzazioni db_owner. Per abilitare Stretch Database in un database è necessario disporre anche delle autorizzazioni CONTROL DATABASE.  

> [!NOTE]
> Se in seguito si disabilita Stretch Database, tenere presente che la disabilitazione di Stretch Database per una tabella o un database non elimina l'oggetto remoto. Se si vuole eliminare la tabella remota o il database remoto, è necessario eliminarlo tramite il portale di gestione di Azure. Gli oggetti remoti continuano a generare costi di Azure fino a quando non vengono eliminati manualmente. 
 
## <a name="before-you-get-started"></a>Prima di iniziare  
  
-   Prima di configurare un database per Stretch Database, è consigliabile eseguire l'ottimizzazione guidata Stretch Database per identificare i database e le tabelle idonei. L'ottimizzazione guidata Stretch Database consente anche di identificare i problemi di blocco. Per altre informazioni, vedere [Identificare i database e le tabelle per Estensione database eseguendo Stretch Database Advisor](../../sql-server/stretch-database/stretch-database-databases-and-tables-stretch-database-advisor.md).  
  
-   Vedere [Limitazioni di Stretch Database](../../sql-server/stretch-database/limitations-for-stretch-database.md).  
  
-   Stretch Database esegue la migrazione dei dati in Azure. È quindi necessario avere un account Azure e una sottoscrizione per la fatturazione. Per creare un account Azure, [fare clic qui](https://azure.microsoft.com/pricing/free-trial/).  
  
-   Recuperare le informazioni sulla connessione e sull'account di accesso necessarie per creare un nuovo server di Azure o selezionare un server di Azure esistente.  
  
##  <a name="prerequisite-enable-stretch-database-on-the-server"></a><a name="EnableTSQLServer"></a> Prerequisito: abilitare Stretch Database nel server  
 Prima di abilitare Stretch Database in un database o una tabella, è necessario abilitarlo nel server locale. Questa operazione richiede le autorizzazioni sysadmin o serveradmin.  
  
-   Se si dispone delle autorizzazioni amministrative necessarie, la procedura guidata **Abilitare il database per l'estensione** configura il server per Estensione.  
  
-   Se non si dispone delle autorizzazioni necessarie, l'amministratore deve abilitare l'opzione manualmente eseguendo **sp_configure** prima che venga avviata la procedura guidata o deve eseguire la procedura guidata.  
  
 Per abilitare manualmente Stretch Database nel server, eseguire **sp_configure** e attivare l'opzione **remote data archive** . L'esempio seguente abilita l'opzione **remote data archive** impostandone il valore su 1.  
  
```sql
EXEC sp_configure 'remote data archive' , '1';  
GO

RECONFIGURE;  
GO  
```  
  
 Per altre informazioni, vedere [Configure the remote data archive Server Configuration Option](../../database-engine/configure-windows/configure-the-remote-data-archive-server-configuration-option.md) (Configurare l'opzione di configurazione remote data archive) e [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
##  <a name="use-the-wizard-to-enable-stretch-database-on-a-database"></a><a name="Wizard"></a> Usare la procedura guidata per abilitare Stretch Database in un database  
 Per informazioni sulla procedura guidata Abilitare il database per l'estensione e sulle informazioni da immettere e le scelte da compiere, vedere [Get started by running the Enable Database for Stretch Wizard](../../sql-server/stretch-database/get-started-by-running-the-enable-database-for-stretch-wizard.md)(Iniziare eseguendo la procedura guidata Abilitare il database per l'estensione).  
  
##  <a name="use-transact-sql-to-enable-stretch-database-on-a-database"></a><a name="EnableTSQLDatabase"></a> Usare Transact-SQL per abilitare Stretch Database in un database  
 Prima di abilitare Stretch Database nelle singole tabelle, è necessario abilitarlo nel database.  
  
 L'abilitazione di Stretch Database in un database o una tabella richiede autorizzazioni db_owner. Per abilitare Stretch Database in un database è necessario disporre anche delle autorizzazioni CONTROL DATABASE.  
  
1.  Prima di iniziare, scegliere un server Azure esistente per i dati di cui Stretch Database esegue la migrazione o creare un nuovo server di Azure.  
  
2.  Nel server di Azure, creare una regola del firewall con l'intervallo di indirizzi IP di SQL Server per consentire a SQL Server di comunicare con il server remoto.  

    Per trovare facilmente i valori necessari e creare la regola del firewall, provare a connettersi al server di Azure da Esplora oggetti in SQL Server Management Studio (SSMS). SQL Server Management Studio consente di creare la regola aprendo la finestra di dialogo seguente, che include già i valori dell'indirizzo IP richiesti.
    
    ![Regola del firewall per l'estensione](../../sql-server/stretch-database/media/firewall-rule-for-stretch.png)
  
3.  Per configurare un database di SQL Server per Stretch Database, il database deve avere una chiave master del database. La chiave master del database consente di proteggere le credenziali usate da Stretch Database per la connessione al database remoto. L'esempio che segue crea una nuova chiave master del database.  
  
    ```sql  
    USE <database>; 
    GO  
  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD='<password>'; 
    GO
    ```  
    Per altre informazioni sulla chiave master del database, vedere [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md) e [Creazione della chiave master di un database](../../relational-databases/security/encryption/create-a-database-master-key.md).
    
4.  Quando si configura un database per Stretch Database, è necessario specificare le relative credenziali da usare per la comunicazione tra SQL Server locale e il server Azure remoto. Sono disponibili due opzioni:  
  
    -   È possibile fornire le credenziali amministratore.  
  
        -   Se si abilita Stretch Database eseguendo la procedura guidata, è possibile creare le credenziali in questa fase.  
  
        -   Se si abilita Stretch Database eseguendo **ALTER DATABASE**, è necessario creare manualmente le credenziali prima di eseguire **ALTER DATABASE** per abilitare Stretch Database. 
        
        L'esempio che segue crea una nuova credenziale.
  
        ```sql  
        CREATE DATABASE SCOPED CREDENTIAL <db_scoped_credential_name>  
            WITH IDENTITY = '<identity>' , SECRET = '<secret>' ;
        GO   
        ```  

         Per altre informazioni sulla credenziale, vedere [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md). Per la creazione delle credenziali, è necessaria l'autorizzazione ALTER ANY CREDENTIAL.  

    -   È possibile usare un account del servizio federato per SQL Server per comunicare con il server remoto di Azure quando vengono soddisfatte le condizioni seguenti.  
  
        -   L'account del servizio usato per l'esecuzione dell'istanza di SQL Server è un account di dominio.  
  
        -   L'account di dominio appartiene a un dominio il cui Active Directory è federato con Azure Active Directory.  
  
        -   Il server Azure remoto è configurato per supportare l'autenticazione di Azure Active Directory.  
  
        -   L'account del servizio in cui è in esecuzione l'istanza di SQL Server deve essere configurato come account dbmanager o sysadmin nel server remoto di Azure.  
  
5.  Per configurare un database per Stretch Database, eseguire il comando ALTER DATABASE.  
  
    1.  Per l'argomento SERVER, specificare il nome di un server di Azure esistente, inclusa la parte del nome `.database.windows.net` , ad esempio `MyStretchDatabaseServer.database.windows.net`.  
  
    2.  Fornire le credenziali di amministratore esistenti con l'argomento CREDENTIAL o specificare FEDERATED_SERVICE_ACCOUNT = ON. Nell'esempio seguente vengono fornite credenziali esistenti.  
  
    ```sql  
    ALTER DATABASE <database name>  
        SET REMOTE_DATA_ARCHIVE = ON  
            (  
                SERVER = '<server_name>' ,  
                CREDENTIAL = <db_scoped_credential_name>  
            ) ;  
    GO
    ```  
  
## <a name="next-steps"></a>Passaggi successivi  
-   [Abilitare Stretch Database per una tabella](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) per abilitare tabelle aggiuntive.  
  
-   [Monitor and troubleshoot data migration &#40;Stretch Database&#41; (Monitorare e risolvere i problemi relativi alla migrazione dei dati (Estensione database))](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md) per visualizzare lo stato della migrazione dei dati.  
  
-   [Pause and resume data migration &#40;Stretch Database&#41; (Sospendere e riprendere la migrazione dei dati (Estensione database))](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md)  
  
-   [Gestire e risolvere i problemi di Stretch Database](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)  
  
-   [Eseguire il backup di database con estensione abilitata](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md)  
  
-   [Ripristinare database con estensione abilitata](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Identificare i database e le tabelle per Estensione database eseguendo Stretch Database Advisor](../../sql-server/stretch-database/stretch-database-databases-and-tables-stretch-database-advisor.md)   
 [Opzioni ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)  
  
  
