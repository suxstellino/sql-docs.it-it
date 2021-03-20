---
description: SET DATEFORMAT (Transact-SQL)
title: SET DATEFORMAT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATEFORMAT
- SET DATEFORMAT
- SET_DATEFORMAT_TSQL
- DATEFORMAT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], formats
- dates [SQL Server], ordering date parts
- SET DATEFORMAT option [SQL Server]
- DATEFORMAT option [SQL Server]
- date and time [SQL Server], SET DATEFORMAT
- options [SQL Server], date
- date and time [SQL Server], DATEFORMAT
- dateparts [SQL Server], dateformat
ms.assetid: da217878-7ec4-477e-aa13-604073c948f8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a3ec299dd8d8daee6ab9711c86bc2d4f54b7f11e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751481"
---
# <a name="set-dateformat-transact-sql"></a>SET DATEFORMAT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Imposta l'ordine delle parti della data relative a mese, giorno e anno per l'interpretazione di stringhe di caratteri della data. Queste stringhe sono di tipo **data**, **smalldatetime**, **datetime**, **datetime2** o **datetimeoffset**.  
  
 Per una panoramica di tutti i tipi di dati e delle funzioni di data e ora [!INCLUDE[tsql](../../includes/tsql-md.md)], vedere [Funzioni e tipi di dati di data e ora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
SET DATEFORMAT { format | @format_var }   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *format* |  **@** _format_var_  
 Ordine delle parti della data. I parametri validi sono **mdy**, **dmy**, **ymd**, **ydm**, **myd** e **dym**. Può essere un valore Unicode o un valore DBCS (Double Byte Character Set) convertito in Unicode. L'impostazione predefinita per l'inglese (Stati Uniti) è **mdy**. Per il valore predefinito di DATEFORMAT di tutte le lingue supportate, vedere [sp_helplanguage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helplanguage-transact-sql.md).  
  
## <a name="remarks"></a>Commenti  
 Il valore **ydm** di DATEFORMAT non è supportato per i tipi di dati **date**, **datetime2** e **datetimeoffset**.  
  
 L'impostazione DATEFORMAT può interpretare le stringhe di caratteri in modo diverso per i tipi di dati date, a seconda del relativo formato stringa. Ad esempio, le interpretazioni di **datetime** e **smalldatetime** potrebbero non corrispondere a **date**, **datetime2** o **datetimeoffset**. DATEFORMAT influisce sull'interpretazione di stringhe di caratteri nel momento in cui queste vengono convertite in valori di data per il database. Non influisce sulla visualizzazione di valori del tipo di dati date o sul loro formato di archiviazione nel database.  
  
 Alcuni formati delle stringhe di caratteri, ad esempio ISO 8601, sono interpretati indipendentemente dall'impostazione DATEFORMAT.  
  
 L'opzione SET DATEFORMAT viene impostata in fase di esecuzione, non in fase di analisi.  
  
 L'opzione SET DATEFORMAT ignora l'impostazione esplicita del formato di data dell'opzione [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente sono utilizzate diverse stringhe relative alla data come input in sessioni con la stessa impostazione `DATEFORMAT`.  
  
```sql
-- Set date format to day/month/year.  
SET DATEFORMAT dmy;  
GO  
DECLARE @datevar DATETIME2 = '31/12/2008 09:01:01.1234567';  
SELECT @datevar;  
GO  
-- Result: 2008-12-31 09:01:01.123  
SET DATEFORMAT dmy;  
GO  
DECLARE @datevar DATETIME2 = '12/31/2008 09:01:01.1234567';  
SELECT @datevar;  
GO  
-- Result: Msg 241: Conversion failed when converting date and/or time -- from character string.  
  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  

