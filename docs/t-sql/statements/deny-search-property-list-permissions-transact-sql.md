---
title: DENY - Autorizzazioni per elenco di proprietà di ricerca
description: Negare le autorizzazioni per un elenco di proprietà di ricerca.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], permissions
- DENY statement, search property list permissions
- denying permissions [SQL Server], search property lists
- search property lists [SQL Server], permissions
ms.assetid: 96513cb4-a9c0-4834-97a4-ddc0777b8415
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6e5ec634f87010b012bcc05a17d0711415770383
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177772"
---
# <a name="deny-search-property-list-permissions-transact-sql"></a>Autorizzazioni per l'elenco delle proprietà di ricerca DENY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Nega le autorizzazioni per un elenco di proprietà di ricerca.  
 
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DENY permission [ ,...n ] ON  
        SEARCH PROPERTY LIST :: search_property_list_name  
    TO database_principal [ ,...n ] [ CASCADE ]  
    [ AS denying_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *permission*  
 Nome di un'autorizzazione. I mapping validi tra le autorizzazioni e le entità a protezione diretta sono descritti nella sezione "Osservazioni" più avanti in questo argomento.  
  
ON SEARCH PROPERTY LIST **::** _search_property_list_name_  
 Specifica l'elenco delle proprietà di ricerca per cui viene negata l'autorizzazione. Il qualificatore di ambito :: è obbligatorio.  
  
*database_principal*  
 Specifica l'entità a cui viene negata l'autorizzazione. Le possibili entità sono le seguenti:  
  
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
 Specifica un'entità dalla quale l'entità che esegue la query ottiene il diritto di negare l'autorizzazione. Le possibili entità sono le seguenti:  
  
-   utente del database  
-   ruolo del database  
-   ruolo applicazione  
-   utente del database sul quale viene eseguito il mapping a un account di accesso di Windows  
-   utente del database di cui è stato eseguito il mapping a un gruppo di Windows  
-   utente del database di cui è stato eseguito il mapping a un certificato  
-   utente del database di cui è stato eseguito il mapping a una chiave asimmetrica  
-   utente del database non mappato ad alcuna entità server.  
  
## <a name="remarks"></a>Osservazioni  
  
## <a name="search-property-list-permissions"></a>Autorizzazioni SEARCH PROPERTY LIST  
 Un elenco di proprietà di ricerca è un'entità a protezione diretta a livello di database contenuta nel database padre nella gerarchia delle autorizzazioni. Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che è possibile negare per un elenco di proprietà di ricerca, insieme alle autorizzazioni più generali che le includono in modo implicito.  
  
|Autorizzazioni per l'elenco delle proprietà di ricerca|Autorizzazioni dell'elenco delle proprietà di ricerca in cui è inclusa|Autorizzazione del database in cui è inclusa|  
|-------------------------------------|------------------------------------------------|------------------------------------|  
|ALTER|CONTROL|ALTER ANY FULLTEXT CATALOG|  
|CONTROL|CONTROL|CONTROL|  
|REFERENCES|CONTROL|REFERENCES|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL per il catalogo full-text. Se si utilizza l'opzione AS, l'entità specificata deve essere proprietaria del catalogo full-text.  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-search-property-list-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [GRANT - autorizzazioni per l'elenco delle proprietà di ricerca &#40;Transact-SQL&#41;](../../t-sql/statements/grant-search-property-list-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [REVOKE - Autorizzazioni per l'elenco delle proprietà di ricerca &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-search-property-list-permissions-transact-sql.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.registered_search_property_lists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)   
 [Eseguire ricerche nelle proprietà dei documenti con elenchi delle proprietà di ricerca](../../relational-databases/search/search-document-properties-with-search-property-lists.md)  
  
  
