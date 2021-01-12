---
description: JSON_MODIFY (Transact-SQL)
title: JSON_MODIFY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
ms.assetid: 96bc8255-a037-4907-aec4-1a9c30814651
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: chadam
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: 547a304b4f5a9b6069d023a751f73d99e8456bbc
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092235"
---
# <a name="json_modify-transact-sql"></a>JSON_MODIFY (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Aggiorna il valore di una proprietà in una stringa JSON e restituisce la stringa JSON aggiornata.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
JSON_MODIFY ( expression , path , newValue )  
```  
  
## <a name="arguments"></a>Argomenti

 *expression*  
 Espressione. In genere il nome di una variabile o di una colonna che contiene testo JSON.  
  
 **JSON_MODIFY** restituisce un errore se *expression* non contiene codice JSON valido.  
  
 *path*  
 Espressione corrispondente a un percorso JSON che specifica la proprietà da aggiornare.

 La sintassi di *path* è la seguente:  
  
 `[append] [ lax | strict ] $.<json path>`  
  
- *append*  
    Modificatore facoltativo che specifica che il nuovo valore deve essere aggiunto alla matrice a cui fa riferimento *\<json path>* .  
  
- *lax*  
    Specifica che la proprietà a cui fa riferimento *\<json path>* non deve necessariamente esistere. Se la proprietà non è presente, JSON_MODIFY tenta di inserire il nuovo valore nel percorso specificato. L'inserimento può non riuscire se la proprietà non può essere inserita nel percorso. Se non si specifica né *lax* né *strict*, *lax* è la modalità predefinita.  
  
- *strict*  
    Specifica che la proprietà a cui fa riferimento *\<json path>* deve essere presente nell'espressione JSON. Se la proprietà non è presente, JSON_MODIFY restituisce un errore.  
  
- *\<json path>*  
    Specifica il percorso della proprietà da aggiornare. Per altre informazioni, vedere [Espressioni di percorso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  
  
In [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] e nel [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)] è possibile specificare una variabile come valore di *path*.

**JSON_MODIFY** restituisce un errore se il formato di *path* non è valido.  
  
 *newValue*  
 Nuovo valore della proprietà specificata da *path*.  
 Il nuovo valore deve essere di tipo [n]varchar o text.
  
 In modalità lax, se il nuovo valore è NULL, JSON_MODIFY elimina la chiave specificata.  
  
Se il tipo del valore è NVARCHAR o VARCHAR, JSON_MODIFY sottopone a escape tutti i caratteri speciali nel nuovo valore. Un valore di testo non viene sottoposto a escape se si tratta di codice JSON correttamente formattato generato da FOR JSON, JSON_QUERY o JSON_MODIFY.  
  
## <a name="return-value"></a>Valore restituito

 Restituisce il valore aggiornato di *expression* come testo JSON correttamente formattato.  
  
## <a name="remarks"></a>Osservazioni

 La funzione JSON_MODIFY consente di aggiornare il valore di una proprietà esistente, inserire una nuova coppia chiave:valore o eliminare una chiave in base a una combinazione di modalità e valori forniti.  
  
 La tabella seguente confronta il comportamento di **JSON_MODIFY** in modalità lax e in modalità strict. Per altre informazioni sulla specifica facoltativa della modalità del percorso (lax o strict), vedere [Espressioni di percorso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  
  
|Valore esistente|Percorso esistente|Modalità lax|Modalità strict|  
|--------------------|-----------------|--------------|-----------------|  
|Non NULL|Sì|Aggiorna il valore esistente.|Aggiorna il valore esistente.|  
|Non NULL|No|Prova a creare una nuova coppia chiave:valore nel percorso specificato.<br /><br /> Questa operazione può avere esito negativo. Se ad esempio si specifica il percorso `$.user.setting.theme`, JSON_MODIFY non inserisce la chiave `theme` se l'oggetto `$.user` o `$.user.settings` non esiste o se settings è una matrice o un valore scalare.|Errore: INVALID_PROPERTY|  
|NULL|Sì|Elimina la proprietà esistente.|Imposta il valore esistente su Null.|  
|NULL|No|Nessuna azione. Il primo argomento viene restituito come risultato.|Errore: INVALID_PROPERTY|  
  
 In modalità lax, JSON_MODIFY tenta di creare una nuova coppia chiave:valore, ma in alcuni casi l'operazione non riesce.  
  
## <a name="examples"></a>Esempi  
  
### <a name="example---basic-operations"></a>Esempio: operazioni di base

 L'esempio seguente illustra le operazioni di base che è possibile eseguire con testo JSON.  
  
 **Query**
  
```sql

DECLARE @info NVARCHAR(100)='{"name":"John","skills":["C#","SQL"]}'

PRINT @info

-- Update name  

SET @info=JSON_MODIFY(@info,'$.name','Mike')

PRINT @info

-- Insert surname  

SET @info=JSON_MODIFY(@info,'$.surname','Smith')

PRINT @info

-- Delete name  

SET @info=JSON_MODIFY(@info,'$.name',NULL)

PRINT @info

-- Add skill  

SET @info=JSON_MODIFY(@info,'append $.skills','Azure')

PRINT @info
```  
  
 **Risultati**
  
```json  
{
    "name": "John",
    "skills": ["C#", "SQL"]
} {
    "name": "Mike",
    "skills": ["C#", "SQL"]
} {
    "name": "Mike",
    "skills": ["C#", "SQL"],
    "surname": "Smith"
} {
    "skills": ["C#", "SQL"],
    "surname": "Smith"
} {
    "skills": ["C#", "SQL", "Azure"],
    "surname": "Smith"
}
```  
  
### <a name="example---multiple-updates"></a>Esempio: più aggiornamenti

 Con JSON_MODIFY è possibile aggiornare una sola proprietà. Se è necessario eseguire più aggiornamenti, è possibile usare più chiamate a JSON_MODIFY.  
  
 **Query**
  
```sql
DECLARE @info NVARCHAR(100)='{"name":"John","skills":["C#","SQL"]}'

PRINT @info

-- Multiple updates  

SET @info=JSON_MODIFY(JSON_MODIFY(JSON_MODIFY(@info,'$.name','Mike'),'$.surname','Smith'),'append $.skills','Azure')

PRINT @info
```  
  
 **Risultati**
  
```json  
{
    "name": "John",
    "skills": ["C#", "SQL"]
} {
    "name": "Mike",
    "skills": ["C#", "SQL", "Azure"],
    "surname": "Smith"
}
```  
  
### <a name="example---rename-a-key"></a>Esempio: ridenominazione di una chiave  
 L'esempio seguente illustra come rinominare una proprietà all'interno di testo JSON con la funzione JSON_MODIFY. Prima è possibile inserire il valore di una proprietà esistente come nuova coppia chiave-valore. È quindi possibile eliminare la chiave precedente impostando il valore della proprietà precedente su NULL.  
  
 **Query**
  
```sql  
DECLARE @product NVARCHAR(100)='{"price":49.99}'

PRINT @product

-- Rename property  

SET @product=
 JSON_MODIFY(
  JSON_MODIFY(@product,'$.Price',CAST(JSON_VALUE(@product,'$.price') AS NUMERIC(4,2))),
  '$.price',
  NULL
 )

PRINT @product
```  
  
 **Risultati**
  
```json  
{
    "price": 49.99
} {
    "Price": 49.99
}
```  
  
 Se si non esegue il cast del nuovo valore a un tipo numerico, JSON_MODIFY lo considera come testo e lo racchiude tra virgolette doppie.  
  
### <a name="example---increment-a-value"></a>Esempio: incremento di un valore

 L'esempio seguente illustra come incrementare il valore di una proprietà all'interno di testo JSON con la funzione JSON_MODIFY. Prima è possibile inserire il valore della proprietà esistente come nuova coppia chiave-valore. È quindi possibile eliminare la chiave precedente impostando il valore della proprietà precedente su NULL.  
  
 **Query**
  
```sql  
DECLARE @stats NVARCHAR(100)='{"click_count": 173}'

PRINT @stats

-- Increment value  

SET @stats=JSON_MODIFY(@stats,'$.click_count',
 CAST(JSON_VALUE(@stats,'$.click_count') AS INT)+1)

PRINT @stats
```  
  
 **Risultati**
  
```json  
{
    "click_count": 173
} {
    "click_count": 174
}
```  
  
### <a name="example---modify-a-json-object"></a>Esempio: modifica di un oggetto JSON

 JSON_MODIFY considera l'argomento *newValue* come testo normale anche se contiene testo JSON correttamente formattato. Di conseguenza, l'output JSON della funzione è racchiuso tra virgolette doppie e tutti i caratteri speciali sono sottoposti a escape, come illustrato nell'esempio seguente.  
  
 **Query**  
  
```sql
DECLARE @info NVARCHAR(100)='{"name":"John","skills":["C#","SQL"]}'

PRINT @info

-- Update skills array

SET @info=JSON_MODIFY(@info,'$.skills','["C#","T-SQL","Azure"]')

PRINT @info
```  
  
 **Risultati**
  
```json  
{
    "name": "John",
    "skills": ["C#", "SQL"]
} {
    "name": "John",
    "skills": "["C#","T-SQL","Azure"]"
}
```  
  
 Per evitare l'escape automatico, specificare *newValue* tramite la funzione JSON_QUERY. JSON_MODIFY sa che il valore restituito da JSON_MODIFY è codice JSON correttamente formattato e quindi non lo sottopone a escape.  
  
 **Query**  
  
```sql
DECLARE @info NVARCHAR(100)='{"name":"John","skills":["C#","SQL"]}'

PRINT @info

-- Update skills array  

SET @info=JSON_MODIFY(@info,'$.skills',JSON_QUERY('["C#","T-SQL","Azure"]'))

PRINT @info
```  
  
 **Risultati**
  
```json  
{
    "name": "John",
    "skills": ["C#", "SQL"]
} {
    "name": "John",
    "skills": ["C#", "T-SQL", "Azure"]
}
```  
  
### <a name="example---update-a-json-column"></a>Esempio: aggiornamento di una colonna JSON

 L'esempio seguente aggiorna il valore di una proprietà in una colonna di tabella contenente codice JSON.  
  
```sql  
UPDATE Employee
SET jsonCol=JSON_MODIFY(jsonCol,'$.info.address.town','London')
WHERE EmployeeID=17
```  
  
## <a name="see-also"></a>Vedere anche

- [JSON Path Expressions (Espressioni di percorso JSON) &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md)   
- [Dati JSON &#40;SQL Server&#41;](../../relational-databases/json/json-data-sql-server.md)  
  
