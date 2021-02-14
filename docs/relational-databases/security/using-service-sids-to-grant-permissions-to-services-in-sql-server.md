---
description: Uso dei SID dei servizi per concedere autorizzazioni ai servizi in SQL Server
title: Uso dei SID dei servizi per concedere autorizzazioni ai servizi
ms.custom: seo-dt-2019
author: randomnote1
ms.author: dareist
ms.date: 05/02/2019
ms.topic: conceptual
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.openlocfilehash: 9d0c008c3fa5df37ddb2658607d1966a3e1b3164
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077535"
---
# <a name="using-service-sids-to-grant-permissions-to-services-in-sql-server"></a>Uso dei SID dei servizi per concedere autorizzazioni ai servizi in SQL Server

SQL Server usa [ID di sicurezza (SID) per servizio](https://support.microsoft.com/help/2620201/sql-server-uses-a-service-sid-to-provide-service-isolation) (chiamati anche entità di sicurezza di servizio) per consentire l'assegnazione di autorizzazioni direttamente a un servizio specifico. In particolare, in SQL Server vengono usati per concedere le autorizzazioni al motore e ai servizi agente (rispettivamente NT SERVICE\MSSQL$<InstanceName> e NT SERVICE\SQLAGENT$<InstanceName>). In questo modo tali servizi possono accedere al motore di database solo quando i servizi sono in esecuzione.

È possibile usare questo stesso metodo quando si concedono autorizzazioni ad altri servizi. L'uso di un SID del servizio consente di eliminare il sovraccarico associato alla gestione e alla manutenzione degli account del servizio e offre un controllo più rigoroso e granulare sulle autorizzazioni concesse alle risorse di sistema.

Ecco alcuni esempi di servizi in cui è possibile usare un SID del servizio:

- Servizio integrità di System Center Operations Manager (NT SERVICE\HealthService)
- Servizio Windows Server Failover Clustering (WSFC) (NT SERVICE\ClusSvc)

Per impostazione predefinita, ad alcuni servizi non è assegnato alcun SID. Per creare il SID del servizio, è necessario usare [SC.exe](/windows/desktop/services/configuring-a-service-using-sc). [Questo metodo](https://kevinholman.com/2016/08/25/sql-mp-run-as-accounts-no-longer-required/) è stato adottato dagli amministratori di Microsoft System Center Operations Manager per concedere l'autorizzazione per HealthService in SQL Server.

Dopo aver creato e confermato il SID del servizio, è necessario concedergli l'autorizzazione in SQL Server. Per concedere l'autorizzazione, creare un account di accesso di Windows usando [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) o una query. Dopo aver creato l'account di accesso, è possibile concedergli le autorizzazioni, aggiungerlo ai ruoli ed eseguirne il mapping ai database come qualsiasi altro account di accesso.

> [!TIP]
> Se viene restituito l'errore `Login failed for user 'NT AUTHORITY\SYSTEM'`, verificare che il SID del servizio sia disponibile per il servizio desiderato, che l'account di accesso del SID del servizio sia stato creato in SQL Server e che al SID siano state concesse le autorizzazioni appropriate in SQL Server.

## <a name="security"></a>Sicurezza

### <a name="eliminate-service-accounts"></a>Eliminazione degli account del servizio

Gli account del servizio sono stati usati in genere per consentire ai servizi di accedere a SQL Server. La gestione degli account del servizio è però più complessa perché è necessario mantenere e aggiornare periodicamente la password dell'account. Le credenziali dell'account del servizio potrebbero inoltre essere usate da un individuo che prova a mascherare le proprie attività mentre esegue azioni nell'istanza.

### <a name="granular-permissions-to-system-accounts"></a>Autorizzazioni granulari per gli account di sistema

Per concedere le autorizzazioni agli account di sistema, viene creato un account di accesso per gli account [LocalSystem](/windows/win32/services/localsystem-account) ([NT AUTHORITY\SYSTEM in en-us](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md#Localized_service_names)) o [NetworkService](/windows/desktop/Services/networkservice-account) ([NT AUTHORITY\NETWORK SERVICE in en-us](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md#Localized_service_names)), ai quali vengono quindi concesse le autorizzazioni. Con questo metodo tutti i processi o servizi ottengono le autorizzazioni per SQL, che viene eseguito con un account di sistema.

Con un SID del servizio è invece possibile concedere le autorizzazioni per un servizio specifico. Il servizio può accedere solo alle risorse per le quali ha ottenuto le autorizzazioni quando è in esecuzione. Se, ad esempio, `HealthService` viene eseguito con l'account `LocalSystem` e ottiene l'autorizzazione `View Server State`, `LocalSystem` disporrà solo dell'autorizzazione `View Server State` mentre è in esecuzione nel contesto di `HealthService`. Se un qualsiasi altro processo prova ad accedere allo stato del server di SQL con l'account `LocalSystem`, l'accesso verrà negato.

## <a name="examples"></a>Esempi

### <a name="a-create-a-service-sid"></a>R. Creare un SID del servizio

Il comando di PowerShell seguente consente di creare un SID del servizio nel servizio integrità di System Center Operations Manager.

```PowerShell
sc.exe --% sidtype "HealthService" unrestricted
```

> [!IMPORTANT]
> `--%` indica a PowerShell di arrestare l'analisi della parte rimanente del comando. Questo comportamento è utile quando si usano comandi e applicazioni legacy.

### <a name="b-query-a-service-sid"></a>B. Eseguire query su un SID del servizio

Per controllare un SID del servizio o per verificarne l'esistenza, eseguire il comando seguente in PowerShell.

```PowerShell
sc.exe --% qsidtype "HealthService"
```

> [!IMPORTANT]
> `--%` indica a PowerShell di arrestare l'analisi della parte rimanente del comando. Questo comportamento è utile quando si usano comandi e applicazioni legacy.

### <a name="c-add-a-newly-created-service-sid-as-a-login"></a>C. Aggiungere un SID del servizio appena creato come account di accesso

L'esempio seguente consente di creare un account di accesso per il servizio integrità di System Center Operations Manager con T-SQL.

```SQL
CREATE LOGIN [NT SERVICE\HealthService] FROM WINDOWS
GO
```

### <a name="d-add-an-existing-service-sid-as-a-login"></a>D. Aggiungere un SID del servizio esistente come account di accesso

L'esempio seguente consente di creare un account di accesso per Servizio cluster con T-SQL. La concessione delle autorizzazioni direttamente al Servizio cluster consente di evitare di concedere autorizzazioni eccessive all'account SYSTEM.

```SQL
CREATE LOGIN [NT SERVICE\ClusSvc] FROM WINDOWS
GO
```

### <a name="e-grant-permissions-to-a-service-sid"></a>E. Concedere autorizzazioni a un SID del servizio

Concedere le autorizzazioni necessarie per gestire i gruppi di disponibilità al Servizio cluster.

```SQL
GRANT ALTER ANY AVAILABILITY GROUP TO [NT SERVICE\ClusSvc]
GO

GRANT CONNECT SQL TO [NT SERVICE\ClusSvc]
GO

GRANT VIEW SERVER STATE TO [NT SERVICE\ClusSvc]
GO
```

  > [!NOTE]
  > La rimozione degli account di accesso SID del servizio o la relativa rimozione dal ruolo del server sysadmin può causare problemi per i vari componenti di SQL Server che si connettono al motore di database di SQL Server. Alcuni dei problemi sono i seguenti:
  > - SQL Server Agent non può avviare o connettersi a un servizio SQL Server
  > - Nei programmi di installazione di SQL Server si verifica il problema indicato nell'articolo della Microsoft Knowledge Base seguente: https://mskb.pkisolutions.com/kb/955813
  >
  > Per un'istanza predefinita di SQL Server, è possibile risolvere il problema aggiungendo il SID del servizio usando i comandi Transact-SQL seguenti:
  >
  > ```sql
  > CREATE LOGIN [NT SERVICE\MSSQLSERVER] FROM WINDOWS WITH DEFAULT_DATABASE=[master], DEFAULT_LANGUAGE=[us_english]
  > 
  > ALTER ROLE sysadmin ADD MEMBER [NT SERVICE\MSSQLSERVER]
  > 
  > CREATE LOGIN [NT SERVICE\SQLSERVERAGENT] FROM WINDOWS WITH DEFAULT_DATABASE=[master], DEFAULT_LANGUAGE=[us_english]
  > 
  > ALTER ROLE sysadmin ADD MEMBER [NT SERVICE\SQLSERVERAGENT]
  > ```
  > Per un'istanza denominata di SQL Server, usare i comandi Transact-SQL seguenti:
  > ```sql
  > CREATE LOGIN [NT SERVICE\MSSQL$SQL2019] FROM WINDOWS WITH DEFAULT_DATABASE=[master], DEFAULT_LANGUAGE=[us_english]
  > 
  > ALTER ROLE sysadmin ADD MEMBER [NT SERVICE\MSSQL$SQL2019]
  > 
  > CREATE LOGIN [NT SERVICE\SQLAgent$SQL2019] FROM WINDOWS WITH DEFAULT_DATABASE=[master], DEFAULT_LANGUAGE=[us_english]
  > 
  > ALTER ROLE sysadmin ADD MEMBER [NT SERVICE\SQLAgent$SQL2019]
  > 
  > ```
  > In questo esempio `SQL2019` è il nome dell'istanza di SQL Server.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sulla struttura del SID del servizio, vedere [SERVICE_SID_INFO structure](/windows/win32/api/winsvc/ns-winsvc-service_sid_info) (Struttura SERVICE_SID_INFO).

Vedere le altre opzioni disponibili quando [si crea un account di accesso](../../t-sql/statements/create-login-transact-sql.md).

Per usare la sicurezza basata sui ruoli con i SID del servizio, vedere l'articolo sulla [creazione dei ruoli](../../t-sql/statements/create-role-transact-sql.md) in SQL Server.

Scoprire altri modi per [concedere le autorizzazioni](../../t-sql/statements/grant-transact-sql.md) ai SID del servizio in SQL Server.
