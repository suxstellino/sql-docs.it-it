---
description: CREATE ROUTE (Transact-SQL)
title: CREATE ROUTE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_ROUTE_TSQL
- ROUTE
- CREATE ROUTE
- ROUTE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- lifetimes [Service Broker]
- routes [Service Broker], creating
- forwarding brokers [SQL Server]
- service names [Service Broker]
- database mirroring [SQL Server], routing
- expired routes [SQL Server]
- activating routes
- CREATE ROUTE statement
ms.assetid: 7e695364-1a98-4cfd-8ebd-137ac5a425b3
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: fc34a3eb5987a7348733755ee69aafda87e16ac3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091960"
---
# <a name="create-route-transact-sql"></a>CREATE ROUTE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]

  Aggiunge una nuova route alla tabella di routing per il database corrente. Per i messaggi in uscita, [!INCLUDE[ssSB](../../includes/sssb-md.md)] determina il routing controllando la tabella di routing nel database locale. Per i messaggi in conversazioni che hanno origine in un'altra istanza, inclusi i messaggi da inoltrare, in [!INCLUDE[ssSB](../../includes/sssb-md.md)] vengono controllate le route nel database **msdb**.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CREATE ROUTE route_name  
[ AUTHORIZATION owner_name ]  
WITH    
   [ SERVICE_NAME = 'service_name', ]  
   [ BROKER_INSTANCE = 'broker_instance_identifier' , ]  
   [ LIFETIME = route_lifetime , ]  
   ADDRESS =  'next_hop_address'  
   [ , MIRROR_ADDRESS = 'next_hop_mirror_address' ]  
[ ; ]  
```  
  
## <a name="arguments"></a>Argomenti  
 *route_name*  
 Nome della route da creare. La nuova route viene creata nel database corrente e diventa di proprietà dell'identità specificata nella clausola AUTHORIZATION. Non è possibile specificare i nomi del server, del database e dello schema. *route_name* deve essere un valore **sysname** valido.  
  
 AUTHORIZATION *owner_name*  
 Imposta il proprietario della route sull'utente o il ruolo di database specificato. Per *owner_name* è possibile specificare il nome di qualsiasi utente o ruolo valido se l'utente corrente è un membro del ruolo predefinito del database **db_owner** o del ruolo predefinito del server **sysadmin**. In caso contrario, *owner_name* deve corrispondere al nome dell'utente corrente, al nome di un utente per il quale l'utente corrente dispone di autorizzazione IMPERSONATE oppure al nome di un ruolo a cui appartiene l'utente corrente. Se la clausola viene omessa, la route appartiene all'utente corrente.  
  
 WITH  
 Introduce le clausole per la definizione della nuova route creata.  
  
 SERVICE_NAME = **'** _service\_name_ **'**  
 Specifica il nome del servizio remoto a cui la route fa riferimento. *service_name* deve corrispondere esattamente al nome usato dal servizio remoto. Per creare corrispondenza con [!INCLUDE[ssSB](../../includes/sssb-md.md)]service_name *,*  usa un confronto byte per byte. In altre parole, nel confronto viene fatta distinzione tra maiuscole e minuscole e non vengono considerate le regole di confronto correnti. Se la clausola SERVICE_NAME viene omessa, per la route viene utilizzato qualsiasi nome di servizio, ma in questo caso la route avrà una priorità di corrispondenza inferiore rispetto a una route per cui si specifica SERVICE_NAME. Una route con nome di servizio **'SQL/ServiceBroker/BrokerConfiguration'** è una route per un servizio di configurazione di Service Broker. Per una route per questo servizio non è necessario specificare un'istanza di Service Broker.  
  
 BROKER_INSTANCE = **'** _broker\_instance\_identifier_ **'**  
 Specifica il database che ospita il servizio di destinazione. Il parametro *broker_instance_identifier* deve corrispondere all'identificatore dell'istanza di Service Broker per il database remoto. Per ottenere tale identificatore, è possibile eseguire la query seguente nel database selezionato:  
  
```sql  
SELECT service_broker_guid  
FROM sys.databases  
WHERE database_id = DB_ID()  
```  
  
 Se si omette la clausola BROKER_INSTANCE, per la route viene utilizzata qualsiasi istanza di Service Broker. Le route impostate per qualsiasi istanza di Service Broker hanno una priorità maggiore di corrispondenza rispetto alle route con un'istanza di Service Broker esplicita quando la conversazione non specifica un'istanza di Service Broker. Per le conversazioni che specificano un'istanza di Service Broker, invece, le route impostate per un'istanza specifica di Service Broker hanno una priorità maggiore rispetto a quelle impostate per qualsiasi istanza.  
  
 LIFETIME **=** _route\_lifetime_  
 Specifica per quanti secondi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mantiene la route nella tabella di routing. Al termine di questo periodo di tempo, la route scade e non viene più presa in considerazione da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per la scelta della route per una nuova conversazione. Se le clausola viene omessa, il valore *route_lifetime* è Null e la route ha durata illimitata.  
  
 ADDRESS **='** _next\_hop\_address_ **'**  
Per Istanza gestita di SQL, `ADDRESS` deve essere locale. 

Specifica l'indirizzo di rete per la route. Il parametro *next_hop_address* specifica un indirizzo TCP/IP nel formato seguente:  
  
 **TCP://** { *dns_name* | *netbios_name* | *ip_address* } **:** _port\_number_  
  
 Il parametro *port_number* specificato deve corrispondere al numero di porta dell'endpoint di [!INCLUDE[ssSB](../../includes/sssb-md.md)] per un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel computer specificato. Per ottenere tale valore, eseguire la query seguente nel database selezionato:  
  
```sql  
SELECT tcpe.port  
FROM sys.tcp_endpoints AS tcpe  
INNER JOIN sys.service_broker_endpoints AS ssbe  
   ON ssbe.endpoint_id = tcpe.endpoint_id  
WHERE ssbe.name = N'MyServiceBrokerEndpoint';  
```  
  
 Se il servizio è ospitato in un database con mirroring, è inoltre necessario specificare l'indirizzo MIRROR_ADDRESS per l'altra istanza che ospita un database con mirroring. In caso contrario, la route non eseguirà il failover sul mirror.  
  
 Se per il parametro **next_hop_address** di una route viene specificato il valore *'LOCAL'* , il messaggio viene recapitato a un servizio nell'istanza corrente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Se per il parametro **next_hop_address** di una route viene specificato il valore *'TRANSPORT'* , l'indirizzo di rete viene determinato in base all'indirizzo di rete nel nome del servizio. Per una route con valore **'TRANSPORT'** non è necessario specificare un nome di servizio o un'istanza di Service Broker.  
  
 MIRROR_ADDRESS **='** _next\_hop\_mirror\_address_ **'**  
 Specifica l'indirizzo di rete di un database con mirroring con un database con mirroring ospitato in *next_hop_address*. Il parametro *next_hop_mirror_address* specifica un indirizzo TCP/IP nel formato seguente:  
  
 **TCP://** { *dns_name* | *netbios_name* | *ip_address* } **:** *port_number*  
  
 Il parametro *port_number* specificato deve corrispondere al numero di porta dell'endpoint di [!INCLUDE[ssSB](../../includes/sssb-md.md)] per un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel computer specificato. Per ottenere tale valore, eseguire la query seguente nel database selezionato:  
  
```sql  
SELECT tcpe.port  
FROM sys.tcp_endpoints AS tcpe  
INNER JOIN sys.service_broker_endpoints AS ssbe  
   ON ssbe.endpoint_id = tcpe.endpoint_id  
WHERE ssbe.name = N'MyServiceBrokerEndpoint';  
```  
  
 Se si specifica MIRROR_ADDRESS, la route deve specificare la clausola SERVICE_NAME e la clausola BROKER_INSTANCE. Per una route con valore **'LOCAL'** o **'TRANSPORT'** per il parametro *next_hop_address* non è necessario specificare un indirizzo mirror.  
  
## <a name="remarks"></a>Osservazioni  
 La tabella di routing in cui sono archiviate le route è una tabella di metadati che può essere letta tramite la vista del catalogo **sys.routes**. È possibile aggiornare questa vista del catalogo solo tramite le istruzioni CREATE ROUTE, ALTER ROUTE e DROP ROUTE.  
  
 Per impostazione predefinita, la tabella di routing in ogni database utente contiene una sola route. Tale route è denominata **AutoCreatedLocal**. La route è impostata con il valore **'LOCAL'** per il parametro *next_hop_address* e corrisponde a qualsiasi nome di servizio e identificatore di istanza di Service Broker.  
  
 Se per il parametro **next_hop_address** di una route viene specificato il valore *'TRANSPORT'* , l'indirizzo di rete viene determinato in base all'indirizzo di rete nel nome del servizio. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] elabora correttamente i nomi di servizi che iniziano con un indirizzo di rete in un formato valido per *next_hop_address*.  
  
 La tabella di routing può includere qualsiasi numero di route che specificano lo stesso servizio, indirizzo di rete e identificatore dell'istanza di Service Broker. In questo caso, [!INCLUDE[ssSB](../../includes/sssb-md.md)] sceglie una route utilizzando una procedura progettata in modo da individuare la corrispondenza più esatta tra le informazioni specificate nella conversazione e le informazioni della tabella di routing.  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] non rimuove le route scadute dalla tabella di routing. È possibile riattivare una route scaduta tramite l'istruzione ALTER ROUTE.  
  
 Una route non può essere un oggetto temporaneo. Sono consentiti nomi di route che iniziano con **#** , ma si tratta di oggetti permanenti.  
  
## <a name="permissions"></a>Autorizzazioni  
 L'autorizzazione per la creazione di una route viene assegnata per impostazione predefinita ai membri del ruolo predefinito del database **db_ddladmin** o **db_owner** e ai membri del ruolo predefinito del server **sysadmin**.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-tcpip-route-by-using-a-dns-name"></a>R. Creazione di una route TCP/IP utilizzando un nome DNS  
 Nell'esempio seguente viene creata una route per il servizio `//Adventure-Works.com/Expenses`. La route specifica che i messaggi per questo servizio verranno trasmessi tramite TCP alla porta `1234` nell'host identificato dal nome DNS `www.Adventure-Works.com`. All'arrivo, i messaggi verranno recapitati dal server di destinazione all'istanza di Service Broker identificata dall'ID univoco `D8D4D268-00A3-4C62-8F91-634B89C1E315`.  
  
```sql 
CREATE ROUTE ExpenseRoute  
    WITH  
    SERVICE_NAME = '//Adventure-Works.com/Expenses',  
    BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89C1E315',  
    ADDRESS = 'TCP://www.Adventure-Works.com:1234' ;  
```  
  
### <a name="b-creating-a-tcpip-route-by-using-a-netbios-name"></a>B. Creazione di una route TCP/IP utilizzando un nome NetBIOS  
 Nell'esempio seguente viene creata una route per il servizio `//Adventure-Works.com/Expenses`. La route specifica che i messaggi per questo servizio verranno trasmessi tramite TCP alla porta `1234` nell'host identificato dal nome NetBIOS `SERVER02`. All'arrivo, il messaggio verrà recapitato dall'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di destinazione all'istanza di database identificata dall'ID univoco `D8D4D268-00A3-4C62-8F91-634B89C1E315`.  
  
```sql  
CREATE ROUTE ExpenseRoute  
    WITH   
    SERVICE_NAME = '//Adventure-Works.com/Expenses',  
    BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89C1E315',  
    ADDRESS = 'TCP://SERVER02:1234' ;  
```  
  
### <a name="c-creating-a-tcpip-route-by-using-an-ip-address"></a>C. Creazione di una route TCP/IP utilizzando un indirizzo IP  
 Nell'esempio seguente viene creata una route per il servizio `//Adventure-Works.com/Expenses`. La route specifica che i messaggi per questo servizio verranno trasmessi tramite TCP alla porta `1234` nell'host all'indirizzo IP `192.168.10.2`. All'arrivo, il messaggio verrà recapitato dall'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di destinazione all'istanza di Service Broker identificata dall'ID univoco `D8D4D268-00A3-4C62-8F91-634B89C1E315`.  
  
```sql  
CREATE ROUTE ExpenseRoute  
    WITH  
    SERVICE_NAME = '//Adventure-Works.com/Expenses',  
    BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89C1E315',  
    ADDRESS = 'TCP://192.168.10.2:1234' ;  
```  
  
### <a name="d-creating-a-route-to-a-forwarding-broker"></a>D. Creazione di una route per un'istanza di Service Broker di inoltro  
 Nell'esempio seguente viene creata una route per l'istanza di Service Broker di inoltro sul server `dispatch.Adventure-Works.com`. Poiché non vengono specificati il nome del servizio e l'identificatore dell'istanza di Service Broker, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizza questa route per i servizi per cui non è definita nessun'altra route.  
  
```sql  
CREATE ROUTE ExpenseRoute  
    WITH  
    ADDRESS = 'TCP://dispatch.Adventure-Works.com' ;   
```  
  
### <a name="e-creating-a-route-to-a-local-service"></a>E. Creazione di una route per un servizio locale  
 Nell'esempio seguente viene creata una route per il servizio `//Adventure-Works.com/LogRequests` nella stessa istanza della route.  
  
```sql  
CREATE ROUTE LogRequests  
    WITH  
    SERVICE_NAME = '//Adventure-Works.com/LogRequests',  
    ADDRESS = 'LOCAL' ;  
```  
  
### <a name="f-creating-a-route-with-a-specified-lifetime"></a>F. Creazione di una route con una durata specifica  
 Nell'esempio seguente viene creata una route per il servizio `//Adventure-Works.com/Expenses`. La durata della route è di `259200` secondi, equivalenti a 72 ore.  
  
```sql  
CREATE ROUTE ExpenseRoute  
    WITH  
    SERVICE_NAME = '//Adventure-Works.com/Expenses',  
    LIFETIME = 259200,  
    ADDRESS = 'TCP://services.Adventure-Works.com:1234' ;  
```  
  
### <a name="g-creating-a-route-to-a-mirrored-database"></a>G. Creazione di una route per un database con mirroring  
 Nell'esempio seguente viene creata una route per il servizio `//Adventure-Works.com/Expenses`. Il servizio è ospitato in un database con mirroring. Uno dei database con mirroring è disponibile all'indirizzo `services.Adventure-Works.com:1234`, mentre l'altro database si trova all'indirizzo `services-mirror.Adventure-Works.com:1234`.  
  
```sql  
CREATE ROUTE ExpenseRoute  
    WITH  
    SERVICE_NAME = '//Adventure-Works.com/Expenses',  
    BROKER_INSTANCE = '69fcc80c-2239-4700-8437-1001ecddf933',  
    ADDRESS = 'TCP://services.Adventure-Works.com:1234',   
    MIRROR_ADDRESS = 'TCP://services-mirror.Adventure-Works.com:1234' ;  
```  
  
### <a name="h-creating-a-route-that-uses-the-service-name-for-routing"></a>H. Creazione di una route che utilizza il nome del servizio per il routing  
 Nell'esempio seguente viene creata una route che utilizza il nome del servizio per determinare l'indirizzo di rete a cui inviare il messaggio. Si noti che una route con indirizzo di rete `'TRANSPORT'` ha una priorità di corrispondenza inferiore rispetto alle altre route.  
  
```sql  
CREATE ROUTE TransportRoute  
    WITH ADDRESS = 'TRANSPORT' ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER ROUTE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-route-transact-sql.md)   
 [DROP ROUTE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-route-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
