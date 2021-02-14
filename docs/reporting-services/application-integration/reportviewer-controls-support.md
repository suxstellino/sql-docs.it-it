---
title: Supporto per le versioni del controllo Report Viewer
description: Il controllo Microsoft Report Viewer è compatibile con SQL Server Reporting Services e Server di report di Power BI che seguono i moderni criteri del ciclo di vita del supporto.
author: maggiesMSFT
ms.custom: seo-lt-2019
ms.author: maggies
ms.reviewer: jonhp
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: application-integration
ms.topic: reference
ms.date: 12/01/2020
ms.openlocfilehash: fac7ebfc88432ef3bd8abde6c0e4e62115311520
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015641"
---
# <a name="support-for-report-viewer-current-branch-versions"></a>Supporto per le versioni Current Branch di Report Viewer

**_Si applica a: Microsoft Report Viewer 150.900.148 e versioni successive_**

Il **controllo Microsoft Report Viewer** è compatibile con SQL Server Reporting Services e Server di report di Power BI che seguono i moderni [criteri del ciclo di vita del supporto](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy) di Microsoft. Queste informazioni si applicano alle versioni **ASP.NET** e **WinForms** distribuite con [NuGet](https://www.nuget.org/). Tutte le versioni rilasciate sono disponibili tramite [NuGet](https://www.nuget.org/). Per le patch, le funzionalità e gli altri aggiornamenti viene eseguito il rollforward alla versione più recente. Per ricevere le modifiche, è necessario applicare le versioni più recenti. Report Viewer continua a ricevere **aggiornamenti per la sicurezza e critici** con un preavviso di almeno un anno nel caso di modifiche ai criteri di supporto.

Per una cronologia delle versioni del controllo Report Viewer, vedere i collegamenti seguenti:

- [WinForms](https://www.nuget.org/packages/Microsoft.ReportingServices.ReportViewerControl.Winforms/)
- [Web Form ASP.NET](https://www.nuget.org/packages/Microsoft.ReportingServices.ReportViewerControl.WebForms/)

## <a name="application-server-and-report-server-combinations"></a>Combinazioni di server applicazioni e server di report

Alcune funzionalità del controllo Visualizzatore di report si basano sui comportamenti predefiniti del sistema operativo. Può quindi essere necessario eseguire la stessa versione sia per il client (il server applicazioni che esegue il controllo Visualizzatore di report), sia per il server (in cui viene eseguito Reporting Services). Sono supportate le seguenti combinazioni di server applicazioni e server di report:

| Server applicazioni | Server di report |
| :----------------- | :------ |
| Windows Server 2012 | Windows Server 2012 |
| Windows Server 2012 | R2 per Windows Server 2012 |
| R2 per Windows Server 2012 | R2 per Windows Server 2012 |
| R2 per Windows Server 2012 | Windows Server 2012 |
| Windows Server 2016 e versioni successive | Windows Server 2016 e versioni successive |

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sul controllo Visualizzatore di report, vedere la [guida introduttiva all'integrazione di Reporting Services con i controlli Visualizzatore di report](integrating-reporting-services-using-reportviewer-controls-get-started.md).
