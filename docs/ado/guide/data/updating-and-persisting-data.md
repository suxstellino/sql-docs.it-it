---
description: Aggiornamento e persistenza dei dati
title: Aggiornamento e salvataggio permanente dei dati | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- updating data [ADO]
- data updates [ADO]
- ADO, updating data
ms.assetid: 8dc27274-4f96-43d1-913c-4ff7d01b9a27
author: rothja
ms.author: jroth
ms.openlocfilehash: 90ea623d429a17db86040675b1b0112cda4237e6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032323"
---
# <a name="updating-and-persisting-data"></a>Aggiornamento e persistenza dei dati
I capitoli precedenti hanno illustrato come usare ADO per ottenere i dati in un'origine dati, come spostarsi nei dati e anche come modificare i dati. Naturalmente, se l'obiettivo dell'applicazione è consentire agli utenti di apportare modifiche ai dati, è necessario comprendere come salvare le modifiche. È possibile salvare in modo permanente le modifiche del **Recordset** in un file usando il metodo **Save** oppure è possibile inviare di nuovo le modifiche all'origine dati per l'archiviazione usando i metodi **Update** o **UpdateBatch** .  
  
 Nei capitoli precedenti, i dati sono stati modificati in più righe del **Recordset**. ADO supporta due nozioni di base relative all'aggiunta, all'eliminazione e alla modifica delle righe di dati.  
  
 Il primo concetto è che le modifiche non vengono apportate immediatamente al **Recordset**; vengono invece eseguiti in un *buffer di copia* interno. Se si decide di non voler apportare le modifiche, le modifiche nel buffer di copia vengono scartate. Se si decide di salvare le modifiche, le modifiche apportate al buffer di copia vengono applicate al **Recordset**.  
  
 Il secondo concetto è che le modifiche vengono propagate all'origine dati non appena si dichiara il lavoro su una riga completa (ovvero la modalità *immediata* ) o tutte le modifiche apportate a un set di righe vengono raccolte fino a quando non si dichiara il lavoro per il set completato (ovvero la modalità *batch* ). La proprietà **LockType** determina quando vengono apportate modifiche all'origine dati sottostante. **adLockOptimistic** o **adLockPessimistic** specifica la modalità immediata, mentre **adLockBatchOptimistic** specifica la modalità batch. La proprietà **CursorLocation** può influire sulle impostazioni **LockType** disponibili. Ad esempio, l'impostazione **adLockPessimistic** non è supportata se la proprietà **CursorLocation** è impostata su **adUseClient**.  
  
 In modalità immediata, ogni chiamata del metodo **Update** propaga le modifiche apportate all'origine dati. In modalità batch ogni chiamata di **aggiornamento** o spostamento della posizione della riga corrente Salva le modifiche apportate al buffer di copia, ma solo il metodo **UpdateBatch** propaga le modifiche nell'origine dati.  
  
 In questa sezione vengono trattati gli argomenti seguenti.  
  
-   [Aggiornamento dei dati](../../../ado/guide/data/updating-data.md)  
  
-   [Persistenza dei dati](../../../ado/guide/data/persisting-data.md)
