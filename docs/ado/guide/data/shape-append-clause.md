---
description: Clausola APPEND di Shape
title: Clausola APPEND di forma | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- shape commands [ADO]
- data shaping [ADO], APPEND clause
- append clause [ADO]
ms.assetid: f90fcf55-6b24-401d-94e1-d65bd24bd342
author: rothja
ms.author: jroth
ms.openlocfilehash: 7c6d62d410e6709c93ad6e3d04db627f14f091a1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036991"
---
# <a name="shape-append-clause"></a>Clausola APPEND di Shape
La clausola SHAPE Command APPEND Accoda una colonna o colonne a un **Recordset**. Spesso queste colonne sono colonne del capitolo, che fanno riferimento a un **Recordset** figlio.  
  
## <a name="syntax"></a>Sintassi  
  
```  
SHAPE [parent-command [[AS] parent-alias]] APPEND column-list  
```  
  
## <a name="description"></a>Descrizione  
 Le parti di questa clausola sono le seguenti:  
  
 *comando padre*  
 Zero o uno dei seguenti elementi (è possibile omettere completamente il *comando padre* ):  
  
-   Comando del provider racchiuso tra parentesi graffe (" {} ") che restituisce un oggetto **Recordset** . Il comando viene emesso al provider di dati sottostante e la relativa sintassi dipende dai requisiti del provider. Si tratta in genere del linguaggio SQL, sebbene ADO non richieda un linguaggio di query specifico.  
  
-   Un altro comando Shape incorporato tra parentesi.  
  
-   Parola chiave TABLE, seguita dal nome di una tabella nel provider di dati.  
  
 *parent-alias*  
 Alias facoltativo che fa riferimento al **Recordset** padre.  
  
 *Elenco colonne*  
 Uno o più degli elementi seguenti:  
  
-   Una colonna aggregata.  
  
-   Una colonna calcolata.  
  
-   Nuova colonna creata utilizzando la nuova clausola.  
  
-   Una colonna del capitolo. Una definizione di colonna del capitolo è racchiusa tra parentesi ("()"). Vedere la sintassi seguente.  
  
```  
SHAPE [parent-command [[AS] parent-alias]]  
   APPEND (child-recordset [ [[AS] child-alias]   
      RELATE parent-column TO child-column | PARAMETER param-number, ... ])  
   [[AS] chapter-alias]   
   [, ... ]  
```  
  
## <a name="remarks"></a>Commenti  
 *recordset figlio*  
 -   Comando del provider racchiuso tra parentesi graffe (" {} ") che restituisce un oggetto **Recordset** . Il comando viene emesso al provider di dati sottostante e la relativa sintassi dipende dai requisiti del provider. Si tratta in genere del linguaggio SQL, sebbene ADO non richieda un linguaggio di query specifico.  
  
-   Un altro comando Shape incorporato tra parentesi.  
  
-   Nome di un **Recordset** con forma esistente.  
  
-   Parola chiave TABLE, seguita dal nome di una tabella nel provider di dati.  
  
 *alias figlio*  
 Alias che fa riferimento al **Recordset** figlio.  
  
 *colonna padre*  
 Colonna nel **Recordset** restituita dal *comando padre.*  
  
 *colonna figlio*  
 Colonna nel **Recordset** restituita dal *comando figlio*.  
  
 *param-Number*  
 Vedere [operazione di comandi con parametri](../../../ado/guide/data/operation-of-parameterized-commands.md).  
  
 *capitolo-alias*  
 Alias che fa riferimento alla colonna del capitolo aggiunta all'elemento padre.  
  
> [!NOTE]
>  La clausola *"padre-colonna* in *colonna figlio"* è in realtà un elenco, dove ogni relazione definita è separata da una virgola  
  
> [!NOTE]
>  La clausola dopo la parola chiave APPEND è effettivamente un elenco, in cui ogni clausola è separata da una virgola e definisce un'altra colonna da accodare all'elemento padre.  
  
Quando si creano comandi del provider dall'input dell'utente come parte di un comando SHAPE, SHAPE considererà il comando del provider fornito dall'utente come stringa opaca e lo passerà fedelmente al provider. Ad esempio, nel comando SHAPE seguente,  
  
```  
SHAPE {select * from t1} APPEND ({select * from t2} RELATE k1 TO k2)  
```  
  
 SHAPE eseguirà due comandi: `select * from t1` e ( `select * from t2 RELATE k1 TO k2)` . Se l'utente fornisce un comando composto costituito da più comandi del provider separati da punti e virgola, la forma non è in grado di distinguere la differenza. Quindi, nel comando SHAPE seguente,  
  
```  
SHAPE {select * from t1; drop table t1} APPEND ({select * from t2} RELATE k1 TO k2)  
```  
  
 La forma viene eseguita `select * from t1; drop table t1` e ( `select * from t2 RELATE k1 TO k2),` senza rendersene conto che `drop table t1` è un comando del provider separato e in questo caso pericoloso. Le applicazioni devono sempre convalidare l'input dell'utente per impedire che si verifichino potenziali attacchi pirati informatici.  
  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Funzionamento dei comandi senza parametri](../../../ado/guide/data/operation-of-non-parameterized-commands.md)  
  
-   [Funzionamento dei comandi con parametri](../../../ado/guide/data/operation-of-parameterized-commands.md)  
  
-   [Comandi ibridi](../../../ado/guide/data/hybrid-commands.md)  
  
-   [Clausole COMPUTE intermedie di Shape](../../../ado/guide/data/intervening-shape-compute-clauses.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Esempio di data shaping](../../../ado/guide/data/data-shaping-example.md)   
 [Grammatica forma formale](../../../ado/guide/data/formal-shape-grammar.md)   
 [Comandi Shape in generale](../../../ado/guide/data/shape-commands-in-general.md)
