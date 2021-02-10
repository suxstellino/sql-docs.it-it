---
description: Oggetto Dimension (ADO MD)
title: Oggetto Dimension (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Dimension
helpviewer_keywords:
- Dimension object [ADO MD]
ms.assetid: 66adbbd2-23a3-4c19-a91b-84c31309aa1b
author: rothja
ms.author: jroth
ms.openlocfilehash: 311ea31a50bb9004425ad8db14c1beb309e15eb0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054806"
---
# <a name="dimension-object-ado-md"></a>Oggetto Dimension (ADO MD)
Rappresenta una delle dimensioni di un cubo multidimensionale che contiene una o più gerarchie di membri.  
  
## <a name="remarks"></a>Commenti  
 Con le raccolte e le proprietà di un oggetto **dimensione** , è possibile eseguire le operazioni seguenti:  
  
-   Identificare la **dimensione** con le proprietà [Name](./name-property-ado-md.md) e [UniqueName](./uniquename-property-ado-md.md) .  
  
-   Restituisce una stringa significativa che descrive la **dimensione** con la proprietà [Description](./description-property-ado-md.md) .  
  
-   Restituisce gli oggetti [gerarchia](./hierarchy-object-ado-md.md) che compongono la **dimensione** con la raccolta [gerarchie](./hierarchies-collection-ado-md.md) .  
  
-   Utilizzare la raccolta di [Proprietà](../ado-api/properties-collection-ado.md) ADO standard per ottenere informazioni aggiuntive sull'oggetto **Dimension** .  
  
 La raccolta **Properties** contiene proprietà fornite dal provider. Nella tabella seguente sono elencate le proprietà che potrebbero essere disponibili. L'elenco di proprietà effettivo può variare a seconda dell'implementazione del provider. Per un elenco più completo delle proprietà disponibili, vedere la documentazione relativa al provider.  
  
|Nome|Descrizione|  
|----------|-----------------|  
|CatalogName|Nome del catalogo a cui appartiene il cubo.|  
|CubeName|Nome del cubo.|  
|DefaultHierarchy|Nome univoco della gerarchia predefinita.|  
|Descrizione|Descrizione significativa del cubo.|  
|DimensionCaption|Etichetta o didascalia associata alla dimensione.|  
|DimensionCardinality|Numero di membri nella dimensione.|  
|DimensionGUID|GUID della dimensione.|  
|DimensionName|Nome della dimensione.|  
|DimensionOrdinal|Numero ordinale della dimensione tra il gruppo di dimensioni che formano il cubo.|  
|DimensionType|Tipo di dimensione.|  
|DimensionUniqueName|Nome non ambiguo della dimensione.|  
|SchemaName|Nome dello schema a cui appartiene il cubo.|  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi](./dimension-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di CubeDef (VBScript)](./cubedef-example-vbscript.md)   
 [Oggetto CubeDef (ADO MD)](./cubedef-object-ado-md.md)   
 [Raccolta Dimensions (ADO MD)](./dimensions-collection-ado-md.md)   
 [Raccolta gerarchie (ADO MD)](./hierarchies-collection-ado-md.md)   
 [Raccolta Properties (ADO)](../ado-api/properties-collection-ado.md)