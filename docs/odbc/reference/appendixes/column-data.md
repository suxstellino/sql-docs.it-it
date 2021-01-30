---
description: Dati della colonna
title: Dati colonna | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- column data [ODBC]
- ODBC cursor library [ODBC], cache
- cursor library [ODBC], cache
- cache [ODBC]
ms.assetid: 0425818c-9469-493f-9e3c-fc03d9411c5c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1287a4deadc6f35bf83f6c42dcebc633f6557fed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212344"
---
# <a name="column-data"></a>Dati della colonna
> [!IMPORTANT]  
>  Questa funzionalità verrà rimossa in una versione futura di Windows. Evitare di utilizzare questa funzionalità nelle nuove attività di sviluppo e pianificare la modifica delle applicazioni che attualmente utilizzano questa funzionalità. Microsoft consiglia di utilizzare la funzionalità di cursore del driver.  
  
 La libreria di cursori crea un buffer nella cache per ogni buffer di dati associato al set di risultati con **SQLBindCol**. USA i valori in questi buffer per costruire una clausola **where** quando emula un'istruzione Update o DELETE posizionata. Aggiorna questi buffer dai buffer del set di righe quando recupera i dati dall'origine dati e quando esegue istruzioni UPDATE posizionate.  
  
 Quando la libreria di cursori aggiorna la cache dai buffer dei set di righe, trasferisce i dati in base al tipo di dati C specificato in **SQLBindCol**. Se, ad esempio, il tipo di dati C di un buffer del set di righe è SQL_C_SLONG, la libreria di cursori trasferisce quattro byte di dati. Se è SQL_C_CHAR e *bufferLength* è 10, la libreria di cursori trasferisce 10 byte di dati. La libreria di cursori non esegue alcun controllo o conversione dei tipi sui dati trasferiti.  
  
> [!NOTE]  
>  La libreria di cursori non aggiorna la relativa cache per una colonna se **StrLen_or_IndPtr* nel buffer del set di righe corrispondente è SQL_DATA_AT_EXEC o il risultato della macro SQL_LEN_DATA_AT_EXEC.  
  
 Quando viene aggiornata una colonna, in un'origine dati vengono rilasciati i dati di tipo carattere a lunghezza fissa e i dati binari a lunghezza fissa zero, se necessario. Ad esempio, un'origine dati archivia "Smith" in una colonna CHAR (10) come "Smith". La libreria di cursori non esegue il riempimento o l'azzeramento dei dati nei buffer dei set di righe quando copia questi dati nella cache dopo l'esecuzione di un'istruzione UPDATE posizionata. Di conseguenza, se un'applicazione richiede che i valori nella cache della libreria di cursori siano riempiti da spazi vuoti o con riempimento zero, è necessario che i valori nei buffer del set di righe vengano riempiti o azzerati prima di eseguire un'istruzione UPDATE posizionata.
