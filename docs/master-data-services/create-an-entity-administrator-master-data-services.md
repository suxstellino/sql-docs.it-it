---
description: Creare un amministratore di entità (Master Data Services)
title: Creare un amministratore di entità
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 717be1e8-488e-4219-8d1e-ca9084b864cd
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: f7b808eec05b20d5aa52b26d2470beddd860c196
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341460"
---
# <a name="create-an-entity-administrator-master-data-services"></a>Creare un amministratore di entità (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]creare un amministratore di entità quando si vuole che un gruppo o un utente abbia tutte le autorizzazioni in una o più entità oppure l'autorizzazione per approvare gli insiemi di modifiche in sospeso.  
  
> [!TIP]  
>  Per semplificare l'amministrazione, creare un gruppo locale o di Windows e configurarlo come amministratore di entità. È quindi possibile aggiungere e rimuovere utenti nel gruppo senza accedere a [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)].  
  
## <a name="prerequisites"></a>Prerequisiti  
 Per eseguire questa procedura:  
  
-   È necessario disporre dell'autorizzazione di accesso all'area funzionale **Autorizzazioni utenti e gruppi** .  
  
-   È necessario essere un amministratore del modello. Per ulteriori informazioni, vedere [amministratori &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
## <a name="to-create-an-entity-administrator"></a>Per creare un amministratore di entità  
  
1.  In [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], fare clic su **Autorizzazioni utenti e gruppi**.  
  
2.  Selezionare la riga relativa all'utente o al gruppo che si vuole modificare e quindi fare clic su **Modifica utente selezionato**.  
  
3.  Fare clic sulla scheda **Modelli** , selezionare facoltativamente un modello dall'elenco **Modelli** e quindi fare clic su **Modifica**.  
  
4.  Fare clic sull'entità a cui si vogliono concedere le autorizzazioni e quindi fare clic su **Amministratore** dal menu.  
  
5.  Completare il passaggio 4 per ogni entità per cui si vuole che il gruppo o l'utente sia configurato come amministratore.  
  
6.  Fare clic su **Salva**.  
  
## <a name="see-also"></a>Vedere anche  
 [Amministratori &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)   
 [Assegnare autorizzazioni per oggetti modello &#40;Master Data Services&#41;](../master-data-services/assign-model-object-permissions-master-data-services.md)   
 [Assegnare autorizzazioni membri gerarchia &#40;Master Data Services&#41;](../master-data-services/assign-hierarchy-member-permissions-master-data-services.md)   
 [Autorizzazioni per oggetti modello &#40;Master Data Services&#41;](../master-data-services/model-object-permissions-master-data-services.md)   
 [Autorizzazioni membri gerarchie &#40;Master Data Services&#41;](../master-data-services/hierarchy-member-permissions-master-data-services.md)  
  
  
