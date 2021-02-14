---
title: Creazione di una libreria di estensioni per il recapito | Microsoft Docs
description: Informazioni su come assegnare un'estensione per il recapito creata in Reporting Services a uno spazio dei nomi univoco e integrarla in un file di assembly o di libreria.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- delivery extensions [Reporting Services], namespace assignments
- library [Reporting Services]
- assigning namespaces to extensions
ms.assetid: 63b32f93-4bab-4b07-bd72-39a47aca1cda
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 390b35970b58c4348d9ebd35e1b67540db37222c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065343"
---
# <a name="creating-a-delivery-extension-library"></a>Creazione di una libreria di estensioni per il recapito
  A ogni estensione per il recapito di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] creata deve essere assegnato uno spazio dei nomi univoco e ogni estensione deve essere inclusa in una libreria o in un file di assembly. Il nome esatto dello spazio dei nomi non è importante, ma è necessario che sia univoco e non condiviso con altre estensioni. È necessario creare spazi dei nomi univoci personalizzati per le estensioni per il recapito della società.  
  
 Nell'esempio seguente viene illustrato il codice per iniziare a creare un'estensione per il recapito di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] che utilizza gli spazi dei nomi contenenti le interfacce per il recapito e le classi di utilità.  
  
```vb  
Imports System  
Imports Microsoft.ReportingServices.Interfaces  
  
Namespace CompanyName.ExtensionName  
   ...  
```  
  
```csharp  
using System;  
using Microsoft.ReportingServices.Interfaces;  
  
namespace CompanyName.ExtensionName  
{  
   ...  
```  
  
 Quando si compila un'estensione per il recapito di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], è necessario fornire al compilatore un riferimento a Microsoft.ReportingServices.Interfaces.dll, in quanto le classi e le interfacce dell'estensione per il recapito sono incluse in tale elemento. Lo spazio dei nomi <xref:Microsoft.ReportingServices.Interfaces> è necessario per implementare l'interfaccia <xref:Microsoft.ReportingServices.Interfaces.IExtension>, l'interfaccia <xref:Microsoft.ReportingServices.Interfaces.IDeliveryExtension> e altro ancora. Se ad esempio tutti i file che contengono il codice per implementare un'estensione per il recapito [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] scritti in C# fossero inclusi in una singola directory con estensione cs, da tale directory verrebbe inviato il comando seguente per compilare i file archiviati in CompanyName.ExtensionName.dll.  
  
```csharp  
csc /t:library /out:CompanyName.ExtensionName.dll *.cs /r:System.dll   
/r:Microsoft.ReportingServices.Interfaces.dll  
```  
  
 L'esempio di codice seguente visualizza il comando che verrebbe usato per i file [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] con estensione vb.  
  
```vb  
vbc /t:library /out:CompanyName.ExtensionName.dll *.vb /r:System.dll   
/r:Microsoft.ReportingServices.Interfaces.dll  
```  
  
> [!NOTE]  
>  È inoltre possibile progettare, sviluppare e compilare un'estensione per il recapito utilizzando [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)]. Per ulteriori informazioni sullo sviluppo di assembly in [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)], vedere la documentazione di [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)].  
  
## <a name="see-also"></a>Vedere anche  
 [Estensioni di Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Implementazione di un'estensione per il recapito](../../../reporting-services/extensions/delivery-extension/implementing-a-delivery-extension.md)   
 [Libreria di estensioni di Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
