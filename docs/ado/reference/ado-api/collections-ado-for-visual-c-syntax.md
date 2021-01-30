---
description: Raccolte (sintassi ADO per Visual C++)
title: Raccolte (sintassi ADO per Visual C++) | Microsoft Docs
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
- ADO for Visual C++ syntax [ADO]
- syntax indexes [ADO], ADO for Visual C++ syntax
- collections [ADO], ADO for Visual C++ syntax
ms.assetid: 6a0109a0-f2d9-4f7c-8e1e-42763f9acaea
author: rothja
ms.author: jroth
ms.openlocfilehash: 9cda61fc0d1d206a75059263c5450c813a9e5ddb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164687"
---
# <a name="collections-ado-for-visual-c-syntax"></a>Raccolte (sintassi ADO per Visual C++)
## <a name="parameters"></a>Parametri  
  
### <a name="methods"></a>Metodi  
  
```  
Append(IDispatch *Object);  
Delete(VARIANT Index);  
Refresh(void);  
```  
  
 Per ulteriori informazioni, vedere  
  
-   [Metodo Append (ADO)](./append-method-ado.md)  
  
-   [Metodo Delete (raccolta Parameters ADO)](./delete-method-ado-parameters-collection.md)  
  
-   [Metodo Refresh (ADO)](./refresh-method-ado.md)  
  
### <a name="properties"></a>Proprietà  
  
```  
get_Count(long *c);  
get_Item(VARIANT Index, _ADOParameter **ppvObject);  
```  
  
 Per ulteriori informazioni, vedere  
  
-   [Proprietà Count (ADO)](./count-property-ado.md)  
  
-   [Proprietà Item (ADO)](./item-property-ado.md)  
  
## <a name="fields"></a>Campi  
  
### <a name="methods"></a>Metodi  
  
```  
Append(BSTR bstrName, DataTypeEnum Type, long DefinedSize, FieldAttributeEnum Attrib);  
Delete(VARIANT Index);  
Refresh(void);  
```  
  
 Per ulteriori informazioni, vedere  
  
-   [Metodo Append (ADO)](./append-method-ado.md)  
  
-   [Metodo Delete (raccolta Parameters ADO)](./delete-method-ado-parameters-collection.md)  
  
-   [Metodo Refresh (ADO)](./refresh-method-ado.md)  
  
### <a name="properties"></a>Proprietà  
  
```  
get_Count(long *c);  
get_Item(VARIANT Index, ADOField **ppvObject);  
```  
  
 Per ulteriori informazioni, vedere  
  
-   [Proprietà Count (ADO)](./count-property-ado.md)  
  
-   [Proprietà Item (ADO)](./item-property-ado.md)  
  
## <a name="errors"></a>Errors  
  
### <a name="methods"></a>Metodi  
  
```  
Clear(void);  
Refresh(void);  
```  
  
 Per ulteriori informazioni, vedere  
  
-   [Metodo Clear (ADO)](./clear-method-ado.md)  
  
-   [Metodo Refresh (ADO)](./refresh-method-ado.md)  
  
### <a name="properties"></a>Proprietà  
  
```  
get_Count(long *c);  
get_Item(VARIANT Index, ADOError **ppvObject);  
```  
  
 Per ulteriori informazioni, vedere  
  
-   [Proprietà Count (ADO)](./count-property-ado.md)  
  
-   [Proprietà Item (ADO)](./item-property-ado.md)  
  
## <a name="properties"></a>Proprietà  
  
### <a name="methods"></a>Metodi  
  
```  
Refresh(void);  
```  
  
 Per ulteriori informazioni, vedere  
  
-   [Metodo Refresh (ADO)](./refresh-method-ado.md)  
  
### <a name="properties"></a>Proprietà  
  
```  
get_Count(long *c);  
get_Item(VARIANT Index, ADOProperty **ppvObject);  
```  
  
 Per ulteriori informazioni, vedere  
  
-   [Proprietà Count (ADO)](./count-property-ado.md)  
  
-   [Proprietà Item (ADO)](./item-property-ado.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Raccolta Errors (ADO)](./errors-collection-ado.md)   
 [Raccolta Fields (ADO)](./fields-collection-ado.md)   
 [Raccolta Parameters (ADO)](./parameters-collection-ado.md)   
 [Raccolta Properties (ADO)](./properties-collection-ado.md)