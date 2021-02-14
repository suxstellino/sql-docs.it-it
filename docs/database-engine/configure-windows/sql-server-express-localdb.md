---
title: Local DB di SQL Server Express | Microsoft Docs
description: Acquisire familiarità con SQL Server Express LocalDB. Gli sviluppatori possono usare questo motore di database leggero per scrivere e testare codice Transact-SQL.
ms.custom: ''
ms.date: 04/17/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- user instances
- LocalDB, described
- local database runtime
- file database
- LocalDB
ms.assetid: 5a641a46-7cfb-4d7b-a90d-6e4625719d74
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9b7f57127bd8d1bd6b289ba82291ba9c1dd87424
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340431"
---
# <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Local DB di Microsoft SQL Server Express è una funzionalità di [SQL Server Express](../../sql-server/editions-and-components-of-sql-server-version-15.md) destinata agli sviluppatori. È disponibile in SQL Server Express with Advanced Services.

Con l'installazione di Local DB viene copiato un set di file minimo necessario per avviare il [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Una volta installato LocalDB, è possibile avviare una connessione tramite una stringa di connessione speciale. Durante la connessione, l'infrastruttura [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] necessaria viene creata automaticamente e avviata, consentendo all'applicazione di usare il database senza attività di configurazione complesse. Con Developer Tools gli sviluppatori dispongono di un [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] che consente di scrivere e verificare il codice [!INCLUDE[tsql](../../includes/tsql-md.md)] senza dover gestire un'istanza del server completa di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

## <a name="installation-media"></a>Supporto di installazione 

Local DB è una funzionalità selezionabile durante l'installazione di SQL Server Express ed è disponibile quando si scarica il supporto. Se si scarica il supporto, scegliere **Express Advanced** o il pacchetto Local DB. 

- [SQL Server Express 2019](https://go.microsoft.com/fwlink/?LinkID=866658)
- [SQL Server Express 2017](https://go.microsoft.com/fwlink/?LinkID=853017)
- [SQL Server Express 2016](https://go.microsoft.com/fwlink/?LinkID=799012)

In alternativa è possibile installare Local DB tramite il [Programma di installazione di Visual Studio](https://visualstudio.microsoft.com/downloads/) oppure come parte del carico di lavoro **Elaborazione ed archiviazione dati**, del carico di lavoro **Sviluppo ASP.NET e Web** o come componente singolo.


## <a name="install-localdb"></a>Installare LocalDB

Installare Local DB tramite l'installazione guidata o usando il programma SqlLocal DB.msi. Local DB è un'opzione per l'installazione di SQL Server Express Local DB. 
 
Selezionare Local DB nella pagina **Selezione funzionalità/Funzionalità condivise** durante l'installazione. È possibile un'unica installazione dei file binari di Local DB per ogni versione principale del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. È possibile avviare più processi del [!INCLUDE[ssDE](../../includes/ssde-md.md)] e in tutti verranno usati gli stessi file binari. Un'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] avviata come Local DB presenta le stesse limitazioni di [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)].

Un'istanza di [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] Local DB viene gestita con l'utilità `SqlLocalDB.exe`. Local DB di [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] deve essere usato al posto della funzionalità dell'istanza utente [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] deprecata.

## <a name="description"></a>Descrizione

Il programma di installazione di Local DB usa il programma `SqlLocalDB.msi` per installare i file necessari nel computer. Una volta installato, Local DB è un'istanza di [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] con cui è possibile creare e aprire database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. I file del database di sistema per il database sono archiviati nel percorso locale AppData che generalmente è nascosto. Ad esempio: `C:\Users\<user>\AppData\Local\Microsoft\Microsoft SQL Server Local DB\Instances\LocalDBApp1\`. I file del database utente sono archiviati nel percorso indicato dall'utente, in genere nella cartella `C:\Users\<user>\Documents\`.

Per altre informazioni sull'inclusione di Local DB in un'applicazione, vedere [Cenni preliminari sui dati locali](/previous-versions/visualstudio/visual-studio-2012/ms233817(v=vs.110)) per [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] e [Creare un database e aggiungere tabelle in Visual Studio](/visualstudio/data-tools/create-a-sql-database-by-using-a-designer).

Per altre informazioni sull'API Local DB, vedere [Riferimento al database locale di SQL Server Express](../../relational-databases/sql-server-express-localdb-reference.md).

Con l'utilità `SqlLocalDb` è possibile creare nuove istanze di Local DB, avviare e arrestare un'istanza di Local DB, nonché includere opzioni per consentire la gestione di Local DB. Per altre informazioni sull'utilità `SqlLocalDb`, vedere [Utilità SqlLocalDB](../../tools/sqllocaldb-utility.md).

Le regole di confronto dell'istanza per Local DB sono impostate su `SQL_Latin1_General_CP1_CI_AS` e non possono essere modificate. Le regole di confronto a livello di database, di colonna e di espressione sono supportate normalmente. Ai database indipendenti vengono applicate le regole di confronto dei metadati e di `tempdb` definite da [Regole di confronto dei database indipendenti](../../relational-databases/databases/contained-database-collations.md).

### <a name="restrictions"></a>Restrizioni

- Non è possibile applicare patch a Local DB oltre i Service Pack. Gli aggiornamenti cumulativi e gli aggiornamenti della sicurezza non possono essere applicati manualmente e non verranno applicati tramite Windows Update, Windows Update per le aziende o altri metodi.

- Local DB non può essere gestito in modalità remota tramite SQL Management Studio.

- Local DB non può essere un Sottoscrittore della replica di tipo merge.

- Local DB non supporta FILESTREAM.

- Local DB consente solo code locali per Service Broker.

- Un'istanza di Local DB di proprietà degli account predefiniti, ad esempio `NT AUTHORITY\SYSTEM`, può avere problemi di gestibilità a causa del reindirizzamento del file system di Windows. Usare invece un account di Windows normale come proprietario.

### <a name="automatic-and-named-instances"></a>Istanze denominate e automatiche

Local DB supporta due tipi di istanze, ovvero istanze automatiche e istanze denominate.

- Le istanze automatiche di Local DB sono pubbliche. Vengono create e gestite automaticamente per l'utente e possono essere usate da qualsiasi applicazione. Un'istanza automatica di Local DB esiste per ogni versione di Local DB installata nel computer dell'utente. Le istanze automatiche di Local DB forniscono una gestione continua dell'istanza. Non c'è necessità di creare l'istanza in quanto già funziona. Questa funzionalità consente di eseguire facilmente la migrazione e l'installazione dell'applicazione in un computer diverso. Se nel computer di destinazione è installata la versione specificata di Local DB, l'istanza automatica di Local DB per quella versione è disponibile anche nel computer di destinazione. Le istanze automatiche di Local DB hanno di un modello speciale per il nome dell'istanza che appartiene a uno spazio dei nomi riservato. In tal modo si evitano conflitti di nomi con le istanze denominate di Local DB. Il nome per l'istanza automatica è **MSSQLLocalDB**.

- Le istanze denominate di Local DB sono private. Sono di proprietà di una sola applicazione, responsabile della creazione e della gestione dell'istanza. Le istanze denominate forniscono l'isolamento dalle altre istanze e possono migliorare le prestazioni riducendo la contesa di risorse con altri utenti del database. Le istanze denominate devono essere create in modo esplicito dall'utente tramite l'API di gestione di Local DB o in modo implicito tramite il file app.config per un'applicazione gestita, sebbene anche l'applicazione gestita possa usare l'API. Ogni istanza denominata di Local DB dispone di una versione associata di Local DB che punta al relativo set di file binari di Local DB. Il nome di un'istanza di Local DB è composto da un massimo di 128 caratteri con tipo di dati **sysname**. I nomi delle istanze denominate normali di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] invece possono essere solo nomi NetBIOS normali di 16 caratteri ASCII. Il nome di un'istanza di Local DB può contenere qualsiasi carattere Unicode valido all'interno di un nome di file. Un'istanza denominata che usa un nome dell'istanza automatica diventa un'istanza automatica.

I diversi utenti di un computer possono avere istanze con lo stesso nome. Ogni istanza è un processo diverso in esecuzione come utente diverso.

## <a name="shared-instances-of-localdb"></a>Istanze condivise di Local DB

Per supportare scenari dove più utenti del computer devono connettersi a una sola istanza di Local DB, Local DB supporta la condivisione dell'istanza. Il proprietario di un'istanza può decidere di consentire agli altri utenti del computer di connettersi all'istanza. È possibile condividere le istanze automatiche e denominate di Local DB. Per condividere un'istanza di Local DB, un utente ne seleziona un nome condiviso (alias). Poiché il nome condiviso è visibile a tutti gli utenti del computer, questo nome condiviso deve essere univoco nel computer. Il nome condiviso di un'istanza di Local DB ha lo stesso formato dell'istanza denominata di Local DB.

Solo un amministratore del computer può creare un'istanza condivisa di Local DB. La condivisione di un'istanza di Local DB può essere annullata da un amministratore o dal proprietario dell'istanza condivisa di Local DB. Per condividere e annullare la condivisione di un'istanza di Local DB, usare i metodi `LocalDBShareInstance` e `LocalDBUnShareInstance` dell'API Local DB o le opzioni di condivisione e annullamento della condivisione dell'utilità `SqlLocalDb`.

## <a name="start-localdb-and-connect-to-localdb"></a>Avviare Local DB e connettersi a Local DB

### <a name="connect-to-the-automatic-instance"></a>Connettersi all'istanza automatica

Il modo più semplice per usare Local DB è connettersi all'istanza automatica di proprietà dell'utente corrente tramite la stringa di connessione `Server=(localdb)\MSSQLLocalDB;Integrated Security=true`. Per connettersi a un database specifico usando il nome del file, usare una stringa di connessione simile a `Server=(LocalDB)\MSSQLLocalDB;Integrated Security=true;AttachDbFileName=D:\Data\MyDB1.mdf`.

La convenzione di denominazione e la stringa di connessione per il formato Local DB sono state modificate in SQL Server 2014. In precedenza il nome dell'istanza era un carattere v singolo seguito da Local DB e dal numero di versione. A partire da SQL Server 2014 questo formato di nome di istanza non è più supportato, ed è invece necessario usare la stringa di connessione indicata in precedenza.  

>[!NOTE]
> - La prima volta che un utente prova a connettersi a Local DB in un computer, l'istanza automatica deve essere creata e avviata. Il tempo aggiuntivo necessario per la creazione dell'istanza può determinare un errore di tentativo di connessione generando un messaggio di timeout. In una situazione simile, attendere pochi secondi per consentire il completamento del processo di creazione, quindi riconnettersi.


### <a name="create-and-connect-to-a-named-instance"></a>Creare un'istanza denominata e connettersi

Oltre all'istanza automatica, Local DB supporta anche istanze denominate. Usare il programma SqlLocalDB.exe per creare, avviare e arrestare un'istanza denominata di Local DB. Per altre informazioni su SqlLocalDB.exe, vedere [Utilità SqlLocalDB](../../tools/sqllocaldb-utility.md).

```console
REM Create an instance of LocalDB
"C:\Program Files\Microsoft SQL Server\130\Tools\Binn\SqlLocalDB.exe" create LocalDBApp1
REM Start the instance of LocalDB
"C:\Program Files\Microsoft SQL Server\130\Tools\Binn\SqlLocalDB.exe" start LocalDBApp1
REM Gather information about the instance of LocalDB
"C:\Program Files\Microsoft SQL Server\130\Tools\Binn\SqlLocalDB.exe" info LocalDBApp1
```

 Nell'ultima riga sopra indicata vengono restituite informazioni simili alle seguenti.

|Category|Valore|
|-|-|
|Nome|`LocalDBApp1`|
|Versione|\<Current Version>|
|Nome condiviso|""|
|Proprietario|"\<Your Windows User>"|
|Creazione automatica|No|
|State|in esecuzione|
|Ultima ora inizio|\<Date and Time>|
|Nome pipe dell'istanza|np:\\\\.\pipe\LOCALDB#F365A78E\tsql\query|

>[!NOTE]
>Se nell'applicazione viene usata una versione di .NET precedente alla versione 4.0.2 è necessario connettersi direttamente alla named pipe di Local DB. Il valore Nome pipe dell'istanza è la named pipe su cui l'istanza di Local DB è in ascolto. La parte del nome pipe dell'istanza che segue LOCALDB# verrà modificata a ogni avvio dell'istanza di Local DB. Per connettersi all'istanza di Local DB tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], immettere il nome pipe dell'istanza nella casella **Nome server** della finestra di dialogo **Connetti al [!INCLUDE[ssDE](../../includes/ssde-md.md)]** . Dal programma personalizzato è possibile stabilire una connessione all'istanza di Local DB usando una stringa di connessione simile a `SqlConnection conn = new SqlConnection(@"Server=np:\\.\pipe\LOCALDB#F365A78E\tsql\query");`

### <a name="connect-to-a-shared-instance-of-localdb"></a>Connettersi a un'istanza condivisa di Local DB

Per connettersi a un'istanza condivisa di Local DB aggiungere `\.\` (barra rovesciata + punto + barra rovesciata) alla stringa di connessione per fare riferimento allo spazio dei nomi riservato per le istanze condivise. Ad esempio, per connettersi a un'istanza condivisa di Local DB denominata `AppData` usare `(localdb)\.\AppData` come stringa di connessione. Un utente che si connette a un'istanza condivisa di Local DB di cui non è proprietario, deve disporre di un account di accesso con autenticazione di Windows o autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="troubleshooting"></a>Risoluzione dei problemi

Per informazioni sulla risoluzione dei problemi di Local DB, vedere [Troubleshooting SQL Server 2012 Express LocalDB](https://social.technet.microsoft.com/wiki/contents/articles/4609.aspx) (Risoluzione dei problemi di SQL Server 2012 Express Local DB).

## <a name="permissions"></a>Autorizzazioni

Un'istanza di Local DB di SQL Server Express è un'istanza creata da un utente per l'uso personale. Qualsiasi utente del computer è in grado di creare un database mediante un'istanza di Local DB, archiviando i file nel profilo utente ed eseguendo il processo con le relative credenziali. Per impostazione predefinita, l'accesso all'istanza di Local DB è limitato al proprietario. I dati contenuti in Local DB sono protetti dall'accesso ai file di database tramite il file system. Se i file di database dell'utente sono archiviati in un percorso condiviso, il database può essere aperto da qualsiasi utente con accesso al file system in quel percorso usando un'istanza di Local DB di cui è proprietario. Se i file di database si trovano in un percorso protetto, ad esempio la cartella dati utente, solo quell'utente e gli amministratori con accesso a quella cartella, possono aprire il database. I file di Local DB possono essere aperti solo da un'istanza di Local DB alla volta.

>[!NOTE]
>Local DB viene eseguito sempre nel contesto di sicurezza dell'utente. Questo significa che Local DB non viene mai eseguito con credenziali del gruppo di amministratori locali. Di conseguenza, tutti i file di database usati da un'istanza di Local DB devono essere accessibili usando l'account di Windows dell'utente proprietario, senza considerare l'appartenenza al gruppo di amministratori locali.

## <a name="see-also"></a>Vedere anche

[Utilità SqlLocalDB](../../tools/sqllocaldb-utility.md)
