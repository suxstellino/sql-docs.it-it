---
description: 'Appendice F: Libreria di cursori ODBC'
title: 'Appendice F: libreria di cursori ODBC | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC cursor library [ODBC], about cursor library
- ODBC cursor library [ODBC]
- cursor library [ODBC], about cursor library
- cursor library [ODBC]
ms.assetid: a03084df-4e48-48ef-917d-4a3fae48a605
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 44fcba1e4c4b2b7f82c99c82399884d7c9bdad85
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212523"
---
# <a name="appendix-f-odbc-cursor-library"></a>Appendice F: Libreria di cursori ODBC
> [!IMPORTANT]  
>  Questa funzionalità verrà rimossa in una versione futura di Windows. Evitare di utilizzare questa funzionalità nelle nuove attività di sviluppo e pianificare la modifica delle applicazioni che attualmente utilizzano questa funzionalità. Microsoft consiglia di utilizzare la funzionalità di cursore del driver.  
  
 La libreria di cursori ODBC (Odbccr32.dll) supporta i cursori scorrevoli a blocchi per qualsiasi driver conforme al livello di conformità dell'API di livello 1 e può essere ridistribuito dagli sviluppatori con le applicazioni o i driver. La libreria di cursori supporta inoltre le istruzioni Update e Delete posizionate per i set di risultati generati dalle istruzioni **Select** . Sebbene supporti solo cursori statici e di sola trasmissione, la libreria di cursori soddisfa le esigenze di molte applicazioni. Inoltre, può fornire buone prestazioni, soprattutto per set di risultati di dimensioni ridotte o medie, e per le applicazioni che non dispongono di un corretto supporto per i cursori.  
  
 La libreria di cursori è una libreria di collegamento dinamico (DLL) che risiede tra Gestione driver e il driver. Quando un'applicazione chiama una funzione, gestione driver chiama la funzione nella libreria di cursori, che esegue la funzione o la chiama nel driver specificato. Per una determinata connessione, un'applicazione specifica se la libreria di cursori è sempre utilizzata, utilizzata se il driver non supporta i cursori scorrevoli o non viene mai usato.  
  
 La libreria di cursori viene visualizzata come driver per Gestione driver. Se la libreria di cursori risiede tra Gestione driver e un driver ODBC *2. x* , la libreria di cursori viene visualizzata come un driver ODBC *2. x* . Se la libreria di cursori risiede tra Gestione driver e un driver ODBC *3. x* , la libreria di cursori viene visualizzata come un driver ODBC *3. x* . Il comportamento esposto dalla libreria di cursori dipende dalla versione del driver che sta utilizzando, ad eccezione degli offset di binding, supportato per i driver ODBC *2. x* e ODBC *3. x* .  
  
 Per implementare cursori a blocchi in **SQLFetch** e **SQLFetchScroll**, la libreria di cursori chiama ripetutamente **SQLFetch** nel driver. Per implementare lo scorrimento, memorizza nella cache i dati recuperati in memoria e nei file su disco. Quando un'applicazione richiede un nuovo set di righe, la libreria di cursori lo recupera secondo necessità dal driver o dalla cache.  
  
 Per implementare le istruzioni Update e Delete posizionate, la libreria di cursori crea un'istruzione **Update** o **Delete** con una clausola **where** che specifica il valore memorizzato nella cache di ogni colonna associata nella riga. Quando viene eseguita un'istruzione UPDATE posizionata, la libreria di cursori aggiorna la cache dai valori nei buffer del set di righe.  
  
 Per ulteriori informazioni sulla libreria di cursori ODBC, vedere le sezioni seguenti di questa appendice:  
  
-   [Uso della libreria di cursori ODBC](../../../odbc/reference/appendixes/using-the-odbc-cursor-library.md)  
  
-   [Esecuzione di istruzioni di eliminazione e aggiornamento posizionato](../../../odbc/reference/appendixes/executing-positioned-update-and-delete-statements.md)  
  
-   [Esempio di codice della libreria di cursori](../../../odbc/reference/appendixes/cursor-library-code-example.md)  
  
-   [Note sull'implementazione](../../../odbc/reference/appendixes/implementation-notes.md)  
  
-   [Codici di errore della libreria di cursori ODBC](../../../odbc/reference/appendixes/odbc-cursor-library-error-codes.md)
