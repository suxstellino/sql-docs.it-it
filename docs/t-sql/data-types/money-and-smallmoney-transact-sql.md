---
description: money e smallmoney (Transact-SQL)
title: money e smallmoney (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- money_TSQL
- money
- smallmoney
- smallmoney_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- money data type, about money data type
- money data type
- smallmoney data type
- values [SQL Server], monetary
- currency [SQL Server]
ms.assetid: 57861137-89ea-4b89-b361-390597d7bccc
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8218bf8b98cd072dd60f8458c75819761153b634
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2020
ms.locfileid: "91111284"
---
# <a name="money-and-smallmoney-transact-sql"></a>money e smallmoney (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Tipi di dati che rappresentano valori monetari o valutari.
  
## <a name="remarks"></a>Osservazioni  
  
|Tipo di dati|Range|Archiviazione|  
|---|---|---|
|**money**|Da -922.337.203.685.477,5808 a 922.337.203.685.477,5807 (da -922.337.203.685.477,58<br />a 922.337.203.685.477,58 per Informatica.  Informatica supporta solo due posizioni decimali e non quattro).|8 byte|  
|**smallmoney**|Da -214.748,3648 a 214.748,3647|4 byte|  
  
I tipi di dati **money** e **smallmoney** sono caratterizzati da una precisione pari a dieci millesimi delle unità monetarie rappresentate. Per Informatica i tipi di dati **money** e **smallmoney** sono caratterizzati da una precisione pari a un centesimo delle unità monetarie rappresentate.
  
Per separare le unità di valuta parziali, ad esempio i centesimi, da quelle intere, utilizzare il punto. Ad esempio, 2.15 indica 2 dollari e 15 centesimi.
  
È possibile utilizzare questi tipi di dati per i simboli di valuta illustrati di seguito.
  
![Tabella dei simboli di valuta, valori esadecimali](../../t-sql/data-types/media/money01.gif "Tabella dei simboli di valuta, valori esadecimali")
  
Non è necessario racchiudere i dati di tipo valuta tra virgolette singole ('). È importante tenere presente che anche se è possibile specificare un simbolo di valuta che precede i valori di valuta, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le informazioni relative alla valuta archiviate non sono associate al simbolo, ma è presente solo il valore numerico.
  
## <a name="converting-money-data"></a>Conversione dei dati di tipo money
Nella conversione dal tipo di dati Integer a **money** le unità vengono interpretate come unità di valuta. Ad esempio, il valore Integer 4 viene convertito nell'equivalente di 4 unità di valuta per il tipo **money**.
  
Nell'esempio seguente i valori **smallmoney** e **money** vengono convertiti rispettivamente nei tipi di dati **varchar** e **decimal**.
  
```sql
DECLARE @mymoney_sm SMALLMONEY = 3148.29,  
        @mymoney    MONEY = 3148.29;  
SELECT  CAST(@mymoney_sm AS VARCHAR) AS 'SM_MONEY varchar',  
        CAST(@mymoney AS DECIMAL)    AS 'MONEY DECIMAL';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
SM_MONEY VARCHAR               MONEY DECIMAL  
------------------------------ ----------------------  
3148.29                        3148    
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vedere anche
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)
[CAST e CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)
[Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)
[sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)
  
  
