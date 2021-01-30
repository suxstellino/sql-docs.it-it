---
description: Tipi di dati file di testo
title: Tipi di dati del file di testo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- text file driver [ODBC], data types
- ODBC desktop database drivers [ODBC], text file driver
- desktop database drivers [ODBC], text file driver
- text file data types [ODBC]
- Jet-based ODBC drivers [ODBC], text file driver
ms.assetid: e113112e-ae42-469e-8e4b-a365a10d9071
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b1e447d2f20c8efb5e0c0cddc793baf73384e556
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182537"
---
# <a name="text-file-data-types"></a>Tipi di dati file di testo
Nella tabella seguente viene illustrato come viene eseguito il mapping dei tipi di dati di testo ai tipi di dati SQL ODBC. Si noti che non tutti i tipi di dati ODBC SQL sono supportati dal driver di testo ODBC.  
  
|Tipo di dati text|Tipo di dati ODBC|  
|--------------------|--------------------|  
|CHAR|SQL_VARCHAR|  
|DATETIME|SQL_TIMESTAMP|  
|FLOAT|SQL_DOUBLE|  
|INTEGER|SQL_INTEGER|  
|LONGCHAR|SQL_LONGVARCHAR|  
  
> [!NOTE]  
>  **SQLGetTypeInfo** restituisce tipi di dati ODBC. Tutte le conversioni nell'Appendice D di *ODBC Programmer ' s Reference* sono supportate per i tipi di dati SQL elencati nella tabella precedente.  
  
 Nella tabella seguente vengono illustrate le limitazioni relative ai tipi di dati di testo.  
  
|Tipo di dati|Descrizione|  
|---------------|-----------------|  
|CHAR|La creazione di una colonna CHAR con una lunghezza zero o non specificata restituisce effettivamente una colonna a 255 bit.<br /><br /> Nei file delimitati è possibile che una colonna CHAR non includa delimitatori di virgolette doppie all'inizio e alla fine. nei file a lunghezza fissa, le virgolette doppie non vengono utilizzate come delimitatori.|  
|DATETIME|MM-DD-YY (ad esempio, 01-17-92)<br /><br /> MMM-DD-AA (ad esempio, Jan-17-92)<br /><br /> DD-MMM-AA (ad esempio, 17-Jan-92)<br /><br /> AAAA-MM-GG (ad esempio, 1992-01-17)<br /><br /> AAAA-MMM-DD (ad esempio, 1992-Jan-17)<br /><br /> I separatori di data misti non sono consentiti all'interno di una tabella.<br /><br /> Il testo ISAM formatta un campo DATETIME nel formato Stati Uniti o europeo, a seconda dell'impostazione internazionale nel pannello di controllo di Windows.|  
|FLOAT|La larghezza massima include il segno e la virgola decimale. In Schema.ini, la larghezza è indicata nel modo seguente:<br /><br /> 14,083 è a larghezza mobile 6<br /><br /> -14,083 è di larghezza mobile 7<br /><br /> + 14,083 è di larghezza mobile 7<br /><br /> 14083. è a larghezza mobile 6<br /><br /> ODBC restituisce sempre 8 per le colonne FLOAT.<br /><br /> Le colonne FLOAT possono anche essere in notazione scientifica, ad esempio:<br /><br /> -3.04 e + 2 è una larghezza mobile 8<br /><br /> 25E4 è di larghezza mobile 4<br /><br /> **Nota** Non è possibile combinare la notazione decimale e scientifica in una colonna.<br /><br /> I valori NULL sono rappresentati da una stringa riempita vuota nei file a lunghezza fissa e vengono omessi nei file delimitati.<br /><br /> I dati float possono essere riempiti con spazi vuoti iniziali.|  
|INTEGER|I valori validi per le colonne INTEGER sono compresi tra 32767 e 32766.<br /><br /> In Schema.ini, la larghezza è indicata nel modo seguente:<br /><br /> 14083 è di lunghezza intera 5<br /><br /> 0 è una larghezza INTEGER 1<br /><br /> ODBC restituisce sempre 4 per le colonne di tipo INTEGER.<br /><br /> La larghezza massima include un segno. La larghezza massima di una colonna INTEGER è 11, sebbene la larghezza possa essere maggiore a causa di spazi vuoti consentiti nelle tabelle a formato fisso.|  
|LONGCHAR|Il limite teorico per la larghezza di una colonna LONGCHAR in una tabella a lunghezza fissa o delimitata è 65500K. È più probabile che il testo ISAM fornisca un supporto affidabile fino a circa 32K.|  
  
 Per altre limitazioni sui tipi di dati, vedere [limitazioni del tipo di dati](../../odbc/microsoft/data-type-limitations.md).
