---
title: GRANT - Autorizzazioni per elenco di proprietà di ricerca
description: Concedere le autorizzazioni per un elenco di proprietà di ricerca.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], permissions
- search property lists [SQL Server], permissions
- granting permissions [SQL Server], search property lists
- GRANT statement, search property list permissions
ms.assetid: bb2d2550-9c0e-4a88-b50c-12e481d4d3ae
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 12ba95e2f19d85fd98c6e7cc90ec743b60772184
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754371"
---
# <a name="grant-search-property-list-permissions-transact-sql"></a>Autorizzazioni per l'elenco delle proprietà di ricerca GRANT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Concede le autorizzazioni per un elenco di proprietà di ricerca.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
GRANT permission [ ,...n ] ON   
    SEARCH PROPERTY LIST :: search_property_list_name  
    TO database_principal [ ,...n ]  
    [ WITH GRANT OPTION ]  
    [ AS granting_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *permission*  
 Nome di un'autorizzazione. I mapping validi tra le autorizzazioni e le entità a protezione diretta sono descritti nella sezione "Osservazioni" più avanti in questo argomento.  
  
 Nell'elenco delle proprietà di ricerca **::**_search_property_list_name_  
 Specifica l'elenco delle proprietà di ricerca per cui viene concessa l'autorizzazione. Il qualificatore di ambito **::** è obbligatorio.  
  
 **Per visualizzare gli elenchi delle proprietà di ricerca esistenti**  
  
-   [sys.registered_search_property_lists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)  
  
 *database_principal*  
 Specifica l'entità a cui viene concessa l'autorizzazione. Le possibili entità sono le seguenti:  
  
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
  
 AS *granting_principal*  
 Specifica un'entità dalla quale l'entità che esegue la query ottiene il diritto di concedere l'autorizzazione. Le possibili entità sono le seguenti:  
  
-   utente del database  
  
-   ruolo del database  
  
-   ruolo applicazione  
  
-   utente del database sul quale viene eseguito il mapping a un account di accesso di Windows  
  
-   utente del database di cui è stato eseguito il mapping a un gruppo di Windows  
  
-   utente del database di cui è stato eseguito il mapping a un certificato  
  
-   utente del database di cui è stato eseguito il mapping a una chiave asimmetrica  
  
-   utente del database non mappato ad alcuna entità server.  
  
## <a name="remarks"></a>Commenti  
  
## <a name="search-property-list-permissions"></a>Autorizzazioni SEARCH PROPERTY LIST  
 Un elenco di proprietà di ricerca è un'entità a protezione diretta a livello di database contenuta nel database padre nella gerarchia delle autorizzazioni. Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che è possibile concedere per un elenco di proprietà di ricerca, insieme alle autorizzazioni più generali che le includono in modo implicito.  
  
|Autorizzazioni per l'elenco delle proprietà di ricerca|Autorizzazioni dell'elenco delle proprietà di ricerca in cui è inclusa|Autorizzazione del database in cui è inclusa|  
|-------------------------------------|------------------------------------------------|------------------------------------|  
|ALTER|CONTROL|ALTER ANY FULLTEXT CATALOG|  
|CONTROL|CONTROL|CONTROL|  
|REFERENCES|CONTROL|REFERENCES|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente che concede le autorizzazioni (o l'entità specificata con l'opzione AS) deve disporre della relativa autorizzazione con GRANT OPTION oppure di un'autorizzazione di livello superiore che include l'autorizzazione che viene concessa.  
  
 Se si utilizza l'opzione AS, sono previsti i requisiti aggiuntivi seguenti.  
  
|AS *granting_principal*|Autorizzazione aggiuntiva necessaria|  
|------------------------------|------------------------------------|  
|Utente del database|Autorizzazione IMPERSONATE per l'utente, appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|  
|Utente del database di cui è stato eseguito il mapping a un account di accesso di Windows|Autorizzazione IMPERSONATE per l'utente, appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|  
|Utente del database di cui è stato eseguito il mapping a un gruppo di Windows|Appartenenza al gruppo di Windows, appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|  
|Utente del database di cui è stato eseguito il mapping a un certificato|Appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|  
|Utente del database di cui è stato eseguito il mapping a una chiave asimmetrica|Appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|  
|Utente del database di cui non è stato eseguito il mapping ad alcuna entità server|Autorizzazione IMPERSONATE per l'utente, appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|  
|Ruolo del database|Autorizzazione ALTER per il ruolo, appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|  
|Ruolo applicazione|Autorizzazione ALTER per il ruolo, appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|  
  
 I proprietari degli oggetti possono concedere autorizzazioni per gli oggetti di cui sono proprietari. Le entità con l'autorizzazione CONTROL per un'entità a sicurezza diretta possono concedere l'autorizzazione per quella entità.  
  
 Gli utenti che dispongono dell'autorizzazione CONTROL SERVER, ad esempio i membri del ruolo predefinito del server sysadmin, possono concedere qualsiasi autorizzazione per qualsiasi entità a protezione diretta nel server. Gli utenti che dispongono dell'autorizzazione CONTROL in un database, ad esempio i membri del ruolo predefinito del database db_owner, possono concedere qualsiasi autorizzazione per qualsiasi entità a protezione diretta nel database. Gli utenti che dispongono dell'autorizzazione CONTROL in uno schema, possono concedere qualsiasi autorizzazione per qualsiasi oggetto all'interno dello schema.  
  
## <a name="examples"></a>Esempi  
  
### <a name="granting-permissions-to-a-search-property-list"></a>Concessione di autorizzazioni a un elenco di proprietà di ricerca  
 Nell'esempio seguente, a `Mary` viene concessa l'autorizzazione `VIEW DEFINITION` per l'elenco delle proprietà di ricerca `DocumentTablePropertyList`.  
  
```sql  
GRANT VIEW DEFINITION  
    ON SEARCH PROPERTY LIST :: DocumentTablePropertyList  
    TO Mary ;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-search-property-list-transact-sql.md)   
 [DENY-autorizzazioni per l'elenco delle proprietà di ricerca &#40;&#41;Transact-SQL ](../../t-sql/statements/deny-search-property-list-permissions-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [REVOKE-autorizzazioni per l'elenco delle proprietà di ricerca &#40;&#41;Transact-SQL ](../../t-sql/statements/revoke-search-property-list-permissions-transact-sql.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.registered_search_property_lists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)   
 [Eseguire ricerche nelle proprietà dei documenti con elenchi delle proprietà di ricerca](../../relational-databases/search/search-document-properties-with-search-property-lists.md)  
  
  
