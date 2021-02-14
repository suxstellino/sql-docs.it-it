---
title: Stati di mirroring (SQL Server) | Microsoft Docs
description: Informazioni sugli stati del database in una sessione di mirroring del database in SQL Server. Lo stato riflette lo stato delle comunicazioni, il flusso di dati e la differenza nei dati.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- states [SQL Server], database mirroring
- PENDING_FAILOVER state
- mirroring states [SQL Server]
- DISCONNECTED state
- SYNCHRONIZING state
- SYNCHRONIZED state
- SUSPENDED state
- database mirroring [SQL Server], states
ms.assetid: 90062917-74f9-471b-b49e-bc153ae1a468
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: bec627b89d9aa1265fa70627185a5f18967562ac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343296"
---
# <a name="mirroring-states-sql-server"></a>Stati di mirroring (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Durante una sessione di mirroring del database, il database con mirroring si trova sempre in uno stato specifico, ovvero lo *stato di mirroring*. Lo stato del database riflette lo stato delle comunicazioni, il flusso di dati e la differenza nei dati tra i partner. La sessione di mirroring del database adotta lo stesso stato del database principale.  
  
 Nel corso della sessione di mirroring del database viene eseguito il monitoraggio reciproco delle istanze del server. Per eseguire il monitoraggio del database, i partner utilizzano lo stato di mirroring. Ad eccezione dello stato PENDING_FAILOVER, lo stato del database principale è sempre identico a quello del database mirror. Se per la sessione è impostato un server di controllo del mirroring, per eseguire il monitoraggio di tale server ciascun partner utilizza il relativo stato della connessione (CONNECTED o DISCONNECTED).  
  
 Gli stati di mirroring possibili del database sono i seguenti:  
  
|Stato mirroring|Descrizione|  
|---------------------|-----------------|  
|SYNCHRONIZING|Il contenuto del database mirror è in ritardo rispetto a quello del database principale. Il server principale invia i record del log al server mirror, che applica le modifiche al database mirror ai fini del rollforward.<br /><br /> All'inizio di una sessione di mirroring, lo stato del database è SYNCHRONIZING. Il server principale soddisfa le richieste del database e il mirror tenta di aggiornarsi.|  
|SYNCHRONIZED|Quando il server mirror è sufficientemente aggiornato rispetto al server principale, lo stato di mirroring diventa SYNCHRONIZED. Il database resta in questo stato fino a quando il server principale continua a inviare modifiche al server mirror e quest'ultimo continua ad applicare le modifiche al database mirror.<br /><br /> Se la protezione delle transazioni è impostata su FULL, il failover automatico e quello manuale sono supportati nello stato SYNCHRONIZED, quindi non può verificarsi una perdita di dati dopo un failover.<br /><br /> Se la protezione delle transazioni è disattivata, è possibile che si verifichi la perdita di dati anche in stato SYNCHRONIZED.<br /><br /> In SQL Server Management Studio lo stato del database visualizzato è Ripristino. Per lo stato effettivo, eseguire una query sulla colonna `mirroring_state_desc` in [sys.database_mirroring](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md) |  
|SUSPENDED|La copia mirror del database non è disponibile. Il database principale viene eseguito senza inviare log al server mirror. Tale condizione è nota come *esecuzione senza mirroring*. Si tratta dello stato attivo dopo un failover.<br /><br /> Una sessione può diventare SUSPENDED anche in seguito a errori di rollforward oppure se viene sospesa dall'amministratore.<br /><br /> Lo stato SUSPENDED è persistente in quanto non viene modificato in seguito a operazioni di chiusura e avvio dei partner.|  
|PENDING_FAILOVER|Questo stato viene applicato solo sul server principale dopo l'avvio di un failover, quando il server non è ancora passato al ruolo di mirror.<br /><br /> Quando il failover viene avviato, lo stato del database principale diventa PENDING_FAILOVER, vengono rapidamente interrotte eventuali connessioni utente e subito dopo il database assume il ruolo di mirror.|  
|DISCONNECTED|Le comunicazioni tra un partner e l'altro sono state interrotte.|  
  
## <a name="see-also"></a>Vedere anche  
 [Monitoraggio del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)  
  
  
