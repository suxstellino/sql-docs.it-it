---
title: Gestione della formattazione del codice
description: Informazioni sui tre stili di rientro del testo e su come usarli, oltre a informazioni su come creare URL selezionabili.
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- indenting code [SQL Server]
- displaying URLs
- code formatting [SQL Server Management Studio]
- collapsing text
- formats [SQL Server], code formatting in SQL Server Management Studio
- hiding text
- formats [SQL Server]
- text [SQL Server], code formats
- automatic indentation
- converting text to lower case
- Query Editor [SQL Server Management Studio], managing code formats
- URL displayed in code [SQL Server Management Studio]
- converting text to upper case
- text [SQL Server]
- unindenting code
ms.assetid: ddbac4d2-6bdc-4467-a352-e869ec880eed
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 550f34f022262b24cd7992c7b3ebe056b946f4cc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354626"
---
# <a name="manage-code-formatting"></a>Gestione della formattazione del codice

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

L'editor consente di formattare il codice con rientri, testo nascosto, URL e così via. È inoltre possibile formattare il codice automaticamente durante la digitazione tramite la funzionalità intelligente per la gestione dei rientri.  
  
## <a name="indenting"></a>Stili rientri  
 È possibile scegliere tra tre diversi stili di rientro del testo. È inoltre possibile specificare il numero di spazi di ogni singolo rientro o tabulazione e se per il rientro l'editor deve utilizzare tabulazioni o spazi.  
  
#### <a name="to-choose-an-indenting-style"></a>Per scegliere uno stile di rientro  
  
1.  Scegliere **Opzioni** dal menu **Strumenti**.  
  
2.  Fare clic su **Editor di testo**.  
  
3.  Selezionare la cartella **Tutti i linguaggi** per impostare i rientri per tutti i linguaggi.  
  
4.  Fare clic su **Tabulazioni**.  
  
5.  Selezionare una delle opzioni seguenti:  
  
    -   **Nessuno**. Il cursore si porta all'inizio della riga successiva.  
  
    -   **Blocco**. Il cursore allinea la riga successiva con quella precedente.  
  
    -   **Intelligenti** (impostazione predefinita). Il servizio di linguaggio determina lo stile di rientro appropriato da utilizzare.  
  
    > [!NOTE]  
    >  Non tutte le opzioni di rientro sono disponibili per tutti i linguaggi.  
  
#### <a name="to-change-indent-tab-settings"></a>Per modificare le impostazioni di tabulazione dei rientri  
  
1.  Scegliere **Opzioni** dal menu **Strumenti**.  
  
2.  Fare clic su **Editor di testo**.  
  
3.  Selezionare la cartella **Tutti i linguaggi** per impostare i rientri per tutti i linguaggi.  
  
4.  Fare clic su **Tabulazioni**.  
  
5.  Per specificare i caratteri per tabulazioni e rientri, fare clic su **Mantieni tabulazioni**. Per specificare spazi, selezionare **Inserisci spazi**.  
  
     Se si seleziona **Inserisci spazi**, immettere il numero di spazi per ogni tabulazione o rientro rispettivamente in **Dimensione tabulazione** o **Dimensione rientro**.  
  
#### <a name="to-indent-code"></a>Per rientrare il codice  
  
1.  Selezionare il testo che si desidera rientrare.  
  
2.  Premere TAB o fare clic sul pulsante **Rientra** sulla barra degli strumenti Standard.  
  
#### <a name="to-unindent-code"></a>Per ridurre il rientro del codice  
  
1.  Selezionare il testo a cui applicare la riduzione del rientro.  
  
2.  Premere MAIUSC+TAB, oppure fare clic sul pulsante **Riduci rientro** sulla barra degli strumenti Standard.  
  
#### <a name="to-automatically-indent-all-of-your-code"></a>Per rientrare automaticamente tutto il codice  
  
1.  Scegliere **Opzioni** dal menu **Strumenti**.  
  
2.  Fare clic su **Editor di testo**.  
  
3.  Fare clic su **Tutti i linguaggi**.  
  
4.  Fare clic su **Tabulazioni**.  
  
5.  Scegliere **Intelligenti**.  
  
> [!NOTE]  
>  L'opzione **Intelligenti** non è disponibile per tutti i linguaggi.  
  
#### <a name="to-convert-white-space-to-tabs"></a>Per convertire gli spazi vuoti in tabulazioni  
  
1.  Selezionare il testo in cui si desidera convertire lo spazio vuoto in tabulazioni.  
  
2.  Scegliere **Avanzate** dal menu **Modifica** e quindi fare clic su **Inserisci tabulazione**.  
  
#### <a name="to-convert-tabs-to-spaces"></a>Per convertire le tabulazioni in spazi  
  
1.  Selezionare il testo in cui si desidera convertire le tabulazioni in spazi.  
  
2.  Scegliere **Avanzate** dal menu **Modifica** e quindi fare clic su **Rimuovi tabulazione**.  
  
 Il comportamento di questi comandi dipende dalle impostazioni di tabulazione nella finestra di dialogo **Opzioni** . Se, ad esempio, l'impostazione di tabulazione è 4, **Inserisci tabulazione** comporta la creazione di una tabulazione ogni 4 spazi contigui, mentre **Rimuovi tabulazione** comporta la creazione di 4 spazi per ogni tabulazione.  
  
## <a name="converting-text-to-upper-and-lower-case"></a>Conversione del testo in lettere maiuscole e minuscole  
 Sono previsti dei comandi per convertire un testo in tutte lettere maiuscole o minuscole.  
  
#### <a name="to-switch-text-to-upper-or-lower-case"></a>Per convertire un testo in lettere maiuscole o minuscole  
  
1.  Selezionare il testo che si desidera convertire.  
  
2.  Per convertirlo in lettere maiuscole, premere CTRL+MAIUSC+U, oppure scegliere **Maiuscole** dal sottomenu **Avanzate** del menu **Modifica** .  
  
3.  Per convertirlo in lettere minuscole, premere CTRL+MAIUSC+L, oppure scegliere **Minuscole** dal sottomenu **Avanzate** del menu **Modifica** .  
  
> [!NOTE]  
>  Per un elenco completo dei tasti di scelta rapida, vedere [Tasti di scelta rapida di SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md).  
  
## <a name="displaying-and-linking-to-urls"></a>Visualizzazione e collegamenti agli URL  
 All'interno del codice è possibile creare e visualizzare URL selezionabili. Per impostazione predefinita:  
  
-   Gli URL sono sottolineati.  
  
-   Il puntatore del mouse si trasforma in una mano quando viene spostato su di essi.  
  
-   Se l'URL è valido, viene aperto quando si fa clic su di esso.  
  
#### <a name="to-display-a-clickable-url"></a>Per visualizzare un URL selezionabile  
  
1.  Scegliere **Opzioni** dal menu **Strumenti**.  
  
2.  Fare clic su **Editor di testo**.  
  
3.  Per modificare l'opzione per un solo linguaggio, fare clic sulla cartella corrispondente e quindi scegliere **Generale**. Per modificarla per tutti i linguaggi, fare clic su **Tutti i linguaggi** e quindi scegliere **Generale**.  
  
4.  Selezionare **Consenti navigazione URL con clic singolo**.  
  
  
