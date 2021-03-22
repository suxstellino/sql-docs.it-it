---
description: Informazioni su come gestire le dimensioni delle transazioni per assicurarsi di non introdurre blocchi nell'applicazione che bloccano altri utenti.
title: Gestione delle dimensioni delle transazioni
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 82900342-bc80-445f-98a4-468a303aae1e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2289e35116fc9ae39210db1b2af7dcb6e7dfad7a
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793039"
---
# <a name="managing-transaction-size"></a>Gestione delle dimensioni delle transazioni

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Quando si utilizzano le transazioni, è importante fare in modo che le transazioni siano più brevi possibile. La modalità predefinita di autocommit, che può essere abilitata o disabilitata tramite il metodo [setAutoCommit](reference/setautocommit-method-sqlserverconnection.md) , eseguirà il commit di ogni azione. Questa modalità è la modalità più semplice da usare per la maggior parte degli sviluppatori.

Quando si utilizzano transazioni manuali, assicurarsi che il codice esegua il commit della transazione il più rapidamente possibile. Se una transazione viene tenuta aperta, gli altri utenti non possono accedere ai dati. Ad esempio, un'ottima pratica di programmazione potrebbe consistere nell'inserire una chiamata di rollback nel blocco catch e in una chiamata di commit in un `finally` blocco. Questa procedura dipende tuttavia dalla progettazione dell'applicazione.

Per migliorare la concorrenza, è sufficiente ridurre le dimensioni delle transazioni. Se, ad esempio, si avvia una transazione manuale e si modificano 10.000 righe in una tabella di 20.000 righe, la tabella verrà bloccata da tutti gli altri utenti, anche se leggono solo i dati. Riducendo le modifiche a 2.000 righe, si lascia disponibile il 90% della tabella.

Inoltre, assicurarsi di usare l'impostazione del timeout di blocco se l'applicazione prevede problemi di blocco. È possibile impostare il timeout usando il metodo [setLockTimeout](reference/setlocktimeout-method-sqlserverdatasource.md) . Il valore predefinito per il timeout di blocco è-1, il che significa che il blocco verrà bloccato in modo indefinito durante l'attesa del blocco. È possibile impostare il timeout di blocco su 30 secondi, in modo da determinare il timeout della connessione bloccata in 30 secondi se bloccata da un'altra connessione.

## <a name="see-also"></a>Vedere anche

[Uso del driver JDBC per il miglioramento di prestazioni e affidabilità](improving-performance-and-reliability-with-the-jdbc-driver.md)
