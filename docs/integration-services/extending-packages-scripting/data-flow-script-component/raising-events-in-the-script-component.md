---
description: Generazione di eventi nel componente script
title: Generazione di eventi nel componente script | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- Script component [Integration Services], raising events
ms.assetid: bb389073-e1d0-4794-8d29-c8b293b6a5e3
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7881dcb05fb615b4045efb505a164496e7a8ef5f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354955"
---
# <a name="raising-events-in-the-script-component"></a>Generazione di eventi nel componente script

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Gli eventi consentono di segnalare errori, avvisi e altre informazioni, ad esempio l'avanzamento o lo stato delle attività, al pacchetto contenitore. Il pacchetto fornisce gestori eventi per la gestione di notifiche degli eventi. Il componente script può generare eventi chiamando metodi sulla proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ComponentMetaData%2A> della classe **ScriptMain**. Per altre informazioni sulla gestione degli eventi da parte dei pacchetti di [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)], vedere [Gestori eventi di Integration Services &#40;SSIS&#41;](../../../integration-services/integration-services-ssis-event-handlers.md).  
  
 Gli eventi possono essere registrati in qualsiasi provider di log abilitato nel pacchetto. I provider di log archiviano informazioni sugli eventi in un archivio dati. Il componente script può anche utilizzare il metodo <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.Log%2A> per registrare informazioni in un provider di log senza generare un evento. Per ulteriori informazioni sull'utilizzo del metodo <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.Log%2A>, vedere la sezione seguente.  
  
 Per generare un evento, l'attività Script chiama uno dei metodi seguenti dell'interfaccia <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> esposta dalla proprietà <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ComponentMetaData%2A>:  
  
|Event|Descrizione|  
|-----------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireCustomEvent%2A>|Genera un evento personalizzato definito dall'utente nel pacchetto.|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireError%2A>|Informa il pacchetto di una condizione di errore.|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireInformation%2A>|Fornisce informazioni all'utente.|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireProgress%2A>|Informa il pacchetto dello stato del componente.|  
|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireWarning%2A>|Informa il pacchetto che il componente è in uno stato che garantisce la notifica all'utente, ma non è una condizione di errore.|  
  
 Di seguito è riportato un semplice esempio di generazione di un evento Error:  
  
 `Dim myMetadata as IDTSComponentMetaData100`  
  
 `myMetaData = Me.ComponentMetaData`  
  
 `myMetaData.FireError(...)`  
  
## <a name="see-also"></a>Vedere anche  
 [Gestori eventi di Integration Services &#40;SSIS&#41;](../../../integration-services/integration-services-ssis-event-handlers.md)   
 [Aggiunta di un gestore eventi a un pacchetto](../../integration-services-ssis-event-handlers.md)  
  
