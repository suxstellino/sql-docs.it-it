---
title: Cronologia di mirroring del database | Microsoft Docs
description: Informazioni su come visualizzare la cronologia dello stato di mirroring per un database con mirroring in un'istanza del server specificata in SQL Server Management Studio.
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.dbmmonitor.databasemirroringhistory.f1
ms.assetid: 1d6e4b10-4a23-47d7-9918-c417992f09d3
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 4dee4a7fd6632cfb8ef2f4a6950f20979c912b54
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353142"
---
# <a name="database-mirroring-history"></a>Cronologia di mirroring del database
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Utilizzare questa finestra di dialogo per visualizzare la cronologia dello stato del mirroring per un database con mirroring in un'istanza del server specificata.  
  
 **Per utilizzare SQL Server Management Studio per il monitoraggio del mirroring del database**  
  
-   [Avviare il monitoraggio mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="options"></a>Opzioni  
 **Istanza del server**  
 Nome dell'istanza del server dalla quale viene segnalata la cronologia.  
  
 **Database**  
 Nome del database la cui cronologia viene indicata.  
  
 **Filtra elenco per**  
 Elenca i valori di filtro. Scegliere un nuovo valore per aggiornare la griglia in modo automatico.  
  
 I possibili valori di filtro sono:  
  
-   Ultime due ore  
  
-   Ultime quattro ore  
  
-   Ultime otto ore  
  
-   Ultimo giorno  
  
-   Ultimi due giorni  
  
-   Ultimi 100 record  
  
-   Ultimi 500 record  
  
-   Ultimi 1000 record  
  
-   Tutti i record  
  
 **Aggiorna**  
 Fare clic per aggiornare l'elenco di cronologia.  
  
> [!NOTE]  
>  La finestra di dialogo non consente di aggiornare automaticamente l'elenco di cronologia. Per aggiornare l'elenco, fare clic su **Aggiorna** o scegliere un'altra opzione di filtro. Solo i membri del ruolo predefinito del server **sysadmin** possono aggiornare la cronologia di mirroring.  
  
 **History**  
 Consente di visualizzare l'elenco di cronologia. Fare clic su un'intestazione di colonna per ordinare la griglia in base a tale colonna. L'elenco contiene le colonne seguenti:  
  
|Nome colonna|Descrizione|  
|-----------------|-----------------|  
|**Data e ora registrazione**|Timestamp della riga della cronologia.|  
|**Ruolo**|Ruolo del mirroring corrente dell'istanza del server per il database, Principale o Mirror.|  
|**Stato mirroring**|Stato del database:<br /><br /> Disconnesso<br /><br /> Failover in sospeso<br /><br /> Suspended<br /><br /> Sincronizzato<br /><br /> Sincronizzazione in corso<br /><br /> Unknown|  
|**Connessione server di controllo del mirroring del database**|Stato della connessione del server di controllo del mirroring nella sessione di mirroring del database. I possibili valori Connesso o Disconnesso. Se non è presente alcun server di controllo del mirroring, il valore è NULL.|  
|**Log non inviato**|Dimensioni, espresse in kilobyte (KB), del log non inviato nella coda di invio sull'istanza del server principale.|  
|**Tempo invio**|Periodo di tempo approssimativo necessario all'istanza del server principale per inviare il log attualmente nella coda di invio all'istanza del server mirror ( *velocità di invio*). Dato che la frequenza delle transazioni in ingresso può variare notevolmente, il tempo di invio del log rappresenta una stima. Tuttavia, la velocità di invio può essere utile per stimare approssimativamente il tempo necessario per un failover manuale.|  
|**Send Rate**|Velocità cui avviene l'invio delle transazioni all'istanza del server mirror, espressa in KB al secondo.|  
|**Frequenza nuove transazioni**|Velocità cui avviene l'immissione delle transazioni in ingresso nel log dell'entità, espressa in KB al secondo. Per determinare se il mirroring sia in ritardo, aggiornato o in anticipo, confrontare questo valore con il valore **Tempo invio** .|  
|**Transazione non inviata meno recente**|Tempo di memorizzazione della transazione non inviata meno recente nella coda di invio. Il tempo di memorizzazione di questa transazione indica la quantità di transazioni, espressa in minuti, non ancora inviata all'istanza del server mirror. Questo valore consente di misurare la potenziale perdita di dati in termini di tempo.|  
|**Log non ripristinato**|Quantità del log in attesa nella coda di rollforward, espressa in KB.|  
|**Tempo ripristino**|Numero approssimativo di minuti necessari per l'applicazione del log presente nella coda di rollforward al database mirror.|  
|**Velocità ripristino**|Velocità cui avviene il ripristino delle transazioni nel database mirror, espressa in KB al secondo.|  
|**Overhead commit mirror**|Ritardo medio per transazione in millisecondi (solo in modalità sincrone). Questo ritardo rappresenta la quantità di overhead generato mentre l'istanza del server principale è in attesa che l'istanza del server mirror scriva il record di log della transazione nella coda di rollforward.|  
  
## <a name="see-also"></a>Vedere anche  
 [Avviare il monitoraggio mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [Monitoraggio del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [Avvio della Configurazione guidata sicurezza mirroring del database &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
  
