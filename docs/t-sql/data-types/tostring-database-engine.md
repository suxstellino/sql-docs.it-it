---
description: ToString (Motore di database)
title: ToString (Motore di database)
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ToString
- ToString_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ToString [Database Engine]
ms.assetid: 5fc11ca5-c26d-4518-9512-67aa0270f110
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: fc4af80c3e3c05009eb488a87caa0107f3185e87
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179635"
---
# <a name="tostring-database-engine"></a>ToString (Motore di database)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Restituisce una stringa con la rappresentazione logica di *this*. ToString viene chiamato implicitamente quando viene eseguita una conversione da **hierarchyid** a un tipo stringa. Funziona in modo opposto a [Parse &#40;Motore di database&#41;](../../t-sql/data-types/parse-database-engine.md).
  
## <a name="syntax"></a>Sintassi  

```syntaxsql
-- Transact-SQL syntax
node.ToString  ( )
-- This is functionally equivalent to the following syntax  
-- which implicitly calls ToString():  
CAST(node AS nvarchar(4000))  
```  
  
```syntaxsql
-- CLR syntax
string ToString  ( )
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipi restituiti

**Tipo SQL Server restituito: nvarchar(4000)**
  
**Tipo CLR restituito: String**
  
## <a name="remarks"></a>Osservazioni  
Restituisce il percorso logico nella gerarchia. Ad esempio, `/2/1/` rappresenta la quarta riga ([!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) nella struttura gerarchica seguente di un file system:
  
```sql
/        C:\  
/1/      C:\Database Files  
/2/      C:\Program Files  
/2/1/    C:\Program Files\Microsoft SQL Server  
/2/2/    C:\Program Files\Microsoft Visual Studio  
/3/      C:\Windows  
```  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-transact-sql-example-in-a-table"></a>R. Esempio Transact-SQL in una tabella  
Nell'esempio seguente viene restituita sia la colonna `OrgNode` sia il tipo di dati **hierarchyid**, in un formato stringa più leggibile:
  
```sql
SELECT OrgNode,  
OrgNode.ToString() AS Node  
FROM HumanResources.EmployeeDemo  
ORDER BY OrgNode ;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
OrgNode   Node  
0x        /  
0x58      /1/  
0x5AC0    /1/1/  
0x5B40    /1/2/  
0x5BC0    /1/3/  
0x5C20    /1/4/  
...  
```  
  
### <a name="b-converting-transact-sql-values-without-a-table"></a>B. Conversione di valori Transact-SQL senza una tabella  
Nell'esempio di codice seguente viene usato `ToString` per convertire un valore **hierarchyid** in una stringa e `Parse` per convertire un valore stringa in un valore **hierarchyid**.
  
```sql
DECLARE @StringValue AS nvarchar(4000), @hierarchyidValue AS hierarchyid  
SET @StringValue = '/1/1/3/'  
SET @hierarchyidValue = 0x5ADE  
  
SELECT hierarchyid::Parse(@StringValue) AS hierarchyidRepresentation,  
@hierarchyidValue.ToString() AS StringRepresentation ;
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
hierarchyidRepresentation    StringRepresentation
-------------------------    -----------------------
0x5ADE                       /1/1/3/
```
  
### <a name="c-clr-example"></a>C. Esempio CLR  
Nel frammento di codice seguente viene chiamato il metodo ToString():
  
```sql
this.ToString()  
```  
  
## <a name="see-also"></a>Vedere anche
[Guida di riferimento ai metodi per il tipo di dati hierarchyid](./hierarchyid-data-type-method-reference.md)  
[Dati gerarchici &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
  
