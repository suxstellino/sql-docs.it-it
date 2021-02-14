---
description: Configurare notifiche di posta elettronica (Master Data Services)
title: Configurare le notifiche tramite posta elettronica
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- e-mail [Master Data Services], configuring
- notifications [Master Data Services], configuring notifications
ms.assetid: 4241a6ab-7465-471b-9890-57c6b572037e
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 06ca29b0a912a1a2a59c8f6b901edacce742cd6f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272762"
---
# <a name="configure-email-notifications-master-data-services"></a>Configurare notifiche di posta elettronica (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Per fare in modo che [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] invii automaticamente messaggi di posta elettronica, è necessario configurare messaggi di posta elettronica di notifica.  
  
### <a name="to-configure-notifications"></a>Per configurare le notifiche  
  
1.  Nella pagina [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]Database **di** selezionare il database [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
2.  Nella sezione **Impostazioni sistema** fare clic su **Crea profilo**.  
  
3.  Completare tutti i campi obbligatori. Per altre informazioni, vedere [Finestra di dialogo Crea account e profilo di Posta elettronica database &#40;Gestione configurazione Master Data Services&#41;](../master-data-services/create-database-mail-profile-and-account-dialog-box.md).  
  
4.  Fare clic su **OK**.  
  
    > [!NOTE]  
    >  Dopo avere configurato le notifiche, non è possibile usare [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] per apportare modifiche. È necessario apportare le modifiche direttamente nel database di [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Per altre informazioni, vedere [Oggetti di configurazione di Posta elettronica database](../relational-databases/database-mail/database-mail-configuration-objects.md).  
  
## <a name="next-steps"></a>Passaggi successivi  
  
-   Alcune impostazioni di [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] hanno effetto sulle notifiche. È possibile regolare tali impostazioni in [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] o direttamente nella tabella Impostazioni sistema del database [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Per altre informazioni, vedere [Impostazioni di sistema &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Notifiche &#40;Master Data Services&#41;](../master-data-services/notifications-master-data-services.md)   
 [Risoluzione dei problemi relativi alle notifiche tramite posta elettronica (Master Data Services)](https://social.technet.microsoft.com/wiki/contents/articles/troubleshooting-email-notifications-master-data-services.aspx)   
 [Configurare le regole di business per l'invio di notifiche &#40;Master Data Services&#41;](../master-data-services/configure-business-rules-to-send-notifications-master-data-services.md)  
  
  
