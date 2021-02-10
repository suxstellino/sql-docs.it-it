---
title: Controllo del codice sorgente
description: Azure Data Studio supporta Git per la gestione del controllo del codice sorgente. Informazioni su come aprire un repository Git esistente e su come inizializzarne uno nuovo.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, sstein
ms.custom: seodec18
ms.date: 09/24/2019
ms.openlocfilehash: 014376dd56045b9e5ef440054ae2c1d1df836cad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040021"
---
# <a name="source-control-in-azure-data-studio"></a>Controllo del codice sorgente in Azure Data Studio

Azure Data Studio supporta Git per il controllo della versione e del codice sorgente.

## <a name="git-support-in-azure-data-studio"></a>Supporto Git in Azure Data Studio

Azure Data Studio è in dotazione con Gestione controllo del codice sorgente (SCM) di Git, ma è comunque necessario [installare Git (versione 2.0.0 o successiva)](https://git-scm.com/download) per rendere disponibili queste funzionalità.

## <a name="open-an-existing-git-repository"></a>Aprire un repository Git esistente

1. Nel menu **File** selezionare **Apri cartella...**
2. Passare alla cartella contenente i file registrati da Git e quindi fare clic su **Seleziona cartella**. A questo punto è possibile selezionare le sottocartelle presenti nel repository locale.

## <a name="initialize-a-new-git-repository"></a>Inizializzare un nuovo repository Git

1. Selezionare **Controllo codice sorgente** e quindi l'icona di Git.

   ![Icona Git di Controllo codice sorgente](media/source-control/source-control.png)

1. Immettere il percorso della cartella che si vuole inizializzare come repository Git e premere **Invio**.

   ![Inizializzare repository Git](media/source-control/initialize-git-repository.png)

## <a name="working-with-git-repositories"></a>Uso dei repository Git

Azure Data Studio eredita l'implementazione di Git da VS Code ma attualmente non supporta altri provider di Gestione controllo servizi. Per informazioni dettagliate sull'uso di Git dopo l'apertura o l'inizializzazione di un repository, vedere [Supporto di Git in VS Code](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support).

## <a name="additional-resources"></a>Risorse aggiuntive

- [Documentazione di Git](https://git-scm.com/documentation)
