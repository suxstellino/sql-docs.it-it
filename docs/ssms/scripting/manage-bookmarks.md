---
title: Gestione di segnalibri
description: La finestra Segnalibri in un editor del codice consente di creare collegamenti a posizioni nel codice. Informazioni su come creare, eliminare, attivare e disabilitare segnalibri e su come usarli per spostarsi all'interno del codice.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- bookmarks [SQL Server Management Studio]
ms.assetid: 67cc3fd6-3238-4c58-a3ec-2d3b0438143a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3828647bddd0755a5f537fe0158d07a67d3c13d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271728"
---
# <a name="manage-bookmarks"></a>Gestione di segnalibri

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Quando si lavora in un editor di codice, la finestra **Segnalibri** consente di creare collegamenti a righe di codice specifiche all'interno del documento. Per visualizzare questa finestra, usare il menu **Visualizza** .  
  
 Per creare e selezionare i segnalibri, fare clic sui pulsanti disponibili sulla barra degli strumenti **Editor di testo** e nella parte superiore della finestra **Segnalibri** . È possibile aggiungere e rimuovere segnalibri, attivare o disabilitare segnalibri e organizzare i segnalibri in cartelle. Alcuni comandi sono anche disponibili dal menu di scelta rapida della finestra **Segnalibri** . Per aggiungere o rimuovere un segnalibro, posizionare il punto di inserimento nella riga desiderata all'interno dell'editor e quindi fare clic su **Attiva/Disattiva segnalibro**. Per attivare un segnalibro, selezionare la relativa casella di controllo nella finestra **Segnalibri** . Per disabilitare un segnalibro senza rimuoverlo, deselezionare la relativa casella di controllo.  
  
## <a name="text-editor-toolbar"></a>Barra degli strumenti Editor di testo  
 I pulsanti seguenti risultano abilitati nella barra degli strumenti **Editor di testo** quando viene aperto un documento di testo nell'editor. Per visualizzare la barra degli strumenti **Editor di testo** nell'editor di query, scegliere **Barre degli strumenti** dal menu **Visualizza**, quindi fare clic su **Editor di testo**.  
  
 **Attiva o disattiva un segnalibro nella riga corrente**  
 Consente di aggiungere o rimuovere un segnalibro dalla riga del documento selezionata nell'editor attivo. La riga di codice con il segnalibro non viene modificata.  
  
 **Sposta il punto di inserimento al segnalibro precedente**  
 Seleziona il segnalibro precedente abilitato nella finestra **Segnalibri** . Quando viene raggiunto il primo segnalibro, la selezione si sposta rapidamente all'ultimo segnalibro. Se appropriato, viene aperto il file in cui compare il segnalibro selezionato nell'editor. Viene eseguito lo scorrimento del documento fino alla riga con il segnalibro e il punto di inserimento viene posizionato in tale riga.  
  
 **Sposta il punto di inserimento al segnalibro successivo**  
 Seleziona il segnalibro successivo abilitato nella finestra **Segnalibri** . Quando viene raggiunto l'ultimo segnalibro, la selezione si sposta rapidamente al primo segnalibro. Se appropriato, viene aperto il file in cui compare il segnalibro selezionato nell'editor. Viene eseguito lo scorrimento del documento fino alla riga con il segnalibro e il punto di inserimento viene posizionato in tale riga.  
  
 **Cancella tutti i segnalibri nel documento corrente**  
 Viene visualizzato un messaggio di conferma e vengono quindi rimossi tutti i segnalibri dal documento attivo. Le righe di codice con segnalibri non vengono rimosse.  
  
> [!CAUTION]  
>  Non è possibile annullare questa procedura. Per creare nuovi segnalibri, in seguito sarà necessario usare **Attiva o disattiva un segnalibro nella riga corrente** . Per disabilitare i segnalibri senza rimuoverli, deselezionare le relative caselle di controllo nella finestra **Segnalibri** .  
  
## <a name="bookmarks-window"></a>Finestra Segnalibri  
 Per organizzare i segnalibri, creare cartelle di segnalibri nella finestra **Segnalibri** . Trascinare e rilasciare i segnalibri nelle cartelle. I pulsanti seguenti sono disponibili nella parte superiore della finestra **Segnalibri** .  
  
 **Attiva o disattiva un segnalibro nella riga corrente**  
 Consente di aggiungere o rimuovere un segnalibro dalla riga del documento selezionata nell'editor attivo. La riga di codice con il segnalibro non viene modificata.  
  
 **Nuova cartella**  
 Aggiunge una cartella nella finestra **Segnalibri** .  
  
> [!TIP]  
>  In caso di file di codice particolarmente lunghi, può risultare utile organizzare i segnalibri in cartelle correlate alle attività. La selezione di una cartella consente di abilitare i pulsanti **Vai al segnalibro precedente nella cartella** e **Vai al segnalibro successivo nella cartella** .  
  
 **Sposta il punto di inserimento al segnalibro precedente**  
 Seleziona il segnalibro precedente abilitato nella finestra **Segnalibri** . Quando viene raggiunto il primo segnalibro, la selezione si sposta rapidamente all'ultimo segnalibro. Se appropriato, viene aperto il file in cui compare il segnalibro selezionato nell'editor. Viene eseguito lo scorrimento del documento fino alla riga con il segnalibro e il punto di inserimento viene posizionato in tale riga.  
  
 **Sposta il punto di inserimento al segnalibro successivo**  
 Seleziona il segnalibro successivo abilitato nella finestra **Segnalibri** . Quando viene raggiunto l'ultimo segnalibro, la selezione si sposta rapidamente al primo segnalibro. Se appropriato, viene aperto il file in cui compare il segnalibro selezionato nell'editor. Viene eseguito lo scorrimento del documento fino alla riga con il segnalibro e il punto di inserimento viene posizionato in tale riga.  
  
 **Sposta il punto di inserimento al segnalibro precedente nella cartella corrente**  
 Seleziona il segnalibro precedente abilitato all'interno della stessa cartella nella finestra **Segnalibri** . Quando viene raggiunto il primo segnalibro, la selezione si sposta rapidamente all'ultimo segnalibro presente nella cartella. Se appropriato, viene aperto il file in cui compare il segnalibro selezionato nell'editor. Viene eseguito lo scorrimento del documento fino alla riga con il segnalibro e il punto di inserimento viene posizionato in tale riga.  
  
 **Sposta il punto di inserimento al segnalibro successivo nella cartella corrente**  
 Seleziona il segnalibro successivo abilitato all'interno della stessa cartella nella finestra **Segnalibri** . Quando viene raggiunto il primo segnalibro, la selezione si sposta rapidamente all'ultimo segnalibro presente nella cartella. Se appropriato, viene aperto il file in cui compare il segnalibro selezionato nell'editor. Viene eseguito lo scorrimento del documento fino alla riga con il segnalibro e il punto di inserimento viene posizionato in tale riga.  
  
 **Abilita/Disabilita tutti i segnalibri**  
 Deseleziona o abilita le caselle di controllo per tutti i segnalibri nella finestra **Segnalibri** . I segnalibri non vengono rimossi e le righe di codice identificate dai segnalibri non vengono modificate.  
  
 **Elimina**  
 Rimuove il segnalibro attualmente selezionato dalla finestra **Segnalibri** e dal documento in cui il segnalibro è stato inserito. La riga di codice corrispondente non viene rimossa.  
  
 Caselle di controllo dei segnalibri  
 Ogni segnalibro dispone di una relativa casella di controllo. Per attivare un segnalibro esistente, selezionare la relativa casella di controllo nella finestra **Segnalibri** . Per nascondere un segnalibro esistente senza rimuoverlo, deselezionare la relativa casella di controllo nella finestra **Segnalibri** .  
  
## <a name="bookmarks-window-shortcut-menu"></a>Menu di scelta rapida della finestra Segnalibri  
 Quando si fa clic con il pulsante destro del mouse su una voce nella finestra **Segnalibri** , vengono resi disponibili i seguenti comandi nel menu di scelta rapida.  
  
 **Elimina**  
 Rimuove il segnalibro attualmente selezionato dalla finestra **Segnalibri** e dal documento in cui il segnalibro è stato inserito. La riga di codice corrispondente non viene rimossa.  
  
 **Rinominare**  
 Consente di assegnare un nuovo nome visualizzato a un segnalibro o a una cartella.  
  
 **Abilita/Disabilita segnalibro**  
 Deseleziona o seleziona la casella di controllo per il segnalibro selezionato nella finestra **Segnalibri** . Il segnalibro non viene rimosso e la riga di codice identificata dal segnalibro non viene modificata.  
  
 **Abilita/Disabilita tutti i segnalibri**  
 Deseleziona o abilita le caselle di controllo per tutti i segnalibri nella finestra **Segnalibri** . I segnalibri non vengono rimossi e le righe di codice identificate dai segnalibri non vengono modificate.  
  
## <a name="see-also"></a>Vedere anche  
 [Tasti di scelta rapida di SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md)  
