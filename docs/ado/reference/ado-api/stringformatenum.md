---
description: StringFormatEnum
title: StringFormatEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- StringFormatEnum
helpviewer_keywords:
- StringFormatEnum enumeration [ADO]
ms.assetid: 28f7d1ec-092b-4323-a39d-d3f882c6c81a
author: rothja
ms.author: jroth
ms.openlocfilehash: b23a6bc4354f4c67c07bcdc1f4b4d463fff072f2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056506"
---
# <a name="stringformatenum"></a>StringFormatEnum
Specifica il formato durante il recupero di un [Recordset](./recordset-object-ado.md) come stringa.  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adClipString**|2|Delimita le righe in base a *RowDelimiter*, colonne per *ColumnDelimiter* e valori null per *NullExpr*. Questi tre parametri del metodo [GetString](./getstring-method-ado.md) sono validi solo con un *StringFormat* di **adClipString**.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Pacchetto: **com. ms. wfc. Data**  
  
|Costante|  
|--------------|  
|AdoEnums. StringFormat. CLIPSTRING|  
  
## <a name="applies-to"></a>Si applica a  
 [Metodo GetString (ADO)](./getstring-method-ado.md)