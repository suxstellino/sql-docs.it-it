---
description: GetReparentedValue (Motore di database)
title: GetReparentedValue (Motore di database) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Reparent_TSQL
- Reparent
dev_langs:
- TSQL
helpviewer_keywords:
- Reparent [Database Engine]
ms.assetid: f47f8e25-08ef-498b-84f4-a317aca1f358
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c71c30acd6832ed2f53941bd3e841eb235c7b1cf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165161"
---
# <a name="getreparentedvalue-database-engine"></a>GetReparentedValue (Motore di database)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Restituisce un nodo il cui percorso dalla radice è il percorso di _newRoot_, seguito dal percorso da _oldRoot_.
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
-- Transact-SQL syntax  
node. GetReparentedValue ( oldRoot, newRoot )  
```  
  
```syntaxsql
-- CLR syntax  
SqlHierarchyId GetReparentedValue ( SqlHierarchyId oldRoot , SqlHierarchyId newRoot )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
_oldRoot_  
Valore **hierarchyid** del nodo che rappresenta il livello della gerarchia da modificare.
  
_newRoot_  
**hierarchyid** che rappresenta il nodo. Sostituire la sezione _oldRoot_ del nodo corrente per spostare il nodo.
  
## <a name="return-types"></a>Tipi restituiti  
**Tipo SQL Server restituito: hierarchyid**
  
**Tipo CLR restituito: SqlHierarchyId**
  
## <a name="remarks"></a>Commenti  
Consente di modificare l'albero spostando nodi da _oldRoot_ a _newRoot_. GetReparentedValue può essere usato per spostare un nodo di una gerarchia in un nuovo percorso della gerarchia. Il tipo di dati **hierarchyid** rappresenta la struttura gerarchica, ma non la applica. Gli utenti devono verificare che hierarchyid sia strutturato in modo appropriato per il nuovo percorso. Un indice univoco applicato al tipo di dati **hierarchyid** può impedire la presenza di voci duplicate. Per un esempio di spostamento di un sottoalbero intero, vedere [Dati gerarchici &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md).
  
## <a name="examples"></a>Esempi  
  
### <a name="a-comparing-two-node-locations"></a>R. Confronto tra due percorsi di nodi  
Nell'esempio seguente viene illustrato il valore hierarchyid corrente di un nodo. Viene anche visualizzato il valore **hierarchyid** del nodo, nel caso in cui quest'ultimo venga spostato e diventi un discendente del nodo **\@NewParent**. Per visualizzare le relazioni gerarchiche, viene utilizzato il metodo `ToString()`.
  
```sql
DECLARE @SubjectEmployee hierarchyid , @OldParent hierarchyid, @NewParent hierarchyid  
SELECT @SubjectEmployee = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\gail0' ;  
SELECT @OldParent = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\roberto0' ; -- who is /1/1/  
SELECT @NewParent = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\wanida0' ; -- who is /2/3/  
  
SELECT OrgNode.ToString() AS Current_OrgNode_AS_Text,   
(@SubjectEmployee. GetReparentedValue(@OldParent, @NewParent) ).ToString() AS Proposed_OrgNode_AS_Text,  
OrgNode AS Current_OrgNode,  
@SubjectEmployee. GetReparentedValue(@OldParent, @NewParent) AS Proposed_OrgNode,  
*  
FROM HumanResources.EmployeeDemo  
WHERE OrgNode = @SubjectEmployee ;  
GO  
```  
  
### <a name="b-updating-a-node-to-a-new-location"></a>B. Aggiornamento di un nodo in un nuovo percorso  
Nell'esempio seguente viene utilizzato `GetReparentedValue()` in un'istruzione UPDATE per spostare un nodo da un percorso obsoleto in un nuovo percorso nella gerarchia:
  
```sql
DECLARE @SubjectEmployee hierarchyid , @OldParent hierarchyid, @NewParent hierarchyid  
SELECT @SubjectEmployee = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\gail0' ; -- Node /1/1/2/  
SELECT @OldParent = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\roberto0' ; -- Node /1/1/  
SELECT @NewParent = OrgNode FROM HumanResources.EmployeeDemo  
  WHERE LoginID = 'adventure-works\wanida0' ; -- Node /2/3/  
  
UPDATE HumanResources.EmployeeDemo  
SET OrgNode = @SubjectEmployee. GetReparentedValue(@OldParent, @NewParent)   
WHERE OrgNode = @SubjectEmployee ;  
  
SELECT OrgNode.ToString() AS Current_OrgNode_AS_Text,   
*  
FROM HumanResources.EmployeeDemo  
WHERE LoginID = 'adventure-works\gail0' ; -- Now node /2/3/2/  
```  
  
### <a name="c-clr-example"></a>C. Esempio CLR  
Nel frammento di codice seguente viene chiamato il metodo GetReparentedValue ():
  
```sql
this. GetReparentedValue(oldParent, newParent)  
```  
  
## <a name="see-also"></a>Vedere anche
[Guida di riferimento ai metodi per il tipo di dati hierarchyid](./hierarchyid-data-type-method-reference.md)  
[Dati gerarchici &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
  
