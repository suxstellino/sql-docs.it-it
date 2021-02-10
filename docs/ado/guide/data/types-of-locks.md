---
description: Tipi di blocchi
title: Tipi di blocchi | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- AdLockBatchOptimistic [ADO]
- AdLockReadOnly [ADO]
- AdLockUnspecified [ADO]
- locks [ADO], types
- AdLockOptimistic [ADO]
- AdLockPessimistic [ADO]
ms.assetid: 12a978c0-b8a0-4ef0-87f0-a43c13659272
author: rothja
ms.author: jroth
ms.openlocfilehash: 8d72b169247df929d923f3b2362a497647b8f8e2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036901"
---
# <a name="types-of-locks"></a>Tipi di blocchi
## <a name="adlockbatchoptimistic"></a>adLockBatchOptimistic  
 Indica gli aggiornamenti in batch ottimistici. Obbligatorio per la modalità di aggiornamento batch.  
  
 Molte applicazioni recuperano un numero di righe in una sola volta e quindi devono eseguire aggiornamenti coordinati che includono l'intero set di righe da inserire, aggiornare o eliminare. Con i cursori batch, è necessario un solo round trip al server, migliorando così le prestazioni degli aggiornamenti e diminuendo il traffico di rete. Utilizzando una libreria di cursori batch, è possibile creare un cursore statico e quindi disconnettersi dall'origine dati. A questo punto è possibile apportare modifiche alle righe e successivamente riconnettersi e pubblicare le modifiche nell'origine dati in un batch.  
  
## <a name="adlockoptimistic"></a>adLockOptimistic  
 Indica che il provider utilizza record con blocco ottimistico solo quando si chiama il metodo **Update** . Ciò significa che è possibile che un altro utente modifichi i dati tra il momento in cui si modifica il record e quando si chiama **Update**, che crea conflitti. Utilizzare questo tipo di blocco nelle situazioni in cui le probabilità di collisione sono ridotte o se è possibile risolvere facilmente i conflitti.  
  
## <a name="adlockpessimistic"></a>adLockPessimistic  
 Indica il blocco pessimistico, record per record. Il provider esegue le operazioni necessarie per garantire la corretta modifica dei record, in genere bloccando i record nell'origine dati immediatamente prima della modifica. Naturalmente, ciò significa che i record non sono disponibili ad altri utenti dopo che si è iniziato a modificare, fino a quando non si rilascia il blocco chiamando **Update.** Utilizzare questo tipo di blocco in un sistema in cui non è possibile permettersi modifiche simultanee ai dati, ad esempio in un sistema di prenotazione.  
  
## <a name="adlockreadonly"></a>adLockReadOnly  
 Indica i record di sola lettura. Non è possibile modificare i dati. Un blocco di sola lettura è il tipo di blocco "più veloce" perché non richiede che il server mantenga un blocco sui record.  
  
## <a name="adlockunspecified"></a>adLockUnspecified  
 Non specifica un tipo di blocco.
