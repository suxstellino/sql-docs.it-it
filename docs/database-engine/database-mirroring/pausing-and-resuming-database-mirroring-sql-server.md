---
title: Sospendere e riprendere il mirroring del database
description: Informazioni su come sospendere e riprendere in seguito una sessione di mirroring del database di SQL Server per mantenere lo stato della sessione durante la sospensione del mirroring.
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- sessions [SQL Server], database mirroring
- resuming database mirroring
- database mirroring [SQL Server], pausing
- database mirroring [SQL Server], resuming
- pausing database mirroring
ms.assetid: c67802c6-ee8c-4cbd-a6d4-f7b80413a4ab
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c565c8ca660c0616c061ba70f3c24d3a7d347191
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343733"
---
# <a name="pausing-and-resuming-database-mirroring-sql-server"></a>Sospensione e ripresa del mirroring del database (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Il proprietario del database può sospendere una sessione di mirroring del database, riprenderla in qualsiasi momento e preservare così lo stato della sessione durante la sospensione del mirroring. In caso di colli di bottiglia, la sospensione può essere utile per migliorare le prestazioni del server principale.  
  
 Quando si sospende una sessione, il database principale resta disponibile. In caso di sospensione, lo stato della sessione di mirroring viene impostato su SUSPENDED e il database mirror perde la sincronizzazione con il database principale, che viene quindi eseguito in una condizione nota come esposizione senza mirroring.  
  
 È consigliabile riprendere una sessione sospesa il più rapidamente possibile poiché, fintanto che la sessione di mirroring del database rimane sospesa, non è possibile troncare il log delle transazioni. Se la sessione di mirroring del database resta sospesa troppo a lungo, lo spazio del log delle transazioni può esaurirsi rendendo il database non disponibile. Per una spiegazione di questo problema, vedere "Impatto della sospensione e ripresa sul troncamento del log" di seguito in questo argomento.  
  
> [!IMPORTANT]  
>  In un servizio forzato, quando il server principale originale esegue nuovamente la connessione il mirroring viene sospeso. Se si riprende il mirroring in questa situazione, è possibile che si verifichi una perdita di dati nel server principale originale. Per informazioni sulla gestione della potenziale perdita di dati, vedere [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).  
  
 **Contenuto dell'argomento**  
  
-   [Impatto della sospensione e ripresa sul troncamento del log](#EffectOnLogTrunc)  
  
-   [Evitare il riempimento del log delle transazioni](#AvoidFullLog)  
  
-   [Attività correlate](#RelatedTasks)  
  
##  <a name="how-pausing-and-resuming-affect-log-truncation"></a><a name="EffectOnLogTrunc"></a> Impatto della sospensione e ripresa sul troncamento del log  
 In genere, quando su un database viene eseguito un checkpoint automatico, il relativo log delle transazioni viene troncato in corrispondenza di tale checkpoint dopo il successivo backup del log. Durante il periodo in cui una sessione di mirroring del database rimane sospesa, tutti i record del log corrente restano attivi poiché il server principale è in attesa di inviarli al server mirror. I record del log non inviati si accumulano nel log delle transazioni del database principale fino alla ripresa della sessione e al momento dell'invio dei record del log dal server principale al server mirror.  
  
 Quando la sessione viene ripresa, il server principale inizia immediatamente a inviare i record del log accumulati al server mirror. Dopo la conferma da parte del server mirror che il record del log corrispondente al checkpoint automatico meno recente è stato accodato, il server principale tronca il log del database principale fino a tale checkpoint. Il server mirror tronca la coda di rollforward in corrispondenza dello stesso record del log. Poiché questo processo viene ripetuto per ogni checkpoint successivo, il log viene troncato in più fasi per i singoli checkpoint.  
  
> [!NOTE]  
>  Per altre informazioni sui checkpoint e sul troncamento del log, vedere [Checkpoint di database &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md).  
  
##  <a name="avoid-a-full-transaction-log"></a><a name="AvoidFullLog"></a> Evitare il riempimento del log delle transazioni  
 Se il log si riempie, perché raggiunge le dimensioni massime o l'istanza del server esaurisce lo spazio, il database non può eseguire ulteriori aggiornamenti. Per evitare questo problema, è possibile procedere in due modi:  
  
-   Riprendere la sessione di mirroring del database prima che il log esaurisca lo spazio, oppure aggiungere spazio di log. Se si riprende il mirroring, il server principale invia il log cumulativo attivo al server mirror e il database mirror viene posto in stato SYNCHRONIZING. Il server mirror può quindi salvare il log su disco e avviarne il rollforward.  
  
-   Arrestare la sessione di mirroring del database rimuovendo il mirroring.  
  
     A differenza della sospensione di una sessione, la rimozione del mirroring provoca l'eliminazione di tutte le informazioni relative alla sessione di mirroring. Ogni istanza del server partner mantiene la sua copia del database. Se recuperata, pertanto, la copia precedente del database mirror può risultare diversa dalla copia precedente del database principale e indicare un periodo di tempo inferiore rispetto a quello trascorso dalla sospensione della sessione. Per altre informazioni, vedere [Rimozione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Attività correlate  
 **Per sospendere o riprendere il mirroring del database**  
  
-   [Sospendere o riprendere una sessione di mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/pause-or-resume-a-database-mirroring-session-sql-server.md)  
  
 **Per arrestare il mirroring del database**  
  
-   [Rimuovere il mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-database-mirroring-sql-server.md)  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Rimozione del mirroring del database &#40;SQL Server&#41;](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md)  
  
  
