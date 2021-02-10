---
description: Significato della posizione del cursore
title: Significato della posizione del cursore | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- server-side cursors [ADO]
- cursors [ADO], client-side
- client-side cursors [ADO]
- cursors [ADO], server-side
ms.assetid: 70ef5b1c-0459-41a1-b796-031f61a29a8a
author: rothja
ms.author: jroth
ms.openlocfilehash: a3caa56a5c3560367cc2f816c6aa2b1c12cbba2b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036931"
---
# <a name="the-significance-of-cursor-location"></a>Significato della posizione del cursore
Ogni cursore usa risorse temporanee per conservare i dati. Queste risorse possono essere memoria, un file di paging del disco, file su disco temporanei o persino archiviazione temporanea nel database. Il cursore viene chiamato cursore sul *lato client* quando queste risorse si trovano nel computer client. Il cursore viene chiamato cursore sul *lato server* quando queste risorse si trovano nel server.  
  
## <a name="client-side-cursors"></a>Cursori Client-Side  
 In ADO chiamare per un cursore sul lato client usando il **CursorLocationEnum adUseClient.** Con un cursore non keyset lato client, il server invia l'intero set di risultati attraverso la rete al computer client. Il computer client fornisce e gestisce le risorse temporanee necessarie per il cursore e il set di risultati. L'applicazione sul lato client può esplorare l'intero set di risultati per determinare le righe richieste.  
  
 I cursori lato client statici e gestiti da keyset possono inserire un carico significativo sulla workstation se includono troppe righe. Sebbene tutte le librerie di cursori siano in grado di creare cursori con migliaia di righe, le applicazioni progettate per recuperare tali set di righe di grandi dimensioni potrebbero non funzionare correttamente. Esistono eccezioni, ovviamente. Per alcune applicazioni, un cursore lato client di grandi dimensioni potrebbe essere perfettamente appropriato e le prestazioni potrebbero non essere un problema.  
  
 Un vantaggio ovvio del cursore sul lato client è la risposta rapida. Dopo che il set di risultati è stato scaricato nel computer client, l'esplorazione delle righe è molto rapida. L'applicazione è in genere più scalabile con cursori sul lato client perché i requisiti delle risorse del cursore vengono posizionati in ogni client separato e non nel server.  
  
## <a name="server-side-cursors"></a>Cursori Server-Side  
 In ADO, chiamare per un cursore sul lato server tramite il **CursorLocationEnum adUseServer come.** Con un cursore sul lato server, il server gestisce il set di risultati utilizzando le risorse fornite dal computer server. Il cursore sul lato server restituisce solo i dati richiesti sulla rete. Questo tipo di cursore può talvolta offrire prestazioni migliori rispetto al cursore sul lato client, soprattutto in situazioni in cui un traffico di rete eccessivo costituisce un problema.  
  
 Tuttavia, è importante sottolineare che un cursore sul lato server è ad almeno un consumo di risorse preziose del server per ogni client attivo. È necessario pianificare di conseguenza per assicurarsi che l'hardware del server sia in grado di gestire tutti i cursori sul lato server richiesti dai client attivi. Inoltre, un cursore sul lato server può essere lento perché fornisce solo l'accesso a singola riga. non è disponibile alcun cursore batch.  
  
 I cursori sul lato server sono utili per l'inserimento, l'aggiornamento o l'eliminazione di record. Con i cursori sul lato server è possibile avere più istruzioni attive nella stessa connessione.
