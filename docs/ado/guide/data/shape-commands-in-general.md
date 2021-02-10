---
description: Comandi Shape in generale
title: Comandi per la forma in generale | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- shape commands [ADO]
- data shaping [ADO], shape commands
ms.assetid: 1fac7831-a187-4b15-9b43-aad380c5556c
author: rothja
ms.author: jroth
ms.openlocfilehash: 021d47eb011a4c0628b6f356772e82c591747103
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032463"
---
# <a name="shape-commands-in-general"></a>Comandi Shape in generale
Data Shaping definisce le colonne di un **Recordset** con forma, le relazioni tra le entità rappresentate dalle colonne e il modo in cui il **Recordset** viene popolato con i dati.  
  
 Un **Recordset** con forma può essere costituito dai tipi di colonne seguenti.  
  
|Tipo di colonna|Descrizione|  
|-----------------|-----------------|  
|data|Campi da un **Recordset** restituito da un comando di query a un provider di dati, a una tabella o a un **Recordset** precedentemente definito.|  
|capitolo|Un riferimento a un altro **Recordset**, denominato *capitolo*. Le colonne del capitolo consentono di definire una relazione *padre-figlio* in cui *l'elemento padre* è il **Recordset** che contiene la colonna del capitolo e l' *elemento figlio* è il **Recordset** rappresentato dal capitolo.|  
|aggregate|Il valore della colonna deriva dall'esecuzione di una funzione di *aggregazione* su tutte le righe o una colonna di tutte le righe di un **Recordset** figlio. (Vedere funzioni di aggregazione nell'argomento seguente, [funzioni di aggregazione, funzione Calc e parola chiave New](../../../ado/guide/data/aggregate-functions-the-calc-function-and-the-new-keyword.md)).|  
|espressione calcolata|Il valore della colonna viene derivato calcolando un'espressione Visual Basic, Applications Edition sulle colonne nella stessa riga del **Recordset**. L'espressione è l'argomento della funzione CALC. (Vedere espressione calcolata nell'argomento seguente, [funzioni di aggregazione, funzione Calc e la parola chiave New](../../../ado/guide/data/aggregate-functions-the-calc-function-and-the-new-keyword.md) e in [Visual Basic, Applications Edition Functions](../../../ado/guide/data/visual-basic-for-applications-functions.md)).|  
|Nuovo|Campi vuoti e fabbricati, che possono essere popolati con i dati in un secondo momento. La colonna viene definita con la parola chiave NEW. (Vedere la parola chiave NEW nell'argomento seguente, [funzioni di aggregazione, funzione Calc e nuova parola chiave](../../../ado/guide/data/aggregate-functions-the-calc-function-and-the-new-keyword.md)).|  
  
 Un comando Shape può contenere una clausola che specifica un comando di query a un provider di dati sottostante che restituirà un oggetto **Recordset** . La sintassi della query dipende dai requisiti del provider di dati sottostante. Si tratta in genere di SQL, sebbene ADO non richieda l'utilizzo di un linguaggio di query specifico.  
  
 I comandi Shape possono essere emessi da oggetti **Recordset** o impostando la proprietà [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) dell'oggetto [Command](../../../ado/reference/ado-api/command-object-ado.md) e quindi chiamando il metodo [Execute](../../../ado/reference/ado-api/execute-method-ado-command.md) .  
  
 È possibile utilizzare una clausola SQL JOIN per correlare due tabelle. un **Recordset** gerarchico, tuttavia, può rappresentare le informazioni in modo più efficiente. Ogni riga di un **Recordset** creato da un join ripete le informazioni in maniera ridondante da una delle tabelle. Un **Recordset** gerarchico ha un solo **Recordset** padre per ognuno dei più oggetti **Recordset** figlio.  
  
 I comandi Shape possono essere annidati. Ovvero il comando *padre* o il *comando figlio* può essere un altro comando Shape.  
  
 Il provider di forme restituisce sempre un cursore client, anche quando l'utente specifica una posizione del cursore di **adUseServer come**.  
  
 È possibile accedere ai componenti **Recordset** del **Recordset** di base a livello di codice o tramite un controllo visivo appropriato.  
  
 Microsoft fornisce uno strumento visivo che genera comandi di forma (vedere la [finestra di progettazione dell'ambiente dati](/previous-versions/visualstudio/aa445793(v=vs.60)) nella documentazione di Visual Basic 6) e un altro che Visualizza i cursori gerarchici (vedere "utilizzo del controllo FlexGrid gerarchico Microsoft" nella documentazione di Visual Basic 6).  
  
 Per informazioni sull'esplorazione di un **Recordset** gerarchico, vedere [accesso alle righe in un recordset gerarchico](../../../ado/guide/data/accessing-rows-in-a-hierarchical-recordset.md).  
  
 Per informazioni precise sui comandi di forma sintatticamente corretti, vedere [grammatica formale di forme](../../../ado/guide/data/formal-shape-grammar.md).  
  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Funzioni di aggregazione, funzione CALC e parola chiave NEW](../../../ado/guide/data/aggregate-functions-the-calc-function-and-the-new-keyword.md)  
  
-   [Invio di comandi al provider di dati sottostante](../../../ado/guide/data/issuing-commands-to-the-underlying-data-provider.md)