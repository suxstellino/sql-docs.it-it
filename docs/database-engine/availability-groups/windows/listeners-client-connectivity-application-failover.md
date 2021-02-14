---
title: Connettersi a un listener del gruppo di disponibilità
description: Contiene informazioni sulla connessione a un listener del gruppo di disponibilità Always On, ad esempio come connettersi alla replica primaria e a una replica secondaria di sola lettura e come usare TLS/SSL e Kerberos.
ms.custom: contperf-fy21q1
ms.date: 02/27/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], listeners
- read-only routing
- read-intent connections [AlwaysOn Availability Groups]
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], read-only routing
- Availability Groups [SQL Server], client connectivity
ms.assetid: 76fb3eca-6b08-4610-8d79-64019dd56c44
author: cawrites
ms.author: chadam
ms.openlocfilehash: 72005ed42951571d4903e42d128378a42b2bc007
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348447"
---
# <a name="connect-to-an-always-on-availability-group-listener"></a>Connettersi a un listener del gruppo di disponibilità Always On 
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  
Dopo aver [configurato il listener del gruppo di disponibilità](create-or-configure-an-availability-group-listener-sql-server.md), sarà necessario aggiornare la stringa di connessione per connettersi al listener del gruppo di disponibilità Always On. In questo modo il traffico viene indirizzato automaticamente dall'applicazione alla replica desiderata senza dover aggiornare manualmente la stringa di connessione dopo ogni failover. 
  

##  <a name="connect-to-the-primary-replica"></a><a name="ConnectToPrimary"></a> Connettersi alla replica primaria  

Specificare il nome DNS del listener del gruppo di disponibilità nella stringa di connessione per connettersi alla replica primaria per l'accesso in lettura e scrittura. 

Ad esempio, per connettersi alla replica primaria in SQL Server Management Studio tramite il listener, immettere il nome DNS del listener nel campo Nome del server: 

![Connettersi al listener in SSMS](media/listeners-client-connectivity-application-failover/connect-to-listener-in-ssms.png)


Durante un failover, quando la replica primaria viene modificata, le connessioni esistenti al listener vengono disconnesse e le nuove connessioni vengono indirizzate alla nuova replica primaria.  

Esempio di una stringa di connessione di base per il provider ADO.NET (System.Data.SqlClient): 
  
```  
Server=tcp: AGListener,1433;Database=MyDB;Integrated Security=SSPI  
```  

È possibile verificare a quale replica si è attualmente connessi tramite il listener eseguendo il comando Transact-SQL (T-SQL) seguente:

```sql
SELECT @@SERVERNAME
```

Ad esempio, quando SQLVM1 è la replica primaria: 

![Controllare la connettività della replica](media/listeners-client-connectivity-application-failover/replica-server-name.png)


È comunque possibile connettersi direttamente all'istanza di SQL Server usando il nome dell'istanza della replica primaria o secondaria invece di usare il listener del gruppo di disponibilità. Non si avrà tuttavia il vantaggio di poter indirizzare automaticamente le nuove connessioni alla nuova replica primaria corrente. Non si avrà neppure il vantaggio del routing di sola lettura, in cui le connessioni specificate con `read-intent` vengono automaticamente indirizzate alla replica secondaria leggibile. 

##  <a name="connect-to-a-read-only-replica"></a><a name="ConnectToSecondary"></a> Connettersi a una replica di sola lettura 

Con _routing di sola lettura_ si intende il routing automatico delle connessioni al listener in ingresso a una replica secondaria leggibile configurata per consentire carichi di lavoro di sola lettura. 

Le connessioni vengono indirizzate automaticamente alla replica di sola lettura se si verificano le condizioni seguenti: 
 
-   Almeno una replica secondaria viene impostata sull'accesso di sola lettura e ogni replica secondaria di sola lettura e la replica primaria vengono [configurate per supportare il routing di sola lettura](configure-read-only-routing-for-an-availability-group-sql-server.md). 

-   La stringa di connessione fa riferimento a un database coinvolto nel gruppo di disponibilità. In alternativa, è possibile configurare il database come database predefinito nell'account di accesso usato nella connessione. Per altre informazioni, vedere [questo articolo sul funzionamento dell'algoritmo con il routing di sola lettura](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson).

-   La stringa di connessione fa riferimento a un listener del gruppo di disponibilità e la finalità dell'applicazione della connessione in ingresso è impostata su sola lettura, ad esempio con la parola chiave **Application Intent=ReadOnly** nelle proprietà o negli attributi di connessione oppure nelle stringhe di connessione ODBC o OLEDB. 

L'attributo della finalità dell'applicazione viene archiviato nella sessione del client durante l'accesso. L'istanza di SQL Server elaborerà questa finalità e determinerà come procedere in base alla configurazione del gruppo di disponibilità e allo stato in lettura e scrittura corrente del database di destinazione nella replica secondaria.  

Ad esempio, per connettersi a una replica di sola lettura usando SQL Server Management Studio, selezionare **Opzioni** nella finestra di dialogo **Connetti al server**, selezionare la scheda **Parametri aggiuntivi per la connessione** e quindi specificare `ApplicationIntent=ReadOnly` nella casella di testo:

![Connessione di sola lettura in SSMS](media/listeners-client-connectivity-application-failover/read-only-intent-in-ssms.png)
  
Esempio di una stringa di connessione per il provider ADO.NET (System.Data.SqlClient) che specifica la finalità dell'applicazione in sola lettura: 

```  
Server=tcp:AGListener;Database=AdventureWorks;Integrated Security=SSPI;ApplicationIntent=ReadOnly  
```  
  
Per altre informazioni, vedere [Configurare l'accesso in sola lettura in una replica di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  

## <a name="non-default-port"></a>Porta non predefinita

Quando si crea il listener, si designa una porta da usare per il listener. Se la porta è la porta predefinita 1433, non è necessario specificare un numero di porta quando ci si connette al listener. Se tuttavia la porta non è la 1433, deve essere specificata nella stringa di connessione con il formato `listenername,port`, ad esempio:

![Connettersi con una porta non predefinita](media/listeners-client-connectivity-application-failover/specify-port-in-ssms.png)

Esempio di una stringa di connessione per il provider ADO.NET (System.Data.SqlClient) che specifica una porta non predefinita per il listener: 

```  
Server=tcp:AGListener,1445;Database=AdventureWorks;Integrated Security=SSPI 
```  

##  <a name="bypass-listeners"></a><a name="BypassAGl"></a> Ignorare i listener  

Se i listener dei gruppi di disponibilità consentono il supporto per il reindirizzamento del failover e il routing in sola lettura, le connessioni client non devono utilizzarli. Una connessione client può anche fare riferimento all'istanza di SQL Server anziché stabilire la connessione al listener del gruppo di disponibilità.  
  
Per l'istanza di SQL Server è irrilevante se una connessione effettua l'accesso tramite il listener del gruppo di disponibilità o un altro endpoint dell'istanza.  L'istanza di SQL Server verificherà lo stato del database di destinazione e consentirà o meno la connettività in base alla configurazione del gruppo di disponibilità e lo stato corrente del database sull'istanza.  Ad esempio, se un'applicazione client si connette direttamente a un'istanza della porta di SQL Server e a un database di destinazione ospitato in un gruppo di disponibilità e il database di destinazione si trova nello stato primario ed è online, la connettività ha esito positivo.  Se il database di destinazione è offline o si trova in uno stato transizionale, la connettività al database ha esito negativo.  
  
In alternativa, durante la migrazione dal mirroring del database a [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], le applicazioni possono specificare la stringa di connessione per il mirroring del database purché esista una sola replica secondaria e non siano consentite le connessioni utente. 

##  <a name="database-mirroring-connection-strings"></a><a name="DbmConnectionString"></a> Stringhe di connessione per il mirroring del database 
 Se un gruppo di disponibilità include solo una replica di disponibilità ed è configurato ALLOW_CONNECTIONS = READ_ONLY o ALLOW_CONNECTIONS = NONE per la replica secondaria, i client possono connettersi alla replica primaria tramite una stringa di connessione per il mirroring del database. Questo approccio può essere utile durante la migrazione di un'applicazione esistente dal mirroring del database a un gruppo di disponibilità, a condizione che il gruppo di disponibilità sia limitato a due repliche di disponibilità (una replica primaria e una replica secondaria). Se si aggiungono altre repliche di disponibilità, è necessario creare un listener per il gruppo di disponibilità e aggiornare le applicazioni affinché utilizzino il nome DNS del listener del gruppo di disponibilità.  
  
 Quando si utilizzano le stringhe di connessione per il mirroring del database, il client può utilizzare [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client o il provider di dati .NET Framework per [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. La stringa di connessione fornita da un client deve specificare almeno il nome di un'istanza del server, ovvero il *nome del partner iniziale*, per identificare l'istanza del server che ospita inizialmente la replica di disponibilità alla quale si desidera connettersi. Facoltativamente, la stringa di connessione può fornire anche il nome di un'altra istanza del server, il *nome del partner di failover*, per identificare l'istanza del server che ospita inizialmente la replica secondaria come nome del partner di failover.  
  
 Per altre informazioni sulle stringhe di connessione per il mirroring del database, vedere [Connettere client a una sessione di mirroring del database &#40;SQL Server&#41;](../../../database-engine/database-mirroring/connect-clients-to-a-database-mirroring-session-sql-server.md).  
  
  
##  <a name="multi-subnet-failovers"></a><a name="SupportAgMultiSubnetFailover"></a> Failover su più subnet  
 
 Se si usano librerie client che supportano l'opzione di connessione MultiSubnetFailover nella stringa di connessione, è possibile ottimizzare il failover del gruppo di disponibilità su un'altra subnet impostando MultiSubnetFailover su "True" o "Yes", a seconda della sintassi del provider in uso.  
  
> [!NOTE]  
>  È consigliabile utilizzare questa impostazione per le connessioni con una o più subnet a listener di gruppi di disponibilità e a istanze del cluster di failover di SQL Server.  Abilitando questa opzione si aggiungono ulteriori ottimizzazioni, anche per scenari con una sola subnet.  
  
 L'opzione di connessione **MultiSubnetFailover** funziona unicamente con il protocollo di rete TCP ed è supportata solo in caso di connessione a un listener del gruppo di disponibilità e per qualsiasi nome di rete virtuale che si connette a [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  
 Di seguito è riportato un esempio di una stringa di connessione per il provider ADO.NET (System.Data.SqlClient) che consente il failover su più subnet:  
  
```  
Server=tcp:AGListener,1433;Database=AdventureWorks;Integrated Security=SSPI; MultiSubnetFailover=True  
```  
  
 L'opzione di connessione **MultiSubnetFailover** deve essere impostata su **True** anche se il gruppo di disponibilità si estende su una sola subnet.  In questo modo è possibile preconfigurare nuovi client affinché supportino l'espansione futura su più subnet senza necessità di modificare la stringa di connessione client, oltre a ottimizzare le prestazioni dei failover su una sola subnet.  L'opzione di connessione **MultiSubnetFailover** non è obbligatoria, ma permette di accelerare il failover su subnet.  Il driver client tenta infatti di aprire un socket TCP per ogni indirizzo IP in parallelo associato al gruppo di disponibilità.  Il driver client attende che il primo indirizzo IP risponda, quindi utilizza tale risposta per la connessione.  
  
##  <a name="listeners--tlsssl-certificates"></a><a name="SSLcertificates"></a> Listener e certificati TLS/SSL  

Quando ci si connette a un listener del gruppo di disponibilità, se le istanze di SQL Server usano i certificati TLS/SSL insieme alla crittografia della sessione, il driver client che tenta la connessione deve supportare il nome soggetto alternativo nel certificato TLS/SSL per forzare la crittografia.  Il supporto del driver SQL Server per il Nome soggetto alternativo del certificato è pianificato per ADO.NET (SqlClient), Microsoft JDBC e SQL Native Client (SNAC).  
  
È necessario configurare un certificato X.509 per ogni nodo server che partecipa nel cluster di failover con un elenco di tutti i listener dei gruppi di disponibilità impostati nel Nome soggetto alternativo del certificato. 

Il formato dei valori del certificato è: 

```  
CN = Server.FQDN  
SAN = Server.FQDN,Listener1.FQDN,Listener2.FQDN
```

Si supponga, ad esempio, di avere i valori seguenti: 

```
Servername: Win2019   
Instance: SQL2019   
AG: AG2019   
Listener: Listener2019   
Domain: contoso.com  (which is also the FQDN)
```

Per un cluster WSFC che ha un singolo gruppo di disponibilità, il certificato deve avere il nome di dominio completo (FQDN) del server e del listener: 

```
CN: Win2019.contoso.com
SAN: Win2019.contoso.com, Listener2019.contoso.com 
```

Con questa configurazione, le connessioni verranno crittografate quando ci si connette all'istanza (`WIN2019\SQL2019`) o al listener (`Listener2019`). 

A seconda della configurazione della rete, un piccolo sottoinsieme di clienti potrebbe dover aggiungere anche NetBIOS al nome alternativo del soggetto. In tal caso, i valori del certificato devono essere: 

```
CN: Win2019.contoso.com
SAN: Win2019,Win2019.contoso.com,Listener2019,Listener2019.contoso.com
```

Se il cluster WSFC ha tre listener del gruppo di disponibilità, ad esempio: Listener1, Listener2, Listener3

I valori del certificato devono essere: 

```
CN: Win2019.contoso.com
SAN: Win2019.contoso.com,Listener1.contoso.com,Listener2.contoso.com,Listener3.contoso.com
```
  
  
##  <a name="listeners-and-kerberos-spns"></a><a name="SPNs"></a> Listener e Kerberos (SPN) 

Un amministratore di dominio deve configurare un nome dell'entità servizio (SPN) in Active Directory per ogni listener del gruppo di disponibilità per abilitare Kerberos per le connessioni client al listener. Per registrare il nome SPN, è necessario usare l'account del servizio dell'istanza del server che ospita la replica di disponibilità. Per fare in modo che il nome SPN funzioni su tutte le repliche, è necessario riutilizzare lo stesso account del servizio per tutte le istanze nel cluster WSFC che ospita il gruppo di disponibilità.  
  
 Usare lo strumento della riga di comando di Windows **setspn** per configurare il nome SPN.  Ad esempio, per configurare un nome SPNr per un gruppo di disponibilità denominato `AG1listener.Adventure-Works.com` ospitato in un set di istanze di SQL Server configurate per essere eseguite nell'account di dominio `corp\svclogin2`:  
  
```  
setspn -A MSSQLSvc/AG1listener.Adventure-Works.com:1433 corp\svclogin2  
```  
  
 Per ulteriori informazioni sulla registrazione manuale di un nome SPN per SQL Server, vedere [Registrazione di un nome dell'entità servizio per le connessioni Kerberos](../../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md).  
  


## <a name="next-steps"></a>Passaggi successivi

Dopo aver stabilito la connessione al listener, provare a eseguire l'offload dei [carichi di lavoro di sola lettura](overview-of-always-on-availability-groups-sql-server.md) e dei [backup](configure-backup-on-availability-replicas-sql-server.md) nella replica secondaria per migliorare le prestazioni. È anche possibile esaminare diverse [strategie di monitoraggio dei gruppi di disponibilità](monitoring-of-availability-groups-sql-server.md) per garantire l'integrità del gruppo di disponibilità. 

Per altre informazioni sui gruppi di disponibilità, vedere [Panoramica dei gruppi di disponibilità Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).