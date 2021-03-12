---
description: TODATETIMEOFFSET (Transact-SQL)
title: TODATETIMEOFFSET (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/22/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TO_DATETIMEOFFSET_TSQL
- SWITCH_TZ_TSQL
- SWITCH_TZ
- TO_DATETIMEOFFSET
dev_langs:
- TSQL
helpviewer_keywords:
- date and time [SQL Server], TODATETIMEOFFSET
- TODATETIMEOFFSET function
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
ms.assetid: b5fafc08-efd4-4a3b-a0b3-068981a0a685
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 63dc3c4dbb16716a2d936be7e8d7661e912917d1
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622784"
---
# <a name="todatetimeoffset-transact-sql"></a>TODATETIMEOFFSET (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce un valore **datetimeoffset** convertito da un'espressione **datetime2**.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
TODATETIMEOFFSET ( datetime_expression , timezoneoffset_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *datetime_expression*  
 [Espressione](../../t-sql/language-elements/expressions-transact-sql.md) che viene risolta in un valore [datetime2](../../t-sql/data-types/datetime2-transact-sql.md).  
  
> [!NOTE]  
>  L'espressione non può essere di tipo **text**, **ntext** o **image** perché questi tipi non possono essere convertiti in modo implicito in **varchar** o **nvarchar**.  
  
 *timezoneoffset_expression*  
 Espressione che rappresenta la differenza di fuso orario in minuti (se è un numero intero), ad esempio -120, o in ore e minuti (se è una stringa), ad esempio '+13:00'. L'intervallo è compreso tra +14 e -14 (in ore). L'espressione viene interpretata nell'ora locale per il timezoneoffset_expression specificato.  

  
> [!NOTE]  
>  Se l'espressione è una stringa di caratteri, il formato deve essere {+|-}TZH:THM.  
  
## <a name="return-type"></a>Tipo restituito  
 **datetimeoffset**. La precisione frazionaria è uguale all'argomento *datetime_expression* .  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-changing-the-time-zone-offset-of-the-current-date-and-time"></a>R. Modifica della differenza di fuso orario della data e ora correnti  
 Nell'esempio seguente viene impostata la differenza di fuso orario della data e ora correnti sul valore `-07:00`.  
  
```sql  
DECLARE @todaysDateTime DATETIME2;  
SET @todaysDateTime = GETDATE();  
SELECT TODATETIMEOFFSET (@todaysDateTime, '-07:00');  
-- RETURNS 2019-04-22 16:23:51.7666667 -07:00  
```  
  
### <a name="b-changing-the-time-zone-offset-in-minutes"></a>B. Modifica della differenza di fuso orario in minuti  
 Nell'esempio seguente la differenza di fuso orario viene impostata sul valore `-120` minuti.  
  
```sql  
SELECT TODATETIMEOFFSET(SYSDATETIME(), -120)
-- RETURNS: 2019-04-22 11:39:21.6986813 -02:00  
```  
  
### <a name="c-adding-a-13-hour-time-zone-offset"></a>C. Aggiunta di una differenza di fuso orario di 13 ore  
 Nell'esempio seguente viene aggiunta una differenza di fuso orario di 13 ore a una data e a un'ora.  
  
```sql  
SELECT TODATETIMEOFFSET(SYSDATETIME(), '+13:00')
-- RETURNS: 2019-04-22 11:39:29.0339301 +13:00
```  
  
## <a name="see-also"></a>Vedere anche  
 [CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [Funzioni e tipi di dati di data e ora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)   
 [AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)  
  
  

