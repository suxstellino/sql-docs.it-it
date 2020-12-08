---
title: Oggetto Transactions di SQL Server | Microsoft Docs
description: Informazioni sull'oggetto Transactions, che fornisce i contatori per monitorare le transazioni attive nel motore di database e gli effetti delle transazioni in SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:Transactions
- Transactions object
ms.assetid: 85240267-78fd-476a-9ef6-010d6cf32dd8
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2f86c13a6f4cc6672b457653f6d923b63245fbc7
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505546"
---
# <a name="sql-server-transactions-object"></a>Oggetto Transactions di SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  L'oggetto **Transactions**, disponibile in Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], include i contatori per il monitoraggio del numero di transazioni attive in un'istanza di [!INCLUDE[ssDE](../../includes/ssde-md.md)], nonché per valutare gli effetti di tali transazioni sulle risorse, ad esempio l'archivio delle versioni di riga del livello di isolamento dello snapshot in **tempdb**. Le transazioni sono unità logiche di lavoro. Per salvaguardare l'integrità logica dei dati, l'intero set di operazioni deve essere eseguito correttamente o cancellato da un database. Le transazioni vengono utilizzate per tutte le modifiche apportate ai dati nei database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Quando si imposta un database in modo da consentire il livello di isolamento dello snapshot, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve tenere traccia delle modifiche apportate a ciascuna riga del database. A ogni modifica di una riga, una copia della riga precedente alla modifica viene registrata in un archivio delle versioni di riga in **tempdb**. È possibile usare molti dei contatori dell'oggetto **Transaction** per eseguire il monitoraggio delle dimensioni e della percentuale di crescita dell'archivio delle versioni di riga in **tempdb**.  
  
 I contatori dell'oggetto **Transactions** segnalano tutte le transazioni in un'unica istanza di [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 La tabella seguente descrive i contatori di **SQLServer:Transactions** .  
  
|Contatori dell'oggetto Transactions di SQL Server|Descrizione|  
|--------------------------------------|-----------------|  
|**Spazio disponibile in tempdb (KB)**|Spazio disponibile in **tempdb** espresso in KB. Lo spazio disponibile deve essere sufficiente per contenere sia l'archivio delle versioni del livello di isolamento dello snapshot che tutti i nuovi oggetti temporanei creati in questa istanza di [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|**Tempo massimo esecuzione transazione**|Durata in secondi dall'inizio della transazione che è stata attiva più a lungo di tutte le altre transazioni correnti. Questo contatore mostra attività solo quando il database è nel livello di isolamento dello snapshot Read committed. Non registra attività se il database si trova in un altro livello di isolamento.|  
|**Transazioni di versione non snapshot**|Numero di transazioni attualmente attive che non usano il livello di isolamento dello snapshot e hanno apportato modifiche ai dati che hanno generato versioni di riga nell'archivio delle versioni di riga di **tempdb** .|  
|**Transazioni snapshot**|Numero delle transazioni attualmente attive che utilizzano il livello di isolamento dello snapshot.<br /><br /> Nota: il contatore dell'oggetto **Transazioni snapshot** risponde al primo accesso ai dati, non quando viene eseguita l'istruzione `BEGIN TRANSACTION`.|  
|**Transazioni**|Numero delle transazioni attualmente attive di tutti i tipi.|  
|**Percentuale conflitti aggiornamento**|Percentuale delle transazioni che utilizzano il livello di isolamento dello snapshot e hanno rilevato conflitti di aggiornamento nell'ultimo secondo. Un conflitto di aggiornamento si verifica quando una transazione del livello di isolamento dello snapshot tenta di modificare una riga modificata da un'altra transazione di cui non è stato eseguito il commit all'avvio della transazione del livello di isolamento dello snapshot.|  
|**Base percentuale conflitti aggiornamento**|Solo per uso interno.|
|**Transazioni snapshot di aggiornamento**|Numero delle transazioni attualmente attive che utilizzano il livello di isolamento dello snapshot e che hanno apportato modifiche ai dati.|  
|**Frequenza pulizia versioni (KB/s)**|Frequenza, espressa in KB al secondo, con cui le versioni di riga vengono rimosse dall'archivio delle versioni di riga del livello di isolamento dello snapshot in **tempdb**.|  
|**Frequenza generazione versioni (KB/s)**|Frequenza, espressa in KB al secondo, con cui nuove versioni di riga vengono aggiunte all'archivio delle versioni di riga del livello di isolamento dello snapshot in **tempdb**.|  
|**Dimensioni archivio versioni (KB)**|Spazio, espresso in KB, usato in **tempdb** per l'archiviazione delle versioni di riga del livello di isolamento dello snapshot.|  
|**Conteggio unità archivio versioni**|Numero di unità di allocazione attive nell'archivio delle versioni di riga del livello di isolamento dello snapshot in **tempdb**.|  
|**Creazione unità archivio versioni**|Numero di unità di allocazione create nell'archivio delle versioni di riga del livello di isolamento snapshot dall'avvio dell'istanza di [!INCLUDE[ssDE](../../includes/ssde-md.md)] .|  
|**Troncamento unità archivio versioni**|Numero di unità di allocazione rimosse dall'archivio delle versioni di riga del livello di isolamento snapshot dall'avvio dell'istanza di [!INCLUDE[ssDE](../../includes/ssde-md.md)] .|  
  
## <a name="see-also"></a>Vedere anche  
 [Monitorare l'utilizzo delle risorse &#40;Monitor di sistema&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
