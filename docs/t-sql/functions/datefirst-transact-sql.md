---
description: '&#x40;&#x40;DATEFIRST (Transact-SQL)'
title: '@@DATEFIRST (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATE_FORMAT_TSQL
- DATE FORMAT
- '@@DATEFIRST_TSQL'
- '@@DATEFIRST'
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- date and time [SQL Server], SET DATEFIRST
- first day of week [SQL Server]
- dates [SQL Server], first day of week
- day of week [SQL Server]
- SET DATEFIRST option [SQL Server]
- date and time [SQL Server], DATEFIRST
- DATEFIRST option [SQL Server]
- date and time [SQL Server], @@DATEFIRST
- weekdays [SQL Server]
- '@@DATEFIRST function [SQL Server]'
- functions [SQL Server], date and time
- options [SQL Server], date
ms.assetid: a178868e-49d5-4bd5-a5e2-1283409c8ce6
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 43fc035c598fcf6c0f48807808e608e5cc247e12
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104738161"
---
# <a name="x40x40datefirst-transact-sql"></a>&#x40;&#x40;DATEFIRST (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Questa funzione restituisce il valore corrente di [SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md), per una sessione specifica.
  
Vedere [Funzioni e tipi di dati di data e ora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md) per una panoramica di tutti i tipi di dati e delle funzioni di data e ora di [!INCLUDE[tsql](../../includes/tsql-md.md)].
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
@@DATEFIRST  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-type"></a>Tipo restituito  
**tinyint**
  
## <a name="remarks"></a>Osservazioni  
SET DATEFIRST *n* specifica il primo giorno (SUNDAY, MONDAY, TUESDAY e così via) della settimana. Il valore di *n* è compreso tra 1 e 7.

```sql
SET DATEFIRST 3;
GO  
SELECT @@DATEFIRST; -- 3 (Wednesday)
GO
```  

Per un ambiente in inglese Stati Uniti, il valore predefinito di @@DATEFIRST è 7 (domenica).
  
Questa impostazione della lingua influisce sull'interpretazione delle stringhe di caratteri perché SQL Server converte tali stringhe in valori di data per l'archiviazione del database. Questa impostazione influisce anche sulla visualizzazione dei valori di data archiviati nel database. Questa impostazione non influisce sul formato di archiviazione dei dati relativi alla data.

Questo esempio imposta prima la lingua su `Italian`. L'istruzione `SELECT @@DATEFIRST;` restituisce `1`. L'istruzione successiva imposta la lingua su `us_english`. L'istruzione finale `SELECT @@DATEFIRST;` restituisce `7`.
  
```sql
SET LANGUAGE Italian;  
GO  
SELECT @@DATEFIRST;  
GO  
SET LANGUAGE us_english;  
GO  
SELECT @@DATEFIRST;  
```  
  
## <a name="examples"></a>Esempi  
Questo esempio imposta il primo giorno della settimana su `5` (venerdì) e presuppone che il giorno corrente, `Today`, cada di sabato. L'istruzione `SELECT` restituisce il valore di `DATEFIRST` e il numero del giorno corrente della settimana.
  
```sql
SET DATEFIRST 5;  
SELECT @@DATEFIRST AS 'First Day'  
    ,DATEPART(dw, SYSDATETIME()) AS 'Today';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
First Day         Today  
----------------  --------------  
5                 2  
```  
  
## <a name="example"></a>Esempio
 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
```sql
SELECT @@DATEFIRST;  
```  
  
## <a name="see-also"></a>Vedi anche
[Funzioni di configurazione &#40;Transact-SQL&#41;](../../t-sql/functions/configuration-functions-transact-sql.md)
  
  

