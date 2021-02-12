---
title: Architettura di estendibilità nelle estensioni di linguaggio di SQL Server
titleSuffix: ''
description: Informazioni sull'architettura di estendibilità usata per le estensioni del linguaggio di SQL Server, che consente di eseguire codice esterno in SQL Server. In SQL Server 2019 sono supportati Java, Python e R. Il codice viene eseguito in un ambiente di runtime del linguaggio come estensione del motore di database principale.
author: dphansen
ms.author: davidph
ms.date: 11/05/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: f7d4c2e2408bbcc78c0ea74f0d50bd4e0599b826
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069782"
---
# <a name="extensibility-architecture-in-sql-server-language-extensions"></a>Architettura di estendibilità nelle estensioni di linguaggio di SQL Server

[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Informazioni sull'architettura di estendibilità usata per le estensioni del linguaggio di SQL Server, che consente di eseguire codice esterno in SQL Server. In SQL Server 2019 sono supportati Java, Python e R. Il codice viene eseguito in un ambiente di runtime del linguaggio come estensione del motore di database principale.

## <a name="background"></a>Background

Lo scopo del framework di estendibilità è fornire un'interfaccia tra SQL Server e i linguaggi esterni. Eseguendo un linguaggio attendibile all'interno di un framework protetto gestito da SQL Server, gli amministratori di database possono mantenere la sicurezza consentendo ai data scientist di accedere ai dati aziendali.

<!-- We need to get a diagram like the one below.
The following diagram visually describes opportunities and benefits of the extensible architecture.

  ![Goals of integration with SQL Server](../media/ml-service-value-add.png "Machine Learning Services Value Add")
-->

Qualsiasi linguaggio esterno supportato può essere eseguito chiamando una stored procedure e i risultati vengono restituiti sotto forma tabulare direttamente a SQL Server. Ciò semplifica l'uso del linguaggio esterno da qualsiasi applicazione in grado di inviare una query SQL e gestire i risultati.

## <a name="architecture-diagrams"></a>Diagrammi dell'architettura

L'architettura è progettata in modo tale che il codice esterno viene eseguito in un processo separato da SQL Server, ma con componenti che gestiscono internamente la catena di richieste di dati e operazioni in SQL Server. 
  
  ***Architettura dei componenti in Windows:***

  ![Architettura dei componenti in Windows](../media/generic-architecture-windows.png "Architettura dei componenti in Windows")
  
  ***Architettura dei componenti in Linux:***
  
  ![Architettura dei componenti in Linux](../media/generic-architecture-linux.png "Architettura dei componenti in Linux")
  
I componenti includono un servizio **Launchpad** usato per richiamare i runtime esterni (ad esempio Java) e la logica specifica della libreria per il caricamento di interpreti e librerie.

<a name="launchpad"></a>

## <a name="launchpad"></a>Launchpad

[!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] è un servizio che gestisce la durata, le risorse e i limiti di sicurezza del processo esterno responsabile dell'esecuzione dello script. Il funzionamento è analogo al modo in cui il servizio di indicizzazione e query full-text avvia un host separato per l'elaborazione delle query full-text. Il servizio Launchpad può avviare solo utilità di avvio attendibili pubblicate da Microsoft o che Microsoft ha certificato come conformi ai requisiti di prestazioni e gestione delle risorse.

Il servizio [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] viene eseguito in **SQLRUserGroup**, che usa un ambiente [AppContainer](/windows/desktop/secauthz/appcontainer-isolation) per l'isolamento dell'esecuzione.

Viene creato un servizio [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] separato per ogni istanza del motore di database a cui sono state aggiunte estensioni di linguaggio macchina di SQL Server. È disponibile un servizio Launchpad per ogni istanza del motore di database, quindi se si hanno più istanze con supporto per gli script esterni, si avrà un servizio Launchpad per ciascuna di esse. Un'istanza del motore di database è associata al servizio Launchpad creato per tale istanza. Per tutte le chiamate di script esterni in una stored procedure o in T-SQL, il servizio SQL Server chiama il servizio Launchpad creato per la stessa istanza.

Per eseguire attività in un linguaggio supportato specifico, il servizio Launchpad ottiene un account di lavoro protetto dal pool e avvia un processo satellite per gestire il runtime esterno. Ogni processo satellite eredita l'account utente del servizio Launchpad e usa tale account di lavoro per la durata dell'esecuzione dello script. Se lo script usa processi paralleli, questi vengono creati con lo stesso account di lavoro singolo.

## <a name="communication-channels-between-components"></a>Canali di comunicazione tra componenti

In questa sezione vengono descritti i protocolli di comunicazione tra i componenti e le piattaforme dati.

+ **TCP/IP**

  Per impostazione predefinita, le comunicazioni interne tra SQL Server e SQL Satellite usano TCP/IP.

+ **ODBC**

  Le comunicazioni tra i client di data science esterni e un'istanza remota di SQL Server usano ODBC. L'account che invia i processi di script a SQL Server deve avere entrambe le autorizzazioni per connettersi all'istanza ed eseguire gli script esterni.

  Inoltre, a seconda dell'attività, per l'account potrebbero essere necessarie le autorizzazioni seguenti:

  + Lettura dei dati usati dal processo
  + Scrittura dei dati nelle tabelle: ad esempio, quando si salvano i risultati in una tabella
  + Creazione di oggetti di database: ad esempio, se si salva uno script esterno come parte di una nuova stored procedure.

  Quando si usa SQL Server come contesto di calcolo per uno script eseguito da un client remoto e l'eseguibile deve recuperare i dati da un'origine esterna, viene usato ODBC per il writeback. SQL Server mappa l'identità dell'utente che invia il comando remoto all'identità dell'utente dell'istanza corrente ed esegue il comando ODBC usando le credenziali di tale utente. La stringa di connessione necessaria per eseguire questa chiamata a ODBC viene ottenuta dal codice client.

+ **Altri protocolli**

  I processi che potrebbero dover funzionare in "blocchi" o ritrasferire i dati a un client remoto possono anche usare il [formato di file XDF](/machine-learning-server/r/concept-what-is-xdf). L'effettivo trasferimento dei dati avviene tramite BLOB codificati.

## <a name="next-steps"></a>Passaggi successivi

+ [Cosa sono le estensioni del linguaggio?](../language-extensions-overview.md)