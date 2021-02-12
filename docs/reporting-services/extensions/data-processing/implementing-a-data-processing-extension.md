---
title: Implementazione di un'estensione per l'elaborazione dati | Microsoft Docs
description: Informazioni su come creare un ponte tra un'origine dati e un set di dati in Reporting Services implementando un'estensione per l'elaborazione dati.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- custom data processing extensions [Reporting Services]
- data sources [Reporting Services], data processing extensions
- data processing extensions [Reporting Services]
- extensions [Reporting Services], data processing
ms.assetid: 8dc2b44e-5ad9-411d-a29f-7213e29321a9
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f4d4102c44cfedd25ed6a9870a303ebafb2515cf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065408"
---
# <a name="implementing-a-data-processing-extension"></a>Implementazione di un'estensione per l'elaborazione dati
  Le estensioni per l'elaborazione dati in [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] consentono di eseguire la connessione a un'origine dati e di recuperare i dati. Fungono inoltre da ponte tra un'origine dati e un set di dati. Le estensioni per l'elaborazione dati di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] sono modellate in base a un subset delle interfacce dei provider di dati [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)].  
  
## <a name="in-this-section"></a>Contenuto della sezione  
 [Panoramica delle estensioni per l'elaborazione dati](../../../reporting-services/extensions/data-processing/data-processing-extensions-overview.md)  
 Vengono fornite informazioni introduttive per la scrittura di un'estensione per l'elaborazione dati per [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
 [Preparazione all'implementazione di un'estensione per l'elaborazione dati](../../../reporting-services/extensions/data-processing/preparing-to-implement-a-data-processing-extension.md)  
 Vengono descritte le interfacce disponibili quando si implementa un'estensione per l'elaborazione dati di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] e vengono indicati i casi in cui è necessario implementare un'interfaccia specifica.  
  
 [Creazione di una libreria di estensioni per l'elaborazione dati](../../../reporting-services/extensions/data-processing/creating-a-data-processing-extension-library.md)  
 Vengono descritte le operazioni di assegnazione di uno spazio dei nomi per l'estensione per l'elaborazione dati di [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] e di compilazione dell'estensione per l'elaborazione dati nella DLL di una libreria.  
  
 [Implementazione di una classe Connection per un'estensione per l'elaborazione dati](../../../reporting-services/extensions/data-processing/implementing-a-connection-class-for-a-data-processing-extension.md)  
 Vengono descritti gli attributi di una connessione e come implementare una classe **Connection** personalizzata per l'estensione per l'elaborazione dati.  
  
 [Implementazione di una classe Command per un'estensione per l'elaborazione dati](../../../reporting-services/extensions/data-processing/implementing-a-command-class-for-a-data-processing-extension.md)  
 Vengono descritti gli attributi di un comando e come implementare una classe **Command** personalizzata per l'estensione per l'elaborazione dati.  
  
 [Implementazione di una classe DataReader per un'estensione per l'elaborazione dati](../../../reporting-services/extensions/data-processing/implementing-a-datareader-class-for-a-data-processing-extension.md)  
 Vengono descritti gli attributi di un lettore di dati e come implementare una classe **DataReader** personalizzata per l'estensione per l'elaborazione dati.  
  
 [Uso di un set di dati esterno con Reporting Services](../../../reporting-services/extensions/data-processing/using-an-external-dataset-with-reporting-services.md)  
 Viene descritto come esporre gli oggetti **DataSet** personalizzati nel server di report per l'uso.  
  
 [Distribuzione di un'estensione per l'elaborazione dati](../../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension.md)  
 Viene descritto come distribuire un'estensione per l'elaborazione dati.  
  
 [Debug del codice di un'estensione per l'elaborazione dati](../../../reporting-services/extensions/data-processing/debugging-data-processing-extension-code.md)  
 Viene descritto come eseguire il debug del codice nelle estensioni per l'elaborazione dati.  
  
 Per un esempio di estensione per l'elaborazione dati completamente implementata, vedere la pagina degli [esempi del prodotto SQL Server Reporting Services](https://go.microsoft.com/fwlink/?LinkId=177889).  
  
## <a name="see-also"></a>Vedere anche  
 [Estensioni di Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Libreria di estensioni di Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
