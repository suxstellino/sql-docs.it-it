---
description: sp_addextendedproperty (Transact-SQL)
title: sp_addextendedproperty (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addextendedproperty
- sp_addextendedproperty_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addextendedproperty
ms.assetid: 565483ea-875b-4133-b327-d0006d2d7b4c
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 448f2c67d7e7c09f0e2768cc0d6c359224d15b45
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182777"
---
# <a name="sp_addextendedproperty-transact-sql"></a>sp_addextendedproperty (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Aggiunge una nuova proprietà estesa a un oggetto di database.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addextendedproperty  
    [ @name = ] { 'property_name' }  
    [ , [ @value = ] { 'value' }   
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
[;]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @name ] = {'*property_name*'}  
 Nome della proprietà che si desidera aggiungere. *property_name* è di **tipo sysname** e non può essere null. I nomi possono includere inoltre stringhe vuote o di caratteri non alfanumerici, nonché valori binary.  
  
 [ @value =] {'*valore*'}  
 Valore da associare alla proprietà. il valore è **sql_variant** e il *valore* predefinito è null. La dimensione di *value* non può superare 7.500 byte.  
  
 [ @level0type =] {'*level0_object_type*'}  
 Tipo di oggetto di livello 0. *level0_object_type* è di tipo **varchar (128)** e il valore predefinito è null.  
  
 I possibili valori sono ASSEMBLY, CONTRACT, EVENT NOTIFICATION, FILEGROUP, MESSAGE TYPE, PARTITION FUNCTION, PARTITION SCHEME, REMOTE SERVICE BINDING, ROUTE, SCHEMA, SERVICE, USER, TRIGGER, TYPE, PLAN GUIDE e NULL.  
  
> [!IMPORTANT]  
>  La possibilità di specificare USER come tipo di livello 0 in una proprietà estesa di un oggetto di tipo di livello 1 verrà rimossa in una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. In alternativa, utilizzare SCHEMA come tipo di livello 0. Quando, ad esempio, si definisce una proprietà estesa in una tabella, specificare lo schema della tabella anziché un nome utente. La possibilità di specificare TYPE come tipo di livello 0 verrà rimossa in una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per TYPE utilizzare SCHEMA come tipo di livello 0 e TYPE come tipo di livello 1.  
  
 [ @level0name =] {'*level0_object_name*'}  
 Nome del tipo di oggetto di livello 0 specificato. *level0_object_name* è di **tipo sysname** e il valore predefinito è null.  
  
 [ @level1type =] {'*level1_object_type*'}  
 Tipo di oggetto di livello 1. *level1_object_type* è di tipo **varchar (128)** e il valore predefinito è null. GLI input validi sono AGGREGAte, DEFAULT, FUNCTION, LOGICAL FILE NAME, PROCEDURE, QUEUE, RULE, SEQUENCE, SYNONYM, TABLE, TABLE_TYPE, TYPE, VIEW, XML SCHEMA COLLECTION e NULL.    
 [ @level1name =] {'*level1_object_name*'}  
 Nome del tipo di oggetto di livello 1 specificato. *level1_object_name* è di **tipo sysname** e il valore predefinito è null.  
  
 [ @level2type =] {'*level2_object_type*'}  
 Tipo di oggetto di livello 2. *level2_object_type* è di tipo **varchar (128)** e il valore predefinito è null. I possibili valori sono COLUMN, CONSTRAINT, EVENT NOTIFICATION, INDEX, PARAMETER, TRIGGER e NULL.  
  
 [ @level2name =] {'*level2_object_name*'}  
 Nome del tipo di oggetto di livello 2 specificato. *level2_object_name* è di **tipo sysname** e il valore predefinito è null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 Ai fini della definizione delle proprietà estese, gli oggetti inclusi in un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono classificati in base a tre livelli, ovvero 0, 1 e 2. Il livello 0 è il livello più alto e viene definito come oggetti inclusi nell'ambito del database. Gli oggetti di livello 1 sono inclusi nell'ambito di uno schema o utente, mentre gli oggetti di livello 2 sono contenuti dagli oggetti di livello 1. È possibile definire le proprietà estese per gli oggetti di qualsiasi livello.  
  
 È necessario qualificare i riferimenti a un oggetto in un livello mediante i nomi degli oggetti proprietari di livello superiore o che li contengono. Se, ad esempio, si aggiunge una proprietà estesa a una colonna di tabella (livello 2), è necessario specificare anche il nome della tabella (livello 1) che include la colonna e lo schema (livello 0) contenente la tabella.  
  
 Se tutti i tipi e i nomi di oggetto sono Null, la proprietà appartiene al database corrente stesso.  
  
 Le proprietà estese non sono consentite negli oggetti di sistema, negli oggetti esterni all'ambito di un database definito dall'utente oppure negli oggetti non elencati come valori validi nella sezione Argomenti.  
  
 Le proprietà estese non sono consentite nelle tabelle ottimizzate per la memoria.  
  
## <a name="replicating-extended-properties"></a>Replica delle proprietà estese  
 Le proprietà estese vengono replicate solo nella sincronizzazione iniziale tra il server di pubblicazione e il Sottoscrittore. Se si aggiungono o si modificano proprietà estese dopo la sincronizzazione iniziale, le modifiche apportate non vengono replicate. Per ulteriori informazioni sulla replica di oggetti di database, vedere [pubblicare dati e oggetti di database](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
## <a name="schema-vs-user"></a>Differenza tra schemi  Utente  
 Non è consigliabile specificare USER come tipo di livello 0 quando si applica una proprietà estesa a un oggetto di database perché ciò può causare ambiguità nella risoluzione dei nomi. Si supponga, ad esempio che l'utente Mary sia proprietaria di due schemi (Mary e MySchema) e che entrambi gli schemi includano una tabella denominata MyTable. Se Mary aggiunge una proprietà estesa alla tabella MyTable e specifica **@level0type = n'User '**, **@level0name = Mary**, non è chiaro a quale tabella viene applicata la proprietà estesa. Per mantenere la compatibilità con le versioni precedenti, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] applicherà la proprietà alla tabella inclusa nello schema denominato Mary.  
  
## <a name="permissions"></a>Autorizzazioni  
 I membri dei ruoli predefiniti del database db_owner e db_ddladmin possono aggiungere le proprietà estese a qualsiasi oggetto, anche se il ruolo db_ddladmin non può aggiungere proprietà al database stesso oppure a utenti o ruoli.  
  
 Gli utenti possono aggiungere le proprietà estese agli oggetti di cui sono proprietari oppure per i quali dispongono delle autorizzazioni ALTER o CONTROL.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-adding-an-extended-property-to-a-database"></a>R. Aggiunta di una proprietà estesa a un database  
 Nell'esempio seguente la proprietà `'Caption'` con valore `'AdventureWorks2012 Sample OLTP Database'` viene aggiunta al database di esempio `AdventureWorks2012` .  
  
```  
USE AdventureWorks2012;  
GO  
--Add a caption to the AdventureWorks2012 Database object itself.  
EXEC sp_addextendedproperty   
@name = N'Caption',   
@value = 'AdventureWorks2012 Sample OLTP Database';  
```  
  
### <a name="b-adding-an-extended-property-to-a-column-in-a-table"></a>B. Aggiunta di una proprietà estesa a una colonna in una tabella  
 Nell'esempio seguente la proprietà Caption viene aggiunta alla colonna `PostalCode` nella tabella `Address`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_addextendedproperty   
@name = N'Caption',   
@value = 'Postal code is a required column.',  
@level0type = N'Schema', @level0name = 'Person',  
@level1type = N'Table',  @level1name = 'Address',  
@level2type = N'Column', @level2name = 'PostalCode';  
GO  
```  
  
### <a name="c-adding-an-input-mask-property-to-a-column"></a>C. Aggiunta di una proprietà Input Mask a una colonna  
 Nell'esempio seguente viene aggiunta la proprietà Input Mask con valore '`99999 or 99999-9999 or #### ###`' alla colonna `PostalCode` della tabella `Address`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_addextendedproperty   
@name = N'Input Mask ', @value = '99999 or 99999-9999 or #### ###',  
@level0type = N'Schema', @level0name = 'Person',  
@level1type = N'Table', @level1name = 'Address',   
@level2type = N'Column',@level2name = 'PostalCode';  
GO  
```  
  
### <a name="d-adding-an-extended-property-to-a-filegroup"></a>D. Aggiunta di una proprietà estesa a un filegroup  
 Nell'esempio seguente viene aggiunta una proprietà estesa al filegroup `PRIMARY` .  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_DescriptionExample',   
@value = N'Primary filegroup for the AdventureWorks2012 sample database.',   
@level0type = N'FILEGROUP', @level0name = 'PRIMARY';  
GO  
```  
  
### <a name="e-adding-an-extended-property-to-a-schema"></a>E. Aggiunta di una proprietà estesa a uno schema  
 Nell'esempio seguente viene aggiunta una proprietà estesa allo schema `HumanResources` .  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_addextendedproperty   
@name = N'MS_DescriptionExample',  
@value = N'Contains objects related to employees and departments.',  
@level0type = N'SCHEMA',   
@level0name = 'HumanResources';  
```  
  
### <a name="f-adding-an-extended-property-to-a-table"></a>F. Aggiunta di una proprietà estesa a una tabella  
 Nell'esempio seguente viene aggiunta una proprietà estesa alla tabella `Address` nello schema `Person` .  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_DescriptionExample',   
@value = N'Street address information for customers, employees, and vendors.',   
@level0type = N'SCHEMA', @level0name = 'Person',  
@level1type = N'TABLE',  @level1name = 'Address';  
GO  
```  
  
### <a name="g-adding-an-extended-property-to-a-role"></a>G. Aggiunta di una proprietà estesa a un ruolo  
 Nell'esempio seguente viene creato un ruolo applicazione, cui viene aggiunta una proprietà estesa.  
  
```  
USE AdventureWorks2012;   
GO  
CREATE APPLICATION ROLE Buyers  
WITH Password = '987G^bv876sPY)Y5m23';   
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_Description',   
@value = N'Application Role for the Purchasing Department.',  
@level0type = N'USER',  
@level0name = 'Buyers';  
```  
  
### <a name="h-adding-an-extended-property-to-a-type"></a>H. Aggiunta di una proprietà estesa a un tipo  
 Nell'esempio seguente viene aggiunta una proprietà estesa a un tipo.  
  
```  
USE AdventureWorks2012;   
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_Description',   
@value = N'Data type (alias) to use for any column that represents an order number. For example a sales order number or purchase order number.',   
@level0type = N'SCHEMA',   
@level0name = N'dbo',   
@level1type = N'TYPE',   
@level1name = N'OrderNumber';  
```  
  
### <a name="i-adding-an-extended-property-to-a-user"></a>I. Aggiunta di una proprietà estesa a un utente  
 Nell'esempio seguente viene creato un utente, cui viene aggiunta una proprietà estesa.  
  
```  
USE AdventureWorks2012;   
GO  
CREATE USER CustomApp WITHOUT LOGIN ;   
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_Description',   
@value = N'User for an application.',   
@level0type = N'USER',   
@level0name = N'CustomApp';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sys.fn_listextendedproperty &#40;&#41;Transact-SQL ](../../relational-databases/system-functions/sys-fn-listextendedproperty-transact-sql.md)   
 [sp_dropextendedproperty &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropextendedproperty-transact-sql.md)   
 [sp_updateextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updateextendedproperty-transact-sql.md)  
  
  
