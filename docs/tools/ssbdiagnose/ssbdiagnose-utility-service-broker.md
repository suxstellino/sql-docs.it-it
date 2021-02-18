---
title: Utilità ssbdiagnose (Service Broker)
description: L'utilità ssbdiagnose segnala la presenza di problemi nelle conversazioni di Service Broker o nella configurazione dei servizi di Service Broker.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- Service Broker, runtime reports
- Service Broker, command prompt utilities
- troubleshooting [Service Broker], conversations
- troubleshooting [Service Broker], configurations
- command prompt utilities [Service Broker]
- Service Broker, troubleshooting
- Service Broker, configuration reports
- Service Broker, tools
- troubleshooting [Service Broker], runtime
- conversations [Service Broker], troubleshooting
- troubleshooting [Service Broker], ssbdiagnose utility
- tools [Service Broker], ssbdiagnose
- Service Broker, ssbdiagnose utility
- ssbdiagnose
ms.assetid: 0c1636e8-a3db-438e-be4c-1ea40d1f4877
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: a5001821d5a396f65ee739341424a2eaae3dad78
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335899"
---
# <a name="ssbdiagnose-utility-service-broker"></a>Utilità ssbdiagnose (Service Broker)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

L'utilità **ssbdiagnose** segnala la presenza di problemi in conversazioni di [!INCLUDE[ssSB](../../includes/sssb-md.md)] o nella configurazione di servizi di [!INCLUDE[ssSB](../../includes/sssb-md.md)] . I controlli della configurazione possono essere eseguiti per due servizi oppure per un unico servizio. I problemi vengono segnalati nella finestra del prompt dei comandi in testo leggibile oppure in un file XML formattato che può essere reindirizzato a un file oppure a un altro programma.

## <a name="syntax"></a>Sintassi  
  
```  
  
ssbdiagnose   
[ [ -XML ]  
    [ -LEVEL { ERROR | WARNING | INFO } ]  
  [-IGNORE error_id ] [ ...n]  
    [ <baseconnectionoptions> ]  
  { <configurationreport> | <runtimereport> }  
]  
| -?  
  
<configurationreport> ::=  
    CONFIGURATION  
  { [ FROM SERVICE service_name  
      [ <fromconnectionoptions> ]  
      [ MIRROR <mirrorconnectionoptions> ]  
    ]  
    [ TO SERVICE service_name[, broker_id ]  
      [ <toconnectionoptions> ]  
      [ MIRROR <mirrorconnectionoptions> ]  
    ]  
  }  
    ON CONTRACT contract_name  
  [ ENCRYPTION { ON | OFF | ANONYMOUS } ]  
  
<runtime_report> ::=  
    RUNTIME  
    [-SHOWEVENTS ]  
        [ -NEW  
         [ -ID { conversation_handle  
                | conversation_group_id  
                 | conversation_id  
                  }  
        ] [ ...n]  
        ]  
    [ -TIMEOUT timeout_interval ]  
    [ <runtimeconnectionoptions> ]  
  
<baseconnectionoptions> ::=  
  <connectionoptions>  
  
<fromconnectionoptions> ::=  
  <connectionoptions>  
  
<toconnectionoptions> ::=  
  <connectionoptions>  
  
<mirrorconnectionoptions> ::=  
  <connectionoptions>  
  
<runtimeconnectionoptions> ::=  
  [ CONNECT TO <connectionoptions> ] [ ...n]  
  
<connectionoptions> ::=  
    [ -E | { -U login_id [ -P password ] } ]  
  [ -S server_name[\instance_name] ]  
  [ -d database_name ]  
  [ -l login_timeout ]  
  
```  
  
## <a name="command-line-options"></a>Opzioni da riga di comando  
 **-XML**  
 Specifica che l'output di **ssbdiagnose** deve essere generato come file XML formattato. che può essere reindirizzato a un file oppure a un'altra applicazione. Se l'opzione **-XML** non viene specificata, l'output di **ssbdiagnose** viene formattato come testo leggibile.  
  
 **-LEVEL** { **ERROR** | **WARNING** | **INFO**}  
 Specifica il livello dei messaggi da segnalare.  
  
 **ERROR**: segnala solo i messaggi di errore.  
  
 **WARNING**: segnala i messaggi di errore e di avviso.  
  
 **INFO**: segnala i messaggi di errore, di avviso e informativi.  
  
 L'impostazione predefinita è **WARNING**.  
  
 **-IGNORE** _error_id_  
 Specifica che i messaggi o gli errori con il valore *error_id* specificato non devono essere inclusi nei report. È possibile specificare **-IGNORE** più volte per eliminare più ID messaggio.  
  
 **\<baseconnectionoptions>**  
 Specifica le informazioni di connessione di base usate da **ssbdiagnose** quando le opzioni di connessione non sono incluse in una clausola specifica. Le informazioni di connessione indicate in una clausola specifica prevalgono sulle informazioni specificate in **baseconnectionoption** . Questa situazione viene gestita separatamente per ciascun parametro. Ad esempio, se vengono specificati **-S** e **-d** in **baseconnetionoptions** e solo **-d** è specificato in **toconnetionoptions**, **ssbdiagnose** userà -S di **baseconnetionoptions** e -d di **toconnetionoptions**.  
  
 **CONFIGURATION**  
 Richiede un report degli errori di configurazione tra una coppia di servizi di [!INCLUDE[ssSB](../../includes/sssb-md.md)] o per un singolo servizio.  
  
 **FROM SERVICE** _service_name_  
 Specifica il servizio che avvia le conversazioni.  
  
 **\<fromconnectionoptions>**  
 Specifica le informazioni necessarie per connettersi al database che contiene il servizio Initiator. Se **fromconnectionoptions** non viene specificato, **ssbdiagnose** usa le informazioni di connessione di **baseconnectionoptions** per la connessione al database del servizio Initiator. Se specificato, **fromconnectionoptions** deve includere il database che contiene il servizio Initiator. Se **fromconnectionoptions** non è specificato, **baseconnectionoptions** deve specificare il database del servizio Initiator.  
  
 **TO SERVICE** _service_name_[, *broker_id* ]  
 Specifica il servizio che rappresenta la destinazione delle conversazioni.  
  
 *service_name*: specifica il nome del servizio di destinazione.  
  
 *broker_id*: specifica l'ID di [!INCLUDE[ssSB](../../includes/sssb-md.md)] che identifica il database di destinazione. *broker_id* è un GUID. Per individuarlo, è possibile eseguire la query seguente sul database di destinazione:  
  
```  
SELECT service_broker_guid  
FROM sys.databases  
WHERE database_id = DB_ID();  
```  
  
 **\<toconnectionoptions>**  
 Specifica le informazioni necessarie per connettersi al database che contiene il servizio di destinazione. Se **toconnectionoptions** non viene specificato, **ssbdiagnose** usa le informazioni di connessione di **baseconnectionoptions** per la connessione al database di destinazione.  
  
 **MIRROR**  
 Specifica che il servizio [!INCLUDE[ssSB](../../includes/sssb-md.md)] associato è ospitato in un database con mirroring. **ssbdiagnose** verifica che la route per il servizio sia una route con mirroring, in cui MIRROR_ADDRESS è stato specificato in CREATE ROUTE.  
  
 **\<mirrorconnectionoptions>**  
 Specifica le informazioni necessarie per connettersi al database mirror. Se **mirrorconnectionoptions** non viene specificato, **ssbdiagnose** usa le informazioni di connessione di **baseconnectionoptions** per la connessione al database mirror.  
  
 **ON CONTRACT** _contract_name_  
 Richiede che **ssbdiagnose** controlli solo le configurazioni che usano il contratto specificato. Se ON CONTRACT non è specificato, **ssbdiagnose** offre informazioni solo sul contratto denominato DEFAULT.  
  
 **ENCRYPTION** { **ON** | **OFF** | **ANONYMOUS** }  
 Richiede di verificare che il dialogo sia configurato correttamente per il livello di crittografia specificato:  
  
 **ON**: impostazione predefinita. Viene configurata la sicurezza completa del dialogo. Questo significa che i certificati sono stati distribuiti in entrambi i lati del dialogo, che è presente un'associazione al servizio remoto e che nell'istruzione GRANT SEND per il servizio di destinazione è stato specificato l'utente che avvia il dialogo.  
  
 **OFF**: non viene configurata alcuna sicurezza del dialogo. Questo significa che non è stato distribuito alcun certificato, non è stata creata alcuna associazione al servizio remoto e in GRANT SEND per il servizio Initiator è stato specificato il ruolo **public** .  
  
 **ANONYMOUS**: viene configurata la sicurezza anonima del dialogo. Questo significa che è stato distribuito solo un certificato, che nell'associazione al servizio remoto è stata specificata la clausola anonima e che in GRANT SEND per il servizio di destinazione è stato specificato il ruolo **public** .  
  
 **RUNTIME**  
 Richiede che venga generato un report di problemi che provocano errori di run-time per una conversazione di [!INCLUDE[ssSB](../../includes/sssb-md.md)] . Se non viene specificata l'opzione **-NEW** o **-ID** , **ssbdiagnose** esegue il monitoraggio di tutte le conversazioni in tutti i database specificati nelle opzioni di connessione. Se viene specificata l'opzione **-NEW** o **-ID** , **ssbdiagnose** compila un elenco degli ID specificati nei parametri.  
  
 Durante l'esecuzione, **ssbdiagnose** registra tutti gli eventi di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] che indicano errori di run-time, ovvero gli eventi che si verificano per gli ID specificati più quelli a livello di sistema. Se vengono rilevati errori di run-time, **ssbdiagnose** esegue un report di configurazione sulla configurazione associata.  
  
 Per impostazione predefinita, nel report di output non vengono inclusi gli errori di run-time, ma solo i risultati dell'analisi di configurazione. Per includere gli errori di runtime nel report, usare **-SHOWEVENTS** .  
  
 **-SHOWEVENTS**  
 Specifica che **ssbdiagnose** deve includere eventi di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] durante la generazione di un report RUNTIME. Vengono segnalati solo gli eventi considerati come condizioni di errore. Per impostazione predefinita, **ssbdiagnose** esegue solo il monitoraggio degli eventi di errore, ma non li inserisce nell'output.  
  
 **-NEW**  
 Richiede il monitoraggio in fase di esecuzione della prima conversazione iniziata dopo l'avvio di **ssbdiagnose** .  
  
 **-ID**  
 Richiede il monitoraggio in fase di esecuzione degli elementi di conversazione specificati. È possibile specificare l'opzione **-ID** più volte.  
  
 Se si specifica un handle di conversazione, vengono segnalati solo gli eventi correlati all'endpoint di conversazione associato. Se si specifica un ID di conversazione, vengono segnalati tutti gli eventi relativi a tale conversazione e agli endpoint dell'Initiator e di destinazione della conversazione stessa, mentre se si specifica un ID di un gruppo di conversazioni, vengono segnalati tutti gli eventi relativi a tutti gli endpoint e a tutte le conversazioni del gruppo.  
  
 *conversation_handle*  
 Identificatore univoco di un endpoint di conversazione in un'applicazione. Per un endpoint di una conversazione gli handle di conversazione sono univoci , mentre per gli endpoint dell'Initiator e di destinazione gli handle di conversazione sono diversi.  
  
 Gli handle di conversazione vengono restituiti alle applicazioni dal parametro *\@dialog_handle* dell'istruzione **BEGIN DIALOG** e dalla colonna **conversation_handle** nel set di risultati di un'istruzione **RECEIVE**.  
  
 Gli handle di conversazione vengono indicati nella colonna **conversation_handle** delle viste del catalogo **sys.transmission_queue** e **sys.conversation_endpoints** .  
  
 *conversation_group_id*  
 Identificatore univoco di un gruppo di conversazioni.  
  
 Gli ID gruppo di conversazioni vengono restituiti nelle applicazioni dal parametro *\@conversation_group_id* dell'istruzione **GET CONVERSATION GROUP** e dalla colonna **conversation_group_id** nel set di risultati di un'istruzione **RECEIVE**.  
  
 Gli ID gruppo di conversazioni vengono indicati nelle colonne **conversation_group_id** delle viste del catalogo **sys.conversation_groups** e **sys.conversation_endpoints** .  
  
 *conversation_id*  
 Identificatore univoco di una conversazione. Gli ID di conversazione sono gli stessi per sia gli endpoint dell'Initiator che per quelli di destinazione di una conversazione.  
  
 Gli ID conversazione vengono indicati nella colonna **conversation_id** della vista del catalogo **sys.conversation_endpoints** .  
  
 **-TIMEOUT** _timeout_interval_  
 Specifica il numero di secondi per l'esecuzione un report **RUNTIME** . Se l'opzione **-TIMEOUT** non viene specificata, il report di runtime viene eseguito per un periodo di tempo illimitato. **- TIMEOUT** viene usata solo nei report **RUNTIME** e non nei report **CONFIGURATION** . Usare CTRL + C per uscire da **ssbdiagnose** se **-TIMEOUT** non è stata specificata oppure per terminare un report di runtime prima che scada l'intervallo di time **-** out. Il valore *timeout_interval* deve essere un numero compreso tra 1 e 2,147,483,647.  
  
 **\<runtimeconnectionoptions>**  
 Specifica le informazioni di connessione per i database che contengono i servizi associati agli elementi di conversazione monitorati. Se tutti i servizi si trovano nello stesso database, è necessario specificare solo una clausola **CONNECT TO** , mentre se i servizi si trovano in database separati è necessario specificare una clausola **CONNECT TO** per ogni database. Se **runtimeconnectionoptions** non viene specificato, **ssbdiagnose** usa le informazioni di connessione di **baseconnectionoptions**.  
  
 **-E**  
 Apre una connessione con autenticazione di Windows a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] usando l'account di Windows corrente come ID di accesso. Le credenziali di accesso devono corrispondere a un membro del ruolo predefinito del server **sysadmin** .  
  
 L'opzione -E consente di ignorare le impostazioni relative all'utente e alla password delle variabili di ambiente SQLCMDUSER e SQLCMDPASSWORD.  
  
 Se non viene specificata né **-E** né **-U** , **ssbdiagnose** usa il valore della variabile di ambiente SQLCMDUSER. Se SQLCMDUSER non è impostata, **ssbdiagnose** usa l'autenticazione di Windows.  
  
 Se si usa l'opzione **-E** in combinazione con l'opzione **-U** o con l'opzione **-P** , viene generato un messaggio di errore.  
  
 **-U** _login_id_  
 Apre una connessione con autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tramite l'ID di accesso specificato. Le credenziali di accesso devono corrispondere a un membro del ruolo predefinito del server **sysadmin** .  
  
 Se non viene specificata né **-E** né **-U** , **ssbdiagnose** usa il valore della variabile di ambiente SQLCMDUSER. Se SQLCMDUSER non è impostata, **ssbdiagnose** tenta di effettuare la connessione usando la modalità di autenticazione di Windows basata sull'account di Windows dell'utente che esegue **ssbdiagnose**.  
  
 Se si usa l'opzione **-U** in combinazione con l'opzione **-E** , viene generato un messaggio di errore. Se l'opzione **-U** è seguita da più di un argomento, viene generato un messaggio di errore e il programma viene chiuso.  
  
 **-P** _password_  
 Specifica la password per l'ID di accesso **-U** . Alle password viene applicata la distinzione tra maiuscole e minuscole. Se si usa l'opzione **-U** senza usare **-P** , **ssbdiagnose** usa il valore della variabile di ambiente SQLCMDPASSWORD. Se SQLCMDPASSWORD non è impostata, **ssbdiagnose** richiede l'immissione di una password.  
  
> [!IMPORTANT]  
>  Quando si digita un comando SET SQLCMDPASSWORD, la password sarà visibile a chiunque sia in grado di vedere il monitor.  
  
 Se l'opzione **-P** viene specificata senza che sia indicata una password, **ssbdiagnose** usa la password predefinita (NULL).  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)] Per altre informazioni, vedere [Password complesse](../../relational-databases/security/strong-passwords.md).  
  
 La richiesta della password viene visualizzata mediante la stampa nella console, come indicato di seguito: `Password:`  
  
 L'input dell'utente è nascosto. L'input non viene pertanto visualizzato e il cursore rimane in posizione.  
  
 Se si usa l'opzione **-P** in combinazione con l'opzione **-E** , viene generato un messaggio di errore.  
  
 Se l'opzione **-P** è seguita da più di un argomento, viene generato un messaggio di errore.  
  
 **-S** _server_name_[\\*instance_name*]  
 Specifica l'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] che contiene i servizi di [!INCLUDE[ssSB](../../includes/sssb-md.md)] da analizzare.  
  
 Specificare *server_name* per connettersi all'istanza predefinita del [!INCLUDE[ssDE](../../includes/ssde-md.md)] in tale server. Specificare _server\_name_ **\\** _instance\_name_ per connettersi a un'istanza denominata di [!INCLUDE[ssDE](../../includes/ssde-md.md)] su tale server. Se **-S** non viene specificata, **ssbdiagnose** usa il valore della variabile di ambiente SQLCMDSERVER. Se SQLCMDSERVER non è impostata, **ssbdiagnose** si connette all'istanza predefinita del [!INCLUDE[ssDE](../../includes/ssde-md.md)] sul computer locale.  
  
 **-d** _database_name_  
 Specifica il database che contiene i servizi di [!INCLUDE[ssSB](../../includes/sssb-md.md)] da analizzare. Se il database non esiste, viene generato un messaggio di errore. Se l'opzione **-d** non è specificata, per impostazione predefinita viene usato il database specificato nella proprietà default-database dell'account di accesso.  
  
 **-l** _login_timeout_  
 Specifica il numero di secondi prima del timeout di un tentativo di connessione. Se l'opzione **-l** non è specificata, **ssbdiagnose** usa il valore impostato per la variabile di ambiente SQLCMDLOGINTIMEOUT. Se SQLCMDLOGINTIMEOUT non è impostata, il timeout predefinito è di trenta secondi. Il valore del timeout deve essere un numero compreso tra 0 e 65.534. Se il valore specificato non è numerico o non è compreso nell'intervallo, **ssbdiagnose** genera un messaggio di errore. Il valore 0 specifica un timeout infinito.  
  
 **-?**  
 Visualizza la guida della riga di comando.  
  
## <a name="remarks"></a>Osservazioni  
 Usare **ssbdiagnose** per eseguire queste operazioni:  
  
-   Verificare che non siano presenti errori di configurazione in un'applicazione di [!INCLUDE[ssSB](../../includes/sssb-md.md)] appena configurata.  
  
-   Verificare che non siano presenti errori di configurazione in seguito alla modifica della configurazione di un'applicazione di [!INCLUDE[ssSB](../../includes/sssb-md.md)] esistente.  
  
-   Verificare che non siano presenti errori di configurazione in seguito allo scollegamento di un database di [!INCLUDE[ssSB](../../includes/sssb-md.md)] e al successivo collegamento a una nuova istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   Ricercare eventuali errori di configurazione quando i messaggi non vengono trasmessi correttamente tra servizi.  
  
-   Ottenere un report di qualsiasi errore che si verifica in un set di elementi di conversazione di [!INCLUDE[ssSB](../../includes/sssb-md.md)] .  
  
## <a name="configuration-reporting"></a>Report di configurazione  
 Per analizzare correttamente la configurazione usata da una conversazione, eseguire un report di configurazione di **ssbdiagnose** che usa le stesse opzioni della conversazione. Se per **ssbdiagnose** si specifica un livello di opzioni inferiore rispetto a quello usato dalla conversazione, **ssbdiagnose** potrebbe non segnalare le condizioni necessarie alla conversazione, mentre se si specifica un livello di opzioni superiore **ssbdiagnose** potrebbe segnalare elementi non richiesti dalla conversazione. Una conversazione tra due servizi nello stesso database, ad esempio, può essere eseguita utilizzando l'opzione ENCPRYPTION OFF. Se si esegue **ssbdiagnose** per convalidare la configurazione tra i due servizi, ma si usa l'impostazione ENCRYPTION ON predefinita, **ssbdiagnose** segnala che nel database manca una chiave master, che non è richiesta per la conversazione.  
  
 Il report di configurazione di **ssbdiagnose** analizza solo un servizio oppure una singola coppia di servizi di [!INCLUDE[ssSB](../../includes/sssb-md.md)] ogni volta che viene eseguito. Per eseguire report su più coppie di servizi di [!INCLUDE[ssSB](../../includes/sssb-md.md)] , compilare un file di comando con estensione cmd che chiama l'utilità **ssbdiagnose** .  
  
## <a name="runtime-reporting"></a>Report in fase di esecuzione  
 Quando viene specificata l'opzione -RUNTIME, **ssbdiagnose** cerca in tutti i database specificati in **runtimeconnectionoptions** e **baseconnectionoption** s per generare un elenco degli ID di [!INCLUDE[ssSB](../../includes/sssb-md.md)] . L'elenco completo di ID dipende dagli elementi specificati per le opzioni -NEW e -ID:  
  
-   Se non viene specificata né **-NEW** né **-ID** , nell'elenco vengono incluse tutte le conversazioni per tutti i database specificati nelle opzioni di connessione.  
  
-   Se l'opzione **-NEW** è specificata, **ssbdiagnose** include gli elementi per la prima conversazione avviata dopo l'esecuzione di **ssbdiagnose** , tra cui l'ID e gli handle di conversazione relativi sia agli endpoint di conversazione dell'Initiator e di destinazione.  
  
-   Se l'opzione **-ID** è specificata con un handle di conversazione, nell'elenco viene inserito solo tale handle.  
  
-   Se l'opzione **-ID** è specificata con un ID conversazione, all'elenco vengono aggiunti l'ID conversazione e gli handle per entrambi gli endpoint di conversazione.  
  
-   Se l'opzione **-ID** è specificata con un ID gruppo di conversazioni, all'elenco vengono aggiunti tutti gli ID conversazione e gli handle di conversazione di tale gruppo.  
  
 Nell'elenco non sono inclusi elementi da database non coperti dalle opzioni di connessione. Se ad esempio si usa l'opzione **-ID** per specificare un ID conversazione, ma si specifica solo una clausola **runtimeconnectionoptions** per il database del servizio Initiator e non il database di destinazione, **ssbdiagnose** non includerà l'handle di conversazione di destinazione nell'elenco degli ID, ma solo l'ID conversazione e l'handle di conversazione del servizio Initiator.  
  
 **ssbdiagnose** esegue il monitoraggio degli eventi di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] dei database coperti da **runtimeconnectionoptions** e **baseconnectionoptions**. Cerca gli eventi di [!INCLUDE[ssSB](../../includes/sssb-md.md)] che indicano la presenza di un errore in base a uno o più ID di [!INCLUDE[ssSB](../../includes/sssb-md.md)] nell'elenco di runtime. **ssbdiagnose** cerca anche gli eventi di errore di [!INCLUDE[ssSB](../../includes/sssb-md.md)] a livello di sistema non specificamente associati alcun gruppo di conversazioni.  
  
 Se **ssbdiagnose** rileva errori di conversazione, l'utilità tenterà di segnalare la causa radice degli eventi eseguendo anche un report di configurazione. **ssbdiagnose** usa i metadati presenti nei database per tentare di determinare le istanze, gli ID [!INCLUDE[ssSB](../../includes/sssb-md.md)] , i database, i servizi e i contratti usati dalla conversazione ed esegue quindi un report di configurazione utilizzando tutte le informazioni disponibili.  
  
 Per impostazione predefinita, **ssbdiagnose** non segnala eventi di errore, ma solo i problemi sottostanti rilevati durante il controllo della configurazione. In questo modo la quantità di informazioni segnalate viene ridotta ed è possibile concentrarsi sui problemi di configurazione sottostanti. Per visualizzare gli eventi di errore rilevati da **ssbdiagnose** , è possibile specificare **-SHOWEVENTS**.  
  
## <a name="issues-reported-by-ssbdiagnose"></a>Problemi segnalati di ssbdiagnose  
 **ssbdiagnose** segnala tre classi di problemi. Nel file di output XML ogni classe di problemi viene segnalata come un tipo separato dell'elemento Issue. Di seguito sono riportati i tre tipi di problemi segnalati da **ssbdiagnose** :  
  
 **Diagnosi**  
 Segnala un problema di configurazione, ovvero problemi rilevati durante l'esecuzione di un report **CONFIGURATION** o durante la fase di configurazione di un report **RUNTIME** . **ssbdiagnose** segnala ogni problema di configurazione solo una volta.  
  
 **Event**  
 Segnala un evento di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] che indica che si è verificato un problema in una conversazione monitorata durante un report **RUNTIME** . **ssbdiagnose** segnala gli eventi ogni volta che vengono generati. Se il problema viene rilevato in più conversazioni, gli eventi possono essere segnalati più volte.  
  
 **Problema**  
 Segnala un problema che impedisce a **ssbdiagnose** di completare un'analisi di configurazione o di monitorare le conversazioni.  
  
## <a name="sqlcmd-environment-variables"></a>Variabili di ambiente sqlcmd  
 L'utilità **ssbdiagnose** supporta le variabili di ambiente SQLCMDSERVER, SQLCMDUSER, SQLCMDPASSWORD e SQLCMDLOGINTIMOUT usate anche dall'utilità **sqlcmd** . Per impostare le variabili di ambiente, è possibile usare il comando SET del prompt dei comandi o il comando **setvar** negli script [!INCLUDE[tsql](../../includes/tsql-md.md)] eseguiti tramite **sqlcmd**. Per altre informazioni sull'uso di **setvar** in **sqlcmd**, vedere [Utilizzo di sqlcmd con variabili di scripting](../../ssms/scripting/sqlcmd-use-with-scripting-variables.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 In ogni clausola **connectionoptions** l'account di accesso specificato mediante **-E** o **-U** deve essere un membro del ruolo predefinito del server **sysadmin** nell'istanza specificata in **-S**.  
  
## <a name="examples"></a>Esempi  
 Questa sezione include esempi d'uso di **ssbdiagnose** a un prompt dei comandi.  
  
### <a name="a-checking-the-configuration-of-two-services-in-the-same-database"></a>R. Controllo della configurazione di due servizi nello stesso database  
 Nell'esempio seguente viene illustrato come richiedere un report di configurazione quando si verificano le seguenti condizioni:  
  
-   Il servizio Initiator e quello di destinazione si trovano nello stesso database.  
  
-   Il database si trova nell'istanza predefinita del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   L'istanza si trova nello stesso computer in cui viene eseguita **ssbdiagnose** .  
  
 L'utilità **ssbdiagnose** segnala la configurazione che usa il contratto DEFAULT poiché l'opzione ON CONTRACT non è specificata.  
  
```  
ssbdiagnose -E -d MyDatabase CONFIGURATION FROM SERVICE /test/initiator TO SERVICE /test/target  
```  
  
### <a name="b-checking-the-configuration-of-two-services-on-separate-computers-that-use-one-login"></a>B. Controllo della configurazione di due servizi in computer separati che utilizzano un unico account di accesso  
 Nell'esempio seguente viene illustrato come richiedere un report di configurazione quando il servizio Initiator e quello di destinazione si trovano in computer separati, ma l'accesso ai servizi può essere eseguito utilizzando lo stesso account con autenticazione di Windows.  
  
```  
ssbdiagnose -E CONFIGURATION FROM SERVICE /text/initiator -S InitiatorComputer -d InitiatorDatabase TO SERVICE /test/target -S TargetComputer -d TargetDatabase ON CONTRACT TestContract  
```  
  
### <a name="c-checking-the-configuration-of-two-services-on-separate-computers-that-use-separate-logins"></a>C. Controllo della configurazione di due servizi in computer separati che utilizzano account di accesso diversi  
 Nell'esempio seguente viene illustrato come richiedere un report di configurazione quando il servizio Initiator e quello di destinazione si trovano in computer separati ed è necessario utilizzare due account di accesso con autenticazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] diversi per ogni istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
```  
ssbdiagnose CONFIGURATION FROM SERVICE /text/initiator   
-S InitiatorComputer -U InitiatorLogin -p !wEx23Dvb   
-d InitiatorDatabase TO SERVICE /test/target -S TargetComputer   
-U TargetLogin -p ER!49jiy -d TargetDatabase ON CONTRACT TestContract  
```  
  
### <a name="d-checking-mirrored-service-configurations-on-separate-computers-with-anonymous-encryption"></a>D. Controllo delle configurazioni del servizio con mirroring in computer separati con crittografia anonima  
 Nell'esempio seguente viene illustrato come richiedere un report di configurazione quando il servizio Initiator e quello di destinazione si trovano in computer separati e viene eseguito il mirroring del servizio Initiator a un'istanza denominata. Il report verifica inoltre che i servizi siano configurati in modo da utilizzare la crittografia anonima.  
  
```  
ssbdiagnose -E CONFIGURATION FROM SERVICE /text/initiator   
-S InitiatorComputer -d InitiatorDatabase MIRROR   
-S MirrorComputer/MirrorInstance TO SERVICE /test/target   
-S TargetComputer -d TargetDatabase ON CONTRACT TestContract ENCRYPTION ANONYMOUS  
```  
  
### <a name="e-checking-the-configuration-of-two-contracts"></a>E. Controllo della configurazione di due contratti  
 Nell'esempio seguente viene illustrato come compilare un file di comando per richiedere un report di configurazione quando si verificano le seguenti condizioni:  
  
-   Il servizio Initiator e quello di destinazione si trovano nello stesso database.  
  
-   Il database si trova nell'istanza predefinita del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   L'istanza si trova nello stesso computer in cui viene eseguita **ssbdiagnose** .  
  
 Ogni volta che **ssbdiagnose** viene eseguita, segnala la configurazione per un contratto diverso tra gli stessi servizi.  
  
```  
ssbdiagnose -E -d MyDatabase CONFIGURATION FROM SERVICE   
/test/initiator TO SERVICE /test/target ON CONTRACT PayRaiseContract  
ssbdiagnose -E -d MyDatabase CONFIGURATION FROM SERVICE /test/initiator   
TO SERVICE /test/target ON CONTRACT PromotionContract  
```  
  
### <a name="f-monitor-the-status-of-a-specific-conversation-on-the-local-computer-with-a-time-out"></a>F. Monitoraggio dello stato di una conversazione specifica nel computer locale con un timeout  
 L'esempio seguente illustra come monitorare una conversazione specifica per cui il servizio Initiator e quello di destinazione si trovano nello stesso database nell'istanza predefinita dello stesso computer in cui è in esecuzione **ssbdiagnose**. L'intervallo di timeout è impostato su 20 secondi.  
  
```  
ssbdiagnose -E -d TestDatabase RUNTIME -ID D68D77A9-B1CF-41BF-A5CE-279ABCAB140D -TIMEOUT 20  
```  
  
### <a name="g-monitor-the-status-of-a-conversation-that-spans-two-computers"></a>G. Monitoraggio dello stato di una conversazione eseguita tra due computer  
 Nell'esempio seguente viene illustrato come monitorare una conversazione specifica per cui il servizio Initiator e quello di destinazione si trovano in database diversi.  
  
```  
ssbdiagnose RUNTIME -ID D68D77A9-B1CF-41BF-A5CE-279ABCAB140D   
-TIMEOUT 10 CONNECT TO -E -S InitiatorComputer/InitiatorInstance   
-d InitiatorDatabase CONNECT TO -E -S TargetComputer/TargetInstance   
-d TargetDatabase  
```  
  
### <a name="h-monitor-the-status-of-a-conversation-in-two-databases-in-the-same-instance"></a>H. Monitoraggio dello stato di una conversazione in due database nella stessa istanza  
 Nell'esempio seguente viene illustrato come monitorare una conversazione specifica per cui il servizio Initiator e quello di destinazione si trovano in database diversi della stessa istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. L'esempio usa **baseconnectionoptions** per specificare le informazioni sull'istanza e sull'account di accesso e le due clausole CONNECT TO per specificare i database. L'opzione -SHOWEVENTS viene specificata in modo che tutti gli eventi in fase di esecuzione vengano inclusi nel report restituito.  
  
```  
ssbdiagnose -E -S TestComputer/DevTestInstance RUNTIME -SHOWEVENTS   
-ID 5094d4a7-e38c-4c37-da37-1d58b1cb8455 -TIMEOUT 10 CONNECT TO   
-d InitiatorDatabase CONNECT TO -d TargetDatabase  
```  
  
### <a name="i-monitor-the-status-of-two-conversations-between-two-databases"></a>I. Monitoraggio dello stato di due conversazioni tra due database  
 Nell'esempio seguente viene illustrato come monitorare due conversazioni per cui il servizio Initiator e quello di destinazione si trovano in database diversi della stessa istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. L'esempio usa **baseconnectionoptions** per specificare le informazioni sull'istanza e sull'account di accesso e le due clausole CONNECT TO per specificare i database.  
  
```  
ssbdiagnose -E -S TestComputer/DevTestInstance RUNTIME   
-ID 5094d4a7-e38c-4c37-da37-1d58b1cb8455   
-ID 9b293be9-226b-4e22-e169-1d2c2c15be86 -TIMEOUT 10 CONNECT TO   
-d InitiatorDatabase CONNECT TO -d TargetDatabase  
```  
  
### <a name="j-monitor-the-status-of-all-conversations-between-two-databases"></a>J. Monitoraggio dello stato di tutte le conversazioni tra due database  
 Nell'esempio seguente viene illustrato come monitorare tutta la conversazione tra due database nella stessa istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. L'esempio usa **baseconnectionoptions** per specificare le informazioni sull'istanza e sull'account di accesso e le due clausole CONNECT TO per specificare i database.  
  
```  
ssbdiagnose -E -S TestComputer/DevTestInstance RUNTIME   
-TIMEOUT 10 CONNECT TO -d InitiatorDatabase CONNECT TO   
-d TargetDatabase  
```  
  
### <a name="k-ignore-specific-errors"></a>K. Ignorare errori specifici  
 Nell'esempio seguente viene illustrato come ignorare errori noti (303 e 304) in un'attivazione configurata attualmente in un sistema di prova.  
  
```  
ssbdiagnose -IGNORE 303 -IGNORE 304 -E -d TestDatabase   
CONFIGURATION FROM SERVICE /test/initiator TO SERVICE /test/target   
ON CONTRACT TextContract  
```  
  
### <a name="l-redirecting-ssbdiagnose-xml-output"></a>L. Reindirizzamento dell'output XML di ssbdiagnose  
 L'esempio seguente illustra come richiedere che **ssbdiagnose** generi come output un file con estensione xml reindirizzato a un altro file. Il file TestDiag.xml può quindi essere aperto da un'applicazione per analizzare o segnalare i file con estensione xml di **ssbdiagnose** . In alternativa, è possibile visualizzarlo mediante un editor XML generico, ad esempio XML Notepad.  
  
```  
ssbdiagnose -XML -E -d MyDatabase CONFIGURATION FROM SERVICE   
/test/initiator TO SERVICE /test/target > c:\MyDiagnostics\TestDiag.xml  
```  
  
### <a name="m-using-an-environment-variable"></a>M. Utilizzo di una variabile di ambiente  
 L'esempio seguente imposta prima la variabile di ambiente SQLCMDSERVER per specificare il nome del server, quindi esegue **ssbdiagnose** senza specificare **-S**.  
  
```  
SET SQLCMDSERVER=MyComputer  
ssbdiagnose -XML -E -d MyDatabase CONFIGURATION FROM SERVICE   
/test/initiator TO SERVICE /test/target  
```  
  
## <a name="see-also"></a>Vedere anche  
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)   
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [CREATE BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/create-broker-priority-transact-sql.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE CONTRACT &#40;Transact-SQL&#41;](../../t-sql/statements/create-contract-transact-sql.md)   
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md)   
 [CREATE MESSAGE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-message-type-transact-sql.md)   
 [CREATE QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/create-queue-transact-sql.md)   
 [CREATE REMOTE SERVICE BINDING &#40;Transact-SQL&#41;](../../t-sql/statements/create-remote-service-binding-transact-sql.md)   
 [CREATE ROUTE &#40;Transact-SQL&#41;](../../t-sql/statements/create-route-transact-sql.md)   
 [CREATE SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/create-service-transact-sql.md)   
 [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md)   
 [sys.transmission_queue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-transmission-queue-transact-sql.md)   
 [sys.conversation_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)   
 [sys.conversation_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-groups-transact-sql.md)  
  
