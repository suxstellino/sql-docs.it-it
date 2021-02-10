---
description: ConnectOptionEnum
title: ConnectOptionEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ConnectOptionEnum
helpviewer_keywords:
- ConnectOptionEnum enumeration [ADO]
ms.assetid: bff07eeb-dee3-4e4e-9b2d-d56061ea744d
author: rothja
ms.author: jroth
ms.openlocfilehash: e159309a0fb1875851ff6ae0a7bf56ade908bafd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100026076"
---
# <a name="connectoptionenum"></a>ConnectOptionEnum
Specifica se il metodo [Open](./open-method-ado-connection.md) di un oggetto [Connection](./connection-object-ado.md) deve essere restituito dopo che la connessione è stata stabilita (in modalità sincrona) o prima (in modo asincrono).  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adAsyncConnect**|16|Apre la connessione in modo asincrono. L'evento [ConnectComplete](./connectcomplete-and-disconnect-events-ado.md) può essere utilizzato per determinare quando la connessione è disponibile.|  
|**adConnectUnspecified**|-1|Valore predefinito. Apre la connessione in modo sincrono.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Pacchetto: **com. ms. wfc. Data**  
  
|Costante|  
|--------------|  
|AdoEnums. ConnectOption. ASYNCCONNECT|  
|AdoEnums. ConnectOption. CONNECTUNSPECIFIED|  
  
## <a name="applies-to"></a>Si applica a  
 [Metodo Open (Connection - ADO)](./open-method-ado-connection.md)