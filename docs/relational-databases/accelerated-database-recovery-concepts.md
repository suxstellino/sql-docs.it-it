---
description: Recupero del database accelerato
title: Ripristino accelerato del database | Microsoft Docs
ms.date: 05/20/2020
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- accelerated database recovery [SQL Server], recovery-only
- database recovery [SQL Server]
author: mashamsft
ms.author: mathoma
ms.reviewer: kfarlee
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: 70b68fa62c523a4c65bb4e70538e1882bc67290d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350075"
---
# <a name="accelerated-database-recovery"></a>Recupero del database accelerato

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Il ripristino accelerato del database (ADR, Accelerated Database Recovery) consente di migliorare la disponibilità del database, in particolare in presenza di transazioni a esecuzione prolungata, riprogettando il processo di ripristino del motore di database SQL. Il ripristino accelerato del database è una novità di SQL Server 2019. 

È disponibile anche per i database nel database SQL di Azure, in Istanza gestita di SQL di Azure e Azure Synapse SQL. Per impostazione predefinita è abilitato nel database SQL e in Istanza gestita di SQL, dove non può essere disabilitato. 

## <a name="overview"></a>Panoramica

I principali vantaggi del ripristino accelerato del database sono i seguenti:

- **Ripristino rapido e coerente del database**

  Grazie al ripristino accelerato del database, le transazioni a esecuzione prolungata non influiscono sul tempo di ripristino complessivo e ciò consente un ripristino rapido e coerente del database indipendentemente dal numero di transazioni attive nel sistema o dalle relative dimensioni.

- **Rollback di transazione istantaneo**

  Con il ripristino accelerato del database, il rollback di transazione è istantaneo, indipendentemente dal tempo in cui la transazione è stata attiva o dal numero di aggiornamenti eseguiti.

- **Troncamento del log aggressivo**

  Con il ripristino accelerato del database, il log delle transazioni viene troncato in modo aggressivo, anche in presenza di transazioni a esecuzione prolungata attive, per impedire la crescita fuori controllo.

## <a name="the-current-database-recovery-process"></a>Processo di ripristino del database corrente

Senza il ripristino accelerato del database, il processo di ripristino del database in SQL Server segue il modello [ARIES](https://people.eecs.berkeley.edu/~brewer/cs262/Aries.pdf) ed è costituito da tre fasi, illustrate nel diagramma seguente e spiegate in modo più dettagliato dopo il diagramma.

![Processo di ripristino corrente](./media/accelerated-database-recovery-concepts/current-recovery-process.png)

- **Fase di analisi**

  SQL Server esegue un'analisi in avanti del log delle transazioni dall'inizio dell'ultimo checkpoint riuscito, o il numero di sequenza del file di log (LSN) della pagina dirty meno recente, fino alla fine, per determinare lo stato di ogni transazione al momento dell'arresto di SQL Server.

- **Fase di rollforward**

  SQL Server esegue un'analisi in avanti del log delle transazioni dalla transazione meno recente di cui non è stato eseguito il commit fino alla fine, per portare il database allo stato in cui si trovava al momento dell'arresto anomalo del sistema, eseguendo il rollforward di tutte le operazioni di cui è stato eseguito il commit.

- **Fase di rollback**

  Per ogni transazione attiva al momento dell'arresto anomalo, SQL Server attraversa il log all'indietro, eseguendo il rollback delle operazioni eseguite dalla transazione.

In base a questa progettazione, il tempo impiegato dal motore di database per il ripristino da un riavvio imprevisto è (approssimativamente) proporzionale alle dimensioni della transazione attiva più lunga nel sistema al momento dell'arresto anomalo. Il ripristino richiede un rollback di tutte le transazioni incomplete. La quantità di tempo necessaria è proporzionale al lavoro eseguito dalla transazione e al tempo per cui è stata attiva. Il processo di ripristino di SQL Server può quindi richiedere molto tempo in presenza di transazioni a esecuzione prolungata, ad esempio operazioni di inserimento bulk di grandi dimensioni o operazioni di compilazione di indici su una tabella di grandi dimensioni.

Inoltre, l'annullamento o il rollback di una transazione di grandi dimensioni in base a questa progettazione può richiedere molto tempo in quanto viene usata la stessa fase di ripristino di rollback descritta in precedenza.

Il motore di database non può inoltre troncare il log delle transazioni quando sono presenti transazioni a esecuzione prolungata perché i relativi record di log sono necessari per i processi di ripristino e rollback. Di conseguenza, alcuni log delle transazioni diventano molto grandi e utilizzano ingenti quantità di spazio su disco.

## <a name="the-accelerated-database-recovery-process"></a>Processo di ripristino accelerato del database

Il ripristino accelerato del database risolve i problemi precedenti riprogettando completamente il processo di ripristino del motore di database per:

- Rendere il ripristino costante e istantaneo, in quanto non è necessario analizzare il log da o fino all'inizio della transazione attiva meno recente. Con il ripristino accelerato del database, il log delle transazioni viene elaborato solo dall'ultimo checkpoint completato o dal numero di sequenza del file di log (LSN) della pagina dirty meno recente. Di conseguenza, le transazioni a esecuzione prolungata non influiscono sul tempo di ripristino.
- Ridurre al minimo lo spazio necessario per il log delle transazioni in quanto non è più necessario elaborare il log per l'intera transazione. Di conseguenza, è possibile troncare il log delle transazioni in modo aggressivo quando si verificano checkpoint e backup.

A livello generale, il ripristino accelerato del database consente il ripristino rapido del database tramite il controllo delle versioni di tutte le modifiche fisiche del database e il rollback solo delle operazioni logiche, che sono limitate e che consentono un rollback quasi istantaneo. Tutte le transazioni attive al momento dell'arresto anomalo vengono contrassegnate come interrotte e, di conseguenza, tutte le versioni generate da queste transazioni possono essere ignorate dalle query utente simultanee.

Il processo di ripristino accelerato del database prevede le stesse tre fasi del processo di ripristino corrente. Il funzionamento di queste fasi con il ripristino accelerato del database è illustrato nel diagramma seguente.

![Processo di ripristino accelerato del database](./media/accelerated-database-recovery-concepts/adr-recovery-process.png)

- **Fase di analisi**

  Il processo è identico a quello corrente, con l'aggiunta della ricostruzione di sLog e della copia dei record di log per le operazioni senza controllo delle versioni.
  
- **Fase di rollforward**

  Suddivisa in due fasi secondarie
  - Fase secondaria 1

      Rollforward da sLog (dalla transazione meno recente di cui non è stato eseguito il commit fino all'ultimo checkpoint). La fase di rollforward è un'operazione rapida, in quanto è necessario elaborare solo pochi record da sLog.

  - Fase secondaria 2

     La fase di rollforward dal log delle transazioni inizia dall'ultimo checkpoint, invece che dalla transazione meno recente di cui non è stato eseguito il commit.
     
- **Fase di rollback**

   La fase di rollback con il ripristino accelerato del database viene completata quasi istantaneamente usando sLog per il rollback delle operazioni senza controllo delle versioni e l'archivio versioni permanente (PVS, Persisted Version Store) con ripristino logico per eseguire il rollback basato sulla versione a livello di riga.

È anche possibile guardare questo video di 8 minuti che spiega il recupero accelerato del database

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Advanced-Database-Recovery--Data-Exposed/player?WT.mc_id=dataexposed-c9-niner]

## <a name="adr-recovery-components"></a>Componenti del ripristino accelerato del database

I quattro componenti chiave del ripristino accelerato del database sono i seguenti:

- **Archivio versioni permanente**

  L'archivio versioni permanente è un meccanismo del motore di database per salvare in modo permanente le versioni di riga generate nel database stesso al posto di usare l'archivio versioni `tempdb` tradizionale. L'archivio versioni permanente consente l'isolamento delle risorse e migliora la disponibilità di database secondari leggibili.

- **Ripristino logico**

  Il ripristino logico è il processo asincrono responsabile dell'esecuzione del rollback basato sulla versione a livello di riga, che consente di eseguire in modo istantaneo il rollback di transazione e l'annullamento per tutte le operazioni con controllo delle versioni.

  - Tiene traccia di tutte le transazioni interrotte
  - Esegue il rollback usando l'archivio versioni permanente per tutte le transazioni utente
  - Rilascia tutti i blocchi immediatamente dopo l'interruzione della transazione

- **sLog**

  sLog è un flusso di log in memoria secondario che archivia i record di log per le operazioni senza controllo delle versioni, ad esempio l'invalidamento della cache dei metadati, le acquisizioni di blocchi e così via. sLog ha le caratteristiche seguenti:

  - Volume ridotto e in memoria
  - Salvataggio in modo permanente sul disco mediante la serializzazione durante il processo di checkpoint
  - Troncamento periodico quando viene eseguito il commit delle transazioni
  - Accelerazione delle fasi di rollback e rollforward grazie all'elaborazione solo delle operazioni senza controllo delle versioni  
  - Troncamento del log delle transazioni aggressivo mantenendo solo i record di log necessari

- **Pulizia**

  La pulizia è un processo asincrono che viene riattivato periodicamente e pulisce le versioni di pagina non necessarie.

## <a name="who-should-consider-accelerated-database-recovery"></a>Utenti che possono trarre vantaggio dal ripristino accelerato del database

Per i tipi di clienti seguenti è consigliabile prendere in considerazione l'abilitazione del ripristino accelerato del database:

- Clienti con carichi di lavoro con transazioni a esecuzione prolungata.
- Clienti che hanno riscontrato casi in cui le transazioni attive causano un aumento significativo delle dimensioni del log delle transazioni.  
- Clienti che hanno riscontrato lunghi periodi di indisponibilità del database a causa di un ripristino a esecuzione prolungata di SQL Server, ad esempio in caso di riavvio imprevisto di SQL Server o rollback di transazione manuale.

>[!IMPORTANT]
>Il ripristino accelerato del database non è supportato per i database registrati nel mirroring del database.

## <a name="see-also"></a>Vedere anche  

[Gestire il ripristino accelerato del database](accelerated-database-recovery-management.md)
