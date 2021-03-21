---
description: sp_columns (Transact-SQL)
title: sp_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/17/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_columns_TSQL
- sp_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sp_columns
ms.assetid: 2dec79cf-2baf-4c0f-8cbb-afb1a8654e1e
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7ef2d20ec911fc361ebbb7162853ee2e2cfe748c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750241"
---
# <a name="sp_columns-transact-sql"></a>sp_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Vengono restituite le informazioni di colonna per gli oggetti specificati per cui è possibile eseguire una query nell'ambiente corrente.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_columns [ @table_name = ] object  
     [ , [ @table_owner = ] owner ]   
     [ , [ @table_qualifier = ] qualifier ]   
     [ , [ @column_name = ] column ]   
     [ , [ @ODBCVer = ] ODBCVer ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ \@table_name = ] object` Nome dell'oggetto utilizzato per restituire le informazioni del catalogo. l' *oggetto* può essere una tabella, una vista o un altro oggetto con colonne quali funzioni con valori di tabella. l' *oggetto* è di **tipo nvarchar (384)** e non prevede alcun valore predefinito. La ricerca con caratteri jolly è supportata.  
  
`[ \@table_owner = ] owner` Proprietario dell'oggetto utilizzato per restituire le informazioni del catalogo. *owner* è di **tipo nvarchar (384)** e il valore predefinito è null. La ricerca con caratteri jolly è supportata. Se il *proprietario* non è specificato, vengono applicate le regole di visibilità degli oggetti predefinite del sistema DBMS sottostante.  
  
 Se l'utente corrente è il proprietario di un oggetto con il nome specificato, vengono restituite le colonne di tale oggetto. Se *owner* viene omesso e l'utente corrente non è il proprietario di un oggetto con l' *oggetto* specificato, **sp_columns** Cerca un oggetto con l' *oggetto* specificato di proprietà del proprietario del database. Se viene individuato, vengono restituite le colonne di tale oggetto.  
  
`[ \@table_qualifier = ] qualifier` Nome del qualificatore dell'oggetto. *Qualifier* è di **tipo sysname** e il valore predefinito è null. Vari prodotti DBMS supportano la denominazione in tre parti per gli oggetti (_qualificatore_**.** _proprietario_**.** _nome_). In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] questa colonna rappresenta il nome del database. In alcuni prodotti rappresenta il nome del server dell'ambiente di database dell'oggetto.  
  
`[ \@column_name = ] column` È una singola colonna e viene utilizzata quando si desidera una sola colonna di informazioni di catalogo. *Column* è di **tipo nvarchar (384)** e il valore predefinito è null. Se la *colonna* non è specificata, vengono restituite tutte le colonne. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la *colonna* rappresenta il nome della colonna elencato nella tabella **syscolumns** . La ricerca con caratteri jolly è supportata. Per ottenere la massima interoperabilità, è consigliabile che nel client di gateway siano utilizzati solo i caratteri jolly standard di SQL-92, ovvero i caratteri % e _.  
  
`[ \@ODBCVer = ] ODBCVer` Versione di ODBC in uso. *ODBCVer* è di **tipo int** e il valore predefinito è 2. che indica ODBC versione 2. I valori validi sono 2 e 3. Per le differenze di comportamento tra le versioni 2 e 3, vedere la specifica ODBC **SQLColumns** .  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nessuno  
  
## <a name="result-sets"></a>Set di risultati  
 Il catalogo **sp_columns** stored procedure equivale a **SQLColumns** in ODBC. I risultati restituiti vengono ordinati in base **TABLE_QUALIFIER**, **TABLE_OWNER** e **table_name**.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**TABLE_QUALIFIER**|**sysname**|Nome del qualificatore dell'oggetto. Questo campo può essere NULL.|  
|**TABLE_OWNER**|**sysname**|Nome del proprietario dell'oggetto. Questo campo restituisce sempre un valore.|  
|**TABLE_NAME**|**sysname**|Nome dell'oggetto. Questo campo restituisce sempre un valore.|  
|**COLUMN_NAME**|**sysname**|Nome della colonna, per ogni colonna della **table_name** restituita. Questo campo restituisce sempre un valore.|  
|**DATA_TYPE**|**smallint**|Codice integer per il tipo di dati ODBC. Se si tratta di un tipo di dati di cui non è possibile eseguire il mapping a un tipo ODBC, il valore è NULL. Il nome del tipo di dati nativo viene restituito nella colonna **type_name** .|  
|**TYPE_NAME**|**sysname**|Stringa che rappresenta un tipo di dati. Il DBMS sottostante utilizza questo nome del tipo di dati.|  
|**PRECISION**|**int**|Numero di cifre significative. Il valore restituito per la colonna **Precision** è in base 10.|  
|**LENGTH**|**int**|Dimensioni del trasferimento dei dati. <sup>1</sup>|  
|**SCALA**|**smallint**|Numero di cifre a destra del separatore decimale.|  
|**RADIX**|**smallint**|Base per i tipi di dati numerici.|  
|**NULLABLE**|**smallint**|Specifica se i valori Null sono supportati.<br /><br /> 1 = I valori Null sono supportati.<br /><br /> 0 = I valori Null non sono supportati (NOT NULL).|  
|**COMMENTI**|**varchar (254)**|In questo campo viene sempre restituito NULL.|  
|**COLUMN_DEF**|**nvarchar(4000)**|Valore predefinito della colonna.|  
|**SQL_DATA_TYPE**|**smallint**|Valore del tipo di dati SQL visualizzato nel campo TYPE del descrittore. Questa colonna corrisponde alla colonna **data_type** , ad eccezione dei tipi di dati **DateTime** e **Interval** SQL-92. In questa colonna viene sempre restituito un valore.|  
|**SQL_DATETIME_SUB**|**smallint**|Codice di sottotipo per i tipi di dati **DateTime** e SQL-92 **Interval** . Per gli altri tipi di dati in questa colonna viene restituito NULL.|  
|**CHAR_OCTET_LENGTH**|**int**|Lunghezza massima, espressa in byte, di una colonna di tipo carattere o integer. Per tutti gli altri tipi di dati in questa colonna viene restituito NULL.|  
|**ORDINAL_POSITION**|**int**|Posizione ordinale della colonna nell'oggetto. La prima colonna nell'oggetto è 1. In questa colonna viene sempre restituito un valore.|  
|**IS_NULLABLE**|**varchar (254)**|Supporto di valori Null della colonna dell'oggetto. Per determinare il supporto di valori Null vengono seguite le regole ISO. In un sistema DBMS conforme a ISO SQL non vengono restituite stringhe vuote.<br /><br /> YES = La colonna ammette valori Null.<br /><br /> NO = La colonna non ammette valori Null.<br /><br /> Quando non è noto se i valori Null sono supportati, in questa colonna viene restituita una stringa di lunghezza zero.<br /><br /> Il valore restituito per questa colonna è diverso dal valore restituito per la colonna **Nullable** .|  
|**SS_DATA_TYPE**|**tinyint**|Tipo di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzato dalle stored procedure estese. Per altre informazioni, vedere [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).|  
  
 <sup>1</sup> per ulteriori informazioni, vedere la documentazione di Microsoft ODBC.  
  
## <a name="permissions"></a>Autorizzazioni  
 Sono necessarie le autorizzazioni SELECT e VIEW DEFINITION per lo schema.  
  
## <a name="remarks"></a>Commenti  
 **sp_columns** segue i requisiti per gli identificatori delimitati. Per altre informazioni, vedere [Identificatori del database](../../relational-databases/databases/database-identifiers.md).  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono restituite informazioni sulle colonne della tabella specificata.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_columns @table_name = N'Department',  
   @table_owner = N'HumanResources';  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Nell'esempio seguente vengono restituite informazioni sulle colonne della tabella specificata.  
  
```sql  
-- Uses AdventureWorks  
  
EXEC sp_columns @table_name = N'DimEmployee',  
   @table_owner = N'dbo';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_tables &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-tables-transact-sql.md)   
 [Stored procedure del catalogo &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  


