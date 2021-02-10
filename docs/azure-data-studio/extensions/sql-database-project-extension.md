---
title: Estensione progetti di database SQL
description: Installare e usare l'estensione progetti di database SQL per Azure Data Studio.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: ''
ms.date: 12/15/2020
ms.openlocfilehash: b49359ea35a5fbd0f8ffd51606c0ae17f14c9eac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048405"
---
# <a name="sql-database-projects-extension-preview"></a>Estensione progetti di database SQL (anteprima)

L'estensione progetti di database SQL (anteprima) è un'estensione per lo sviluppo di database SQL in un ambiente di sviluppo basato su progetto. 


## <a name="features"></a>Funzionalità

1. Creare un progetto da un database connesso.
2. Creare un nuovo progetto vuoto.
3. Aprire un progetto creato in precedenza in [Azure Data Studio](sql-database-project-extension-getting-started.md) o in [SQL Server Data Tools](../../ssdt/sql-server-data-tools.md).
4. Modificare il progetto aggiungendo o rimuovendo una tabella, una vista, una stored procedure o script personalizzati nel progetto.
5. Organizzare file/script in cartelle.
6. Aggiungere riferimenti a database di sistema o al file DACPAC utente.
7. Compilare un progetto singolo.
8. Distribuire un progetto singolo.
9. Caricare i dettagli della connessione (autenticazione di Windows per SQL) e le variabili di SQLCMD dal profilo di distribuzione.

Guardare questo breve video di 10 minuti per un'introduzione all'estensione progetti di database SQL in Azure Data Studio:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Build-SQL-Database-Projects-Easily-in-Azure-Data-Studio/player?WT.mc_id=dataexposed-c9-niner]

## <a name="install-the-sql-database-projects-extension"></a>Installare l'estensione progetti di database SQL

1. Aprire Gestione estensioni per accedere alle estensioni disponibili.  A questo scopo, selezionare l'icona delle estensioni o selezionare **Estensioni** dal menu **Visualizza**.
2. Identificare l'estensione *progetti di database SQL* digitandone il nome, per intero o in parte, nella casella di ricerca delle estensioni. Selezionare un'estensione disponibile per visualizzarne i dettagli.

   ![Installare l'estensione](media/sql-database-projects-extension/install-database-projects.png)

3. Selezionare l'estensione desiderata e **installarla**.
4. Selezionare **Ricarica** per abilitare l'estensione (necessario solo la prima volta che si installa un'estensione).
5. Selezionare l'icona File dalla barra attività oppure selezionare **Explorer** dal menu **Visualizza**. È ora disponibile un nuovo viewlet per **Progetti**.

   > [!NOTE]
   > .NET Core SDK è necessario per la funzionalità di compilazione del progetto e verrà richiesta l'installazione di .NET Core SDK se non viene rilevato dall'estensione.  È possibile scaricare e installare .NET Core SDK (v3.1 o versione successiva) da [https://dotnet.microsoft.com/download/dotnet-core/3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1).

   > [!NOTE]
   > Per usufruire della funzionalità completa, insieme all'estensione progetti di database SQL è consigliabile installare l'[estensione confronto schemi](schema-compare-extension.md).

## <a name="known-limitations"></a>Limitazioni note

- Il caricamento dei file come collegamento non è attualmente supportato nel viewlet di Azure Data Studio, ma i file verranno caricati nel livello principale dell'albero e la compilazione incorporerà questi file come previsto.
- Gli oggetti SQLCLR nel progetto non sono supportati nella versione .NET Core di DacFx.
- Le attività (compilazione/pubblicazione) non sono definite dall'utente.
- Destinazioni di pubblicazioni definite da DacFx.
- Il supporto per l'ambiente WSL è limitato.

## <a name="workspace"></a>Area di lavoro
I progetti di database SQL in Azure Data Studio sono contenuti all'interno di un'area di lavoro logica.  Un'area di lavoro gestisce le cartelle visibili nel riquadro Esplora e i progetti visibili nel riquadro Progetti. L'aggiunta e la rimozione di progetti da un'area di lavoro possono essere eseguite tramite l'interfaccia di Azure Data Studio nel riquadro Progetti. Le impostazioni per un'area di lavoro possono comunque essere modificate manualmente nel file `.code-workspace`, se necessario.

Nel file di esempio `.code-workspace` riportato di seguito, la matrice `folders` elenca tutte le cartelle incluse nel riquadro Esplora e la matrice `dataworkspace.projects` all'interno di `settings` elenca tutti i progetti SQL inclusi nel riquadro Progetti.

```json
{
    "folders": [
        {
            "path": "."
        },
        {
            "name": "WideWorldImportersDW",
            "path": "..\\WideWorldImportersDW"
        }
    ],
    "settings": {
        "dataworkspace.projects": [
            "AdventureWorksLT.sqlproj",
            "..\\WideWorldImportersDW\\WideWorldImportersDW.sqlproj"
        ]
    }
}
```

## <a name="next-steps"></a>Passaggi successivi

- [Introduzione all'estensione progetti di database SQL](sql-database-project-extension-getting-started.md)
- [Compilare e pubblicare un progetto con l'estensione progetti di database SQL per Azure Data Studio](sql-database-project-extension-build.md)
