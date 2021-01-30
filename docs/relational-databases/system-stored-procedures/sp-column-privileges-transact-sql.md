---
description: sp_column_privileges (Transact-SQL)
title: sp_column_privileges (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_column_privileges_TSQL
- sp_column_privileges
dev_langs:
- TSQL
helpviewer_keywords:
- sp_column_privileges
ms.assetid: a3784301-2517-4b1d-bbd9-47404483fad0
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: db99cb2e9acb8ede62a066291cce68d209e495c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174530"
---
# <a name="sp_column_privileges-transact-sql"></a>sp_column_privileges (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce informazioni sui privilegi delle colonne di una tabella dell'ambiente corrente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_column_privileges [ @table_name = ] 'table_name'   
     [ , [ @table_owner = ] 'table_owner' ]   
     [ , [ @table_qualifier = ] 'table_qualifier' ]   
     [ , [ @column_name = ] 'column' ]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @table_name =]'*table_name*'  
 Tabella utilizzata per restituire informazioni del catalogo. *table_name* è di **tipo sysname** e non prevede alcun valore predefinito. I criteri di ricerca con caratteri jolly non sono supportati.  
  
 [ @table_owner =]'*TABLE_OWNER*'  
 Proprietario della tabella utilizzata per restituire informazioni sul catalogo. *TABLE_OWNER* è di **tipo sysname** e il valore predefinito è null. I criteri di ricerca con caratteri jolly non sono supportati. Se *TABLE_OWNER* viene omesso, vengono applicate le regole di visibilità della tabella predefinite del sistema di gestione di database (DBMS) sottostante.  
  
 Se l'utente corrente è il proprietario di una tabella avente il nome specificato, vengono restituite le colonne di tale tabella. Se *TABLE_OWNER* non viene specificato e l'utente corrente non è il proprietario di una tabella con la *table_name* specificata, sp_column privilegi cerca una tabella con il *table_name* specificato di proprietà del proprietario del database. Se viene individuata, vengono restituite le colonne di tale tabella.  
  
 [ @table_qualifier =]'*TABLE_QUALIFIER*'  
 Nome del qualificatore di tabella. *TABLE_QUALIFIER* è di *tipo sysname* e il valore predefinito è null. Vari prodotti DBMS supportano la denominazione in tre parti per le tabelle (_qualificatore_**.** _proprietario_**.** _nome_). In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa colonna rappresenta il nome del database. In altri prodotti rappresenta il nome del server dell'ambiente di database della tabella.  
  
 [ @column_name =]'*colonna*'  
 Colonna utilizzata quando si desidera ottenere una sola colonna di informazioni del catalogo. *Column* è di **tipo nvarchar (** 384 **)** e il valore predefinito è null. Se la *colonna* non è specificata, vengono restituite tutte le colonne. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la *colonna* rappresenta il nome della colonna elencato nella tabella sys. Columns. la *colonna* può includere caratteri jolly usando criteri di corrispondenza con caratteri jolly del sistema DBMS sottostante. Per ottenere la massima interoperabilità, è consigliabile che nel client del gateway vengano utilizzati solo i caratteri jolly dello standard ISO, ovvero i caratteri % e _.  
  
## <a name="result-sets"></a>Set di risultati  
 sp_column_privileges è equivalente a SQLColumnPrivileges in ODBC. I risultati restituiti sono ordinati per TABLE_QUALIFIER, TABLE_OWNER, TABLE_NAME, COLUMN_NAME e PRIVILEGE.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|TABLE_QUALIFIER|**sysname**|Nome del qualificatore della tabella. Questo campo può essere NULL.|  
|TABLE_OWNER|**sysname**|Nome del proprietario della tabella. Questo campo restituisce sempre un valore.|  
|TABLE_NAME|**sysname**|Nome della tabella. Questo campo restituisce sempre un valore.|  
|COLUMN_NAME|**sysname**|Nome di ogni colonna della tabella TABLE_NAME restituita. Questo campo restituisce sempre un valore.|  
|GRANTOR|**sysname**|Nome dell'utente del database che ha concesso le autorizzazioni per la colonna COLUMN_NAME all'utente GRANTEE specificato. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa colonna corrisponde sempre a TABLE_OWNER. Questo campo restituisce sempre un valore.<br /><br /> La colonna GRANTOR può rappresentare il proprietario del database (TABLE_OWNER) o un utente a cui il proprietario del database ha concesso le autorizzazioni tramite la clausola WITH GRANT OPTION dell'istruzione GRANT.|  
|GRANTEE|**sysname**|Nome dell'utente del database a cui l'utente GRANTOR specificato ha concesso le autorizzazioni per la colonna COLUMN_NAME. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa colonna include sempre un utente di database della tabella sysusers. Questo campo restituisce sempre un valore.|  
|PRIVILEGE|**varchar (** 32 **)**|Una delle autorizzazioni di colonna disponibili. Le autorizzazioni di colonna possono essere rappresentate da uno dei valori riportati di seguito o da altri valori supportati dall'origine dei dati in fase di definizione dell'implementazione:<br /><br /> SELECT = l'utente GRANTEE può recuperare dati per le colonne.<br /><br /> INSERT = l'utente GRANTEE può fornire dati per la colonna specificata quando inserisce nuove righe nella tabella.<br /><br /> UPDATE = l'utente GRANTEE può modificare i dati della colonna.<br /><br /> REFERENCES = l'utente GRANTEE può fare riferimento a una colonna di una tabella esterna in una relazione chiave primaria/chiave esterna. Questo tipo di relazione viene definito tramite vincoli di tabella.|  
|IS_GRANTABLE|**varchar (** 3 **)**|Indica se l'utente GRANTEE può concedere autorizzazioni ad altri utenti (autorizzazione per la concessione di autorizzazioni). I possibili valori sono YES, NO e NULL. Un valore sconosciuto, o NULL, fa riferimento a un'origine dei dati per la quale questo tipo di assegnazione indiretta delle autorizzazioni non è consentito.|  
  
## <a name="remarks"></a>Commenti  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le autorizzazioni vengono concesse tramite l'istruzione GRANT e rimosse tramite l'istruzione REVOKE.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione SELECT per lo schema.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite informazioni sui privilegi di una colonna specifica.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_column_privileges @table_name = 'Employee'   
    ,@table_owner = 'HumanResources'  
    ,@table_qualifier = 'AdventureWorks2012'  
    ,@column_name = 'SalariedFlag';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
