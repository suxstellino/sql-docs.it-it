---
description: sp_unbindrule (Transact-SQL)
title: sp_unbindrule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_unbindrule_TSQL
- sp_unbindrule
dev_langs:
- TSQL
helpviewer_keywords:
- sp_unbindrule
ms.assetid: f54ee155-c3c9-4f1a-952e-632a8339f0cc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 42b652e2cc604465ac0db1d2c6f65739916c8684
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209674"
---
# <a name="sp_unbindrule-transact-sql"></a>sp_unbindrule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Disassocia una regola da una colonna o da un tipo di dati alias nel database corrente.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] È consigliabile creare definizioni predefinite utilizzando la parola chiave DEFAULT nelle istruzioni [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) o [Create Table](../../t-sql/statements/create-table-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_unbindrule [ @objname = ] 'object_name'   
     [ , [ @futureonly = ] 'futureonly_flag' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @objname = ] 'object_name'` Nome della tabella e della colonna o tipo di dati alias da cui la regola non è associata. *object_name* è di **tipo nvarchar (776)** e non prevede alcun valore predefinito. Tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si tenta innanzitutto di risolvere gli identificatori costituiti da due parti in nomi di colonne e quindi in tipi di dati alias. La disassociazione di una regola da un tipo di dati alias viene estesa anche alle colonne dello stesso tipo di dati a cui è applicata la regola. Non vengono tuttavia modificate le colonne di questo stesso tipo di dati a cui la regola è associata in modo diretto.  
  
> [!NOTE]  
>  *object_name* possono contenere parentesi quadre **[]** come caratteri identificatore delimitati. Per altre informazioni, vedere [Identificatori del database](../../relational-databases/databases/database-identifiers.md).  
  
`[ @futureonly = ] 'futureonly_flag'` Viene utilizzato solo quando si esegue la disassociazione di una regola da un tipo di dati alias. *futureonly_flag* è di tipo **varchar (15)** e il valore predefinito è null. Quando *futureonly_flag* è **futureonly**, le colonne esistenti del tipo di dati non perdono la regola specificata.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 Per visualizzare il testo di una regola, eseguire **sp_helptext** specificando il nome della regola come parametro.  
  
 Quando una regola non è associata, le informazioni sull'associazione vengono rimosse dalla tabella **sys. Columns** se la regola è stata associata a una colonna e dalla tabella **sys. Types** se la regola è stata associata a un tipo di dati alias.  
  
 Una regola disassociata da un tipo di dati alias viene disassociata anche dalle colonne a cui è applicato tale tipo di dati. La regola può essere ancora associata a colonne i cui tipi di dati sono stati modificati in seguito dalla clausola ALTER COLUMN di un'istruzione ALTER TABLE, è necessario separare in modo specifico la regola da queste colonne utilizzando **sp_unbindrule** e specificando il nome della colonna.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per disassociare una regola dalla colonna di una tabella è necessaria l'autorizzazione ALTER sulla tabella. Per disassociare una regola da un tipo di dati alias è necessaria l'autorizzazione CONTROL sul tipo di dati o l'autorizzazione ALTER sullo schema a cui appartiene il tipo.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-unbinding-a-rule-from-a-column"></a>R. Disassociazione di una regola da una colonna  
 Nell'esempio seguente la regola viene disassociata dalla colonna `startdate` di una tabella `employees`.  
  
```  
EXEC sp_unbindrule 'employees.startdate';  
```  
  
### <a name="b-unbinding-a-rule-from-an-alias-data-type"></a>B. Disassociazione di una regola da un tipo di dati alias  
 Nell'esempio seguente la regola viene disassociata dal tipo di dati alias `ssn`. La disassociazione viene applicata alle colonne di questo tipo di dati, sia esistenti che create successivamente.  
  
```  
EXEC sp_unbindrule ssn;  
```  
  
### <a name="c-using-futureonly_flag"></a>C. Utilizzo di futureonly_flag  
 Nell'esempio seguente la regola viene disassociata dal tipo di dati alias `ssn` senza influire sulle colonne `ssn` esistenti.  
  
```  
EXEC sp_unbindrule 'ssn', 'futureonly';  
```  
  
### <a name="d-using-delimited-identifiers"></a>D. Utilizzo di identificatori delimitati  
 Nell'esempio seguente viene illustrato l'utilizzo di identificatori delimitati nel parametro *object_name* .  
  
```  
CREATE TABLE [t.4] (c1 int); -- Notice the period as part of the table   
-- name.  
GO  
CREATE RULE rule2 AS @value > 100;  
GO  
EXEC sp_bindrule rule2, '[t.4].c1' -- The object contains two   
-- periods; the first is part of the table name and the second   
-- distinguishes the table name from the column name.  
GO  
EXEC sp_unbindrule '[t.4].c1';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE RULE &#40;Transact-SQL&#41;](../../t-sql/statements/create-rule-transact-sql.md)   
 [DROP RULE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-rule-transact-sql.md)   
 [sp_bindrule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindrule-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
