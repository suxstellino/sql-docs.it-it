---
description: Utilizzo di annotazioni nei pacchetti
title: Usare le annotazioni nei pacchetti | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- self-documenting packages
- adding annotations
- annotations [Integration Services]
ms.assetid: 48c8ed9a-b10d-490c-9ba7-4b77aa44e3dd
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 88a927a23696e5de7a1e079cb517078dc318b69c
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2020
ms.locfileid: "92193763"
---
# <a name="use-annotations-in-packages"></a>Utilizzo di annotazioni nei pacchetti

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  In Progettazione [!INCLUDE[ssIS](../includes/ssis-md.md)] sono disponibili annotazioni che è possibile utilizzare per ottenere pacchetti autodocumentati, più semplici da capire e gestire. È possibile aggiungere annotazioni alle aree di progettazione dei flussi di controllo, dei flussi di dati e dei gestori di eventi di Progettazione [!INCLUDE[ssIS](../includes/ssis-md.md)] . Nelle annotazioni può essere contenuto qualsiasi tipo di testo e possono essere utilizzate per aggiungere etichette, commenti e altre informazioni descrittive a un pacchetto. Le annotazioni sono disponibili solo in modalità progettazione. Non possono essere ad esempio scritte nei log.  
  
 Quando si preme INVIO, il testo viene riportato nella riga successiva. Le dimensioni della casella delle annotazioni aumentano automaticamente man mano che si aggiungono le righe di testo. Le annotazioni dei pacchetti vengono rese persistenti come testo non crittografato nella sezione CDATA del file del pacchetto.  
  
 Per altre informazioni sulle modifiche al formato del file del pacchetto, vedere [Formato del pacchetto SSIS](./integration-services-ssis-packages.md).  
  
 Quando si salva un pacchetto, Progettazione [!INCLUDE[ssIS](../includes/ssis-md.md)] salva le annotazioni nel pacchetto.  
  
## <a name="add-an-annotation-to-a-package"></a>Aggiunta di un'annotazione a un pacchetto  
  
1.  In [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]aprire il progetto di [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] contenente il pacchetto a cui si desidera aggiungere un'annotazione.  
  
2.  In Esplora soluzioni fare doppio clic sul pacchetto per aprirlo.  
  
3.  In Progettazione [!INCLUDE[ssIS](../includes/ssis-md.md)] fare clic con il pulsante destro del mouse in un punto qualsiasi dell'area di progettazione della scheda **Flusso di controllo**, **Flusso di dati** o **Gestore evento** , quindi fare clic su **Aggiungi annotazione**. Nell'area di progettazione della scheda viene visualizzato un blocco di testo.  
  
4.  Aggiungere il testo.  
  
    > [!NOTE]  
    >  Se non si aggiunge alcun testo, quando si fa clic all'esterno del blocco di testo quest'ultimo verrà rimosso.  
  
5.  Per modificare le dimensioni o il formato del testo nell'annotazione, fare clic con il pulsante destro del mouse sull'annotazione, quindi scegliere **Imposta carattere annotazioni testo**.  
  
6.  Pre aggiungere una riga di testo aggiuntiva, premere INVIO.  
  
     Le dimensioni della casella delle annotazioni aumentano automaticamente man mano che si aggiungono le righe di testo.  
  
7.  Per aggiungere un'annotazione a un gruppo, fare clic con il pulsante destro del mouse sull'annotazione, quindi scegliere **Gruppo**.  
  
8.  Per salvare il pacchetto aggiornato, scegliere **Salva tutto** dal menu **File**.