---
description: SET ANSI_PADDING (Transact-SQL)
title: SET ANSI_PADDING (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/04/2017
ms.prod: sql
ms.prod_service: synapse-analytics, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ANSI_PADDING_TSQL
- ANSI_PADDING
- SET_ANSI_PADDING_TSQL
- SET ANSI_PADDING
dev_langs:
- TSQL
helpviewer_keywords:
- data types [SQL Server], short values
- ANSI_PADDING option
- short values [SQL Server]
- SET ANSI_PADDING statement
- trailing blanks
ms.assetid: 92bd29a3-9beb-410e-b7e0-7bc1dc1ae6d0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8e90a5092b02eaf3f646c1a12b22ad1601a01b63
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754311"
---
# <a name="set-ansi_padding-transact-sql"></a>SET ANSI_PADDING (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Controlla il modo in cui la colonna archivia i valori di dimensioni inferiori a quelle definite della colonna e i valori contenenti spazi vuoti finali nei dati **char**, **varchar**, **binary** e **varbinary** .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi
 
### <a name="syntax-for-ssnoversion-mdmd-and-sssodfull-mdmd"></a>Sintassi per [!INCLUDE[ssnoversion-md.md](../../includes/ssnoversion-md.md)] e [!INCLUDE[sssodfull-md.md](../../includes/sssodfull-md.md)] 
```syntaxsql
SET ANSI_PADDING { ON | OFF }
```

### <a name="syntax-for-sssdw-mdmd-and-sspdw-mdmd"></a>Sintassi per [!INCLUDE[sssdw-md.md](../../includes/sssdw-md.md)] e [!INCLUDE[sspdw-md.md](../../includes/sspdw-md.md)]
```syntaxsql
SET ANSI_PADDING ON
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Osservazioni
 Le colonne definite con i tipi di dati **char**, **varchar**, **binary** e **varbinary** hanno dimensioni definite.  
  
 Questa impostazione influisce solo sulla definizione di nuove colonne. Dopo la creazione della colonna, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] archivia i valori in base all'impostazione specificata in fase di creazione. Le successive modifiche dell'impostazione non influiscono sulle colonne esistenti.  
  
> [!NOTE]  
> ANSI_PADDING deve essere sempre impostata su ON.  
  
 Nella tabella seguente vengono illustrati gli effetti dell'impostazione dell'opzione SET ANSI_PADDING quando i valori vengono inseriti in colonne con dati di tipo **char**, **varchar**, **binary** e **varbinary**.  
  
|Impostazione|char(*n*) NOT NULL o binary(*n*) NOT NULL|char(*n*) NULL o binary(*n*) NULL|varchar(*n*) o varbinary(*n*)|  
|-------------|----------------------------------------------------|--------------------------------------------|----------------------------------------|  
|ON|Completa il valore originale con spazi vuoti finali in colonne di tipo **char** e con zeri finali in colonne di tipo **binary** in modo da riempire l'intera lunghezza della colonna.|Segue le stesse regole di **char(** _n_ **)** o **binary(** _n_ **)** NOT NULL quando l'opzione SET ANSI_PADDING è impostata su ON.|Gli spazi vuoti finali nei valori di tipo carattere inseriti in colonne di tipo **varchar** non vengono eliminati. Gli zeri finali in valori binari inseriti in colonne di tipo **varbinary** non vengono eliminati. I valori non vengono riempiti con caratteri nulli per l'intera lunghezza della colonna.|  
|OFF|Completa il valore originale con spazi vuoti finali in colonne di tipo **char** e con zeri finali in colonne di tipo **binary** in modo da riempire l'intera lunghezza della colonna.|Segue le stesse regole di **varchar** o **varbinary** quando l'opzione SET ANSI_PADDING è impostata su OFF.|Gli spazi vuoti finali in valori di tipo carattere inseriti in colonne di tipo **varchar** vengono eliminati. Gli zeri finali in valori binari inseriti in colonne di tipo **varbinary** vengono eliminati.|  
  
> [!NOTE]  
> Durante l'operazione di riempimento le colonne di tipo **char** vengono riempite con spazi, mentre le colonne di tipo **binary** vengono riempite con zeri. Durante l'eliminazione, dalle colonne di tipo **char** vengono eliminati gli spazi vuoti finali, mentre dalle colonne di tipo **binary** vengono eliminati gli zeri finali.  
  
È necessario che l'opzione ANSI_PADDING sia impostata su ON durante la creazione o la modifica di indici in colonne calcolate o viste indicizzate. Per altre informazioni sulle impostazioni dell'opzione SET necessarie per viste indicizzate e indici nelle colonne calcolate, vedere "Considerazioni sull'uso delle istruzioni SET" nell'argomento [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md).  
  
L'impostazione predefinita di SET ANSI_PADDING è ON. Il driver ODBC di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client e il provider OLE DB di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] impostano automaticamente l'opzione ANSI_PADDING su ON al momento della connessione. Questa può essere configurata nelle origini dati ODBC, negli attributi di connessione ODBC o nelle proprietà di connessione OLE DB impostate nell'applicazione prima della connessione. L'opzione predefinita per SET ANSI_PADDING è OFF per le connessioni di applicazioni DB-Library.  
  
 L'impostazione SET ANSI_PADDING non ha effetto sui tipi di dati **nchar**, **nvarchar**, **ntext**, **text**, **image**, **varbinary(max)**, **varchar(max)** e **nvarchar(max)**. Viene sempre applicata l'opzione SET ANSI_PADDING ON. Gli spazi vuoti e gli zero finali non vengono pertanto eliminati.  
  
Quando l'opzione ANSI_DEFAULTS è impostata su ON, l'opzione ANSI_PADDING è abilitata.  
  
L'impostazione di ANSI_PADDING viene definita in fase di esecuzione, non in fase di analisi.  
  
Per visualizzare l'impostazione corrente per questa impostazione, eseguire la query riportata di seguito.  
  
```sql  
DECLARE @ANSI_PADDING VARCHAR(3) = 'OFF';  
IF ( (16 & @@OPTIONS) = 16 ) SET @ANSI_PADDING = 'ON';  
SELECT @ANSI_PADDING AS ANSI_PADDING;  
```  
  
## <a name="permissions"></a>Autorizzazioni  
È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="examples"></a>Esempi  
Nell'esempio seguente viene illustrato l'impatto dell'impostazione sui diversi tipi di dati.  

Impostare ANSI_PADDING su ON e testare.

```sql  
PRINT 'Testing with ANSI_PADDING ON'  
SET ANSI_PADDING ON;  
GO  
  
CREATE TABLE t1 (  
   charcol CHAR(16) NULL,   
   varcharcol VARCHAR(16) NULL,   
   varbinarycol VARBINARY(8)  
);  
GO  
INSERT INTO t1 VALUES ('No blanks', 'No blanks', 0x00ee);  
INSERT INTO t1 VALUES ('Trailing blank ', 'Trailing blank ', 0x00ee00);  
  
SELECT 'CHAR' = '>' + charcol + '\<', 'VARCHAR'='>' + varcharcol + '\<',  
   varbinarycol  
FROM t1;  
GO  
```

Ora impostare ANSI_PADDING su OFF e testare.

```sql
PRINT 'Testing with ANSI_PADDING OFF';  
SET ANSI_PADDING OFF;  
GO  
  
CREATE TABLE t2 (  
   charcol CHAR(16) NULL,   
   varcharcol VARCHAR(16) NULL,   
   varbinarycol VARBINARY(8)  
);  
GO  
INSERT INTO t2 VALUES ('No blanks', 'No blanks', 0x00ee);  
INSERT INTO t2 VALUES ('Trailing blank ', 'Trailing blank ', 0x00ee00);  
  
SELECT 'CHAR' = '>' + charcol + '\<', 'VARCHAR'='>' + varcharcol + '<',  
   varbinarycol  
FROM t2;  
GO  
  
DROP TABLE t1;  
DROP TABLE t2;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SESSIONPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/sessionproperty-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [SET ANSI_DEFAULTS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-defaults-transact-sql.md)  
  
  
