---
title: Proprietà Description | Microsoft Docs
description: Informazioni sulla proprietà Description dell'oggetto Error in ADO che restituisce un valore stringa contenente una descrizione dell'errore.
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Error::Description
- Error::GetDescription
- Error::get_Description
helpviewer_keywords:
- Description property
ms.assetid: 4b5d6790-6c29-42aa-bf78-d9cfb8ad7965
author: rothja
ms.author: jroth
ms.openlocfilehash: 7181bffdad12291c08bf4fb5c6a00b4b1b2a6cc5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034311"
---
# <a name="description-property"></a>Proprietà Description
Descrive un oggetto [Error](../../../ado/reference/ado-api/error-object.md) .  
  
## <a name="return-value"></a>Valore restituito  
 Restituisce un valore **stringa** che contiene una descrizione dell'errore.  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **Description** per ottenere una breve descrizione dell'errore. Visualizzare questa proprietà per avvisare l'utente di un errore che non è possibile o non si desidera gestire. La stringa proverrà da ADO o da un provider.  
  
 I provider sono responsabili del passaggio di un testo di errore specifico a ADO. ADO aggiunge un oggetto [Error](../../../ado/reference/ado-api/error-object.md) **alla raccolta Errors** per ogni errore o avviso del provider ricevuto. Enumerare la raccolta **Errors** per tracciare gli errori passati dal provider.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Error](../../../ado/reference/ado-api/error-object.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà Description, HelpContext, filelima, NativeError, Number, source e SQLState (VB)](../../../ado/reference/ado-api/description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Esempio di proprietà Description, HelpContext, filelima, NativeError, Number, source e SQLState (VC + +)](../../../ado/reference/ado-api/description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)   
 [HelpContext, proprietà di filelima](../../../ado/reference/ado-api/helpcontext-helpfile-properties.md)   
 [Proprietà Number (ADO)](../../../ado/reference/ado-api/number-property-ado.md)   
 [Proprietà Source (Error - ADO)](../../../ado/reference/ado-api/source-property-ado-error.md)
