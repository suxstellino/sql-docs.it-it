---
title: Aggiornare i dati nei cursori (OLE DB Driver)
description: Informazioni sul funzionamento di un'applicazione consumer OLE DB Driver per SQL Server con le richieste in un set di righe modificabile tramite i cursori di SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- updating data [SQL Server]
- isolation levels [SQL Server]
- delayed update mode [OLE DB]
- immediate update mode [OLE DB]
- cursors [OLE DB]
- data updates [SQL Server], OLE DB
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 53096897b248205f36dc0a479db0d6750ff4069f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755751"
---
# <a name="updating-data-in-sql-server-cursors"></a>Aggiornamento dei dati nei cursori di SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  In caso di recupero e di aggiornamento di dati mediante cursori [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], un'applicazione consumer OLE DB Driver per SQL Server viene associata in base a considerazioni e vincoli identici a quelli che si applicano a qualsiasi altra applicazione client.  
  
 Solo le righe dei cursori [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] partecipano al controllo di accesso ai dati simultaneo. Quando il consumer richiede un set di righe modificabile, il controllo della concorrenza viene effettuato da DBPROP_LOCKMODE. Per modificare il livello di controllo di accesso simultaneo, il consumer imposta la proprietà DBPROP_LOCKMODE prima di aprire il set di righe.  
  
 I livelli di isolamento della transazione possono provocare ritardi significativi nel posizionamento delle righe se la progettazione delle applicazioni client lascia aperte le transazioni per lunghi periodi di tempo. Per impostazione predefinita, il driver OLE DB per SQL Server usa il livello di isolamento Read Committed specificato da DBPROPVAL_TI_READCOMMITTED. OLE DB Driver per SQL Server supporta l'isolamento della lettura dirty quando la concorrenza del set di righe è di sola lettura. In un set di righe modificabile il consumer può pertanto richiedere un livello di isolamento superiore ma non inferiore.  
  
## <a name="immediate-and-delayed-update-modes"></a>Modalità di aggiornamento immediato e posticipato  
 In modalità di aggiornamento immediato, ogni chiamata a **IRowsetChange::SetData** provoca un round trip al computer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Se il consumer apporta più modifiche a una sola riga, risulta più efficiente inviare tutte le modifiche con un'unica chiamata a **SetData**.  
  
 In modalità di aggiornamento posticipato, viene eseguito un round trip al computer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] per ogni riga indicata nei parametri *cRows* e *rghRows* di **IRowsetUpdate::Update**.  
  
 In entrambe le modalità un round trip rappresenta una transazione distinta quando per il set di righe non è aperto alcun oggetto transazione.  
  
 Quando si usa **IRowsetUpdate::Update**, OLE DB Driver per SQL Server prova a elaborare ogni riga indicata. Eventuali errori dovuti a valori di stato, lunghezza o dati non validi per una riga non determinano l'interruzione dell'elaborazione del driver OLE DB per SQL Server. È possibile che vengano modificate tutte o nessuna delle altre righe che partecipano all'aggiornamento. Quando il driver OLE DB per SQL Server restituisce DB_S_ERRORSOCCURRED, il consumer deve esaminare la matrice *prgRowStatus* restituita per determinare l'errore per una riga specifica.  
  
 Un consumer non deve presupporre che le righe vengano elaborate in base a un ordine specifico. Se un consumer richiede un'elaborazione ordinata di modifica dei dati su più di una riga, deve stabilire l'ordine nella logica dell'applicazione e aprire una transazione per includere il processo.  
  
## <a name="see-also"></a>Vedere anche  
 [Aggiornamento dei dati nei set di righe](../../oledb/ole-db-rowsets/updating-data-in-rowsets.md)  
  
  
