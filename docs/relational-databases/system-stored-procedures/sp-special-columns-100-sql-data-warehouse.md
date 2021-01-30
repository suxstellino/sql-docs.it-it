---
description: sp_special_columns_100 (analisi delle sinapsi di Azure)
title: sp_special_columns_100 (analisi delle sinapsi di Azure)
ms.custom: ''
ms.date: 03/14/2017
ms.prod_service: sql-data-warehouse, pdw
ms.service: sql-data-warehouse
ms.subservice: design
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 5774fadc-77cc-46f8-8f9f-a0f9efe95e21
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 26aef826cfe6b35d6428240296b31dbc8a99474c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189271"
---
# <a name="sp_special_columns_100-azure-synapse-analytics"></a>sp_special_columns_100 (analisi delle sinapsi di Azure)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Restituisce il set di colonne ottimale per l'identificazione univoca di una riga nella tabella. Restituisce inoltre le colonne che vengono aggiornate automaticamente quando qualsiasi valore della riga viene aggiornato tramite una transazione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_special_columns_100 [ @table_name = ] 'table_name'     
     [ , [ @table_owner = ] 'table_owner' ]   
     [ , [ @qualifier = ] 'qualifier' ]   
     [ , [ @col_type = ] 'col_type' ]   
     [ , [ @scope = ] 'scope' ]  
     [ , [ @nullable = ] 'nullable' ]   
     [ , [ @ODBCVer = ] 'ODBCVer' ]   
[ ; ]  
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>Argomenti  
 [ @table_name =]'*table_name*'  
 Nome della tabella utilizzata per restituire informazioni sul catalogo. *Name* è di **tipo sysname** e non prevede alcun valore predefinito. I criteri di ricerca con caratteri jolly non sono supportati.  
  
 [ @table_owner =]'*TABLE_OWNER*'  
 Proprietario della tabella utilizzata per restituire informazioni sul catalogo. *owner* è di **tipo sysname** e il valore predefinito è null. I criteri di ricerca con caratteri jolly non sono supportati. Se il *proprietario* non è specificato, vengono applicate le regole di visibilità della tabella predefinite del sistema DBMS sottostante.  
  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se l'utente corrente è il proprietario di una tabella con il nome specificato, vengono restituite le colonne di tale tabella. Se *owner* non è specificato e l'utente corrente non è il proprietario di una tabella con il *nome* specificato, questa procedura cerca una tabella con il *nome* specificato di proprietà del proprietario del database. Se la tabella esiste, vengono restituite le colonne corrispondenti.  
  
 [ @qualifier =]'*qualificatore*'  
 Nome del qualificatore di tabella. *Qualifier* è di **tipo sysname** e il valore predefinito è null. Vari prodotti DBMS supportano la denominazione in tre parti per le tabelle (*Qualifier.Owner.Name*). In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa colonna rappresenta il nome del database. In alcuni prodotti rappresenta il nome del server dell'ambiente di database della tabella.  
  
 [ @col_type =]'*col_type*'  
 Tipo di colonna. *col_type* è di tipo **char (** 1 **)** e il valore predefinito è r. il tipo r restituisce la colonna o il set di colonne ottimale che, tramite il recupero di valori dalla colonna o dalle colonne, consente di identificare in modo univoco ogni riga nella tabella specificata. Una colonna può essere una pseudocolonna progettata a questo scopo oppure la colonna o le colonne di un indice univoco della tabella. Il tipo di colonna V restituisce le eventuali colonne della tabella specificata che vengono aggiornate automaticamente dall'origine dati in corrispondenza dell'aggiornamento di un valore della riga tramite una transazione.  
  
 [ @scope =]'*scope*'  
 Ambito minimo richiesto per ROWID. l' *ambito* è **char (** 1 **)** e il valore predefinito è T. Scope C specifica che ROWID è valido solo se posizionato in corrispondenza di tale riga. L'ambito T indica che il valore ROWID è valido per la transazione.  
  
 [ @nullable =]'*Nullable*'  
 Indica se le colonne speciali possono accettare un valore null. *Nullable* è **char (** 1 **)** e il valore predefinito è U. O specifica le colonne speciali che non ammettono valori null. mentre U specifica le colonne che ammettono parzialmente valori Null.  
  
 [ @ODBCVer =]'*ODBCVer*'  
 Versione ODBC utilizzata. *ODBCVer* è di **tipo int (** 4 **)** e il valore predefinito è 2. che indica ODBC versione 2.0. Per ulteriori informazioni sulle differenze tra ODBC versione 2.0 e ODBC versione 3.0, vedere la specifica ODBC SQLSpecialColumns per ODBC versione 3.0.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nessuno  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|SCOPE|**smallint**|Ambito effettivo dell'ID della riga. Il Può essere 0, 1 o 2. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce sempre 0. Questo campo restituisce sempre un valore.<br /><br /> 0 = SQL_SCOPE_CURROW. La validità dell'ID di riga è garantita solo mentre si è posizionati in tale riga. Una selezione successiva in base all'ID di riga potrebbe non restituire la riga se questa è stata aggiornata o eliminata da un'altra transazione.<br /><br /> 1 = SQL_SCOPE_TRANSACTION. La validità dell'ID di riga è garantita per l'intera durata della transazione corrente.<br /><br /> 2 = SQL_SCOPE_SESSION. La validità dell'ID della riga è garantita per l'intera durata della sessione (oltre i limiti delle transazioni).|  
|COLUMN_NAME|**sysname**|Nome della colonna per ogni colonna della *tabella* restituita. Questo campo restituisce sempre un valore.|  
|DATA_TYPE|**smallint**|Tipo di dati SQL ODBC.|  
|TYPE_NAME|**sysname**|Nome del tipo di dati dipendente dall'origine dati; ad esempio, **char**, **varchar**, **Money** o **Text**.|  
|PRECISION|**Int**|Precisione della colonna nell'origine dati. Questo campo restituisce sempre un valore.|  
|LENGTH|**Int**|Lunghezza, in byte, necessaria per il tipo di dati nel relativo formato binario nell'origine dati, ad esempio 10 per **char (** 10 **)**, 4 per **Integer** e 2 per **smallint**.|  
|SCALE|**smallint**|Scala della colonna nell'origine dati. Per i tipi di dati in cui la scala non è applicabile viene restituito NULL.|  
|PSEUDO_COLUMN|**smallint**|Indica se la colonna è una pseudocolonna. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce sempre 1:<br /><br /> 0 = SQL_PC_UNKNOWN<br /><br /> 1 = SQL_PC_NOT_PSEUDO<br /><br /> 2 = SQL_PC_PSEUDO|  
  
## <a name="remarks"></a>Commenti  
 sp_special_columns corrisponde a SQLSpecialColumns in ODBC. I risultati restituiti vengono ordinati in base a SCOPE.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione SELECT per lo schema.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Nell'esempio seguente vengono restituite informazioni sulla colonna che identifica in modo univoco le righe nella tabella `FactFinance`.  
  
```sql  
-- Uses AdventureWorks  
  
EXEC sp_special_columns_100 @table_name = 'FactFinance';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di analisi delle sinapsi di Azure](../../relational-databases/system-stored-procedures/sql-data-warehouse-stored-procedures.md)  
  
  

