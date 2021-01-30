---
description: Dimensioni di visualizzazione
title: Dimensioni visualizzazione | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- display size of data types [ODBC]
- size of data types [ODBC]
- data types [ODBC], display size
- SQL data types [ODBC], column characteristics
ms.assetid: 9f7f766f-2492-463c-aab7-f2476e222042
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a39a0f986164cf6bc14e8be6e221232dd936e4bb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194879"
---
# <a name="display-size"></a>Dimensioni di visualizzazione
Le dimensioni di visualizzazione di una colonna corrispondono al numero massimo di caratteri necessari per visualizzare i dati in formato carattere. Nella tabella seguente vengono definite le dimensioni di visualizzazione per ogni tipo di dati SQL ODBC.  
  
|Identificatore del tipo SQL|Dimensioni dello schermo|  
|-------------------------|------------------|  
|Tutti i tipi di carattere [a]|Il numero di caratteri definito (per i tipi fissi) o massimo (per i tipi di variabile) necessari per visualizzare i dati in formato carattere.|  
|SQL_DECIMAL SQL_NUMERIC|Precisione della colonna più 2 (un segno, cifre di *precisione* e un separatore decimale). Ad esempio, le dimensioni di visualizzazione di una colonna definita come NUMERIca (10, 3) sono pari a 12.|  
|SQL_BIT|1 (1 cifra).|  
|SQL_TINYINT|4 se firmato (un segno e 3 cifre) o 3 se senza segno (3 cifre).|  
|SQL_SMALLINT|6 se firmato (un segno e 5 cifre) o 5 se non firmato (5 cifre).|  
|SQL_INTEGER|11 se firmato (un segno e 10 cifre) o 10 se non firmato (10 cifre).|  
|SQL_BIGINT|20 (un segno e 19 cifre se con segno o 20 cifre se non firmato).|  
|SQL_REAL|14 (un segno, 7 cifre, un separatore decimale, la lettera *e*, un segno e 2 cifre).|  
|SQL_FLOAT SQL_DOUBLE|24 (un segno, 15 cifre, un separatore decimale, la lettera *e*, un segno e 3 cifre).|  
|Tutti i tipi binari [a]|Lunghezza definita o massima (per i tipi di variabile) della colonna volte 2. (Ogni byte binario è rappresentato da un numero esadecimale a 2 cifre).|  
|SQL_TYPE_DATE|10 (una data nel formato *aaaa-mm-gg*).|  
|SQL_TYPE_TIME|8 (un'ora nel formato *hh: mm: SS*)<br /><br /> - oppure -<br /><br /> 9 + *s* (un'ora nel formato *hh: mm: SS*[. fff...], dove *s* è la precisione dei secondi frazionari).|  
|SQL_TYPE_TIMESTAMP|19 (per un timestamp nel formato *aaaa-mm-gg hh: mm: SS* )<br /><br /> - oppure -<br /><br /> 20 + *s* (per un timestamp nel formato *aaaa-mm-gg hh: mm: SS*[. fff...], dove *s* è la precisione dei secondi frazionari).|  
|Tutti i tipi di dati intervallo|Vedere [lunghezza del tipo di dati intervallo](../../../odbc/reference/appendixes/interval-data-type-length.md).|  
|SQL_GUID|36 (numero di caratteri nel formato *aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee*|  
  
 [a] se il driver non è in grado di determinare la lunghezza della colonna o del parametro dei tipi di variabile, restituisce SQL_NO_TOTAL.
