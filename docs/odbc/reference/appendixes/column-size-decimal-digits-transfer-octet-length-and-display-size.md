---
description: Dimensioni colonne, cifre decimali, lunghezza ottetto di trasferimento e dimensioni di visualizzazione-ODBC
title: Dimensioni colonne, cifre decimali, lunghezza ottetto di trasferimento, dimensioni visualizzazione | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- display size of data types [ODBC]
- data types [ODBC], column size
- transfer octet length of data types [ODBC]
- size of data types [ODBC]
- data types [ODBC], display size
- decimal digits of data types [ODBC]
- data types [ODBC], decimal digits
- SQL data types [ODBC], column characteristics
- column size of data types [ODBC]
- data types [ODBC], transfer octet length
ms.assetid: 723107a1-be08-4ea3-a8c0-b2c45d38d1aa
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8a23183fd60d947771cb041af977d518157ac573
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182534"
---
# <a name="column-size-decimal-digits-transfer-octet-length-and-display-size---odbc"></a>Dimensioni colonne, cifre decimali, lunghezza ottetto di trasferimento e dimensioni di visualizzazione-ODBC
I tipi di dati sono caratterizzati dalle dimensioni della colonna (o del parametro), delle cifre decimali, della lunghezza e della dimensione di visualizzazione. Le funzioni ODBC seguenti restituiscono questi attributi per un parametro in un'istruzione SQL o per un tipo di dati SQL in un'origine dati. Ogni funzione ODBC restituisce un set diverso di questi attributi, come indicato di seguito:  
  
-   **SQLDescribeCol** restituisce le dimensioni della colonna e le cifre decimali delle colonne descritte.  
  
-   **SQLDescribeParam** restituisce le dimensioni del parametro e le cifre decimali dei parametri descritti. **SQLBindParameter** imposta le dimensioni del parametro e le cifre decimali per un parametro in un'istruzione SQL.  
  
-   Le funzioni di catalogo **SQLColumns**, **SQLProcedureColumns** e **SQLGetTypeInfo** restituiscono attributi per una colonna in una tabella, un set di risultati o un parametro di routine e gli attributi del catalogo dei tipi di dati nell'origine dati. **SQLColumns** restituisce le dimensioni della colonna, le cifre decimali e la lunghezza di una colonna in tabelle specificate, ad esempio la tabella di base, la vista o una tabella di sistema. **SQLProcedureColumns** restituisce le dimensioni della colonna, le cifre decimali e la lunghezza di una colonna in una procedura. **SQLGetTypeInfo** restituisce le dimensioni massime della colonna e le cifre decimali minime e massime di un tipo di dati SQL in un'origine dati.  
  
 I valori restituiti da queste funzioni per la colonna o le dimensioni del parametro corrispondono a "Precision", come definito in ODBC 2. *x*. Tuttavia, i valori non corrispondono necessariamente ai valori restituiti in SQL_DESC_PRECISION o in un altro campo del descrittore. Lo stesso vale per le cifre decimali, che corrispondono a "scale" come definito in ODBC 2. *x*. Non corrisponde necessariamente ai valori restituiti in SQL_DESC_SCALE o in un altro campo del descrittore, ma deriva da campi di descrizione diversi a seconda del tipo di dati. Per ulteriori informazioni, vedere [dimensioni della colonna](../../../odbc/reference/appendixes/column-size.md) e [cifre decimali](../../../odbc/reference/appendixes/decimal-digits.md).  
  
 Analogamente, i valori per la lunghezza dell'ottetto di trasferimento non provengono da SQL_DESC_LENGTH. Provengono dalla SQL_DESC_OCTET_LENGTH di un campo di un descrittore per tutti i tipi di carattere e binari. Nessun campo del descrittore che contiene queste informazioni per altri tipi.  
  
 Il valore delle dimensioni di visualizzazione per tutti i tipi di dati corrisponde al valore in un singolo campo del descrittore, SQL_DESC_DISPLAY_SIZE.  
  
 I campi del descrittore descrivono le caratteristiche di un set di risultati. I campi di descrizione non contengono valori validi per i dati prima dell'esecuzione dell'istruzione. I valori per le dimensioni della colonna, le cifre decimali e le dimensioni di visualizzazione restituiti da **SQLColumns**, **SQLProcedureColumns** e **SQLGetTypeInfo**, d'altra parte, restituiscono le caratteristiche degli oggetti di database, ad esempio le colonne della tabella e i tipi di dati, presenti nel catalogo dell'origine dati. Analogamente, nel set di risultati **SQLColAttribute** restituisce le dimensioni della colonna, le cifre decimali e la lunghezza dell'ottetto di trasferimento delle colonne nell'origine dati. questi valori non sono necessariamente uguali ai valori nei campi SQL_DESC_PRECISION, SQL_DESC_SCALE e descrittore di SQL_DESC_OCTET_LENGTH.  
  
 Per ulteriori informazioni su questi campi del descrittore, vedere [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).  
  
 Argomenti correlati:  
  
-   [Dimensioni della colonna](../../../odbc/reference/appendixes/column-size.md)  
-   [Cifre decimali](../../../odbc/reference/appendixes/decimal-digits.md)  
-   [Lunghezza dell'ottetto di trasferimento](../../../odbc/reference/appendixes/transfer-octet-length.md)  
-   [Dimensioni di visualizzazione](../../../odbc/reference/appendixes/display-size.md)
