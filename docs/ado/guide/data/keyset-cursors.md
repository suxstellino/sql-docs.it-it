---
description: Cursori keyset
title: Cursori keyset | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Keyset cursors [ADO]
- cursors [ADO], Keyset
ms.assetid: 14b51b17-6fd9-4146-af45-ca4b0fe6d48a
author: rothja
ms.author: jroth
ms.openlocfilehash: 4071772212226b2b09886476c6577cac73cd3f79
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037271"
---
# <a name="keyset-cursors"></a>Cursori keyset
Il cursore keyset fornisce funzionalità tra un cursore statico e un cursore dinamico nella capacità di rilevare le modifiche. Come un cursore statico, non sempre rileva le modifiche all'appartenenza e all'ordine del set di risultati. Come un cursore dinamico, rileva le modifiche ai valori delle righe nel set di risultati.  
  
 I cursori gestiti da keyset vengono controllati da un set di identificatori univoci (chiavi), noti come keyset. Le chiavi sono costituite da un set di colonne che identificano in modo univoco le righe del set di risultati. Il keyset corrisponde al set di valori di chiave di tutte le righe restituite dall'istruzione della query.  
  
 Con i cursori gestiti da keyset viene generata e salvata una chiave per ogni riga nel cursore e tale chiave viene archiviata nella workstation client o nel server. Quando si accede a ogni riga, la chiave archiviata viene usata per recuperare i valori correnti dei dati dall'origine dati. In un cursore gestito da keyset, quando il set di chiavi è completamente popolato, l'appartenenza del set di risultati è bloccata. Le aggiunte o gli aggiornamenti successivi che interessano l'appartenenza faranno parte del set di risultati solo quando verrà riaperto.  
  
 Le modifiche ai valori dei dati (effettuate dal proprietario o da altri processi del keyset) sono visibili quando l'utente scorre il set di risultati. Gli inserimenti eseguiti all'esterno del cursore da altri processi sono visibili solo se il cursore viene chiuso e riaperto. Gli inserimenti eseguiti dall'interno del cursore sono visibili alla fine del set di risultati.  
  
 Quando un cursore gestito da keyset tenta di recuperare una riga che è stata eliminata, la riga viene visualizzata come "Hole" nel set di risultati. La chiave per la riga è presente nel keyset, ma la riga non esiste più nel set di risultati. Se vengono aggiornati i valori di chiave in una riga, la riga viene considerata eliminata e quindi inserita, quindi tali righe vengono visualizzate anche come buchi nel set di risultati. Mentre un cursore gestito da keyset è sempre in grado di rilevare le righe eliminate da altri processi, può facoltativamente rimuovere le chiavi per le righe eliminate. I cursori gestiti da keyset che eseguono questa operazione non possono rilevare le proprie eliminazioni perché l'evidenza è stata rimossa.  
  
 Un aggiornamento a una colonna chiave funziona come un'eliminazione della chiave precedente seguita da un inserimento della nuova chiave. Il nuovo valore della chiave non è visibile se l'aggiornamento non è stato eseguito tramite il cursore. Se l'aggiornamento è stato eseguito tramite il cursore, il nuovo valore della chiave è visibile alla fine del set di risultati.  
  
 Si verifica una variazione sui cursori gestiti da keyset, detti cursori standard gestiti da keyset. In un cursore standard gestito da keyset, l'appartenenza delle righe nel set di risultati e l'ordine delle righe vengono fissate al momento dell'apertura del cursore, ma vengono visualizzate le modifiche apportate ai valori del proprietario del cursore e le modifiche di cui è stato eseguito il commit apportate da altri processi. Se una modifica annulla la qualifica di una riga per l'appartenenza o influiscono sull'ordine di una riga, la riga non viene visualizzata o spostata a meno che il cursore non venga chiuso e riaperto. I dati inseriti non vengono visualizzati, ma le modifiche apportate ai dati esistenti vengono visualizzate durante il recupero delle righe.  
  
 Il cursore gestito da keyset è difficile da utilizzare correttamente perché la sensibilità alle modifiche dei dati dipende da molte circostanze diverse, come descritto in precedenza. Tuttavia, se l'applicazione non riguarda gli aggiornamenti simultanei, può gestire a livello di codice chiavi non valide e deve accedere direttamente a determinate righe con chiave, il cursore gestito da keyset potrebbe funzionare. Utilizzare il **CursorTypeEnum adOpenKeyset** per indicare che si desidera utilizzare un cursore keyset in ADO.  
  
## <a name="see-also"></a>Vedere anche  
 [Cursori di sola trasmissione](./forward-only-cursors.md)   
 [Cursori statici](./static-cursors.md)   
 [Cursori dinamici](./dynamic-cursors.md)