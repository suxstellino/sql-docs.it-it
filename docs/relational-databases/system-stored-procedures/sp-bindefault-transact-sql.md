---
description: sp_bindefault (Transact-SQL)
title: sp_bindefault (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_bindefault
- sp_bindefault_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_bindefault
ms.assetid: 3da70c10-68d0-4c16-94a5-9e84c4a520f6
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4aacb6bff81f204e89e8704406d61ffb69ae84fb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199200"
---
# <a name="sp_bindefault-transact-sql"></a>sp_bindefault (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Associa un valore predefinito a una colonna o a un tipo di dati alias.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Si consiglia di creare definizioni predefinite usando invece la parola chiave DEFAULT delle istruzioni [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) o [Create Table](../../t-sql/statements/create-table-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_bindefault [ @defname = ] 'default' ,   
    [ @objname = ] 'object_name'   
    [ , [ @futureonly = ] 'futureonly_flag' ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @defname = ] 'default'` Nome dell'oggetto predefinito creato da CREATE DEFAULT. il *valore predefinito* è **nvarchar (776)** e non prevede alcun valore predefinito.  
  
`[ @objname = ] 'object_name'` Nome della tabella e della colonna o tipo di dati alias a cui associare il valore predefinito. *object_name* è di **tipo nvarchar (776)** e non prevede alcun valore predefinito. non è possibile definire *object_name* con i tipi **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, **XML** o CLR definiti dall'utente.  
  
 Se *object_name* è un nome in una parte, viene risolto come tipo di dati alias. Se è un nome in due o tre parti, viene prima risolto come tabella e colonna. Se la risoluzione non riesce, viene risolto come tipo di dati alias. Per impostazione predefinita, le colonne esistenti del tipo di dati alias ereditano *default*, a meno che un valore predefinito non sia stato associato direttamente alla colonna. Impossibile associare un valore predefinito a una colonna di tipo **Text**, **ntext**, **Image**, **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, **XML**, **timestamp** o CLR definito dall'utente, una colonna con la proprietà Identity, una colonna calcolata o una colonna che dispone già di un vincolo Default.  
  
> [!NOTE]  
>  *object_name* possono contenere parentesi quadre **[]** come identificatori delimitati. Per altre informazioni, vedere [Identificatori del database](../../relational-databases/databases/database-identifiers.md).  
  
`[ @futureonly = ] 'futureonly_flag'` Viene utilizzato solo quando si associa un valore predefinito a un tipo di dati alias. *futureonly_flag* è di tipo **varchar (15)** e il valore predefinito è null. Quando questo parametro è impostato su **futureonly**, le colonne esistenti del tipo di dati non possono ereditare il nuovo valore predefinito. Questo parametro non viene mai utilizzato per l'associazione di un valore predefinito a una colonna. Se *futureonly_flag* è null, il nuovo valore predefinito viene associato a tutte le colonne del tipo di dati alias che attualmente non dispongono di un valore predefinito o che utilizzano il valore predefinito esistente del tipo di dati alias.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 È possibile utilizzare **sp_bindefault** per associare un nuovo valore predefinito a una colonna, sebbene sia preferibile utilizzare il vincolo predefinito oppure a un tipo di dati alias senza disassociare un valore predefinito esistente. Il valore predefinito esistente viene ignorato. Non è possibile associare un valore predefinito a un tipo di dati di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o a un tipo CLR definito dall'utente. Se il valore predefinito non è compatibile con la colonna a cui è stato associato, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] restituisce un messaggio di errore quando si tenta di inserire il valore predefinito, non in fase di associazione.  
  
 Le colonne esistenti del tipo di dati alias ereditano il nuovo valore predefinito, a meno che non sia associato un valore predefinito direttamente a tali colonne o *futureonly_flag* sia specificato come **futureonly**. Le nuove colonne del tipo di dati alias ereditano sempre il valore predefinito.  
  
 Quando si associa un valore predefinito a una colonna, le informazioni correlate vengono aggiunte alla vista del catalogo **sys. Columns** . Quando si associa un valore predefinito a un tipo di dati alias, le informazioni correlate vengono aggiunte alla vista del catalogo **sys. Types** .  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente deve essere il proprietario della tabella oppure un membro del ruolo predefinito del server **sysadmin** o dei ruoli predefiniti del database **db_owner** e **db_ddladmin** .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-binding-a-default-to-a-column"></a>R. Associazione di un valore predefinito a una colonna  
 Il valore predefinito `today` è stato definito nel database corrente tramite l'istruzione CREATE DEFAULT, in questo esempio tale valore viene associato alla colonna `HireDate` della tabella `Employee`. Quando si aggiunge una riga alla tabella `Employee` senza specificare dati nella colonna `HireDate` la colonna include il valore predefinito `today`.  
  
```  
USE master;  
GO  
EXEC sp_bindefault 'today', 'HumanResources.Employee.HireDate';  
```  
  
### <a name="b-binding-a-default-to-an-alias-data-type"></a>B. Associazione di un valore predefinito a un tipo di dati alias  
 Il valore predefinito `def_ssn` e il tipo di dati alias `ssn` esistono già. Nell'esempio seguente il valore predefinito `def_ssn` viene associato a `ssn`. Il valore predefinito viene ereditato da tutte le colonne a cui è stato assegnato il tipo di dati alias `ssn` in fase di creazione della tabella. Le colonne esistenti di tipo **SSN** ereditano anche la **def_ssn** predefinita, a meno che non sia specificato **futureonly** per *futureonly_flag* valore o, a meno che la colonna non disponga di un oggetto predefinito associato direttamente a tale colonna. I valori predefiniti associati alle colonne sono sempre prioritari rispetto a quelli associati ai tipi di dati.  
  
```  
USE master;  
GO  
EXEC sp_bindefault 'def_ssn', 'ssn';  
```  
  
### <a name="c-using-the-futureonly_flag"></a>C. Utilizzo di futureonly_flag  
 Nell'esempio seguente il valore predefinito `def_ssn` viene associato al tipo di dati alias `ssn`. Poiché **futureonly** è specificato, non sono interessate colonne esistenti di tipo `ssn` .  
  
```  
USE master;  
GO  
EXEC sp_bindefault 'def_ssn', 'ssn', 'futureonly';  
```  
  
### <a name="d-using-delimited-identifiers"></a>D. Utilizzo di identificatori delimitati  
 Nell'esempio seguente viene illustrato l'utilizzo di identificatori delimitati, `[t.1]` , in *object_name*.  
  
```  
USE master;  
GO  
CREATE TABLE [t.1] (c1 int);   
-- Notice the period as part of the table name.  
EXEC sp_bindefault 'default1', '[t.1].c1' ;  
-- The object contains two periods;   
-- the first is part of the table name,   
-- and the second distinguishes the table name from the column name.  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md)   
 [DROP DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-default-transact-sql.md)   
 [sp_unbindefault &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-unbindefault-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
