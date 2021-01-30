---
description: Tipi di dati dBASE
title: Tipi di dati dBASE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- Jet-based ODBC drivers [ODBC], DBasedriver
- desktop database drivers [ODBC], DBasedriver
- DBase driver [ODBC], data types
- data types [ODBC], DBase driver
- dbase data types [ODBC]
- ODBC desktop database drivers [ODBC], DBasedriver
ms.assetid: a0e31e6b-d02b-4ee2-9b37-5baf6a11c0a6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4ef5b42157a011bf719614b7378cd73733e28afc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165424"
---
# <a name="dbase-data-types"></a>Tipi di dati dBASE
Nella tabella seguente viene illustrato come viene eseguito il mapping dei tipi di dati dBASE ai tipi di dati SQL ODBC. Si noti che non tutti i tipi di dati ODBC SQL sono supportati.  
  
|tipo di dati dBASE|Tipo di dati ODBC|  
|---------------------|--------------------|  
|CHAR|SQL_VARCHAR|  
|DATE|SQL_DATE|  
|FLOAT [1]|SQL_DOUBLE|  
|LOGICO|SQL_BIT|  
|MEMO|SQL_LONGVARCHAR|  
|NUMERICO (BCD)|SQL_DOUBLE|  
|OLEOBJECT [1]|SQL_LONGBINARY|  
  
 [1] valido solo per dBASE versione 5. *x*  
  
 La precisione in dBASE III consente i numeri con esponenti fino a due cifre e in numeri di dBASE IV con esponenti fino a tre cifre. Poiché i numeri vengono archiviati come testo, vengono convertiti in numeri. Se il numero da convertire non rientra in un campo, è possibile che si verifichino risultati inspiegabili.  
  
 Mentre dBASE consente di specificare una precisione e una scala con un tipo di dati numerico, non è supportata dal driver dBASE ODBC. Il driver dBASE ODBC restituisce sempre una precisione di 15 e una scala di 0 per un tipo di dati numerico.  
  
 Una colonna creata con il tipo di dati numerico che utilizza il driver dBASE ODBC esegue il mapping al tipo di dati ODBC SQL_DOUBLE. I dati in questa colonna sono pertanto soggetti a arrotondamento. Questo comportamento non corrisponde a quello del tipo di dati numerico in dBASE (tipo N), che è Binary Coded Decimal (BCD).  
  
> [!NOTE]  
>  **SQLGetTypeInfo** restituisce tipi di dati SQL ODBC. Tutte le conversioni nell'Appendice D di *ODBC Programmer ' s Reference* sono supportate per i tipi di dati ODBC SQL elencati in precedenza in questo argomento.  
  
 Nella tabella seguente vengono illustrate le limitazioni relative ai tipi di dati dBASE.  
  
|Tipo di dati|Descrizione|  
|---------------|-----------------|  
|CHAR|La creazione di una colonna CHAR con una lunghezza zero o non specificata restituisce effettivamente una colonna a 254 byte.|  
|Dati crittografati|Il driver dBASE non supporta le tabelle dBASE crittografate.|  
|LOGICO|Il driver dBASE non è in grado di creare un indice in una colonna logica.|  
|MEMO|La lunghezza massima di una colonna di promemoria è di 65.500 byte.|  
  
 Per altre limitazioni sui tipi di dati, vedere [limitazioni del tipo di dati](../../odbc/microsoft/data-type-limitations.md).
