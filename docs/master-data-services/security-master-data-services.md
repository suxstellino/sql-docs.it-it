---
title: Sicurezza
description: Informazioni sulla sicurezza in Master Data Services, inclusi i tipi di utenti, su come impostare la sicurezza, la sicurezza nel componente aggiuntivo per Excel e le attività correlate.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 56bc41ea-de28-4184-aa7e-99111ae55af5
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 97209bd6d906a73f6199f06a09e14e1ab4d0ac22
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352990"
---
# <a name="security-master-data-services"></a>Sicurezza (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]utilizzare la sicurezza per assicurare che agli utenti sia consentito l'accesso ai dati master specifici necessari per l'esecuzione dei processi e per impedire l'accesso ai dati che non devono essere loro disponibili.  
  
 È possibile utilizzare anche la sicurezza per creare un amministratore di un modello specifico e un'area funzionale (ad esempio, per consentire a qualcuno di creare versioni del modello Customer o per dare la possibilità di impostare autorizzazioni di sicurezza).  
  
 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] è basata su utenti e gruppi locali o del dominio Active Directory. La sicurezza MDS consente all'utente di utilizzare un livello di dettaglio granulare durante l'individuazione dei dati ai quali un utente può accedere. A causa della granularità, la sicurezza può diventare facilmente complicata ed è necessario prestare attenzione durante l'utilizzo di utenti e gruppi sovrapposti. Per altre informazioni, vedere [Autorizzazioni utenti e gruppi sovrapposte &#40;Master Data Services&#41;](../master-data-services/overlapping-user-and-group-permissions-master-data-services.md).  
  
 È possibile assegnare l'accesso di sicurezza nell'area funzionale **Autorizzazioni utenti e gruppi** dell'applicazione Web [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] o tramite il servizio Web.  
  
## <a name="types-of-users"></a>Tipi di utenti  
 Esistono due tipi di utenti in [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]:  
  
-   Quelli che accedono ai dati nell'area funzionale **Esplora risorse** .  
  
-   Quelli che dispongono della possibilità di eseguire attività amministrative in aree diverse da **Esplora risorse**. Questi utenti sono chiamati [Amministratori &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
## <a name="how-to-set-security"></a>Impostazione della sicurezza  
 Per concedere ad un utente o gruppo l'autorizzazione di accesso a dati o a funzionalità in MDS, è necessario assegnare:  
  
-   L'[accesso all'area funzionale](../master-data-services/functional-area-permissions-master-data-services.md), che determina a quali delle cinque aree funzionali dell'interfaccia utente un utente può accedere.  
  
-   [Autorizzazioni per oggetti modello](../master-data-services/model-object-permissions-master-data-services.md)che determinano gli attributi ai quali un utente può accedere e il tipo di accesso (lettura o aggiornamento) che l'utente possiede su quegli attributi. L'utente può anche assegnare autorizzazioni di amministratore al livello del modello.  
  
-   Facoltativamente, le [autorizzazioni per i membri della gerarchia](../master-data-services/hierarchy-member-permissions-master-data-services.md)che determinano i membri ai quali un utente può accedere e il tipo di accesso (lettura, aggiornamento ed eliminazione) che l'utente possiede su quei membri.  
  
 Quando si assegnano le autorizzazioni a attributi e membri, le autorizzazioni si intersecano e le regole determinano quale autorizzazione ha la precedenza. Per altre informazioni, vedere [Modalità di determinazione delle autorizzazioni &#40;Master Data Services&#41;](../master-data-services/how-permissions-are-determined-master-data-services.md).  
  
## <a name="security-in-the-add-in-for-excel"></a>Sicurezza nel componente aggiuntivo di Excel  
 La sicurezza impostata nell'applicazione Web [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] viene anche applicata a [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)]. Gli utenti possono visualizzare e utilizzare solo i dati per cui dispongono dell'autorizzazione. Gli amministratori possono eseguire attività amministrative.  
  
 L'unico problema è che tutta la sicurezza assegnata in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] diventa effettiva in Excel solo dopo un periodo di tempo di 20 minuti. Tale intervallo viene definito dall'impostazione *MdsMaximumUserInformationCacheInterval* nel file web.config. Per modificare l'intervallo, modificare l'impostazione e riavviare IIS.  
  
## <a name="related-tasks"></a>Attività correlate  
  
|Descrizione dell'attività|Argomento|  
|----------------------|-----------|  
|Creare un utente che dispone delle autorizzazioni complete per un modello.|[Creare un amministratore di modelli &#40;Master Data Services&#41;](../master-data-services/create-a-model-administrator-master-data-services.md)|  
|Aggiungere un gruppo Active Directory a [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]; questo è il primo passaggio nell'assegnazione di un'autorizzazione al gruppo per accedere ai dati nell'applicazione Web [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .|[Aggiungere un gruppo &#40;Master Data Services&#41;](../master-data-services/add-a-group-master-data-services.md)|  
|Assegnare l'autorizzazione a un'area funzionale dell'applicazione Web [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .|[Assegnare autorizzazioni per aree funzionali &#40;Master Data Services&#41;](../master-data-services/assign-functional-area-permissions-master-data-services.md)|  
|Assegnare l'autorizzazione ai valori dell'attributo assegnando l'autorizzazione agli oggetti modello.|[Assegnare autorizzazioni per oggetti modello &#40;Master Data Services&#41;](../master-data-services/assign-model-object-permissions-master-data-services.md)|  
|Assegnare l'autorizzazione ai valori del membro assegnando l'autorizzazione ai nodi della gerarchia.|[Assegnare autorizzazioni membri gerarchia &#40;Master Data Services&#41;](../master-data-services/assign-hierarchy-member-permissions-master-data-services.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Amministratori &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)   
 [Utenti e gruppi &#40;Master Data Services&#41;](../master-data-services/users-and-groups-master-data-services.md)   
 [Autorizzazioni per aree funzionali &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md)   
 [Autorizzazioni per oggetti modello &#40;Master Data Services&#41;](../master-data-services/model-object-permissions-master-data-services.md)   
 [Autorizzazioni membri gerarchia &#40;Master Data Services&#41;](../master-data-services/hierarchy-member-permissions-master-data-services.md)   
 [Modalità di determinazione delle autorizzazioni &#40;Master Data Services&#41;](../master-data-services/how-permissions-are-determined-master-data-services.md)  
  
  
