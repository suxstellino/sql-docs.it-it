---
title: Creazione di una libreria di estensioni per l'elaborazione dati | Microsoft Docs
description: Informazioni su come creare un'estensione per l'elaborazione dati di Reporting Services. Visualizzare il codice di esempio ed esaminare i requisiti dello spazio dei nomi e del file di libreria che è necessario soddisfare.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- data processing extensions [Reporting Services], namespace assignments
- library [Reporting Services]
- assigning namespaces to extensions
ms.assetid: 82f4b71b-dd39-467d-8d8c-6771eb2b12de
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bd55657e325035821798c2b7d6a960a6f78cd59b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065446"
---
# <a name="creating-a-data-processing-extension-library"></a>Creazione di una libreria di estensioni per l'elaborazione dati
  A ogni estensione per l'elaborazione dati di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] creata deve essere assegnato uno spazio dei nomi univoco e ogni estensione deve essere compilata in una libreria o in un file di assembly. Il nome esatto dello spazio dei nomi non è importante, ma è necessario che sia univoco e non condiviso con altre estensioni. [!INCLUDE[msCoName](../../../includes/msconame-md.md)] utilizza lo spazio dei nomi <xref:Microsoft.ReportingServices.DataProcessing> per le estensioni per l'elaborazione dati disponibili in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. È necessario creare spazi dei nomi univoci personalizzati per le estensioni per l'elaborazione dati della società.  
  
 Nell'esempio seguente viene illustrato il codice per iniziare a creare un'estensione per l'elaborazione dati di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] che utilizza gli spazi dei nomi contenenti le interfacce per l'elaborazione dati e le classi di utilità.  
  
```vb  
Imports System  
Imports Microsoft.ReportingServices.DataProcessing  
Imports Microsoft.ReportingServices.Interfaces  
  
Namespace CompanyName.ExtensionName  
   ...  
```  
  
```csharp  
using System;  
using Microsoft.ReportingServices.DataProcessing;  
using Microsoft.ReportingServices.Interfaces;  
  
namespace CompanyName.ExtensionName  
{  
   ...  
```  
  
 Quando si compila un'estensione per l'elaborazione dati di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], è necessario fornire al compilatore un riferimento a Microsoft.ReportingServices.Interfaces.dll, in quanto le interfacce dell'estensione per l'elaborazione dati sono incluse in tale elemento. Lo spazio dei nomi <xref:Microsoft.ReportingServices.DataProcessing> è necessario per implementare le interfacce dell'estensione per l'elaborazione dati, mentre lo spazio dei nomi <xref:Microsoft.ReportingServices.Interfaces> è necessario per implementare l'interfaccia <xref:Microsoft.ReportingServices.Interfaces.IExtension>. Se, ad esempio, tutti i file che contengono il codice per implementare un'estensione per l'elaborazione dati di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] scritti in C# fossero inclusi in una singola directory con estensione cs, da tale directory verrebbe inviato il comando seguente per compilare i file archiviati in CompanyName.ExtensionName.dll.  
  
```csharp  
csc /t:library /out:CompanyName.ExtensionName.dll *.cs /r:System.dll /r:Microsoft.ReportingServices.Interfaces.dll  
```  
  
 L'esempio di codice seguente visualizza il comando che verrebbe usato per i file [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] con estensione vb.  
  
```vb  
vbc /t:library /out:CompanyName.ExtensionName.dll *.vb /r:System.dll /r:Microsoft.ReportingServices.Interfaces.dll  
```  
  
> [!NOTE]  
>  È inoltre possibile progettare, sviluppare e compilare un'estensione per l'elaborazione dati utilizzando [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)]. Per ulteriori informazioni sullo sviluppo di assembly in [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)], vedere la documentazione di [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)].  
  
## <a name="see-also"></a>Vedere anche  
 [Estensioni di Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Implementazione di un'estensione per l'elaborazione dati](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Libreria di estensioni di Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
