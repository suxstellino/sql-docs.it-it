---
description: Prevenzione degli errori
title: Errori previsti | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- anticipating errors [ADO]
- errors [ADO], preventing
- preventing errors [ADO]
ms.assetid: ea1d4a97-58c3-476b-a496-cc80db2a90d5
author: rothja
ms.author: jroth
ms.openlocfilehash: 4a8c7c3867d7e133428ff8e17cca8e54e5a54297
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037811"
---
# <a name="anticipating-errors"></a>Prevenzione degli errori
La prevenzione degli errori è importante almeno quanto la gestione degli errori. Questa sezione finale contiene un breve elenco di precauzioni che l'applicazione può eseguire per rendere meno probabile che si verifichino errori.  
  
 Verificare lo stato degli oggetti controllando il valore nella proprietà **state** prima di provare a eseguire un'operazione utilizzando tali oggetti. Se, ad esempio, l'applicazione usa una **connessione** globale, controllare la proprietà **state** per verificare se è già aperta prima di chiamare il metodo **Open** .  
  
-   Tutti i programmi che accettano dati da un utente devono includere il codice per convalidare i dati prima di inviarli all'archivio dati. Non è possibile utilizzare l'archivio dati, il provider, ADO o anche il linguaggio di programmazione per notificare eventuali problemi. È necessario controllare ogni byte immesso dagli utenti, assicurandosi che i dati siano del tipo corretto per il campo e che i campi obbligatori non siano vuoti.  
  
 Controllare i dati prima di provare a scrivere i dati nell'archivio dati. Il modo più semplice per eseguire questa operazione consiste nel gestire l'evento **WillMove** o l'evento **WillUpdateRecordset** . Per informazioni più complete sulla gestione degli eventi ADO, vedere [gestione degli eventi ADO](./handling-ado-events.md).  
  
 Prima di tentare di spostare il puntatore del record, verificare che gli oggetti **Recordset** non superino i limiti del **Recordset** . Se si tenta di eseguire **MoveNext** quando **EOF** è true o **MovePrev** quando **BOF** è true, si verificherà un errore. Se si esegue uno dei metodi **Move** quando sia **EOF** che **BOF** sono true, verrà generato un errore.  
  
 Si verificheranno errori anche se si tenta di eseguire operazioni quali **Seek** e **Find** in un **Recordset** vuoto.