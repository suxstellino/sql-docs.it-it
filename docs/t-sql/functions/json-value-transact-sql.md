---
description: JSON_VALUE (Transact-SQL)
title: JSON_VALUE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- JSON_VALUE
- JSON_VALUE_TSQL
helpviewer_keywords:
- JSON_VALUE function
- JSON, extracting
- JSON, querying
ms.assetid: cd016e14-11eb-4eaf-bf05-c7cfcc820a10
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: chadam
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: b7a1d7ba5eacf32d6e52daa79bb447d0e1df0f7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165002"
---
# <a name="json_value-transact-sql"></a>JSON_VALUE (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

 Estrae un valore scalare da una stringa JSON.  
  
 Per estrarre un oggetto o una matrice da una stringa JSON anziché da un valore scalare, vedere [JSON_QUERY &#40;Transact-SQL&#41;](../../t-sql/functions/json-query-transact-sql.md). Per informazioni sulle differenze tra **JSON_VALUE** e **JSON_QUERY**, vedere [Confronto tra JSON_VALUE e JSON_QUERY](../../relational-databases/json/validate-query-and-change-json-data-with-built-in-functions-sql-server.md#JSONCompare).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
JSON_VALUE ( expression , path )  
```  
  
## <a name="arguments"></a>Argomenti

 *expression*  
 Espressione. In genere il nome di una variabile o di una colonna che contiene testo JSON.  

 Se **JSON_VALUE** individua JSON non valido in *expression* prima di trovare il valore identificato da *path*, la funzione restituisce un errore. Se **JSON_VALUE** non trova il valore identificato da *path*, analizza l'intero testo e restituisce un errore se rileva JSON non valido in un punto qualsiasi di *expression*.
  
 *path*  
 Percorso JSON che specifica la proprietà da estrarre. Per altre informazioni, vedere [Espressioni di percorso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  

In [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] e nel [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)] è possibile specificare una variabile come valore di *path*.
  
 Se il formato di *path* non è valido, **JSON_VALUE** restituisce un errore.  
  
## <a name="return-value"></a>Valore restituito

 Restituisce un valore di testo singolo di tipo nvarchar (4000). Le regole di confronto del valore restituito sono le stesse di quelle dell'espressione di input.  
  
 Se il valore è maggiore di 4000 caratteri:  
  
- In modalità lax **JSON_VALUE** restituisce Null.  
  
- In modalità strict **JSON_VALUE** restituisce un errore.  
  
 Se è necessario restituire valori scalari maggiori di 4000 caratteri, usare **OPENJSON** anziché **JSON_VALUE**. Per altre informazioni, vedere [OPENJSON &#40;Transact-SQL&#41;](../../t-sql/functions/openjson-transact-sql.md).  
  
## <a name="remarks"></a>Osservazioni

### <a name="lax-mode-and-strict-mode"></a>Modalità lax e modalità strict

 Si consideri il testo JSON seguente:  
  
```json  
DECLARE @jsonInfo NVARCHAR(MAX)

SET @jsonInfo=N'{  
     "info":{    
       "type":1,  
       "address":{    
         "town":"Bristol",  
         "county":"Avon",  
         "country":"England"  
       },  
       "tags":["Sport", "Water polo"]  
    },  
    "type":"Basic"  
 }'  
```  
  
 Nella tabella seguente viene confrontato il comportamento di **JSON_VALUE** in modalità lax e in modalità strict. Per altre informazioni sulla specifica facoltativa della modalità del percorso (lax o strict), vedere [Espressioni di percorso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  
  
|Path|Valore restituito in modalità lax|Valore restituito in modalità strict|Altre informazioni|  
|----------|------------------------------|---------------------------------|---------------|  
|$|NULL|Errore|Non è un valore scalare.<br /><br /> Usare **JSON_QUERY**.|  
|$.info.type|N'1'|N'1'|N/a|  
|$.info.address.town|N'Bristol'|N'Bristol'|N/a|  
|$.info."address"|NULL|Errore|Non è un valore scalare.<br /><br /> Usare **JSON_QUERY**.|  
|$.info.tags|NULL|Errore|Non è un valore scalare.<br /><br /> Usare **JSON_QUERY**.|  
|$.info.type[0]|NULL|Errore|Non è una matrice.|  
|$.info.none|NULL|Errore|La proprietà non esiste.|  
| &nbsp; | &nbsp; | &nbsp; | &nbsp; |
  
## <a name="examples"></a>Esempi  
  
### <a name="example-1"></a>Esempio 1
 L'esempio seguente usa i valori delle proprietà JSON `town` e `state` nei risultati della query. Poiché **JSON_VALUE** mantiene le regole di confronto dell'origine, l'ordinamento dei risultati dipende dalle regole di confronto della colonna `jsonInfo`. 

> [!NOTE]
> In questo esempio si presuppone che una tabella denominata `Person.Person` contenga una colonna `jsonInfo` di testo JSON e che questa colonna abbia la struttura illustrata in precedenza nella spiegazione della modalità lax e della modalità strict. Nel database di esempio AdventureWorks, la tabella `Person` non contiene infatti una colonna `jsonInfo`.
  
```sql  
SELECT FirstName, LastName,
 JSON_VALUE(jsonInfo,'$.info.address[0].town') AS Town
FROM Person.Person
WHERE JSON_VALUE(jsonInfo,'$.info.address[0].state') LIKE 'US%'
ORDER BY JSON_VALUE(jsonInfo,'$.info.address[0].town')
```  
  
### <a name="example-2"></a>Esempio 2
 Nell'esempio seguente viene estratto il valore della proprietà JSON `town` in una variabile locale.  
  
```sql
DECLARE @jsonInfo NVARCHAR(MAX)
DECLARE @town NVARCHAR(32)

SET @jsonInfo=N'{"info":{"address":[{"town":"Paris"},{"town":"London"}]}}';

SET @town=JSON_VALUE(@jsonInfo,'$.info.address[0].town'); -- Paris
SET @town=JSON_VALUE(@jsonInfo,'$.info.address[1].town'); -- London
```  
  
### <a name="example-3"></a>Esempio 3
 Nell'esempio seguente vengono create le colonne calcolate in base ai valori delle proprietà JSON.  
  
```sql  
CREATE TABLE dbo.Store
 (
  StoreID INT IDENTITY(1,1) NOT NULL,
  Address VARCHAR(500),
  jsonContent NVARCHAR(4000),
  Longitude AS JSON_VALUE(jsonContent, '$.address[0].longitude'),
  Latitude AS JSON_VALUE(jsonContent, '$.address[0].latitude')
 )
```  
  
## <a name="see-also"></a>Vedi anche
 [Espressioni di percorso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md)   
 [Dati JSON &#40;SQL Server&#41;](../../relational-databases/json/json-data-sql-server.md)  
  
