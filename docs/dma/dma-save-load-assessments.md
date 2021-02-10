---
title: Salvare e caricare valutazioni con Data Migration Assistant
description: Informazioni su come usare Data Migration Assistant per salvare e caricare valutazioni.
ms.date: 01/10/2020
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: 9f427c7d706e3665cc5cbca092bfbc99a333a1ea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030629"
---
# <a name="save-and-load-assessments-with-data-migration-assistant"></a>Salvare e caricare valutazioni con Data Migration Assistant

Le istruzioni dettagliate riportate di seguito consentono di utilizzare Data Migration Assistant versione 5.0 o successiva per salvare una valutazione del database in un file e quindi caricare una valutazione da un file.

> [!NOTE]
> Oltre a caricare le valutazioni salvate con la versione più recente di DMA, gli utenti possono sfruttare questa funzionalità anche per caricare le valutazioni esportate come file con estensione JSON da versioni precedenti di Data Migration Assistant per visualizzare i risultati in v 5.0 e versioni successive.

## <a name="saving-an-assessment-to-a-file"></a>Salvataggio di una valutazione in un file

1. Dopo aver eseguito una valutazione con Data Migration Assistant, selezionare **Salva valutazione**.

   ![Apertura della finestra di dialogo Salva valutazione](../dma/media/dma-save-load-assessments/dma-open-save-dialog.png)

   Il **salvataggio standard..** . viene visualizzata la finestra di dialogo.

   > [!NOTE]
   > Per altre informazioni su come eseguire una valutazione in Data Migration Assistant, vedere l'articolo [eseguire una valutazione della migrazione SQL Server con Data Migration Assistant](../dma/dma-assesssqlonprem.md).

2. Specificare un nome per il file e quindi fare clic su **Salva**.

   ![Denominazione e salvataggio di un file di valutazione](../dma/media/dma-save-load-assessments/dma-name-save-assessment.png)

## <a name="loading-an-assessment-saved-to-a-file"></a>Caricamento di una valutazione salvata in un file

1. Per caricare una valutazione salvata in precedenza in un file, avviare Data Migration Assistant e quindi nella scheda **tutte le valutazioni** selezionare **Load Assessment**.

   ![Apertura della finestra di dialogo valutazione del carico](../dma/media/dma-save-load-assessments/dma-open-load-dialog.png)

2. Passare al file di valutazione salvato che si vuole caricare, selezionare il file e quindi selezionare **Apri**.

   ![Apertura di un file di valutazione](../dma/media/dma-save-load-assessments/dma-open-assessment.png)

   Una voce per la valutazione caricata viene visualizzata nella scheda **tutte le valutazioni** .

   ![Visualizzazione della voce di valutazione](../dma/media/dma-save-load-assessments/dma-display-assessment-entry.png)

3. Selezionare la voce Assessment, quindi selezionare **Open Assessment**.

   ![Apertura del dettaglio di valutazione](../dma/media/dma-save-load-assessments/dma-open-assessment-detail.png)

   Data Migration Assistant ora Visualizza i dettagli della valutazione eseguita in precedenza.

   ![Visualizzazione del dettaglio di valutazione](../dma/media/dma-save-load-assessments/dma-display-assessment-detail.png)
