---
description: Proprietà Source (Error - ADO)
title: Proprietà Source (errore ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Error::get_Source
- Error::Source
- Error::GetSource
helpviewer_keywords:
- Source property [ADO Error]
ms.assetid: 4044ba15-f013-4c4c-9fe1-b4410fe9a778
author: rothja
ms.author: jroth
ms.openlocfilehash: b468c7b99d69e9da026a462b0c33084a4880eea3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051412"
---
# <a name="source-property-ado-error"></a>Proprietà Source (Error - ADO)
Indica il nome dell'oggetto o dell'applicazione che ha generato originariamente un errore.  
  
## <a name="return-value"></a>Valore restituito  
 Restituisce un valore **stringa** che indica il nome di un oggetto o di un'applicazione.  
  
## <a name="remarks"></a>Commenti  
 Utilizzare la proprietà **source** in un oggetto [Error](./error-object.md) per determinare il nome dell'oggetto o dell'applicazione che ha generato originariamente un errore. Potrebbe trattarsi del nome della classe o dell'ID a livello di codice dell'oggetto. Per gli errori in ADO, il valore della proprietà sarà **ADODB.** _ObjectName_, dove *nomeoggetto* è il nome dell'oggetto che ha generato l'errore. Per ADOX e ADO MD, il valore sarà **ADOX.** _ObjectName_ e **ADOMD.** _ObjectName_, rispettivamente.  
  
 In base alla documentazione degli errori delle proprietà di **origine**, [numero](./number-property-ado.md)e [Descrizione](./description-property.md) degli oggetti **errore** , è possibile scrivere codice che gestirà l'errore in modo appropriato.  
  
 La proprietà di **origine** è di sola lettura per gli oggetti **Error** .  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Error](./error-object.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di proprietà Description, HelpContext, filelima, NativeError, Number, source e SQLState (VB)](./description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Esempio di proprietà Description, HelpContext, filelima, NativeError, Number, source e SQLState (VC + +)](./description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)   
 [Description (proprietà)](./description-property.md)   
 [HelpContext, proprietà di filelima](./helpcontext-helpfile-properties.md)   
 [Proprietà Number (ADO)](./number-property-ado.md)   
 [Proprietà Source (record ADO)](./source-property-ado-record.md)   
 [Proprietà Source (Recordset - ADO)](./source-property-ado-recordset.md)