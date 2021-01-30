---
description: IDENT_CURRENT (Transact-SQL)
title: IDENT_CURRENT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IDENT_CURRENT
- IDENT_CURRENT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- last identity value generated for table
- identity values [SQL Server], last generated
- identity columns, current value
- IDENT_CURRENT function
ms.assetid: 21517ced-39f5-4cd8-8d9c-0a0b8aff554a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 608f7c4745d2d5590fc4d2956f7fca580c9b4111
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184017"
---
# <a name="ident_current-transact-sql"></a>IDENT_CURRENT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Restituisce l'ultimo valore Identity generato per una tabella o una vista specificata. L'ultimo valore Identity generato può essere per qualsiasi sessione e qualsiasi ambito.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
IDENT_CURRENT( 'table_or_view' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*table_or_view*  
Nome della tabella o della vista per la quale viene restituito il valore Identity. *table_or_view* è di tipo **varchar** e non prevede alcun valore predefinito.  
  
## <a name="return-types"></a>Tipi restituiti  
**numerico**([@@MAXPRECISION](../../t-sql/functions/max-precision-transact-sql.md),0))  
  
## <a name="exceptions"></a>Eccezioni  
Restituisce NULL in caso di errore o se un chiamante non dispone dell'autorizzazione necessaria per visualizzare l'oggetto.  
  
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] un utente può visualizzare esclusivamente i metadati delle entità a sicurezza diretta di cui è proprietario o per cui ha ricevuto un'autorizzazione. Di conseguenza, le funzioni predefinite di creazione dei metadati come IDENT_CURRENT possono restituire NULL se l'utente non dispone di alcuna autorizzazione per l'oggetto. Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="remarks"></a>Commenti  
IDENT_CURRENT è simile alle funzioni per valori Identity SCOPE_IDENTITY e @@IDENTITY di [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]. Queste tre funzioni restituiscono infatti gli ultimi valori Identity generati. Tuttavia, l'ambito e la sessione in cui *ultimo* è definito in ciascuna di queste funzioni differiscono:  

-   La funzione IDENT_CURRENT restituisce l'ultimo valore Identity generato per una tabella specifica in qualsiasi sessione e in qualsiasi ambito.  
-   La funzione @@IDENTITY restituisce l'ultimo valore Identity generato per qualsiasi tabella della sessione corrente in tutti gli ambiti.  
-   La funzione SCOPE_IDENTITY restituisce l'ultimo valore Identity generato per qualsiasi tabella della sessione e dell'ambito correnti.  
  
Quando il valore IDENT_CURRENT è NULL, perché la tabella non conteneva righe oppure è stata troncata, la funzione IDENT_CURRENT restituisce il valore di inizializzazione.  
  
Le istruzioni e le transazioni con esito negativo sono in grado di modificare i dati Identity correnti di una tabella e creare gap nei valori della colonna Identity. Non viene mai eseguito il rollback del valore Identity, anche se non si esegue il commit della transazione che ha tentato l'inserimento del valore nella tabella. Se, ad esempio, un'istruzione INSERT ha esito negativo a causa di una violazione di IGNORE_DUP_KEY, il valore Identity corrente per la tabella viene comunque incrementato.  

Quando si usa IDENT_CURRENT su una vista che contiene join, viene restituito NULL, a prescindere dal fatto che una o più delle tabelle unite in join abbiano una colonna Identity. 
  
> [!IMPORTANT]
> Prestare attenzione quando si usa IDENT_CURRENT per stimare il successivo valore Identity generato. È possibile che il valore generato effettivo sia diverso dalla somma di IDENT_CURRENT e IDENT_INCR a causa di inserimenti effettuati da altre sessioni.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-the-last-identity-value-generated-for-a-specified-table"></a>R. Restituzione dell'ultimo valore Identity generato per una tabella specificata  
 Nell'esempio seguente viene restituito l'ultimo valore Identity generato per la tabella `Person.Address` nel database `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT IDENT_CURRENT ('Person.Address') AS Current_Identity;  
GO  
```  
  
### <a name="b-comparing-identity-values-returned-by-ident_current-identity-and-scope_identity"></a>B. Confronto dei valori Identity restituiti da IDENT_CURRENT, @@IDENTITY e SCOPE_IDENTITY  
 Nell'esempio seguente vengono illustrati i diversi valori Identity restituiti dalle funzioni `IDENT_CURRENT`, `@@IDENTITY` e `SCOPE_IDENTITY`.  
  
```sql 
USE AdventureWorks2012;  
GO  
IF OBJECT_ID(N't6', N'U') IS NOT NULL   
    DROP TABLE t6;  
GO  
IF OBJECT_ID(N't7', N'U') IS NOT NULL   
    DROP TABLE t7;  
GO  
CREATE TABLE t6(id INT IDENTITY);  
CREATE TABLE t7(id INT IDENTITY(100,1));  
GO  
CREATE TRIGGER t6ins ON t6 FOR INSERT   
AS  
BEGIN  
   INSERT t7 DEFAULT VALUES  
END;  
GO  
--End of trigger definition  
  
SELECT id FROM t6;  
--IDs empty.  
  
SELECT id FROM t7;  
--ID is empty.  
  
--Do the following in Session 1  
INSERT t6 DEFAULT VALUES;  
SELECT @@IDENTITY;  
/*Returns the value 100. This was inserted by the trigger.*/  
  
SELECT SCOPE_IDENTITY();  
/* Returns the value 1. This was inserted by the   
INSERT statement two statements before this query.*/  
  
SELECT IDENT_CURRENT('t7');  
/* Returns value inserted into t7, that is in the trigger.*/  
  
SELECT IDENT_CURRENT('t6');  
/* Returns value inserted into t6. This was the INSERT statement four statements before this query.*/  
  
-- Do the following in Session 2.  
SELECT @@IDENTITY;  
/* Returns NULL because there has been no INSERT action   
up to this point in this session.*/  
  
SELECT SCOPE_IDENTITY();  
/* Returns NULL because there has been no INSERT action   
up to this point in this scope in this session.*/  
  
SELECT IDENT_CURRENT('t7');  
/* Returns the last value inserted into t7.*/  
```  
  
## <a name="see-also"></a>Vedere anche  
 [@@IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/identity-transact-sql.md)   
 [SCOPE_IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/scope-identity-transact-sql.md)   
 [IDENT_INCR &#40;Transact-SQL&#41;](../../t-sql/functions/ident-incr-transact-sql.md)   
 [IDENT_SEED &#40;Transact-SQL&#41;](../../t-sql/functions/ident-seed-transact-sql.md)   
 [Espressioni &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Funzioni di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)  
  
  
