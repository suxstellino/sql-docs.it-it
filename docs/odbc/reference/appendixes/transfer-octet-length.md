---
description: Lunghezza dell'ottetto di trasferimento
title: Lunghezza ottetto di trasferimento | Microsoft Docs
ms.custom: ''
ms.date: 10/28/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- transfer octet length of data types [ODBC]
- size of data types [ODBC]
- SQL data types [ODBC], column characteristics
- data types [ODBC], transfer octet length
ms.assetid: 9fdc9762-e203-4cff-9212-54f450bf18d9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b910f544be952a18ae2961e9939960820e3805bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202489"
---
# <a name="transfer-octet-length"></a>Lunghezza dell'ottetto di trasferimento
La lunghezza di un ottetto di trasferimento di una colonna è il numero massimo di byte restituiti all'applicazione quando i dati vengono trasferiti al tipo di dati C predefinito. Per i dati di tipo carattere, la lunghezza dell'ottetto di trasferimento non include lo spazio per il carattere di terminazione null. La lunghezza di un ottetto di trasferimento di una colonna può essere diversa dal numero di byte necessari per archiviare i dati nell'origine dati.  
  
 La lunghezza dell'ottetto di trasferimento definita per ogni tipo di dati SQL ODBC è illustrata nella tabella seguente.  
  
|Identificatore del tipo SQL|Length|  
|-------------------------|------------|  
|Tutti i tipi di carattere [a]|Lunghezza definita o massima (per il tipo di variabile) della colonna in byte. Si tratta dello stesso valore del campo del descrittore SQL_DESC_OCTET_LENGTH.|  
|SQL_DECIMAL<br />SQL_NUMERIC|Numero di byte necessari per conservare la rappresentazione dei caratteri di questi dati se il set di caratteri è ANSI e due volte questo numero se il set di caratteri è UNICODE. Questo è il numero massimo di cifre più due, perché i dati vengono restituiti come stringa di caratteri e sono necessari caratteri per le cifre, un segno e un separatore decimale. La lunghezza di trasferimento di una colonna definita come NUMERIC (10, 3), ad esempio, è 12.|  
|SQL_TINYINT|1|  
|SQL_SMALLINT|2|  
|SQL_INTEGER|4|  
|SQL_BIGINT| 8 |  
|SQL_REAL|4|  
|SQL_FLOAT|8|  
|SQL_DOUBLE|8|  
|SQL_BIT|1|  
|Tutti i tipi binari [a]|Numero di byte necessari per conservare il numero di caratteri definito (per i tipi fissi) o massimo (per i tipi di variabile).|  
|SQL_TYPE_DATE<br />SQL_TYPE_TIME|6 (dimensione della struttura SQL_DATE_STRUCT o SQL_TIME_STRUCT).|  
|SQL_TYPE_TIMESTAMP|16 (dimensione della struttura di SQL_TIMESTAMP_STRUCT).|  
|Tutti i tipi di dati intervallo|34 (dimensione della struttura intervallo).|  
|SQL_GUID|16 (dimensione della struttura GUID).|  
| &nbsp; | &nbsp; |

 [a] se il driver non è in grado di determinare la lunghezza della colonna o del parametro per i tipi di variabile, restituisce SQL_NO_TOTAL.
