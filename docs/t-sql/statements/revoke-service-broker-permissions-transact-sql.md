---
description: REVOKE (autorizzazioni di Service Broker) (Transact-SQL)
title: REVOKE - autorizzazioni per Service Broker (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- routes [Service Broker], permissions
- Service Broker, permissions
- remote service bindings [Service Broker], permissions
- permissions [Service Broker]
- message types [Service Broker], permissions
- contracts [Service Broker], permissions
- services [Service Broker], permissions
- REVOKE statement, Service Broker
ms.assetid: 70f1d938-97e2-48a4-9bc0-8be9f2f2c36d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d28fe5ba46671e38089727735701107a9427ac4e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194966"
---
# <a name="revoke-service-broker-permissions-transact-sql"></a>REVOKE (autorizzazioni di Service Broker) (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Revoca le autorizzazioni per un contratto, un tipo di messaggio, un'associazione al servizio remoto, una route o un servizio di [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
REVOKE [ GRANT OPTION FOR ] permission [ ,...n ] ON  
    {   
       [ CONTRACT :: contract_name ]   
       | [ MESSAGE TYPE :: message_type_name ]    
       | [ REMOTE SERVICE BINDING :: remote_binding_name ]    
       | [ ROUTE :: route_name ]   
       | [ SERVICE :: service_name ]      
        }  
    { TO | FROM } database_principal [ ,...n ]  
    [ CASCADE ]  
    [ AS revoking_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 GRANT OPTION FOR  
 Indica che il diritto a concedere l'autorizzazione specificata ad altre entità verrà rimosso. L'autorizzazione stessa non verrà revocata.  
  
> [!IMPORTANT]  
>  Se l'autorizzazione specificata è stata concessa all'entità senza l'opzione GRANT, l'autorizzazione stessa verrà revocata.  
  
 *permission*  
 Specifica un'autorizzazione che può essere revocata per un'entità a sicurezza diretta di [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Per un elenco di queste autorizzazioni, vedere la sezione Osservazioni di seguito in questo argomento.  
  
 CONTRACT **::** _contract_name_  
 Specifica il contratto a cui viene revocata l'autorizzazione. Il qualificatore di ambito **::** è obbligatorio.  
  
 MESSAGE TYPE **::**_message_type_name_  
 Specifica il tipo di messaggio a cui viene revocata l'autorizzazione. Il qualificatore di ambito **::** è obbligatorio.  
  
 REMOTE SERVICE BINDING **::**_remote_binding_name_  
 Specifica l'associazione al servizio remoto a cui viene revocata l'autorizzazione. Il qualificatore di ambito **::** è obbligatorio.  
  
 ROUTE **::**_route_name_  
 Specifica la route a cui viene revocata l'autorizzazione. Il qualificatore di ambito **::** è obbligatorio.  
  
 SERVICE **::** _message_type_name_  
 Specifica il servizio a cui viene revocata l'autorizzazione. Il qualificatore di ambito **::** è obbligatorio.  
  
 *database_principal*  
 Specifica l'entità da cui viene revocata l'autorizzazione. *database_principal* può essere una delle entità seguenti:  
  
-   Utente del database  
  
-   Ruolo del database  
  
-   Ruolo applicazione  
  
-   Utente del database di cui è stato eseguito il mapping a un account di accesso di Windows  
  
-   Utente del database di cui è stato eseguito il mapping a un gruppo di Windows  
  
-   Utente del database di cui è stato eseguito il mapping a un certificato  
  
-   Utente del database di cui è stato eseguito il mapping a una chiave asimmetrica  
  
-   Utente del database sul quale non viene eseguito il mapping ad alcuna entità server  
  
 CASCADE  
 Indica che l'autorizzazione che viene revocata anche ad altre entità a cui è stata concessa o negata da questa entità.  
  
> [!CAUTION]  
>  La revoca propagata di un'autorizzazione concessa con WITH GRANT OPTION comporterà la revoca sia delle autorizzazioni GRANT che delle autorizzazioni DENY per tale autorizzazione.  
  
 AS *revoking_principal*  
 Specifica un'entità dalla quale l'entità che esegue la query ottiene il diritto di revocare l'autorizzazione. *revoking_principal* può essere una delle entità seguenti:  
  
-   Utente del database  
  
-   Ruolo del database  
  
-   Ruolo applicazione  
  
-   Utente del database di cui è stato eseguito il mapping a un account di accesso di Windows  
  
-   Utente del database di cui è stato eseguito il mapping a un gruppo di Windows  
  
-   Utente del database di cui è stato eseguito il mapping a un certificato  
  
-   Utente del database di cui è stato eseguito il mapping a una chiave asimmetrica  
  
-   Utente del database sul quale non viene eseguito il mapping ad alcuna entità server  
  
## <a name="remarks"></a>Commenti  
  
## <a name="service-broker-contracts"></a>Contratti di Service Broker  
 Un contratto di [!INCLUDE[ssSB](../../includes/sssb-md.md)] è un'entità a sicurezza diretta a livello di database contenuta nel database padre nella gerarchia di autorizzazioni. Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che possono essere revocate per un contratto di [!INCLUDE[ssSB](../../includes/sssb-md.md)], con le autorizzazioni più generali in cui sono incluse in modo implicito.  
  
|Autorizzazione del contratto di Service Broker|Autorizzazione del contratto di Service Broker in cui è inclusa|Autorizzazione del database in cui è inclusa|  
|----------------------------------------|---------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY CONTRACT|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-message-types"></a>Tipi di messaggio di Service Broker  
 Un tipo di messaggio di [!INCLUDE[ssSB](../../includes/sssb-md.md)] è un'entità a sicurezza diretta a livello di database contenuta nel database padre nella gerarchia di autorizzazioni. Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che possono essere revocate per un tipo di messaggio di [!INCLUDE[ssSB](../../includes/sssb-md.md)], con le autorizzazioni più generali in cui sono incluse in modo implicito.  
  
|Autorizzazione del tipo di messaggio di Service Broker|Autorizzazione del tipo di messaggio di Service Broker in cui è inclusa|Autorizzazione del database in cui è inclusa|  
|--------------------------------------------|-------------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY MESSAGE TYPE|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-remote-service-bindings"></a>Associazioni ai servizi remoti di Service Broker  
 Un'associazione al servizio remoto di [!INCLUDE[ssSB](../../includes/sssb-md.md)] è un'entità a sicurezza diretta a livello di database contenuta nel database padre nella gerarchia di autorizzazioni. Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che possono essere revocate per un'associazione al servizio remoto di [!INCLUDE[ssSB](../../includes/sssb-md.md)], con le autorizzazioni più generali in cui sono incluse in modo implicito.  
  
|Autorizzazione dell'associazione al servizio remoto di Service Broker|Autorizzazione dell'associazione al servizio remoto di Service Broker in cui è inclusa|Autorizzazione del database in cui è inclusa|  
|------------------------------------------------------|-----------------------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY REMOTE SERVICE BINDING|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-routes"></a>Route di Service Broker  
 Una route di [!INCLUDE[ssSB](../../includes/sssb-md.md)] è un'entità a sicurezza diretta a livello di database contenuta nel database padre nella gerarchia di autorizzazioni. Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che possono essere revocate per una route di [!INCLUDE[ssSB](../../includes/sssb-md.md)], con le autorizzazioni più generali in cui sono incluse in modo implicito.  
  
|Autorizzazione della route di Service Broker|Autorizzazione della route di Service Broker in cui è inclusa|Autorizzazione del database in cui è inclusa|  
|-------------------------------------|------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ROUTE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
### <a name="service-broker-services"></a>Servizi di Service Broker  
 Un servizio di [!INCLUDE[ssSB](../../includes/sssb-md.md)] è un'entità a sicurezza diretta a livello di database contenuta nel database padre nella gerarchia di autorizzazioni. Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che possono essere revocate per un servizio di [!INCLUDE[ssSB](../../includes/sssb-md.md)], con le autorizzazioni più generali in cui sono incluse in modo implicito.  
  
|Autorizzazione del servizio di Service Broker|Autorizzazione del servizio di Service Broker in cui è inclusa|Autorizzazione del database in cui è inclusa|  
|---------------------------------------|--------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|SEND|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY SERVICE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessaria l'autorizzazione CONTROL per il contratto, il tipo di messaggio, l'associazione al servizio remoto, la route o il servizio di [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
## <a name="see-also"></a>Vedere anche  
 [GRANT - autorizzazioni per Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)   
 [DENY - autorizzazioni per Service Broker &#40;Transact-SQL&#41;](../../t-sql/statements/deny-service-broker-permissions-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [Autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
