---
title: Abilitare l'integrazione di Data Quality Services
description: Nel componente aggiuntivo Master Data Services per Excel la funzionalità di corrispondenza viene fornita da Data Quality Services (DQS).
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: ab32938d-a80e-4106-80d4-94b2de3d67dc
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 9d1bf623e4684dfcb459d4e73d533abe35d71090
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344767"
---
# <a name="enable-data-quality-services-integration-with-master-data-services"></a>Abilitare l'integrazione di Data Quality Services con Master Data Services

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]la funzionalità di corrispondenza viene fornita da Data Quality Services (DQS). Questa funzionalità deve essere abilitata per essere utilizzata.  
  
## <a name="prerequisites"></a>Prerequisiti  
  
-   È necessario che siano disponibili un database e un'applicazione Web [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] .  
  
-   La funzionalità Data Quality Services e il client Data Quality devono essere installati nella stessa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che ospita il database MDS. Per altre informazioni, vedere [Installare Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md).  
  
### <a name="to-enable-data-quality-services-integration"></a>Per abilitare l'integrazione con Data Quality Services  
  
1.  Aprire [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)].  
  
2.  Nel riquadro sinistro fare clic su **Configurazione Web**.  
  
3.  Nella pagina **Configurazione Web** selezionare il sito Web e l'applicazione Web.  
  
4.  Nella sezione **Abilita integrazione DQS** fare clic su **Abilitare l'integrazione con Data Quality Services**.  
  
5.  Nella finestra di dialogo di conferma fare clic su **OK**.  
  
## <a name="see-also"></a>Vedere anche  
 [Corrispondenza Data Quality nel Componente aggiuntivo MDS per Excel](../../master-data-services/microsoft-excel-add-in/data-quality-matching-in-the-mds-add-in-for-excel.md)   
 [Componente aggiuntivo Master Data Services per Microsoft Excel](../../master-data-services/microsoft-excel-add-in/master-data-services-add-in-for-microsoft-excel.md)   
 [Installazione di Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)  
  
  
