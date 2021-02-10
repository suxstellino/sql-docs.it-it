---
description: Nuove funzionalità di GUI in SSMA per Sybase (SybaseToSQL)
title: Nuove funzionalità di GUI in SSMA per Sybase (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: d3c60e8c-f0a7-4590-8ece-c68ceaeaea4a
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 7a05337a256775e32cf015e108db07b378346378
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068747"
---
# <a name="new-gui-features-in-ssma-for-sybase-sybasetosql"></a>Nuove funzionalità di GUI in SSMA per Sybase (SybaseToSQL)
Questo capitolo descrive le nuove funzionalità dell'interfaccia utente di SSMA.  
  
## <a name="layouts"></a>Layout  
Questa funzionalità consente di scegliere uno dei due lay-out predefiniti di Windows o di creare un layout personalizzato. Per accedere al sottomenu layout, scegliere layout dal menu Visualizza. Qui è possibile scegliere uno dei layout esistenti, aggiungere nuovo layout o gestire i layout.  
  
### <a name="add-current-layout"></a>Aggiungi layout corrente  
Per salvare il layout di Windows corrente, scegliere layout dal menu Visualizza, quindi fare clic su Aggiungi layout corrente.  
  
### <a name="choose-predefined-layout"></a>Scegliere il layout predefinito  
Per scegliere uno dei layout predefiniti, scegliere layout dal menu Visualizza, quindi fare clic su layout predefinito o su senza Esplora. È anche possibile usare i tasti di scelta rapida CTRL + ALT + 1 o CTRL + ALT + 2 per i layout predefiniti.  
  
### <a name="choose-user-defined-layout"></a>Scegliere il layout definito dall'utente  
Per scegliere il layout definito dall'utente, scegliere layout dal menu Visualizza, quindi fare clic su uno dei layout definiti dall'utente. È anche possibile usare i tasti di scelta rapida definiti per i layout.  
  
### <a name="manage-layouts"></a>Gestisci layout  
Per aprire la finestra di dialogo Gestisci layout, scegliere layout dal menu Visualizza, quindi fare clic su Gestisci layout. Nella finestra di dialogo Gestisci layout sarà disponibile un elenco dei layout esistenti sul lato sinistro della finestra di dialogo. Qui è possibile selezionare il layout per modificarne le impostazioni. È anche possibile modificare l'ordine dei layout nell'elenco o eliminare il layout usando i pulsanti nella parte superiore dell'elenco. Sul lato destro della finestra di dialogo è possibile modificare le impostazioni di layout seguenti:  
  
-   Nome del layout  
  
-   Sincronizzazione di Esplora metadati  
  
-   Visibilità e larghezza degli Esplora metadati di origine e di destinazione  
  
-   Visibilità delle finestre di origine o di destinazione e relative dimensioni  
  
-   Visibilità e altezza delle finestre ausiliarie  
  
## <a name="bookmarks"></a>Segnalibri  
Questa funzionalità consente di impostare uno o più segnalibri nel codice di origine o di destinazione, di trovare rapidamente un segnalibro usando i collegamenti, di gestire i segnalibri con una finestra di dialogo intuitiva.  
  
### <a name="toggle-bookmark"></a>Attiva/Disattiva segnalibro  
È possibile impostare o rimuovere un segnalibro nei modi seguenti:  
  
-   Pulsante di attivazione/disattivazione del pulsante nella parte superiore della finestra SQL di origine o di destinazione  
  
-   Fare clic sull'area grigia a sinistra della finestra SQL  
  
-   Usare CTRL + MAIUSC + &lt; 0.. 9 &gt; per impostare il segnalibro numerato  
  
### <a name="bookmark-navigation"></a>Spostamento segnalibro  
È possibile esaminare i segnalibri nei modi seguenti:  
  
-   Usare i pulsanti segnalibro successivo, segnalibro precedente nella parte superiore della finestra SQL  
  
-   Usare CTRL + &lt; 0.. 9 &gt; per trovare un segnalibro numerato  
  
-   Usare i pulsanti Vai a o Visualizza origine nella finestra di dialogo Gestisci segnalibri  
  
### <a name="removing-bookmark"></a>Rimozione del segnalibro  
È possibile rimuovere un segnalibro nei modi seguenti:  
  
-   Usare il pulsante Cancella nella parte superiore della finestra SQL per rimuovere tutti i segnalibri nel documento corrente  
  
-   Usare i pulsanti Rimuovi o Rimuovi tutto nella finestra di dialogo Gestisci segnalibri  
  
### <a name="manage-bookmarks"></a>Gestire i segnalibri  
Per aprire la finestra di dialogo Gestisci segnalibri, nel menu Modifica fare clic su Gestisci segnalibri. Nella finestra di dialogo viene visualizzato un elenco di segnalibri esistenti. È possibile utilizzare i pulsanti sul lato destro della finestra di dialogo per gestire i segnalibri.  
  
## <a name="object-history"></a>Cronologia oggetti  
La cronologia degli oggetti GUI offre i vantaggi seguenti quando si navigano gli oggetti:  
  
-   Per spostarsi tra gli oggetti già visitati, è possibile utilizzare i pulsanti torna indietro e vai in secondo piano  
  
-   Quando si torna all'oggetto, si torna alla stessa scheda a sinistra  
  
-   Quando si torna all'oggetto e la scheda è SQL, si torna alla stessa posizione del cursore a sinistra  
  
## <a name="advanced-search-capabilities"></a>Funzionalità di ricerca avanzate  
Le funzionalità di ricerca avanzate forniscono funzionalità di ricerca avanzate e flessibili e consentono di trovare la dichiarazione dell'oggetto, ottenere informazioni sugli oggetti, eseguire ricerche rapide, eseguire ricerche di oggetti avanzati nelle categorie usando modelli e così via.  
  
### <a name="get-quick-information"></a>Ottenere informazioni rapide  
È possibile ottenere informazioni rapide sull'oggetto in corrispondenza della posizione del cursore nei modi seguenti:  
  
-   Fare clic sul pulsante informazioni rapide nella parte superiore della finestra SQL  
  
-   Selezionare informazioni rapide nel menu a comparsa con il pulsante destro del mouse  
  
-   Premere CTRL + MAIUSC + barra spaziatrice  
  
### <a name="find-declaration"></a>Trova dichiarazione  
È possibile passare alla dichiarazione dell'oggetto in corrispondenza della posizione del cursore nei modi seguenti:  
  
-   Fare clic sul pulsante Vai alla dichiarazione nella parte superiore della finestra SQL  
  
-   Selezionare Vai a dichiarazione nel menu a comparsa con il pulsante destro del mouse  
  
-   Premere F12  
  
### <a name="quick-search"></a>Ricerca rapida  
È possibile eseguire la ricerca di testo rapido usando le funzionalità seguenti:  
  
-   È possibile avviare la ricerca usando il tasto di scelta rapida CTRL + F  
  
-   È possibile ripetere l'ultima ricerca in futuro utilizzando F3  
  
-   È possibile ripetere l'ultima ricerca indietro utilizzando MAIUSC + F3  
  
-   È possibile trovare l'occorrenza successiva della parola nella posizione del cursore usando Ctrl + F3  
  
-   È possibile trovare l'occorrenza precedente della parola in corrispondenza della posizione del cursore utilizzando CTRL + MAIUSC + F3  
  
-   È anche possibile eseguire tutte queste operazioni con le voci di menu.  
  
### <a name="advanced-search"></a>Ricerca avanzata  
Per aprire la finestra di dialogo ricerca avanzata, nel punto di menu Modifica trova, quindi fare clic su ricerca avanzata. Nella finestra di dialogo sarà possibile trovare qualsiasi oggetto usando il modello. Nella parte superiore della finestra di dialogo è possibile scegliere area di ricerca e categorie di oggetti.  
  
