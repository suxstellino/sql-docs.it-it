---
description: ALTER APPLICATION ROLE (Transact-SQL)
title: ALTER APPLICATION ROLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_APPLICATION_ROLE_TSQL
- ALTER APPLICATION ROLE
dev_langs:
- TSQL
helpviewer_keywords:
- modifying application roles
- passwords [SQL Server], application roles
- ALTER APPLICATION ROLE statement
- application roles [SQL Server], modifying
ms.assetid: c6cd5d0f-18f4-49be-b161-64d9c5569086
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f697afc214f830ad853e5e82597f749d66e82ecd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194697"
---
# <a name="alter-application-role-transact-sql"></a>ALTER APPLICATION ROLE (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Modifica il nome, la password o lo schema predefinito di un ruolo applicazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi
  
```syntaxsql
  
ALTER APPLICATION ROLE application_role_name
    WITH <set_item> [ ,...n ]  
  
<set_item> ::=
    NAME = new_application_role_name
    | PASSWORD = 'password'  
    | DEFAULT_SCHEMA = schema_name  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

 *application_role_name*  
 Nome del ruolo applicazione da modificare.  
  
 NAME =*new_application_role_name*  
 Specifica il nuovo nome del ruolo applicazione. Il nome non deve già essere usato per fare riferimento a un'altra entità nel database.  
  
 PASSWORD ='*password*'  
 Viene specificata la password per il ruolo applicazione. *password* deve soddisfare i requisiti per i criteri password di Windows del computer che sta eseguendo l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È necessario usare sempre password complesse.  
  
 DEFAULT_SCHEMA =*schema_name*  
 Viene specificato il primo schema in cui tramite il server verrà eseguita la ricerca per la risoluzione dei nomi degli oggetti. *schema_name* può essere uno schema che non esiste nel database.  
  
## <a name="remarks"></a>Osservazioni

Se il nuovo nome del ruolo applicazione esiste già nel database, l'istruzione non potrà essere completata. Quando si modifica il nome, la password o lo schema predefinito di un ruolo applicazione, l'ID associato al ruolo non viene modificato.  
  
> [!IMPORTANT]  
>  I criteri di scadenza per le password non vengono applicati alle password del ruolo applicazione. Per questo motivo, si consiglia di prestare la massima attenzione nella scelta di password complesse. Le applicazioni che richiamano i ruoli applicazione devono archiviare le relative password.  
  
 I ruoli applicazione sono visibili nella vista del catalogo sys.database_principals.  
  
> [!CAUTION]  
>  Il comportamento degli schemi in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] è diverso rispetto alle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È possibile che il codice in cui gli schemi sono equivalenti agli utenti del database non restituisca risultati corretti. Le viste del catalogo precedenti, inclusa sysobjects, non devono essere usate in un database in cui è stata usata una delle istruzioni DDL seguenti: CREATE SCHEMA, ALTER SCHEMA, DROP SCHEMA, CREATE USER, ALTER USER, DROP USER, CREATE ROLE, ALTER ROLE, DROP ROLE, CREATE APPROLE, ALTER APPROLE, DROP APPROLE, ALTER AUTHORIZATION. In questi database è necessario usare le nuove viste del catalogo, in cui si tiene conto della separazione tra entità e schemi introdotta in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Per altre informazioni sulle viste del catalogo, vedere [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER ANY APPLICATION ROLE nel database. Per modificare lo schema predefinito, è inoltre richiesta l'autorizzazione ALTER per il ruolo applicazione. Un ruolo applicazione può modificare il proprio schema predefinito, ma non il nome o la password.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-changing-the-name-of-application-role"></a>R. Modifica del nome del ruolo applicazione  
 Nell'esempio seguente viene modificato il nome del ruolo applicazione `weekly_receipts` in `receipts_ledger`.  
  
```sql  
USE AdventureWorks2012;  
CREATE APPLICATION ROLE weekly_receipts   
    WITH PASSWORD = '987Gbv8$76sPYY5m23' ,   
    DEFAULT_SCHEMA = Sales;  
GO  
ALTER APPLICATION ROLE weekly_receipts   
    WITH NAME = receipts_ledger;  
GO  
```  
  
### <a name="b-changing-the-password-of-application-role"></a>B. Modifica della password del ruolo applicazione  
 Nell'esempio seguente viene modificata la password del ruolo applicazione `receipts_ledger`.  
  
```sql  
ALTER APPLICATION ROLE receipts_ledger   
    WITH PASSWORD = '897yUUbv867y$200nk2i';  
GO  
```  
  
### <a name="c-changing-the-name-password-and-default-schema"></a>C. Modifica del nome, della password e dello schema predefinito  
 Nell'esempio seguente vengono modificati contemporaneamente il nome, la password e lo schema predefinito del ruolo applicazione `receipts_ledger`.  
  
```sql  
ALTER APPLICATION ROLE receipts_ledger   
    WITH NAME = weekly_ledger,   
    PASSWORD = '897yUUbv77bsrEE00nk2i',   
    DEFAULT_SCHEMA = Production;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Ruoli applicazione](../../relational-databases/security/authentication-access/application-roles.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [DROP APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-application-role-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
