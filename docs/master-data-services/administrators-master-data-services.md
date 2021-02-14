---
title: Administrators
description: 'Informazioni sui tipi di amministratori in Master Data Services: amministratori di modelli, amministratori di entità e utenti con privilegi avanzati.'
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- administrators [Master Data Services], about administrators
- administrators [Master Data Services]
- models [Master Data Services], administrators
ms.assetid: d330aa4e-6ade-4b09-b376-1b15d6c78f7d
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: bfe03a31b5d8f35770f8ded41e4d8fdb86fb9d30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339666"
---
# <a name="administrators-master-data-services"></a>Amministratori (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Questo articolo descrive i tipi di amministratori di [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]: amministratori di modelli, amministratori di entità e utenti con privilegi avanzati.  
  
## <a name="model-administrators"></a>Amministratori di modelli  
 In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] , un amministratore di modelli è un utente che dispone dell'autorizzazione di **amministratore** assegnata all'oggetto modello di livello superiore nella scheda **oggetti modello** . Quando un utente dispone delle autorizzazioni di amministratore per un particolare modello, tutte le altre autorizzazioni per gli oggetti figlio del modello (autorizzazioni per oggetti e membri del modello) vengono trumpate dall'autorizzazione di **amministratore** del modello ed effettivamente ignorate.  
  
-   Se l'utente dispone dell'accesso all'area funzionale **Esplora risorse** , l'utente può aggiungere, eliminare e aggiornare tutti i dati master in questa area.  
  
-   Se l'utente dispone di accesso ad altre aree funzionali, questo utente può eseguire tutte le attività amministrative disponibili nell'area funzionale.  
  
 Ogni modello può avere più amministratori. Ogni utente può essere un amministratore di uno, alcuni o tutti i modelli della distribuzione [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
 Un utente può essere configurato come amministratore di modelli in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] o a livello di codice. Per altre informazioni, vedere [Creare un amministratore di modelli &#40;Master Data Services&#41;](../master-data-services/create-a-model-administrator-master-data-services.md).  
  
## <a name="entity-administrators"></a>Amministratori di entità  
 In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] , un amministratore di entità è un utente che dispone delle autorizzazioni di amministratore assegnate all'oggetto entità nella scheda oggetti modello. Quando un utente dispone di autorizzazioni di amministratore per un'entità, tutte le altre autorizzazioni per gli oggetti figlio dell'entità (autorizzazioni per oggetti e membri modello) sono sostituite dalle autorizzazioni di amministratore e vengono ignorate.  
  
-   Se l'utente dispone dell'accesso all'area funzionale **Esplora risorse** , l'utente può aggiungere, eliminare e aggiornare tutti i dati master in questa area.  
  
-   Se le modifiche all'entità richiedono l'approvazione dell'amministratore, un amministratore di entità può esaminare e quindi approvare o rifiutare gli insiemi di modifiche.  
  
 Ogni entità può avere più amministratori. Ogni utente può essere un amministratore di entità per una, alcune o tutte le entità.  
  
 Un utente può essere configurato come amministratore di entità in [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] o a livello di codice. Per altre informazioni, vedere [Creare un amministratore di entità &#40;Master Data Services&#41;](../master-data-services/create-an-entity-administrator-master-data-services.md).  
  
## <a name="master-data-services-super-user"></a>Utente con privilegi avanzati di Master Data Services  
 In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]è possibile assegnare a un utente le autorizzazioni per l'area funzionale Utente con privilegi avanzati. Un utente con le autorizzazioni per l'area funzionale Utente con privilegi avanzati ha l'autorizzazione di amministratore per tutti i modelli e le autorizzazioni per tutte le altre aree funzionali. Per informazioni sulle autorizzazioni per le aree funzionali, vedere [Autorizzazioni per aree funzionali &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md).  
  
 L'utente con privilegi avanzati predefinito viene specificato per l'**Account amministratore** quando si crea il database [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] usando la [Procedura guidata Crea database &#40;Gestione configurazione Master Data Services&#41;](../master-data-services/create-database-wizard-master-data-services-configuration-manager.md).  
  
 L'utente con privilegi avanzati può eseguire le operazioni seguenti:  
  
-   Accedere a tutte le aree funzionali.  
  
-   Aggiungere, eliminare e aggiornare tutti i dati master per tutti i modelli nell'area funzionale **Visualizzatore** .  
  
 È possibile assegnare le autorizzazioni di utente con privilegi avanzati a più utenti e/o gruppi di utenti.  
  
## <a name="comparing-administrator-types"></a>Confronto tra tipi di amministratore  
  
|Tipo di amministratore|Descrizione|  
|------------------------|-----------------|  
|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] Utente con privilegi avanzati|Le autorizzazioni assegnate in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] non influiscono sull'accesso dell'amministratore.<br /><br /> Può essere un utente con privilegi avanzati in autorizzazioni per aree funzionali assegnate in modo esplicito o autorizzazioni ereditate da un gruppo.<br /><br /> Ha automaticamente tutte le autorizzazioni per tutti i modelli.<br /><br /> Dispone automaticamente dell'accesso a tutte le aree funzionali.|  
|Amministratore di modelli|Può essere un amministratore di modelli in base alle autorizzazioni di amministratore assegnate in modo esplicito o in base alle autorizzazioni ereditate da un gruppo.<br /><br /> Dispone dell'accesso solo alle aree funzionali per le quali è stato concesso l'accesso.<br /><br /> Ha automaticamente tutte le autorizzazioni per tutti gli oggetti e i membri nel modello specifico.|  
|Amministratore di entità|Può essere un amministratore di entità in base alle autorizzazioni di amministratore assegnate in modo esplicito o in base alle autorizzazioni ereditate da un gruppo.<br /><br /> Dispone dell'accesso solo alle aree funzionali per le quali è stato concesso l'accesso.<br /><br /> Ha automaticamente tutte le autorizzazioni per tutti gli oggetti e i membri nell'entità specifica.<br /><br /> Può approvare gli insiemi di modifiche in sospeso se le modifiche all'entità richiedono l'approvazione.|  
  
## <a name="external-resources"></a>Risorse esterne  
 Post del blog sui [miglioramenti della sicurezza](/archive/blogs/e7/improvements-to-autoplay)su msdn.com.  
  
## <a name="see-also"></a>Vedere anche  
 [Creare un amministratore di modelli &#40;Master Data Services&#41;](../master-data-services/create-a-model-administrator-master-data-services.md)   
 [Creazione di un database di Master Data Services](../master-data-services/install-windows/create-a-master-data-services-database.md)   
 [Notifiche &#40;Master Data Services&#41;](../master-data-services/notifications-master-data-services.md)  
  
