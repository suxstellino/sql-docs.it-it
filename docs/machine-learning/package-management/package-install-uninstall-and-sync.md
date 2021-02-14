---
title: Sincronizzazione dei pacchetti R dal file system
description: Aggiornare le librerie R in SQL Server con le versioni più recenti installate nel file system.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 06/13/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2017
ms.openlocfilehash: 72f10f291041f44c0598d40b21ca0bfe379a42d4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073889"
---
# <a name="r-package-synchronization-for-sql-server"></a>Sincronizzazione dei pacchetti per SQL Server
[!INCLUDE [SQL Server 2017 only](../../includes/applies-to-version/sqlserver2017-only.md)]

La versione di RevoScaleR inclusa in SQL Server 2017 consente di sincronizzare le raccolte di pacchetti R tra i file system, l'istanza e il database in cui vengono usati i pacchetti.

Questa funzionalità è stata aggiunta per semplificare il backup delle raccolte di pacchetti R associati ai database di SQL Server. Con questa funzionalità, un amministratore può ripristinare non solo il database, ma tutti i pacchetti R usati dai data scientist che lavorano nel database.

Questo articolo descrive la funzionalità di sincronizzazione dei pacchetti e l'uso della funzione [rxSyncPackages](/machine-learning-server/r-reference/revoscaler/rxsyncpackages) per eseguire le attività seguenti:

+ Sincronizzare un elenco di pacchetti per un intero database di SQL Server

+ Sincronizzare i pacchetti usati da un singolo utente o da un gruppo di utenti

+ Se un utente passa a un'istanza diversa di SQL Server, è possibile eseguire un backup del database di lavoro dell'utente e ripristinarlo nel nuovo server. I pacchetti per l'utente vengono installati nel file system nel nuovo server, come richiesto da R.

È ad esempio possibile usare la sincronizzazione dei pacchetti negli scenari seguenti:

+ L'amministratore di database ha ripristinato un'istanza di SQL Server in un nuovo computer e chiede agli utenti di connettersi dai client R ed eseguire `rxSyncPackages` per aggiornare e ripristinare i pacchetti.

+ Si ritiene che un pacchetto R nel file system sia danneggiato, pertanto si esegue `rxSyncPackages` nell'istanza di SQL Server.

## <a name="requirements"></a>Requisiti

Prima di poter usare la sincronizzazione dei pacchetti, è necessario disporre della versione appropriata di Microsoft R o Machine Learning Server. Questa funzionalità è disponibile in Microsoft R versione 9.1.0 o versioni successive. 

È anche necessario abilitare la [funzionalità di gestione dei pacchetti](r-package-how-to-enable-or-disable.md) nel server.

### <a name="determine-whether-your-server-supports-package-management"></a>Determinare se il server supporta la gestione dei pacchetti

Questa funzionalità è disponibile in SQL Server 2017 CTP 2 o versioni successive.

È possibile aggiungere questa funzionalità a un'istanza di SQL Server 2016 aggiornando l'istanza affinché usi la versione più recente di Microsoft R. Per altre informazioni, vedere [Usare SqlBindR.exe per aggiornare SQL Server R Services](../install/upgrade-r-and-python.md).

### <a name="enable-the-package-management-feature"></a>Abilitare la funzionalità di gestione dei pacchetti

Per usare la sincronizzazione dei pacchetti, è necessario che la nuova funzionalità di gestione dei pacchetti sia abilitata nell'istanza di SQL Server e nei singoli database. Per altre informazioni, vedere [Abilitare o disabilitare la gestione dei pacchetti per SQL Server](r-package-how-to-enable-or-disable.md).

1. L'amministratore del server abilita la funzionalità per l'istanza di SQL Server.
2. Per ogni database, l'amministratore concede ai singoli utenti la possibilità di installare o condividere i pacchetti R usando i ruoli del database.

A questo punto è possibile usare le funzioni RevoScaleR, ad esempio [rxInstallPackages](/machine-learning-server/r-reference/revoscaler/rxinstallpackages), per installare i pacchetti in un database.  Le informazioni sugli utenti e sui pacchetti che è possibile usare vengono archiviate nell'istanza di SQL Server. 

Ogni volta che si aggiunge un nuovo pacchetto usando le funzioni di gestione dei pacchetti, vengono aggiornati sia i record in SQL Server sia il file system. Queste informazioni possono essere usate per ripristinare le informazioni sui pacchetti per l'intero database.

### <a name="permissions"></a>Autorizzazioni

+ La persona che esegue la funzione di sincronizzazione dei pacchetti deve essere un'entità di sicurezza nell'istanza di SQL Server e nel database che contiene i pacchetti.

+ Il chiamante della funzione deve essere un membro di uno di questi ruoli di gestione del pacchetto: **rpkgs-shared** o **rpkgs-private**.

+ Per sincronizzare i pacchetti contrassegnati come **condiviso**, è necessario che l'utente che esegue la funzione sia membro del ruolo **rpkgs-shared** e che i pacchetti spostati siano stati installati in una libreria con ambito condiviso.

+ Per sincronizzare i pacchetti contrassegnati come **privato**, il proprietario o l'amministratore del pacchetto deve eseguire la funzione e i pacchetti devono essere privati.

+ Per sincronizzare i pacchetti per conto di altri utenti, il proprietario deve essere un membro del ruolo del database **db_owner**.

## <a name="how-package-synchronization-works"></a>Funzionamento della sincronizzazione dei pacchetti

Per usare la sincronizzazione dei pacchetti, chiamare [rxSyncPackages](/r-server/r-reference/revoscaler/rxsyncpackages), una nuova funzione in [RevoScaleR](/machine-learning-server/r-reference/revoscaler/revoscaler). 

Per ogni chiamata a `rxSyncPackages`, specificare un'istanza di SQL Server e un database. Quindi, elencare i pacchetti da sincronizzare o specificare l'ambito dei pacchetti.

1. Creare il contesto di calcolo di SQL Server usando la funzione `RxInSqlServer`. Se non si specifica un contesto di calcolo, viene usato il contesto di calcolo corrente.

2. Specificare il nome di un database nell'istanza nel contesto di calcolo specificato. Vengono sincronizzati i pacchetti per ogni database.

3. Specificare i pacchetti da sincronizzare usando l'argomento dell'ambito.

    Se si usa l'ambito **privato**, vengono sincronizzati solo i pacchetti di proprietà del proprietario specificato. Se si specifica l'ambito **condiviso**, vengono sincronizzati tutti i pacchetti che non sono privati nel database. 
    
    Se si esegue la funzione senza specificare l'ambito **privato** o **condiviso**, vengono sincronizzati tutti i pacchetti.

4. Se il comando ha esito positivo, i pacchetti esistenti nel file system vengono aggiunti al database, con l'ambito e il proprietario specificati.

    Se il file system è danneggiato, i pacchetti vengono ripristinati in base all'elenco gestito nel database.

    Se la funzionalità di gestione dei pacchetti non è disponibile nel database di destinazione, viene generato un errore: "The package management feature is either not enabled on the SQL Server or version is too old" (La funzionalità di gestione dei pacchetti non è abilitata nell'istanza di SQL Server o la versione è obsoleta)

### <a name="example-1-synchronize-all-package-by-database"></a>Esempio 1. Sincronizzare tutti i pacchetti in base al database

Questo esempio recupera tutti i nuovi pacchetti dal file system locale e li installa nel database [TestDB]. Poiché non è specificato un proprietario, l'elenco include tutti i pacchetti che sono stati installati per ambiti privati e condivisi.

```R
connectionString <- "Driver=SQL Server;Server=myServer;Database=TestDB;Trusted_Connection=True;"
computeContext <- RxInSqlServer(connectionString = connectionString )
rxSyncPackages(computeContext=computeContext, verbose=TRUE)
```

### <a name="example-2-restrict-synchronized-packages-by-scope"></a>Esempio 2. Limitare i pacchetti sincronizzati in base all'ambito

Gli esempi seguenti sincronizzano solo i pacchetti nell'ambito specificato.

```R
#Shared scope
rxSyncPackages(computeContext=computeContext, scope="shared", verbose=TRUE)

#Private scope
rxSyncPackages(computeContext=computeContext, scope="private", verbose=TRUE)
```

### <a name="example-3-restrict-synchronized-packages-by-owner"></a>Esempio 3. Limitare i pacchetti sincronizzati in base al proprietario

L'esempio seguente illustra come sincronizzare solo i pacchetti installati per un utente specifico. In questo esempio l'utente è identificato dal nome di accesso SQL *user1*.

```R
rxSyncPackages(computeContext=computeContext, scope="private", owner = "user1", verbose=TRUE))
```

## <a name="related-resources"></a>Risorse correlate

[Gestione dei pacchetti R per SQL Server](install-additional-r-packages-on-sql-server.md)