---
description: Funzioni numeriche
title: Funzioni numeriche | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- functions [ODBC], numeric functions
- numeric functions [ODBC]
ms.assetid: 4fa548dc-e8b0-4179-92ff-81d6a79d10c3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fb822d707b89dd0f7597ad4acf6783118ec92f54
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072575"
---
# <a name="numeric-functions"></a>Funzioni numeriche
Nella tabella seguente vengono descritte le funzioni numeriche incluse nel set di funzioni scalari ODBC. Chiamando **SQLGetInfo** con un *tipo di informazioni* SQL_NUMERIC_FUNCTIONS, un'applicazione può determinare quali funzioni numeriche sono supportate da un driver.  
  
 Tutte le funzioni numeriche restituiscono valori di tipo di dati SQL_FLOAT ad eccezione di ABS, ROUND, TRUNCATE, SIGN, FLOOR e CEILING, che restituiscono valori dello stesso tipo di dati dei parametri di input.  
  
 Gli argomenti identificati come *numeric_exp* possono essere il nome di una colonna, il risultato di un'altra funzione scalare o un valore *numerico-Litera* l, in cui il tipo di dati sottostante potrebbe essere rappresentato come SQL_NUMERIC, SQL_DECIMAL, SQL_TINYINT, SQL_SMALLINT, SQL_INTEGER, SQL_BIGINT, SQL_FLOAT, SQL_REAL o SQL_DOUBLE.  
  
 Gli argomenti identificati come *float_exp* possono essere il nome di una colonna, il risultato di un'altra funzione scalare o un *valore letterale numerico*, in cui il tipo di dati sottostante può essere rappresentato come SQL_FLOAT.  
  
 Gli argomenti identificati come *integer_exp* possono essere il nome di una colonna, il risultato di un'altra funzione scalare o un *valore letterale numerico*, in cui il tipo di dati sottostante può essere rappresentato come SQL_TINYINT, SQL_SMALLINT, SQL_INTEGER o SQL_BIGINT.  
  
 Le funzioni scalari CURRENT_DATE, CURRENT_TIME e CURRENT_TIMESTAMP sono state aggiunte in ODBC 3,0 per essere allineate con SQL-92.  
  
|Funzione|Descrizione|  
|--------------|-----------------|  
|**ABS (** _numeric_exp_ **)**  (ODBC 1,0)|Restituisce il valore assoluto di *numeric_exp*.|  
|**ARCCOS (** _float_exp_ **)**  (ODBC 1,0)|Restituisce l'arcoseno di *float_exp* come angolo, espresso in radianti.|  
|**Asin (** _float_exp_ **)**  (ODBC 1,0)|Restituisce il arcoseno di *float_exp* come angolo, espresso in radianti.|  
|**Atan (** _float_exp_ **)**  (ODBC 1,0)|Restituisce il arcotangente di *float_exp* come angolo, espresso in radianti.|  
|**Atan2 (** _float_exp1_, _float_exp2_**)**  (ODBC 2,0)|Restituisce l'arcotangente delle coordinate *x* e *y* , specificate da *float_exp1* e *float_exp2* rispettivamente come angolo, espresso in radianti.|  
|**Ceiling (** _numeric_exp_ **)**  (ODBC 1,0)|Restituisce l'intero più piccolo maggiore o uguale a *numeric_exp*. Il valore restituito è dello stesso tipo di dati del parametro di input.|  
|**Cos (** _float_exp_ **)**  (ODBC 1,0)|Restituisce il coseno di *float_exp*, dove *float_exp* è un angolo espresso in radianti.|  
|**Lettino (** _float_exp_ **)**  (ODBC 1,0)|Restituisce la cotangente di *float_exp*, dove *float_exp* è un angolo espresso in radianti.|  
|**Gradi (** _numeric_exp_ **)**  (ODBC 2,0)|Restituisce il numero di gradi convertiti da *numeric_exp* radianti.|  
|**Exp (** _float_exp_ **)**  (ODBC 1,0)|Restituisce il valore esponenziale del *float_exp*.|  
|**Floor (** _numeric_exp_ **)**  (ODBC 1,0)|Restituisce l'intero più grande minore o uguale a *numeric_exp*. Il valore restituito è dello stesso tipo di dati del parametro di input.|  
|**Log (** _float_exp_ **)**  (ODBC 1,0)|Restituisce il logaritmo naturale della *float_exp*.|  
|**Log10 (** _float_exp_ **)**  (ODBC 2,0)|Restituisce il logaritmo in base 10 della *float_exp*.|  
|**Mod (** _integer_exp1_, _integer_exp2_**)**  (ODBC 1,0)|Restituisce il resto (modulo) del *integer_exp1* diviso per *integer_exp2*.|  
|**Pi ()**  (ODBC 1,0)|Restituisce il valore costante di pi greco come valore a virgola mobile.|  
|**Power (** _numeric_exp_, _integer_exp_**)**  (ODBC 2,0)|Restituisce il valore di *numeric_exp* alla potenza di *integer_exp*.|  
|**Radianti (** _numeric_exp_ **)**  (ODBC 2,0)|Restituisce il numero di radianti convertiti da *numeric_exp* gradi.|  
|**Rand (**[*integer_exp*]**)**  (ODBC 1,0)|Restituisce un valore a virgola mobile casuale utilizzando *integer_exp* come valore di inizializzazione facoltativo.|  
|**Round (** _numeric_exp_, _integer_exp_**)**  (ODBC 2,0)|Restituisce *numeric_exp* arrotondato alla *integer_exp* posizionata a destra del separatore decimale. Se *integer_exp* è un valore negativo, *numeric_exp* viene arrotondato a &#124;*integer_exp*&#124; posizioni a sinistra del separatore decimale.|  
|**Sign (** _numeric_exp_ **)**  (ODBC 1,0)|Restituisce un indicatore del segno di *numeric_exp*. Se *numeric_exp* è minore di zero, viene restituito-1. Se *numeric_exp* è uguale a zero, viene restituito 0. Se *numeric_exp* è maggiore di zero, viene restituito 1.|  
|**Sin (** _float_exp_ **)**  (ODBC 1,0)|Restituisce il seno di *float_exp*, dove *float_exp* è un angolo espresso in radianti.|  
|**Sqrt (** _float_exp_ **)**  (ODBC 1,0)|Restituisce la radice quadrata della *float_exp*.|  
|**Tan (** _float_exp_ **)**  (ODBC 1,0)|Restituisce la tangente di *float_exp*, dove *float_exp* è un angolo espresso in radianti.|  
|**Truncate (** _numeric_exp_, _integer_exp_**)**  (ODBC 2,0)|Restituisce *numeric_exp* troncato in *integer_exp* posiziona il punto decimale. Se *integer_exp* è un valore negativo, *numeric_exp* viene troncato &#124;*integer_exp*&#124; posizioni a sinistra del separatore decimale.|
