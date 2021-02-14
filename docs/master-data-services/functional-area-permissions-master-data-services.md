---
description: Autorizzazioni per aree funzionali (Master Data Services)
title: Autorizzazioni per aree funzionali
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- functional area permissions [Master Data Services], about functional area permissions
- functional area permissions [Master Data Services]
- permissions [Master Data Services], functional areas
ms.assetid: a80b87b3-b904-4cda-8582-0761c2617c57
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 45386aaa746522c18f7e3de293ecced923cfa807
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272522"
---
# <a name="functional-area-permissions-master-data-services"></a>Autorizzazioni per aree funzionali (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  È possibile assegnare autorizzazioni a ogni area funzionale dell'interfaccia utente di [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] . Le aree funzionali sono le seguenti:  
  
-   **Explorer**  
  
-   **Gestione versioni**  
  
-   **Gestione integrazione**  
  
-   **Amministrazione sistema**  
  
-   **Autorizzazioni utenti e gruppi**  
  
-   **Utente con privilegi avanzati**  
  
 Quando si assegnano autorizzazioni a un'area funzionale, si rende visibile un'area dell'interfaccia utente all'utente o al gruppo.  
  
 All'interno dell'area funzionale **Visualizzatore** le autorizzazioni aggiuntive assegnate a oggetti modello e membri della gerarchia determinano i dati accessibili da un utente. All'interno di tutte le altre aree funzionali, un utente deve essere un amministratore del modello per visualizzare e agire su un modello. Per ulteriori informazioni, vedere [amministratori &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
> [!IMPORTANT]  
>  Un utente con le autorizzazioni di utente con privilegi elevati ha l'autorizzazione di amministratore per tutti i modelli e ha anche tutte le altre autorizzazioni per le aree funzionali.  
  
 Per accedere a **, è necessario che un utente o un gruppo disponga almeno delle autorizzazioni per un'area funzionale e un modello nella scheda** Modelli [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)].  
  
## <a name="see-also"></a>Vedere anche  
 [Assegnare autorizzazioni per aree funzionali &#40;Master Data Services&#41;](../master-data-services/assign-functional-area-permissions-master-data-services.md)   
 [Autorizzazioni per oggetti modello &#40;Master Data Services&#41;](../master-data-services/model-object-permissions-master-data-services.md)   
 [Autorizzazioni membri gerarchia &#40;Master Data Services&#41;](../master-data-services/hierarchy-member-permissions-master-data-services.md)   
 [Modalità di determinazione delle autorizzazioni &#40;Master Data Services&#41;](../master-data-services/how-permissions-are-determined-master-data-services.md)  
  
  
