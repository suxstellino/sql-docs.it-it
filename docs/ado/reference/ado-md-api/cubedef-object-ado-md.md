---
description: Oggetto CubeDef (ADO MD)
title: Oggetto CubeDef (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CubeDef
helpviewer_keywords:
- CubeDef object [ADO MD]
ms.assetid: feb2581c-fc41-471c-bb69-29f8a55fda70
author: rothja
ms.author: jroth
ms.openlocfilehash: bae255b0de860a1caee016d5e8867c7fb9b3ffee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174317"
---
# <a name="cubedef-object-ado-md"></a>Oggetto CubeDef (ADO MD)
Rappresenta un cubo da uno schema multidimensionale, contenente un set di dimensioni correlate.  
  
## <a name="remarks"></a>Commenti  
 Con le raccolte e le proprietà di un oggetto **CubeDef** , è possibile eseguire le operazioni seguenti:  
  
-   Identificare un **CubeDef** con la proprietà [Name](./name-property-ado-md.md) .  
  
-   Restituisce una stringa che descrive il cubo con la proprietà [Description](./description-property-ado-md.md) .  
  
-   Restituisce le dimensioni che costituiscono il cubo con la raccolta [Dimensions](./dimensions-collection-ado-md.md) .  
  
-   Ottenere ulteriori informazioni su **CubeDef** con la raccolta delle [Proprietà](../ado-api/properties-collection-ado.md) ADO standard.  
  
 La raccolta **Properties** contiene proprietà fornite dal provider. Nella tabella seguente sono elencate le proprietà che potrebbero essere disponibili. L'elenco di proprietà effettivo può variare a seconda dell'implementazione del provider. Per un elenco più completo delle proprietà disponibili, vedere la documentazione relativa al provider.  
  
|Nome|Descrizione|  
|----------|-----------------|  
|CatalogName|Nome del catalogo a cui appartiene il cubo.|  
|CreatedOn|Data e ora di creazione del cubo.|  
|CubeGUID|GUID del cubo.|  
|CubeName|Nome del cubo.|  
|CubeType|Tipo del cubo.|  
|DataUpdatedBy|ID utente della persona che esegue l'ultimo aggiornamento dei dati.|  
|Descrizione|Descrizione significativa del cubo.|  
|LastSchemaUpdate|Data e ora dell'ultimo aggiornamento dello schema.|  
|SchemaName|Nome dello schema a cui appartiene il cubo.|  
|SchemaUpdatedBy|ID utente della persona che esegue l'ultimo aggiornamento dello schema.|  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi](./cubedef-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di CubeDef (VBScript)](./cubedef-example-vbscript.md)   
 [Oggetto Catalog (ADO MD)](./catalog-object-ado-md.md)   
 [Raccolta CubeDefs (ADO MD)](./cubedefs-collection-ado-md.md)   
 [Raccolta Dimensions (ADO MD)](./dimensions-collection-ado-md.md)   
 [Raccolta Properties (ADO)](../ado-api/properties-collection-ado.md)