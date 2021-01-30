---
description: sp_tables (Transact-SQL)
title: sp_tables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_tables
- sp_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_tables
ms.assetid: 787a2fa5-87a1-49bd-938b-6043c245f46b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a176073f8951cd15acd34aa00b92fc268f90c27e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180579"
---
# <a name="sp_tables-transact-sql"></a>sp_tables (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Viene restituito un elenco di oggetti su cui è possibile eseguire una query nell'ambiente corrente, ovvero qualsiasi tabella o vista eccetto gli oggetti sinonimo.  
  
> [!NOTE]  
>  Per determinare il nome dell'oggetto di base di un sinonimo, eseguire una query sulla vista del catalogo [sys. sinonims](../../relational-databases/system-catalog-views/sys-synonyms-transact-sql.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_tables [ [ @table_name = ] 'name' ]   
     [ , [ @table_owner = ] 'owner' ]   
     [ , [ @table_qualifier = ] 'qualifier' ]   
     [ , [ @table_type = ] "type" ]   
     [ , [@fUsePattern = ] 'fUsePattern'];  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @table_name = ] 'name'` Tabella utilizzata per restituire le informazioni del catalogo. *Name* è di **tipo nvarchar (384)** e il valore predefinito è null. La ricerca con caratteri jolly è supportata.  
  
`[ @table_owner = ] 'owner'` Proprietario della tabella utilizzata per restituire le informazioni del catalogo. *owner* è di **tipo nvarchar (384)** e il valore predefinito è null. La ricerca con caratteri jolly è supportata. Se owner viene omesso, vengono applicate le regole di visibilità della tabella predefinite nel sistema DBMS sottostante.  
  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se l'utente corrente è il proprietario di una tabella con il nome specificato, vengono restituite le colonne di tale tabella. Se owner viene omesso e l'utente corrente non è il proprietario di una tabella avente il nome specificato, viene eseguita la ricerca di una tabella avente il nome specificato e il cui proprietario corrisponde al proprietario del database. Se viene individuata, vengono restituite le colonne di tale tabella.  
  
`[ @table_qualifier = ] 'qualifier'` Nome del qualificatore di tabella. *Qualifier* è di **tipo sysname** e il valore predefinito è null. Vari prodotti DBMS supportano la denominazione in tre parti per le tabelle (_qualificatore_**.** _proprietario_**.** _nome_). In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa colonna rappresenta il nome del database. In altri prodotti rappresenta il nome del server dell'ambiente di database della tabella.  
  
``[ , [ @table_type = ] "'type', 'type'" ]`` Elenco di valori separati da virgole, che fornisce informazioni su tutte le tabelle dei tipi di tabella specificati. Sono incluse **Table**, **SYSTEMTABLE** e **View**. il *tipo* è **varchar (100)** e il valore predefinito è null.  
  
> [!NOTE]  
>  È necessario racchiudere ogni tipo di tabella tra virgolette singole e l'intero parametro tra virgolette doppie. I tipi di tabella devono essere specificati in maiuscolo. Se l'opzione SET QUOTED_IDENTIFIER è impostata su ON, è necessario sostituire le virgolette singole con quelle doppie e racchiudere l'intero parametro tra virgolette singole.  
  
`[ @fUsePattern = ] 'fUsePattern'` Determina se i caratteri di sottolineatura (_), percentuale (%) e parentesi quadra ([o]) vengono interpretati come caratteri jolly. I valori validi sono 0 (utilizzo dei criteri di ricerca disattivato) e 1 (utilizzo dei criteri di ricerca attivato). *fUsePattern* è di **bit** e il valore predefinito è 1.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nessuno  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**TABLE_QUALIFIER**|**sysname**|Nome del qualificatore della tabella. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa colonna rappresenta il nome del database. Questo campo può essere NULL.|  
|**TABLE_OWNER**|**sysname**|Nome del proprietario della tabella. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa colonna rappresenta il nome dell'utente del database che ha creato la tabella. Questo campo restituisce sempre un valore.|  
|**TABLE_NAME**|**sysname**|Nome della tabella. Questo campo restituisce sempre un valore.|  
|**TABLE_TYPE**|**varchar(32)**|Tabella, tabella di sistema o vista.|  
|**COMMENTI**|**varchar (254)**|In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non viene restituito alcun valore per questa colonna.|  
  
## <a name="remarks"></a>Commenti  
 Per ottenere la massima interoperabilità, è consigliabile che nel client del gateway siano utilizzati solo i caratteri jolly standard SQL-92, ovvero i caratteri % e _.  
  
 Le informazioni sui privilegi relativi all'accesso in lettura o scrittura dell'utente corrente per una tabella specifica non vengono necessariamente verificate e di conseguenza l'accesso non è garantito. Questo set di risultati include non solo tabelle e viste, ma anche sinonimi e alias di gateway dei prodotti DBMS che supportano questi tipi. Se l'attributo server **ACCESSIBLE_TABLES** è Y nel set di risultati per **sp_server_info**, vengono restituite solo le tabelle a cui è possibile accedere dall'utente corrente.  
  
 **sp_tables** equivale a **SQLTables** in ODBC. I risultati restituiti vengono ordinati in base **TABLE_TYPE**, **TABLE_QUALIFIER**, **TABLE_OWNER** e **table_name**.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione SELECT per lo schema.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-a-list-of-objects-that-can-be-queried-in-the-current-environment"></a>R. Restituzione di un elenco di oggetti su cui è possibile eseguire query nell'ambiente corrente  
 Nell'esempio seguente viene restituito un elenco di oggetti su cui è possibile eseguire una query nell'ambiente corrente.  
  
```sql  
EXEC sp_tables ;  
```  
  
### <a name="b-returning-information-about-the-tables-in-a-specified-schema"></a>B. Restituzione di informazioni sulle tabelle in uno schema specificato  
 Nell'esempio seguente vengono restituite informazioni sulle tabelle appartenenti allo schema `Person` nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_tables   
   @table_name = '%',  
   @table_owner = 'Person',  
   @table_qualifier = 'AdventureWorks2012';  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-returning-a-list-of-objects-that-can-be-queried-in-the-current-environment"></a>C. Restituzione di un elenco di oggetti su cui è possibile eseguire query nell'ambiente corrente  
 Nell'esempio seguente viene restituito un elenco di oggetti su cui è possibile eseguire una query nell'ambiente corrente.  
  
```sql  
EXEC sp_tables ;  
```  
  
### <a name="d-returning-information-about-the-tables-in-a-specified-schema"></a>D. Restituzione di informazioni sulle tabelle in uno schema specificato  
 Nell'esempio seguente vengono restituite informazioni sulle tabelle delle dimensioni nel `AdventureWorksPDW201` database.  
  
```sql  
-- Uses AdventureWorks  
  
EXEC sp_tables   
   @table_name = 'Dim%',  
   @table_owner = 'dbo',  
   @table_qualifier = 'AdventureWorksPDW2012';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys. sinonimi &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-synonyms-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

