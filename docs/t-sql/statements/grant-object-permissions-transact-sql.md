---
description: GRANT - autorizzazioni per oggetti (Transact-SQL)
title: GRANT - autorizzazioni per oggetti (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server], objects
- GRANT statement, objects
ms.assetid: c001c2e7-d092-43d4-8fa6-693b3ec4c3ea
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 681cb64aa6ef4874d331d61e2d8c07df2ed06ac1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161236"
---
# <a name="grant-object-permissions-transact-sql"></a>GRANT - autorizzazioni per oggetti (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Concede le autorizzazioni per tabelle, viste, funzioni con valori di tabella, stored procedure, stored procedure estese, funzioni scalari, funzioni di aggregazione, code di servizi o sinonimi.  
  

  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
GRANT <permission> [ ,...n ] ON   
    [ OBJECT :: ][ schema_name ]. object_name [ ( column [ ,...n ] ) ]  
    TO <database_principal> [ ,...n ]   
    [ WITH GRANT OPTION ]  
    [ AS <database_principal> ]  
  
<permission> ::=  
    ALL [ PRIVILEGES ] | permission [ ( column [ ,...n ] ) ]  
  
<database_principal> ::=   
        Database_user   
    | Database_role   
    | Application_role   
    | Database_user_mapped_to_Windows_User   
    | Database_user_mapped_to_Windows_Group   
    | Database_user_mapped_to_certificate   
    | Database_user_mapped_to_asymmetric_key   
    | Database_user_with_no_login  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *permission*  
 Specifica un'autorizzazione che può essere concessa per un oggetto contenuto nello schema. Per un elenco delle autorizzazioni, vedere la sezione Osservazioni di seguito in questo argomento.  
  
 ALL  
 L'opzione ALL non concede tutte le autorizzazioni possibili. L'opzione ALL equivale a concedere tutte le autorizzazioni [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)]-92 applicabili all'oggetto specificato. Il significato di ALL è variabile, come indicato di seguito:  
  
- Autorizzazioni per funzioni scalari: EXECUTE, REFERENCES.  
- Autorizzazioni per funzioni con valori di tabella: DELETE, INSERT, REFERENCES, SELECT, UPDATE.  
- Autorizzazioni per stored procedure: EXECUTE.  
- Autorizzazioni per tabelle: DELETE, INSERT, REFERENCES, SELECT, UPDATE.  
- Autorizzazioni per viste: DELETE, INSERT, REFERENCES, SELECT, UPDATE.  
  
PRIVILEGES  
 Incluso per compatibilità con [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)]-92. Non modifica il funzionamento di ALL.  
  
*column*  
 Specifica il nome di una colonna in una tabella, vista o funzione con valori di tabella per la quale viene concessa l'autorizzazione. Le parentesi ( ) sono obbligatorie. Per una colonna si possono concedere solo le autorizzazioni SELECT, REFERENCES e UPDATE. È possibile specificare *column* nella clausola delle autorizzazioni o dopo il nome dell'entità a protezione diretta.  
  
> [!CAUTION]  
>  Un'istruzione DENY a livello di tabella non ha la precedenza rispetto a un'istruzione GRANT a livello di colonna. Questa incoerenza nella gerarchia delle autorizzazioni è stata mantenuta per compatibilità con le versioni precedenti.  
  
 ON [ OBJECT :: ] [ *schema_name* ] . *object_name*  
 Specifica l'oggetto per cui viene concessa l'autorizzazione. L'utilizzo di OBJECT è facoltativo se si specifica *schema_name*. Se si utilizza OBJECT, il qualificatore di ambito :: è obbligatorio. Se si omette *schema_name*, viene usato lo schema predefinito. Se si specifica *schema_name*, il qualificatore di ambito dello schema (.) è obbligatorio.  
  
 TO \<database_principal>  
 Specifica l'entità a cui viene concessa l'autorizzazione.  
  
 WITH GRANT OPTION  
 Indica che l'entità potrà inoltre concedere l'autorizzazione specificata ad altre entità.  
  
 AS \<database_principal>Specifica un'entità dalla quale l'entità che esegue la query ottiene il diritto di concedere l'autorizzazione.  
  
 *Database_user*  
 Specifica un utente di database.  
  
 *Database_role*  
 Specifica un ruolo del database.  
  
 *Application_role*  
 Specifica un ruolo applicazione.  
  
 *Database_user_mapped_to_Windows_User*  
 Specifica un utente del database sul quale viene eseguito il mapping a un utente di Windows.  
  
 *Database_user_mapped_to_Windows_Group*  
 Specifica un utente del database sul quale viene eseguito il mapping a un gruppo di Windows.  
  
 *Database_user_mapped_to_certificate*  
 Specifica un utente del database sul quale viene eseguito il mapping a un certificato.  
  
 *Database_user_mapped_to_asymmetric_key*  
 Specifica un utente del database sul quale viene eseguito il mapping a una chiave asimmetrica.  
  
 *Database_user_with_no_login*  
 Specifica un utente del database per cui non esiste un'entità corrispondente a livello del server.  
  
## <a name="remarks"></a>Osservazioni  
  
> [!IMPORTANT]  
>  Una combinazione di autorizzazioni ALTER e REFERENCE potrebbe consentire in alcuni casi al beneficiario di visualizzare dati o eseguire funzioni non autorizzate. Ad esempio: un utente con autorizzazione ALTER per una tabella e autorizzazione REFERENCE per una funzione può creare una colonna calcolata su una funzione e determinarne l'esecuzione. In questo caso, sarebbe inoltre necessario disporre dell'autorizzazione SELECT per la colonna calcolata.  
  
 Le informazioni sugli oggetti sono visibili in varie viste del catalogo. Per altre informazioni, vedere [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md).  
  
 Un oggetto è un'entità a protezione diretta a livello di schema contenuta nello schema padre nella gerarchia delle autorizzazioni. Nella tabella seguente sono elencate le autorizzazioni più specifiche e limitate che è possibile concedere per un oggetto, insieme alle autorizzazioni più generali che le includono in modo implicito.  
  
|Autorizzazione per l'oggetto|Autorizzazione dell'oggetto in cui è inclusa|Autorizzazione dello schema in cui è inclusa|  
|-----------------------|----------------------------------|----------------------------------|  
|ALTER|CONTROL|ALTER|  
|CONTROL|CONTROL|CONTROL|  
|DELETE|CONTROL|DELETE|  
|EXECUTE|CONTROL|EXECUTE|  
|INSERT|CONTROL|INSERT|  
|RECEIVE|CONTROL|CONTROL|  
|REFERENCES|CONTROL|REFERENCES|  
|SELECT|RECEIVE|SELECT|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|UPDATE|CONTROL|UPDATE|  
|VIEW CHANGE TRACKING|CONTROL|VIEW CHANGE TRACKING|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente che concede le autorizzazioni (o l'entità specificata con l'opzione AS) deve disporre della relativa autorizzazione con GRANT OPTION oppure di un'autorizzazione di livello superiore che include l'autorizzazione che viene concessa.  
  
 Se si utilizza l'opzione AS, sono previsti i requisiti aggiuntivi seguenti.  
  
|AS|Autorizzazione aggiuntiva necessaria|  
|--------|------------------------------------|  
|Utente del database|Autorizzazione IMPERSONATE per l'utente, appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|  
|Utente del database di cui è stato eseguito il mapping a un account di accesso di Windows|Autorizzazione IMPERSONATE per l'utente, appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|  
|Utente del database di cui è stato eseguito il mapping a un gruppo di Windows|Appartenenza al gruppo di Windows, appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|  
|Utente del database di cui è stato eseguito il mapping a un certificato|Appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|  
|Utente del database di cui è stato eseguito il mapping a una chiave asimmetrica|Appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|  
|Utente del database di cui non è stato eseguito il mapping ad alcuna entità server|Autorizzazione IMPERSONATE per l'utente, appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|  
|Ruolo del database|Autorizzazione ALTER per il ruolo, appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|  
|Ruolo applicazione|Autorizzazione ALTER per il ruolo, appartenenza al ruolo predefinito del database db_securityadmin, appartenenza al ruolo predefinito del database db_owner o appartenenza al ruolo predefinito del server sysadmin.|  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-granting-select-permission-on-a-table"></a>R. Concessione dell'autorizzazione SELECT per una tabella  
 Nell'esempio seguente viene concessa l'autorizzazione `SELECT` all'utente `RosaQdM` per la tabella `Person.Address` nel database `AdventureWorks2012`.  
  
```sql  
GRANT SELECT ON OBJECT::Person.Address TO RosaQdM;  
GO  
```  
  
### <a name="b-granting-execute-permission-on-a-stored-procedure"></a>B. Concessione dell'autorizzazione EXECUTE per una stored procedure  
 Nell'esempio seguente viene concessa l'autorizzazione `EXECUTE` per la stored procedure `HumanResources.uspUpdateEmployeeHireInfo` a un ruolo applicazione denominato `Recruiting11`.  
  
```sql  
USE AdventureWorks2012;   
GRANT EXECUTE ON OBJECT::HumanResources.uspUpdateEmployeeHireInfo  
    TO Recruiting11;  
GO   
```  
  
### <a name="c-granting-references-permission-on-a-view-with-grant-option"></a>C. Concessione dell'autorizzazione REFERENCES per una vista con GRANT OPTION  
 Nell'esempio seguente viene concessa l'autorizzazione `REFERENCES` per la colonna `BusinessEntityID` nella vista `HumanResources.vEmployee` all'utente `Wanida` con `GRANT OPTION`.  
  
```sql  
GRANT REFERENCES (BusinessEntityID) ON OBJECT::HumanResources.vEmployee   
    TO Wanida WITH GRANT OPTION;  
GO  
```  
  
### <a name="d-granting-select-permission-on-a-table-without-using-the-object-phrase"></a>D. Concessione dell'autorizzazione SELECT per una tabella senza utilizzare OBJECT  
 Nell'esempio seguente viene concessa l'autorizzazione `SELECT` all'utente `RosaQdM` per la tabella `Person.Address` nel database `AdventureWorks2012`.  
  
```sql  
GRANT SELECT ON Person.Address TO RosaQdM;  
GO  
```  
  
### <a name="e-granting-select-permission-on-a-table-to-a-domain-account"></a>E. Concessione dell'autorizzazione SELECT per una tabella a un account di dominio  
 Nell'esempio seguente viene concessa l'autorizzazione `SELECT` all'utente `AdventureWorks2012\RosaQdM` per la tabella `Person.Address` nel database `AdventureWorks2012`.  
  
```sql  
GRANT SELECT ON Person.Address TO [AdventureWorks2012\RosaQdM];  
GO  
```  
  
### <a name="f-granting-execute-permission-on-a-procedure-to-a-role"></a>F. Concessione dell'autorizzazione EXECUTE per una stored procedure a un ruolo  
 Nell'esempio seguente viene creato un ruolo a cui viene concessa l'autorizzazione `EXECUTE` per la stored procedure `uspGetBillOfMaterials` nel database `AdventureWorks2012` .  
  
```sql  
CREATE ROLE newrole ;  
GRANT EXECUTE ON dbo.uspGetBillOfMaterials TO newrole ;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [DENY - autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/deny-object-permissions-transact-sql.md)   
 [REVOKE - autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-object-permissions-transact-sql.md)   
 [Viste del catalogo dell'oggetto &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Autorizzazioni &#40;motore di database&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Entità &#40;motore di database&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Securables](../../relational-databases/security/securables.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)  
  
  

