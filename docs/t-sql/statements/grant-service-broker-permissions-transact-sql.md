---
description: GRANT - autorizzazioni per Service Broker (Transact-SQL)
title: GRANT - autorizzazioni per Service Broker (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server], Service Broker
- routes [Service Broker], permissions
- Service Broker, permissions
- GRANT statement, Service Broker
- remote service bindings [Service Broker], permissions
- message types [Service Broker], permissions
- contracts [Service Broker], permissions
ms.assetid: c5579976-97c4-4123-be0c-d0b98a9e38fb
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: a6518ccca07c6b8014d830381d4c045b67625c8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209554"
---
# <a name="grant-service-broker-permissions-transact-sql"></a>GRANT - autorizzazioni per Service Broker (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Concede le autorizzazioni per un contratto, un tipo di messaggio, un'associazione remota, una route o un servizio di Service Broker.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
GRANT permission  [ ,...n ] ON  
    {    
              [ CONTRACT :: contract_name ]   
       | [ MESSAGE TYPE :: message_type_name ]    
       | [ REMOTE SERVICE BINDING :: remote_binding_name ]    
       | [ ROUTE :: route_name ]   
       | [ SERVICE :: service_name ]      
    }  
    TO database_principal [ ,...n ]   
    [ WITH GRANT OPTION ]  
        [ AS granting_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *permission*  
 Specifica un'autorizzazione che può essere concessa per un'entità a protezione diretta di Service Broker.  Vedere l'elenco riportato di seguito.  
  
 CONTRACT **::** _contract_name_  
 Specifica il contratto per cui viene concessa l'autorizzazione. Il qualificatore di ambito "::" è obbligatorio.  
  
 MESSAGE TYPE **::**_message_type_name_  
 Specifica il tipo di messaggio per cui viene concessa l'autorizzazione. Il qualificatore di ambito "::" è obbligatorio.  
  
 REMOTE SERVICE BINDING **::**_remote_binding_name_  
 Specifica l'associazione al servizio remoto per cui viene concessa l'autorizzazione. Il qualificatore di ambito "::" è obbligatorio.  
  
 ROUTE **::**_route_name_  
 Specifica la route per cui viene concessa l'autorizzazione. Il qualificatore di ambito "::" è obbligatorio.  
  
 SERVICE **::**_service_name_  
 Specifica il servizio per cui viene concessa l'autorizzazione. Il qualificatore di ambito "::" è obbligatorio.  
  
 *database_principal*  
 Specifica l'entità a cui viene concessa l'autorizzazione. I tipi validi sono:  
  
-   utente del database  
  
-   ruolo del database  
  
-   ruolo applicazione  
  
-   utente del database sul quale viene eseguito il mapping a un account di accesso di Windows  
  
-   utente del database di cui è stato eseguito il mapping a un gruppo di Windows  
  
-   utente del database di cui è stato eseguito il mapping a un certificato  
  
-   utente del database di cui è stato eseguito il mapping a una chiave asimmetrica  
  
-   utente del database non mappato ad alcuna entità server.  
  
 GRANT OPTION  
 Indica che l'entità potrà inoltre concedere l'autorizzazione specificata ad altre entità.  
  
 *granting_principal*  
 Specifica un'entità dalla quale l'entità che esegue la query ottiene il diritto di concedere l'autorizzazione. I tipi validi sono:  
  
-   utente del database  
  
-   ruolo del database  
  
-   ruolo applicazione  
  
-   utente del database sul quale viene eseguito il mapping a un account di accesso di Windows  
  
-   utente del database di cui è stato eseguito il mapping a un gruppo di Windows  
  
-   utente del database di cui è stato eseguito il mapping a un certificato  
  
-   utente del database di cui è stato eseguito il mapping a una chiave asimmetrica  
  
-   utente del database non mappato ad alcuna entità server.  
  
## <a name="remarks"></a>Commenti  
  
## <a name="service-broker-contracts"></a>Contratti di Service Broker  
 Un contratto di Service Broker è un'entità a protezione diretta a livello di database contenuta nel database padre nella gerarchia delle autorizzazioni. Di seguito sono elencate le autorizzazioni più specifiche e limitate che è possibile concedere in un contratto di Service Broker, insieme alle autorizzazioni più generali che le includono in modo implicito.  
  
|Autorizzazione del contratto di Service Broker|Autorizzazione del contratto di Service Broker in cui è inclusa|Autorizzazione del database in cui è inclusa|  
|----------------------------------------|---------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY CONTRACT|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-message-types"></a>Tipi di messaggio di Service Broker  
 Un tipo di messaggio di Service Broker è un'entità a protezione diretta a livello di database contenuta nel database padre nella gerarchia delle autorizzazioni. Di seguito sono elencate le autorizzazioni più specifiche e limitate che è possibile concedere per un tipo di messaggio di Service Broker, insieme alle autorizzazioni più generali che le includono in modo implicito.  
  
|Autorizzazione del tipo di messaggio di Service Broker|Autorizzazione del tipo di messaggio di Service Broker in cui è inclusa|Autorizzazione del database in cui è inclusa|  
|--------------------------------------------|-------------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY MESSAGE TYPE|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-remote-service-bindings"></a>Associazioni ai servizi remoti di Service Broker  
 Un'associazione al servizio remoto di Service Broker è un'entità a protezione diretta a livello di database contenuta nel database padre nella gerarchia delle autorizzazioni. Di seguito sono elencate le autorizzazioni più specifiche e limitate che è possibile concedere per un'associazione al servizio remoto di Service Broker, insieme alle autorizzazioni più generali che le includono in modo implicito.  
  
|Autorizzazione dell'associazione al servizio remoto di Service Broker|Autorizzazione dell'associazione al servizio remoto di Service Broker in cui è inclusa|Autorizzazione del database in cui è inclusa|  
|------------------------------------------------------|-----------------------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY REMOTE SERVICE BINDING|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="service-broker-routes"></a>Route di Service Broker  
 Una route di Service Broker è un'entità a protezione diretta a livello di database contenuta nel database padre nella gerarchia delle autorizzazioni. Di seguito sono elencate le autorizzazioni più specifiche e limitate che è possibile concedere per una route di Service Broker, insieme alle autorizzazioni più generali che le includono in modo implicito.  
  
|Autorizzazione della route di Service Broker|Autorizzazione della route di Service Broker in cui è inclusa|Autorizzazione del database in cui è inclusa|  
|-------------------------------------|------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ROUTE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
### <a name="service-broker-services"></a>Servizi di Service Broker  
 Un servizio di Service Broker è un'entità a protezione diretta a livello di database contenuta nel database padre nella gerarchia delle autorizzazioni. Di seguito sono elencate le autorizzazioni più specifiche e limitate che è possibile concedere per un servizio di Service Broker, insieme alle autorizzazioni più generali che le includono in modo implicito.  
  
|Autorizzazione del servizio di Service Broker|Autorizzazione del servizio di Service Broker in cui è inclusa|Autorizzazione del database in cui è inclusa|  
|---------------------------------------|--------------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|SEND|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY SERVICE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente che concede le autorizzazioni (o l'entità specificata con l'opzione AS) deve disporre della relativa autorizzazione con GRANT OPTION oppure di un'autorizzazione di livello superiore che include l'autorizzazione che viene concessa.  
  
 Se si usano l'opzione AS, sono previsti questi requisiti aggiuntivi.  
  
|AS *granting_principal*|Autorizzazione aggiuntiva necessaria|  
|------------------------------|------------------------------------|  
|Utente del database|Autorizzazione IMPERSONATE per l'utente, appartenenza al ruolo predefinito del database **db_securityadmin**, appartenenza al ruolo predefinito del database **db_owner** o appartenenza al ruolo predefinito del server **sysadmin**.|  
|Utente del database di cui è stato eseguito il mapping a un account di accesso di Windows|Autorizzazione IMPERSONATE per l'utente, appartenenza al ruolo predefinito del database **db_securityadmin**, appartenenza al ruolo predefinito del database **db_owner** o appartenenza al ruolo predefinito del server **sysadmin**.|  
|Utente del database di cui è stato eseguito il mapping a un gruppo di Windows|Appartenenza al gruppo di Windows, appartenenza al ruolo predefinito del database **db_securityadmin**, appartenenza al ruolo predefinito del database **db_owner** o appartenenza al ruolo predefinito del server **sysadmin**.|  
|Utente del database di cui è stato eseguito il mapping a un certificato|Appartenenza al ruolo predefinito del database **db_securityadmin**, appartenenza al ruolo predefinito del database **db_owner** o appartenenza al ruolo predefinito del server **sysadmin**.|  
|Utente del database di cui è stato eseguito il mapping a una chiave asimmetrica|Appartenenza al ruolo predefinito del database **db_securityadmin**, appartenenza al ruolo predefinito del database **db_owner** o appartenenza al ruolo predefinito del server **sysadmin**.|  
|Utente del database di cui non è stato eseguito il mapping ad alcuna entità server|Autorizzazione IMPERSONATE per l'utente, appartenenza al ruolo predefinito del database **db_securityadmin**, appartenenza al ruolo predefinito del database **db_owner** o appartenenza al ruolo predefinito del server **sysadmin**.|  
|Ruolo del database|Autorizzazione ALTER per il ruolo, appartenenza al ruolo predefinito del database **db_securityadmin**, appartenenza al ruolo predefinito del database **db_owner** o appartenenza al ruolo predefinito del server **sysadmin**.|  
|Ruolo applicazione|Autorizzazione ALTER per il ruolo, appartenenza al ruolo predefinito del database **db_securityadmin**, appartenenza al ruolo predefinito del database **db_owner** o appartenenza al ruolo predefinito del server **sysadmin**.|  
  
 I proprietari degli oggetti possono concedere autorizzazioni per gli oggetti di cui sono proprietari. Le entità con l'autorizzazione CONTROL per un'entità a sicurezza diretta possono concedere l'autorizzazione per quella entità.  
  
 Gli utenti che dispongono dell'autorizzazione CONTROL SERVER, ad esempio i membri del ruolo predefinito del server **sysadmin**, possono concedere qualsiasi autorizzazione per qualsiasi entità a sicurezza diretta nel server. Gli utenti che dispongono dell'autorizzazione CONTROL per un database, ad esempio i membri del ruolo predefinito del database **db_owner**, possono concedere qualsiasi autorizzazione per qualsiasi entità a protezione diretta nel database. Gli utenti che dispongono dell'autorizzazione CONTROL in uno schema, possono concedere qualsiasi autorizzazione per qualsiasi oggetto all'interno dello schema.  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [Autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
