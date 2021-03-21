---
description: Scrittura di pagine
title: Scrittura di pagine | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- pages
ms.assetid: 409c8753-03c4-436d-839c-6a5879971551
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d0d41d9f67a1c9b210c9b3697e061d55dd3517b5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753211"
---
# <a name="writing-pages"></a>Scrittura di pagine
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

L'I/O di un'istanza di [!INCLUDE[ssDE](../includes/ssde-md.md)] include scritture logiche e fisiche. La scrittura logica viene eseguita quando vengono modificati i dati di una pagina nella cache del buffer. La scrittura fisica viene eseguita quando la pagina viene scritta dalla [cache del buffer](../relational-databases/memory-management-architecture-guide.md) nel disco.

Quando una pagina viene modificata nella cache del buffer, non viene immediatamente riscritta nel disco, ma viene contrassegnata come dirty. Questo significa che possono essere eseguite più scritture logiche di una pagina prima che la pagina stessa venga scritta fisicamente nel disco. Per ogni scrittura logica, viene inserito un record del log delle transazioni nella cache del log, per registrare la modifica. I record di log devono essere scritti sul disco prima che la pagina dirty associata venga rimossa dalla cache buffer e scritta sul disco. SQL Server usa una tecnica nota come registrazione write-ahead che impedisce la scrittura di una pagina dirty prima che il record del log associato venga scritto nel disco. Questa procedura è fondamentale per garantire il corretto funzionamento della gestione del recupero. Per altre informazioni, vedere [Log delle transazioni write-ahead](../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md).

Nella figura seguente viene illustrato il processo di scrittura di una pagina di dati modificata.

![Writing_Pages](../relational-databases/media/writing-pages.gif)

Dopo la scrittura di una pagina tramite Gestione buffer, viene eseguita la ricerca di pagine dirty adiacenti che possano essere incluse in un'unica operazione di scrittura sequenziale. Le pagine adiacenti hanno ID di pagina consecutivi e appartengono allo stesso file, ma non è necessario che siano contigue in memoria. La ricerca continua in avanti e indietro fino a quando non si verifica uno degli eventi seguenti:

 * Viene individuata una pagina pulita.
 * Vengono individuate 32 pagine.
 * Viene trovata una pagina dirty il cui numero di sequenza del file di log (LSN) non è ancora stato scaricato nel log.
 * Viene trovata una pagina per la quale non è possibile impostare immediatamente un latch.

In questo modo, è possibile scrivere l'intero set di pagine nel disco con un'unica operazione di scrittura sequenziale. 

Prima della scrittura, viene aggiunta alla pagina la forma di protezione specificata nel database. Se viene aggiunta la protezione della pagina incompleta, è necessario impostare un latch in modo esclusivo per l'I/O. Ciò è dovuto al fatto che la protezione della pagina incompleta modifica la pagina, rendendola inadatta per la lettura da parte di qualsiasi altro thread. Se viene aggiunta la protezione della pagina checksum o se il database non utilizza la protezione della pagina, la pagina viene associata a un latch UP (relativo alla data) per l'I/O. Questo latch impedisce a chiunque altro di modificare la pagina durante la scrittura, ma consente tuttavia ai lettori di utilizzarla. Per altre informazioni sulle opzioni di protezione della pagina per quanto riguarda l'I/O su disco, vedere [Gestione del buffer](../relational-databases/memory-management-architecture-guide.md).

Una pagina dirty può venire scritta nel disco in tre modi: 

* Scrittura Lazywriter   
 Lazywriter è un processo di sistema che mantiene disponibili i buffer liberi rimuovendo dalla cache buffer le pagine utilizzate con meno frequenza. Le pagine dirty vengono scritte per prime nel disco. 

* Eager Writer   
 Tramite il processo Eager Writer vengono scritte le pagine di dati dirty associate a operazioni a registrazione minima, ad esempio inserimento bulk e selezione. Questo processo consente la creazione e la scrittura di nuove pagine in parallelo. Questo significa che l'operazione che ha eseguito la chiamata non deve attendere il completamento dell'intera operazione prima della scrittura delle pagine nel disco.

* Checkpoint   
 Tramite il processo di gestione dei checkpoint viene eseguita periodicamente l'analisi della cache buffer alla ricerca di buffer con pagine di un database specifico e tutte le pagine dirty vengono scritte nel disco. I checkpoint consentono di risparmiare tempo durante un successivo recupero, grazie alla creazione di un punto in cui è certo che tutte le pagine dirty siano state scritte sul disco. L'utente può richiedere un'operazione di checkpoint utilizzando il comando CHECKPOINT oppure in [!INCLUDE[ssDE](../includes/ssde-md.md)] possono venire generati checkpoint automatici in base alla quantità di spazio del log utilizzato e al tempo trascorso dall'ultimo checkpoint. Viene inoltre generato un checkpoint quando si verificano determinate attività, ad esempio quando un file di dati o di log viene aggiunto o rimosso da un database oppure quando viene arrestata l'istanza di SQL Server. Per altre informazioni, vedere [Relazione tra i checkpoint e la parte attiva del log](../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md).

I processi di scrittura Lazywriter, Eager Writer e di gestione dei checkpoint non attendono il completamento dell'operazione di I/O. Questi processi utilizzano sempre l'I/O asincrono, o sovrapposto, e continuano a eseguire altre operazioni, verificando solo successivamente se l'operazione di I/O è stata eseguita correttamente. In questo modo, in SQL Server viene ottimizzato l'utilizzo della CPU e delle risorse di I/O per le attività appropriate.

## <a name="see-also"></a>Vedere anche
[Guida sull'architettura di pagina ed extent](../relational-databases/pages-and-extents-architecture-guide.md)   
 [Lettura di pagine](../relational-databases/reading-pages.md)
