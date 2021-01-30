---
description: 'Appendice E: Funzioni scalari'
title: 'Appendice E: funzioni scalari | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQL-92 functions [ODBC]
- scalar functions [ODBC]
- functions [ODBC], scalar
ms.assetid: 59c7cd5e-32d6-43ab-bac3-7010322d105a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 94ad0914477de0e2dfd25667755757bbc36a8b29
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212534"
---
# <a name="appendix-e-scalar-functions"></a>Appendice E: Funzioni scalari
ODBC specifica i seguenti tipi di funzioni scalari, con informazioni dettagliate su ognuno di questi tipi di funzione forniti nelle sezioni corrispondenti di questa appendice. Le descrizioni delle funzioni includono la sintassi associata.  
  
 Questa appendice contiene gli argomenti seguenti.  
  
-   [Funzioni per i valori stringa](../../../odbc/reference/appendixes/string-functions.md)  
  
-   [Funzioni numeriche](../../../odbc/reference/appendixes/numeric-functions.md)  
  
-   [Funzioni di data, ora e intervallo](../../../odbc/reference/appendixes/time-date-and-interval-functions.md)  
  
-   [Funzioni di sistema](../../../odbc/reference/appendixes/system-functions.md)  
  
-   [Funzione di conversione esplicita del tipo di dati](../../../odbc/reference/appendixes/explicit-data-type-conversion-function.md)  
  
-   [Funzione SQL-92 CAST](../../../odbc/reference/appendixes/sql-92-cast-function.md)  
  
 ODBC non impone un tipo di dati per i valori restituiti dalle funzioni scalari, perché le funzioni sono spesso specifiche dell'origine dati. Le applicazioni devono utilizzare la funzione CONVERT Scalar, quando possibile, per forzare la conversione del tipo di dati.  
  
## <a name="odbc-and-sql-92-scalar-functions"></a>Funzioni scalari ODBC e SQL-92  
 Le tabelle in questa appendice includono funzioni che sono state aggiunte in ODBC 3,0 per l'allineamento con SQL-92. Le funzioni aggiunte per un particolare tipo di funzione scalare, come definito in ODBC, sono indicate in ogni sezione.  
  
 ODBC e SQL-92 classificano le funzioni scalari in modo diverso. ODBC classifica le funzioni scalari in base al tipo di argomento; SQL-92 li classifica per valore restituito. La funzione EXTRACT, ad esempio, è classificata come funzione di timeout da ODBC, perché l'argomento Extract-Field è una parola chiave DateTime e l'argomento Extract-source è un'espressione datetime o Interval. SQL-92, d'altra parte, classifica l'estrazione come funzione scalare numerica, perché il valore restituito è un valore numerico.  
  
 Un'applicazione può determinare quali funzioni scalari supportano un driver chiamando **SQLGetInfo**. I tipi di informazioni sono inclusi sia per ODBC che per le classificazioni SQL-92 delle funzioni scalari. Poiché queste classificazioni sono diverse, il supporto per alcune funzioni scalari può essere indicato in tipi di informazioni che non corrispondono a ODBC e SQL-92. Il supporto per EXTRACT in ODBC, ad esempio, è indicato dal tipo di informazioni SQL_TIMEDATE_FUNCTIONS; il supporto per l'estrazione in SQL-92, d'altra parte, è indicato dal tipo di informazioni SQL_SQL92_NUMERIC_VALUE_FUNCTIONS.
