---
description: ALTER ROUTE (Transact-SQL)
title: ALTER ROUTE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_ROUTE_TSQL
- ALTER ROUTE
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER ROUTE statement
- dropping routes
- modifying routes
- removing routes
- routes [Service Broker], modifying
ms.assetid: 8dfb7b16-3dac-4e1e-8c97-adf2aad07830
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: d1fc273bdb22d58974ec5fdc562cf3502348b196
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204038"
---
# <a name="alter-route-transact-sql"></a>ALTER ROUTE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifica le informazioni relative a una route esistente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
ALTER ROUTE route_name  
WITH    
  [ SERVICE_NAME = 'service_name' [ , ] ]  
  [ BROKER_INSTANCE = 'broker_instance' [ , ] ]  
  [ LIFETIME = route_lifetime [ , ] ]  
  [ ADDRESS =  'next_hop_address' [ , ] ]  
  [ MIRROR_ADDRESS = 'next_hop_mirror_address' ]  
[ ; ]  
  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *route_name*  
 Nome della route da modificare. Non è possibile specificare i nomi del server, del database e dello schema.  
  
 WITH  
 Introduce le clausole che definiscono la route da modificare.  
  
 SERVICE_NAME **='** _service\_name_ **'**  
 Specifica il nome del servizio remoto a cui la route fa riferimento. *service_name* deve corrispondere esattamente al nome usato dal servizio remoto. Per creare corrispondenza con [!INCLUDE[ssSB](../../includes/sssb-md.md)]service_name *,*  usa un confronto byte per byte. In altre parole, nel confronto viene fatta distinzione tra maiuscole e minuscole e non vengono considerate le regole di confronto correnti. Una route con nome di servizio **'SQL/ServiceBroker/BrokerConfiguration'** è una route per un servizio di configurazione di Service Broker. Per una route per questo servizio non è necessario specificare un'istanza di Service Broker.  
  
 Se la clausola SERVICE_NAME viene omessa, il nome del servizio per la route rimane invariato.  
  
 BROKER_INSTANCE **='** _broker\_instance_ **'**  
 Specifica il database che ospita il servizio di destinazione. Il parametro *broker_instance* deve corrispondere all'identificatore dell'istanza di Service Broker per il database remoto. Per ottenere tale identificatore, è possibile eseguire la query seguente nel database selezionato:  
  
```sql  
SELECT service_broker_guid  
FROM sys.databases  
WHERE database_id = DB_ID();  
```  
  
 Se la clausola BROKER_NAME viene omessa, l'istanza di Service Broker per la route rimane invariata.  
  
> [!NOTE]  
>  Questa opzione non è disponibile in un database indipendente.  
  
 LIFETIME **=** _route\_lifetime_  
 Specifica per quanti secondi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mantiene la route nella tabella di routing. Al termine di questo periodo di tempo, la route scade e non viene più presa in considerazione da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per la scelta della route per una nuova conversazione. Se questa clausola viene omessa, la durata della route rimane invariata.  
  
 ADDRESS **='** _next\_hop\_address_'  

 Per Istanza gestita di SQL di Azure, `ADDRESS` deve essere locale.

 Specifica l'indirizzo di rete per la route. Il parametro *next_hop_address* specifica un indirizzo TCP/IP nel formato seguente:  
  
 **TCP://** { *dns_name* | *netbios_name* |*ip_address* } **:** *port_number*  
  
 Il parametro *port_number* specificato deve corrispondere al numero di porta dell'endpoint di [!INCLUDE[ssSB](../../includes/sssb-md.md)] per un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nel computer specificato. Per ottenere tale valore, eseguire la query seguente nel database selezionato:  
  
```sql  
SELECT tcpe.port  
FROM sys.tcp_endpoints AS tcpe  
INNER JOIN sys.service_broker_endpoints AS ssbe  
   ON ssbe.endpoint_id = tcpe.endpoint_id  
WHERE ssbe.name = N'MyServiceBrokerEndpoint';  
```  
  
 Se per il parametro **next_hop_address** di una route viene specificato il valore *'LOCAL'* , il messaggio viene recapitato a un servizio nell'istanza corrente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Se per il parametro **next_hop_address** di una route viene specificato il valore *'TRANSPORT'* , l'indirizzo di rete viene determinato in base all'indirizzo di rete nel nome del servizio. Per una route con valore **'TRANSPORT'** è possibile specificare un nome di servizio o un'istanza di Service Broker.  
  
 Se il parametro *next_hop_address* corrisponde al server principale per un database mirror, è necessario specificare anche MIRROR_ADDRESS per il server mirror. In caso contrario non può venire eseguito il failover automatico della route al server mirror.  
  
> [!NOTE]  
>  Questa opzione non è disponibile in un database indipendente.  
  
 MIRROR_ADDRESS **='** _next\_hop\_mirror\_address_ **'**  
 Specifica l'indirizzo di rete per il server mirror di una coppia con mirroring in cui il server principale si trova all'indirizzo *next_hop_address*. Il parametro *next_hop_mirror_address* specifica un indirizzo TCP/IP nel formato seguente:  
  
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
  
> [!NOTE]  
>  Questa opzione non è disponibile in un database indipendente.  
  
## <a name="remarks"></a>Osservazioni  
 La tabella di routing in cui sono archiviate le route è una tabella di metadati che può essere letta tramite la vista del catalogo **sys.routes**. e aggiornata esclusivamente utilizzando le istruzioni CREATE ROUTE, ALTER ROUTE e DROP ROUTE.  
  
 Le clausole che non sono specificate nel comando ALTER ROUTE rimangono invariate. Non è possibile pertanto eseguire il comando ALTER per specificare che la route non scade, che corrisponde a un nome di servizio o che corrisponde a un'istanza di Service Broker. Per modificare queste caratteristiche di una route è necessario eliminare la route esistente e creare una nuova route con le nuove informazioni.  
  
 Se per il parametro **next_hop_address** di una route viene specificato il valore *'TRANSPORT'* , l'indirizzo di rete viene determinato in base all'indirizzo di rete nel nome del servizio. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] elabora correttamente i nomi di servizi che iniziano con un indirizzo di rete in un formato valido per *next_hop_address*. I servizi con nomi che contengono indirizzi di rete validi verranno indirizzati all'indirizzo di rete indicato nel nome del servizio.  
  
 La tabella di routing può includere qualsiasi numero di route che specificano lo stesso servizio, indirizzo di rete e/o identificatore dell'istanza di Service Broker. In questo caso, [!INCLUDE[ssSB](../../includes/sssb-md.md)] sceglie una route utilizzando una procedura progettata in modo da individuare la corrispondenza più esatta tra le informazioni specificate nella conversazione e le informazioni della tabella di routing.  
  
 Per modificare il parametro AUTHORIZATION per un servizio, utilizzare l'istruzione ALTER AUTHORIZATION.  
  
## <a name="permissions"></a>Autorizzazioni  
 L'autorizzazione per modificare una route viene assegnata per impostazione predefinita al proprietario della route, ai membri dei ruoli predefiniti del database **db_ddladmin** o **db_owner** e ai membri del ruolo predefinito del server **sysadmin**.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-changing-the-service-for-a-route"></a>R. Modifica del servizio per una route  
 Nell'esempio seguente viene modificata la route `ExpenseRoute` in modo da puntare al servizio remoto `//Adventure-Works.com/Expenses`.  
  
```sql  
ALTER ROUTE ExpenseRoute  
   WITH   
     SERVICE_NAME = '//Adventure-Works.com/Expenses';  
```  
  
### <a name="b-changing-the-target-database-for-a-route"></a>B. Modifica del database di destinazione per una route  
 Nell'esempio seguente il database di destinazione per la route `ExpenseRoute` viene modificato e impostato sul database identificato dall'ID univoco `D8D4D268-00A3-4C62-8F91-634B89B1E317.`  
  
```sql  
ALTER ROUTE ExpenseRoute  
   WITH   
     BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89B1E317';  
```  
  
### <a name="c-changing-the-address-for-a-route"></a>C. Modifica dell'indirizzo per una route  
 Nell'esempio seguente l'indirizzo di rete per la route `ExpenseRoute` viene modificato e impostato sulla porta TCP `1234` nell'host con indirizzo IP `10.2.19.72`.  
  
```sql  
ALTER ROUTE ExpenseRoute   
   WITH   
     ADDRESS = 'TCP://10.2.19.72:1234';  
```  
  
### <a name="d-changing-the-database-and-address-for-a-route"></a>D. Modifica del database e dell'indirizzo per una route  
 Nell'esempio seguente l'indirizzo di rete per la route `ExpenseRoute` viene modificato e impostato sulla porta TCP `1234` nell'host con nome DNS `www.Adventure-Works.com`. Viene inoltre modificato il database di destinazione, che viene impostato sul database identificato dall'identificatore univoco `D8D4D268-00A3-4C62-8F91-634B89B1E317`.  
  
```sql  
ALTER ROUTE ExpenseRoute  
   WITH   
     BROKER_INSTANCE = 'D8D4D268-00A3-4C62-8F91-634B89B1E317',  
     ADDRESS = 'TCP://www.Adventure-Works.com:1234';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE ROUTE &#40;Transact-SQL&#41;](../../t-sql/statements/create-route-transact-sql.md)   
 [DROP ROUTE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-route-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
