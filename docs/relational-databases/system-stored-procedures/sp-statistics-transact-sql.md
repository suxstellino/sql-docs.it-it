---
description: sp_statistics (Transact-SQL)
title: sp_statistics (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_statistics_TSQL
- sp_statistics
dev_langs:
- TSQL
helpviewer_keywords:
- sp_statistics
ms.assetid: 0bb6495f-258a-47ec-9f74-fd16671d23b8
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f9584cf09ef7602b6b821d51230bc72d556e0169
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752471"
---
# <a name="sp_statistics-transact-sql"></a>sp_statistics (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce un elenco di tutti gli indici e le statistiche per la tabella o vista indicizzata specificata.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_statistics [ @table_name = ] 'table_name'    
     [ , [ @table_owner = ] 'owner' ]   
     [ , [ @table_qualifier = ] 'qualifier' ]   
     [ , [ @index_name = ] 'index_name' ]   
     [ , [ @is_unique = ] 'is_unique' ]  
     [ , [ @accuracy = ] 'accuracy' ]  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Argomenti  
`[ @table_name = ] 'table_name'` Specifica la tabella utilizzata per restituire le informazioni del catalogo. *table_name* è di **tipo sysname** e non prevede alcun valore predefinito. I criteri di ricerca con caratteri jolly non sono supportati.  
  
`[ @table_owner = ] 'owner'` Nome del proprietario della tabella utilizzata per restituire le informazioni del catalogo. *TABLE_OWNER* è di **tipo sysname** e il valore predefinito è null. I criteri di ricerca con caratteri jolly non sono supportati. Se il *proprietario* non è specificato, vengono applicate le regole di visibilità della tabella predefinite del sistema DBMS sottostante.  
  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se l'utente corrente è il proprietario di una tabella con il nome specificato, vengono restituiti gli indici di tale tabella. Se *owner* non è specificato e l'utente corrente non è il proprietario di una tabella con il *nome* specificato, questa procedura cerca una tabella con il *nome* specificato di proprietà del proprietario del database. Se tale tabella esiste, vengono restituiti gli indici corrispondenti.  
  
`[ @table_qualifier = ] 'qualifier'` Nome del qualificatore di tabella. *Qualifier* è di **tipo sysname** e il valore predefinito è null. Vari prodotti DBMS supportano la denominazione in tre parti per le tabelle (_qualificatore_**.** _proprietario_**.** _nome_). In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questo parametro rappresenta il nome del database. In altri prodotti rappresenta il nome del server dell'ambiente di database della tabella.  
  
`[ @index_name = ] 'index_name'` Nome dell'indice. *index_name* è di **tipo sysname** e il valore predefinito è%. La ricerca con caratteri jolly è supportata.  
  
`[ @is_unique = ] 'is_unique'` Indica se devono essere restituiti solo indici univoci (se **Y**). *is_unique* è di **carattere (1)** e il valore predefinito è **N**.  
  
`[ @accuracy = ] 'accuracy'` È il livello di precisione della cardinalità e della pagina per le statistiche. l' *accuratezza* è **char (1)** e il valore predefinito è **Q**. Specificare **e** per assicurarsi che le statistiche vengano aggiornate in modo che la cardinalità e le pagine siano accurate.  
  
 Il valore **E** (SQL_ENSURE) chiede al driver di recuperare le statistiche in modo incondizionato.  
  
 Il valore **Q** (SQL_QUICK) chiede al driver di recuperare la cardinalità e le pagine solo se sono prontamente disponibili dal server. In tal caso, il driver non garantisce che i valori siano aggiornati. Per le applicazioni scritte in base allo standard Open Group viene restituito sempre il comportamento di SQL_QUICK da driver conformi a ODBC 3.x.  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**TABLE_QUALIFIER**|**sysname**|Nome del qualificatore della tabella. Questa colonna può essere NULL.|  
|**TABLE_OWNER**|**sysname**|Nome del proprietario della tabella. In questa colonna viene sempre restituito un valore.|  
|**TABLE_NAME**|**sysname**|Nome della tabella. In questa colonna viene sempre restituito un valore.|  
|**NON_UNIQUE**|**smallint**|NOT NULL.<br /><br /> 0 = Univoco<br /><br /> 1 = Non univoco|  
|**INDEX_QUALIFIER**|**sysname**|Nome del proprietario dell'indice. In alcuni prodotti DBMS gli indici possono essere creati da utenti diversi dal proprietario della tabella. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa colonna è sempre uguale a **table_name**.|  
|**INDEX_NAME**|**sysname**|Nome dell'indice. In questa colonna viene sempre restituito un valore.|  
|**TYPE**|**smallint**|Questa colonna restituisce sempre un valore:<br /><br /> 0 = Statistiche di una tabella<br /><br /> 1 = Cluster<br /><br /> 2 = Hash<br /><br /> 3 = non cluster|  
|**SEQ_IN_INDEX**|**smallint**|Posizione della colonna all'interno dell'indice.|  
|**COLUMN_NAME**|**sysname**|Nome della colonna per ogni colonna dell' **table_name** restituito. In questa colonna viene sempre restituito un valore.|  
|**COLLATION**|**char(1)**|Ordine utilizzato nelle regole di confronto. I possibili valori sono i seguenti:<br /><br /> A = Crescente<br /><br /> D = Decrescente<br /><br /> NULL = Non applicabile|  
|**CARDINALITÀ**|**int**|Numero di righe nella tabella o di valori univoci nell'indice.|  
|**PAGINE**|**int**|Numero di pagine in cui archiviare l'indice o la tabella.|  
|**FILTER_CONDITION**|**varchar(128)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non restituisce un valore.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 Gli indici nel set di risultati vengono visualizzati in ordine crescente in base alle colonne **NON_UNIQUE**, **tipo**, **index_name** e **SEQ_IN_INDEX**.  
  
 Gli indici di tipo cluster sono quelli in cui i dati della tabella sono archiviati nell'ordine dell'indice, Corrisponde agli [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] indici cluster.  
  
 Il tipo di indice Hash accetta ricerche di corrispondenze esatte o ricerche basate su intervalli, ma non viene utilizzato per ricerche con criteri.  
  
 **sp_statistics** equivale a **SQLStatistics** in ODBC. I risultati restituiti vengono ordinati in base **NON_UNIQUE**, **Type**, **INDEX_QUALIFIER**, **index_name** e **SEQ_IN_INDEX**. Per ulteriori informazioni, vedere le informazioni di [riferimento sulle API ODBC](../../odbc/reference/syntax/odbc-reference.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione SELECT per lo schema.  
  
## <a name="example-sssdwfull-and-sspdw"></a>Esempio: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Nell'esempio seguente vengono restituite informazioni sulla `DimEmployee` tabella.  
  
```sql  
-- Uses AdventureWorks  
  
EXEC sp_statistics DimEmployee;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure del catalogo &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
