---
title: Gestione connessione OLE DB | Microsoft Docs
description: Una gestione connessione OLE DB consente la connessione di un pacchetto a un'origine dati tramite un provider OLE DB.
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.oledbconnection.f1
helpviewer_keywords:
- OLE DB connection manager
- data sources [Integration Services], connections
- connection managers [Integration Services], OLE DB
- connections [Integration Services], OLE DB
ms.assetid: 91e3622e-4b1a-439a-80c7-a00b90d66979
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7c50a2e201bcba99299998dc232e1246005eb168
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97639396"
---
# <a name="ole-db-connection-manager"></a>Gestione connessione OLE DB

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Una gestione connessione OLE DB consente la connessione di un pacchetto a un'origine dati tramite un provider OLE DB. In una gestione connessione OLE DB tramite che si connette a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ad esempio, è possibile usare il provider [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].    
    
> [!NOTE]
>  Il provider OLE DB di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 11.0 non supporta le nuove parole chiave per le stringhe di connessione (MultiSubnetFailover=True) per il clustering di failover su più subnet. Per altre informazioni, vedere le [Note sulla versione di SQL Server](https://go.microsoft.com/fwlink/?LinkId=247824).    
> 
> [!NOTE]
>  Se l'origine dati è [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Excel 2007 o [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Access 2007, è richiesto un provider di dati diverso rispetto alle versioni precedenti di Excel o Access. Per altre informazioni, vedere [Connettersi a una cartella di lavoro di Excel](../load-data-to-from-excel-with-ssis.md) e [Connettersi a un database di Access](./integration-services-ssis-connections.md).    
    
La gestione connessione OLE DB viene usata da diversi componenti di flusso di dati e attività di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Ad esempio, l'origine OLE DB e la destinazione OLE DB usano questa gestione connessione per estrarre e caricare i dati. L'attività Esegui SQL può usare questa gestione connessione per connettersi a un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ed eseguire query.    
    
La gestione connessione OLE DB può essere usata anche per accedere alle origini dei dati OLE DB nelle attività personalizzate scritte in codice non gestito che usa un linguaggio come C++.    
    
Quando si aggiunge una gestione connessione OLE DB a un pacchetto, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crea una gestione connessione che in fase di esecuzione viene risolta in una connessione OLE DB, imposta le proprietà di tale gestione connessione e quindi la aggiunge alla raccolta **Connessioni** del pacchetto.    
    
La proprietà `ConnectionManagerType` della gestione connessione viene impostata su `OLEDB`.    
    
Configurare la gestione connessione OLE DB nel modo seguente:    
    
-   Specificare una stringa di connessione configurata in modo da soddisfare i requisiti del provider selezionato.    
    
-   Se richiesto dal provider, includere il nome dell'origine dei dati a cui connettersi.    
    
-   Specificare le credenziali di sicurezza come previsto dal provider selezionato.    
    
-   Indicare se la connessione creata dalla gestione connessione deve essere mantenuta in fase di esecuzione.    
    
## <a name="log-calls-and-troubleshoot-connections"></a>Registrare le chiamate e risolvere i problemi delle connessioni    
 È possibile registrare le chiamate eseguite dalla gestione connessione OLE DB a provider di dati esterni. Si possono quindi risolvere i problemi relativi alle connessioni stabilite dalla gestione connessione OLE DB a origini dati esterne. Per registrare le chiamate eseguite dalla gestione connessione OLE DB a provider di dati esterni, abilitare la registrazione dei pacchetti e selezionare l'evento **Diagnostic** a livello di pacchetto. Per altre informazioni, vedere [Risoluzione dei problemi relativi agli strumenti per l'esecuzione del pacchetto](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).    
    
## <a name="configure-the-ole-db-connection-manager"></a>Configurare la gestione connessione OLE DB    
 È possibile impostare le proprietà con Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] o a livello di codice. Per altre informazioni sulle proprietà che è possibile impostare in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] , vedere [Configura gestione connessione OLE DB](). Per informazioni sulla configurazione di una gestione connessione a livello di programmazione, vedere la documentazione per la classe **T:Microsoft.SqlServer.Dts.Runtime.ConnectionManager** nella Guida per gli sviluppatori.    
    
### <a name="configure-ole-db-connection-manager"></a>Configura gestione connessione OLE DB

Usare la finestra di dialogo **Configura gestione connessione OLE DB** per aggiungere una connessione a un'origine dati. Questa connessione può essere nuova o una copia di una connessione esistente.  
  
> [!NOTE]  
>  Se l'origine dati è [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Excel 2007, è richiesta una gestione connessione diversa rispetto alle versioni precedenti di Excel. Per altre informazioni, vedere [Connessione a una cartella di lavoro di Excel](../load-data-to-from-excel-with-ssis.md).  
>   
>  Se l'origine dati è [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Access 2007, è richiesto un provider OLE DB diverso rispetto alle versioni precedenti di Access. Per altre informazioni, vedere [Connessione a un database di Access](./integration-services-ssis-connections.md).  
  
 Per sapere di più sulla gestione connessione OLE DB, vedere [Gestione connessione OLE DB](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
#### <a name="options"></a>Opzioni  
 **Connessioni dati**  
 Consente di selezionare una connessione dati OLE DB esistente nell'elenco.  
  
 **Proprietà connessione dati**  
 Consente di visualizzare proprietà e valori per la connessione dati OLE DB selezionata.  
  
 **Nuovo**  
 Consente di creare una connessione dati OLE DB tramite la finestra di dialogo **Gestione connessione** .  
  
 **Elimina**  
 Selezionare una connessione dati e quindi eliminarla selezionando **Elimina**.  
  
#### <a name="managed-identities-for-azure-resources-authentication"></a>Identità gestite per l'autenticazione delle risorse di Azure
Quando si eseguono pacchetti SSIS in [Azure-SSIS Integration Runtime in Azure Data Factory](/azure/data-factory/concepts-integration-runtime#azure-ssis-integration-runtime), usare l'[identità gestita](/azure/data-factory/connector-azure-sql-database#managed-identity) associata alla data factory per l'autenticazione del database SQL di Azure o di Istanza gestita. La factory specificata può accedere e copiare i dati dal database o nel database usando questa identità.

> [!NOTE]
>  Quando si usa l'autenticazione di Azure Active Directory (Azure AD), inclusa l'autenticazione identità gestita, per connettersi al database SQL di Azure (o Istanza gestita), si potrebbe verificare un errore di esecuzione del pacchetto o una modifica del comportamento imprevista. Per altre informazioni, vedere [Funzionalità e limitazioni di Azure AD](/azure/sql-database/sql-database-aad-authentication#azure-ad-features-and-limitations).

Per usare l'autenticazione identità gestita per il database SQL di Azure, seguire questa procedura per configurare il database:

1. [Effettuare il provisioning di un amministratore di Azure Active Directory](/azure/sql-database/sql-database-aad-authentication-configure#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server) per il server SQL Azure nel portale di Azure, se non è già stato fatto. L'amministratore di Azure AD può essere un utente di Azure AD o gruppo di Azure AD. Se si concede al gruppo con identità gestita un ruolo di amministratore, ignorare i passaggi 2 e 3. L'amministratore avrà accesso completo al database.

1. [Creare utenti del database indipendente](/azure/sql-database/sql-database-aad-authentication-configure#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities) per l'identità gestita data factory. Connettersi al database da cui o in cui si vuole copiare i dati usando strumenti come SQL Server Management Studio con un'identità di Azure AD che abbia almeno l'autorizzazione ALTER ANY USER. Eseguire il codice T-SQL seguente: 
    
    ```sql
    CREATE USER [your data factory name] FROM EXTERNAL PROVIDER;
    ```

1. Concedere le autorizzazioni necessarie per l'identità gestita data factory come si fa normalmente per gli utenti SQL e altri utenti. Per i ruoli appropriati, vedere [Ruoli a livello di database](../../relational-databases/security/authentication-access/database-level-roles.md). Eseguire il codice seguente. Per altre opzioni, vedere [questo documento](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md).

    ```sql
    EXEC sp_addrolemember [role name], [your data factory name];
    ```

Per usare l'autenticazione identità gestita per Istanza gestita di SQL di Azure, seguire questa procedura per configurare il database:
    
1. [Effettuare il provisioning di un amministratore di Azure Active Directory](/azure/sql-database/sql-database-aad-authentication-configure#provision-an-azure-active-directory-administrator-for-your-managed-instance) per l'istanza gestita nel portale di Azure, se non è già stato fatto. L'amministratore di Azure AD può essere un utente di Azure AD o gruppo di Azure AD. Se si concede al gruppo con identità gestita un ruolo di amministratore, ignorare i passaggi 2-4. L'amministratore avrà accesso completo al database.

1. [Creare account di accesso](../../t-sql/statements/create-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true) per l'identità gestita data factory. In SQL Server Management Studio (SSMS) connettersi all'istanza gestita con un account SQL Server di tipo **sysadmin**. Eseguire il codice T-SQL seguente nel database **master**:

    ```sql
    CREATE LOGIN [your data factory name] FROM EXTERNAL PROVIDER;
    ```

1. [Creare utenti del database indipendente](/azure/sql-database/sql-database-aad-authentication-configure#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities) per l'identità gestita data factory. Connettersi al database da cui o in cui si vuole copiare i dati ed eseguire il codice T-SQL seguente: 
  
    ```sql
    CREATE USER [your data factory name] FROM EXTERNAL PROVIDER;
    ```

1. Concedere le autorizzazioni necessarie per l'identità gestita data factory, come si fa normalmente per gli utenti SQL e altri utenti. Eseguire il codice seguente. Per altre opzioni, vedere [questo documento](../../t-sql/statements/alter-role-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true).

    ```sql
    ALTER ROLE [role name e.g., db_owner] ADD MEMBER [your data factory name];
    ```

Quindi configurare il provider OLE DB per la gestione connessione OLE DB. Sono disponibili le opzioni seguenti:
    
- **Configurare in fase di progettazione.** In Progettazione SSIS fare doppio clic sulla gestione connessione OLE DB per aprire la finestra **Gestione connessione**. Nell'elenco a discesa **Provider** selezionare [**Microsoft OLE DB Driver for SQL Server**](https://go.microsoft.com/fwlink/?linkid=871294).
    > [!NOTE]
    >  È possibile che altri provider inclusi nell'elenco a discesa non supportino l'autenticazione identità gestita.
    
- **Configurare in fase di esecuzione.** Quando si esegue il pacchetto con [SQL Server Management Studio (SSMS)](../ssis-quickstart-run-ssms.md) o l'[attività Esegui pacchetto SSIS di Azure Data Factory](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity), individuare la proprietà della gestione connessione **ConnectionString** per la gestione connessione OLE DB. Aggiornare la proprietà di connessione `Provider` impostandola su `MSOLEDBSQL`, ovvero Microsoft OLE DB Driver per SQL Server.
    ```vb
    Data Source=serverName;Initial Catalog=databaseName;Provider=MSOLEDBSQL;...
    ```

Infine configurare l'autenticazione identità gestita per la gestione connessione OLE DB. Sono disponibili le opzioni seguenti:
    
- **Configurare in fase di progettazione.** In Progettazione SSIS fare clic con il pulsante destro del mouse sulla gestione connessione OLE DB e selezionare **Proprietà**. Aggiornare la proprietà `ConnectUsingManagedIdentity` impostandola su `True`.
    > [!NOTE]
    >  Attualmente la proprietà di gestione connessione `ConnectUsingManagedIdentity`non ha effetto, ovvero l'autenticazione identità gestita non funziona, quando il pacchetto SSIS viene eseguito in Progettazione SSIS o [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server.

- **Configurare in fase di esecuzione.** Quando si esegue il pacchetto con SSMS o un'attività **Esegui pacchetto SSIS** individuare la gestione connessione OLE DB e aggiornarne la proprietà `ConnectUsingManagedIdentity` impostandola su `True`.
    > [!NOTE]
    >  Nel runtime di integrazione Azure-SSIS tutti gli altri metodi di autenticazione, ad esempio la sicurezza integrata e la password, preconfigurati nella gestione connessione OLE DB vengono ignorati quando viene usata l'autenticazione identità gestita per stabilire la connessione al database.

> [!NOTE]
>  Per configurare l'autenticazione identità gestita nei pacchetti esistenti, il modo migliore consiste nel ricompilare almeno una volta il progetto SSIS con la [versione più recente di Progettazione SSIS](../../ssdt/download-sql-server-data-tools-ssdt.md). Ridistribuire il progetto SSIS in Azure-SSIS Integration Runtime, in modo che la nuova proprietà di gestione connessione `ConnectUsingManagedIdentity` venga aggiunta automaticamente a tutte le gestioni connessioni OLE DB nel progetto SSIS. In alternativa, è possibile usare direttamente un override di proprietà con il percorso della proprietà **\Package.Connections[{nome della gestione connessione}].Properties[ConnectUsingManagedIdentity]** in fase di esecuzione.

## <a name="see-also"></a>Vedere anche    
 [Origine OLE DB](../../integration-services/data-flow/ole-db-source.md)     
 [Destinazione OLE DB](../../integration-services/data-flow/ole-db-destination.md)     
 [Attività Esegui SQL](../../integration-services/control-flow/execute-sql-task.md)     
 [Connessioni in Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md)    
    
