---
title: Configurare il routing di sola lettura per un gruppo di disponibilità
description: Indirizzare automaticamente tutto il traffico di sola lettura a una replica secondaria usando il routing di sola lettura per il gruppo di disponibilità Always On, con Transact-SQL (T-SQL) o PowerShell.
ms.custom: seodec18
ms.date: 02/25/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- read-only routing
- Availability Groups [SQL Server], readable secondary replicas
- Availability Groups [SQL Server], read-only routing
- readable secondary replicas
- Availability Groups [SQL Server], client connectivity
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 7bd89ddd-0403-4930-a5eb-3c78718533d4
author: cawrites
ms.author: chadam
ms.openlocfilehash: 51bbc2416b84f5e3288a1e011d5dde8232c8b699
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343535"
---
# <a name="configure-read-only-routing-for-an-always-on-availability-group"></a>Configurare il routing di sola lettura per un gruppo di disponibilità Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Per configurare un gruppo di disponibilità Always On in modo da supportare il routing di sola lettura in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)], è possibile usare [!INCLUDE[tsql](../../../includes/tsql-md.md)] o PowerShell. Con *routing di sola lettura* si intende la capacità di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] di instradare le richieste di connessione in sola lettura valide a una [replica secondaria leggibile](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md) Always On, ovvero una replica configurata per consentire carichi di lavoro di sola lettura quando viene eseguita nel ruolo secondario. Per supportare il routing di sola lettura, il gruppo di disponibilità deve possedere un [listener del gruppo di disponibilità](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md). I client in sola lettura devono indirizzare le richieste di connessione al listener e le stringhe di connessione del client devono specificare la finalità dell'applicazione come in sola lettura, ovvero devono essere *richieste di connessione con finalità di lettura*.  

Il routing di sola lettura è disponibile in [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] e versioni successive.

> [!NOTE]  
>  Per informazioni su come configurare una replica secondaria leggibile, vedere [Configurare l'accesso in sola lettura in una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md).  

  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
  
-   È necessario che il gruppo di disponibilità disponga di un listener del gruppo di disponibilità. Per altre informazioni, vedere [Creare o configurare un listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
-   È necessario che una o più repliche di disponibilità siano configurate per accettare il routing di sola lettura nel ruolo secondario (cioè devono essere [repliche secondarie leggibili](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)). Per altre informazioni, vedere [Configurare l'accesso in sola lettura in una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md).  
  
-   È necessario essere connessi all'istanza del server che ospita la replica primaria corrente.  

-   Se si usa un account di accesso SQL, assicurarsi che l'account sia configurato correttamente. Per altre informazioni, vedere [Gestione di account di accesso e processi per i database di un gruppo di disponibilità (SQL Server)](logins-and-jobs-for-availability-group-databases.md).
  
##  <a name="what-replica-properties-do-you-need-to-configure-to-support-read-only-routing"></a><a name="RORReplicaProperties"></a> Proprietà della replica da configurare per il supporto del routing di sola lettura  
  
-   Per ogni replica secondaria leggibile che deve supportare il routing di sola lettura, è necessario specificare un *URL di routing di sola lettura*. L'URL viene usato solo quando la replica locale viene eseguita nel ruolo secondario. L'URL di routing di sola lettura deve essere specificato per ogni singola replica in base alle esigenze. Ogni URL di routing di sola lettura viene usato per il routing delle richieste di connessione con finalità di lettura a una replica secondaria leggibile specifica. In genere, a ogni replica secondaria leggibile viene assegnato un URL di routing di sola lettura.  
  
     Per informazioni sul calcolo dell'URL di routing di sola lettura per una replica di disponibilità, vedere [Calcolo di read_only_routing_url per Always On](https://web.archive.org/web/20170512023255/https://blogs.msdn.microsoft.com/mattn/2012/04/25/calculating-read_only_routing_url-for-alwayson/)
  
-   Per ogni replica di disponibilità che deve supportare il routing di sola lettura quando viene eseguita come replica primaria, è necessario specificare un *elenco di routing di sola lettura*. L'elenco di routing di sola lettura viene usato solo quando la replica locale viene eseguita nel ruolo primario. L'elenco deve essere specificato per ogni singola replica in base alle esigenze. In genere, ciascun elenco di routing di sola lettura deve contenere tutti gli URL di routing di sola lettura, con l'URL della replica locale alla fine dell'elenco.  
  
    > [!NOTE]  
    >  Le richieste di connessione con finalità di lettura vengono instradate alla prima voce leggibile disponibile nell'elenco di routing di sola lettura della replica primaria corrente. Tuttavia è supportato il bilanciamento del carico tra le repliche di sola lettura. Per altre informazioni, vedere [Configurare il bilanciamento del carico tra le repliche di sola lettura](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md#loadbalancing).  
  
> [!NOTE]  
>  Per informazioni sui listener del gruppo di disponibilità e altre informazioni sul routing di sola lettura, vedere [Listener del gruppo di disponibilità, connettività client e failover dell'applicazione &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
  
|Attività|Autorizzazioni|  
|----------|-----------------|  
|Per configurare le repliche durante la creazione di un gruppo di disponibilità|Sono necessarie l'appartenenza al ruolo predefinito del server **sysadmin** e l'autorizzazione server CREATE AVAILABILITY GROUP oppure l'autorizzazione ALTER ANY AVAILABILITY GROUP o CONTROL SERVER.|  
|Per modificare una replica di disponibilità|È necessaria l'autorizzazione ALTER AVAILABILITY GROUP nel gruppo di disponibilità, l'autorizzazione CONTROL AVAILABILITY GROUP, l'autorizzazione ALTER ANY AVAILABILITY GROUP o l'autorizzazione CONTROL SERVER.|  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
### <a name="configure-a-read-only-routing-list"></a>Configurare un elenco di routing di sola lettura  
 Usare la procedura seguente per configurare il routing di sola lettura con Transact-SQL. Per un esempio di codice, vedere [Esempio (Transact-SQL)](#TsqlExample), più avanti in questa sezione.  
  
1.  Connettersi all'istanza del server che ospita la replica primaria.  
  
2.  Se si specifica una replica per un nuovo gruppo di disponibilità, usare l'istruzione [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] . Se si aggiunge o modifica una replica per un gruppo di disponibilità esistente, usare l'istruzione [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] .  
  
    -   Per configurare il routing di sola lettura per il ruolo secondario, nella clausola ADD REPLICA o MODIFY REPLICA WITH specificare l'opzione SECONDARY_ROLE, come segue:  
  
         SECONDARY_ROLE **(** READ_ONLY_ROUTING_URL **='** TCP **://** _system-address_ **:** _port_ **')**  
  
         I parametri dell'URL del routing di sola lettura sono i seguenti:  
  
         *system-address*  
         Stringa, ad esempio un nome di sistema, un nome di dominio completo o un indirizzo IP, che identifica in modo univoco il computer di destinazione.  
  
         *port*  
         Numero di porta utilizzato dal motore di database dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
         Esempio:   `SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER01.contoso.com:1433')`  
  
         In una clausola MODIFY REPLICA l'argomento ALLOW_CONNECTIONS è facoltativo se la replica è già configurata per consentire connessioni in sola lettura.  
  
         Per altre informazioni, vedere [Calcolo di Read_only_routing_url per Always On](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson).  
  
    -   Per configurare il routing di sola lettura per il ruolo primario, nella clausola ADD REPLICA o MODIFY REPLICA WITH specificare l'opzione PRIMARY_ROLE, come segue:  
  
         PRIMARY_ROLE **(** READ_ONLY_ROUTING_LIST **=('** _server_ **'** [ **,** ...*n* ] **))**  
  
         dove *server* identifica un'istanza del server in cui viene ospitata una replica secondaria di sola lettura nel gruppo di disponibilità.  
  
         Esempio:   `PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('Server1','Server2'))`  
  
        > [!NOTE]  
        >  È necessario impostare l'URL del routing di sola lettura prima di configurare l'elenco di routing di sola lettura.  
  
###  <a name="configure-load-balancing-across-read-only-replicas"></a><a name="loadbalancing"></a> Configurare il bilanciamento del carico tra le repliche di sola lettura  
 A partire da [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)], è possibile configurare il bilanciamento del carico in un set di repliche di sola lettura. In precedenza, il routing di sola lettura indirizzava sempre il traffico alla prima replica disponibile nell'elenco di routing. Per sfruttare i vantaggi di questa funzionalità, usare un livello di parentesi nidificate per racchiudere le istanze del server **READ_ONLY_ROUTING_LIST** nei comandi **CREATE AVAILABILITY GROUP** o **ALTER AVAILABILITY GROUP** .  
  
 Ad esempio, l'elenco di routing seguente bilancia il carico della richiesta di connessione con finalità di lettura tra due repliche di sola lettura, `Server1` e `Server2`. Le parentesi nidificate che racchiudono questi server identificano il set con carico bilanciato. Se nessuna replica è disponibile in tale set, verrà eseguito il tentativo di connettersi in modo sequenziale alle altre repliche, `Server3` e `Server4`, nell'elenco di routing di sola lettura.  
  
```sql  
READ_ONLY_ROUTING_LIST = (('Server1','Server2'), 'Server3', 'Server4')  
```  
  
 Si noti che ogni voce nell'elenco di routing può essere un set di repliche di sola lettura con carico bilanciato. L'esempio seguente illustra questa operazione.  
  
```sql  
READ_ONLY_ROUTING_LIST = (('Server1','Server2'), ('Server3', 'Server4', 'Server5'), 'Server6')  
```  
  
 È supportato solo un livello di parentesi nidificate.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Esempio (Transact-SQL)  
 Nell'esempio seguente vengono modificate due repliche di disponibilità di un gruppo di disponibilità esistente, `AG1` , per supportare il routing di sola lettura se a una di queste repliche è attualmente assegnato il ruolo primario. Per identificare le istanze del server che ospitano la replica di disponibilità, in questo esempio vengono specificati i nomi delle istanze, `COMPUTER01` e `COMPUTER02`.  
  
```  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER01' WITH   
(SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER01' WITH   
(SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER01.contoso.com:1433'));  
  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER02' WITH   
(SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
ALTER AVAILABILITY GROUP [AG1]  
 MODIFY REPLICA ON  
N'COMPUTER02' WITH   
(SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER02.contoso.com:1433'));  
  
ALTER AVAILABILITY GROUP [AG1]   
MODIFY REPLICA ON  
N'COMPUTER01' WITH   
(PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER02','COMPUTER01')));  
  
ALTER AVAILABILITY GROUP [AG1]   
MODIFY REPLICA ON  
N'COMPUTER02' WITH   
(PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER01','COMPUTER02')));  
GO  
  
```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilizzo di PowerShell  
  
### <a name="configure-a-read-only-routing-list"></a>Configurare un elenco di routing di sola lettura  
 Usare la procedura seguente per configurare il routing di sola lettura con PowerShell. Per un esempio di codice, vedere [Esempio (PowerShell)](#PSExample), più avanti in questa sezione.  
  
1.  Impostare il valore predefinito (**cd**) sull'istanza del server che ospita la replica primaria.  
  
2.  Quando si aggiunge una replica di disponibilità a un gruppo di disponibilità, usare il cmdlet **New-SqlAvailabilityReplica** . Quando si modifica una replica di disponibilità esistente, usare il cmdlet **Set-SqlAvailabilityReplica** . I parametri pertinenti sono i seguenti:  
  
    -   Per configurare il routing di sola lettura per il ruolo secondario, specificare il parametro **ReadonlyRoutingConnectionUrl"** _url_ **"** ,  
  
         dove *url* è il nome di dominio completo (FQDN) e la porta di connettività da usare in caso di routing alla replica per le connessioni di sola lettura. Ad esempio: `-ReadonlyRoutingConnectionUrl "TCP://DBSERVER8.manufacturing.Adventure-Works.com:7024"`  
  
         Per altre informazioni, vedere [Calcolo di Read_only_routing_url per Always On](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson).  
  
    -   Per configurare l'accesso alla connessione per il ruolo primario, specificare **ReadonlyRoutingList"** _server_ **"** [ **,** ...*n* ], dove *server* identifica un'istanza del server che ospita una replica secondaria di sola lettura nel gruppo di disponibilità. Ad esempio: `-ReadOnlyRoutingList "SecondaryServer","PrimaryServer"`  
  
        > [!NOTE]  
        >  È necessario impostare l'URL del routing di sola lettura di una replica prima di configurare il relativo elenco di routing di sola lettura.  
  
    > [!NOTE]  
    >  Per visualizzare la sintassi di un cmdlet, usare il cmdlet **Get-Help** nell'ambiente PowerShell di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
### <a name="set-up-and-use-the-sql-server-powershell-provider"></a>Impostare e usare il provider PowerShell per SQL Server  
  
-   [Provider PowerShell per SQL Server](../../../powershell/sql-server-powershell-provider.md)  
  
-   [Visualizzare la Guida di SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  
  
###  <a name="example-powershell"></a><a name="PSExample"></a> Esempio (PowerShell)  
 Nell'esempio seguente vengono configurate la replica primaria e una replica secondaria in un gruppo di disponibilità per il routing di sola lettura. Innanzi tutto, nell'esempio viene assegnato un URL di routing di sola lettura a ciascuna replica. L'elenco di routing di sola lettura viene quindi impostato sulla replica primaria. Le connessioni la cui proprietà "ReadOnly" è impostata nella stringa di connessione verranno reindirizzate alla replica secondaria. Se la replica secondaria non è leggibile (in base all'impostazione **ConnectionModeInSecondaryRole** ), la connessione verrà nuovamente indirizzata alla replica primaria.  
  
```  
Set-Location SQLSERVER:\SQL\PrimaryServer\default\AvailabilityGroups\MyAg  
$primaryReplica = Get-Item "AvailabilityReplicas\PrimaryServer"  
$secondaryReplica = Get-Item "AvailabilityReplicas\SecondaryServer"  
  
Set-SqlAvailabilityReplica -ReadOnlyRoutingConnectionUrl "TCP://PrimaryServer.domain.com:1433" -InputObject $primaryReplica  
Set-SqlAvailabilityReplica -ReadOnlyRoutingConnectionUrl "TCP://SecondaryServer.domain.com:1433" -InputObject $secondaryReplica  
Set-SqlAvailabilityReplica -ReadOnlyRoutingList "SecondaryServer","PrimaryServer" -InputObject $primaryReplica  
```  
  
##  <a name="follow-up-after-configuring-read-only-routing"></a><a name="FollowUp"></a> Completamento: Dopo la configurazione del routing di sola lettura  
 Una volta configurate la replica primaria corrente e le repliche secondarie leggibili per supportare il routing di sola lettura in entrambi i ruoli, le repliche secondarie leggibili potranno ricevere richieste di connessione con finalità di lettura dai client che si connettono tramite il listener del gruppo di disponibilità.  
  
> [!TIP]  
>  Quando si usa [bcp Utility](../../../tools/bcp-utility.md) o [sqlcmd Utility](../../../tools/sqlcmd-utility.md), è possibile specificare l'accesso in sola lettura a qualsiasi replica secondaria abilitata per l'accesso in sola lettura specificando l'opzione **-K ReadOnly** .  
  
###  <a name="requirements-and-recommendations-for-client-connection-strings"></a><a name="ConnStringReqsRecs"></a> Requisiti e indicazioni per le stringhe di connessione del client  
 Per consentire a un'applicazione client di utilizzare il routing di sola lettura, è necessario che la relativa stringa di connessione soddisfi i requisiti seguenti:  
  
-   Utilizzare il protocollo TCP.  
  
-   Impostare la proprietà o l'attributo della finalità dell'applicazione su readonly.  
  
-   Fare riferimento al listener di un gruppo di disponibilità configurato per supportare il routing di sola lettura.  
  
-   Fare riferimento a un database in tale gruppo di disponibilità.  
  
 È inoltre consigliabile che le stringhe di connessione consentano il failover su più subnet, che supporta un thread client parallelo per ogni replica in ogni subnet. In questo modo di riduce al minimo il tempo di riconnessione del client dopo un failover.  
  
 La sintassi per una stringa di connessione dipende dal provider SQL Server utilizzato da un'applicazione. Nella stringa di connessione di esempio seguente per il Provider di dati .NET Framework 4.0.2 per SQL Server sono illustrate le parti di una stringa di connessione necessarie e consigliate per il funzionamento del routing di sola lettura.  
  
```  
Server=tcp:MyAgListener,1433;Database=Db1;IntegratedSecurity=SSPI;ApplicationIntent=ReadOnly;MultiSubnetFailover=True  
```  
  
 Per altre informazioni sulla finalità dell'applicazione di sola lettura e sul routing di sola lettura, vedere [Listener del gruppo di disponibilità, connettività client e failover dell'applicazione &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
### <a name="if-read-only-routing-is-not-working-correctly"></a>Se il routing di sola lettura non funziona correttamente  
 Per informazioni sulla risoluzione dei problemi di una configurazione di routing di sola lettura, vedere [Il routing di sola lettura non funziona correttamente](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md#ROR).  
  
##  <a name="next-steps"></a><a name="RelatedTasks"></a> Passaggi successivi 
**Per visualizzare le configurazioni del routing di sola lettura**  
  
-   [sys.availability_read_only_routing_lists &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)  
  
-   [sys.availability_replicas &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md) (colonna **read_only_routing_url**)  
  
**Per configurare l'accesso alla connessione client**  
  
-   [Creare o configurare un listener del gruppo di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Configurare l'accesso in sola lettura in una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
**Per utilizzare stringhe di connessione nelle applicazioni**  
  
-   [Supporto di SQL Server Native Client per il ripristino di emergenza a disponibilità elevata](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)  
  
-   [Utilizzo delle parole chiave delle stringhe di connessione con SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)  
  
**Blog:**  
  
-    [Calcolo di read_only_routing_url per Always On](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson)  
  
-    [SQL Server Always On Team Blogs (Blog di SQL Server Always On): blog ufficiale del team di SQL Server Always On](/archive/blogs/sqlalwayson/)  
  
-    [Pagina relativa ai blog del Servizio Supporto Tecnico Clienti per gli ingegneri di SQL Server](/archive/blogs/psssql/)  
  
**White paper:**  
  
-    [Pagina relativa ai white paper Microsoft per SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
-    [Pagina relativa ai white paper del team di consulenza clienti di SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  

**Contenuto aggiuntivo**

- [Panoramica di Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   

- [Repliche secondarie attive: Repliche secondarie leggibili &#40;Gruppi di disponibilità Always On&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   

- [Informazioni sull'accesso alla connessione client per le repliche di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)   
 
- [Listener del gruppo di disponibilità, connettività client e failover dell'applicazione &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)