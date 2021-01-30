---
description: Funzioni di stringa
title: Funzioni stringa | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- functions [ODBC], string functions
- string functions [ODBC]
ms.assetid: 270f669e-8aab-4db0-95a4-f2b3c69538b3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fe6c50c7ad96f1025b27f60e2de74ba31f60698f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202522"
---
# <a name="string-functions"></a>Funzioni di stringa
Nella tabella seguente sono elencate le funzioni di modifica delle stringhe. Un'applicazione può determinare quali funzioni stringa sono supportate da un driver chiamando **SQLGetInfo** con un *tipo di informazioni* SQL_STRING_FUNCTIONS.  
  
## <a name="remarks"></a>Commenti  
 Gli argomenti identificati come *string_exp* possono essere il nome di una colonna, un *valore letterale stringa di caratteri* o il risultato di un'altra funzione scalare, in cui il tipo di dati sottostante può essere rappresentato come SQL_CHAR, SQL_VARCHAR o SQL_LONGVARCHAR.  
  
 Gli argomenti identificati come *character_exp* sono stringhe di caratteri a lunghezza variabile.  
  
 Gli argomenti denotati come *inizio*, *lunghezza* o *conteggio* possono essere *valori letterali numerici* o il risultato di un'altra funzione scalare, in cui il tipo di dati sottostante può essere rappresentato come SQL_TINYINT, SQL_SMALLINT o SQL_INTEGER.  
  
 Le funzioni di stringa elencate di seguito sono basate su 1; ovvero il primo carattere della stringa è il carattere 1.  
  
 Le funzioni scalari di stringa BIT_LENGTH, CHAR_LENGTH, CHARACTER_LENGTH, OCTET_LENGTH e POSITION sono state aggiunte in ODBC 3,0 per essere allineate con SQL-92.  
  
|Funzione|Descrizione|  
|--------------|-----------------|  
|**ASCII (** _string_exp_ **)**  (ODBC 1,0)|Restituisce il codice ASCII del carattere più a sinistra di *string_exp* come valore integer.|  
|**BIT_LENGTH (** _string_exp_ **)**  (ODBC 3,0)|Restituisce la lunghezza in bit dell'espressione stringa.<br /><br /> Non funziona solo per i tipi di dati stringa, pertanto non convertirà in modo implicito *string_exp* in stringa ma restituirà la dimensione (interna) del tipo di dati specificato.|  
|**Char (** _codice_ **)**  (ODBC 1,0)|Restituisce il carattere che ha il valore del codice ASCII specificato dal *codice*. Il valore del *codice* deve essere compreso tra 0 e 255. in caso contrario, il valore restituito sarà dipendente dall'origine dati.|  
|**CHAR_LENGTH (** _string_exp_ **)**  (ODBC 3,0)|Restituisce la lunghezza in caratteri dell'espressione stringa, se l'espressione stringa è di un tipo di dati character. in caso contrario, restituisce la lunghezza in byte dell'espressione stringa (il valore integer più piccolo non inferiore al numero di bit diviso per 8). Questa funzione corrisponde alla funzione CHARACTER_LENGTH.|  
|**CHARACTER_LENGTH (** _string_exp_ **)**  (ODBC 3,0)|Restituisce la lunghezza in caratteri dell'espressione stringa, se l'espressione stringa è di un tipo di dati character. in caso contrario, restituisce la lunghezza in byte dell'espressione stringa (il valore integer più piccolo non inferiore al numero di bit diviso per 8). Questa funzione corrisponde alla funzione CHAR_LENGTH.|  
|**Concat (** _string_exp1_,_string_exp2_**)**  (ODBC 1,0)|Restituisce una stringa di caratteri che è il risultato della concatenazione *string_exp2* *string_exp1*. La stringa risultante dipende da DBMS. Se, ad esempio, la colonna rappresentata da *string_exp1* conteneva un valore null, DB2 restituirebbe null, ma SQL server restituirebbe la stringa non null.|  
|**Differenza (** _string_exp1_,_string_exp2_**)**  (ODBC 2,0)|Restituisce un valore intero che indica la differenza tra i valori restituiti dalla funzione SOUNDEX per *string_exp1* e *string_exp2*.|  
|**Insert (** _string_exp1_, *Start*, *length*, _string_exp2_**)**  (ODBC 1,0)|Restituisce una stringa di caratteri in cui i caratteri di *lunghezza* sono stati eliminati da *string_exp1* *, a partire dall'inizio e* in cui è stato inserito *string_exp2* in *string_exp,* *a partire dall'inizio.*|  
|**LCase (** _string_exp_ **)** (ODBC 1,0)|Restituisce una stringa uguale a quella in *string_exp*, con tutti i caratteri maiuscoli convertiti in minuscolo.|  
|**Left (** _string_exp_, _conteggio_**)** (ODBC 1,0)|Restituisce i caratteri di *conteggio* più a sinistra del *string_exp*.|  
|**Lunghezza (** _string_exp_ **)** (ODBC 1,0)|Restituisce il numero di caratteri in *string_exp,* esclusi gli spazi vuoti finali.<br /><br /> La **lunghezza** accetta solo stringhe. Quindi convertirà in modo implicito *string_exp* in una stringa e restituirà la lunghezza di questa stringa (non le dimensioni interne del tipo di dati).|  
|**Individuare (** _string_exp1_, *string_exp2*[, *Start*]**)** (ODBC 1,0)|Restituisce la posizione iniziale della prima occorrenza di *string_exp1* all'interno *string_exp2*. La ricerca della prima occorrenza di *string_exp1* inizia con la posizione del primo carattere in *string_exp2* , a meno che non sia specificato l'argomento facoltativo, *Start*. Se *Start* è specificato, la ricerca inizia con la posizione del carattere indicata dal valore di *Start*. La posizione del primo carattere in *string_exp2* è indicata dal valore 1. Se *string_exp1* non viene trovato all'interno di *string_exp2*, viene restituito il valore 0.<br /><br /> Se un'applicazione può chiamare la funzione scalare LOCAte con gli argomenti *string_exp1*, *string_exp2* e *start* , il driver restituisce SQL_FN_STR_LOCATE quando **SQLGetInfo** viene chiamato con un' *opzione* di SQL_STRING_FUNCTIONS. Se l'applicazione può chiamare la funzione scalare LOCAte solo con gli argomenti *string_exp1* e *string_exp2* , il driver restituisce SQL_FN_STR_LOCATE_2 quando **SQLGetInfo** viene chiamato con un' *opzione* di SQL_STRING_FUNCTIONS. I driver che supportano la chiamata della funzione loca con due o tre argomenti restituiscono sia SQL_FN_STR_LOCATE sia SQL_FN_STR_LOCATE_2.|  
|**LTRIM (** _string_exp_ **)** (ODBC 1,0)|Restituisce i caratteri di *string_exp* con gli spazi vuoti iniziali rimossi.|  
|**OCTET_LENGTH (** _string_exp_ **)** (ODBC 3,0)|Restituisce la lunghezza in byte dell'espressione stringa. Il risultato è il più piccolo numero integer non inferiore al numero di bit diviso per 8.<br /><br /> Non funziona solo per i tipi di dati stringa, pertanto non convertirà in modo implicito *string_exp* in stringa ma restituirà la dimensione (interna) del tipo di dati specificato.|  
|**Position (** _character_exp_ **in** _character_exp_**)** (ODBC 3,0)|Restituisce la posizione della prima espressione di caratteri nella seconda espressione di caratteri. Il risultato è un valore numerico esatto con una precisione definita dall'implementazione e una scala pari a 0.|  
|**Ripetizione (** _string_exp,_ _conteggio_**)** (ODBC 1,0)|Restituisce una stringa di caratteri composta da *string_exp* tempi di *conteggio* ripetuti.|  
|**Replace (** _string_exp1_, *string_exp2*, _string_exp3_**)** (ODBC 1,0)|Eseguire la ricerca *string_exp1* foroccurrences di *string_exp2* e sostituire con *string_exp3*.|  
|**Right (** _string_exp_, _conteggio_**)** (ODBC 1,0)|Restituisce i caratteri di *conteggio* più a destra del *string_exp*.|  
|**RTRIM (** _string_exp_ **)** (ODBC 1,0)|Restituisce i caratteri di *string_exp* con gli spazi vuoti finali rimossi.|  
|**SOUNDEX (** _string_exp_ **)** (ODBC 2,0)|Restituisce una stringa di caratteri dipendente dall'origine dati che rappresenta il suono delle parole in *string_exp*. Ad esempio, SQL Server restituisce un codice SOUNDEX a 4 cifre; Oracle restituisce una rappresentazione fonetica di ogni parola.|  
|**Spazio (** _conteggio_ **)** (ODBC 2,0)|Restituisce una stringa di caratteri costituita da spazi di *conteggio* .|  
|**Substring (** _string_exp_, *inizio*, lunghezza **)** (ODBC 1,0)|Restituisce una stringa di caratteri derivata da *string_exp*, a partire dalla posizione del carattere specificata da *Start* per i caratteri di *lunghezza* .|  
|**UCase (** _string_exp_ **)** (ODBC 1,0)|Restituisce una stringa uguale a quella in *string_exp*, con tutti i caratteri minuscoli convertiti in caratteri maiuscoli.|
