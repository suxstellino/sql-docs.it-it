---
description: Tipi di dati datetime
title: Tipi di dati DateTime | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- time data type [ODBC]
- datetime data types [ODBC]
- date data type [ODBC]
- data types [ODBC], date
- backward compatibility [ODBC], datetime data types
- timestamp data type [ODBC]
- data types [ODBC], timestamp
- data types [ODBC], backward compatibility
- compatibility [ODBC], datetime data types
- data types [ODBC], time
ms.assetid: 6b9363c9-04bf-4492-a210-7aa15dea4af8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b2bee41df18a0a05c7b7812c0a23321feec13ed7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194921"
---
# <a name="datetime-data-types"></a>Tipi di dati datetime
In ODBC *3. x*, gli identificatori per i tipi di dati di data, ora e timestamp SQL sono stati modificati da SQL_DATE, SQL_TIME e SQL_TIMESTAMP (con istanze di **#define** nel file di intestazione 9, 10 e 11) per SQL_TYPE_DATE, SQL_TYPE_TIME e SQL_TYPE_TIMESTAMP (con istanze di **#define** nel file di intestazione 91, 92 e 93), rispettivamente. Gli identificatori di tipo C corrispondenti sono stati modificati da SQL_C_DATE, SQL_C_TIME e SQL_C_TIMESTAMP SQL_C_TYPE_DATE, SQL_C_TYPE_TIME e SQL_C_TYPE_TIMESTAMP rispettivamente e le istanze di **#define** sono state modificate di conseguenza.  
  
 Le dimensioni della colonna e le cifre decimali restituite per i tipi di dati DateTime SQL in ODBC *3. x* corrispondono alla precisione e alla scala restituite in ODBC *2. x*. Questi valori sono diversi dai valori nei campi SQL_DESC_PRECISION e descrittore SQL_DESC_SCALE. Per ulteriori informazioni, vedere [dimensioni della colonna, cifre decimali, lunghezza dell'ottetto di trasferimento e dimensioni di visualizzazione](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) in Appendice D: tipi di dati.  
  
 Queste modifiche influiscono su **SQLDescribeCol**, **SQLDescribeParam** e **SQLColAttributes**; **SQLBindCol**, **SQLBindParameter** e **SQLGetData**; e **SQLColumns**, **SQLGetTypeInfo**, **SQLProcedureColumns**, **SQLStatistics** e **SQLSpecialColumns**.  
  
 Un driver ODBC *3. x* elabora le chiamate di funzione elencate nel paragrafo precedente in base all'impostazione dell'attributo SQL_ATTR_ODBC_VERSION Environment. Per **SQLColumns**, **SQLGetTypeInfo**, **SQLProcedureColumns**, **SQLSpecialColumns** e **sqlstatistics**, se SQL_ATTR_ODBC_VERSION è impostato su SQL_OV_ODBC3, le funzioni restituiscono SQL_TYPE_DATE, SQL_TYPE_TIME e SQL_TYPE_TIMESTAMP nel campo data_type. La colonna COLUMN_SIZE (nel set di risultati restituito da **SQLColumns**, **SQLGetTypeInfo**, **SQLProcedureColumns** e **SQLSpecialColumns**) contiene la precisione binaria per il tipo numerico approssimativo. La colonna NUM_PREC_RADIX (nel set di risultati restituito da **SQLColumns**, **SQLGetTypeInfo** e **SQLProcedureColumns**) contiene il valore 2. Se SQL_ATTR_ODBC_VERSION è impostato su SQL_OV_ODBC2, le funzioni restituiscono SQL_DATE, SQL_TIME e SQL_TIMESTAMP nel campo DATA_TYPE, la colonna COLUMN_SIZE contiene la precisione decimale per il tipo numerico approssimativo e la colonna NUM_PREC_RADIX contiene un valore pari a 10.  
  
 Quando tutti i tipi di dati vengono richiesti in una chiamata a **SQLGetTypeInfo**, il set di risultati restituito dalla funzione conterrà sia SQL_TYPE_DATE, SQL_TYPE_TIME e SQL_TYPE_TIMESTAMP come definito in ODBC *3. x* e SQL_DATE, SQL_TIME e SQL_TIMESTAMP come definito in ODBC *2. x*.  
  
 A causa del modo in cui gestione driver ODBC *3. x* esegue il mapping dei tipi di dati data, ora e timestamp, i driver ODBC *3. x* devono riconoscere solo **#defines** di 91, 92 e 93 per i tipi di dati date, Time e timestamp C immessi negli argomenti *targetType* di **SQLBindCol** e **SQLGetData** o dell'argomento *ValueType* di **SQLBindParameter** ed è necessario riconoscere solo **#defines** 91, 92 e 93 per i tipi di dati di data, ora e timestamp SQL immessi nell'argomento *ParameterType* di **SQLBindParameter** o l'argomento *DataType* di **SQLGetTypeInfo**. Per altre informazioni, vedere [modifiche al tipo di dati DateTime](../../../odbc/reference/develop-app/datetime-data-type-changes.md).
