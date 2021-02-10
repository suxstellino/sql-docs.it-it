---
description: Oggetto Property (ADOX)
title: Oggetto Property (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Property object [ADOX]
ms.assetid: 6a56def6-dbe6-4ccc-a491-8d076889f019
author: rothja
ms.author: jroth
ms.openlocfilehash: 3fba73c2f9b0f1c722feaab311f1b74fbcf91a35
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049791"
---
# <a name="property-object-adox"></a>Oggetto Property (ADOX)
Rappresenta una caratteristica di un oggetto ADOX.  
  
## <a name="remarks"></a>Commenti  
 Gli oggetti ADOX hanno due tipi di proprietà: incorporata e dinamica.  
  
 Le proprietà predefinite sono tali proprietà immediatamente disponibili per qualsiasi nuovo oggetto, utilizzando la sintassi myObject. Property. Non vengono visualizzati come oggetti proprietà nella [raccolta delle proprietà](../ado-api/properties-collection-ado.md)di un oggetto, quindi, sebbene sia possibile modificarne i valori, non è possibile modificarne le caratteristiche.  
  
 Le proprietà dinamiche sono definite dal provider di dati sottostante e vengono visualizzate nella raccolta Properties per l'oggetto ADOX appropriato.  È possibile fare riferimento alle proprietà dinamiche solo tramite la raccolta, usando la sintassi oggetti. Properties (0) o MyObject. Properties ("Name").  
  
 Non è possibile eliminare entrambi i tipi di proprietà.  
  
 Un oggetto proprietà dinamica dispone di quattro proprietà predefinite:  
  
 La proprietà [Name](../ado-api/name-property-ado.md) è una stringa che identifica la proprietà.  
  
 La proprietà [Type](../ado-api/type-property-ado.md) è un numero intero che specifica il tipo di dati della proprietà.  
  
 La proprietà [value](../ado-api/value-property-ado.md) è una variante che contiene l'impostazione della proprietà. Value è la proprietà predefinita per un oggetto Property.  
  
 La proprietà [Attributes](../ado-api/attributes-property-ado.md) è un valore Long che indica le caratteristiche della proprietà specifica del provider.  
  
 Questa sezione contiene l'argomento seguente.  
  
-   [Proprietà, metodi ed eventi dell'oggetto Property ADOX](./adox-property-object-properties-methods-and-events.md)