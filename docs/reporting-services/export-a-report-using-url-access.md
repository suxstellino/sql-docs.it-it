---
title: Esportare un report tramite l'accesso con URL | Microsoft Docs
description: Informazioni su come esportare un report tramite accesso con URL specificando il formato in cui eseguire il rendering di un report usando il parametro URL rs:Format.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- formats [Reporting Services], URL rendering
- URL access [Reporting Services], rendering formats
ms.assetid: 6a3b7fc3-3d91-4d12-8371-42ea12e74517
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d61f509fa07528b71cd7cbb23488bc14f33959ae
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472492"
---
# <a name="export-a-report-using-url-access"></a>Esportare un report tramite l'accesso con URL
  Facoltativamente, è possibile definire il formato di rendering di un report con il parametro URL *rs:Format* .  Il rendering dei formati HTML 4.0 e HTM5 (estensione per il rendering) viene eseguito nel browser e per gli altri formati il browser richiede di salvare l'output del report in un file locale.  
  
 Ad esempio per ottenere una copia PDF di un report direttamente da un server di report in modalità nativa:  
  
```  
https://myrshost/ReportServer?/myreport&rs:Format=PDF  
```  

::: moniker range="=sql-server-2016"
  
 Da un server di report in modalità integrata SharePoint:  
  
```  
https://myspsite/subsite/_vti_bin/reportserver?https://myspsite/subsite/myrereport.rdl&rs:Format=PDF  
```  
 
::: moniker-end
 
 Ad esempio, il comando URL seguente nel browser esporta un report PPTX da un'istanza denominata del server di report:  
  
```  
https://servername/ReportServer_THESQLINSTANCE/Pages/ReportViewer.aspx?%2freportfolder%2freport+name+with+spaces&rs:Format=pptx  
```  
  
 I valori validi per questo parametro sono basati sulle estensioni per il rendering di report installate nel server di report a cui si sta effettuando l'accesso. Le estensioni comuni sono HTML4.0, MHTML, IMAGE, EXCELOPENXML (xlsx), WORDOPENXML (docx), CSV, PDF, XML e NULL. Se un'estensione per il rendering specificata non è installata nel server di report, il rendering del report non viene eseguito e un errore viene generato e visualizzato nel browser.  
  
 Se non si include il parametro *Format* come parte dell'URL, il server di report rileva il browser ed esegue il rendering del report nel formato HTML appropriato.  
  
## <a name="see-also"></a>Vedere anche  
 [Accesso con URL &#40;SSRS&#41;](../reporting-services/url-access-ssrs.md)   
 [Riferimento ai parametri di accesso con URL](../reporting-services/url-access-parameter-reference.md)  
  
  
