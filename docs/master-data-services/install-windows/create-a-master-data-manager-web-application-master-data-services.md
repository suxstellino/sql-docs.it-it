---
title: Creare un'applicazione Web gestione dati master
description: L'applicazione Web Gestione dati master fornisce un'interfaccia per consentire agli utenti di utilizzare i dati master e agli amministratori di configurare e amministrare MDS.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 241d46d7-8008-47f6-bebd-0dfff1cc856a
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: df266fd09cfdd586d5f2fc438791104bf7097938
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344819"
---
# <a name="create-a-master-data-manager-web-application-master-data-services"></a>Creare un'applicazione Web gestione dati master (Master Data Services)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  L'applicazione Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] offre un'interfaccia per gli utenti, affinché possano usare i dati master, e per gli amministratori, perché possano configurare e amministrare MDS.  
  
 Un'applicazione Web deve essere contenuta sempre da un sito web. Per creare un'applicazione Web, è necessario effettuare una delle operazioni seguenti:  
  
-   Utilizzare il sito Web predefinito e quindi creare l'applicazione Web,  
  
-   Utilizzare il sito Web esistente e quindi creare l'applicazione Web oppure  
  
-   Creare un nuovo sito Web mediante il quale viene creata automaticamente un'applicazione Web.  
  
 Dopo aver creato l'applicazione Web, associarla al database [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] .  
  
## <a name="prerequisites"></a>Prerequisiti  
  
-   Per informazioni sui requisiti del computer in cui è ospitata l'applicazione Web, vedere [Requisiti dell'applicazione Web &#40;Master Data Services&#41;](../../master-data-services/install-windows/web-application-requirements-master-data-services.md).  
  
## <a name="to-create-a-master-data-manager-web-application-in-a-new-website"></a>Per creare un'applicazione Web Gestione dati master in un nuovo sito Web  
 Quando si crea un nuovo sito Web, l'applicazione Web radice è l'applicazione Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] . Inoltre, l'applicazione Web viene aggiunta a un nuovo pool di applicazioni.  
  
> [!NOTE]  
>  Se si segue questa procedura, non è possibile specificare un percorso virtuale e un alias dell'applicazione Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] . Se si vuole specificare un percorso virtuale e un alias per [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)], è necessario creare un'applicazione Web in un sito Web esistente che non sia già configurato come applicazione Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] .  
  
 Inoltre [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] supporta la creazione di siti solo con associazioni HTTP. Per aggiungere un'associazione HTTPS, creare un nuovo sito e un'applicazione in [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] , quindi vedere [Rendere sicura un'applicazione Web Gestione dati master](../../master-data-services/install-windows/secure-a-master-data-manager-web-application.md) .  
  
#### <a name="to-create-a-master-data-manager-web-application-in-a-new-website"></a>Per creare un'applicazione Web Gestione dati master in un nuovo sito Web  
  
1.  Aprire [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)].  
  
2.  Nel riquadro sinistro fare clic su **Configurazione Web**.  
  
3.  Nell'elenco Sito Web della pagina **Configurazione Web** selezionare **Crea nuovo sito Web**.  
  
4.  Nella finestra di dialogo **Crea sito Web** specificare le informazioni per un nuovo sito Web. Per altre informazioni sulle opzioni dell'interfaccia utente nella finestra di dialogo, vedere [Finestra di dialogo Crea sito Web &#40;Gestione configurazione Master Data Services&#41;](../../master-data-services/create-website-dialog-box-master-data-services-configuration-manager.md).  
  
5.  Fare clic su **OK**.  
  
## <a name="to-create-a-master-data-manager-web-application-in-an-existing-website"></a>Per creare un'applicazione Gestione dati master in un sito Web esistente  
 Quando si crea un'applicazione Web in un sito Web esistente, è possibile scegliere il percorso virtuale e l'alias dell'applicazione Web. L'applicazione Web viene aggiunta a un nuovo pool di applicazioni.  
  
#### <a name="to-create-a-master-data-manager-web-application-in-an-existing-website"></a>Per creare un'applicazione Gestione dati master in un sito Web esistente  
  
1.  Aprire [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)].  
  
2.  Nel riquadro sinistro fare clic su **Configurazione Web**.  
  
3.  Nell'elenco **Sito Web** della pagina **Configurazione Web** selezionare il sito Web in cui si vuole creare l'applicazione Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] .  
  
4.  Fare clic su **Crea applicazione**.  
  
5.  Nella finestra di dialogo **Crea applicazione Web** specificare le informazioni per una nuova applicazione Web. Per altre informazioni sulle opzioni dell'interfaccia utente nella finestra di dialogo, vedere [Finestra di dialogo Crea applicazione Web &#40;Gestione configurazione Master Data Services&#41;](../../master-data-services/create-web-application-dialog-box-master-data-services-configuration-manager.md).  
  
6.  Fare clic su **OK**.  
  
## <a name="next-steps"></a>Passaggi successivi  
  
-   Associare l'applicazione Web a un database [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] . Per altre informazioni, vedere [Associare un'applicazione Web e un database Master Data Services](../../master-data-services/install-windows/associate-a-master-data-services-database-and-web-application.md).  
  
-   Facoltativamente, configurare il sito Web che ospita l' [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] applicazione Web per utilizzare un'associazione HTTPS se si desidera crittografare il contenuto utilizzando Transport Layer Security (TLS), noto in precedenza come Secure Sockets Layer (SSL). È necessario utilizzare uno strumento Internet Information Services (IIS), ad esempio Gestione IIS, per configurare il certificato server per il server Web e per configurare un'associazione HTTPS e le impostazioni TLS per il sito. Per altre informazioni, vedere [Rendere sicura un'applicazione Web Gestione dati master](../../master-data-services/install-windows/secure-a-master-data-manager-web-application.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Installazione di Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)  
  
  
