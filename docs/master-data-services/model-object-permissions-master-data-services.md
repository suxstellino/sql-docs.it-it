---
description: Autorizzazioni per oggetti modello (Master Data Services)
title: Autorizzazioni per oggetti modello
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- permissions [Master Data Services], model objects
- models [Master Data Services], object permissions
ms.assetid: fab6335b-4cae-47de-ae7c-6c4743e0680f
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: fc4372748ee9556293eee5c074afc2f404134849
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340319"
---
# <a name="model-object-permissions-master-data-services"></a>Autorizzazioni per oggetti modello (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Le autorizzazioni per oggetti modello sono obbligatorie. Esse determinano a quali attributi può accedere un utente nell'area funzionale **Visualizzatore** dell'interfaccia utente.  
  
 Ad esempio, se si assegna all'utente un'autorizzazione **Update** per l'entità Product, l'utente può aggiornare tutti gli attributi dell'entità Product. Se si assegna l'autorizzazione **Update** per un singolo attributo, l'utente potrà aggiornare solo quell'attributo.  
  
 Per determinare la sicurezza assegnata su ogni singolo valore di attributo, le autorizzazioni degli oggetti modello vengono combinate alle autorizzazioni dei membri della gerarchia che determinano i membri ai quali un utente può accedere.  
  
 Per concedere a un utente l'accesso a un'area funzionale diversa da **Visualizzatore**, è necessario che l'utente sia un amministratore del modello, il che implica anche l'assegnazione delle autorizzazioni per gli oggetti modello. Per ulteriori informazioni, vedere [amministratori &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
 Le autorizzazioni per gli oggetti modello vengono assegnate nell' [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] area funzionale **autorizzazioni utenti e gruppi** della scheda **modelli** nell'interfaccia utente (UI). In questa scheda il modello viene rappresentato come struttura ad albero. Quando si assegna un'autorizzazione a un oggetto dell'albero, tutti gli oggetti sottostanti ereditano tale autorizzazione. Per eseguire l'override dell'ereditarietà, è possibile assegnare autorizzazioni ai singoli oggetti.  
  
 È possibile assegnare una combinazione di autorizzazioni di lettura, creazione, aggiornamento ed eliminazione o negare le autorizzazioni per gli oggetti modello. La mancata assegnazione di autorizzazioni nella scheda **Modelli** determina l'impossibilità da parte dell'utente di visualizzare modelli o dati in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)].  
  
## <a name="best-practice"></a>Procedura consigliata  
 In generale, è consigliabile assegnare l'autorizzazione **ALL** all'oggetto modello e quindi assegnare in modo esplicito l'autorizzazione agli oggetti sottostanti.  
  
## <a name="external-resources"></a>Risorse esterne  
 Post del blog sui [miglioramenti della sicurezza](/archive/blogs/e7/improvements-to-autoplay)su msdn.com.  
  
## <a name="see-also"></a>Vedere anche  
 [Assegnare autorizzazioni per oggetti modello &#40;Master Data Services&#41;](../master-data-services/assign-model-object-permissions-master-data-services.md)   
 [Autorizzazioni del modello &#40;Master Data Services&#41;](../master-data-services/model-permissions-master-data-services.md)   
 [Autorizzazioni per aree funzionali &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md)   
 [Autorizzazioni membri gerarchia &#40;Master Data Services&#41;](../master-data-services/hierarchy-member-permissions-master-data-services.md)   
 [Modalità di determinazione delle autorizzazioni &#40;Master Data Services&#41;](../master-data-services/how-permissions-are-determined-master-data-services.md)  
  
