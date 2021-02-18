---
title: Disinstallare Reporting Services | Microsoft Docs
description: Questo articolo descrive come disinstallare Reporting Services, senza rimuovere il contenuto creato o la configurazione modificata.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 5c764a00-d4bc-465d-b32e-e4efce052ce4
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 650f22c78e0913d8b92c431de65963b802232141
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336053"
---
# <a name="uninstall-reporting-services"></a>Disinstallare Reporting Services
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  La disinstallazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] non rimuove il contenuto creato o la configurazione modificata. Se tuttavia è presente contenuto che sarà necessario dopo la disinstallazione, è consigliabile crearne copie prima di avviare il processo di disinstallazione.  
  
## <a name="uninstall-sharepoint-mode"></a>Disinstallare la modalità SharePoint  
 Quando si disinstalla la modalità SharePoint di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , vengono rimossi gli elementi seguenti:  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e proxy del servizio.  
  
-   File utilizzati per l'installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 Le applicazioni del servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] non vengono rimosse. Se le applicazioni del servizio non sono più necessarie, eliminarle mediante Windows PowerShell o Amministrazione centrale SharePoint.  
  
 Gli elementi del report e i metadati correlati non vengono rimossi. Queste informazioni sono contenute nei database del contenuto e di configurazione correlati alle applicazioni del servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . I database non vengono rimossi ed è possibile eseguirne manualmente la migrazione a un'altra installazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] in modalità SharePoint. Se le informazioni non sono più necessarie, eliminare i database. Per ulteriori informazioni, vedere [Upgrade and Migrate Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md).  
  
 Di seguito sono riportati nomi di esempio dei tre database di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] che non vengono rimossi.  
  
-   **Database del server di report:** ReportingService_7f616e2d253040e8ab5653b3c09a065e  
  
-   **Database temporaneo del server di report:** ReportingService_7f616e2d253040e8ab5653b3c09a065eTempDB  
  
-   **Database di avvisi del server di report:** ReportingService_7f616e2d253040e8ab5653b3c09a065e_Alerting  
  
### <a name="uninstall-the-add-in-for-sharepoint-products"></a>Disinstallare il componente aggiuntivo per Prodotti SharePoint.  
 Quando si disinstalla il componente aggiuntivo da un computer, è possibile scegliere di disinstallare solo i file o rimuovere anche la funzionalità [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dalla farm. Per informazioni dettagliate sull'installazione del componente aggiuntivo [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per i prodotti SharePoint, vedere [Installare o disinstallare il componente aggiuntivo Reporting Services per SharePoint](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md).  
  
## <a name="uninstall-native-mode"></a>Disinstallare la modalità nativa  
 Quando si disinstalla la modalità nativa [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , tutto ciò che è stato **creato** o **modificato** dopo l'installazione viene mantenuto. Ad esempio, file di database, file di log, file di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ed elementi di contenuto quali report e file di origine dati.  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] è una funzionalità dell'istanza, pertanto non è elencata in Programmi e funzionalità nel Pannello di controllo di Windows. Per disinstallare la modalità nativa [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
1.  Nel Pannello di controllo di Windows fare clic su **Programmi e funzionalità**.  
  
2.  In **Programmi e funzionalità** selezionare **Microsoft SQL Server 2016**.  
  
3.  Nella disinstallazione guidata selezionare l'istanza che include la funzionalità [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]**RS**.  
  
     ![rs_nativemode_uninstall_selectinstance](../../sql-server/install/media/rs-nativemode-uninstall-selectinstance.gif "rs_nativemode_uninstall_selectinstance")  
  
4.  Dopo aver selezionato l'istanza, selezionare la funzionalità [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
     ![rs_nativemode_uninstall_selectfeatures](../../sql-server/install/media/rs-nativemode-uninstall-selectfeatures.gif "rs_nativemode_uninstall_selectfeatures")  
  
5.  Completare la procedura guidata.  
  
## <a name="see-also"></a>Vedere anche  
 [Disinstallare un'istanza esistente di SQL Server &#40;Programma di installazione&#41;](../../sql-server/install/uninstall-an-existing-instance-of-sql-server-setup.md)   
 [Installare o disinstallare il componente aggiuntivo PowerPivot per SharePoint &#40;SharePoint 2013&#41;](/analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013)   
 [Installare o disinstallare il componente aggiuntivo Reporting Services per SharePoint](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md)  
  
