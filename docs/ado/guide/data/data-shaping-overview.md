---
description: Panoramica del data shaping
title: Cenni preliminari sulla data shaping | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data shaping [ADO], overview
ms.assetid: 4cb5fd29-4e56-46ac-ae48-a6771c321c0c
author: rothja
ms.author: jroth
ms.openlocfilehash: dbe025e38960b458c2ad5a224bad44c37868d51f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037651"
---
# <a name="data-shaping-overview"></a>Panoramica del data shaping
Per *data shaping* si intende la creazione di relazioni gerarchiche tra due o più entità logiche in una query. La gerarchia può essere visualizzata nelle relazioni padre-figlio tra un record di un [Recordset](../../reference/ado-api/recordset-object-ado.md)e uno o più record (noto anche come capitolo) di un altro **Recordset**. In una relazione padre-figlio, il **Recordset** padre contiene il **Recordset** figlio. Un esempio di tale relazione gerarchica è Customers e Orders. Per ogni cliente in un database, possono essere presenti zero o più ordini. La relazione gerarchica può essere ricorsiva, vale a dire che i record nipoti possono essere annidati in un record figlio. In linea di principio, un record gerarchico può essere annidato in qualsiasi profondità. In pratica, ADO limita la ricorsione a un massimo di 512 **Recordset**.  
  
 In generale, le colonne di un **Recordset** con forma possono contenere dati di un provider di dati, ad esempio Microsoft® SQL Server, riferimenti a un altro **Recordset**, valori derivati da un calcolo su una singola riga di un **Recordset** o valori derivati da un'operazione su una colonna di un intero **Recordset**. Una colonna può anche essere appena fabbricata e vuota.  
  
 Quando si recupera il valore di una colonna che contiene un riferimento a un altro **Recordset**, ADO restituisce automaticamente il **Recordset** effettivo rappresentato dal riferimento. Il riferimento a un **Recordset** è in realtà un riferimento a un subset del figlio, denominato *capitolo*. Un singolo elemento padre può fare riferimento A più di un **Recordset** figlio.  
  
 Il supporto ADO per la definizione dei dati consente di eseguire una query su un'origine dati e restituire un **Recordset** in cui un record (padre) rappresenta un **Recordset**(figlio). Nello scenario relativo all'ordine dei clienti, è possibile usare la data shaping per recuperare le informazioni dei clienti, nonché gli ordini effettuati da ogni cliente in una singola query. Il **Recordset** risultante è noto anche come **Recordset** con formato.  
  
 Inoltre, la definizione dei dati in ADO consente di creare nuovi oggetti **Recordset** senza un'origine dati sottostante usando la parola chiave **New** per descrivere i campi dei **Recordset** padre e figlio. Il nuovo oggetto **Recordset** può quindi essere popolato con i dati e archiviati in modo permanente. Gli sviluppatori possono anche eseguire diversi calcoli o aggregazioni, ad esempio **Sum**, **AVG** e **Max**, nei campi figlio. Data Shaping consente inoltre di creare un **Recordset** padre da un **Recordset** figlio raggruppando i record nell'elemento figlio e inserendo una riga nell'elemento padre per ogni gruppo nell'elemento figlio.  
  
 Il normale SQL consente di recuperare i dati usando la sintassi di **join** , ma ciò può risultare inefficiente e difficoltoso perché i dati padre ridondanti vengono ripetuti in ogni record restituito per una relazione padre-figlio specificata. Data Shaping può correlare un singolo record padre nel **Recordset** padre a più record figlio nel **Recordset** figlio, evitando la ridondanza di un **join**. La maggior parte degli utenti trova il modello di programmazione di più **Recordset** padre-figlio più naturale e più semplice da usare rispetto al modello di **join a recordset** singolo.