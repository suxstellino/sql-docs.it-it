---
description: sp_bindrule (Transact-SQL)
title: sp_bindrule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_bindrule_TSQL
- sp_bindrule
dev_langs:
- TSQL
helpviewer_keywords:
- sp_bindrule
ms.assetid: 2606073e-c52f-498d-a923-5026b9d97e67
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 266707eca11f6664d816a958e9a3051d3648339b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197694"
---
# <a name="sp_bindrule-transact-sql"></a>sp_bindrule (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Associa una regola a una colonna o a un tipo di dati alias.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Usare invece[vincoli UNIQUE e check](../../relational-databases/tables/unique-constraints-and-check-constraints.md) . I vincoli CHECK vengono creati tramite la parola chiave CHECK delle istruzioni [Create Table](../../t-sql/statements/create-table-transact-sql.md) o [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_bindrule [ @rulename = ] 'rule' ,   
     [ @objname = ] 'object_name'   
     [ , [ @futureonly = ] 'futureonly_flag' ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @rulename = ] 'rule'` Nome di una regola creata dall'istruzione CREATE RULE. la *regola* è di **tipo nvarchar (776)** e non prevede alcun valore predefinito.  
  
`[ @objname = ] 'object_name'` È la tabella e la colonna oppure il tipo di dati alias a cui deve essere associata la regola. Una regola non può essere associata a una colonna **text**, **ntext**, **image**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)**, **xml**, di tipo CLR definito dall'utente o **timestamp**, né a una colonna calcolata.  
  
 *object_name* è di **tipo nvarchar (776)** e non prevede alcun valore predefinito. Se *object_name* è un nome in una parte, viene risolto come tipo di dati alias. Se è un nome in due o tre parti, viene prima risolto come tabella e colonna. Se la risoluzione non riesce, viene risolto come tipo di dati alias. Per impostazione predefinita, le colonne esistenti del tipo di dati alias ereditano la *regola* , a meno che una regola non sia stata associata direttamente alla colonna.  
  
> [!NOTE]  
>  *object_name* possono contenere i caratteri di parentesi quadre **[** e **]** come caratteri di identificatore delimitati. Per altre informazioni, vedere [Identificatori del database](../../relational-databases/databases/database-identifiers.md).  
  
> [!NOTE]  
>  Le regole create nelle espressioni che utilizzano tipi di dati alias possono essere associate a colonne o a tipi di dati alias, ma non è possibile compilarle quando un altro elemento fa riferimento ad esse. Evitare di utilizzare le regole create nei tipi di dati alias.  
  
`[ @futureonly = ] 'futureonly_flag'` Viene utilizzato solo quando si associa una regola a un tipo di dati alias. *future_only_flag* è di tipo **varchar (15)** e il valore predefinito è null. Questo parametro se impostato su **futureonly** impedisce a colonne esistenti di un tipo di dati alias di ereditare la nuova regola. Se *futureonly_flag* è null, la nuova regola viene associata a tutte le colonne del tipo di dati alias che attualmente non dispongono di alcuna regola o che utilizzano la regola esistente del tipo di dati alias.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 È possibile associare una nuova regola a una colonna (sebbene sia preferibile usare un vincolo CHECK) o a un tipo di dati alias con **sp_bindrule** senza annullare l'associazione di una regola esistente. La regola precedente verrà infatti ignorata. Se una regola viene associata a una colonna con un vincolo CHECK esistente, vengono valutate tutte le restrizioni. Non è possibile associare una regola a un tipo di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 La regola viene applicata quando si esegue un'istruzione INSERT, non in fase di associazione. È possibile associare una regola carattere a una colonna di tipo di dati **numeric** , sebbene tale operazione di inserimento non sia valida.  
  
 Le colonne esistenti del tipo di dati alias ereditano la nuova regola, a meno che non sia specificato *futureonly_flag* come **futureonly**. Le nuove colonne definite con il tipo di dati alias ereditano sempre la regola. Tuttavia, se la clausola ALTER COLUMN di un'istruzione ALTER TABLE imposta una colonna su un tipo di dati alias associato a una regola, la regola associata al tipo di dati non viene ereditata dalla colonna. La regola deve essere associata in modo specifico alla colonna utilizzando **sp_bindrule**.  
  
 Quando si associa una regola a una colonna, le informazioni correlate vengono aggiunte alla tabella **sys. Columns** . Quando si associa una regola a un tipo di dati alias, le informazioni correlate vengono aggiunte alla tabella **sys. Types** .  
  
## <a name="permissions"></a>Autorizzazioni  
 Per associare una regola a una colonna di tabella, è necessario disporre dell'autorizzazione ALTER per la tabella. Per associare una regola a un tipo di dati alias, è richiesta l'autorizzazione CONTROL per il tipo di dati alias o l'autorizzazione ALTER per lo schema a cui appartiene il tipo.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-binding-a-rule-to-a-column"></a>R. Associazione di una regola a una colonna  
 Supponendo che la regola `today` sia stata creata nel database corrente tramite l'istruzione CREATE RULE, nell'esempio seguente viene associata tale regola alla colonna `HireDate` della tabella `Employee`. Quando si aggiunge una riga alla tabella `Employee`, i dati della colonna `HireDate` vengono verificati in base alla regola `today`.  
  
```  
USE master;  
GO  
EXEC sp_bindrule 'today', 'HumanResources.Employee.HireDate';  
```  
  
### <a name="b-binding-a-rule-to-an-alias-data-type"></a>B. Associazione di una regola a un tipo di dati alias  
 Supponendo che esistano la regola `rule_ssn` e il tipo di dati alias `ssn`, nell'esempio seguente viene associata la regola `rule_ssn` a `ssn`. In un'istruzione CREATE TABLE le colonne di tipo `ssn` ereditano la regola `rule_ssn`. Anche le colonne esistenti di tipo `ssn` ereditano la `rule_ssn` regola, a meno che non sia specificato **futureonly** per *futureonly_flag* o `ssn` una regola è associata direttamente a essa. Le regole associate alle colonne sono sempre prioritarie rispetto a quelle associate ai tipi di dati.  
  
```  
USE master;  
GO  
EXEC sp_bindrule 'rule_ssn', 'ssn';  
```  
  
### <a name="c-using-the-futureonly_flag"></a>C. Utilizzo di futureonly_flag  
 Nell'esempio seguente viene associata la regola `rule_ssn` al tipo di dati alias `ssn`. Poiché è stato specificato il flag `futureonly`, l'operazione non ha alcun effetto sulle colonne di tipo `ssn` esistenti.  
  
```  
USE master;  
GO  
EXEC sp_bindrule rule_ssn, 'ssn', 'futureonly';  
```  
  
### <a name="d-using-delimited-identifiers"></a>D. Utilizzo di identificatori delimitati  
 Nell'esempio seguente viene illustrato l'utilizzo di identificatori delimitati nel parametro *object_name* .  
  
```  
USE master;  
GO  
CREATE TABLE [t.2] (c1 int) ;  
-- Notice the period as part of the table name.  
EXEC sp_bindrule rule1, '[t.2].c1' ;  
-- The object contains two periods;   
-- the first is part of the table name   
-- and the second distinguishes the table name from the column name.  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE RULE &#40;Transact-SQL&#41;](../../t-sql/statements/create-rule-transact-sql.md)   
 [DROP RULE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-rule-transact-sql.md)   
 [sp_unbindrule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unbindrule-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
