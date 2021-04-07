---
title: Configurare la sicurezza dei dati avanzata
titleSuffix: Azure Arc
description: Configurare la sicurezza dei dati avanzata per l'istanza di SQL Server abilitato per Azure Arc
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 04/06/2021
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: e80c146c1f1fc1d40293e5ce12fb170bfd413f09
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556374"
---
# <a name="configure-advanced-data-security-for-azure-arc-enabled-sql-server-instance"></a>Configurare la sicurezza dei dati avanzata per l'istanza di SQL Server abilitato per Azure Arc

È possibile abilitare la sicurezza dei dati avanzata per le istanze di SQL Server in locale seguendo questa procedura.

## <a name="prerequisites"></a>Prerequisiti

* L'istanza di SQL Server basata su Windows è connessa ad Azure Arc. Seguire le istruzioni per caricare l' [istanza di SQL Server in SQL Server abilitati per Arc](connect.md).

   > [!NOTE]
   > Azure Defender attualmente limitato a SQL Server in esecuzione nei computer Windows.

* All'account utente viene assegnato uno dei [ruoli del Centro sicurezza (Controllo degli accessi in base al ruolo)](/azure/security-center/security-center-permissions)


## <a name="create-a-log-analytics-workspace"></a>Creare un'area di lavoro Log Analytics

1. Cercare il tipo di risorsa __Aree di lavoro Log Analytics__ e aggiungere una nuova risorsa di questo tipo tramite il pannello di creazione.

   ![Creare una nuova area di lavoro](media/configure-advanced-data-security/create-new-log-analytics-workspace.png)

   > [!NOTE]
   > È possibile usare un'area di lavoro di Log Analytics in qualsiasi area, pertanto se esiste già un'area di lavoro è possibile usarla. Tuttavia è consigliabile creare l'area di lavoro nella stessa area in cui viene creata la risorsa __Computer - Azure Arc__.

1. Passare alla pagina Panoramica della risorsa dell'area di lavoro Log Analytics e selezionare "Windows, Linux e altre origini". Copiare l'ID area di lavoro e la chiave primaria per usarli in seguito.

   ![Pannello dell'area di lavoro Log Analytics](media/configure-advanced-data-security/log-analytics-workspace-blade.png)

## <a name="install-microsoft-monitoring-agent-mma"></a>Installare Microsoft Monitoring Agent (MMA)

Il passaggio successivo è necessario solo se l'agente MMA non è ancora stato configurato nel computer remoto.

1. Selezionare la risorsa __Computer - Azure Arc__ per il server virtuale o fisico in cui è installata l'istanza di SQL Server e aggiungere l'estensione __Microsoft Monitoring Agent - Azure Arc__ usando la funzionalità **Estensioni**. Quando viene richiesto di configurare l'area di lavoro Log Analytics, usare l'ID dell'area di lavoro e la chiave primaria salvati nel passaggio precedente.

   ![Installare MMA](media/configure-advanced-data-security/install-mma-extension.png)

1. Dopo aver completato la convalida, fare clic su **Crea** per avviare il flusso di lavoro di distribuzione dell'estensione Microsoft Monitoring Agent - Azure Arc. Al termine della distribuzione, lo stato verrà aggiornato a **Operazione riuscita**.

1. Per informazioni dettagliate, vedere [Gestione delle estensioni con Azure Arc](/azure/azure-arc/servers/manage-vm-extensions).

## <a name="enable-azure-defender"></a>Abilitare Azure Defender

È poi necessario abilitare Azure Defender per l'istanza di SQL Server.

1. Passare a Centro sicurezza e aprire la pagina **Prezzi e impostazioni** sulla barra laterale.

1. Selezionare l'area di lavoro configurata per l'estensione MMA nel passaggio precedente

1. Selezionare **Azure Defender - On**. Verificare che l'opzione **SQL servers on machines** (Server SQL nelle macchine virtuali) sia abilitata.

   ![Aggiornare l'area di lavoro](media/configure-advanced-data-security/enable-azure-defender.png)

 > [!NOTE]
   > La prima analisi per generare la valutazione della vulnerabilità avverrà entro 24 ore dall'abilitazione della sicurezza dei dati avanzata. Successivamente le analisi automatiche verranno eseguite ogni settimana la domenica.

## <a name="explore"></a>Esplora

Esplorare le anomalie e le minacce per la sicurezza nel Centro sicurezza di Azure.

1. Aprire la risorsa SQL Server - Azure Arc e selezionare **Sicurezza** nel menu a sinistra. per visualizzare le raccomandazioni e gli avvisi relativi a tale istanza.

   ![Selezionare l'intestazione di sicurezza](media/configure-advanced-data-security/security-heading-sql-server-arc.png)

1. Fare clic su una delle raccomandazioni per visualizzare i dettagli della vulnerabilità nel __Centro sicurezza__.

   ![Report vulnerabilità](media/configure-advanced-data-security/vulnerabilities-report.png)

1. Fare clic su qualsiasi avviso di sicurezza per visualizzare i dettagli completi ed esplorare ulteriormente l'attacco in [Azure Sentinel](/azure/sentinel/overview). Il diagramma seguente è un esempio dell'avviso di attacco di forza bruta.

   ![Avviso di forza bruta](media/configure-advanced-data-security/brute-force-alert.png)

1. Fare clic su **Intervieni** per attenuare l'avviso.

   ![Attenuazione degli avvisi](media/configure-advanced-data-security/brute-force-alert-mitigation.png)

> [!NOTE]
> Il collegamento generale __Centro sicurezza__ nella parte superiore della pagina non usa l'URL del portale di anteprima, quindi le risorse __SQL Server - Azure Arc__ non saranno visibili in tale collegamento. È consigliabile seguire i collegamenti per le singole raccomandazioni o i singoli avvisi.

## <a name="next-steps"></a>Passaggi successivi

È possibile esaminare ulteriormente gli avvisi di sicurezza e gli attacchi usando [Azure Sentinel](/azure/sentinel/overview). Seguire queste istruzioni per [eseguire l'onboarding di Azure Sentinel](/azure/sentinel/connect-data-sources).