---
description: Lettura di pagine
title: Lettura di pagine | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- pages
ms.assetid: f8da760e-aacb-4661-9f3a-2578d8c11e4e
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 7eeef502690d3253d14d226a598f1d8f5078a040
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755481"
---
# <a name="reading-pages"></a>Lettura di pagine
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

L'I/O di un'istanza del [!INCLUDE[ssDE](../includes/ssde-md.md)] di SQL Server include letture logiche e fisiche. La lettura logica viene eseguita ogni volta che il [!INCLUDE[ssDE](../includes/ssde-md.md)] richiede una pagina dalla [cache del buffer](../relational-databases/memory-management-architecture-guide.md). Se la pagina non si trova nella cache buffer, viene prima eseguita una lettura fisica per copiare la pagina dal disco alla cache.

Le richieste di lettura generate da un'istanza di [!INCLUDE[ssDE](../includes/ssde-md.md)] sono controllate dal motore relazionale e ottimizzate dal motore di archiviazione. Il motore relazionale determina il metodo di accesso più efficace, ad esempio analisi di tabella, analisi di indice o lettura basata su chiavi. I metodi di accesso e i componenti di Gestione buffer del motore di archiviazione determinano il modello generale delle letture da eseguire e ottimizzano le letture necessarie per implementare il metodo di accesso. Il thread che esegue il batch pianifica le operazioni di lettura.

## <a name="read-ahead"></a>Read-Ahead
[!INCLUDE[ssDE](../includes/ssde-md.md)] supporta un meccanismo di ottimizzazione delle prestazioni detto read-ahead. Questo meccanismo anticipa le pagine di dati e di indice necessarie per soddisfare il piano di esecuzione della query e inserisce le pagine nella cache buffer prima che vengano effettivamente utilizzate dalla query. In questo modo viene consentita la sovrapposizione di operazioni di calcolo e di I/O ed è possibile utilizzare al meglio sia la CPU che il disco. 

Il meccanismo read-ahead consente la lettura da parte di [!INCLUDE[ssDE](../includes/ssde-md.md)] di un massimo di 64 pagine contigue (512 KB) incluse in un unico file. La lettura viene eseguita come singola lettura non sequenziale nel numero appropriato di buffer, probabilmente non contigui, nella cache buffer. Se alcune pagine incluse nell'intervallo sono già presenti nella cache buffer, la pagina corrispondente della lettura verrà scartata dopo il completamento dell'operazione di lettura. Le pagine alle due estremità dell'intervallo possono inoltre venire rimosse se nella cache sono già presenti le pagine corrispondenti.

Vi sono due tipi di meccanismi read-ahead, uno per le pagine di dati e uno per le pagine di indice.

### <a name="reading-data-pages"></a>Lettura delle pagine di dati
Le analisi di tabella utilizzate per leggere le pagine di dati sono molto efficaci in [!INCLUDE[ssDE](../includes/ssde-md.md)]. Nelle pagine IAM (Index Allocation Map, mappa di allocazione degli indici) di un database di SQL Server vengono elencati gli extent usati da una tabella o da un indice. Il motore di archiviazione può leggere la pagina IAM per compilare un elenco ordinato degli indirizzi di disco che è necessario leggere. Questo consente al motore di archiviazione di ottimizzare le operazioni di I/O come se si trattasse di letture sequenziali di grandi dimensioni eseguite in sequenza, in base alla relativa posizione nel disco. Per altre informazioni sulle pagine IAM, vedere [Gestione dello spazio utilizzato dagli oggetti](../relational-databases/pages-and-extents-architecture-guide.md).

### <a name="reading-index-pages"></a>Lettura delle pagine di indice
Il motore di archiviazione legge le pagine di indice in modo seriale, in base all'ordine delle chiavi. Nella figura seguente è ad esempio illustrata una rappresentazione semplificata di un set di pagine foglia contenente un set di chiavi e il nodo di indice intermedio che esegue il mapping delle pagine foglia. Per altre informazioni sulla struttura delle pagine in un indice, vedere [Strutture degli indici cluster](../relational-databases/pages-and-extents-architecture-guide.md).

![Reading_Pages](../relational-databases/media/reading-pages.gif)

Il motore di archiviazione utilizza le informazioni della pagina di indice intermedio sopra il livello foglia per pianificare letture seriali per le pagine che contengono le chiavi. Se vengono richieste tutte le chiavi da ABC a DEF, il motore di archiviazione legge innanzitutto la pagina di indice sopra la pagina foglia, non limitandosi, tuttavia, alla semplice lettura in sequenza di ogni pagina di dati, dalla 504 alla 556, ovvero l'ultima che include chiavi comprese nell'intervallo specificato. Al contrario, il motore di archiviazione esegue l'analisi della pagina di indice intermedio, compila un elenco delle pagine foglia da leggere e pianifica quindi tutte le letture in base all'ordine delle chiavi. Il motore di archiviazione riconosce inoltre che le pagine 504/505 e 527/528 sono contigue ed esegue una singola lettura sequenziale per recuperare le pagine adiacenti in una sola operazione. Se in un'operazione seriale è necessario recuperare molte pagine, il motore di archiviazione pianifica un blocco di letture per volta. Quando viene completato un subset di letture, il motore di archiviazione pianifica un numero equivalente di nuove letture fino a quando non sono state pianificate tutte le letture necessarie.

Il motore di archiviazione utilizza la prelettura per rendere più veloci le ricerche nelle tabelle di base da indici non cluster. Le righe foglia di un indice non cluster contengono puntatori alle righe di dati che includono i singoli valori di chiave specifici. Mentre il motore di archiviazione legge le pagine foglia dell'indice non cluster, inizia anche a pianificare le letture asincrone per le righe di dati i cui puntatori sono già stati recuperati. In questo modo, il motore di archiviazione può recuperare le righe di dati dalla tabella sottostante prima di avere completato l'analisi dell'indice non cluster. La prelettura viene utilizzata indipendentemente dal fatto che alla tabella sia associato o meno un indice cluster. In SQL Server Enterprise Edition la prelettura viene usata con maggiore frequenza rispetto ad altre edizioni di SQL Server, di conseguenza il read-ahead viene effettuato su un numero maggiore di pagine. In nessuna edizione è possibile configurare il livello di prelettura. Per altre informazioni sugli indici non cluster, vedere [Strutture degli indici non cluster](../relational-databases/pages-and-extents-architecture-guide.md).

## <a name="advanced-scanning"></a>Analisi avanzata
In SQL Server Enterprise Edition la caratteristica di analisi avanzata consente la condivisione di analisi complete di tabella tra più attività. Se in base al piano di esecuzione di un'istruzione Transact-SQL è necessaria l'analisi delle pagine di dati di una tabella e il [!INCLUDE[ssDE](../includes/ssde-md.md)] rileva che tale tabella è già sottoposta ad analisi da un altro piano di esecuzione, il [!INCLUDE[ssDE](../includes/ssde-md.md)] unisce la seconda analisi alla prima nella posizione corrente della seconda analisi. [!INCLUDE[ssDE](../includes/ssde-md.md)] legge ogni pagina una sola volta e passa le righe di ognuna a entrambi i piani di esecuzione. Questa procedura continua fino a quando viene raggiunta la fine della tabella. 

A questo punto, il primo piano di esecuzione dispone dei risultati completi dell'analisi, mentre il secondo piano di esecuzione deve ancora recuperare le pagine di dati lette prima dell'unione all'analisi in corso. L'analisi del secondo piano di esecuzione riprende quindi dalla prima pagina di dati della tabella e termina in corrispondenza della posizione a partire dalla quale si è unita alla prima analisi. È possibile combinare in questo modo un numero qualsiasi di analisi. [!INCLUDE[ssDE](../includes/ssde-md.md)] continua a esaminare in ciclo tutte le pagine di dati fino al completamento di tutte le analisi. Questo meccanismo è anche detto "analisi ciclica" ed è il motivo per cui non è possibile garantire l'ordine dei risultati restituiti da un'istruzione SELECT senza una clausola ORDER BY. 

Si supponga, ad esempio, che sia disponibile una tabella con 500.000 pagine. L'Utente A esegue un'istruzione Transact-SQL che richiede un'analisi della tabella. Quando tale analisi ha già elaborato 100.000 pagine, l'Utente B esegue un'altra istruzione Transact-SQL che esegue l'analisi della stessa tabella. [!INCLUDE[ssDE](../includes/ssde-md.md)] pianifica un set di richieste di lettura per le pagine successive alla 100.001 e passa le righe di ogni pagina a entrambe le analisi. Quando l'analisi raggiunge la pagina 200.000, l'Utente C esegue un'altra istruzione Transact-SQL che esegue l'analisi della stessa tabella. A partire dalla pagina 200.001, [!INCLUDE[ssDE](../includes/ssde-md.md)] passa a tutte e tre le analisi le righe di ogni pagina che legge. Al termine della lettura della riga 500.000, l'analisi per l'Utente A è completata e quelle per l'Utente B e l'Utente C  ricominciano la lettura a partire dalla pagina 1. Quando [!INCLUDE[ssDE](../includes/ssde-md.md)] raggiunge la pagina 100.000, l'analisi per l'Utente B è completata. L'analisi per l'UtenteC continua quindi finché non legge la pagina 200.000. A questo punto tutte le analisi risultano completate. 

Senza l'analisi avanzata, gli utenti dovrebbero condividere lo spazio nel buffer, contendendosi il disco. Le stesse pagine verrebbero inoltre lette una volta per ogni utente, anziché essere lette una sola volta e condivise tra più utenti, provocando un peggioramento delle prestazioni e un sovraccarico delle risorse.

## <a name="see-also"></a>Vedere anche
[Guida sull'architettura di pagina ed extent](../relational-databases/pages-and-extents-architecture-guide.md)   
 [Scrittura di pagine](../relational-databases/writing-pages.md)
