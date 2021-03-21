---
description: sp_sproc_columns (Transact-SQL)
title: sp_sproc_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_sproc_columns
- sp_sproc_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_sproc_columns
ms.assetid: 62c18c21-35c5-4772-be0d-ffdcc19c97ab
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 66af1e613e5b02eee96357da383a0b535424ebf5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104745031"
---
# <a name="sp_sproc_columns-transact-sql"></a>sp_sproc_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce informazioni relative alle colonne per una sola stored procedure o funzione definita dall'utente nell'ambiente corrente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_sproc_columns [[@procedure_name = ] 'name']   
    [ , [@procedure_owner = ] 'owner']   
    [ , [@procedure_qualifier = ] 'qualifier']   
    [ , [@column_name = ] 'column_name']  
    [ , [@ODBCVer = ] 'ODBCVer']  
    [ , [@fUsePattern = ] 'fUsePattern']  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @procedure_name = ] 'name'` Nome della procedura utilizzata per restituire le informazioni del catalogo. *Name* è di **tipo nvarchar (** 390 **)** e il valore predefinito è%, che indica tutte le tabelle del database corrente. La ricerca con caratteri jolly è supportata.  
  
`[ @procedure_owner = ] 'owner'` Nome del proprietario della stored procedure. *owner* è di **tipo nvarchar (** 384 **)** e il valore predefinito è null. La ricerca con caratteri jolly è supportata. Se il *proprietario* non è specificato, vengono applicate le regole di visibilità predefinite della procedura del sistema DBMS sottostante.  
  
 Se l'utente corrente è il proprietario di una procedura avente il nome specificato, vengono restituite informazioni su tale procedura. Se il *proprietario* non è specificato e l'utente corrente non è il proprietario di una routine con il nome specificato, **sp_sproc_columns** ricerca una procedura con il nome specificato di proprietà del proprietario del database. Se tale procedura viene individuata, vengono restituite informazioni sulle colonne corrispondenti.  
  
`[ @procedure_qualifier = ] 'qualifier'` Nome del qualificatore della procedura. *Qualifier* è di **tipo sysname** e il valore predefinito è null. Vari prodotti DBMS supportano la denominazione in tre parti per le tabelle (*Qualifier.Owner.Name*). In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questo parametro rappresenta il nome del database. In altri prodotti rappresenta il nome del server dell'ambiente di database della tabella.  
  
`[ @column_name = ] 'column_name'` È una singola colonna e viene utilizzata quando si desidera una sola colonna di informazioni di catalogo. *column_name* è di **tipo nvarchar (** 384 **)** e il valore predefinito è null. Se *column_name* viene omesso, vengono restituite tutte le colonne. La ricerca con caratteri jolly è supportata. Per ottenere la massima interoperabilità, è consigliabile che nel client del gateway vengano utilizzati solo i caratteri jolly dello standard ISO, ovvero i caratteri % e _.  
  
`[ @ODBCVer = ] 'ODBCVer'` Versione di ODBC in uso. *ODBCVer* è di **tipo int** e il valore predefinito è 2, che indica ODBC versione 2,0. Per ulteriori informazioni sulla differenza tra ODBC versione 2,0 e ODBC versione 3,0, vedere la specifica ODBC **SQLProcedureColumns** per odbc versione 3,0  
  
`[ @fUsePattern = ] 'fUsePattern'` Determina se i caratteri di sottolineatura (_), percentuale (%) e parentesi quadra ([]) vengono interpretati come caratteri jolly. I valori validi sono 0 (utilizzo dei criteri di ricerca disattivato) e 1 (utilizzo dei criteri di ricerca attivato). *fUsePattern* è di **bit** e il valore predefinito è 1.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nessuno  
  
## <a name="result-sets"></a>Set di risultati  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**PROCEDURE_QUALIFIER**|**sysname**|Nome di qualificatore della procedura. Questa colonna può essere NULL.|  
|**PROCEDURE_OWNER**|**sysname**|Nome del proprietario della procedura. In questa colonna viene sempre restituito un valore.|  
|**PROCEDURE_NAME**|**nvarchar (** 134 **)**|Nome della procedura. In questa colonna viene sempre restituito un valore.|  
|**COLUMN_NAME**|**sysname**|Nome della colonna per ogni colonna dell' **table_name** restituito. In questa colonna viene sempre restituito un valore.|  
|**COLUMN_TYPE**|**smallint**|In questo campo viene sempre restituito un valore.<br /><br /> 0 = SQL_PARAM_TYPE_UNKNOWN<br /><br /> 1 = SQL_PARAM_TYPE_INPUT<br /><br /> 2 = SQL_PARAM_TYPE_OUTPUT<br /><br /> 3 = SQL_RESULT_COL<br /><br /> 4 = SQL_PARAM_OUTPUT<br /><br /> 5 = SQL_RETURN_VALUE|  
|**DATA_TYPE**|**smallint**|Codice integer di un tipo di dati ODBC. Se non è possibile effettuare il mapping di questo tipo di dati a un tipo ISO, il valore è NULL. Il nome del tipo di dati nativo viene restituito nella colonna **type_name** .|  
|**TYPE_NAME**|**sysname**|Rappresentazione in forma di stringa del tipo di dati. Corrisponde al nome del tipo di dati visualizzato dal sistema DBMS sottostante.|  
|**PRECISION**|**int**|Numero di cifre significative. Il valore restituito per la colonna **Precision** è in base 10.|  
|**LENGTH**|**int**|Dimensioni di trasferimento dei dati.|  
|**SCALA**|**smallint**|Numero di cifre a destra del separatore decimale.|  
|**RADIX**|**smallint**|Base per i tipi di dati numerici.|  
|**NULLABLE**|**smallint**|Specifica se i valori Null sono supportati o meno:<br /><br /> 1 = È possibile creare il tipo di dati con supporto per valori Null.<br /><br /> 0 = I valori Null non sono supportati.|  
|**COMMENTI**|**varchar (** 254 **)**|Descrizione della colonna della procedura. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non viene restituito alcun valore per questa colonna.|  
|**COLUMN_DEF**|**nvarchar (** 4000 **)**|Valore predefinito della colonna.|  
|**SQL_DATA_TYPE**|**smallint**|Valore del tipo di dati SQL visualizzato nel campo **Type** del descrittore. Questa colonna corrisponde alla colonna **DATA_TYPE**, tranne che per i tipi di dati **datetime** e ISO **interval**. In questa colonna viene sempre restituito un valore.|  
|**SQL_DATETIME_SUB**|**smallint**|Sottocodice **datetime** ISO **interval** se il valore di **SQL_DATA_TYPE** è **SQL_DATETIME** o **SQL_INTERVAL**. Per i tipi di dati diversi da **DateTime** e ISO **Interval**, questo campo è null.|  
|**CHAR_OCTET_LENGTH**|**int**|Lunghezza massima in byte di una colonna con tipo di dati **character** o **Binary** . Per gli tutti gli altri tipi di dati, il valore di questa colonna è NULL.|  
|**ORDINAL_POSITION**|**int**|Posizione ordinale della colonna nella tabella. La prima colonna nella tabella è 1. In questa colonna viene sempre restituito un valore.|  
|**IS_NULLABLE**|**varchar (254)**|Impostazione relativa al supporto di valori Null nella colonna della tabella. Per determinare il supporto di valori Null vengono seguite le regole ISO. In un sistema DBMS conforme a ISO non vengono restituite stringhe vuote.<br /><br /> Se la colonna ammette valori Null, viene visualizzato YES. In caso contrario viene visualizzato NO.<br /><br /> Quando non è noto se i valori Null sono supportati, in questa colonna viene restituita una stringa di lunghezza zero.<br /><br /> Il valore restituito per questa colonna è diverso dal valore restituito per la colonna NULLABLE.|  
|**SS_DATA_TYPE**|**tinyint**|Tipo di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzato dalle stored procedure estese. Per altre informazioni, vedere [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).|  
  
## <a name="remarks"></a>Commenti  
 **sp_sproc_columns** è equivalente a **SQLProcedureColumns** in ODBC. I risultati restituiti vengono ordinati in base **PROCEDURE_QUALIFIER**, **PROCEDURE_OWNER**, **procedure_name** e l'ordine in cui i parametri vengono visualizzati nella definizione della procedura.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione SELECT per lo schema.  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure del catalogo &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
