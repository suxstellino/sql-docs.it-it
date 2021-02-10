---
description: Esempio di data shaping
title: Esempio di data shaping | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data shaping [ADO], about data shaping
ms.assetid: 1bfdcad4-52e1-45bc-ad21-783657ef0a44
author: rothja
ms.author: jroth
ms.openlocfilehash: 0199dfc2165d5ae54b4b59a45ec740a9c399f114
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033281"
---
# <a name="data-shaping-example"></a>Esempio di data shaping
Il seguente comando data shaping Mostra come compilare un **Recordset** gerarchico dalle tabelle **Customers** e **Orders** del database Northwind.  
  
```  
SHAPE {SELECT CustomerID, ContactName FROM Customers}   
APPEND ({SELECT OrderID, OrderDate, CustomerID FROM Orders} AS chapOrders   
RELATE customerID TO customerID)   
```  
  
 Quando si utilizza questo comando per aprire un oggetto **Recordset** (come illustrato in [Visual Basic esempio di data shaping](./visual-basic-example-of-data-shaping.md)), viene creato un capitolo (**chapOrders**) per ogni record restituito dalla tabella **Customers** . Questo capitolo è costituito da un subset del **Recordset** restituito dalla tabella **Orders** . Il capitolo **chapOrders** contiene tutte le informazioni richieste sugli ordini effettuati dal cliente specificato. In questo esempio il capitolo è costituito da tre colonne: **OrderID**, **OrderDate** e **CustomerID**.  
  
 Le prime due voci del **Recordset** con forma risultante sono le seguenti:  
  
|CustomerID|ContactName|OrderID|OrderDate|CustomerID|  
|----------------|-----------------|-------------|---------------|----------------|  
|ALFKI|Maria Ander|10643<br /><br /> 10692<br /><br /> 10702<br /><br /> 10835<br /><br /> 10952<br /><br /> 11011|1997-08-25<br /><br /> 1997-10-03<br /><br /> 1997-10-13<br /><br /> 1998-01-15<br /><br /> 1998-03-16<br /><br /> 1998-04-09|ALFKI<br /><br /> ALFKI<br /><br /> ALFKI<br /><br /> ALFKI<br /><br /> ALFKI<br /><br /> ALFKI|  
|ANATR|Ana Trujillo|10308<br /><br /> 10625<br /><br /> 10759<br /><br /> 10926|1996-09-18<br /><br /> 1997-08-08<br /><br /> 1997-11-28<br /><br /> 1998-03-04|ANATR<br /><br /> ANATR<br /><br /> ANATR<br /><br /> ANATR|  
  
 In un comando SHAPE, APPEND viene utilizzato per creare un **Recordset** figlio correlato al **Recordset** padre (come restituito dal comando specifico del provider immediatamente dopo la parola chiave Shape descritta in precedenza) dalla clausola RELATE. L'elemento padre e l'elemento figlio hanno in genere almeno una colonna in comune: il valore della colonna in una riga dell'elemento padre corrisponde al valore della colonna in tutte le righe dell'elemento figlio.  
  
 Esiste un secondo modo per usare i comandi di forma, ovvero, per generare un **Recordset** padre da un **Recordset** figlio. I record del **Recordset** figlio sono raggruppati, in genere tramite la clausola by e una riga viene aggiunta al **Recordset** padre per ogni gruppo risultante nell'elemento figlio. Se la clausola BY viene omessa, il **Recordset** figlio formerà un singolo gruppo e il **Recordset** padre conterrà esattamente una riga. Questa operazione è utile per il calcolo delle aggregazioni "totale complessivo" sull'intero **Recordset** figlio.  
  
 Il costrutto del comando SHAPE consente inoltre di creare a livello di codice un **Recordset** a forma di. È quindi possibile accedere ai componenti del **Recordset** a livello di codice o tramite un controllo visivo appropriato. Un comando Shape viene emesso in modo analogo a qualsiasi altro testo del comando ADO. Per ulteriori informazioni, vedere [comandi Shape in generale](./shape-commands-in-general.md).  
  
 Indipendentemente dal modo in cui viene formato il **Recordset** padre, conterrà una colonna del capitolo utilizzata per correlarla a un **Recordset** figlio. Se lo si desidera, il **Recordset** padre può inoltre includere colonne che contengono aggregazioni (Sum, min, Max e così via) sulle righe figlio. Sia l'elemento padre che il **Recordset** figlio possono includere colonne contenenti un'espressione nella riga del **Recordset**, oltre a colonne nuove e inizialmente vuote.  
  
 Questa sezione continua con l'argomento seguente.  
  
-   [Esempio di data shaping in Visual Basic](./visual-basic-example-of-data-shaping.md)