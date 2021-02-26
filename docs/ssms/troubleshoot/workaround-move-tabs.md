---
description: Soluzione alternativa per spostare le schede
title: Abilitare le schede per lo spostamento senza arresto anomalo di SSMS
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: c28ffa44-7b8b-4efa-b755-c7a3b1c11ce4
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan, sstein
ms.custom: seo-lt-2019
ms.date: 11/03/2020
ms.openlocfilehash: ae7c79792d962ce578059e672de7165f477f1c4a
ms.sourcegitcommit: 6c93282cce1216dac327cb28848a3ab4d51b776e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/18/2021
ms.locfileid: "100647080"
---
# <a name="workaround-to-move-tabs"></a>Soluzione alternativa per spostare le schede

[!INCLUDE[Applies to](../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

Potrebbe essere necessaria una soluzione alternativa per abilitare lo stato di trasferimento delle schede dell'editor di query, sia all'interno della finestra che ancora una scheda precedentemente rimossa.  Se sono stati applicati tutti gli aggiornamenti di Windows disponibili e si è in grado di modificare le schede dell'editor di query senza riscontrare un arresto anomalo, attenersi alla [soluzione alternativa](#workaround) riportata di seguito.

## <a name="applicable-environments"></a>Ambienti applicabili
Gli aggiornamenti di Windows per la .NET Framework hanno introdotto un problema noto che comporta un arresto anomalo dell'applicazione per SQL Server Management Studio (SSMS) quando si ancorano schede o si divide la finestra.  Le informazioni più recenti sono reperibili in [SQL Server Commenti degli utenti](https://feedback.azure.com/forums/908035/suggestions/42651556).

## <a name="workaround"></a>Soluzione alternativa

Se l'arresto anomalo persiste dopo l'applicazione di tutti gli aggiornamenti di Windows disponibili, attenersi alla procedura seguente per attenuare il problema:

1. Chiudere tutte le istanze di SQL Server Management Studio (SSMS).

2. Individuare il file dell'applicazione SSMS (exe).  Questa situazione si trova in genere in `C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE` .

3. Aprire il file `Ssms.exe.config` in blocco note come amministratore.

4. Individuare il `AppContextSwitchOverrides` nodo e aggiungere queste due proprietà al valore.
    ```
    ;Switch.System.Windows.Interop.MouseInput.OptOutOfMoveToChromedWindowFix=true; Switch.System.Windows.Interop.MouseInput.DoNotOptOutOfMoveToChromedWindowFix=true
    ```

    ![Modifica ssms.exe.config](../media/troubleshoot/execonfig-edit.png)

5. Salvare il file di configurazione e riaprire SSMS.
