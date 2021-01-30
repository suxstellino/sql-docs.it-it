---
description: GetDescendant (Motore di database)
title: GetDescendant (Motore di database) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GetDescendant_TSQL
- GetDescendant
dev_langs:
- TSQL
helpviewer_keywords:
- GetDescendant [Database Engine]
ms.assetid: f5f39596-033e-4243-acbc-caa188b45b03
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 21c00b305f4bc31782307f7abb779c46ffe97e6d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165174"
---
# <a name="getdescendant-database-engine"></a>GetDescendant (Motore di database)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Restituisce il nodo figlio di un nodo padre.
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Transact-SQL syntax  
parent.GetDescendant ( child1 , child2 )   
```  
  
```syntaxsql
-- CLR syntax  
SqlHierarchyId GetDescendant ( SqlHierarchyId child1 , SqlHierarchyId child2 )   
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*child1*  
NULL o il valore **hierarchyid** di un figlio del nodo corrente.
  
*child2*  
NULL o il valore **hierarchyid** di un figlio del nodo corrente.
  
## <a name="return-types"></a>Tipi restituiti  
**Tipo SQL Server restituito: hierarchyid**
  
**Tipo CLR restituito: SqlHierarchyId**
  
## <a name="remarks"></a>Commenti  
Restituisce un nodo figlio discendente del padre.
-   Se il padre è Null, restituisce Null.  
-   Se il padre è diverso da Null e child1 e child2 sono Null, restituisce un figlio del padre.  
-   Se il padre e child1 sono diversi da Null e child2 è NULL, restituisce un figlio del padre maggiore di child1.  
-   Se il padre e child2 sono diversi da Null e child1 è Null, restituisce un figlio del padre minore di child2.  
-   Se il padre, child1 e child2 sono diversi da Null, restituisce un figlio del padre maggiore di child1 e minore di child2.  
-   Se child1 è diverso da Null, ma non è un figlio del padre, viene generata un'eccezione.  
-   Se child2 è diverso da Null, ma non è un figlio del padre, viene generata un'eccezione.  
-   Se child1 >= child2, viene generata un'eccezione.  
  
GetDescendant è deterministico. Pertanto, se viene chiamato con gli stessi input GetDescendant restituisce sempre lo stesso output. L'identità esatta del figlio restituito può tuttavia variare in base alla relazione con gli altri nodi, come dimostrato nell'esempio C.
  
## <a name="examples"></a>Esempi  
  
### <a name="a-inserting-a-row-as-the-least-descendant-node"></a>R. Inserimento di una riga come ultimo nodo discendente  
Viene assunto un nuovo dipendente, messo in relazione con un dipendente esistente nel nodo `/3/1/`. Eseguire il codice seguente per inserire la nuova riga usando il metodo GetDescendant senza argomenti per specificare il nuovo nodo delle righe come `/3/1/1/`:
  
```sql
DECLARE @Manager hierarchyid;   
SET @Manager = CAST('/3/1/' AS hierarchyid);  
  
INSERT HumanResources.EmployeeDemo (OrgNode, LoginID, Title, HireDate)  
VALUES  
(@Manager.GetDescendant(NULL, NULL),  
'adventure-works\FirstNewEmployee', 'Application Intern', '3/11/07') ;  
```  
  
### <a name="b-inserting-a-row-as-a-greater-descendant-node"></a>B. Inserimento di una riga come nodo discendente maggiore  
Viene assunto un altro nuovo dipendente, che risponde allo stesso responsabile dell'esempio A. Eseguire il codice seguente per inserire la nuova riga mediante il metodo GetDescendant, usando l'argomento child1 per specificare che il nodo della nuova riga seguirà il nodo dell'esempio A, divenendo `/3/1/2/`:
  
```sql
DECLARE @Manager hierarchyid, @Child1 hierarchyid;  
  
SET @Manager = CAST('/3/1/' AS hierarchyid);  
SET @Child1 = CAST('/3/1/1/' AS hierarchyid);  
  
INSERT HumanResources.EmployeeDemo (OrgNode, LoginID, Title, HireDate)  
VALUES  
(@Manager.GetDescendant(@Child1, NULL),  
'adventure-works\SecondNewEmployee', 'Application Intern', '3/11/07') ;  
```  
  
### <a name="c-inserting-a-row-between-two-existing-nodes"></a>C. Inserimento di una riga tra due nodi esistenti  
Viene assunto un terzo dipendente, che risponde allo stesso responsabile dell'esempio A. In questo esempio viene inserita la nuova riga in un nodo maggiore di `FirstNewEmployee` dell'esempio A e minore di `SecondNewEmployee` nell'esempio B. Eseguire il codice seguente usando il metodo GetDescendant. Utilizzare sia l'argomento child1 che l'argomento child2 per specificare che il nodo della nuova riga diventerà il nodo `/3/1/1.1/`:
  
```sql
DECLARE @Manager hierarchyid, @Child1 hierarchyid, @Child2 hierarchyid;  
  
SET @Manager = CAST('/3/1/' AS hierarchyid);  
SET @Child1 = CAST('/3/1/1/' AS hierarchyid);  
SET @Child2 = CAST('/3/1/2/' AS hierarchyid);  
  
INSERT HumanResources.EmployeeDemo (OrgNode, LoginID, Title, HireDate)  
VALUES  
(@Manager.GetDescendant(@Child1, @Child2),  
'adventure-works\ThirdNewEmployee', 'Application Intern', '3/11/07') ;  
  
```  
  
Dopo il completamento degli esempi A, B e C i nodi aggiunti alla tabella si troveranno allo stesso livello dei valori **hierarchyid** seguenti:
  
`/3/1/1/`
  
`/3/1/1.1/`
  
`/3/1/2/`
  
Il nodo `/3/1/1.1/` è maggiore del nodo `/3/1/1/`, ma si trova allo stesso livello della gerarchia.
  
### <a name="d-scalar-examples"></a>D. Esempi scalari  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta inserimenti ed eliminazioni arbitrari di qualsiasi nodo **hierarchyid**. Usando GetDescendant() è sempre possibile generare un nodo tra due nodi **hierarchyid** qualsiasi. Eseguire il codice seguente per generare nodi di esempio utilizzando `GetDescendant`:
  
```sql
DECLARE @h hierarchyid = hierarchyid::GetRoot();  
DECLARE @c hierarchyid = @h.GetDescendant(NULL, NULL);  
SELECT @c.ToString();  
DECLARE @c2 hierarchyid = @h.GetDescendant(@c, NULL);  
SELECT @c2.ToString();  
SET @c2 = @h.GetDescendant(@c, @c2);  
SELECT @c2.ToString();  
SET @c = @h.GetDescendant(@c, @c2);  
SELECT @c.ToString();  
SET @c2 = @h.GetDescendant(@c, @c2);  
SELECT @c2.ToString();  
```  
  
### <a name="e-clr-example"></a>E. Esempio CLR  
Nel frammento di codice seguente viene chiamato il metodo `GetDescendant()`.
  
```sql
SqlHierarchyId parent, child1, child2;  
parent = SqlHierarchyId.GetRoot();  
child1 = parent.GetDescendant(SqlHierarchyId.Null, SqlHierarchyId.Null);  
child2 = parent.GetDescendant(child1, SqlHierarchyId.Null);  
Console.Write(parent.GetDescendant(child1, child2).ToString());  
```  
  
## <a name="see-also"></a>Vedere anche
[Guida di riferimento ai metodi per il tipo di dati hierarchyid](./hierarchyid-data-type-method-reference.md)  
[Dati gerarchici &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
  
