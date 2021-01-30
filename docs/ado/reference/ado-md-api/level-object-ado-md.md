---
description: Oggetto Level (ADO MD)
title: Oggetto Level (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Level
helpviewer_keywords:
- Level object [ADO MD]
ms.assetid: 37815869-ed30-45fd-9aea-0a986c1b305c
author: rothja
ms.author: jroth
ms.openlocfilehash: ebbebb2150cbe9c03e51ac296c39c362a6670e1d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169806"
---
# <a name="level-object-ado-md"></a>Oggetto Level (ADO MD)
Contiene un set di membri, ognuno dei quali ha lo stesso rango all'interno di una gerarchia.  
  
## <a name="remarks"></a>Commenti  
 Con le raccolte e le proprietà di un oggetto **livello** , è possibile eseguire le operazioni seguenti:  
  
-   Identificare il **livello** con le proprietà [Name](./name-property-ado-md.md) e [UniqueName](./uniquename-property-ado-md.md) .  
  
-   Restituisce una stringa da usare quando si Visualizza il **livello** con la proprietà [Caption](./caption-property-ado-md.md) .  
  
-   Restituisce una stringa significativa che descrive il **livello** con la proprietà [Description](./description-property-ado-md.md) .  
  
-   Restituire gli oggetti [membro](./member-object-ado-md.md) che costituiscono il **livello** con la raccolta [Members](./members-collection-ado-md.md) .  
  
-   Restituisce il numero di livelli dalla radice del **livello** con la proprietà [Depth](./depth-property-ado-md.md) .  
  
-   Utilizzare la raccolta delle [Proprietà](../ado-api/properties-collection-ado.md) ADO standard per ottenere informazioni aggiuntive sull'oggetto **Level** .  
  
 La raccolta **Properties** contiene proprietà fornite dal provider. Nella tabella seguente sono elencate le proprietà che potrebbero essere disponibili. L'elenco di proprietà effettivo può variare a seconda dell'implementazione del provider. Per un elenco più completo delle proprietà disponibili, vedere la documentazione relativa al provider.  
  
|Nome|Descrizione|  
|----------|-----------------|  
|CatalogName|Nome del catalogo a cui appartiene il cubo.|  
|CubeName|Nome del cubo.|  
|Descrizione|Descrizione significativa del livello.|  
|DimensionUniqueName|Nome non ambiguo della [dimensione](./dimension-object-ado-md.md).|  
|HierarchyUniqueName|Nome non ambiguo della gerarchia.|  
|LevelCaption|Etichetta o didascalia associata al livello.|  
|LevelCardinality|Numero di membri nel livello.|  
|LevelGUID|GUID del livello.|  
|LevelName|Nome del livello.|  
|LevelNumber|Distanza tra il livello e la radice della gerarchia.|  
|LevelType|Tipo di livello.|  
|LevelUniqueName|Nome non ambiguo del livello.|  
|SchemaName|Nome dello schema a cui appartiene il cubo.|  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi](./level-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di CubeDef (VBScript)](./cubedef-example-vbscript.md)   
 [Oggetto Hierarchy (ADO MD)](./hierarchy-object-ado-md.md)   
 [Raccolta levels (ADO MD)](./levels-collection-ado-md.md)   
 [Raccolta Members (ADO MD)](./members-collection-ado-md.md)   
 [Raccolta Properties (ADO)](../ado-api/properties-collection-ado.md)