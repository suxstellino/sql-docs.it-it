---
title: Struttura SSVARIANT (OLE DB Driver)
description: Struttura SSVARIANT in OLE DB Driver per SQL Server
ms.custom: ''
ms.date: 06/15/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
f1_keywords:
- SSVARIANT
helpviewer_keywords:
- SSVARIANT struct
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7095942d695c57667e47d32bb6c60a97c51fbd59
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752021"
---
# <a name="ssvariant-structure"></a>Struttura SSVARIANT
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  La struttura **SSVARIANT** definita in msoledbsql.h corrisponde a un valore DBTYPE_SQLVARIANT in OLE DB Driver per SQL Server.  
  
 **SSVARIANT** è un'unione discriminante. A seconda del valore del membro vt, il consumer può determinare il membro da leggere. I valori vt corrispondono ai tipi di dati [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pertanto, la struttura **SSVARIANT** può contenere qualsiasi tipo SQL Server. Per altre informazioni sulla struttura dei dati per i tipi di OLE DB standard, vedere [Indicatori di tipo](/previous-versions/windows/desktop/ms711251(v=vs.85)).  
  
## <a name="remarks"></a>Osservazioni  
 Quando DataTypeCompat==80, diversi sottotipi di **SSVARIANT** diventano stringhe. I valori vt seguenti verranno ad esempio visualizzati in **SSVARIANT** come VT_SS_WVARSTRING:  
  
-   VT_SS_DATETIMEOFFSET  
  
-   VT_SS_DATETIME2  
  
-   VT_SS_TIME2  
  
-   VT_SS_DATE  
  
 Quando DateTypeCompat == 0, questi tipi vengono visualizzati nel formato nativo.  
  
 Per altre informazioni su SSPROP_INIT_DATATYPECOMPATIBILITY, vedere [Uso delle parole chiave delle stringhe di connessione con OLE DB Driver per SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md).  
  
 Il file msoledbsql.h contiene macro di accesso di tipo Variant che semplificano la risoluzione dei riferimenti ai tipi di membri nella struttura **SSVARIANT**. Un esempio è rappresentato da V_SS_DATETIMEOFFSET, che è possibile utilizzare come indicato di seguito:  
  
```  
memcpy(&V_SS_DATETIMEOFFSET(pssVar).tsoDateTimeOffsetVal, pDTO, cbNative);  
V_SS_DATETIMEOFFSET(pssVar).bScale = bScale;  
```  
  
 Per il set completo di macro di accesso per ogni membro della struttura **SSVARIANT**, fare riferimento al file msoledbsql.h.  
  
 Nella tabella seguente vengono descritti i membri della struttura **SSVARIANT**:  
  
|Membro|Indicatore del tipo OLE DB|Tipo di dati C di OLE DB|valore vt|Commenti|  
|------------|---------------------------|------------------------|--------------|--------------|  
|vt|SSVARTYPE|||Specifica il tipo di valore contenuto nella struttura **SSVARIANT**.|  
|bTinyIntVal|DBTYPE_UI1|**BYTE**|**VT_SS_UI1**|Supporta il tipo di dati **tinyint**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|sShortIntVal|DBTYPE_I2|**SHORT**|**VT_SS_I2**|Supporta il tipo di dati **smallint**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|lIntVal|DBTYPE_I4|**LONG**|**VT_SS_I4**|Supporta il tipo di dati **int**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|llBigIntVal|DBTYPE_I8|**LARGE_INTEGER**|**VT_SS_I8**|Supporta il tipo di dati **bigint**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|fltRealVal|DBTYPE_R4|**float**|**VT_SS_R4**|Supporta il tipo di dati **real**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|dblFloatVal|DBTYPE_R8|**double**|**VT_SS_R8**|Supporta il tipo di dati **float**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|cyMoneyVal|DBTYPE_CY|**LARGE_INTEGER**|**VT_SS_MONEY VT_SS_SMALLMONEY**|Supporta il tipo di dati **money** e **smallmoney**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|fBitVal|DBTYPE_BOOL|**VARIANT_BOOL**|**VT_SS_BIT**|Supporta il tipo di dati **bit**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|rgbGuidVal|DBTYPE_GUID|**GUID**|**VT_SS_GUID**|Supporta il tipo di dati **uniqueidentifier**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|numNumericVal|DBTYPE_NUMERIC|**DB_NUMERIC**|**VT_SS_NUMERIC**|Supporta il tipo di dati **numeric**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|dDateVal|DBTYPE_DATE|**DBDATE**|**VT_SS_DATE**|Supporta il tipo di dati **date**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|tsDateTimeVal|DBTYPE_DBTIMESTAMP|**DBTIMESTAMP**|**VT_SS_SMALLDATETIME VT_SS_DATETIME VT_SS_DATETIME2**|Supporta i tipi di dati **smalldatetime**, **datetime** e **datetime2**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|Time2Val|DBTYPE_DBTIME2|**DBTIME2**|**VT_SS_TIME2**|Supporta il tipo di dati **time**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].<br /><br /> Include i membri indicati di seguito:<br /><br /> *tTime2Val* (**DBTIME2**)<br /><br /> *bScale* (**BYTE**) Specifica la scala per il valore *tTime2Val*.|  
|DateTimeVal|DBTYPE_DBTIMESTAMP|**DBTIMESTAMP**|**VT_SS_DATETIME2**|Supporta il tipo di dati **datetime2**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].<br /><br /> Include i membri indicati di seguito:<br /><br /> *tsDataTimeVal* (DBTIMESTAMP)<br /><br /> *bScale* (**BYTE**) Specifica la scala per il valore *tsDataTimeVal*.|  
|DateTimeOffsetVal|DBTYPE_DBTIMESTAMPOFSET|**DBTIMESTAMPOFFSET**|**VT_SS_DATETIMEOFFSET**|Supporta il tipo di dati **datetimeoffset**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].<br /><br /> Include i membri indicati di seguito:<br /><br /> *tsoDateTimeOffsetVal* (**DBTIMESTAMPOFFSET**)<br /><br /> *bScale* (**BYTE**) Specifica la scala per il valore *tsoDateTimeOffsetVal*.|  
|NCharVal|Nessun indicatore del tipo OLE DB corrispondente.|**struct _NCharVal**|**VT_SS_WVARSTRING,**<br /><br /> **VT_SS_WSTRING**|Supporta i tipi di dati **nchar** e **nvarchar**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].<br /><br /> Include i membri indicati di seguito:<br /><br /> *sActualLength* (**SHORT**) Specifica la lunghezza effettiva per la stringa a cui punta *pwchNCharVal*. Non include lo zero finale.<br /><br /> *sMaxLength* (**SHORT**) Specifica la lunghezza massima per la stringa a cui punta *pwchNCharVal*.<br /><br /> *pwchNCharVal* (**WCHAR** \*) Puntatore alla stringa.<br /><br /> *rgbReserved* (**BYTE[5]** ) Specifica le informazioni sulle regole di confronto.<br /><br /> Membri non usati: *dwReserved* e *pwchReserved*.|  
|CharVal|Nessun indicatore del tipo OLE DB corrispondente.|**struct _CharVal**|**VT_SS_STRING,**<br /><br /> **VT_SS_VARSTRING**|Supporta i tipi di dati **char** e **varchar**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].<br /><br /> Include i membri indicati di seguito:<br /><br /> *sActualLength* (**SHORT**) Specifica la lunghezza effettiva per la stringa a cui punta *pchCharVal*. Non include lo zero finale.<br /><br /> *sMaxLength* (**SHORT**) Specifica la lunghezza massima per la stringa a cui punta *pchCharVal*.<br /><br /> *pchCharVal* (**CHAR** \*) Puntatore alla stringa.<br /><br /> *rgbReserved* (**BYTE[5]** ) Specifica le informazioni sulle regole di confronto.<br /><br /> Membri non utilizzati:<br /><br /> *dwReserved* e *pwchReserved*.|  
|BinaryVal|Nessun indicatore del tipo OLE DB corrispondente.|**struct _BinaryVal**|**VT_SS_VARBINARY,**<br /><br /> **VT_SS_BINARY**|Supporta i tipi di dati **binary** e **varbinary**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].<br /><br /> Include i membri indicati di seguito:<br /><br /> *sActualLength* (**SHORT**) Specifica la lunghezza effettiva per i dati a cui punta *prgbBinaryVal*.<br /><br /> *sMaxLength* (**SHORT**) Specifica la lunghezza massima per i dati a cui punta *prgbBinaryVal*.<br /><br /> *prgbBinaryVal* (**BYTE** \*) Puntatore ai dati binari.<br /><br /> Membro non usato: *dwReserved*.|  
|UnknownType|UNUSED|UNUSED|UNUSED|UNUSED|  
|BLOBType|UNUSED|UNUSED|UNUSED|UNUSED|  
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |
  
## <a name="known-issues"></a>Problemi noti
### <a name="possible-narrow-string-data-corruption"></a>Possibile danneggiamento dei dati della stringa narrow
Prima della versione 18.4 di OLE DB Driver, l'inserimento in una colonna `sql_variant` può causare il danneggiamento dei dati nel server se si verificano tutte le condizioni seguenti:
- La tabella codici del computer client non corrisponde alla tabella codici delle regole di confronto del database.
- Il buffer client da inserire contiene caratteri della stringa narrow non ASCII codificati nella tabella codici del client.
- Si verifica una delle condizioni seguenti:
  - Il campo `pwszDataSourceType` nella struttura `DBPARAMBINDINFO` che descrive il parametro corrispondente alla colonna `sql_variant` è stato impostato su `L"DBTYPE_SQLVARIANT"`, `L"DBTYPE_VARIANT"` o `L"sql_variant"`. Per informazioni dettagliate, vedere: [ICommandWithParameters::SetParameterInfo](/previous-versions/windows/desktop/ms725393(v=vs.85)).

    *or*
  - È stata preparata la query SQL con parametri usata per l'inserimento.

In particolare, OLE DB Driver non ha convertito i dati nella tabella codici delle regole di confronto del database prima di inserirli. Tuttavia, il driver ha indicato erroneamente al server che i dati sono stati codificati nella tabella codici delle regole di confronto del database. Questo comportamento ha causato una mancata corrispondenza tra i dati e la tabella codici corrispondente archiviata nella colonna `sql_variant`.

Analogamente, dopo il recupero dello stesso valore, OLE DB Driver non ha convertito le stringhe nella tabella codici del client. Tuttavia, poiché i dati inseriti erano già presenti nella tabella codici del client (vedere il paragrafo precedente), l'applicazione client potrebbe interpretare i dati in modo corretto. Anche in questo caso, le applicazioni che usano altri driver potrebbero recuperare questi valori in un formato danneggiato. Il danneggiamento si verifica perché altri driver hanno interpretato la stringa nella tabella codici delle regole di confronto del database e tentano di convertirla nella tabella codici del client.

A partire dalla versione 18.4, OLE DB Driver converte le stringhe narrow nella tabella codici delle regole di confronto del database prima dell'inserimento. Analogamente, il driver converte di nuovo i dati nella tabella codici del client al momento del recupero. Di conseguenza, le applicazioni client soggette al bug citato in precedenza potrebbero riscontrare problemi durante il recupero dei dati inseriti usando una versione precedente di OLE DB Driver. La [procedura di recupero](#recovery-procedure) ha lo scopo di fornire indicazioni per la risoluzione di questi problemi.

### <a name="recovery-procedure"></a>Procedura di recupero
> [!IMPORTANT]  
> Prima di completare la procedura di recupero seguente, assicurarsi di eseguire il backup dei dati esistenti.

Se l'applicazione riscontra problemi durante il recupero dei dati da una colonna `sql_variant` dopo il passaggio alla versione 18.4 di OLE DB Driver, è necessario modificare i dati danneggiati in modo da avere le stesse regole di confronto del database in cui sono archiviati i dati. Lo script seguente può essere usato per recuperare un singolo valore da una colonna `sql_variant`. Lo script è un modello ed è necessario modificarlo per adattarlo allo scenario.

> [!IMPORTANT]  
> Poiché la tabella codici originale dei dati non viene archiviata, è necessario indicare al server il modo in cui sono stati inizialmente codificati i dati. A questo scopo, eseguire lo script nel contesto di un database che include la stessa tabella codici della tabella codici del client che ha inserito inizialmente i dati. Ad esempio, se i dati danneggiati sono stati inseriti da un client configurato con la tabella codici `932`, è necessario eseguire lo script seguente nel contesto di un database con regole di confronto giapponesi, ad esempio `Japanese_XJIS_100_CS_AI`.

```sql
/*
    Description:
        Template that can be used to recover the corrupted value inserted into the sql_variant column.

    Scenario:
        The database is named [YourDatabase] and it contains a table named [YourTable], which contains the corrupted value.
        Schema is named [dbo].
        The corrupted value is stored in a column of type sql_variant named [YourColumn].
        The corrupted value is sql_variant of BaseType char. For details on sql_variant properties, see:
            https://docs.microsoft.com/sql/t-sql/functions/sql-variant-property-transact-sql
*/

-- Base type in sql_variant can hold a maximum of 8000 bytes
-- For details see: 
--  https://docs.microsoft.com/sql/t-sql/data-types/sql-variant-transact-sql#remarks
DECLARE @bin VARBINARY(8000)

-- In the following lines we convert the sql_variant base type to binary.
-- <FilterExpression>
--      Is a placeholder and must be replaced with an expression that filters a single corrupted value to be recovered.
--      Therefore, the expression must result in a single value being returned only.
SET @bin = (SELECT CAST([YourColumn] AS VARBINARY(8000)) FROM [YourDatabase].[dbo].[YourTable] WHERE <FilterExpression>)

-- In the following lines we store the binary value in char(59) (a fixed-size character data type).
-- IMPORTANT NOTE: 
--      This example assumes the corrupted sql_variant's base type is char(59).
--      You MUST adjust the type (that is, char/varchar) and size to match your scenario exactly.
DECLARE @char CHAR(59)
SET @char = CAST((@bin) AS CHAR(59))
DECLARE @sqlvariant sql_variant

-- The following lines recover the corrupted value by translating the value to the collation of the database.
-- <DBCollation>
--      Must be replaced with the collation (for example, Latin1_General_100_CI_AS_SC_UTF8) of the database holding the data.
SET @sqlvariant = @char collate <DBCollation>

-- Finally, we update the corrupted value with the recovered value.
-- "<FilterExpression>"
--      Is a placeholder and must be replaced with an expression that filters a single corrupted value to be recovered.
--      Therefore, the expression must result in a single value being returned only.
UPDATE [YourDatabase].[dbo].[YourTable] SET [YourColumn] = @sqlvariant WHERE <FilterExpression>
```

## <a name="see-also"></a>Vedere anche  
 [Tipi di dati &#40;OLE DB&#41;](../../oledb/ole-db-data-types/data-types-ole-db.md)  
  
