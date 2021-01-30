---
description: GRANT (autorizzazioni per schemi) (Transact-SQL)
title: GRANT (autorizzazioni per schemi) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/19/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- schemas [SQL Server], permissions
- permissions [SQL Server], schemas
- GRANT statement, schemas
- granting permissions [SQL Server], schemas
ms.assetid: b2aa1fc8-e7af-45d2-9f80-737543c8aa95
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bfe31bbd312b2df347eadf33d0c8b53ec327db2d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161225"
---
# <a name="grant-schema-permissions-transact-sql"></a>GRANT (autorizzazioni per schemi) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Concede autorizzazioni per uno schema.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
GRANT permission  [ ,...n ] ON SCHEMA :: schema_name  
    TO database_principal [ ,...n ]  
    [ WITH GRANT OPTION ]  
    [ AS granting_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *permission*  
 Specifica un'autorizzazione che può essere concessa per uno schema. Per un elenco delle autorizzazioni, vedere la sezione Osservazioni di seguito in questo argomento.  
  
 ON SCHEMA **::** schema *_name*  
 Specifica lo schema a cui vengono concesse le autorizzazioni. Il qualificatore di ambito **::** è obbligatorio.  
  
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
  
AS *granting_principal*  
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
  
> [!IMPORTANT]  
>  Una combinazione di autorizzazioni ALTER e REFERENCE potrebbe consentire in alcuni casi al beneficiario di visualizzare dati o eseguire funzioni non autorizzate. Ad esempio: un utente con autorizzazione ALTER per una tabella e autorizzazione REFERENCE per una funzione può creare una colonna calcolata su una funzione e determinarne l'esecuzione. In questo caso, è inoltre necessario disporre dell'autorizzazione SELECT per la colonna calcolata.  
  
 Uno schema è un'entità a protezione diretta a livello di database contenuta nel database padre nella gerarchia di autorizzazioni. Di seguito sono elencate le autorizzazioni più specifiche e limitate che è possibile concedere per uno schema, insieme alle autorizzazioni più generali che le includono in modo implicito.  
  
|Autorizzazione dello schema|Autorizzazione dello schema in cui è inclusa|Autorizzazione del database in cui è inclusa|  
|-----------------------|----------------------------------|------------------------------------|  
|ALTER|CONTROL|ALTER ANY SCHEMA|  
|CONTROL|CONTROL|CONTROL|  
|CREATE SEQUENCE|ALTER|ALTER ANY SCHEMA|  
|DELETE|CONTROL|DELETE|  
|EXECUTE|CONTROL|EXECUTE|  
|INSERT|CONTROL|INSERT|  
|REFERENCES|CONTROL|REFERENCES|  
|SELECT|CONTROL|SELECT|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|UPDATE|CONTROL|UPDATE|  
|VIEW CHANGE TRACKING|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
> [!CAUTION]  
>  Un utente che dispone dell'autorizzazione ALTER per uno schema può utilizzare la concatenazione della proprietà per accedere alle entità a protezione diretta degli altri schemi, incluse quelle per cui tale a utente è stato negato l'accesso in modo esplicito. La concatenazione della proprietà ignora infatti i controlli delle autorizzazioni per gli oggetti a cui viene fatto riferimento se questi sono di proprietà dell'entità a cui appartengono gli oggetti che vi fanno riferimento. Un utente che dispone dell'autorizzazione ALTER per uno schema può creare procedure, sinonimi e viste di proprietà appartenenti al proprietario dello schema. Tramite la concatenazione della proprietà, questi oggetti avranno accesso alle informazioni contenute negli altri schemi appartenenti al proprietario dello schema. Se possibile, è consigliabile evitare di concedere l'autorizzazione ALTER per uno schema se al relativo proprietario appartengono anche altri schemi.  
  
 Ad esempio, questo problema può verificarsi negli scenari seguenti. In questi scenari si presuppone che un utente, denominato U1, disponga dell'autorizzazione ALTER per lo schema S1, che all'utente U1 sia stato negato l'accesso a un oggetto tabella, denominato T1, nello schema S2 e che gli schemi S1 e S2 appartengano allo stesso proprietario.  
  
 L'utente U1 dispone dell'autorizzazione CREATE PROCEDURE per il database e dell'autorizzazione EXECUTE per lo schema S1. L'utente U1 può pertanto creare una stored procedure e quindi accedere all'oggetto negato T1 nella stored procedure.  
  
 L'utente U1 dispone dell'autorizzazione CREATE SYNONYM per il database e dell'autorizzazione SELECT per lo schema S1. L'utente U1 può pertanto creare un sinonimo nello schema S1 per l'oggetto negato T1 e quindi accedere a tale oggetto utilizzando il sinonimo.  
  
 L'utente U1 dispone dell'autorizzazione CREATE VIEW per il database e dell'autorizzazione SELECT per lo schema S1. L'utente U1 può pertanto creare una vista nello schema S1 per eseguire una query sui dati dall'oggetto negato T1 e quindi accedere a tale oggetto utilizzando la vista.
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente che concede le autorizzazioni (o l'entità specificata con l'opzione AS) deve disporre della relativa autorizzazione con GRANT OPTION oppure di un'autorizzazione di livello superiore che include l'autorizzazione che viene concessa.  
  
 Se si usano l'opzione AS, sono previsti questi requisiti aggiuntivi.  
  
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
  
### <a name="a-granting-insert-permission-on-schema-humanresources-to-guest"></a>R. Concessione dell'autorizzazione INSERT per lo schema HumanResources a guest  
  
```sql  
GRANT INSERT ON SCHEMA :: HumanResources TO guest;  
```  
  
### <a name="b-granting-select-permission-on-schema-person-to-database-user-wiljo"></a>B. Concessione dell'autorizzazione SELECT per lo schema Person all'utente di database WilJo  
  
```sql  
GRANT SELECT ON SCHEMA :: Person TO WilJo WITH GRANT OPTION;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [DENY - Autorizzazioni per schemi &#40;Transact-SQL&#41;](../../t-sql/statements/deny-schema-permissions-transact-sql.md)   
 [REVOKE - Autorizzazioni per schemi &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-schema-permissions-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [Autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)  
  
  
