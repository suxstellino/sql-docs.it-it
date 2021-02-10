---
description: Grammatica formale per Shape
title: Grammatica forma formale | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- shape commands [ADO], shape grammar
- data shaping [ADO], shape grammar
ms.assetid: ea691475-0f03-4abe-a785-b77e77712d1d
author: rothja
ms.author: jroth
ms.openlocfilehash: cd626fe6887f784ec62e22ccc276e64feb4a9b14
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037461"
---
# <a name="formal-shape-grammar"></a>Grammatica formale per Shape
Si tratta della grammatica formale per la creazione di qualsiasi comando Shape:  
  
-   I termini grammaticali richiesti sono stringhe di testo delimitate da parentesi angolari ("<>").  
  
-   I termini facoltativi sono delimitati da parentesi quadre ("[]").  
  
-   Le alternative sono indicate da un rovesciata ("&#124;").  
  
-   Le alternative ripetute sono indicate da puntini di sospensione ("...").  
  
-   *Alfa* indica una stringa di lettere alfabetiche.  
  
-   *Digit* indica una stringa di numeri.  
  
-   *Unicode-digit* indica una stringa di cifre Unicode.  
  
 Tutti gli altri termini sono valori letterali.  
  
|Termine|Definizione|  
|----------|----------------|  
|\<shape-command>|SHAPE [ \<table-exp> [[As]] \<alias> ] [ \<shape-action> ]|  
|\<table-exp>|{ \<provider-command-text> } &#124;<br /><br /> ( \<shape-command> ) &#124;<br /><br /> \<quoted-name>&#124; tabella<br /><br /> \<quoted-name>|  
|\<shape-action>|ACCODA \<aliased-field-list> &#124;<br /><br /> Compute \<aliased-field-list> [by \<field-list> ]|  
|\<aliased-field-list>|\<aliased-field> [, \<aliased-field...>]|  
|\<aliased-field>|\<field-exp> [[AS] \<alias> ]|  
|\<field-exp>|( \<relation-exp> ) &#124;<br /><br /> \<calculated-exp> &#124;<br /><br /> \<aggregate-exp> &#124;<br /><br /> \<new-exp>|  
|<relation_exp>|\<table-exp> [[AS] \<alias> ]<br /><br /> CORRELATI \<relation-cond-list>|  
|\<relation-cond-list>|\<relation-cond> [, \<relation-cond>...]|  
|\<relation-cond>|\<field-name> A \<child-ref>|  
|\<child-ref>|\<field-name> &#124;<br /><br /> PARAMETRO \<param-ref>|  
|\<param-ref>|\<number>|  
|\<field-list>|\<field-name> [, \<field-name>]|  
|\<aggregate-exp>|&#124; SUM ( \<qualified-field-name> )<br /><br /> &#124; AVG ( \<qualified-field-name> )<br /><br /> &#124; MIN ( \<qualified-field-name> )<br /><br /> &#124; MAX ( \<qualified-field-name> )<br /><br /> CONTEGGIO ( \<qualified-alias> &#124; \<qualified-name> ) &#124;<br /><br /> STDEV ( \<qualified-field-name> ) &#124;<br /><br /> ANY ( \<qualified-field-name> )|  
|\<calculated-exp>|CALCOLO ( \<expression> )|  
|\<qualified-field-name>|\<alias>.[\<alias>...]\<field-name>|  
|\<alias>|\<quoted-name>|  
|\<field-name>|\<quoted-name> [[AS] \<alias> ]|  
|\<quoted-name>|" \<string> " &#124;<br /><br /> ' \<string> ' &#124;<br /><br /> [ \<string> ] &#124;<br /><br /> \<name>|  
|\<qualified-name>|alias [. alias...]|  
|\<name>|alfa [alpha &#124; digit &#124; _ &#124; # &#124;: &#124;...]|  
|\<number>|digit [digit...]|  
|\<new-exp>|NUOVO \<field-type> [( \<number> [, \<number> ])]|  
|\<field-type>|Tipo di dati OLE DB o ADO.|  
|\<string>|Unicode-char [Unicode-char...]|  
|\<expression>|Espressione Visual Basic, Applications Edition i cui operandi sono altre colonne non di calcolo nella stessa riga.|  
  
## <a name="see-also"></a>Vedere anche  
 [Accesso alle righe in un recordset gerarchico](./accessing-rows-in-a-hierarchical-recordset.md)   
 [Cenni preliminari sulla data shaping](./data-shaping-overview.md)   
 [Provider richiesti per la definizione dei dati](./required-providers-for-data-shaping.md)   
 [Clausola APPEND della forma](./shape-append-clause.md)   
 [Comandi per la forma in generale](./shape-commands-in-general.md)   
 [SHAPE COMPUTE-clausola](./shape-compute-clause.md)   
 [Funzioni di Visual Basic, Applications Edition](./visual-basic-for-applications-functions.md)