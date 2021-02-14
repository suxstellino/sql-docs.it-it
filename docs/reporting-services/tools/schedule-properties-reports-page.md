---
title: Proprietà pianificazione (pagina Report) | Microsoft Docs
description: Informazioni sulla pagina delle proprietà di pianificazione di Reporting Services in SQL Server Management Studio, in cui sono elencati tutti i report destinati a una specifica pianificazione condivisa.
ms.date: 06/30/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
f1_keywords:
- sql13.swb.reportserver.scheduleproperties.reports.f1
ms.assetid: 7db728bd-4b08-43ef-a49a-e8dcdd37cf89
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 132aa67a1542e0a175fb08aed4c9a9a798c779d6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077332"
---
# <a name="schedule-properties-reports-page"></a>Proprietà pianificazione (pagina Report)
  Usare la pagina delle proprietà di pianificazione [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] in [!INCLUDE[ssManStudioFull_md](../../includes/ssmanstudiofull-md.md)] per vedere l'elenco di tutti i report che usano quella specifica pianificazione condivisa. È possibile utilizzare le pianificazioni per aggiornare lo snapshot o generare la cronologia del report, attivare una sottoscrizione oppure specificare la data e l'ora di scadenza di una copia del report memorizzata nella cache. Per individuare la modalità di utilizzo della pianificazione, visualizzare le informazioni sulle proprietà e sulla sottoscrizione del report.  
  
 Anche se in questa pagina è illustrato ogni report che utilizza la pianificazione condivisa, non viene indicato quante volte la pianificazione condivisa è utilizzata all'interno di un singolo report. Si supponga, ad esempio che 20 diversi sottoscrittori del report Company Sales utilizzino tutti la stessa pianificazione condivisa per attivare l'elaborazione della sottoscrizione. In questo caso, il report Company Sales verrà indicato una sola volta nell'elenco, anche se nel report sono presenti 20 riferimenti alla pianificazione condivisa.  
  
 Per aprire la pagina delle proprietà di pianificazione:
 1. Avviare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].
 2. Connettersi a un'istanza di un server di report.
 3. Aprire la cartella **Pianificazioni condivise** .
 4. Fare clic con il pulsante destro del mouse su una pianificazione condivisa e selezionare **Proprietà**.
 5. Fare clic su **Report**.  
  
  È possibile gestire le pianificazioni condivise anche da **Impostazioni sito** del portale Web [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] .
  
> [!NOTE]  
>  Questa funzionalità non è disponibile in ogni edizione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per un elenco delle funzionalità supportate dalle edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere [Edizioni e funzionalità supportate di SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).  
  
## <a name="options"></a>Opzioni  
 **Cartella**  
 Consente di specificare il percorso del report.  
  
 **Report**  
 Consente di specificare il nome del report che utilizza la pianificazione.  
  
## <a name="see-also"></a>Vedere anche  
 [Create, Modify, and Delete Schedules](../../reporting-services/subscriptions/create-modify-and-delete-schedules.md)   
 [Pianificazioni](../../reporting-services/subscriptions/schedules.md)   
 [Guida sensibile al contesto del server di report in Management Studio](../../reporting-services/tools/report-server-in-management-studio-f1-help.md)   
 [Eseguire la connessione a un server di report in Management Studio](../../reporting-services/tools/connect-to-a-report-server-in-management-studio.md)   
 [Configurare le proprietà generali per un report (Gestione report)](../reports/configure-execution-properties-for-a-report-report-manager.md)  
  
