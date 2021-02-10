---
description: Tipi di eventi
title: Tipi di eventi | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- EventComplete event [ADO]
- events [ADO], types
- Will events [ADO]
- complete events [ADO]
- WillEvent event [ADO]
ms.assetid: f3327ea0-635a-43d4-bd78-c1674f62f1a2
author: rothja
ms.author: jroth
ms.openlocfilehash: 9da4e5cefa1c33a2a1b4b5002dc9030550265ca3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032343"
---
# <a name="types-of-events"></a>Tipi di eventi
Esistono due tipi di eventi di base. "Eventi", chiamati prima dell'avvio di un'operazione, in genere includono "Testamento" nei rispettivi nomi, ad esempio **WillChangeRecordset** o **WillConnect**. Gli eventi che vengono chiamati dopo il completamento di un evento in genere includono "complete" nei rispettivi nomi, ad esempio **RecordChangeComplete** o **ConnectComplete**. Sono presenti eccezioni, ad esempio **InfoMessage** , ma queste si verificano dopo il completamento dell'operazione associata.  
  
## <a name="will-events"></a>Eventi  
 I gestori di eventi chiamati prima dell'avvio dell'operazione offrono la possibilità di esaminare o modificare i parametri dell'operazione e quindi di annullare l'operazione o di consentirne il completamento. In genere, queste routine del gestore eventi hanno nomi <strong>di form.</strong>  
  
## <a name="complete-events"></a>Eventi completi  
 I gestori di eventi chiamati dopo il completamento di un'operazione possono notificare all'applicazione che un'operazione è stata conclusa. Un gestore eventi di questo tipo viene inoltre informato quando un gestore eventi di un evento annulla un'operazione in sospeso. Queste routine del gestore eventi hanno in genere nomi dell' <strong> *evento*</strong>del modulo completato.  
  
 Gli eventi e completi vengono in genere utilizzati nelle coppie.  
  
## <a name="other-events"></a>Altri eventi  
 Gli altri gestori eventi, ovvero gli eventi i cui nomi non hanno il formato, <strong>verranno chiamati *evento*</strong> o <strong> *evento* completo</strong> , ma solo dopo il completamento di un'operazione. Questi eventi sono **Disconnect**, **EndOfRecordset** e **InfoMessage**.  
  
## <a name="see-also"></a>Vedere anche  
 [Riepilogo del gestore eventi ADO](../../../ado/guide/data/ado-event-handler-summary.md)   
 [Creazione di un'istanza di evento ADO per lingua](../../../ado/guide/data/ado-event-instantiation-by-language.md)   
 [Parametri evento](../../../ado/guide/data/event-parameters.md)   
 [Interazione tra i gestori eventi](../../../ado/guide/data/how-event-handlers-work-together.md)
