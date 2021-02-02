---
title: Reindirizzare le connessioni in lettura/scrittura alla replica primaria
description: Informazioni su come reindirizzare le connessioni in lettura/scrittura alla replica primaria di un gruppo di disponibilità Always On, indipendentemente dal server specificato nella stringa di connessione.
ms.custom: seo-lt-2019
ms.date: 01/09/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: article
helpviewer_keywords:
- connection access to availability replicas
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], readable secondary replicas
- active secondary replicas [SQL Server], read-only access to
- readable secondary replicas
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: ac4900e284d1e83ef0945e1d0082279c06414b0b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236675"
---
# <a name="secondary-to-primary-replica-readwrite-connection-redirection-always-on-availability-groups"></a>Reindirizzamento della connessione in lettura/scrittura dalla replica secondaria alla primaria (Gruppi di disponibilità AlwaysOn)

[!INCLUDE[appliesto](../../../includes/applies-to-version/sqlserver2019.md)]

[!INCLUDE[sssql19-md](../../../includes/sssql19-md.md)] CTP 2.0 introduce il *reindirizzamento della connessione in lettura/scrittura dalla replica secondaria alla primaria* per i gruppi di disponibilità AlwaysOn. Il reindirizzamento della connessione di lettura/scrittura è disponibile in qualsiasi piattaforma del sistema operativo. Consente di orientare le connessioni dell'applicazione client alla replica primaria, indipendentemente dal server di destinazione specificato nella stringa di connessione. 

Ad esempio la stringa di connessione può avere come destinazione una replica secondaria. A seconda della configurazione della replica del gruppo di disponibilità (AG) e delle impostazioni nella stringa di connessione, è possibile reindirizzare automaticamente la connessione alla replica primaria. 

## <a name="use-cases"></a>Casi d'uso

Nelle versioni precedenti a [!INCLUDE[sssql19-md](../../../includes/sssql19-md.md)] il listener del gruppo di disponibilità e la risorsa cluster corrispondente reindirizzano il traffico utente alla replica primaria per garantire la riconnessione dopo il failover. [!INCLUDE[sssql19-md](../../../includes/sssql19-md.md)] continua a supportare la funzionalità listener del gruppo di disponibilità e aggiunge il reindirizzamento della connessione di replica per gli scenari che non possono includere un listener. Ad esempio:

* La tecnologia cluster con cui si integrano i gruppi di disponibilità SQL Server non offre una funzionalità di tipo listener 
* Una configurazione con più subnet come nel cloud o un IP variabile per più subnet, dove le configurazioni diventano complesse, soggette a errori e con una risoluzione dei problemi impegnativa a causa del numero di componenti attivi
* La scalabilità in lettura o il ripristino di emergenza e il tipo di cluster è `NONE`, perché non è presente un meccanismo diretto che garantisce la riconnessione trasparente in caso di failover manuale

## <a name="requirement"></a>Requisito

Affinché una replica secondaria reindirizzi le richieste di connessione in lettura/scrittura:
* La replica secondaria deve essere online. 
* La specifica della replica `PRIMARY_ROLE` deve includere `READ_WRITE_ROUTING_URL`.
* La stringa di connessione deve essere `ReadWrite` definendo `ApplicationIntent` come `ReadWrite` o non impostando `ApplicationIntent` e consentendo l'uso effettivo dell'impostazione predefinita (`ReadWrite`).

## <a name="set-read_write_routing_url-option"></a>Impostare l'opzione READ_WRITE_ROUTING_URL

Per configurare il reindirizzamento della connessione di lettura/scrittura, impostare `READ_WRITE_ROUTING_URL` per la replica primaria quando si crea il gruppo di disponibilità. 

In [!INCLUDE[sssql19-md](../../../includes/sssql19-md.md)] l'elemento `READ_WRITE_ROUTING_URL` è stato aggiunto alla specifica `<add_replica_option>`. Vedere gli argomenti seguenti: 

* [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)
* [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)


### <a name="primary_roleread_write_routing_url-not-set-default"></a>PRIMARY_ROLE(READ_WRITE_ROUTING_URL) non impostato (impostazione predefinita) 

Per impostazione predefinita il reindirizzamento della connessione di replica di lettura/scrittura non è impostato per una replica. Il modo in cui una replica secondaria gestisce le richieste di connessione dipende dal fatto che la replica secondaria sia impostata o meno per consentire le connessioni e dall'impostazione `ApplicationIntent` nella stringa di connessione. La tabella seguente illustra come una replica secondaria gestisce le connessioni in base a `SECONDARY_ROLE (ALLOW CONNECTIONS = )` e `ApplicationIntent`.

|Valore della proprietà <code>ApplicationIntent</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = NO)</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = READ_ONLY)</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = ALL)</code>|
|-----|-----|-----|-----|
|`ApplicationIntent=ReadWrite`<br/> Predefinito|Le connessioni hanno esito negativo|Le connessioni hanno esito negativo|Le connessioni hanno esito positivo<br/>Le operazioni di lettura hanno esito positivo<br/>Le operazioni di scrittura hanno esito negativo|
|`ApplicationIntent=ReadOnly`|Le connessioni hanno esito negativo|Le connessioni hanno esito positivo|Le connessioni hanno esito positivo

La tabella precedente illustra il comportamento predefinito, analogo a quello delle versioni di SQL Server precedenti a [!INCLUDE[sssql19-md](../../../includes/sssql19-md.md)]. 

### <a name="primary_roleread_write_routing_url-set"></a>PRIMARY_ROLE(READ_WRITE_ROUTING_URL) impostato 

Dopo l'impostazione del reindirizzamento della connessione di lettura/scrittura, la modalità con cui la replica gestisce le richieste di connessione cambia. Il comportamento delle connessioni dipende comunque dalle impostazioni `SECONDARY_ROLE (ALLOW CONNECTIONS = )` e `ApplicationIntent`. La tabella seguente illustra come una replica secondaria con `READ_WRITE_ROUTING` impostato gestisce le connessioni in base a `SECONDARY_ROLE (ALLOW CONNECTIONS = )` e `ApplicationIntent`.

|Valore della proprietà <code>ApplicationIntent</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = NO)</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = READ_ONLY)</code>|<code>SECONDARY_ROLE (ALLOW CONNECTIONS = ALL)</code>|
|-----|-----|-----|-----|
|`ApplicationIntent=ReadWrite`<br/>Predefinito|Le connessioni hanno esito negativo|Le connessioni hanno esito negativo|Le connessioni vengono instradate alla replica primaria|
|`ApplicationIntent=ReadOnly`|Le connessioni hanno esito negativo|Le connessioni hanno esito positivo|Le connessioni hanno esito positivo

La tabella precedente evidenzia che quando per la replica primaria è impostato `READ_WRITE_ROUTING_URL`, la replica secondaria reindirizza le connessioni alla replica primaria se `SECONDARY_ROLE (ALLOW CONNECTIONS = ALL)` e la connessione specifica `ReadWrite`.

## <a name="example"></a>Esempio 

In questo esempio un gruppo di disponibilità ha tre repliche:
* Una replica primaria in COMPUTER01
* Una replica secondaria sincrona in COMPUTER02
* Una replica secondaria asincrona in COMPUTER03

L'immagine seguente rappresenta il gruppo di disponibilità.

![Gruppo di disponibilità con database primario, secondario e secondario asincrono](media/replica-connection-redirection-always-on-availability-groups/01_originalAG.png)

Lo script Transact-SQL seguente crea questo gruppo di disponibilità. In questo esempio, ogni replica specifica l'elemento `READ_WRITE_ROUTING_URL`.
```sql
CREATE AVAILABILITY GROUP MyAg   
     WITH ( CLUSTER_TYPE =  NONE )  
   FOR   
     DATABASE  <Database1>   
   REPLICA ON   
      'COMPUTER01' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER01.<domain>.<tld>:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = MANUAL,  
         SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL,   
            READ_ONLY_ROUTING_URL = 'TCP://COMPUTER01.<domain>.<tld>:1433' ),
         PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE,   
            READ_ONLY_ROUTING_LIST = (COMPUTER02, COMPUTER03),
            READ_WRITE_ROUTING_URL = 'TCP://COMPUTER01.<domain>.<tld>:1433' )   
         SESSION_TIMEOUT = 10  
         ),   
      'COMPUTER02' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER02.<domain>.<tld>:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = MANUAL, 
         SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL,   
            READ_ONLY_ROUTING_URL = 'TCP://COMPUTER02.<domain>.<tld>:1433' ),  
         PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE,   
            READ_ONLY_ROUTING_LIST = (COMPUTER01, COMPUTER03),  
            READ_WRITE_ROUTING_URL = 'TCP://COMPUTER02.<domain>.<tld>:1433' )   
         SESSION_TIMEOUT = 10  
         ),   
      'COMPUTER03' WITH   
         (  
         ENDPOINT_URL = 'TCP://COMPUTER03.<domain>.<tld>:5022',  
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = MANUAL,  
         SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL,   
            READ_ONLY_ROUTING_URL = 'TCP://COMPUTER03.<domain>.<tld>:1433' ),  
         PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE,   
            READ_ONLY_ROUTING_LIST = (COMPUTER01, COMPUTER02),  
            READ_WRITE_ROUTING_URL = 'TCP://COMPUTER03.<domain>.<tld>:1433' )  
         SESSION_TIMEOUT = 10  
         );
GO  
```
   - `<domain>.<tld>`
      - Dominio e dominio di primo livello del nome di dominio completo. Ad esempio: `corporation.com`.


### <a name="connection-behaviors"></a>Comportamenti di connessione

Nel diagramma seguente un'applicazione client si connette a COMPUTER02 con `ApplicationIntent=ReadWrite`. La connessione viene reindirizzata alla replica primaria. 

![La connessione al computer 2 viene reindirizzata alla replica primaria](media/replica-connection-redirection-always-on-availability-groups/02_redirectionAG.png)

La replica secondaria reindirizza le chiamate di lettura/scrittura alla replica primaria. Una connessione di lettura/scrittura a una delle repliche determina il reindirizzamento alla replica primaria. 

Nel diagramma seguente la replica primaria è stata sottoposta manualmente a failover su COMPUTER02. Un'applicazione client si connette a COMPUTER01 con `ApplicationIntent=ReadWrite`. La connessione viene reindirizzata alla replica primaria. 

![Connessione reindirizzata alla nuova replica primaria nel computer 2](media/replica-connection-redirection-always-on-availability-groups/03_redirectionAG.png)

## <a name="see-also"></a>Vedere anche

[Panoramica di Gruppi di disponibilità AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 
[Informazioni sull'accesso alla connessione client per le repliche di disponibilità &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)   

[Listener del gruppo di disponibilità, connettività client e failover dell'applicazione &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)
