---
title: Gestione connessione ADO.NET | Microsoft Docs
description: Una gestione connessione ADO.NET consente l'accesso di un pacchetto alle origini dati tramite un provider .NET.
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.adonetconnection.f1
helpviewer_keywords:
- connection managers [Integration Services], ADO.NET
- ADO.NET connection manager [Integration Services]
- connections [Integration Services], ADO.NET
ms.assetid: fc5daa2f-0159-4bda-9402-c87f1035a96f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8c2efe0145643f3f9e969c298e76115967477019
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637577"
---
# <a name="adonet-connection-manager"></a>Gestione connessione ADO.NET

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Una gestione connessione [!INCLUDE[vstecado](../../includes/vstecado-md.md)] consente l'accesso di un pacchetto alle origini dati tramite un provider .NET. Questa gestione connessione viene di solito usata per accedere a origini dati come [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È anche possibile accedere a origini dati esposte tramite OLE DB e XML in attività personalizzate scritte in codice gestito, usando un linguaggio come C#.  
  
Quando si aggiunge una gestione connessione [!INCLUDE[vstecado](../../includes/vstecado-md.md)] a un pacchetto, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crea una gestione connessione che viene risolta come una connessione [!INCLUDE[vstecado](../../includes/vstecado-md.md)] in fase di esecuzione. Vengono impostate le proprietà della gestione connessione e la gestione connessione viene aggiunta alla raccolta **Connessioni** del pacchetto.  
  
La proprietà `ConnectionManagerType` della gestione connessione viene impostata su `ADO.NET`. Il valore di `ConnectionManagerType` è qualificato con il nome del provider .NET utilizzato dalla gestione connessione.  
  
## <a name="adonet-connection-manager-troubleshooting"></a>Risoluzione dei problemi relativi alla gestione connessione ADO.NET  
È possibile registrare le chiamate eseguite dalla gestione connessione [!INCLUDE[vstecado](../../includes/vstecado-md.md)] a provider di dati esterni. Si possono quindi risolvere i problemi relativi alle connessioni stabilite dalla gestione connessione [!INCLUDE[vstecado](../../includes/vstecado-md.md)] a origini dati esterne. Per registrare le chiamate eseguite dalla gestione connessione [!INCLUDE[vstecado](../../includes/vstecado-md.md)] a provider di dati esterni, abilitare la registrazione dei pacchetti e selezionare l'evento **Diagnostic** a livello di pacchetto. Per altre informazioni, vedere [Risoluzione dei problemi relativi agli strumenti per l'esecuzione del pacchetto](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).  
  
Durante la lettura da parte di una gestione connessione [!INCLUDE[vstecado](../../includes/vstecado-md.md)], i dati di alcuni tipi di dati date di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genereranno i risultati indicati nella tabella seguente.  
  
|Tipo di dati di SQL Server|Risultato|  
|--------------------------|------------|  
|**time**, **datetimeoffset**|L'esecuzione del pacchetto non viene completata correttamente, a meno che non vengano utilizzati comandi SQL con parametri. È necessario servirsi dell'attività Esegui SQL nel pacchetto per utilizzare i comandi SQL con parametri. Per altre informazioni, vedere [Attività Esegui SQL](../../integration-services/control-flow/execute-sql-task.md) e [Parametri e codici restituiti nell'attività Esegui SQL](../control-flow/execute-sql-task.md).|  
|**datetime2**|La gestione connessione [!INCLUDE[vstecado](../../includes/vstecado-md.md)] tronca il valore relativo ai millisecondi.|  
  
> [!NOTE]  
>  Per altre informazioni sui tipi di dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e sul relativo mapping nei tipi di dati [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vedere [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md) e [Tipi di dati di Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="adonet-connection-manager-configuration"></a>Configurazione della gestione connessione ADO.NET  
  
È possibile impostare le proprietà con Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)] o a livello di codice.  
  
-   Specificare una stringa di connessione configurata in modo da soddisfare i requisiti del provider .NET selezionato.  
  
-   Se richiesto dal provider, includere il nome dell'origine dei dati a cui connettersi.  
  
-   Specificare le credenziali di sicurezza come previsto dal provider selezionato.  
  
-   Indicare se la connessione creata dalla gestione connessione deve essere mantenuta in fase di esecuzione.  
  
Molte delle opzioni di configurazione della gestione connessione [!INCLUDE[vstecado](../../includes/vstecado-md.md)] dipendono dal provider .NET usato dalla gestione connessione.  
  
Per altre informazioni sulle proprietà che è possibile impostare in Progettazione [!INCLUDE[ssIS](../../includes/ssis-md.md)], vedere [Configurare la gestione connessione ADO.NET]().  
  
 Per informazioni sulla configurazione di una gestione connessione a livello di programmazione, vedere l'articolo relativo a <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> e [Aggiunta di connessioni a livello di programmazione](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md).  
  
### <a name="configure-adonet-connection-manager"></a>Configurare la gestione connessione ADO.NET
Usare la finestra di dialogo **Configura gestione connessione ADO.NET** per aggiungere una connessione a un'origine dati accessibile tramite un provider di dati .NET Framework, ad esempio il provider SqlClient. La gestione connessione può utilizzare una connessione esistente oppure è possibile crearne una nuova.  
  
 Per ulteriori informazioni sulla gestione connessione ADO.NET, vedere [ADO.NET Connection Manager](../../integration-services/connection-manager/ado-net-connection-manager.md).  
  
#### <a name="options"></a>Opzioni  
**Connessioni dati**  
Consente di selezionare una connessione dati ADO.NET esistente nell'elenco.  
  
**Proprietà connessione dati**  
Consente di visualizzare proprietà e valori per la connessione dati ADO.NET selezionata.  
  
**Nuovo**  
Consente di creare una connessione dati ADO.NET tramite la finestra di dialogo **Gestione connessione** .  
  
**Elimina**  
Selezionare una connessione e quindi eliminarla selezionando **Elimina**.  
  
#### <a name="managed-identities-for-azure-resources-authentication"></a>Identità gestite per l'autenticazione delle risorse di Azure
Quando si eseguono pacchetti SSIS in [Azure-SSIS Integration Runtime in Azure Data Factory](/azure/data-factory/concepts-integration-runtime#azure-ssis-integration-runtime), è possibile usare l'[identità gestita](/azure/data-factory/connector-azure-sql-database#managed-identity) associata alla data factory per l'autenticazione del database SQL di Azure o di Istanza gestita di SQL di Azure. La factory specificata può accedere e copiare i dati dal database o nel database usando questa identità.

> [!NOTE]
>  Quando si usa l'autenticazione di Azure Active Directory (Azure AD), inclusa l'autenticazione identità gestita, per connettersi al database SQL di Azure o a Istanza gestita di SQL di Azure, si potrebbe verificare un errore di esecuzione del pacchetto o una modifica del comportamento imprevista. Per altre informazioni, vedere [Funzionalità e limitazioni di Azure AD](/azure/sql-database/sql-database-aad-authentication#azure-ad-features-and-limitations).

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

Infine configurare l'autenticazione identità gestita per la gestione connessione ADO.NET. Sono disponibili le opzioni seguenti:
    
- **Configurare in fase di progettazione.** In Progettazione SSIS fare clic con il pulsante destro del mouse sulla gestione connessione ADO.NET e selezionare **Proprietà**. Aggiornare la proprietà `ConnectUsingManagedIdentity` impostandola su `True`.
    > [!NOTE]
    >  Attualmente la proprietà di gestione connessione `ConnectUsingManagedIdentity`non ha effetto, ovvero l'autenticazione identità gestita non funziona, quando il pacchetto SSIS viene eseguito in Progettazione SSIS o [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server.
    
- **Configurare in fase di esecuzione.** Quando il pacchetto viene eseguito tramite [SQL Server Management Studio (SSMS)](../ssis-quickstart-run-ssms.md) o l'[attività Esegui pacchetto SSIS di Azure Data Factory](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity), individuare la gestione connessione ADO.NET. Aggiornarne la proprietà `ConnectUsingManagedIdentity` impostandola su `True`.
    > [!NOTE]
    >  Nel runtime di integrazione Azure-SSIS tutti gli altri metodi di autenticazione, ad esempio l'autenticazione integrata e la password, preconfigurati nella gestione connessione ADO.NET vengono ignorati quando viene usata l'autenticazione identità gestita per stabilire la connessione al database.

> [!NOTE]
>  Per configurare l'autenticazione identità gestita nei pacchetti esistenti, il modo migliore consiste nel ricompilare almeno una volta il progetto SSIS con la [versione più recente di Progettazione SSIS](../../ssdt/download-sql-server-data-tools-ssdt.md). Ridistribuire il progetto SSIS in Azure-SSIS Integration Runtime, in modo che la nuova proprietà di gestione connessione `ConnectUsingManagedIdentity` venga aggiunta automaticamente a tutte le gestioni connessioni ADO.NET nel progetto SSIS. In alternativa, è possibile usare direttamente un override di proprietà con il percorso della proprietà **\Package.Connections[{nome della gestione connessione}].Properties[ConnectUsingManagedIdentity]** in fase di esecuzione.

## <a name="see-also"></a>Vedere anche  
 [Connessioni in Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md)  
  
