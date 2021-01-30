---
description: 'Appendice D: Tipi di dati'
title: 'Appendice D: tipi di dati | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- C data types [ODBC], defined
- SQL data types [ODBC], defined
- data types [ODBC]
- data types [ODBC], about data types
ms.assetid: 981d49c3-3531-4543-aa75-5bd9e4f67000
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b2ff95858f654bef654b18105b4fda52c77cca0c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212549"
---
# <a name="appendix-d-data-types"></a>Appendice D: Tipi di dati
ODBC definisce due set di tipi di dati: tipi di dati SQL e tipi di dati C. I tipi di dati SQL indicano il tipo di dati archiviati nell'origine dati. I tipi di dati C indicano il tipo di dati archiviati nei buffer dell'applicazione.  
  
 I tipi di dati SQL sono definiti da ogni sistema DBMS in base allo standard SQL-92. Per ogni tipo di dati SQL specificato nello standard SQL-92, ODBC definisce un identificatore di tipo, ovvero un valore **#define** che viene passato come argomento nelle funzioni ODBC o restituito nei metadati di un set di risultati. Gli unici tipi di dati SQL-92 non supportati da ODBC sono BIT (il tipo ODBC SQL_BIT presenta caratteristiche diverse), BIT_VARYING, TIME_WITH_TIMEZONE, TIMESTAMP_WITH_TIMEZONE e NATIONAL_CHARACTER. I driver sono responsabili del mapping dei tipi di dati SQL specifici dell'origine dati agli identificatori del tipo di dati SQL ODBC e agli identificatori del tipo di dati SQL specifici del driver. Il tipo di dati SQL viene specificato nel campo SQL_DESC_CONCISE_TYPE di un descrittore di implementazione.  
  
 ODBC definisce i tipi di dati C e i corrispondenti identificatori di tipo ODBC. Un'applicazione specifica il tipo di dati C del buffer che riceverà i dati del set di risultati passando l'identificatore di tipo C appropriato nell'argomento *targetType* in una chiamata a **SQLBindCol** o **SQLGetData**. Specifica il tipo C del buffer contenente un parametro di istruzione passando l'identificatore di tipo C appropriato nell'argomento *ValueType* in una chiamata a **SQLBindParameter**. Il tipo di dati C viene specificato nel campo SQL_DESC_CONCISE_TYPE di un descrittore di applicazione.  
  
> [!NOTE]  
>  Non sono disponibili tipi di dati C specifici del driver.  
  
 Ogni tipo di dati SQL corrisponde a un tipo di dati C ODBC. Prima di restituire dati dall'origine dati, il driver lo converte nel tipo di dati C specificato. Prima di inviare i dati all'origine dati, il driver li converte dal tipo di dati C specificato.  
  
 Questa appendice contiene gli argomenti seguenti.  
  
-   [Uso degli identificatori dei tipi di dati](../../../odbc/reference/appendixes/using-data-type-identifiers.md)  
  
-   [Tipi di dati SQL](../../../odbc/reference/appendixes/sql-data-types.md)  
  
-   [Tipi di dati C](../../../odbc/reference/appendixes/c-data-types.md)  
  
-   [Identificatori e descrittori del tipo di dati](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md)  
  
-   [Identificatori di pseudo-tipo](../../../odbc/reference/appendixes/pseudo-type-identifiers.md)  
  
-   [Trasferimento dei dati in forma binaria](../../../odbc/reference/appendixes/transferring-data-in-its-binary-form.md)  
  
-   [Indicazioni per i tipi di dati intervallo e numerici](../../../odbc/reference/appendixes/guidelines-for-interval-and-numeric-data-types.md)  
  
-   [Vincoli del calendario gregoriano](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md)  
  
-   [Dimensione colonna, cifre decimali, lunghezza trasferimento in ottetti e dimensioni visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md)  
  
-   [Conversione di dati da SQL ai tipi di dati C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md)  
  
-   [Conversione di dati da C ai tipi di dati SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md)  
  
 Per una spiegazione dei tipi di dati ODBC, vedere [tipi di dati in ODBC](../../../odbc/reference/develop-app/data-types-in-odbc.md). Per informazioni sui tipi di dati SQL specifici del driver, vedere la documentazione del driver.
