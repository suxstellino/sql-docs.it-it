---
description: Costruzione di istruzioni di ricerca
title: Costruzione di istruzioni ricercate | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- searched statements [ODBC]
- ODBC cursor library [ODBC], statement processing
- ODBC cursor library [ODBC], searched statements
- SQL statements [ODBC], cursor library
- cursor library [ODBC], statement processing
- cursor library [ODBC], searched statements
- SQL statements [ODBC], searched statements
ms.assetid: e429254c-c43f-4fbf-98b2-5f1ed53501ff
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a7928e3c730a7431d7f94a90a85073a3c818ae31
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165393"
---
# <a name="constructing-searched-statements"></a>Costruzione di istruzioni di ricerca
> [!IMPORTANT]  
>  Questa funzionalità verrà rimossa in una versione futura di Windows. Evitare di utilizzare questa funzionalità nelle nuove attività di sviluppo e pianificare la modifica delle applicazioni che attualmente utilizzano questa funzionalità. Microsoft consiglia di utilizzare la funzionalità di cursore del driver.  
  
 Per supportare le istruzioni Update e Delete posizionate, la libreria di cursori crea un'istruzione di **aggiornamento** o **eliminazione** ricercata dall'istruzione posizionata. Per supportare le chiamate a **SQLGetData** in un blocco di dati, la libreria di cursori crea un'istruzione **SELECT** di ricerca per creare un set di risultati contenente la riga di dati corrente. In ognuna di queste istruzioni, la clausola **where** enumera i valori archiviati nella cache per ogni colonna associata che restituisce SQL_PRED_SEARCHABLE o SQL_PRED_BASIC per l'identificatore del campo SQL_DESC_SEARCHABLE in **SQLColAttribute**.  
  
> [!CAUTION]  
>  La clausola **where** costruita dalla libreria di cursori per identificare la riga corrente può non essere in grado di identificare le righe, identificare una riga diversa o identificare più di una riga.  
  
 Se un'istruzione Update o DELETE posizionata ha effetto su più di una riga, la libreria di cursori aggiorna la matrice di stato della riga solo per la riga in cui è posizionato il cursore e restituisce SQL_SUCCESS_WITH_INFO e SQLSTATE 01001 (conflitto dell'operazione del cursore). Se l'istruzione non identifica alcuna riga, la libreria dei cursori non aggiorna la matrice di stato della riga e restituisce SQL_SUCCESS_WITH_INFO e SQLSTATE 01001 (conflitto dell'operazione del cursore). Un'applicazione può chiamare **SQLRowCount** per determinare il numero di righe aggiornate o eliminate.  
  
 Se la clausola **Select** utilizzata per posizionare il cursore per una chiamata a **SQLGetData** identifica più di una riga, non è garantito che **SQLGetData** restituisca i dati corretti. Se non identifica alcuna riga, **SQLGetData** restituisce SQL_NO_DATA.  
  
 Se un'applicazione è conforme alle linee guida seguenti, la clausola **where** costruita dalla libreria di cursori deve identificare in modo univoco la riga corrente, tranne quando ciò è impossibile, ad esempio quando l'origine dati contiene righe duplicate.  
  
-   **Associare le colonne che identificano in modo univoco la riga.** Se le colonne con binding non identificano in modo univoco la riga, la clausola **where** costruita dalla libreria di cursori potrebbe identificare più di una riga. In un'istruzione Update o DELETE posizionata, tale clausola può causare l'aggiornamento o l'eliminazione di più righe. In una chiamata a **SQLGetData**, tale clausola può causare la restituzione dei dati per la riga errata da parte del driver. Il binding di tutte le colonne in una chiave univoca garantisce che ogni riga venga identificata in modo univoco.  
  
-   **Allocare buffer di dati sufficientemente grandi che non si verifichino troncamenti.** La cache della libreria di cursori è una copia dei valori nei buffer del set di righe associati al set di risultati con **SQLBindCol**. Se i dati vengono troncati quando vengono posizionati in questi buffer, verranno troncati anche nella cache. Una clausola **where** costruita da valori troncati potrebbe non identificare correttamente la riga sottostante nell'origine dati.  
  
-   **Specificare buffer di lunghezza non null per i dati C binari.** La libreria di cursori alloca buffer di lunghezza nella propria cache solo se l'argomento *StrLen_or_IndPtr* in **SQLBindCol** è diverso da null. Quando l'argomento *targetType* è SQL_C_BINARY, la libreria di cursori richiede la lunghezza dei dati binari per costruire una clausola **where** dai dati. Se non esiste un buffer di lunghezza per una colonna di SQL_C_BINARY e l'applicazione chiama **SQLGetData** o tenta di eseguire un'istruzione Update o DELETE posizionata, la libreria di cursori restituisce SQL_ERROR e SQLSTATE SL014 (è stata eseguita una richiesta posizionata e non tutti i campi di conteggio delle colonne sono stati memorizzati nel buffer).  
  
-   **Specificare buffer di lunghezza non null per le colonne nullable.** La libreria di cursori alloca buffer di lunghezza nella propria cache solo se l'argomento *StrLen_or_IndPtr* in **SQLBindCol** è diverso da null. Poiché SQL_NULL_DATA viene archiviato nel buffer di lunghezza, la libreria di cursori presuppone che qualsiasi colonna per la quale non sia specificato alcun buffer di lunghezza non ammette i valori null. Se per una colonna nullable non viene specificata alcuna colonna di lunghezza, la libreria di cursori costruisce una clausola **where** che utilizza il valore dei dati per la colonna. Questa clausola non identificherà correttamente la riga.
