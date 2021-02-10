---
description: Informazioni sui blocchi
title: Informazioni sui blocchi | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- cursors [ADO], locking
- locks [ADO], about locking
ms.assetid: f8989555-28c6-4c17-9bf8-7f44a8a5c407
author: rothja
ms.author: jroth
ms.openlocfilehash: 245a0cd91b9d6c7675df146d9d5808b42bb8c618
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036731"
---
# <a name="what-is-a-lock"></a>Informazioni sui blocchi
Il blocco è il processo mediante il quale un DBMS limita l'accesso a una riga in un ambiente multiutente. Quando una riga o una colonna è bloccata in modo esclusivo, gli altri utenti non possono accedere ai dati bloccati fino a quando il blocco non viene rilasciato. Ciò garantisce che due utenti non possano aggiornare contemporaneamente la stessa colonna in una riga.  
  
 I blocchi possono essere molto dispendiosi dal punto di vista delle risorse e devono essere usati solo quando necessario per mantenere l'integrità dei dati. In un database in cui centinaia o migliaia di utenti potrebbero provare ad accedere a un record ogni secondo, ad esempio un database connesso a Internet, il blocco non necessario può causare un rallentamento delle prestazioni dell'applicazione.  
  
 È possibile controllare il modo in cui l'origine dati e la libreria di cursori ADO gestiscono la concorrenza scegliendo l'opzione di blocco appropriata.  
  
 Impostare la proprietà **LockType** prima di aprire un **Recordset** per specificare il tipo di blocco che il provider deve utilizzare per l'apertura. Leggere la proprietà per restituire il tipo di blocco in uso in un oggetto **Recordset** aperto.  
  
 I provider potrebbero non supportare tutti i tipi di blocco. Se un provider non è in grado di supportare l'impostazione **LockType** richiesta, verrà sostituito un altro tipo di blocco. Per determinare l'effettiva funzionalità di blocco disponibile in un oggetto **Recordset** , utilizzare il metodo [Supports](../../../ado/reference/ado-api/supports-method.md) con **ADUpdate** e **adUpdateBatch**.  
  
 L'impostazione **adLockPessimistic** non è supportata se la proprietà [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) è impostata su **adUseClient.** Se viene impostato un valore non supportato, non verrà generato alcun errore. verrà invece usato il **LockType** supportato più vicino.  
  
 La proprietà **LockType** è in lettura/scrittura quando il **Recordset** è chiuso e in sola lettura quando è aperto.  
  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Tipi di blocchi](../../../ado/guide/data/types-of-locks.md)
