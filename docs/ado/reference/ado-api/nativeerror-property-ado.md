---
description: Proprietà NativeError (ADO)
title: Proprietà NativeError (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Error::GetNativeError
- Error::get_NativeError
- Error::NativeError
helpviewer_keywords:
- NativeError property [ADO]
ms.assetid: b9b47e57-18a4-4186-aef5-5bd18d7b1d74
author: rothja
ms.author: jroth
ms.openlocfilehash: 10ed79bf5629ce6439362ce1e80226444928a16b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041591"
---
# <a name="nativeerror-property-ado"></a>Proprietà NativeError (ADO)
Indica il codice di errore specifico del provider per un determinato oggetto [Error](./error-object.md) .  
  
## <a name="return-value"></a>Valore restituito  
 Restituisce un valore **Long** che indica il codice di errore.  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **NativeError** per recuperare le informazioni sull'errore specifiche del database per un determinato oggetto **Error** . Ad esempio, quando si utilizza il provider ODBC Microsoft per OLE DB con un database Microsoft SQL Server, i codici di errore nativi che provengono da SQL Server passano attraverso ODBC e il provider ODBC alla proprietà ADO **NativeError** .  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Error](./error-object.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà Description, HelpContext, filelima, NativeError, Number, source e SQLState (VB)](./description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Esempio di proprietà Description, HelpContext, filelima, NativeError, Number, source e SQLState (VC + +)](./description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)