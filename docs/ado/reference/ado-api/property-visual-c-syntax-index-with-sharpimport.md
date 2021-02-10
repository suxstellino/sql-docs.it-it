---
description: 'Property (Visual C++ indice della sintassi con #import)'
title: 'Property (Visual C++ indice della sintassi con #import) | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
dev_langs:
- C++
helpviewer_keywords:
- 'Property collection [ADO], Visual C++ syntax index with #import'
ms.assetid: 80988ca7-f514-438d-bf6f-9390dfe93fc3
author: rothja
ms.author: jroth
ms.openlocfilehash: 28dfb2b59620a3cb5c4fae0a11fe8b24a9ab3b28
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040871"
---
# <a name="property-visual-c-syntax-index-with-import"></a>Property (Visual C++ indice della sintassi con #import)
## <a name="properties"></a>Proprietà  
  
```  
long GetAttributes( );  
void PutAttributes( long plAttributes );  
__declspec(property(get=GetAttributes,put=PutAttributes)) long  
    Attributes;  
  
_bstr_t GetName( );  
__declspec(property(get=GetName)) _bstr_t Name;  
  
enum DataTypeEnum GetType( );  
__declspec(property(get=GetType)) enum DataTypeEnum Type;  
  
_variant_t GetValue( );  
void PutValue( const _variant_t & pval );  
__declspec(property(get=GetValue,put=PutValue)) _variant_t Value;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto Property (ADO)](./property-object-ado.md)