---
title: Compilare e pubblicare un progetto
description: Eseguire la compilazione e la pubblicazione con l'estensione progetti di database di SQL Server
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 06/25/2020
ms.openlocfilehash: 06021fc598165982156093c12c26434f06cbc422
ms.sourcegitcommit: fa63019cbde76dd981b0c5a97c8e4d57e8d5ca4e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2021
ms.locfileid: "99495709"
---
# <a name="build-and-publish-a-project"></a>Compilare e pubblicare un progetto

Il processo di compilazione nell'estensione di progetti di database SQL (anteprima) per Azure Data Studio consente la creazione di *dacpac* in ambienti Windows, MacOS e Linux. Il progetto può essere distribuito in un ambiente locale o cloud con il processo di pubblicazione.

## <a name="prerequisites"></a>Prerequisiti

- Installare e configurare l'[estensione progetti di database SQL per Azure Data Studio](sql-database-project-extension.md).

## <a name="build-a-database-project"></a>Compilare un progetto di database

 Nel viewlet **Progetti** in **Explorer** fare clic con il pulsante destro del mouse sul nodo radice *.sqlproj* e selezionare **Compila**.

 Verrà visualizzato automaticamente il riquadro di output con l'output del processo di compilazione.  Una compilazione corretta si conclude con il messaggio: 

 ``` ... exited with code: 0 ```

## <a name="publish-a-database-project"></a>Pubblicare un progetto di database

Dopo la compilazione del progetto tramite il processo corrispondente, è possibile pubblicare il database in un'istanza di SQL Server. Per pubblicare un progetto di database, nel viewlet **Progetti** in **Explorer** fare clic con il pulsante destro del mouse sul nodo radice *.sqlproj* e selezionare **Pubblica**.

Nella finestra di dialogo **Pubblica database** visualizzata specificare una connessione server e il nome del database da creare.

## <a name="next-steps"></a>Passaggi successivi

- [Estensione progetti di database SQL per Azure Data Studio](sql-database-project-extension.md)
- [Compilare un progetto di database SQL dalla riga di comando](sql-database-project-extension-build-from-command-line.md)
