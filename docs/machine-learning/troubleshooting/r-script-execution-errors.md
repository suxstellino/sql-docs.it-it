---
title: Errori comuni di scripting R
description: Questo articolo descrive diversi errori di scripting comuni che possono verificarsi quando si esegue codice R in SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 05/31/2018
ms.topic: troubleshooting
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 0f9f086ec09830e65f85ca2df5232e8b401cb9eb
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186415"
---
# <a name="common-r-scripting-errors-in-sql-server"></a>Errori comuni di scripting R in SQL Server
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Questo articolo descrive diversi errori di scripting comuni che possono verificarsi quando si esegue codice R in SQL Server. Non si tratta tuttavia di un elenco completo. Sono disponibili numerosi pacchetti e gli errori possono variare da una versione all'altra dello stesso pacchetto.

## <a name="valid-script-fails-in-t-sql-or-in-stored-procedures"></a>Uno script valido genera un errore in T-SQL o nelle stored procedure

Prima di eseguire il wrapping del codice R in una stored procedure, è consigliabile eseguire il codice R in un IDE esterno o in uno degli strumenti R, ad esempio RTerm o RGui. Usando questi metodi, è possibile testare ed eseguire il debug del codice usando i messaggi di errore dettagliati restituiti da R.

Tuttavia, a volte il codice che funziona perfettamente in un'utilità o in un IDE esterno potrebbe non essere eseguito correttamente in una stored procedure o in un contesto di calcolo di SQL Server. In tal caso, è necessario esaminare una vasta gamma di problemi prima di presupporre che il pacchetto non funzioni in SQL Server.

1. Controllare se Launchpad è in esecuzione.

2. Esaminare i messaggi per verificare se i dati di input o di output contengono colonne con tipi di dati incompatibili o non supportati. Ad esempio, le query in un database SQL spesso restituiscono GUID o RowGUID, entrambi non supportati. Per altre informazioni, vedere [Librerie R e tipi di dati](../r/r-libraries-and-data-types.md).

3. Esaminare le pagine della Guida delle singole funzioni R per determinare se tutti i parametri sono supportati per il contesto di calcolo di SQL Server. Per la Guida di ScaleR, usare i comandi della Guida di R inline oppure vedere le [informazioni di riferimento sul pacchetto](/r-server/r-reference/revoscaler/revoscaler).

Se il runtime di R è funzionante ma lo script restituisce errori, si consiglia di provare a eseguire il debug dello script in un ambiente di sviluppo R dedicato, come R Tools per Visual Studio.

Si consiglia inoltre di rivedere e riscrivere leggermente lo script per correggere eventuali problemi con i tipi di dati che possono verificarsi quando si spostano i dati tra R e il motore di database. Per altre informazioni, vedere [Librerie R e tipi di dati](../r/r-libraries-and-data-types.md).

Inoltre, è possibile usare il pacchetto sqlrutils per aggregare lo script R in un formato più facilmente utilizzabile come stored procedure. Per altre informazioni, vedere:
* [Pacchetto sqlrutils](../r/ref-r-sqlrutils.md)
* [Creare una stored procedure con sqlrutils](/machine-learning-server/r-reference/sqlrutils/how-to-create-a-stored-procedure-from-r)

## <a name="script-returns-inconsistent-results"></a>Lo script restituisce risultati incoerenti

Gli script R possono restituire valori diversi in un contesto di SQL Server, per vari motivi:

- La conversione implicita dei tipi viene eseguita automaticamente su alcuni tipi di dati, quando i dati vengono passati tra SQL Server e R. Per altre informazioni, vedere [Librerie R e tipi di dati](../r/r-libraries-and-data-types.md).

- Determinare se il numero di bit è un fattore. Ad esempio, ci sono spesso delle differenze nei risultati delle operazioni matematiche per le librerie a virgola mobile a 32 bit e a 64 bit.

- Determinare se sono stati prodotti valori diversi da numeri in un'operazione. In tal caso, i risultati potrebbero non essere validi.

- È possibile che differenze minime risultino amplificate quando si accetta un reciproco di un numero prossimo a zero.

- Gli errori di arrotondamento accumulati possono causare valori inferiori a zero anziché pari a zero.

## <a name="implied-authentication-for-remote-execution-via-odbc"></a>Autenticazione implicita per l'esecuzione remota tramite ODBC

Se ci si connette al computer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per eseguire comandi R tramite le funzioni **RevoScaleR**, è possibile che venga restituito un errore durante l'uso di chiamate ODBC che scrivono dati sul server. Questo errore si verifica solo quando si usa l'autenticazione di Windows.

Il motivo è che gli account di lavoro creati per R Services non dispongono dell'autorizzazione per la connessione al server. Non è pertanto possibile eseguire chiamate ODBC per conto dell'utente. Il problema non si verifica con gli account di accesso SQL perché, con gli account di accesso SQL, le credenziali vengono passate in modo esplicito dal client R all'istanza di SQL Server e quindi a ODBC. Tuttavia, l'uso di account di accesso SQL è anche meno sicuro rispetto all'uso dell'autenticazione di Windows.

Per consentire il passaggio sicuro delle credenziali di Windows da uno script avviato in modalità remota, SQL Server deve emulare tali credenziali. Questo processo viene definito di _autenticazione implicita_. Per eseguire questa operazione, gli account di lavoro che eseguono script R o Python nel computer SQL Server devono disporre delle corrette autorizzazioni.

1. Aprire [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] come amministratore dell'istanza in cui si vuole eseguire il codice R.

2. Eseguire lo script seguente. Assicurarsi di modificare il nome del gruppo utenti, se è stato cambiato quello predefinito, oltre che i nomi del computer e dell'istanza.

    ```sql
    USE [master]
    GO
    
    CREATE LOGIN [computername\\SQLRUserGroup] FROM WINDOWS WITH
    DEFAULT_DATABASE=[master], DEFAULT_LANGUAGE=[language]
    GO
    ```

## <a name="avoid-clearing-the-workspace-while-youre-running-r-in-a-sql-compute-context"></a>Evitare di cancellare i dati dell'area di lavoro durante l'esecuzione di R in un contesto di calcolo SQL

Anche se è un'operazione comune nella console R, la cancellazione dei dati dell'area di lavoro può avere conseguenze impreviste in un contesto di calcolo SQL.

`revoScriptConnection` è un oggetto dell'area di lavoro R che contiene informazioni su una sessione R chiamata da SQL Server. Se tuttavia il codice R include un comando per cancellare i dati dell'area di lavoro, ad esempio `rm(list=ls())`, anche tutte le informazioni relative alla sessione e altri oggetti presenti nell'area di lavoro R vengono cancellati.

Come soluzione alternativa, evitare di cancellare in modo indiscriminato variabili e altri oggetti durante l'esecuzione di R in SQL Server. È possibile eliminare variabili specifiche usando la funzione **remove**:

```R
remove('name1', 'name2', ...)
```

Se devono essere eliminate più variabili, si consiglia di salvare i nomi delle variabili temporanee in un elenco e quindi eseguire periodicamente operazioni di Garbage Collection sull'elenco.



## <a name="next-steps"></a>Passaggi successivi

[Risoluzione dei problemi e problemi noti di Machine Learning Services](machine-learning-troubleshooting-overview.md)

[Raccolta di dati per la risoluzione dei problemi di Machine Learning](data-collection-ml-troubleshooting-process.md)

[Installare Machine Learning Services per SQL Server](../install/sql-machine-learning-services-windows-install.md)

[Risolvere i problemi di connessione al motore di database](../../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md)