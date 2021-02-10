---
description: Preparazione ed esecuzione di comandi
title: Preparazione ed esecuzione di comandi | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Command object [ADO], preparing and executing commands
ms.assetid: 7448d9ee-7f4b-47e3-be54-2df8c9bbac32
author: rothja
ms.author: jroth
ms.openlocfilehash: fd88799bce8f3b8a0842c9f2d2752eac062dc4b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037181"
---
# <a name="preparing-and-executing-commands"></a>Preparazione ed esecuzione di comandi
I comandi sono istruzioni rilasciate a un provider per eseguire alcune operazioni sull'origine dati sottostante. Un'istruzione SQL, ad esempio, è un comando per Microsoft SQL provider di dati. In ADO, i comandi vengono in genere rappresentati dagli oggetti **Command** , sebbene sia possibile emettere semplici comandi tramite oggetti **connessione** o **Recordset** .  
  
 È possibile utilizzare l'oggetto **Command** per richiedere qualsiasi tipo di operazione supportato dal provider, supponendo che il provider sia in grado di interpretare correttamente la stringa di comando. Un'operazione comune per i provider di dati consiste nell'eseguire una query su un database e restituire i record in un oggetto **Recordset** , che può essere considerato come un contenitore per contenere il risultato e uno strumento per visualizzare il risultato. Come per molti oggetti ADO, alcune proprietà, metodi o raccolte di oggetti **Command** possono generare errori quando vi viene fatto riferimento, a seconda della funzionalità del provider.  
  
 Oltre a usare gli oggetti **Command** , è possibile usare il metodo **Execute** sull'oggetto **Connection** o il metodo **Open** nell'oggetto **Recordset** per emettere un comando ed eseguirlo. Tuttavia, è consigliabile usare un oggetto **Command** se è necessario riutilizzare un comando nel codice o se è necessario passare informazioni dettagliate sui parametri con il comando. Questi scenari sono trattati in modo più dettagliato più avanti in questa sezione.  
  
> [!NOTE]
>  Un determinato **comando** può restituire un set di risultati come flusso binario o come singolo **record** anziché come **Recordset**, se è supportato dal provider. Inoltre, alcuni **comandi** non hanno lo scopo di restituire alcun set di risultati, ad esempio una query SQL Update. In questa sezione verrà descritto lo scenario più comune, tuttavia, ovvero l'esecuzione di **comandi** che restituiscono risultati come oggetto **Recordset** . Per ulteriori informazioni sulla restituzione dei risultati in **record** s o **Stream** s, vedere [record e flussi](../../../ado/guide/data/records-and-streams.md).  
  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Panoramica dell'oggetto Command](../../../ado/guide/data/command-object-overview.md)  
  
-   [Creazione ed esecuzione di un comando semplice](../../../ado/guide/data/creating-and-executing-a-simple-command.md)  
  
-   [Parametri dell'oggetto Command](../../../ado/guide/data/command-object-parameters.md)  
  
-   [Chiamata di una stored procedure con Command](../../../ado/guide/data/calling-a-stored-procedure-with-a-command.md)  
  
-   [Chiamata di una stored procedure come metodo in un oggetto Connection](../../../ado/guide/data/calling-a-stored-procedure-as-a-method-on-a-connection-object.md)  
  
-   [Comandi con nome](../../../ado/guide/data/named-commands.md)  
  
-   [Passaggio di parametri a un comando con nome](../../../ado/guide/data/passing-parameters-to-a-named-command.md)
