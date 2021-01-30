---
description: DENY - autorizzazioni per chiavi asimmetriche (Transact-SQL)
title: DENY (autorizzazioni per chiavi asimmetriche) (Transact-SQL) | Microsoft Docs
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
- denying permissions [SQL Server], asymmetric keys
- encryption [SQL Server], asymmetric keys
- permissions [SQL Server], asymmetric keys
- asymmetric keys [SQL Server], permissions
- DENY statement, asymmetric keys
- cryptography [SQL Server], asymmetric keys
ms.assetid: dd7d8cd5-536b-460c-ab5b-cb4752bbdfaa
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: b88a749f5e933620932d913c092c502de133021f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177795"
---
# <a name="deny-asymmetric-key-permissions-transact-sql"></a>DENY - autorizzazioni per chiavi asimmetriche (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Nega le autorizzazioni per una chiave asimmetrica.  
   
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
DENY { permission  [ ,...n ] }   
    ON ASYMMETRIC KEY :: asymmetric_key_name   
        TO database_principal [ ,...n ]  
    [ CASCADE ]  
        [ AS denying_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *permission*  
 Specifica un'autorizzazione che può essere negata per una chiave asimmetrica. Vedere l'elenco riportato di seguito.  
  
 ON ASYMMETRIC KEY **::** _asymmetric_key_name_  
 Specifica la chiave asimmetrica per cui viene negata l'autorizzazione. Il qualificatore di ambito "::" è obbligatorio.  
  
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
 Una chiave asimmetrica è un'entità a protezione diretta a livello di database contenuta nel database padre nella gerarchia delle autorizzazioni. Di seguito sono indicate le autorizzazioni più specifiche e limitate che è possibile concedere per una chiave asimmetrica, insieme alle autorizzazioni più generali che le includono in modo implicito.  
  
|Autorizzazione della chiave asimmetrica|Autorizzazione della chiave asimmetrica in cui è inclusa.|Autorizzazione del database in cui è inclusa|  
|-------------------------------|------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ASYMMETRIC KEY|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL per la chiave asimmetrica. Se viene utilizzata la clausola AS, l'entità specificata deve essere proprietaria della chiave asimmetrica.  
  
## <a name="see-also"></a>Vedere anche  
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [Autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
