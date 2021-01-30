---
description: Lunghezza dei dati di colonna
title: Lunghezza dei dati della colonna | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- column data [ODBC]
- length of column data [ODBC]
- ODBC cursor library [ODBC], cache
- cursor library [ODBC], cache
- cache [ODBC]
ms.assetid: c762c881-ebe0-4eac-84d5-f30281fc3eca
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: be0ad52f040c195d4e8eeab9d307aa8f92bcf62d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184405"
---
# <a name="length-of-column-data"></a>Lunghezza dei dati di colonna
> [!IMPORTANT]  
>  Questa funzionalità verrà rimossa in una versione futura di Windows. Evitare di utilizzare questa funzionalità nelle nuove attività di sviluppo e pianificare la modifica delle applicazioni che attualmente utilizzano questa funzionalità. Microsoft consiglia di utilizzare la funzionalità di cursore del driver.  
  
 La libreria di cursori crea un buffer nella cache per ogni buffer di lunghezza/indicatore associato al set di risultati con **SQLBindCol**. USA i valori in questi buffer per costruire una clausola **where** quando emula le istruzioni Update o DELETE posizionate. Aggiorna questi buffer dai buffer del set di righe quando recupera i dati dall'origine dati e quando esegue istruzioni UPDATE posizionate.  
  
 Se il tipo C di un buffer di dati è SQL_C_CHAR o SQL_C_BINARY e il valore di lunghezza/indicatore è SQL_NTS, la lunghezza della stringa dei dati viene inserita nel buffer di lunghezza/indicatore.  
  
> [!NOTE]  
>  La libreria di cursori non aggiorna la relativa cache per una colonna se **StrLen_or_IndPtr* nel buffer del set di righe corrispondente è SQL_DATA_AT_EXEC o il risultato della macro SQL_LEN_DATA_AT_EXEC.
