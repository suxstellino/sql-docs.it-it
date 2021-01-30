---
description: DENY - autorizzazioni per assembly Transact-SQL)
title: DENY - autorizzazioni per assembly (Transact-SQL) | Microsoft Docs
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
- assemblies [CLR integration], permissions
- DENY statement, assemblies
- denying permissions [SQL Server], assemblies
ms.assetid: be0896b3-f793-44c9-9390-1c6e58f33b2b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 51a45ac7c65df9dd6ed28b8d76aaf2d59a627e53
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177803"
---
# <a name="deny-assembly-permissions-transact-sql"></a>DENY - autorizzazioni per assembly Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Nega le autorizzazioni per un assembly.  
  

  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
DENY { permission [ ,...n ] } ON ASSEMBLY :: assembly_name  
    TO database_principal [ ,...n ]   
        [ CASCADE ]  
        [ AS denying_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *permission*  
 Specifica un'autorizzazione che può essere negata per un assembly. Vedere l'elenco riportato di seguito.  
  
 ON ASSEMBLY **::** _assembly_name_  
 Specifica l'assembly per cui viene negata l'autorizzazione. Il qualificatore di ambito "::" è obbligatorio.  
  
 *database_principal*  
 Specifica l'entità a cui viene negata l'autorizzazione. I tipi validi sono:  
  
-   utente del database  
  
-   ruolo del database  
  
-   ruolo applicazione  
  
-   utente del database sul quale viene eseguito il mapping a un account di accesso di Windows  
  
-   utente del database di cui è stato eseguito il mapping a un gruppo di Windows  
  
-   utente del database di cui è stato eseguito il mapping a un certificato  
  
-   utente del database di cui è stato eseguito il mapping a una chiave asimmetrica  
  
-   utente del database non mappato ad alcuna entità server.  
  
 CASCADE  
 Indica che l'autorizzazione negata viene negata anche ad altre entità alle quali è stata concessa da questa entità.  
  
 *denying_principal*  
 Specifica un'entità dalla quale l'entità che esegue la query ottiene il diritto di negare l'autorizzazione. I tipi validi sono:  
  
-   utente del database  
  
-   ruolo del database  
  
-   ruolo applicazione  
  
-   utente del database sul quale viene eseguito il mapping a un account di accesso di Windows  
  
-   utente del database di cui è stato eseguito il mapping a un gruppo di Windows  
  
-   utente del database di cui è stato eseguito il mapping a un certificato  
  
-   utente del database di cui è stato eseguito il mapping a una chiave asimmetrica  
  
-   utente del database non mappato ad alcuna entità server.  
  
## <a name="remarks"></a>Commenti  
 Un assembly è un'entità a protezione diretta a livello di database contenuta nel database padre nella gerarchia delle autorizzazioni. Di seguito sono elencate le autorizzazioni più specifiche e limitate che è possibile negare per un assembly, insieme alle autorizzazioni più generali che le includono in modo implicito.  
  
|Autorizzazione dell'assembly|Autorizzazione dell'assembly in cui è inclusa|Autorizzazione del database in cui è inclusa|  
|-------------------------|------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ASSEMBLY|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL per l'assembly. Se si usano l'opzione AS, l'entità specificata deve essere proprietaria dell'assembly.  
  
## <a name="see-also"></a>Vedere anche  
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [Autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [CREATE ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
