---
description: ConnectPromptEnum
title: ConnectPromptEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ConnectPromptEnum
helpviewer_keywords:
- ConnectPromptEnum enumeration [ADO]
ms.assetid: 21026e24-62b7-4cc9-8aef-62c1fc6cba75
author: rothja
ms.author: jroth
ms.openlocfilehash: 7563179d70968cc98fbbc1055ec32c2c3a1948f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167700"
---
# <a name="connectpromptenum"></a>ConnectPromptEnum
Specifica se deve essere visualizzata una finestra di dialogo per richiedere parametri mancanti quando si apre una connessione a un'origine dati.  
  
|Costante|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|**adPromptAlways**|1|Richiede sempre.|  
|**adPromptComplete**|2|Richiede se sono necessarie altre informazioni.|  
|**adPromptCompleteRequired**|3|Richiede se sono necessarie altre informazioni ma non sono consentiti parametri facoltativi.|  
|**adPromptNever**|4|Non richiede mai.|  
  
## <a name="adowfc-equivalent"></a>Equivalente ADO/WFC  
 Pacchetto: **com. ms. wfc. Data**  
  
|Costante|  
|--------------|  
|AdoEnums. ConnectPrompt. ALWAYs|  
|AdoEnums. ConnectPrompt. COMPLETE|  
|AdoEnums. ConnectPrompt. COMPLETEREQUIRED|  
|AdoEnums. ConnectPrompt. NEVER|  
  
## <a name="applies-to"></a>Si applica a  
 [Proprietà dinamica Prompt (ADO)](./prompt-property-dynamic-ado.md)