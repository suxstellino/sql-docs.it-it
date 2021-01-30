---
description: Elementi usati nelle istruzioni SQL
title: Elementi utilizzati nelle istruzioni SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQL statements [ODBC], elements supported
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
ms.assetid: 85777525-1555-4731-8309-63a464c6b43a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5911de3ce17fc0557b9fad885fa7cdc6ce5024ac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194869"
---
# <a name="elements-used-in-sql-statements"></a>Elementi usati nelle istruzioni SQL
Gli elementi seguenti vengono usati nelle istruzioni SQL elencate in precedenza.  
  
## <a name="element"></a>Elemento  
 *base-table-identifier* :: = *nome-definito dall'utente*  
  
 *base-table-name* :: = *base-table-identifier*  
  
 *booleano-Factor* :: = [not] *booleano-primario*  
  
 *booleano-Primary* :: =*predicato* di confronto &#124; ( *condizione di ricerca* )  
  
 *booleano-term* :: = *Boolean-Factor* [and *Boolean-term*]  
  
 *carattere-stringa-valore letterale* :: ='' {*character*}.. .'' (*character* è qualsiasi carattere nel set di caratteri dell'origine dati/driver. Per includere un carattere virgoletta singola ('') in un valore letterale stringa di caratteri, usare due virgolette letterali [''''].  
  
 *column-identifier* :: = *nome-definito dall'utente*  
  
 *Column-Name* :: = [*nome-tabella*] *identificatore di colonna*  
  
 *operatore di confronto* :: = < &#124; > &#124; \<= &#124; > = &#124; = &#124; <>  
  
 *confronto-predicato* :: = *espressione* operatore-operatore  
  
 *Data-Type* :: = *character-string-type* (*character-string-type* è qualsiasi tipo di dati per cui la colonna "" data_type "" nel set di risultati restituito da SQLGetTypeInfo è SQL_CHAR o SQL_VARCHAR).  
  
 *digit* :: = 0 &#124; 1 &#124; 2 &#124; 3 &#124; 4 &#124; 5 &#124; 6 &#124; 7 &#124; 8 &#124; 9  
  
 *parametro dinamico* :: =?  
  
 *espressione* :: = termine &#124; espressione {+&#124;-}  
  
 *Factor* :: = [ *+*&#124;*-* ]*primario*  
  
 *Insert-value* :: =  
  
 *parametro dinamico*  
  
 *Valore letterale* &#124;  
  
 &#124; NULL  
  
 UTENTE &#124;  
  
 *Letter* :: = *lettere* minuscole &#124; lettere maiuscole  
  
 *valore letterale* :: = *carattere-stringa-valore letterale*  
  
 *lower-case-Letter* :: = a &#124; b &#124; c &#124; d &#124; e &#124; f &#124; g &#124; h &#124; i &#124; j &#124; k &#124; l &#124; m &#124; n &#124; o &#124; p &#124; q &#124; r &#124; s &#124; t &#124; u &#124; v &#124; w &#124; x &#124; y &#124; z  
  
 *ORDER-BY-clause* :: = order by *Sort-Specification* [, *Sort-Specification*]...  
  
 *Primary* :: = *Column-Name*  
  
 &#124; *parametro dinamico*  
  
 *Valore letterale* &#124;  
  
 &#124; ( *espressione* )  
  
 *Search-Condition* :: = *Boolean-term* [o *condizione di ricerca*]  
  
 *select-list* :: = \* &#124; *Select-sottolist* [, *Select-sublist*]...  *select-list* non può contenere parametri.  
  
 *Select-sublist* :: = *Expression*  
  
 *Sort-Specification* :: = {*unsigned-integer &#124; nome-colonna*} [*ASC &#124; desc*]  
  
 *table-identifier* :: = *nome-definito dall'utente*  
  
 *nome tabella* :: = *ID tabella*  
  
 *Table-Reference* :: = *nome-tabella*  
  
 *table-reference-list* :: = *Table-Reference* [,*Table-Reference*]...  
  
 fattore di *termine::* = *Factor* &#124; *termine* { \*&#124;*/* }   
  
 *unsigned-integer* :: = {*digit*}  
  
 *lettere* maiuscole:: = *A &#124; B &#124; C &#124; D &#124; E &#124; F &#124; G &#124; H &#124; I &#124; J &#124; K &#124; L &#124; M &#124; N &#124; O &#124; P &#124; Q &#124; R &#124; S &#124; T &#124; U &#124; V &#124; W &#124; X &#124; Y &#124; Z*  
  
 *nome-definito dall'utente* :: = *letter*[*digit* &#124; *Letter* &#124; *_*]...
