---
description: sp_stored_procedures (Transact-SQL)
title: sp_stored_procedures (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_stored_procedures_TSQL
- sp_stored_procedures
dev_langs:
- TSQL
helpviewer_keywords:
- sp_stored_procedures
ms.assetid: fe52dd83-000a-4665-83fb-7a0024193dec
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3266b5619701d28a4fc65eb79d3f3e16c75e40b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193215"
---
# <a name="sp_stored_procedures-transact-sql"></a>sp_stored_procedures (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Restituisce un elenco delle stored procedure dell'ambiente corrente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_stored_procedures [ [ @sp_name = ] 'name' ]   
    [ , [ @sp_owner = ] 'schema']   
    [ , [ @sp_qualifier = ] 'qualifier' ]  
    [ , [@fUsePattern = ] 'fUsePattern' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @sp_name = ] 'name'` Nome della procedura utilizzata per restituire le informazioni del catalogo. *Name* è di **tipo nvarchar (390)** e il valore predefinito è null. La ricerca con caratteri jolly è supportata.  
  
`[ @sp_owner = ] 'schema'` Nome dello schema a cui appartiene la stored procedure. *schema* è di **tipo nvarchar (384)** e il valore predefinito è null. La ricerca con caratteri jolly è supportata. Se il *proprietario* non è specificato, vengono applicate le regole di visibilità predefinite della procedura del sistema DBMS sottostante.  
  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se lo schema corrente contiene una procedura con il nome specificato, viene restituita tale procedura. Se è specificata una stored procedure non qualificata, [!INCLUDE[ssDE](../../includes/ssde-md.md)] cerca la procedura nell'ordine seguente:  
  
-   Schema **sys** del database corrente.  
  
-   Schema predefinito del chiamante, se eseguito in batch o in un'istruzione SQL dinamica. In alternativa, se il nome della procedura non qualificata si trova nel corpo di un'altra definizione di procedura, la ricerca viene quindi eseguita all'interno dello schema contenente quest'ultima procedura.  
  
-   Schema **dbo** nel database corrente.  
  
`[ @qualifier = ] 'qualifier'` Nome del qualificatore della procedura. *Qualifier* è di **tipo sysname** e il valore predefinito è null. Vari prodotti DBMS supportano la denominazione in tre parti per le tabelle nel formato (_qualificatore_**.** _schema_**.** _nome_. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , *Qualifier* rappresenta il nome del database. In alcuni prodotti rappresenta il nome del server dell'ambiente di database della tabella.  
  
`[ @fUsePattern = ] 'fUsePattern'` Determina se il carattere di sottolineatura (_), la percentuale (%) o le parentesi quadre []) vengono interpretati come caratteri jolly. *fUsePattern* è di **bit** e il valore predefinito è 1.  
  
 **0** = la corrispondenza di criteri è disattivata.  
  
 **1** = i criteri di ricerca sono on.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nessuno  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**PROCEDURE_QUALIFIER**|**sysname**|Nome di qualificatore della procedura. Questa colonna può essere NULL.|  
|**PROCEDURE_OWNER**|**sysname**|Nome del proprietario della procedura. In questa colonna viene sempre restituito un valore.|  
|**PROCEDURE_NAME**|**nvarchar (134)**|Nome della procedura. In questa colonna viene sempre restituito un valore.|  
|**NUM_INPUT_PARAMS**|**int**|Riservato per utilizzi futuri.|  
|**NUM_OUTPUT_PARAMS**|**int**|Riservato per utilizzi futuri.|  
|**NUM_RESULT_SETS**|**int**|Riservato per utilizzi futuri.|  
|**COMMENTI**|**varchar (254)**|Descrizione della procedura. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non viene restituito alcun valore per questa colonna.|  
|**PROCEDURE_TYPE**|**smallint**|Tipo di procedura. Tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene restituito sempre 2.0. I valori validi sono i seguenti:<br /><br /> 0 = SQL_PT_UNKNOWN<br /><br /> 1 = SQL_PT_PROCEDURE<br /><br /> 2 = SQL_PT_FUNCTION|  
  
## <a name="remarks"></a>Commenti  
 Per garantire la massima interoperabilità, con il client del gateway è consigliabile utilizzare solo i caratteri jolly standard SQL, ovvero il segno di percentuale (%) e il carattere di sottolineatura (_).  
  
 Le informazioni sulle autorizzazioni per l'accesso in esecuzione a una stored procedure specifica non vengono necessariamente verificate. Pertanto, l'accesso non è garantito. Si noti che viene utilizzata solo la denominazione in tre parti. Pertanto, con l'esecuzione in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono restituite solo le stored procedure locali e non quelle remote, che richiedono una denominazione in quattro parti. Se l'attributo server ACCESSIBLE_SPROC è Y nel set di risultati per **sp_server_info**, vengono restituite solo le stored procedure che possono essere eseguite dall'utente corrente.  
  
 **sp_stored_procedures** equivale a **SQLProcedures** in ODBC. I risultati restituiti vengono ordinati in base **PROCEDURE_QUALIFIER**, **PROCEDURE_OWNER** e **procedure_name**.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione SELECT per lo schema.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-returning-all-stored-procedures-in-the-current-database"></a>R. Restituzione di tutte le stored procedure nel database corrente  
 Nell'esempio seguente vengono restituite tutte le stored procedure nel database [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_stored_procedures;  
```  
  
### <a name="b-returning-a-single-stored-procedure"></a>B. Restituzione di una singola stored procedure  
 Nell'esempio seguente viene restituito un set di risultati per la stored procedure `uspLogError`.  
  
```  
USE AdventureWorks2012;  
GO  
sp_stored_procedures N'uspLogError', N'dbo', N'AdventureWorks2012', 1;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure del catalogo &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
