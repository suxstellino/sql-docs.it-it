---
description: REVOKE (autorizzazioni per chiavi asimmetriche) (Transact-SQL)
title: REVOKE (autorizzazioni per chiavi asimmetriche) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], asymmetric keys
- asymmetric keys [SQL Server], permissions
- REVOKE statement, asymmetric keys
ms.assetid: 1a1063e8-ffc7-4775-a40d-e155740ad7b2
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 3ab78d858d084a139e3932dab30990e495f6717c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209945"
---
# <a name="revoke-asymmetric-key-permissions-transact-sql"></a>REVOKE (autorizzazioni per chiavi asimmetriche) (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Revoca le autorizzazioni per una chiave asimmetrica.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
REVOKE [ GRANT OPTION FOR ] { permission  [ ,...n ] }   
    ON ASYMMETRIC KEY :: asymmetric_key_name   
    { TO | FROM } database_principal [ ,...n ]  
    [ CASCADE ]  
    [ AS revoking_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 GRANT OPTION FOR  
 Indica che verrà revocata la capacità di concedere l'autorizzazione specificata.  
  
> [!IMPORTANT]  
>  Se l'autorizzazione specificata è stata concessa all'entità senza l'opzione GRANT, l'autorizzazione stessa verrà revocata.  
  
 *permission*  
 Specifica un'autorizzazione che può essere revocata in un assembly. Vedere l'elenco riportato di seguito.  
  
 ON ASYMMETRIC KEY **::** _asymmetric_key_name_  
 Specifica la chiave asimmetrica per cui viene revocata l'autorizzazione. Il qualificatore di ambito **::** è obbligatorio.  
  
 *database_principal*  
 Specifica l'entità da cui viene revocata l'autorizzazione. I tipi validi sono:  
  
-   Utente del database  
  
-   Ruolo del database  
  
-   Ruolo applicazione  
  
-   Utente del database di cui è stato eseguito il mapping a un account di accesso di Windows  
  
-   Utente del database di cui è stato eseguito il mapping a un gruppo di Windows  
  
-   Utente del database di cui è stato eseguito il mapping a un certificato  
  
-   Utente del database di cui è stato eseguito il mapping a una chiave asimmetrica  
  
-   Utente del database sul quale non viene eseguito il mapping ad alcuna entità server.  
  
 CASCADE  
 Indica che l'autorizzazione che viene revocata anche ad altre entità a cui è stata concessa o negata da questa entità. L'autorizzazione stessa non verrà revocata.  
  
> [!CAUTION]  
>  La revoca propagata di un'autorizzazione concessa con WITH GRANT OPTION comporterà la revoca sia delle autorizzazioni GRANT che delle autorizzazioni DENY per tale autorizzazione.  
  
 AS *revoking_principal*  
 Specifica un'entità dalla quale l'entità che esegue la query ottiene il diritto di revocare l'autorizzazione. I tipi validi sono:  
  
-   Utente del database  
  
-   Ruolo del database  
  
-   Ruolo applicazione  
  
-   Utente del database di cui è stato eseguito il mapping a un account di accesso di Windows  
  
-   Utente del database di cui è stato eseguito il mapping a un gruppo di Windows  
  
-   Utente del database di cui è stato eseguito il mapping a un certificato  
  
-   Utente del database di cui è stato eseguito il mapping a una chiave asimmetrica  
  
-   Utente del database sul quale non viene eseguito il mapping ad alcuna entità server.  
  
## <a name="remarks"></a>Commenti  
 Una chiave asimmetrica è un'entità a protezione diretta a livello di database contenuta nel database padre nella gerarchia delle autorizzazioni. Di seguito sono indicate le autorizzazioni più specifiche e limitate che è possibile revocare per una chiave asimmetrica, insieme alle autorizzazioni più generali che le includono in modo implicito.  
  
|Autorizzazione della chiave asimmetrica|Autorizzazione della chiave asimmetrica in cui è inclusa.|Autorizzazione del database in cui è inclusa|  
|-------------------------------|------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ASYMMETRIC KEY|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL per la chiave asimmetrica.  
  
## <a name="see-also"></a>Vedere anche  
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [Autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
