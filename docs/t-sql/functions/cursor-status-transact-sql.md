---
description: CURSOR_STATUS (Transact-SQL)
title: CURSOR_STATUS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CURSOR_STATUS
- CURSOR_STATUS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- status information [SQL Server], cursors
- CURSOR_STATUS function
- cursors [SQL Server], status information
ms.assetid: 3a4a840e-04f8-43bd-aada-35d78c3cb6b0
author: cawrites
ms.author: chadam
ms.openlocfilehash: 853cae9252c43a176d208b7208f2ca595366a151
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093691"
---
# <a name="cursor_status-transact-sql"></a>CURSOR_STATUS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Per un parametro specifico, `CURSOR_STATUS` visualizza se una dichiarazione di cursore ha restituito un cursore e un set di risultati o meno.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
CURSOR_STATUS   
     (  
          { 'local' , 'cursor_name' }   
          | { 'global' , 'cursor_name' }   
          | { 'variable' , 'cursor_variable' }   
     )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
'local'  
Specifica una costante che indica che l'origine del cursore è un nome di cursore locale.
  
'*cursor_name*'  
Nome del cursore. I nomi di cursore devono essere conformi alle [regole per gli identificatori dei database](../../relational-databases/databases/database-identifiers.md).
  
'global'  
Specifica una costante che indica che l'origine del cursore è un nome di cursore globale.
  
'variable'  
Specifica una costante che indica che l'origine del cursore è una variabile locale.
  
'*cursor_variable*'  
Nome di una variabile di cursore. Le variabili di cursore devono essere definite usando il tipo di dati **cursor**.
  
## <a name="return-types"></a>Tipi restituiti
**smallint**
  
|Valore restituito|Nome cursore|Variabile cursore|  
|---|---|---|
|1|Il set di risultati del cursore ha almeno una riga.<br /><br /> Nel caso di cursori INSENSITIVE e KEYSET, il set di risultati include almeno una riga.<br /><br /> Nel caso di cursori dinamici, il set di risultati può includere una o più righe oppure nessuna riga.|Il cursore allocato a questa variabile è aperto.<br /><br /> Nel caso di cursori INSENSITIVE e KEYSET, il set di risultati include almeno una riga.<br /><br /> Nel caso di cursori dinamici, il set di risultati può includere una o più righe oppure nessuna riga.|  
|0|Il set di risultati del cursore è vuoto.*|Il cursore allocato a questa variabile è aperto, ma il set di risultati è vuoto.*|  
|-1|Il cursore è chiuso.|Il cursore allocato a questa variabile è chiuso.|  
|-2|Non applicabile.|Ha una di queste possibilità:<br /><br /> La procedura chiamata in precedenza non ha assegnato un cursore a questa variabile di OUTPUT.<br /><br /> La procedura chiamata in precedenza ha assegnato un cursore a questa variabile di OUTPUT, ma al completamento della procedura il cursore era chiuso. Il cursore viene pertanto deallocato e non viene restituito alla procedura chiamante.<br /><br /> Alla variabile cursore dichiarata non è assegnato alcun cursore.|  
|-3|Il cursore specificato non esiste.|La variabile cursore con il nome specificato non esiste oppure esiste ma a questa non è ancora stato allocato un cursore.|  
  
* I cursori dinamici non restituiscono mai questo risultato.
  
## <a name="examples"></a>Esempi  
Questo esempio usa la funzione `CURSOR_STATUS` per visualizzare lo stato di un cursore dopo la dichiarazione, dopo l'apertura e dopo la chiusura di questo.
  
```sql
CREATE TABLE #TMP  
(  
   ii INT  
)  
GO  
  
INSERT INTO #TMP(ii) VALUES(1)  
INSERT INTO #TMP(ii) VALUES(2)  
INSERT INTO #TMP(ii) VALUES(3)  
  
GO  
  
--Create a cursor.  
DECLARE cur CURSOR  
FOR SELECT * FROM #TMP  
  
--Display the status of the cursor before and after opening  
--closing the cursor.  
  
SELECT CURSOR_STATUS('global','cur') AS 'After declare'  
OPEN cur  
SELECT CURSOR_STATUS('global','cur') AS 'After Open'  
CLOSE cur  
SELECT CURSOR_STATUS('global','cur') AS 'After Close'  
  
--Remove the cursor.  
DEALLOCATE cur  
  
--Drop the table.  
DROP TABLE #TMP  
  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
After declare
---------------
-1  
  
After Open
----------
1  
  
After Close
-----------
-1
```  
  
## <a name="see-also"></a>Vedere anche
[Funzioni per i cursori &#40;Transact-SQL&#41;](../../t-sql/functions/cursor-functions-transact-sql.md)  
[Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
  
  
