---
description: Oggetto Hierarchy (ADO MD)
title: Oggetto Hierarchy (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Hierarchy
helpviewer_keywords:
- Hierarchy object [ADO MD]
ms.assetid: 034af340-ac79-494e-ba5e-2b57da1cb9de
author: rothja
ms.author: jroth
ms.openlocfilehash: 4f84fe2157e9da38ad55a56334436a5d54128823
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051152"
---
# <a name="hierarchy-object-ado-md"></a>Oggetto Hierarchy (ADO MD)
Rappresenta un modo in cui i membri di una [dimensione](./dimension-object-ado-md.md) possono essere aggregati o sottoposti a rollup. Una dimensione può essere aggregata in una o più gerarchie.  
  
## <a name="remarks"></a>Commenti  
 Con le raccolte e le proprietà di un oggetto **gerarchia** , è possibile eseguire le operazioni seguenti:  
  
-   Identificare la **gerarchia** con le proprietà [Name](./name-property-ado-md.md) e [UniqueName](./uniquename-property-ado-md.md) .  
  
-   Restituisce una stringa significativa che descrive la **gerarchia** con la proprietà [Description](./description-property-ado-md.md) .  
  
-   Restituisce gli oggetti [Level](./level-object-ado-md.md) che costituiscono la **gerarchia** con la raccolta [levels](./levels-collection-ado-md.md) .  
  
-   Utilizzare la raccolta di [Proprietà](../ado-api/properties-collection-ado.md) ADO standard per ottenere informazioni aggiuntive sull'oggetto **gerarchia** .  
  
 La raccolta **Properties** contiene proprietà fornite dal provider. Nella tabella seguente sono elencate le proprietà che potrebbero essere disponibili. L'elenco di proprietà effettivo può variare a seconda dell'implementazione del provider. Per un elenco più completo delle proprietà disponibili, vedere la documentazione relativa al provider.  
  
|Nome|Descrizione|  
|----------|-----------------|  
|AllMember|Membro al livello più alto di rollup nella gerarchia.|  
|CatalogName|Nome del catalogo a cui appartiene il cubo.|  
|CubeName|Nome del cubo.|  
|DefaultMember|Nome univoco del membro predefinito per la gerarchia.|  
|Descrizione|Descrizione significativa della gerarchia.|  
|DimensionType|Tipo di dimensione a cui appartiene questa gerarchia.|  
|DimensionUniqueName|Nome non ambiguo della dimensione.|  
|HierarchyCaption|Etichetta o didascalia associata alla gerarchia.|  
|HierarchyCardinality|Numero di membri nella gerarchia.|  
|HierarchyGUID|GUID della gerarchia.|  
|HierarchyName|Nome della gerarchia.|  
|HierarchyUniqueName|Nome non ambiguo della gerarchia.|  
|SchemaName|Nome dello schema a cui appartiene il cubo.|  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi](./hierarchy-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di CubeDef (VBScript)](./cubedef-example-vbscript.md)   
 [Oggetto Dimension (ADO MD)](./dimension-object-ado-md.md)   
 [Raccolta gerarchie (ADO MD)](./hierarchies-collection-ado-md.md)   
 [Raccolta levels (ADO MD)](./levels-collection-ado-md.md)   
 [Raccolta Properties (ADO)](../ado-api/properties-collection-ado.md)