---
description: sp_unbindefault (Transact-SQL)
title: sp_unbindefault (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_unbindefault_TSQL
- sp_unbindefault
dev_langs:
- TSQL
helpviewer_keywords:
- sp_unbindefault
ms.assetid: c96a6c5e-f3ca-4c1e-b64b-0d8ef6986af8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ecc8a6c7a012a36da4deeacf8b4e66e6096c8227
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202344"
---
# <a name="sp_unbindefault-transact-sql"></a>sp_unbindefault (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Disassocia, o rimuove, un valore predefinito da una colonna o da un tipo di dati alias nel database corrente.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] È consigliabile creare definizioni predefinite utilizzando la parola chiave DEFAULT nelle istruzioni [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) o [Create Table](../../t-sql/statements/create-table-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_unbindefault [ @objname = ] 'object_name'   
     [ , [ @futureonly = ] 'futureonly_flag' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @objname = ] 'object_name'` Nome della tabella e della colonna o tipo di dati alias da cui il valore predefinito deve essere non associato. *object_name* è di **tipo nvarchar (776)** e non prevede alcun valore predefinito. Tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si tenta innanzitutto di risolvere gli identificatori costituiti da due parti in nomi di colonne e quindi in tipi di dati alias.  
  
 Quando si disassocia un valore predefinito da un tipo di dati alias, vengono disassociate anche le colonne di tale tipo di dati con lo stesso valore predefinito. Le colonne di tale tipo di dati a cui il valore predefinito è associato in modo diretto non vengono modificate.  
  
> [!NOTE]  
>  *object_name* possono contenere parentesi quadre **[]** come caratteri identificatore delimitati. Per altre informazioni, vedere [Identificatori del database](../../relational-databases/databases/database-identifiers.md).  
  
`[ @futureonly = ] 'futureonly_flag'` Viene utilizzato solo quando si esegue la disassociazione di un valore predefinito da un tipo di dati alias. *futureonly_flag* è di tipo **varchar (15)** e il valore predefinito è null. Quando *futureonly_flag* è **futureonly**, le colonne esistenti del tipo di dati non perdono il valore predefinito specificato.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 Per visualizzare il testo di un valore predefinito, eseguire **sp_helptext** con il nome del parametro predefinito.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per disassociare un valore predefinito da una colonna di una tabella, è necessario disporre dell'autorizzazione ALTER per la tabella. Per disassociare un valore predefinito da un tipo di dati alias, è necessario disporre dell'autorizzazione CONTROL per il tipo o dell'autorizzazione ALTER per lo schema a cui il tipo appartiene.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-unbinding-a-default-from-a-column"></a>R. Disassociazione di un valore predefinito da una colonna  
 Nell'esempio seguente viene disassociato il valore predefinito dalla colonna `hiredate` di una tabella `employees`.  
  
```  
EXEC sp_unbindefault 'employees.hiredate';  
```  
  
### <a name="b-unbinding-a-default-from-an-alias-data-type"></a>B. Disassociazione di un valore predefinito da un tipo di dati alias  
 Nell'esempio seguente viene disassociato il valore predefinito dal tipo di dati alias `ssn`. La disassociazione viene applicata sia alle colonne di tale tipo di dati già esistenti che a quelle che verranno create in futuro.  
  
```  
EXEC sp_unbindefault 'ssn';  
```  
  
### <a name="c-using-the-futureonly_flag"></a>C. Utilizzo di futureonly_flag  
 Nell'esempio seguente la disassociazione viene impostata per i futuri utilizzi del tipo di dati alias `ssn` senza alterare le colonne `ssn` esistenti.  
  
```  
EXEC sp_unbindefault 'ssn', 'futureonly';  
```  
  
### <a name="d-using-delimited-identifiers"></a>D. Utilizzo di identificatori delimitati  
 Nell'esempio seguente viene illustrato l'utilizzo di identificatori delimitati nel parametro *object_name* .  
  
```  
CREATE TABLE [t.3] (c1 int); -- Notice the period as part of the table   
-- name.  
CREATE DEFAULT default2 AS 0;  
GO  
EXEC sp_bindefault 'default2', '[t.3].c1' ;  
-- The object contains two periods;  
-- the first is part of the table name and the second   
-- distinguishes the table name from the column name.  
EXEC sp_unbindefault '[t.3].c1';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md)   
 [DROP DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-default-transact-sql.md)   
 [sp_bindefault &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
