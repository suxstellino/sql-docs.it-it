---
description: PropertyAttributesEnum
title: PropertyAttributesEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- PropertyAttributesEnum
helpviewer_keywords:
- PropertyAttributesEnum enumeration [ADO]
ms.assetid: 96a01955-a6b4-4cbf-9c73-52bcd1e9fb25
author: rothja
ms.author: jroth
ms.openlocfilehash: 694170521f4fc99d5dd9c99d15f48e7461acde09
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040861"
---
# <a name="propertyattributesenum"></a>PropertyAttributesEnum
Specifica gli attributi di un oggetto [Proprietà](./property-object-ado.md) .  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adPropNotSupported**|0|Indica che la proprietà non è supportata dal provider.|  
|**adPropRequired**|1|Indica che l'utente deve specificare un valore per questa proprietà prima dell'inizializzazione dell'origine dati.|  
|**adPropOptional**|2|Indica che l'utente non deve specificare un valore per questa proprietà prima dell'inizializzazione dell'origine dati.|  
|**adPropRead**|512|Indica che l'utente può leggere la proprietà.|  
|**adPropWrite**|1024|Indica che l'utente può impostare la proprietà.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Pacchetto: **com. ms. wfc. Data**  
  
|Costante|  
|--------------|  
|AdoEnums. PropertyAttributes. NOTSUPPORTED|  
|AdoEnums. PropertyAttributes. REQUIRED|  
|AdoEnums. PropertyAttributes. OPTIONAL|  
|AdoEnums. PropertyAttributes. READ|  
|AdoEnums. PropertyAttributes. WRITE|  
  
## <a name="applies-to"></a>Si applica a  
 [Proprietà Attributes (ADO)](./attributes-property-ado.md)