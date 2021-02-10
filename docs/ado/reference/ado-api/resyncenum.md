---
description: ResyncEnum
title: ResyncEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ResyncEnum
helpviewer_keywords:
- ResyncEnum enumeration [ADO]
ms.assetid: d3df2c90-e570-4c40-a79a-25b3448a009c
author: rothja
ms.author: jroth
ms.openlocfilehash: db02c56f55b577c9f74a42f43abc9a1957311c0d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040751"
---
# <a name="resyncenum"></a>ResyncEnum
Specifica se i valori sottostanti vengono sovrascritti da una chiamata alla [Risincronizzazione](./resync-method.md).  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adResyncAllValues**|2|Valore predefinito. Sovrascrive i dati e gli aggiornamenti in sospeso vengono annullati.|  
|**adResyncUnderlyingValues**|1|Non sovrascrive i dati e gli aggiornamenti in sospeso non vengono annullati.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Pacchetto: **com. ms. wfc. Data**  
  
|Costante|  
|--------------|  
|AdoEnums. Resync. ALLVALUES|  
|AdoEnums. Resync. UNDERLYINGVALUES|  
  
## <a name="applies-to"></a>Si applica a  
 [Metodo Resync](./resync-method.md)