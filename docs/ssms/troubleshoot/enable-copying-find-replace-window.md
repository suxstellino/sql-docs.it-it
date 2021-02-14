---
description: 'Soluzione alternativa per abilitare la copia dalla finestra Trova e sostituisci '
title: Abilitare la copia dalla finestra Trova e sostituisci
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
ms.openlocfilehash: f7a11c952fa20b720ad37abc204c7dbb31f0e96f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058606"
---
# <a name="workaround-to-enable-copying-from-find-and-replace-window"></a>Soluzione alternativa per abilitare la copia dalla finestra Trova e sostituisci

[!INCLUDE[Applies to](../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

È necessaria una soluzione alternativa per abilitare la copia dalla finestra Trova e Sostituisci.  Se non è possibile eseguire la copia dalla finestra Trova e Sostituisci in SQL Server Management Studio, seguire la [soluzione alternativa](#workaround) descritta di seguito.

## <a name="error-message"></a>Messaggio di errore

Quando si tenta di copiare il testo dalla finestra Trova e Sostituisci in SQL Server Management Studio, viene visualizzato un messaggio di errore.

> Impossibile tagliare o copiare documenti non salvati negli Appunti da progetti di file esterni. Salvare i documenti prima di tagliarli o copiarli.

![Finestra di dialogo dell'errore per: Impossibile tagliare o copiare documenti non salvati negli Appunti da progetti di file esterni. Salvare i documenti prima di tagliarli o copiarli.](../media/troubleshoot/unable-copy-find-replace-window.png)

## <a name="workaround"></a>Soluzione alternativa

Per abilitare la copia del testo dalla finestra Trova e Sostituisci, seguire questa procedura:

1. Scegliere **Opzioni** dal menu **Strumenti**.

2. In **Ambiente**>**Documenti** deselezionare "Mostra file esterni in Esplora soluzioni".

3. Chiudere e riaprire SQL Server Management Studio.

![Finestra Opzioni con "Mostra file esterni in Esplora soluzioni" deselezionata](../media/troubleshoot/fix-copy-find-replace-window.png)

