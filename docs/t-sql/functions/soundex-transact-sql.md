---
description: SOUNDEX (Transact-SQL)
title: SOUNDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SOUNDEX
- SOUNDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SOUNDEX function
- comparing string data
- strings [SQL Server], similarity
- strings [SQL Server], comparing
- SOUNDEX values
ms.assetid: 8f1ed34e-8467-4512-a211-e0f43dee6584
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d5165b060e54306842e68442b394463ccb944920
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748881"
---
# <a name="soundex-transact-sql"></a>SOUNDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce un codice di quattro caratteri (SOUNDEX) per valutare la similarità di due stringhe.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
SOUNDEX ( character_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *character_expression*  
 [Espressione](../../t-sql/language-elements/expressions-transact-sql.md) alfanumerica di dati di tipo carattere. *character_expression* può essere una costante, una variabile o una colonna.  
  
## <a name="return-types"></a>Tipi restituiti  
 **varchar**  
  
## <a name="remarks"></a>Osservazioni  
 Tramite SOUNDEX una stringa alfanumerica viene convertita in un codice a quattro caratteri basato sul risultato della pronuncia della stringa in inglese. Il primo carattere del codice è il primo carattere di *character_expression*, convertito in maiuscolo. I caratteri compresi tra il secondo e il quarto del codice sono numeri che rappresentano le lettere dell'espressione. Le lettere A, E, I, O, U, H, W e Y vengono ignorate, a meno che non rappresentino la prima lettera della stringa. Se necessario per generare un codice di quattro caratteri, vengono aggiunti gli zeri finali. Per altre informazioni sul codice SOUNDEX, vedere [The Soundex Indexing System](https://www.archives.gov/research/census/soundex.html) (Sistema di indicizzazione di Soundex).  
  
 È possibile confrontare i codici SOUNDEX di stringhe diverse per vedere le pronunce simili della stringa. Tramite la funzione DIFFERENCE viene eseguito SOUNDEX in due stringhe e viene restituito un Integer che rappresenta la similitudine dei codici SOUNDEX per le stringhe in questione.  
  
 SOUNDEX è sensibile alle regole di confronto. Le funzioni stringa possono essere nidificate.  
  
## <a name="soundex-compatibility"></a>Compatibilità di SOUNDEX  
 Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la funzione SOUNDEX viene applicata a un subset di regole SOUNDEX. Con il livello di compatibilità del database 110 o superiore, tramite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene applicato un set più completo di regole.  
  
 Dopo aver effettuato l'aggiornamento al livello di compatibilità 110 o superiore, potrebbe essere necessario ricompilare gli indici, gli heap o i vincoli CHECK in cui viene utilizzata la funzione SOUNDEX.  
  
-   In un heap contenente una colonna calcolata persistente definita con la funzione SOUNDEX non possono essere eseguite query finché l'heap non viene ricompilato eseguendo l'istruzione `ALTER TABLE <table> REBUILD`.  
  
-   I vincoli CHECK definiti con la funzione SOUNDEX vengono disabilitati durante l'aggiornamento. Per abilitare il vincolo, eseguire l'istruzione `ALTER TABLE <table> WITH CHECK CHECK CONSTRAINT ALL`.  
  
-   Negli indici (viste indicizzate incluse) contenenti una colonna calcolata persistente definita con la funzione SOUNDEX non possono essere eseguite query finché l'indice non viene ricompilato eseguendo l'istruzione `ALTER INDEX ALL ON <object> REBUILD`.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene illustrata la funzione SOUNDEX e la funzione DIFFERENCE correlata. Nel primo esempio vengono restituiti i valori `SOUNDEX` standard per tutte le consonanti. Per le stringhe `SOUNDEX` e `Smith``Smythe` restituisce lo stesso risultato in quanto le vocali, la lettera `y`, le doppie e la lettera `h` non vengono incluse.  
  
```sql
-- Using SOUNDEX  
SELECT SOUNDEX ('Smith'), SOUNDEX ('Smythe');  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] Valido per le regole di confronto Latin1_General.  
  
```  
S530  S530    
```  
  
 La funzione `DIFFERENCE` valuta la differenza tra i risultati ottenuti con il modello `SOUNDEX`. Nell'esempio seguente sono illustrate due stringhe che si differenziano in relazione alle vocali. Il valore restituito è `4` (differenza minima).  
  
```sql
-- Using DIFFERENCE  
SELECT DIFFERENCE('Smithers', 'Smythers');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] Valido per le regole di confronto Latin1_General.  
  
```  
4             
```  
  
 Nell'esempio seguente le stringhe sono diverse a livello consonantico. La differenza restituita sarà pertanto `2`, vale a dire la differenza maggiore.  
  
```sql
SELECT DIFFERENCE('Anothers', 'Brothers');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] Valido per le regole di confronto Latin1_General.  
  
```  
2             
```  
  
## <a name="see-also"></a>Vedere anche  
 [DIFFERENCE &#40;Transact-SQL&#41;](../../t-sql/functions/difference-transact-sql.md)   
 [Funzioni stringa &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)   
 [Livello di compatibilità ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)  
  
  

