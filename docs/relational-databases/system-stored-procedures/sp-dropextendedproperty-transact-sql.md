---
description: sp_dropextendedproperty (Transact-SQL)
title: sp_dropextendedproperty (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropextendedproperty_TSQL
- sp_dropextendedproperty
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropextendedproperty
ms.assetid: 4851865a-86ca-4823-991a-182dd1934075
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c42236cc1f0157d774ea38d356ba8f122407c2c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205133"
---
# <a name="sp_dropextendedproperty-transact-sql"></a>sp_dropextendedproperty (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina una proprietà estesa esistente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_dropextendedproperty   
    [ @name = ] { 'property_name' }  
      [ , [ @level0type = ] { 'level0_object_type' }   
        , [ @level0name = ] { 'level0_object_name' }   
            [ , [ @level1type = ] { 'level1_object_type' }   
              , [ @level1name = ] { 'level1_object_name' }   
                [ , [ @level2type = ] { 'level2_object_type' }   
                  , [ @level2name = ] { 'level2_object_name' }   
                ]   
            ]   
        ]   
    ]   
```  
  
## <a name="arguments"></a>Argomenti  
 [ @name =] {'*property_name*'}  
 Nome della proprietà che si desidera eliminare. *property_name* è di **tipo sysname** e non può essere null.  
  
 [ @level0type =] {'*level0_object_type*'}  
 Nome del tipo di oggetto di livello 0 specificato. *level0_object_type* è di tipo **varchar (128)** e il valore predefinito è null.  
  
 I possibili valori sono ASSEMBLY, CONTRACT, EVENT NOTIFICATION, FILEGROUP, MESSAGE TYPE, PARTITION FUNCTION, PARTITION SCHEME, REMOTE SERVICE BINDING, ROUTE, SCHEMA, SERVICE, USER, TRIGGER, TYPE e NULL.  
  
> [!IMPORTANT]  
>  I tipi USER e TYPE come tipi di livello 0 verranno rimossi in una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evitare pertanto di utilizzarle in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui sono state implementate. Utilizzare SCHEMA come tipo di livello 0 anziché USER. Per TYPE utilizzare SCHEMA come tipo di livello 0 e TYPE come tipo di livello 1.  
  
 [ @level0name =] {'*level0_object_name*'}  
 Nome del tipo di oggetto di livello 0 specificato. *level0_object_name* è di **tipo sysname** e il valore predefinito è null.  
  
 [ @level1type =] {'*level1_object_type*'}  
 Tipo di oggetto di livello 1. *level1_object_type* è di tipo **varchar (128)** e il valore predefinito è null. I possibili valori sono AGGREGATE, DEFAULT, FUNCTION, LOGICAL FILE NAME, PROCEDURE, QUEUE, RULE, SYNONYM, TABLE, TABLE_TYPE, TYPE, VIEW, XML SCHEMA COLLECTION e NULL.  
  
 [ @level1name =] {'*level1_object_name*'}  
 Nome del tipo di oggetto di livello 1 specificato. *level1_object_name* è di **tipo sysname** e il valore predefinito è null.  
  
 [ @level2type =] {'*level2_object_type*'}  
 Tipo di oggetto di livello 2. *level2_object_type* è di tipo **varchar (128)** e il valore predefinito è null. I possibili valori sono COLUMN, CONSTRAINT, EVENT NOTIFICATION, INDEX, PARAMETER, TRIGGER e NULL.  
  
 [ @level2name =] {'*level2_object_name*'}  
 Nome del tipo di oggetto di livello 2 specificato. *level2_object_name* è di **tipo sysname** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 Ai fini della definizione delle proprietà estese, gli oggetti inclusi in un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono classificati in base a tre livelli, ovvero 0, 1 e 2. Il livello 0 è il livello più alto e viene definito come oggetti inclusi nell'ambito del database. Gli oggetti di livello 1 sono inclusi nell'ambito di uno schema o utente, mentre gli oggetti di livello 2 sono contenuti dagli oggetti di livello 1. È possibile definire le proprietà estese per gli oggetti di qualsiasi livello. I riferimenti a un oggetto presente in un livello devono essere qualificati con i tipi e i nomi di tutti gli oggetti di livello superiore.  
  
 Dato un *property_name* valido, se tutti i tipi e i nomi di oggetto sono null e una proprietà esiste nel database corrente, la proprietà viene eliminata. Vedere l'esempio B più avanti in questo argomento.  
  
## <a name="permissions"></a>Autorizzazioni  
 I membri dei ruoli predefiniti del database db_owner e db_ddladmin possono eliminare le proprietà estese di qualsiasi oggetto, anche se il ruolo db_ddladmin non può aggiungere proprietà al database stesso oppure a utenti o ruoli.  
  
 Gli utenti possono eliminare le proprietà estese degli oggetti di cui sono proprietari oppure per i quali dispongono delle autorizzazioni ALTER o CONTROL.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-dropping-an-extended-property-on-a-column"></a>R. Eliminazione di una proprietà estesa in una colonna  
 Nell'esempio seguente la proprietà `caption` viene rimossa dalla colonna `id` nella tabella `T1` inclusa nello schema `dbo`.  
  
```  
CREATE TABLE T1 (id int , name char (20));  
GO  
EXEC sp_addextendedproperty   
     @name = 'caption'   
    ,@value = 'Employee ID'   
    ,@level0type = 'schema'   
    ,@level0name = dbo  
    ,@level1type = 'table'  
    ,@level1name = 'T1'  
    ,@level2type = 'column'  
    ,@level2name = id;  
GO  
EXEC sp_dropextendedproperty   
     @name = 'caption'   
    ,@level0type = 'schema'   
    ,@level0name = dbo  
    ,@level1type = 'table'  
    ,@level1name = 'T1'  
    ,@level2type = 'column'  
    ,@level2name = id;  
GO  
DROP TABLE T1;  
GO  
```  
  
### <a name="b-dropping-an-extended-property-on-a-database"></a>B. Eliminazione di una proprietà estesa in un database  
 Nell'esempio seguente la proprietà denominata viene rimossa `MS_Description` dal [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] database di esempio. Poiché la proprietà si trova nel database stesso, non viene specificato alcun tipo e nome di oggetto.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_dropextendedproperty   
@name = N'MS_Description';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sys.fn_listextendedproperty &#40;&#41;Transact-SQL ](../../relational-databases/system-functions/sys-fn-listextendedproperty-transact-sql.md)   
 [sp_addextendedproperty &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md)   
 [sp_updateextendedproperty &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-updateextendedproperty-transact-sql.md)   
 [sys.extended_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/extended-properties-catalog-views-sys-extended-properties.md)  
  
  
