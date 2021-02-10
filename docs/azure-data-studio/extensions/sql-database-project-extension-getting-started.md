---
title: Introduzione all'estensione progetti di database SQL
description: Introduzione all'uso dell'estensione progetti di database SQL per Azure Data Studio
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: ''
ms.date: 07/30/2020
ms.openlocfilehash: 397bdebe2bdea292009e0216c6b3004ac9475b18
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040315"
---
# <a name="getting-started-with-the-sql-database-projects-extension-preview"></a>Introduzione all'estensione progetti di database SQL (anteprima)

Questo articolo illustra tre modi per iniziare a usare l'estensione progetti di database SQL:

1. Per creare un nuovo progetto di database, passare al viewlet **Progetti** in Explorer o cercare **Nuovo Progetto di database** nel riquadro comandi.
2. È possibile aprire progetti di database esistenti tramite **Apri Progetto di database** nel riquadro comandi.
3. Iniziare da un database esistente usando **Importa nuovo progetto di database** dal riquadro comandi.

    ![Nuovo viewlet](media/sql-database-projects-extension/projects-viewlet.png)

## <a name="create-an-empty-database-project"></a>Creare un progetto di database vuoto

Nel viewlet **Progetti** in **Explorer** selezionare il pulsante **Nuovo progetto** e immettere il nome del progetto nella casella di testo di input visualizzata.  Nella finestra di dialogo "Selezionare una cartella" visualizzata selezionare una directory in cui inserire la cartella del progetto, il file con estensione sqlproj e gli altri contenuti.
Il progetto vuoto verrà aperto e visualizzato nel viewlet **Progetti** per la modifica.

## <a name="open-an-existing-project"></a>Aprire un progetto esistente

Nel viewlet **Progetti** selezionare il pulsante **Apri progetto** e aprire un file con estensione *sqlproj* esistente dall'elemento di selezione file visualizzato. I progetti esistenti possono avere origine da Azure Data Studio o da [Visual Studio SQL Server Data Tools](../../ssdt/sql-server-data-tools.md).

Il progetto esistente verrà aperto e il suo contenuto verrà visualizzato nel viewlet **Progetti** per la modifica.

## <a name="create-a-database-project-from-an-existing-database"></a>Creare un progetto database da un database esistente

Nel viewlet **Progetti** selezionare il pulsante **Import Project from Database** (Importa progetto da database) e connettersi a SQL Server.  Dopo che la connessione è stata stabilita, selezionare un database dall'elenco dei database disponibili e impostare il nome del progetto.

Selezionare infine una struttura di destinazione dell'estrazione.  Verrà aperto il nuovo progetto, che conterrà gli script SQL per i contenuti del database selezionato.

## <a name="build-and-publish"></a>Compilare e pubblicare

La distribuzione del progetto di database viene eseguita nell'estensione progetti di database SQL per Azure Data Studio tramite la compilazione del progetto in un [file dell'applicazione livello dati ](../../relational-databases/data-tier-applications/data-tier-applications.md) (DACPAC) e la pubblicazione in una piattaforma supportata. Per altre informazioni su questo processo, vedere [Compilare e pubblicare un progetto](sql-database-project-extension-build.md).

## <a name="schema-compare"></a>Confronto schemi

L'estensione progetti di database SQL interagisce con l'[estensione confronto schemi](schema-compare-extension.md), se installata, per confrontare i contenuti di un progetto con un pacchetto di applicazione livello dati o con un database esistente.  Il confronto degli schemi risultante può essere usato per visualizzare e applicare le differenze dall'origine alla destinazione.

## <a name="next-steps"></a>Passaggi successivi

- [Compilare e pubblicare un progetto con l'estensione progetti di database SQL per Azure Data Studio](sql-database-project-extension-build.md)