---
description: ALTER AVAILABILITY GROUP (Transact-SQL)
title: ALTER AVAILABILITY GROUP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/02/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_AVAILABILITY_GROUP_TSQL
- ALTER_AVAILABILITY_TSQL
- ALTER AVAILABILITY GROUP
- ALTER AVAILABILITY
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- ALTER AVAILABILITY GROUP statement
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], Transact-SQL statements
ms.assetid: f039d0de-ade7-4aaf-8b7b-d207deb3371a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 1770f43c8cefe61d112f928894634eeadd82a38f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191090"
---
# <a name="alter-availability-group-transact-sql"></a>ALTER AVAILABILITY GROUP (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica un gruppo di disponibilità AlwaysOn esistente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La maggior parte degli argomenti ALTER AVAILABILITY GROUP sono supportati solo nella replica primaria corrente. Tuttavia gli argomenti JOIN, FAILOVER e FORCE_FAILOVER_ALLOW_DATA_LOSS sono supportati solo in repliche secondarie.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
ALTER AVAILABILITY GROUP group_name   
  {  
     SET ( <set_option_spec> )   
   | ADD DATABASE database_name   
   | REMOVE DATABASE database_name  
   | ADD REPLICA ON <add_replica_spec>   
   | MODIFY REPLICA ON <modify_replica_spec>  
   | REMOVE REPLICA ON <server_instance>  
   | JOIN  
   | JOIN AVAILABILITY GROUP ON <add_availability_group_spec> [ ,...2 ]  
   | MODIFY AVAILABILITY GROUP ON <modify_availability_group_spec> [ ,...2 ]  
   | GRANT CREATE ANY DATABASE  
   | DENY CREATE ANY DATABASE  
   | FAILOVER  
   | FORCE_FAILOVER_ALLOW_DATA_LOSS   
   | ADD LISTENER 'dns_name' ( <add_listener_option> )  
   | MODIFY LISTENER 'dns_name' ( <modify_listener_option> )  
   | RESTART LISTENER 'dns_name'  
   | REMOVE LISTENER 'dns_name'  
   | OFFLINE  
  }  
[ ; ]  
  
<set_option_spec> ::=   
    AUTOMATED_BACKUP_PREFERENCE = { PRIMARY | SECONDARY_ONLY| SECONDARY | NONE }  
  | FAILURE_CONDITION_LEVEL  = { 1 | 2 | 3 | 4 | 5 }   
  | HEALTH_CHECK_TIMEOUT = milliseconds  
  | DB_FAILOVER  = { ON | OFF }   
  | DTC_SUPPORT  = { PER_DB | NONE }  
  | REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT = { integer }
  
<server_instance> ::=   
 { 'system_name[\instance_name]' | 'FCI_network_name[\instance_name]' }  
  
<add_replica_spec>::=  
  <server_instance> WITH  
    (  
       ENDPOINT_URL = 'TCP://system-address:port',  
       AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT | CONFIGURATION_ONLY },  
       FAILOVER_MODE = { AUTOMATIC | MANUAL }   
       [ , <add_replica_option> [ ,...n ] ]  
    )   
  
  <add_replica_option>::=  
       SEEDING_MODE = { AUTOMATIC | MANUAL }  
     | BACKUP_PRIORITY = n  
     | SECONDARY_ROLE ( {   
            [ ALLOW_CONNECTIONS = { NO | READ_ONLY | ALL } ]   
        [,] [ READ_ONLY_ROUTING_URL = 'TCP://system-address:port' ]  
     } )  
     | PRIMARY_ROLE ( {   
            [ ALLOW_CONNECTIONS = { READ_WRITE | ALL } ]   
        [,] [ READ_ONLY_ROUTING_LIST = { ( '<server_instance>' [ ,...n ] ) | NONE } ]  
        [,] [ READ_WRITE_ROUTING_URL = { ( '<server_instance>' ) ] 
     } )  
     | SESSION_TIMEOUT = integer
  
<modify_replica_spec>::=  
  <server_instance> WITH  
    (    
       ENDPOINT_URL = 'TCP://system-address:port'   
     | AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT }   
     | FAILOVER_MODE = { AUTOMATIC | MANUAL }   
     | SEEDING_MODE = { AUTOMATIC | MANUAL }   
     | BACKUP_PRIORITY = n  
     | SECONDARY_ROLE ( {   
          ALLOW_CONNECTIONS = { NO | READ_ONLY | ALL }   
        | READ_ONLY_ROUTING_URL = 'TCP://system-address:port'   
          } )  
     | PRIMARY_ROLE ( {   
          ALLOW_CONNECTIONS = { READ_WRITE | ALL }   
        | READ_ONLY_ROUTING_LIST = { ( '<server_instance>' [ ,...n ] ) | NONE }   
          } )  
     | SESSION_TIMEOUT = seconds  
    )   
  
<add_availability_group_spec>::=  
 <ag_name> WITH  
    (  
       LISTENER_URL = 'TCP://system-address:port',  
       AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT },  
       FAILOVER_MODE = MANUAL,  
       SEEDING_MODE = { AUTOMATIC | MANUAL }  
    )  
  
<modify_availability_group_spec>::=  
 <ag_name> WITH  
    (  
       LISTENER = 'TCP://system-address:port'  
       | AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT }  
       | SEEDING_MODE = { AUTOMATIC | MANUAL }  
    )  
  
<add_listener_option> ::=  
   {  
      WITH DHCP [ ON ( <network_subnet_option> ) ]  
    | WITH IP ( { ( <ip_address_option> ) } [ , ...n ] ) [ , PORT = listener_port ]  
   }  
  
  <network_subnet_option> ::=  
     'ipv4_address', 'ipv4_mask'    
  
  <ip_address_option> ::=  
     {   
        'four_part_ipv4_address', 'four_part_ipv4_mask'  
      | 'ipv6_address'  
     }  
  
<modify_listener_option>::=  
    {  
       ADD IP ( <ip_address_option> )   
     | PORT = listener_port  
    }  
  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *group_name*  
 Specifica il nome del nuovo gruppo di disponibilità. *group_name* deve essere un identificatore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valido e deve essere univoco in tutti i gruppi di disponibilità nel cluster WSFC.  
  
 AUTOMATED_BACKUP_PREFERENCE **=** { PRIMARY \| SECONDARY_ONLY \| SECONDARY \| NONE }  
 Specifica le preferenze per la modalità di valutazione della replica primaria da parte di un processo di backup nella scelta della posizione in cui eseguire i backup. È possibile generare uno script affinché un processo di backup specifico prenda in considerazione le preferenze di backup automatico. È importante comprendere che le preferenze non vengono applicate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], pertanto non influiscono sui backup ad hoc.  
  
 Supportato solo nella replica primaria.  
  
 Sono disponibili i valori seguenti:  
  
 PRIMARY  
 Specifica che i backup devono essere sempre eseguiti sulla replica primaria. Questa opzione è utile se sono necessarie funzionalità di backup, ad esempio la creazione di backup differenziali, che non sono supportate quando il backup viene eseguito su una replica secondaria.  
  
> [!IMPORTANT]  
>  Se si intende usare il log shipping per preparare un qualsiasi database secondario per un gruppo di disponibilità, impostare la preferenza di backup automatico su **Primario** finché tutti i database secondari non saranno stati preparati e uniti in join al gruppo di disponibilità.  
  
 SECONDARY_ONLY  
 Specifica che i backup non devono mai essere eseguiti sulla replica primaria. Se la replica primaria è l'unica replica online, il backup non viene eseguito.  
  
 SECONDARY  
 Specifica che i backup devono essere eseguiti su una replica secondaria tranne quando la replica primaria è l'unica replica online. In tal caso il backup deve essere eseguito sulla replica primaria. Questo è il comportamento predefinito.  
  
 NONE  
 Specifica che si preferisce che i processi di backup ignorino il ruolo delle repliche di disponibilità nella scelta della replica per l'esecuzione dei backup. Si noti che i processi di backup potrebbero valutare altri fattori, ad esempio la priorità di backup di ogni replica di disponibilità in combinazione con lo stato operativo e lo stato connesso.  
  
> [!IMPORTANT]  
>  Non è prevista l'applicazione dell'impostazione AUTOMATED_BACKUP_PREFERENCE. L'interpretazione di questa preferenza dipende dall'eventuale logica su cui si basano gli script dei processi di backup per i database in un determinato gruppo di disponibilità. L'impostazione relativa alle preferenze di backup automatico non incide sui backup ad hoc. Per altre informazioni, vedere [Configurare il backup su repliche di disponibilità &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md).  
  
> [!NOTE]  
>  Per visualizzare le preferenze di backup automatico di un gruppo di disponibilità esistente, selezionare la colonna **automated_backup_preference** o **automated_backup_preference_desc** della vista del catalogo [sys.availability_groups](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md). Si può anche usare [fn_hadr_backup_is_preferred_replica &#40;Transact-SQL&#41; ](../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md) per determinare la replica di backup preferita.  Questa funzione restituirà sempre 1 per almeno una delle repliche, anche quando `AUTOMATED_BACKUP_PREFERENCE = NONE`.  
  
 FAILURE_CONDITION_LEVEL **=** { 1 \| 2 \| **3** \| 4 \| 5 }  
 Specifica le condizioni di errore che attivano un failover automatico per il gruppo di disponibilità. FAILURE_CONDITION_LEVEL viene impostato a livello di gruppo ma è rilevante solo nelle repliche di disponibilità configurate per la modalità di disponibilità con commit sincrono (AVAILABILITY_MODE **=** SYNCHRONOUS_COMMIT). Le condizioni di errore possono attivare anche un failover automatico solo se entrambe le repliche, primaria e secondaria, sono configurate per la modalità di failover automatico (FAILOVER_MODE **=** AUTOMATIC) e la replica secondaria è attualmente sincronizzata con la replica primaria.  
  
 Supportato solo nella replica primaria.  
  
 I livelli delle condizioni di errore (1-5) vanno dal livello 1, meno restrittivo, al livello 5, più restrittivo. Un livello della condizione specifico include tutti i livelli meno restrittivi. Il livello della condizione più restrittivo, ovvero il livello 5, include pertanto i quattro livelli della condizione meno restrittivi (1-4), il livello 4 include i livelli 1-3 e così via. Nella tabella seguente viene descritta la condizione di errore che corrisponde a ciascun livello.  
  
|Level|Condizione di errore|  
|-----------|-----------------------|  
|1|Specifica che deve essere avviato un failover automatico quando si verifica una delle condizioni seguenti:<br /><br /> Il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è attivo.<br /><br /> Il lease del gruppo di disponibilità per la connessione al cluster WSFC scade poiché non viene ricevuto alcun acknowledgement dall'istanza del server. Per altre informazioni, vedere [How It Works: SQL Server Always On Lease Timeout](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268) (Funzionamento: timeout lease di SQL Server Always On).|  
|2|Specifica che deve essere avviato un failover automatico quando si verifica una delle condizioni seguenti:<br /><br /> L'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non si connette al cluster e viene superata la soglia HEALTH_CHECK_TIMEOUT specificata dall'utente del gruppo di disponibilità.<br /><br /> La replica di disponibilità si trova in uno stato di errore.|  
|3|Specifica che deve essere avviato un failover automatico in caso di errori interni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] critici, ad esempio spinlock orfani, gravi violazioni dell'accesso in scrittura o dumping eccessivo.<br /><br /> Questo è il comportamento predefinito.|  
|4|Specifica che deve essere avviato un failover automatico in caso di errori interni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con gravità moderata, ad esempio una condizione persistente di memoria insufficiente nel pool di risorse interno di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|5|Specifica che deve essere avviato un failover automatico in caso di qualsiasi condizione di errore qualificata, tra cui:<br /><br /> Esaurimento dei thread di lavoro del motore SQL.<br /><br /> Rilevamento di un deadlock irrisolvibile.|  
  
> [!NOTE]  
>  L'assenza di risposta da un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alle richieste del client non è rilevante per i gruppi di disponibilità.  
  
 I valori FAILURE_CONDITION_LEVEL e HEALTH_CHECK_TIMEOUT definiscono *criteri di failover flessibili* per un gruppo specifico, fornendo un controllo granulare sulle condizioni che devono causare un failover automatico. Per altre informazioni, vedere [Criteri di failover flessibili per failover automatico di un gruppo di disponibilità &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md).  
  
 HEALTH_CHECK_TIMEOUT **=** *milliseconds*  
 Specifica il tempo di attesa in millisecondi per la restituzione delle informazioni sull'integrità del server da parte della stored procedure di sistema [sp_server_diagnostics](../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) prima che il cluster WSFC presupponga che l'istanza del server sia lenta o non risponda. HEALTH_CHECK_TIMEOUT viene impostato a livello di gruppo ma è rilevante solo nelle repliche di disponibilità configurate per la modalità di disponibilità con commit sincrono che supportano il failover automatico (AVAILABILITY_MODE **=** SYNCHRONOUS_COMMIT).  Un timeout del controllo di integrità può anche attivare un failover automatico solo se entrambe le repliche, primaria e secondaria, sono configurate per la modalità di failover automatico (FAILOVER_MODE **=** AUTOMATIC) e la replica secondaria è attualmente sincronizzata con la replica primaria.  
  
 Il valore predefinito di HEALTH_CHECK_TIMEOUT è 30000 millisecondi (30 secondi). Il valore minimo è 15000 millisecondi (15 secondi) e il valore massimo è 4294967295 millisecondi.  
  
 Supportato solo nella replica primaria.  
  
> [!IMPORTANT]  
>  **sp_server_diagnostics** non esegue controlli di integrità a livello di database.  
  
 DB_FAILOVER  **=** { ON | OFF }  
 Specifica la risposta da accettare quando un database nella replica primaria è offline. Se impostata su ON, qualsiasi stato diverso da ONLINE per un database del gruppo di disponibilità attiva un failover automatico. Se l'opzione è impostata su OFF, viene usata solo l'integrità dell'istanza per attivare il failover automatico.  
 
 Per altre informazioni su questa impostazione, vedere [Opzione di failover di rilevamento dell'integrità a livello di database](../../database-engine/availability-groups/windows/sql-server-always-on-database-health-detection-failover-option.md) 

DTC_SUPPORT  **=** { PER_DB | NONE }  
Specifica se le transazioni distribuite sono abilitate per questo gruppo di disponibilità. Le transazioni distribuite sono supportate solo per i database del gruppo di disponibilità a partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e le transazioni tra database sono supportate solo a partire da [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] SP2. `PER_DB` crea il gruppo di disponibilità con il supporto per queste transazioni e alzerà automaticamente di livello le transazioni tra database che implicano il coinvolgimento dei database del gruppo di disponibilità in transazioni distribuite. `NONE` impedisce l'innalzamento di livello automatico delle transazioni tra database per le transazioni distribuite e non registra il database con un RMID stabile in DTC. Le transazioni distribuite non vengono impedite quando si usa l'impostazione `NONE`, ma il failover del database e il ripristino automatico possono non riuscire in alcune circostanze. Per altre informazioni, vedere [Transazioni tra database non supportate per il mirroring del database o i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md). 
 
> [!NOTE]
> Il supporto per la modifica dell'impostazione DTC_SUPPORT di un gruppo di disponibilità è stato introdotto in [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] Service Pack 2. Questa opzione non può essere usata con le versioni precedenti. Per modificare questa impostazione nelle versioni precedenti di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], è necessario eliminare e creare di nuovo il gruppo di disponibilità.
 
 REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT   
 Opzione introdotta in SQL Server 2017. Consente di impostare un numero minimo di repliche secondarie sincrone necessarie per eseguire il commit prima che la replica primaria esegua il commit di una transazione. Garantisce che le transazioni di SQL Server restino in attesa finché i log delle transazioni non vengono aggiornati con il numero minimo di repliche secondarie. Il valore predefinito è 0, che determina lo stesso comportamento di SQL Server 2016. Il valore minimo è 0. Il valore massimo è il numero di repliche meno 1. Questa opzione è correlata alle repliche in modalità di commit sincrono. Quando le repliche sono in modalità di commit sincrono, le scritture presenti nella replica primaria attendono finché non viene eseguito il commit delle scritture presenti nelle repliche secondarie sincrone nel log delle transazioni del database di replica. Se un'istanza di SQL Server che ospita una replica secondaria sincrona si blocca, l'istanza di SQL Server che ospita la replica primaria contrassegna tale replica secondaria come non sincronizzata e continua. Quando il database che non risponde torna online risulta "non sincronizzato" e la replica viene contrassegnata come non integra finché il database primario la rende di nuovo sincrona. Questa impostazione garantisce che la replica primaria non proceda con l'esecuzione del commit di ogni transazione da parte del numero minimo di repliche. Se il numero minimo delle repliche non è disponibile, i commit nel database primario non riescono. Per il tipo di cluster `EXTERNAL` l'impostazione viene modificata quando il gruppo di disponibilità viene aggiunto a una risorsa cluster. Vedere [Elevata disponibilità e protezione dei dati per le configurazioni di gruppo di disponibilità](../../linux/sql-server-linux-availability-group-ha.md).
  
 ADD DATABASE *database_name*  
 Specifica un elenco di uno o più database utente che si desidera aggiungere al gruppo di disponibilità. Tali database devono risiedere nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui è ospitata la replica primaria corrente. È possibile specificare più database per un gruppo di disponibilità, ma ogni database può appartenere a un solo gruppo di disponibilità. Per informazioni sul tipo di database che un gruppo di disponibilità può supportare, vedere [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md). Per individuare i database locali già appartenenti a un gruppo di disponibilità, vedere la colonna **replica_id** nella vista del catalogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
 Supportato solo nella replica primaria.  
  
> [!NOTE]  
>  Dopo aver creato il gruppo di disponibilità, sarà necessario connettersi a ogni istanza del server in cui è ospitata una replica secondaria, quindi preparare ciascun database secondario e aggiungerlo al gruppo di disponibilità. Per altre informazioni, vedere [Avviare lo spostamento dati su un database secondario Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
 REMOVE DATABASE *database_name*  
 Rimuove il database primario specificato e i database secondari corrispondenti dal gruppo di disponibilità. Supportato solo nella replica primaria.  
  
 Per informazioni sulle attività consigliate da eseguire dopo la rimozione di un database di disponibilità da un gruppo di disponibilità, vedere [Rimuovere un database primario da un gruppo di disponibilità &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md).  
  
 ADD REPLICA ON  
 Specifica da una a otto istanze di SQL Server per ospitare le repliche secondarie in un gruppo di disponibilità.  Ogni replica viene specificata dall'indirizzo della relativa istanza del server seguito da una clausola WITH (...).  
  
 Supportato solo nella replica primaria.  
  
 È necessario creare un join di ogni nuova replica secondaria al gruppo di disponibilità. Per altre informazioni, vedere la descrizione dell'opzione JOIN più avanti in questa sezione.  
  
 \<server_instance>  
 Specifica l'indirizzo dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui viene ospitata una replica. Il formato dell'indirizzo dipende dal fatto che l'istanza sia l'istanza predefinita o un'istanza denominata e se si tratti di un'istanza autonoma o un'istanza del cluster di failover (FCI). La sintassi è la seguente:  
  
 { ' *_sistema*[\\*nome_istanza*]' | '*nome_rete_FCI*[\\*nome_istanza*]' }  
  
 I componenti di questo indirizzo sono i seguenti:  
  
 *_sistema*  
 Nome NetBIOS del computer in cui si trova l'istanza di destinazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il computer deve essere un nodo WSFC.  
  
 *nome_rete_FCI*  
 Nome di rete usato per accedere a un cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Usare questo nome se l'istanza del server partecipa come partner di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'esecuzione di SELECT [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) in un'istanza del server FCI restituisce l'intera stringa '*FCI_network_name*[\\*instance_name*]', che corrisponde al nome completo della replica.  
  
 *instance_name*  
 Nome di un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ospitata da *system_name* o *FCI_network_name* e in cui è abilitato il servizio AlwaysOn . Per un'istanza del server predefinita, *nome_istanza* è facoltativo. Per il nome dell'istanza non viene fatta distinzione tra maiuscole e minuscole. In un'istanza del server autonoma il nome del valore corrisponde al valore restituito dall'esecuzione di SELECT [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md).  
  
 \  
 Separatore usato solo quando si specifica *instance_name* per separarlo da *system_name* o *FCI_network_name*.  
  
 Per informazioni sui prerequisiti per i nodi WSFC e le istanze del server, vedere [Prerequisiti, restrizioni e raccomandazioni per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
 ENDPOINT_URL ='TCP://*system-address*:*port*'  
 Specifica il percorso URL dell'[endpoint del mirroring del database[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nell'istanza di ](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md) in cui verrà ospitata la replica di disponibilità da aggiungere o modificare.  
  
 ENDPOINT_URL è obbligatorio nella clausola ADD REPLICA ON e facoltativo nella clausola MODIFY REPLICA ON.  Per altre informazioni, vedere [Specificare l'URL dell'endpoint quando si aggiunge o si modifica una replica di disponibilità &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md).  
  
 **'** TCP **://** _system-address_ **:** _port_ **'**  
 Specifica un URL per definire un URL di endpoint o un URL di routing di sola lettura. I parametri URL sono i seguenti:  
  
 *system-address*  
 Stringa, ad esempio un nome di sistema, un nome di dominio completo o un indirizzo IP, che identifica in modo univoco il computer di destinazione.  
  
 *port*  
 Numero di porta associato all'endpoint del mirroring dell'istanza del server (per l'opzione ENDPOINT_URL) o numero di porta usato dal [!INCLUDE[ssDE](../../includes/ssde-md.md)] dell'istanza del server (per l'opzione READ_ONLY_ROUTING_URL).  
  
 AVAILABILITY_MODE **=** { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT | CONFIGURATION_ONLY }  
 Specifica se la replica primaria deve attendere la conferma della finalizzazione (scrittura) dei record del log su disco da parte della replica secondaria prima di poter eseguire il commit della transazione in un database primario specifico. Il commit delle transazioni in database diversi della stessa replica primaria può essere eseguito indipendentemente.  
  
 SYNCHRONOUS_COMMIT  
 Specifica che la replica primaria esegue il commit delle transazioni solo dopo la finalizzazione nella replica secondaria (modalità con commit sincrono). È possibile specificare SYNCHRONOUS_COMMIT per un massimo di tre repliche, inclusa la replica primaria.  
  
 ASYNCHRONOUS_COMMIT  
 Specifica che la replica primaria esegue il commit delle transazioni senza attendere la finalizzazione del log da parte della replica secondaria (modalità di disponibilità con commit sincrono). È possibile specificare ASYNCHRONOUS_COMMIT per un massimo di cinque repliche di disponibilità, inclusa la replica primaria.  

 CONFIGURATION_ONLY specifica che la replica primaria esegue il commit in modo sincrono dei metadati di configurazione del gruppo di disponibilità nel database master in questa replica. La replica non conterrà dati utente. Questa opzione:

- Può essere ospitata in qualsiasi edizione di SQL Server, inclusa la versione Express Edition.
- È necessario che l'endpoint del mirroring dei dati della replica CONFIGURATION_ONLY sia di tipo `WITNESS`.
- Non può essere modificata.
- Non è valida se `CLUSTER_TYPE = WSFC`. 

   Per altre informazioni, vedere l'argomento relativo alla [replica di sola configurazione](../../linux/sql-server-linux-availability-group-ha.md).
    
 AVAILABILITY_MODE è obbligatorio nella clausola ADD REPLICA ON e facoltativo nella clausola MODIFY REPLICA ON. Per altre informazioni, vedere [Modalità di disponibilità &#40;gruppi di disponibilità Always On&#41;](../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
  
 FAILOVER_MODE **=** { AUTOMATIC | MANUAL }  
 Specifica la modalità di failover della replica di disponibilità che si sta definendo.  
  
 AUTOMATIC  
 Abilita il failover automatico. AUTOMATIC è supportato solo se si specifica anche AVAILABILITY_MODE = SYNCHRONOUS_COMMIT. È possibile specificare AUTOMATIC per tre repliche di disponibilità, inclusa la replica primaria.  
  
> [!NOTE]  
>  Prima di SQL Server 2016, le repliche del failover automatico erano solo due, inclusa la replica primaria
  
> [!NOTE]  
>  Le istanze del cluster di failover di SQL Server non supportano il failover automatico da gruppi di disponibilità, pertanto le replica di disponibilità ospitate da un'istanza del cluster di failover possono essere configurate solo per il failover manuale.  
  
 MANUAL  
 Abilita il failover manuale o il failover manuale forzato (*failover forzato*) da parte dell'amministratore del database.  
  
 FAILOVER_MODE è obbligatorio nella clausola ADD REPLICA ON e facoltativo nella clausola MODIFY REPLICA ON. Esistono due tipi di failover manuale, failover manuale senza perdita di dati e failover forzato (con possibile perdita di dati), supportati a seconda delle diverse condizioni.  Per altre informazioni, vedere [Failover e modalità di failover &#40;gruppi di disponibilità AlwaysOn&#41;](../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
 SEEDING_MODE **=** { AUTOMATIC | MANUAL }  
 Specifica in che modo viene eseguito il seeding iniziale della replica secondaria.  
  
 AUTOMATIC  
 Abilita il seeding diretto. Questo metodo esegue il seeding della replica secondaria in rete. Non richiede l'esecuzione del backup e il ripristino di una copia del database primario nella replica.  
  
> [!NOTE]  
>  Per il seeding diretto, è necessario consentire la creazione del database in ogni replica secondaria chiamando **ALTER AVAILABILITY GROUP** con l'opzione **GRANT CREATE ANY DATABASE**.  
  
 MANUAL  
 Specifica il seeding manuale (impostazione predefinita). Questo metodo richiede la creazione di un backup del database nella replica primaria e il ripristino manuale del backup nella replica secondaria.  
  
 BACKUP_PRIORITY **=** _n_  
 Specifica la priorità di esecuzione dei backup nella replica rispetto alle altre repliche nello stesso gruppo di disponibilità. Il valore è un numero intero compreso nell'intervallo 0-100. I valori hanno il significato seguente:  
  
-   1..100 indica che la replica di disponibilità potrebbe essere scelta per l'esecuzione dei backup. 1 indica la priorità più bassa e 100 indica la priorità più alta. Se BACKUP_PRIORITY = 1, la replica di disponibilità verrà scelta per l'esecuzione dei backup solo se non sono attualmente disponibili repliche di disponibilità con priorità più alta.  
  
-   0 indica che la replica di disponibilità non verrà mai scelta per l'esecuzione dei backup. Ciò si rivela utile, ad esempio, per una replica di disponibilità remota in cui non si desidera eseguire mai il failover dei backup.  
  
 Per altre informazioni, vedere [Repliche secondarie attive: Backup su repliche secondarie &#40;Gruppi di disponibilità Always On&#41;](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).  
  
 SECONDARY_ROLE **(** ... **)**  
 Specifica le impostazioni specifiche di ruolo che verranno applicate se questa replica di disponibilità è attualmente proprietaria del ruolo secondario (ovvero ogni volta che è una replica secondaria). All'interno delle parentesi, specificare uno o entrambe opzioni per il ruolo secondario. Se si specificano entrambe, usare un elenco delimitato da virgole.  
  
 Le opzioni per il ruolo secondario sono le seguenti:  
  
 ALLOW_CONNECTIONS **=** { NO | READ_ONLY | ALL }  
 Specifica se i database di una determinata replica di disponibilità che esegue il ruolo secondario, ovvero che funge da replica secondaria, possono accettare connessioni dai client, ovvero:  
  
 NO  
 Non sono consentite connessioni utente ai database secondari di questa replica. I database non sono disponibili per l'accesso in lettura. Questo è il comportamento predefinito.  
  
 READ_ONLY  
 Sono consentite solo connessioni ai database nella replica secondaria nei casi in cui la proprietà Finalità dell'applicazione è impostata su **ReadOnly**. Per altre informazioni su questa proprietà, vedere [Using Connection String Keywords with SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
 ALL  
 Sono consentite tutte le connessioni ai database nella replica secondaria per l'accesso in sola lettura.  
  
 Per altre informazioni, vedere [Repliche secondarie attive: Repliche secondarie leggibili &#40;Gruppi di disponibilità Always On&#41;](../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).  
  
 READ_ONLY_ROUTING_URL **='** TCP **://** _system-address_ **:** _port_ **'**  
 Specifica l'URL da usare per il routing delle richieste di connessione con finalità di lettura a questa replica di disponibilità. Si tratta dell'URL sul quale è in ascolto il motore di database di SQL Server. In genere, l'istanza predefinita del motore di database di SQL Server è in ascolto sulla porta TCP 1433.  
  
 Per un'istanza denominata, è possibile ottenere il numero di porta eseguendo una query sulle colonne **port** e **type_desc** della DMV [sys.dm_tcp_listener_states](../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md). L'istanza del server usa il listener Transact-SQL (**type_desc='TSQL'** ).  
  
 Per altre informazioni sul calcolo dell'URL di routing di sola lettura per una replica di disponibilità, vedere [Calcolo di read_only_routing_url per Always On](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson).  
  
> [!NOTE]  
>  Per un'istanza denominata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il listener Transact-SQL deve essere configurato per usare una porta specifica. Per altre informazioni, vedere [Configurazione di un server per l'attesa su una porta TCP specifica &#40;Gestione configurazione SQL Server&#41;](../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md).  
  
 PRIMARY_ROLE **(** ... **)**  
 Specifica le impostazioni specifiche di ruolo che verranno applicate se questa replica di disponibilità è attualmente proprietaria del ruolo primario (ovvero ogni volta che è una replica primaria). All'interno delle parentesi, specificare uno o entrambe opzioni per il ruolo primario. Se si specificano entrambe, usare un elenco delimitato da virgole.  
  
 Le opzioni per il ruolo primario sono le seguenti:  
  
 ALLOW_CONNECTIONS **=** { READ_WRITE | ALL }  
 Specifica il tipo di connessione che i database di una determinata replica di disponibilità che esegue il ruolo primario, ovvero che funge da replica primaria, possono accettare dai client, tra cui:  
  
 READ_WRITE  
 Non sono consentite le connessioni in cui la proprietà di connessione Finalità dell'applicazione è impostata su **ReadOnly** .  Se la proprietà Finalità dell'applicazione è impostata su **Lettura/Scrittura** o se tale proprietà non è impostata, la connessione è consentita. Per altre informazioni sulla proprietà di connessione Finalità dell'applicazione, vedere [Using Connection String Keywords with SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
 ALL  
 Sono consentite tutte le connessioni ai database nella replica primaria. Questo è il comportamento predefinito.  
  
 READ_ONLY_ROUTING_LIST **=** { **('** \<server_instance> **'** [ **,** ...*n* ] **)** | NONE }  
 Specifica un elenco delimitato da virgole di istanze del server in cui sono ospitate repliche di disponibilità per questo gruppo di disponibilità che soddisfano i requisiti seguenti quando vengono eseguite nel ruolo secondario:  
  
-   Sono configurate per consentire tutte le connessioni o connessioni in sola lettura (vedere l'argomento ALLOW_CONNECTIONS dell'opzione SECONDARY_ROLE, sopra).  
  
-   Dispongono del proprio URL del routing di sola lettura (vedere l'argomento READ_ONLY_ROUTING_URL dell'opzione SECONDARY_ROLE, sopra).  
  
 I valori di READ_ONLY_ROUTING_LIST sono i seguenti:  
  
 \<server_instance>  
 Specifica l'indirizzo dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che funge da host per una replica di disponibilità che è una replica secondaria leggibile quando è in esecuzione nel ruolo secondario.  
  
 Usare un elenco delimitato da virgole per specificare tutte le istanze del server che potrebbero ospitare una replica secondaria leggibile. Il routing di sola lettura seguirà l'ordine nel quale le istanze del server vengono specificate nell'elenco. Se si include l'istanza del server host di una replica nell'elenco di routing di sola lettura della replica, il posizionamento di questa istanza del server alla fine dell'elenco costituisce in genere buona pratica, poiché le connessioni con finalità di lettura vengono indirizzate a una replica secondaria, se ne è disponibile una.  
  
 A partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], è possibile eseguire il bilanciamento del carico delle richieste con finalità di lettura in tutte le repliche secondarie leggibili. A questo scopo, inserire le repliche in un set annidato di parentesi all'interno dell'elenco di routing di sola lettura. Per altre informazioni ed esempi, vedere [Configurare il bilanciamento del carico tra le repliche di sola lettura](../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md#loadbalancing).  
  
 NONE  
 Specifica che quando questa replica di disponibilità è la replica primaria, il routing di sola lettura non è supportato. Questo è il comportamento predefinito. In caso di utilizzo con MODIFY REPLICA ON, questo valore disabilita un elenco esistente, se presente.  

 READ_WRITE_ROUTING_URL **=** { **('** \<server_instance> **')** }  
 Si applica a: SQL Server (a partire da SQL Server 2019 (15.x)) 

 Specifica istanze del server in cui sono ospitate repliche di disponibilità per questo gruppo di disponibilità che soddisfano i requisiti seguenti quando vengono eseguite nel ruolo principale:
-   La specifica della replica PRIMARY_ROLE include READ_WRITE_ROUTING_URL.
-   La stringa di connessione può essere impostata come ReadWrite definendo ApplicationIntent come ReadWrite o non impostando ApplicationIntent e consentendo che venga implementata l'impostazione predefinita (ReadWrite).

Per altre informazioni, vedere [Reindirizzamento della connessione in lettura/scrittura da replica secondaria a primaria - Gruppi di disponibilità AlwaysOn](../../database-engine/availability-groups/windows/secondary-replica-connection-redirection-always-on-availability-groups.md).

 SESSION_TIMEOUT **=** _seconds_  
 Specifica il periodo di timeout della sessione in secondi. Se non si specifica questa opzione, il periodo di timeout predefinito è di 10 secondi. Il valore minimo è 5 secondi.  
  
> [!IMPORTANT]  
>  È consigliabile usare un periodo di timeout di almeno 10 secondi.  
  
 Per altre informazioni sul periodo di timeout della sessione, vedere [Panoramica di Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
 MODIFY REPLICA ON  
 Modifica qualsiasi replica del gruppo di disponibilità. L'elenco di repliche da modificare contiene l'indirizzo dell'istanza del server e una clausola WITH (...) per ogni replica.  
  
 Supportato solo nella replica primaria.  
  
 REMOVE REPLICA ON  
 Rimuove la replica secondaria specificata dal gruppo di disponibilità. Non è possibile rimuovere la replica primaria corrente da un gruppo di disponibilità. Nel momento in cui viene rimossa, la replica non riceverà più i dati. I relativi database secondari vengono rimossi dal gruppo di disponibilità e viene attivato lo stato RESTORING.  
  
 Supportato solo nella replica primaria.  
  
> [!NOTE]  
>  Se si rimuove una replica mentre non è disponibile o si trova in stato di errore, quando torna alla modalità online rileverà di non appartenere più al gruppo di disponibilità.  
  
 JOIN  
 Consente all'istanza del server locale di ospitare una replica secondaria nel gruppo di disponibilità specificato.  
  
 Supportato solo in una replica secondaria non ancora aggiunta al gruppo di disponibilità.  
  
 Per altre informazioni, vedere [Creare un join di una replica secondaria a un gruppo di disponibilità &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
 FAILOVER  
Avvia un failover manuale del gruppo di disponibilità senza perdita di dati nella replica secondaria a cui si è connessi. La replica che ospiterà la replica primaria è la *destinazione di failover*.  La destinazione di failover subentrerà al ruolo primario e recupererà la copia di ogni database per portarli online come nuovi database primari. La replica primaria precedente passa contemporaneamente al ruolo secondario e i relativi database diventano database secondari e vengono immediatamente sospesi. Potenzialmente, questi ruoli possono alternati in successione da una serie di errori.  
  
 Supportato solo su una replica secondaria con commit sincrono che è attualmente sincronizzata con la replica primaria. Notare che per la sincronizzazione di una replica secondaria, è necessario che anche la replica primaria sia in esecuzione in modalità con commit sincrono.  
  
> [!NOTE]  
>  Un comando di failover viene eseguito non appena la destinazione di failover avrà accettato il comando. Tuttavia, il recupero del database si verifica in modo asincrono dopo che il gruppo di disponibilità ha completato il failover.  
  
 Per informazioni su limitazioni, prerequisiti e consigli per eseguire un failover manuale pianificato, vedere [Eseguire un failover manuale pianificato di un gruppo di disponibilità &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md).  
  
 FORCE_FAILOVER_ALLOW_DATA_LOSS  
 > [!CAUTION]  
>  L'uso forzato di un failover, che può comportare una perdita di dati, rappresenta esclusivamente un metodo di ripristino di emergenza. Pertanto, è consigliabile forzare il failover solo se la replica primaria non è più in esecuzione, si è disposti a rischiare la perdita di dati ed è necessario ripristinare immediatamente il servizio nel gruppo di disponibilità.  
  
 Supportato solo in una replica il cui ruolo è in uno stato SECONDARY o RESOLVING. --La replica in cui viene eseguito un comando di failover è nota come *destinazione di failover*.  
  
 Forza il failover del gruppo di disponibilità, con possibile perdita di dati, nella destinazione di failover. La destinazione di failover subentrerà al ruolo primario e recupererà la copia di ogni database per portarli online come nuovi database primari. In tutte le repliche secondarie rimanenti, ogni database secondario viene sospeso finché non viene ripristinato manualmente. Quando la replica primaria precedente diventa disponibile, passerà al ruolo secondario e i relativi database diventeranno database secondari sospesi.  
  
> [!NOTE]  
>  Un comando di failover viene eseguito non appena la destinazione di failover avrà accettato il comando. Tuttavia, il recupero del database si verifica in modo asincrono dopo che il gruppo di disponibilità ha completato il failover.  
  
 Per informazioni su limitazioni, prerequisiti e consigli per il failover forzato e sull'effetto di un failover forzato sui database primari precedenti nel gruppo di disponibilità, vedere [Eseguire un failover manuale forzato di un gruppo di disponibilità &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md).  
  
 ADD LISTENER **'** _dns\_name_ **'(** \<add_listener_option> **)**  
 Definisce un nuovo listener per il gruppo di disponibilità. Supportato solo nella replica primaria.  
  
> [!IMPORTANT]
>  Prima di creare il primo listener, è consigliabile leggere l'argomento [Creare o configurare un listener del gruppo di disponibilità &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
> 
>  Dopo aver creato un listener per un gruppo di disponibilità specifico, è consigliabile effettuare le operazioni seguenti:  
> 
>  -   Chiedere all'amministratore di rete di riservare l'indirizzo IP del listener per un uso esclusivo.  
> -   Fornire il nome host DNS del listener agli sviluppatori dell'applicazione in modo da essere usato nelle stringhe di connessione per la richiesta di connessioni client al gruppo di disponibilità.  
  
 *dns_name*  
 Specifica il nome host DNS del listener del gruppo di disponibilità. È necessario che il nome DNS del listener sia univoco nel dominio e in NetBIOS.  
  
 *dns_name* è un valore di stringa. Può contenere solo caratteri alfanumerici, trattini (-) e caratteri di sottolineatura (_), in qualsiasi ordine. Per i nomi host DNS non viene fatta distinzione tra maiuscole e minuscole. La lunghezza massima è di 63 caratteri.  
  
 È consigliabile specificare una stringa significativa. Ad esempio, per un gruppo di disponibilità denominato `AG1`un nome host DNS significativo potrebbe essere `ag1-listener`.  
  
> [!IMPORTANT]  
>  NetBIOS riconosce solo i primi 15 caratteri di dns_name. Se si dispone di due cluster WSFC controllati dallo stesso dominio Active Directory e si tenta di creare listener del gruppo di disponibilità in entrambi i cluster usando nomi con più di 15 caratteri e un prefisso a 15 caratteri identico, verrà restituito un errore in cui si segnala che non è possibile portare online la risorsa del nome di rete virtuale. Per informazioni sulle regole di denominazione dei prefissi per i nomi DNS, vedere [Assegnare nomi ai domini](https://technet.microsoft.com/library/cc731265\(WS.10\).aspx).  
  
 JOIN AVAILABILITY GROUP ON  
 Eseguire l'associazione a un *gruppo di disponibilità distribuito*. Quando si crea un gruppo di disponibilità distribuito, il gruppo di disponibilità nel cluster in cui viene creato diventa il gruppo di disponibilità primario. Il gruppo di disponibilità associato al gruppo di disponibilità distribuito è il gruppo di disponibilità secondario.  
  
 \<ag_name>  
 Specifica il nome del gruppo di disponibilità che costituisce una metà del gruppo di disponibilità distribuito.  
  
 LISTENER **='** TCP **://** _system-address_ **:** _port_ **'**  
 Specifica il percorso URL per il listener associato al gruppo di disponibilità.  
  
 La clausola LISTENER è obbligatoria.  
  
 **'** TCP **://** _system-address_ **:** _port_ **'**  
 Specifica un URL per il listener associato al gruppo di disponibilità. I parametri URL sono i seguenti:  
  
 *system-address*  
 Stringa, ad esempio un nome di sistema, un nome di dominio completo o un indirizzo IP, che identifica in modo univoco il listener.  
  
 *port*  
 Numero di porta associato all'endpoint del mirroring del gruppo di disponibilità. Si noti che questa non è la porta del listener.  
  
 AVAILABILITY_MODE **=** { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT }  
 Specifica se la replica primaria deve attendere che il gruppo di disponibilità secondario confermi la finalizzazione (scrittura) dei record del log su disco prima che la replica primaria esegua il commit della transazione in un dato database primario.  
  
 SYNCHRONOUS_COMMIT  
 Specifica che la replica primaria eseguirà il commit delle transazioni solo dopo che sono state finalizzate nel gruppo di disponibilità secondario. È possibile specificare SYNCHRONOUS_COMMIT per un massimo di due gruppi di disponibilità, incluso il gruppo di disponibilità primario.  
  
 ASYNCHRONOUS_COMMIT  
 Specifica che la replica primaria esegue il commit delle transazioni senza attendere la finalizzazione del log da parte del gruppo di disponibilità secondario. È possibile specificare ASYNCHRONOUS_COMMIT per un massimo di due gruppi di disponibilità, incluso il gruppo di disponibilità primario.  
  
 La clausola AVAILABILITY_MODE è obbligatoria.  
  
 FAILOVER_MODE **=** { MANUAL }  
 Specifica la modalità di failover del gruppo di disponibilità distribuito.  
  
 MANUAL  
 Abilita il failover manuale pianificato o il failover manuale forzato (in genere denominato *failover forzato*) da parte dell'amministratore del database.  
  
 Il failover automatico sul gruppo di disponibilità secondario non è supportato.  
  
 SEEDING_MODE **=** { AUTOMATIC | MANUAL }  
 Specifica in che modo sarà eseguito il seeding iniziale del gruppo di disponibilità secondario.  
  
 AUTOMATIC  
 Abilita il seeding automatico. Questo metodo eseguirà il seeding del gruppo di disponibilità secondario in rete. Non richiede l'esecuzione del backup e il ripristino di una copia del database primario nelle repliche del gruppo di disponibilità secondario.  
  
 MANUAL  
 Specifica il seeding manuale. Questo metodo richiede la creazione di un backup del database nella replica primaria e il ripristino manuale del backup nelle repliche del gruppo di disponibilità secondario.  
  
 MODIFY AVAILABILITY GROUP ON  
 Modifica tutte le impostazioni del gruppo di disponibilità di un gruppo di disponibilità distribuito. Nell'elenco dei gruppi di disponibilità da modificare sono contenuti il nome del gruppo di disponibilità e una clausola WITH (...) per ogni gruppo di disponibilità.  
  
> [!IMPORTANT]  
>  Questo comando deve essere ripetuto sia nel gruppo di disponibilità primario sia nelle istanze del gruppo di disponibilità secondario.  
  
 GRANT CREATE ANY DATABASE  
 Concedere al gruppo di disponibilità l'autorizzazione per creare database per conto della replica primaria, che supporta il seeding diretto (SEEDING_MODE = AUTOMATIC). Questo parametro deve essere eseguito su ogni replica secondaria che supporta il seeding diretto dopo aver aggiunto il gruppo secondario al gruppo di disponibilità.  È necessaria l'autorizzazione CREATE ANY DATABASE.  
  
 DENY CREATE ANY DATABASE  
 Nega al gruppo di disponibilità la possibilità di creare database per conto della replica primaria.  
  
 \<add_listener_option>  
 ADD LISTENER accetta una delle opzioni seguenti:  
  
 WITH DHCP [ ON { **('** _four\_part\_ipv4\_address_ **','** _four\_part\_ipv4\_mask_ **')** } ]  
 Specifica che il listener del gruppo di disponibilità userà il protocollo DHCP (Dynamic Host Configuration Protocol).  Facoltativamente, utilizzare la clausola ON per identificare la rete su cui verrà creato il listener. DHCP è limitato a una sola subnet utilizzata per ogni istanza del server nel cui gruppo di disponibilità è ospitata una replica di disponibilità.  
  
> [!IMPORTANT]  
>  Non è consigliabile utilizzare DHCP negli ambienti di produzione. Se si verifica un periodo di inattività e il lease IP DHCP scade, è necessario del tempo aggiuntivo per registrare il nuovo indirizzo IP della rete DHCP che è associato al nome DNS del listener e influisce sulla connettività client. DHCP può essere tranquillamente usato per la configurazione dell'ambiente di sviluppo e test per verificare le funzioni di base di gruppi di disponibilità e per l'integrazione con le applicazioni.  
  
 Ad esempio:  
  
 `WITH DHCP ON ('10.120.19.0','255.255.254.0')`  
  
 WITH IP **(** { **('** _four\_part\_ipv4\_address_ **','** _four\_part\_ipv4\_mask_ **')**  |  **('** _ipv6\_address_ **')** } [ **,** ..._n_ ] **)** [ **,** PORT **=** _listener\_port_ ]  
 Specifica che, anziché usare DHCP, nel listener del gruppo di disponibilità saranno usati uno o più indirizzi IP statici. Per creare un gruppo di disponibilità tra più subnet, viene richiesto un indirizzo IP statico nella configurazione del listener per ogni subnet. Per una determinata subnet, l'indirizzo IP statico può essere un indirizzo IPv4 o IPv6. Contattare l'amministratore di rete per ottenere un indirizzo IP statico per ogni subnet in cui verrà ospitata una replica di disponibilità per il nuovo gruppo di disponibilità.  
  
 Ad esempio:  
  
 `WITH IP ( ('10.120.19.155','255.255.254.0') )`  
  
 *ipv4_address*  
 Specifica un indirizzo IPv4 in quattro parti per un listener del gruppo di disponibilità, Ad esempio: `10.120.19.155`.  
  
 *ipv4_mask*  
 Specifica una maschera IPv4 in quattro parti per un listener del gruppo di disponibilità, Ad esempio: `255.255.254.0`.  
  
 *ipv6_address*  
 Specifica un indirizzo IPv6 per un listener del gruppo di disponibilità, Ad esempio: `2001::4898:23:1002:20f:1fff:feff:b3a3`.  
  
 PORT **=** *listener_port*  
 Specifica il numero di porta, *listener_port*, che deve essere usato da un listener del gruppo di disponibilità specificato usando una clausola WITH IP. PORT è facoltativo.  
  
 È supportato il numero di porta predefinito, 1433. Tuttavia, per motivi di sicurezza, è consigliabile usare un numero di porta diverso.  
  
 Ad esempio: `WITH IP ( ('2001::4898:23:1002:20f:1fff:feff:b3a3') ) , PORT = 7777`  
  
 MODIFY LISTENER **'** _dns\_name_ **'(** \<modify\_listener\_option\> **)**  
 Modifica un listener del gruppo di disponibilità esistente. Supportato solo nella replica primaria.  
  
 \<modify\_listener\_option\>  
 MODIFY LISTENER accetta una delle opzioni seguenti:  
  
 ADD IP { **('** _four\_part\_ipv4\_address_ **','** _four\_part\_ipv4_mask_ **')** \| <b>('</b>dns\_name *ipv6\_address* __')__ }  
 Aggiunge l'indirizzo IP specificato al listener del gruppo di disponibilità specificato da *dns\_name*.  
  
 PORT **=** *listener_port*  
 Vedere la descrizione di questo argomento più indietro in questa sezione.  
  
 RESTART LISTENER **'** _dns\_name_ **'**  
 Consente di riavviare il listener associato al nome DNS specificato. Supportato solo nella replica primaria.  
  
 REMOVE LISTENER **'** _dns\_name_ **'**  
 Consente di rimuovere il listener associato al nome DNS specificato. Supportato solo nella replica primaria.  
  
 OFFLINE  
 Porta offline un gruppo di disponibilità online. Non si verifica alcuna perdita di dati per i database con commit sincrono.  
  
 Dopo aver portato un gruppo di disponibilità offline, i relativi database non saranno più disponibili per i client e non sarà possibile riportare nuovamente online il gruppo di disponibilità. Usare pertanto l'opzione OFFLINE solo durante la migrazione tra cluster di [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], se si esegue la migrazione di risorse del gruppo di disponibilità a un nuovo cluster WSFC.  
  
 Per altre informazioni, vedere [Portare un gruppo di disponibilità offline &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/take-an-availability-group-offline-sql-server.md).  
  
## <a name="prerequisites-and-restrictions"></a>Prerequisiti e restrizioni  
 Per informazioni sui prerequisiti e sulle restrizioni relative alle repliche di disponibilità e alle istanze server e ai computer host, vedere [Prerequisiti, restrizioni e consigli per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
 Per informazioni sulle restrizioni relative alle istruzioni Transact-SQL AVAILABILITY GROUP, vedere [Panoramica delle istruzioni Transact-SQL per i gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups.md).  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 È necessaria l'autorizzazione ALTER AVAILABILITY GROUP nel gruppo di disponibilità, l'autorizzazione CONTROL AVAILABILITY GROUP, l'autorizzazione ALTER ANY AVAILABILITY GROUP o l'autorizzazione CONTROL SERVER.  È necessaria anche l'autorizzazione ALTER ANY DATABASE.   
  
## <a name="examples"></a>Esempi  
  
###  <a name="a-joining-a-secondary-replica-to-an-availability-group"></a><a name="Join_Secondary_Replica"></a> A. Creazione di un join di una replica secondaria a un gruppo di disponibilità  
 Nell'esempio seguente viene creato un join di una replica secondaria a cui si è connessi al gruppo di disponibilità `AccountsAG`.  
  
```SQL  
ALTER AVAILABILITY GROUP AccountsAG JOIN;  
GO  
```  
  
###  <a name="b-forcing-failover-of-an-availability-group"></a><a name="Force_Failover"></a> B. Failover forzato di un gruppo di disponibilità  
 Nell'esempio seguente viene eseguito il failover forzato del gruppo di disponibilità `AccountsAG` alla replica secondaria a cui si è connessi.  
  
```SQL
ALTER AVAILABILITY GROUP AccountsAG FORCE_FAILOVER_ALLOW_DATA_LOSS;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [ALTER DATABASE SET HADR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-hadr.md)   
 [DROP AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/drop-availability-group-transact-sql.md)   
 [sys.availability_replicas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [sys.availability_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   
 [Risolvere i problemi relativi alla configurazione dei gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)   
 [Panoramica di gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Listener del gruppo di disponibilità, connettività client e failover dell'applicazione &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)  
  
