---
title: Estensione SQL Server Import
description: Informazioni su come installare e usare l'estensione SQL Server Import per Azure Data Studio, una procedura guidata che converte i file con estensione txt e csv in una tabella SQL.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 09/22/2020
ms.openlocfilehash: a1a4c9a83aca18ef41960f3c497b75f6de210109
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048381"
---
# <a name="sql-server-import-extension"></a>Estensione SQL Server Import

L'estensione SQL Server Import converte i file con estensione txt e csv in una tabella SQL. Questa procedura guidata usa un framework sviluppato da Microsoft Research noto come [Program Synthesis using Examples (PROSE)](https://microsoft.github.io/prose/) per analizzare in modo intelligente il file con un intervento minimo dell'utente. Si tratta di un framework avanzato per il data wrangling, basato sulla stessa tecnologia usata da Anteprima suggerimenti di Microsoft Excel.

Per altre informazioni sulla versione di questa funzionalità per SSMS, è possibile leggere [questo articolo](../../relational-databases/import-export/import-flat-file-wizard.md).

## <a name="install-the-sql-server-import-extension"></a>Installare l'estensione SQL Server Import.

1. Per aprire Gestione estensioni e accedere alle estensioni disponibili, selezionare l'icona delle estensioni oppure l'opzione **Estensioni** dal menu **Visualizza**.
2. Selezionare un'estensione disponibile per visualizzarne i dettagli.

   ![Gestione estensione Import](media/sql-server-import-extension/import-wizard-install.png)

3. Selezionare l'estensione desiderata e **installarla**.
4. Selezionare **Ricarica** per abilitare l'estensione (necessario solo la prima volta che si installa un'estensione).

## <a name="start-import-wizard"></a>Avviare la procedura guidata di Import

1. Per avviare SQL Server Import, creare prima una connessione a un server nella scheda Server.
2. Dopo aver stabilito una connessione, eseguire il drill-down nel database di destinazione all'interno del quale si vuole importare un file in una tabella SQL.
3. Fare clic con il pulsante destro del mouse sul database e scegliere **Importazione guidata**.

    ![Importazione guidata](media/sql-server-import-extension/open-import-wizard.png)

## <a name="importing-a-file"></a>Importazione di un file

1. Quando si fa clic con il pulsante destro del mouse per avviare la procedura guidata, il server e il database risultano già compilati automaticamente. Se sono presenti altre connessioni attive, è possibile selezionarle nell'elenco a discesa. 

    Selezionare un file scegliendo **Sfoglia**. Il nome della tabella viene compilato automaticamente in base al nome del file, ma è anche possibile modificarlo manualmente.

    Per impostazione predefinita, lo schema sarà dbo, ma anche in questo caso è possibile modificarlo. Selezionare **Avanti** per continuare.

    ![File di input](media/sql-server-import-extension/import-wizard-input-file.png)

2. La procedura guidata genererà un'anteprima basata sulle prime 50 righe. Non sono previste altre azioni in questa pagina, se non verificare che i dati siano corretti. Selezionare **Avanti** per continuare.

    ![Anteprima dati](media/sql-server-import-extension/import-wizard-preview-data.png)

3. In questa pagina è possibile modificare il nome delle colonne e il tipo di dati, nonché indicare se si tratta di una chiave primaria e se sono consentiti valori Null. È possibile apportare tutte le modifiche desiderate. Selezionare **Importa dati** per continuare.

    ![Modificare le colonne](media/sql-server-import-extension/import-wizard-modify-columns.png)

4. Questa pagina fornisce un riepilogo delle azioni scelte e consente di verificare che la tabella sia stata inserita correttamente.

    È possibile selezionare **Completato, Precedente** se è necessario apportare modifiche o **Importa nuovo file** se si vuole importare rapidamente un altro file.

    ![Summary](media/sql-server-import-extension/import-wizard-summary.png)

5. Verificare che la tabella sia stata importata correttamente aggiornando il database di destinazione o eseguendo una query SELECT sul nome della tabella.

## <a name="next-steps"></a>Passaggi successivi

- Per altre informazioni sulla procedura guidata di Import, leggere questo [post di blog](https://cloudblogs.microsoft.com/sqlserver/2018/08/30/the-august-release-of-sql-operations-studio-is-now-available/).
- Per altre informazioni su PROSE, leggere la [documentazione di riferimento](https://microsoft.github.io/prose/).