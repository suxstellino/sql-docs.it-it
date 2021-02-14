---
title: Convalida
description: I dati vengono convalidati per garantirne l'accuratezza, in modo automatico o basato sulle regole business create in Master Data Services.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 98eb49e7-b190-4a21-8316-08c07cde14ed
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ecd7fcde67c20c416c7eb3eb0d781f714328a64c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336211"
---
# <a name="validation-master-data-services"></a>Convalida (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], i dati sono convalidati per assicurare l'accuratezza. Una convalida viene eseguita automaticamente, mentre l'altra convalida è basata sulle regole business create dagli amministratori.  
  
## <a name="when-data-validation-occurs"></a>Quando viene eseguita la convalida dei dati  
 La convalida viene eseguita in momenti diversi ed è visualizzata in modo diverso nell'applicazione Web [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
|Tipo di convalida|Standard determinati da|Quando viene eseguita|Visualizzata nella interfaccia utente Web di Gestione dati master come|Visualizzata nel componente aggiuntivo per Excel come|I dati vengono salvati nel database MDS?|  
|---------------------|-----------------------------|--------------------|---------------------------------------------------|-------------------------------------------|------------------------------------------|  
|Convalida delle regole business|Un amministratore MDS|Automaticamente quando un utente aggiunge o modifica dati.<br /><br /> Manualmente quando un utente applica regole business.<br /><br /> Manualmente quando un amministratore nell'area funzionale **Gestione versioni** dell'applicazione Web [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] convalida una versione rispetto alle regole di business.|Errori di convalida|ValidationStatus|Sì|  
|Tipo di dati e convalida del contenuto|Un amministratore MDS, in caso di creazione di oggetti modello (ad esempio, la lunghezza di un attributo o il tipo di dati)|Automaticamente quando un utente aggiunge o modifica dati|Errori di input|InputStatus|No|  
|Tipo di dati e convalida del contenuto|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] o [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]|Automaticamente quando un utente aggiunge o modifica dati|Errori di input|InputStatus|No|  
  
## <a name="related-tasks"></a>Attività correlate  
  
|Descrizione dell'attività|Argomento|  
|----------------------|-----------|  
|Creare regole business e pubblicarle, in modo che i dati vengano convalidati rispetto a tali regole.|[Creare e pubblicare una regola business &#40;Master Data Services&#41;](../master-data-services/create-and-publish-a-business-rule-master-data-services.md)|  
|Convalidare una versione di dati rispetto a regole business. Solo amministratori.|[Convalidare una versione usando le regole di business &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)|  
|Convalidare un subset specifico di dati rispetto a regole business. Tutti gli utenti con l'autorizzazione per l'area funzionale **Visualizzatore** .|[Convalidare membri specifici rispetto a regole business &#40;Master Data Services&#41;](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)|  
|Convalidare un subset specifico di dati rispetto a regole business. Tutti gli utenti con l'autorizzazione per l'area funzionale **Visualizzatore** e che usano [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)].|[Applicare le regole di business &#40;componente aggiuntivo MDS per Excel&#41;](../master-data-services/microsoft-excel-add-in/apply-business-rules-mds-add-in-for-excel.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Regole di business &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)  
  
  
