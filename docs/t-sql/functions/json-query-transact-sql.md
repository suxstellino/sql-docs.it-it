---
description: JSON_QUERY (Transact-SQL)
title: JSON_QUERY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- JSON_QUERY
- JSON_QUERY_TSQL
helpviewer_keywords:
- JSON, extracting
- JSON, querying
- JSON_QUERY function
ms.assetid: 1ab0d90f-19b6-4988-ab4f-22fdf28b7c79
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: chadam
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: f5f81325f27a37f1e6d30ea7c8fb6bdb5dad096b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "100337991"
---
# <a name="json_query-transact-sql"></a>JSON_QUERY (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

 Estrae un oggetto o una matrice da una stringa JSON.  
  
 Per estrarre un valore scalare da una stringa JSON anziché un oggetto o una matrice, vedere [JSON_VALUE &#40;Transact-SQL&#41;](../../t-sql/functions/json-value-transact-sql.md). Per informazioni sulle differenze tra **JSON_VALUE** e **JSON_QUERY**, vedere [Confronto tra JSON_VALUE e JSON_QUERY](../../relational-databases/json/validate-query-and-change-json-data-with-built-in-functions-sql-server.md#JSONCompare).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
JSON_QUERY ( expression [ , path ] )  
```  
  
## <a name="arguments"></a>Argomenti

 *expression*  
 Espressione. In genere il nome di una variabile o di una colonna che contiene testo JSON.  
  
 Se **JSON_QUERY** individua JSON non valido in *expression* prima di trovare il valore identificato da *path*, la funzione restituisce un errore. Se **JSON_QUERY** non trova il valore identificato da *path*, analizza l'intero testo e restituisce un errore se rileva JSON non valido in un punto qualsiasi di *expression*.  
  
 *path*  
 Percorso JSON che specifica l'oggetto o la matrice da estrarre.

In [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] e nel [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)] è possibile specificare una variabile come valore di *path*.

Il percorso JSON può specificare la modalità lax o strict per l'analisi. Se non si specifica la modalità di analisi, la modalità predefinita è lax. Per altre informazioni, vedere [Espressioni di percorso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  

Il valore predefinito per *path* è "$". Di conseguenza, se non si specifica un valore per *path*, **JSON_QUERY** restituisce l'input *expression*.

Se il formato di *path* non è valido, **JSON_QUERY** restituisce un errore.  
  
## <a name="return-value"></a>Valore restituito

 Restituisce un frammento JSON di tipo nvarchar (max). Le regole di confronto del valore restituito sono le stesse di quelle dell'espressione di input.  
  
 Se il valore non è un oggetto o una matrice:  
  
- In modalità lax **JSON_QUERY** restituisce Null.  
  
- In modalità strict **JSON_QUERY** restituisce un errore.  
  
## <a name="remarks"></a>Osservazioni  

### <a name="lax-mode-and-strict-mode"></a>Modalità lax e modalità strict

 Si consideri il testo JSON seguente:  
  
```json  
{
   "info": {
      "type": 1,
      "address": {
         "town": "Bristol",
         "county": "Avon",
         "country": "England"
      },
      "tags": ["Sport", "Water polo"]
   },
   "type": "Basic"
} 
```  
  
 Nella tabella seguente viene confrontato il comportamento di **JSON_QUERY** in modalità lax e in modalità strict. Per altre informazioni sulla specifica facoltativa della modalità del percorso (lax o strict), vedere [Espressioni di percorso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  
  
|Path|Valore restituito in modalità lax|Valore restituito in modalità strict|Altre informazioni|  
|----------|------------------------------|---------------------------------|---------------|  
|$|Restituisce l'intero testo JSON.|Restituisce l'intero testo JSON.|N/a|  
|$.info.type|NULL|Errore|Non è un oggetto o una matrice.<br /><br /> Usare **JSON_VALUE**.|  
|$.info.address.town|NULL|Errore|Non è un oggetto o una matrice.<br /><br /> Usare **JSON_VALUE**.|  
|$.info."address"|N'{ "town":"Bristol", "county":"Avon", "country":"England" }'|N'{ "town":"Bristol", "county":"Avon", "country":"England" }'|N/a|  
|$.info.tags|N'[ "Sport", "Water polo"]'|N'[ "Sport", "Water polo"]'|N/a|  
|$.info.type[0]|NULL|Errore|Non è una matrice.|  
|$.info.none|NULL|Errore|La proprietà non esiste.|  

### <a name="using-json_query-with-for-json"></a>Uso di JSON_QUERY con FOR JSON

**JSON_QUERY** restituisce un frammento JSON valido. Di conseguenza, **FOR JSON** non ignora i caratteri speciali nel valore restituito di **JSON_QUERY**.

Se vengono restituiti risultati con FOR JSON e vengono inclusi dati che sono già in formato JSON, in una colonna o come risultato di un'espressione, eseguire il wrapping dei dati JSON con **JSON_QUERY** senza il parametro *path*.

## <a name="examples"></a>Esempi  
  
### <a name="example-1"></a>Esempio 1

 Nell'esempio seguente viene illustrato come restituire un frammento JSON da una colonna `CustomFields` nei risultati della query.  
  
```sql  
SELECT PersonID,FullName,
  JSON_QUERY(CustomFields,'$.OtherLanguages') AS Languages
FROM Application.People
```  
  
### <a name="example-2"></a>Esempio 2

Nell'esempio seguente viene illustrato come includere frammenti JSON nell'output della clausola FOR JSON.  
  
```sql  
SELECT StockItemID, StockItemName,
         JSON_QUERY(Tags) as Tags,
         JSON_QUERY(CONCAT('["',ValidFrom,'","',ValidTo,'"]')) ValidityPeriod
FROM Warehouse.StockItems
FOR JSON PATH
```  
  
## <a name="see-also"></a>Vedere anche

- [JSON Path Expressions (Espressioni di percorso JSON) &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md)   
- [Dati JSON &#40;SQL Server&#41;](../../relational-databases/json/json-data-sql-server.md)  
