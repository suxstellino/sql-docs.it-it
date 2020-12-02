---
description: Lezione 2-1 - Compilazione dell'utilità di distribuzione
title: "Passaggio 1: Compilazione dell'utilità di distribuzione | Microsoft Docs"
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 1ff4dcff-89b3-4b99-a725-5f7963e98abf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3e1b6c318bd971e5352ad0191ccad51e20f5560a
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2020
ms.locfileid: "92193803"
---
# <a name="lesson-2-1---building-the-deployment-utility"></a>Lezione 2-1 - Compilazione dell'utilità di distribuzione

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


In questa attività verrà configurata e compilata un'utilità di distribuzione per il progetto Deployment Tutorial.  
  
Per poter compilare l'utilità di distribuzione, è necessario modificare le proprietà del progetto Deployment Tutorial. Verrà usata la finestra di dialogo **Deployment Tutorial Property Pages** (Pagine delle proprietà di Deployment Tutorial) per configurare queste proprietà. In questa finestra di dialogo è necessario abilitare la funzionalità di aggiornamento delle configurazioni durante la distribuzione e specificare il processo di compilazione che consente di generare l'utilità di distribuzione. Al termine dell'impostazione delle proprietà, il progetto verrà compilato.  
  
[!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] include un set di finestre che consentono di eseguire il debug dei pacchetti. È possibile visualizzare messaggi di errore, avviso e informativi, tenere traccia dello stato dei pacchetti in fase di esecuzione oppure visualizzare lo stato e i risultati dei processi di compilazione. Per questa lezione, si utilizzerà la finestra Output per visualizzare lo stato e i risultati della compilazione dell'utilità di distribuzione.  
  
### <a name="to-set-the-deployment-utility-properties"></a>Per impostare le proprietà dell'utilità di distribuzione  
  
1.  Se [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] non è già aperto, fare clic sul pulsante **Start**, scegliere **Programmi**, **Microsoft SQL Server** selezionare **Business Intelligence Development Studio**.  
  
2.  Scegliere **Apri** dal menu **File**, fare clic su **Progetto/Soluzione**, selezionare la cartella **Deployment Tutorial** , fare clic su **Apri** e fare doppio clic su **Deployment Tutorial.sln**.  
  
3.  In Esplora soluzioni fare clic con il pulsante destro del mouse su Deployment Tutorial e scegliere **Proprietà**.  
  
4.  Nella finestra di dialogo **Deployment Tutorial Property Pages** (Pagine delle proprietà di Deployment Tutorial) espandere le proprietà di configurazione e fare clic su Utilità di distribuzione.  
  
5.  Nel riquadro a destra della finestra di dialogo **Deployment Tutorial Property Pages** (Pagine delle proprietà di Deployment Tutorial) verificare che **AllowConfigurationChanges** sia impostato su **true**, impostare **CreateDeploymentUtility** su **true** ed eventualmente aggiornare il valore predefinito di **DeploymentOutputPath**.  
  
6.  Fare clic su **OK**.  
  
### <a name="to-build-the-deployment-utility"></a>Per compilare l'utilità di distribuzione  
  
1.  In Esplora soluzioni fare clic su **Deployment Tutorial**.  
  
2.  Scegliere **Output** dal menu **Visualizza**. Per impostazione predefinita, la finestra Output si trova nell'angolo inferiore sinistro di [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)].  
  
3.  Scegliere **Build Deployment Tutorial** (Compila Deployment Tutorial) dal menu **Compila**.  
  
4.  Nella finestra Output verificare le informazioni seguenti:  
  
    Compilazione avviata - Progetto SQL Integration Services: Incrementale ...  
  
    Creazione dell'utilità di distribuzione in corso...  
  
    Utilità di distribuzione creata.  
  
    Compilazione completata -- 0 errori, 0 avvisi  
  
    ========== Compilazione: 0 completate, 0 non riuscite, 1 aggiornate, 0 ignorate ==========  
  
5.  Scegliere **Esci** dal menu **File**. Se richiesto per salvare le modifiche agli elementi di Deployment Tutorial, fare clic su **Sì**.  
  
## <a name="next-task-in-lesson"></a>Attività successiva della lezione  
[Passaggio 2: Verifica del pacchetto di distribuzione](../integration-services/lesson-2-2-verifying-the-deployment-bundle.md)  
  
## <a name="see-also"></a>Vedere anche  
[Creazione di un'utilità di distribuzione](./packages/legacy-package-deployment-ssis.md)  
  
  
