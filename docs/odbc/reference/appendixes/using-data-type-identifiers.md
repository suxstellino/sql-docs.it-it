---
description: Uso degli identificatori dei tipi di dati
title: Utilizzo degli identificatori dei tipi di dati | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], identifiers
- identifiers [ODBC], data types
ms.assetid: 467e0c0c-a818-4737-8a24-3d8e15c7e162
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cd38f5ddb62a28bc3ec2658dca621769e13ab481
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202433"
---
# <a name="using-data-type-identifiers"></a>Uso degli identificatori dei tipi di dati
Le applicazioni utilizzano gli identificatori dei tipi di dati in due modi: per descrivere i buffer al driver e per recuperare i metadati relativi al set di risultati dal driver, in modo che possano determinare il tipo di buffer C da usare per archiviare i dati. Per eseguire queste attività, le applicazioni chiamano le funzioni seguenti:  
  
-   **SQLBindParameter**, **SQLBindCol** e **SQLGetData** : per descrivere il tipo di dati C dei buffer di applicazione.  
  
-   **SQLBindParameter** : per descrivere il tipo di dati SQL di parametri dinamici.  
  
-   **SQLColAttribute** e **SQLDescribeCol** : per recuperare i tipi di dati SQL delle colonne del set di risultati.  
  
-   **SQLDescribeParameter** : per recuperare i tipi di dati SQL dei parametri.  
  
-   **SQLColumns**, **SQLProcedureColumns** e **SQLSpecialColumns** : per recuperare i tipi di dati SQL di varie informazioni sullo schema  
  
-   **SQLGetTypeInfo** -per recuperare un elenco dei tipi di dati supportati  
  
 Gli identificatori dei tipi di dati vengono archiviati nel campo SQL_DESC_CONCISE_TYPE di un descrittore. Le funzioni descrittore **SQLSetDescField** e **SQLSetDescRec** possono essere usate con i tipi appropriati per eseguire le attività elencate nell'elenco precedente. Per ulteriori informazioni, vedere [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).
