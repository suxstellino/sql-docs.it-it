---
description: XactAttributeEnum
title: XactAttributeEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- XactAttributeEnum
helpviewer_keywords:
- XactAttributeEnum enumeration [ADO]
ms.assetid: e7dcecd3-7dc7-445c-b922-f700c3067fbc
author: rothja
ms.author: jroth
ms.openlocfilehash: e741ec0e5458218ddb6dc3a46c3a098acf482611
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056026"
---
# <a name="xactattributeenum"></a>XactAttributeEnum
Specifica gli attributi di transazione di un oggetto [Connection](./connection-object-ado.md) .  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adXactAbortRetaining**|262144|Esegue le interruzioni di conservazione chiamando [RollbackTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) per avviare automaticamente una nuova transazione. Questo comportamento non è supportato da tutti i provider.|  
|**adXactCommitRetaining**|131072|Esegue i commit di conservazione chiamando [CommitTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) per avviare automaticamente una nuova transazione. Questo comportamento non è supportato da tutti i provider.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Pacchetto: **com. ms. wfc. Data**  
  
|Costante|  
|--------------|  
|AdoEnums. XactAttribute. ABORTRETAINING|  
|AdoEnums. XactAttribute. COMMITRETAINING|  
  
## <a name="applies-to"></a>Si applica a  
 [Proprietà Attributes (ADO)](./attributes-property-ado.md)