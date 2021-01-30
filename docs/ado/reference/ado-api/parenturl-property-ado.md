---
description: Proprietà ParentURL (ADO)
title: Proprietà ParentURL (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Record::ParentURL
helpviewer_keywords:
- ParentURL property [ADO]
ms.assetid: 65120ce6-3900-4cd4-b322-3b9816d74737
author: rothja
ms.author: jroth
ms.openlocfilehash: 89b3caf606d47c2be1add80a20830570be3f7fd2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166864"
---
# <a name="parenturl-property-ado"></a>Proprietà ParentURL (ADO)
Indica una stringa URL assoluta che punta al [record](./record-object-ado.md) padre dell'oggetto **record** corrente.  
  
## <a name="return-value"></a>Valore restituito  
 Restituisce un valore **stringa** che indica l'URL del **record** padre.  
  
## <a name="remarks"></a>Commenti  
 La proprietà **ParentURL** dipende dall'origine utilizzata per aprire l'oggetto **record** . Ad esempio, è possibile aprire il **record** con un'origine contenente un percorso relativo di una directory a cui fa riferimento la proprietà [ActiveConnection](./activeconnection-property-ado.md) .  
  
 Si supponga che "Second" sia una cartella contenuta in "First". Aprire l'oggetto **record** utilizzando la sintassi seguente:  
  
```  
record.ActiveConnection = "https://first"  
record.Open "second"  
```  
  
 A questo punto, il valore della `the` proprietà **ParentURL** è `"https://first"` uguale a **ActiveConnection**.  
  
 L'origine può anche essere un URL assoluto, ad esempio, `"https://first/second"` . La proprietà **ParentURL** è quindi `"https://first"` il livello precedente `"second"` .  
  
 Questa proprietà può essere un valore null se:  
  
-   Nessun elemento padre per l'oggetto corrente. ad esempio, se l'oggetto **record** rappresenta la radice di una directory.  
  
-   L'oggetto **record** rappresenta un'entità che non può essere specificata con un URL.  
  
 Questa proprietà è di sola lettura.  
  
> [!NOTE]
>  Questa proprietà è supportata solo dai provider di origine del documento, ad esempio il [provider Microsoft OLE DB per Internet Publishing](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Per ulteriori informazioni, vedere [record e campi Provider-Supplied](../../guide/data/records-and-provider-supplied-fields.md).  
  
> [!NOTE]
>  Gli URL che usano lo schema http richiameranno automaticamente il [provider di Microsoft OLE DB per la pubblicazione Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Per ulteriori informazioni, vedere [URL assoluto e relativo](../../guide/data/absolute-and-relative-urls.md).  
  
> [!NOTE]
>  Se il record corrente contiene un record di dati di un **Recordset** ADO, l'accesso alla proprietà **ParentURL** genera un errore di run-time, a indicare che non è possibile alcun URL.  
  
## <a name="applies-to"></a>Si applica a  
 [Oggetto Record (ADO)](./record-object-ado.md)