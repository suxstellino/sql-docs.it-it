---
title: Utilità sqlcmd
description: L'utilità sqlcmd consente di immettere istruzioni Transact-SQL, procedure di sistema e file di script usando diverse modalità e usa ODBC per eseguire i batch Transact-SQL.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- statements [SQL Server], command prompt
- QUIT command
- Transact-SQL statements, command prompt
- EXIT command
- sqlcmd commands
- ED command
- sqlcmd utility
- command prompt utilities [SQL Server], sqlcmd
- '!! command'
- stored procedures [SQL Server], command prompt
- system stored procedures [SQL Server], command prompt
- sqlcmd utility, about sqlcmd utility
- scripts [SQL Server], command prompt
- RESET command
- GO command
ms.assetid: e1728707-5215-4c04-8320-e36f161b834a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 02/24/2021
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: ad1d3e98d45dc0395da92ee331e48b3fbfc161e6
ms.sourcegitcommit: 295b9dfc758471ef7d238a2b0f92f93e34acbb1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/31/2021
ms.locfileid: "106054785"
---
# <a name="sqlcmd-utility"></a>Utilità sqlcmd

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

> Per SQL Server 2014 e versioni precedenti, vedere l'[utilità sqlcmd](/previous-versions/sql/2014/tools/sqlcmd-utility?view=sql-server-2014&preserve-view=true).
>
> Per l'uso di sqlcmd in Linux, vedere le [istruzioni per l'installazione di sqlcmd e bcp in Linux](../linux/sql-server-linux-setup-tools.md).

 L'utilità **sqlcmd** consente di immettere istruzioni Transact-SQL, procedure di sistema e file di script usando diverse modalità disponibili:

- Al prompt dei comandi.
- Nell'**editor di query** in modalità SQLCMD.
- In un file di script di Windows.
- In un passaggio del processo del sistema operativo (Cmd. exe) di un processo di SQL Server Agent.

L'utilità usa ODBC per eseguire batch Transact-SQL.

## <a name="download-the-latest-version-of-sqlcmd-utility"></a>Scaricare la versione più recente della utilità sqlcmd

**[![Download sqlcmd per x64](../ssdt/media/download.png) Download di Microsoft Command Line Utilities 15 per SQL Server (x64) (2,6 MB)](https://go.microsoft.com/fwlink/?linkid=2142258)**
<br>**[![Download sqlcmd per x86](../ssdt/media/download.png) Download di Microsoft Command Line Utilities 15 per SQL Server (x86) (2,3 MB)](https://go.microsoft.com/fwlink/?linkid=2142257)**

Gli strumenti da riga di comando dipendono dalla disponibilità generale (GA), ma vengono rilasciati con il pacchetto di installazione per [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)].

**Informazioni sulla versione**

Numero di versione: 15.0.2<br>
Numero di build: 15.0.2000.5<br>
Data di rilascio: 11 settembre 2020

La nuova versione di SQLCMD supporta l'autenticazione di Azure AD e include anche il supporto di Multi-Factor Authentication (MFA) per il database SQL, Azure Synapse Analytics e Always Encrypted.
Il nuovo BCP supporta l'autenticazione di Azure AD e include anche il supporto di Multi-Factor Authentication (MFA) per il database SQL e Azure Synapse Analytics.

**Requisiti di sistema** per Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Server 2008 - 2019.

Per questo componente sono necessari sia [Windows Installer 4.5](https://windows-installer.soft32.com/) che [Microsoft ODBC Driver 17 for SQL Server](../connect/odbc/download-odbc-driver-for-sql-server.md).
 
Per controllare la versione di SQLCMD, eseguire il comando `sqlcmd -?` e verificare che la versione in uso sia 15.0.2000.5 o successiva.

> [!NOTE]
> Per supportare la funzionalità Always Encrypted (`-g`) e l'autenticazione di Azure Active Directory (`-G`) è necessaria la versione 13.1 o successiva. Nel computer potrebbero essere installate diverse versioni di sqlcmd.exe. Assicurarsi di usare la versione corretta. Per determinare la versione, eseguire `sqlcmd -?`.

È possibile provare l'utilità sqlcmd da Azure Cloud Shell poiché è preinstallata per impostazione predefinita: [![Avviare Cloud Shell](https://shell.azure.com/images/launchcloudshell.png "Avviare Cloud Shell")](https://shell.azure.com)

Per eseguire istruzioni sqlcmd in SSMS, selezionare la modalità SQLCMD dal menu a discesa Query nella parte superiore della struttura di navigazione.  

> [!IMPORTANT]
> [!INCLUDE[ssManStudioFull_md](../includes/ssmanstudiofull-md.md)] (SSMS) usa Microsoft [!INCLUDE[dnprdnshort_md](../includes/dnprdnshort-md.md)] SqlClient per l'esecuzione in modalità normale e SQLCMD nell'**editor di query**. Se l'utilità **sqlcmd** viene eseguita dalla riga di comando, **sqlcmd** usa il driver ODBC. Poiché possono essere applicate opzioni predefinite diverse, l'esecuzione della stessa query nella modalità SQLCMD di [!INCLUDE[ssManStudioFull_md](../includes/ssmanstudiofull-md.md)] e nell'utilità **sqlcmd** potrebbe generare risultati diversi.  
>

L'utilità **sqlcmd** non richiede attualmente uno spazio tra l'opzione della riga di comando e il valore. In una versione futura, tuttavia, è possibile che tra l'opzione della riga di comando e il valore sia necessario inserire uno spazio.  

 Altri argomenti: 

- [Avvio dell'utilità sqlcmd](../ssms/scripting/sqlcmd-start-the-utility.md)
- [Utilizzo dell'utilità sqlcmd](../ssms/scripting/sqlcmd-use-the-utility.md)
  
## <a name="syntax"></a>Sintassi

```cmd
sqlcmd
   -a packet_size
   -A (dedicated administrator connection)
   -b (terminate batch job if there is an error)
   -c batch_terminator
   -C (trust the server certificate)
   -d db_name
   -D
   -e (echo input)
   -E (use trusted connection)
   -f codepage | i:codepage[,o:codepage] | o:codepage[,i:codepage]
   -g (enable column encryption)
   -G (use Azure Active Directory for authentication)
   -h rows_per_header
   -H workstation_name
   -i input_file
   -I (enable quoted identifiers)
   -j (Print raw error messages)
   -k[1 | 2] (remove or replace control characters)  
   -K application_intent  
   -l login_timeout  
   -L[c] (list servers, optional clean output)  
   -m error_level  
   -M multisubnet_failover  
   -N (encrypt connection)  
   -o output_file  
   -p[1] (print statistics, optional colon format)  
   -P password  
   -q "cmdline query"  
   -Q "cmdline query" (and exit)  
   -r[0 | 1] (msgs to stderr)  
   -R (use client regional settings)  
   -s col_separator  
   -S [protocol:]server[instance_name][,port]  
   -t query_timeout  
   -u (unicode output file)  
   -U login_id  
   -v var = "value"
   -V error_severity_level
   -w column_width
   -W (remove trailing spaces)
   -x (disable variable substitution)
   -X[1] (disable commands, startup script, environment variables, optional exit)
   -y variable_length_type_display_width
   -Y fixed_length_type_display_width
   -z new_password
   -Z new_password (and exit)
   -? (usage)
```
  
## <a name="command-line-options"></a>Opzioni della riga di comando

**Opzioni correlate all'account di accesso**

**-A**  
Consente di accedere a SQL Server tramite una connessione amministrativa dedicata (DAC, Dedicated Administrator Connection). Questo tipo di connessione viene utilizzato per eseguire la risoluzione dei problemi a livello di server Questa connessione funziona solo in computer server che supportano l'applicazione livello dati. Se la connessione DAC non è disponibile, l'utilità **sqlcmd** genera un messaggio di errore e viene chiusa. Per altre informazioni sulle connessioni DAC, vedere [Connessione di diagnostica per gli amministratori di database](../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md). L'opzione -A non è supportata con l'opzione -G. Per eseguire la connessione al database SQL con -A, è necessario essere un amministratore di SQL Server. L'applicazione livello dati non è disponibile per un amministratore Azure Active Directory.

**-C**  
Questa opzione viene utilizzata dal client per configurare l'attendibilità implicita del certificato del server senza necessità di convalida ed equivale all'opzione ADO.NET `TRUSTSERVERCERTIFICATE = true`.  

**-d** _db_name_  
Esegue un'istruzione `USE` *db_name* all'avvio di **sqlcmd**. Questa opzione imposta la variabile di scripting SQLCMDDBNAME di **sqlcmd** . Questo parametro specifica il database iniziale. Il valore predefinito corrisponde alla proprietà relativa al database predefinito dell'accesso. Se il database non esiste, viene generato un messaggio di errore e l'utilità **sqlcmd** viene chiusa.  

**-D**  
Interpreta il nome del server fornito a -S come DSN anziché come nome host. Per altre informazioni, vedere _Supporto di DSN in sqlcmd e bcp_ in [Connessione con sqlcmd](../connect/odbc/linux-mac/connecting-with-sqlcmd.md).

> [!NOTE]
> L'opzione -D è disponibile solo nei client Linux e MacOS. Nei client Windows, in precedenza veniva fatto riferimento a un'opzione ormai obsoleta che è stata rimossa ed è ignorata.

**-l** _login_timeout_  
Specifica il numero dei secondi che devono trascorrere prima che si verifichi il timeout di un accesso di **sqlcmd** al driver ODBC quando si tenta la connessione a un server. Questa opzione imposta la variabile di scripting SQLCMDLOGINTIMEOUT di **sqlcmd** . Il valore predefinito per il timeout di accesso a **sqlcmd** è otto secondi. Quando si usa l'opzione **-G** per la connessione al database SQL o ad Azure Synapse Analytics e l'autenticazione tramite Azure Active Directory, è consigliabile impostare un valore di timeout di almeno 30 secondi. Il valore del timeout deve essere un numero compreso tra 0 e 65.534. Se il valore specificato non è numerico o non è compreso in tale intervallo, **sqlcmd** genera un messaggio di errore. Il valore 0 specifica un timeout infinito.

**-E**  
Usa una connessione trusted anziché un nome utente e una password per l'accesso a SQL Server. In caso di omissione di **-E** , **sqlcmd** usa l'opzione della connessione trusted per impostazione predefinita.  

L'opzione **-E** ignora eventuali impostazioni delle variabili di ambiente relative a nome utente e password, ad esempio SQLCMDPASSWORD. Se si usa l'opzione **-E** in combinazione con l'opzione **-U** o con l'opzione **-P** , viene generato un messaggio di errore.  

**-g**  
Imposta la crittografia delle colonne su `Enabled`. Per altre informazioni, vedere [Always Encrypted](../relational-databases/security/encryption/always-encrypted-database-engine.md). Sono supportate solo le chiavi master presenti nell'archivio certificati Windows. L'opzione -g richiede almeno **sqlcmd** versione [13.1](https://go.microsoft.com/fwlink/?LinkID=825643). Per determinare la versione, eseguire `sqlcmd -?`.

**-G**  
Questa opzione viene usata dal client durante la connessione al database SQL o ad Azure Synapse Analytics per specificare che l'utente deve essere autenticato tramite l'autenticazione di Azure Active Directory. Questa opzione imposta la variabile di scripting SQLCMDUSEAAD = true di **sqlcmd** . L'opzione -G richiede almeno **sqlcmd** versione [13.1](https://go.microsoft.com/fwlink/?LinkID=825643). Per determinare la versione, eseguire `sqlcmd -?`. Per altre informazioni, vedere [Connessione al database SQL o ad Azure Synapse Analytics con l'autenticazione di Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview). L'opzione -A non è supportata con l'opzione -G.

> [!IMPORTANT]
> L' `-G` opzione si applica solo al database SQL di Azure e ad Azure sinapsi Analytics.
> L'autenticazione integrata di AAD non è attualmente supportata in Linux o macOS. Per l'autenticazione integrata di AAD è necessario [Microsoft ODBC driver 17 for SQL Server](../connect/odbc/download-odbc-driver-for-sql-server.md) versione 17.6.1 o successiva e un [ambiente Kerberos configurato](../connect/odbc/linux-mac/using-integrated-authentication.md)correttamente.


- **Nome utente e password di Azure Active Directory:** 

    Quando si vuole usare nome utente e password di Azure Active Directory, è possibile fornire l'opzione **-G** e anche usare nome utente e password fornendo le opzioni **-U** e **-P** .

    ```cmd
    Sqlcmd -S testsrv.database.windows.net -d Target_DB_or_DW -U bob@contoso.com -P MyAADPassword -G
    ```

    Il parametro -G genera la seguente stringa di connessione nel back-end: 

    ```cmd
     SERVER = Target_DB_or_DW.testsrv.database.windows.net;UID= bob@contoso.com;PWD=MyAADPassword;AUTHENTICATION = ActiveDirectoryPassword 
    ```

- **Integrazione con Azure Active Directory**

   Per l'autenticazione integrata di Azure Active Directory, fornire l'opzione **-G** senza nome utente o password.
   *Per l'autenticazione integrata di AAD è necessario [Microsoft ODBC driver 17 for SQL Server](../connect/odbc/download-odbc-driver-for-sql-server.md) versione 17.6.1 o successiva e un [ambiente Kerberos configurato](../connect/odbc/linux-mac/using-integrated-authentication.md)correttamente.*

    ```cmd
    Sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -G
    ```

    Questo genera la seguente stringa di connessione nel back-end: 

    ```cmd
    SERVER = Target_DB_or_DW.testsrv.database.windows.net Authentication = ActiveDirectoryIntegrated; Trusted_Connection=NO
    ```

    > [!NOTE]
    > L'opzione `-E` (Trusted_Connection) non può essere usata insieme all'opzione `-G`.

- **Azure Active Directory Interactive**

    L'autenticazione interattiva di Azure AD per il database SQL di Azure e Azure Synapse Analytics consente di usare un metodo interattivo che supporta l'autenticazione a più fattori. Per altre informazioni, vedere [Autenticazione interattiva di Active Directory](../ssdt/azure-active-directory.md#active-directory-interactive-authentication). 

   La modalità interattiva di Azure AD richiede **sqlcmd** [versione 15.0.1000.34](#download-the-latest-version-of-sqlcmd-utility) o successiva, nonché [ODBC 17.2 o versioni successive](../connect/odbc/download-odbc-driver-for-sql-server.md).  

   Per abilitare l'autenticazione interattiva, specificare l'opzione -G solo con il nome utente (-U) senza una password.

   Nell'esempio seguente vengono esportati i dati usando la modalità interattiva di Azure AD e indicando il nome utente che rappresenta un account AAD. Si tratta dello stesso esempio usato nella sezione precedente: *Nome utente e password di Azure Active Directory*.  

   Per la modalità interattiva è necessario immettere manualmente una password o, per gli account con l'autenticazione a più fattori abilitata, completare il metodo di autenticazione a più fattori configurato.

   ```cmd
   sqlcmd -S testsrv.database.windows.net -d Target_DB_or_DW -G -U alice@aadtest.onmicrosoft.com
   ```

   Il comando precedente genera la seguente stringa di connessione nel back-end:  

   ```cmd
   SERVER = Target_DB_or_DW.testsrv.database.windows.net;UID=alice@aadtest.onmicrosoft.com; AUTHENTICATION = ActiveDirectoryInteractive   
   ```

   Se un utente di Azure AD fa parte di un dominio federato che usa un account Windows, il nome utente richiesto nella riga di comando contiene il relativo account di dominio, ad esempio joe@contoso.com, vedere di seguito:

   ```cmd
   sqlcmd -S testsrv.database.windows.net -d Target_DB_or_DW -G -U joe@contoso.com  
   ```

   Se in un'istanza specifica di Azure AD esistono utenti guest che fanno parte di un gruppo presente nel database SQL con autorizzazioni di database che consentono di eseguire il comando sqlcmd, viene usato l'alias utente guest, ad esempio *keith0@adventureworks.com* .

  >[!IMPORTANT]
  >Quando si usa `-G` e `-U` con SQLCMD l'autenticazione può avere esito negativo se si specifica l'opzione `-U` prima dell'opzione `-G`. Iniziare sempre con l'opzione `-G` seguita dall'opzione `-U`.

**-H** _workstation_name_  
 Nome di una workstation. Questa opzione imposta la variabile di scripting SQLCMDWORKSTATION di **sqlcmd** . Il nome della workstation è riportato nella colonna **hostname** della vista del catalogo **sys.processes** e può essere ottenuto usando la stored procedure **sp_who**. Se questa opzione viene omessa, il valore predefinito è costituito dal nome del computer corrente. Questo nome può essere usato per identificare 

sessioni **sqlcmd** diverse.  

**-j** Visualizza sullo schermo messaggi di errore non elaborati.
  
 **-K** _application_intent_  
 Dichiara il tipo di carico di lavoro dell'applicazione in caso di connessione a un server. L'unico valore attualmente supportato è **ReadOnly**. Se l'opzione **-K** non è specificata, l'utilità sqlcmd non supporta la connettività a una replica secondaria in un gruppo di disponibilità AlwaysOn. Per altre informazioni, vedere [Repliche secondarie attive: Repliche secondarie leggibili (Gruppi di disponibilità AlwaysOn)](../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)  
  
**-M** _multisubnet_failover_  
 Specificare sempre **-M** in caso di connessione al listener di un gruppo di disponibilità di SQL Server o a un'istanza del cluster di failover di SQL Server. Tramite **-M** viene fornito un rilevamento più veloce di una connessione al server attualmente attivo. Se non si specifica **-M**, significa che l'opzione **-M** è disattivata. Per altre informazioni, vedere [Listener, connettività client e failover dell'applicazione](../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md), [Creazione e configurazione di gruppi di disponibilità &#40;SQL Server&#41;](../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md), [Clustering di failover e gruppi di disponibilità AlwaysOn (SQL Server)](../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md) e [Repliche secondarie attive: Repliche secondarie leggibili (Gruppi di disponibilità AlwaysOn)](../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).
  
 **-N**  
 Questa opzione viene utilizzata dal client per richiedere una connessione crittografata.  
  
 **-P** _password_  
 Password specificata dall'utente. Per le password viene fatta distinzione tra maiuscole e minuscole. Se si usa l'opzione -U omettendo l'opzione **-P** e la variabile di ambiente SQLCMDPASSWORD non è stata impostata, **sqlcmd** chiede all'utente di immettere una password. Non è consigliabile usare la password Null, ma è possibile specificarla usando una coppia di virgolette doppie contigue per il valore del parametro:

- **-P ""**

È consigliabile usare una password complessa.

#### <a name="use-a-strong-password"></a>[**Usare una password complessa.**](../relational-databases/security/strong-passwords.md)

 La richiesta della password viene visualizzata mediante la stampa nella console, come indicato di seguito: `Password:`  
  
 L'input dell'utente è nascosto. L'input non viene pertanto visualizzato e il cursore rimane in posizione.  
  
 La variabile di ambiente SQLCMDPASSWORD consente di impostare una password predefinita per la sessione corrente. Per tale motivo, non è necessario specificare le password a livello di codice in file batch.  
  
 L'esempio seguente imposta prima di tutto la variabile SQLCMDPASSWORD al prompt dei comandi e quindi accede all'utilità **sqlcmd** . Al prompt dei comandi digitare:  
  
 `SET SQLCMDPASSWORD= p@a$$w0rd`  
 Al prompt dei comandi successivo digitare:  
  
 `sqlcmd`  
  
 Se la combinazione di nome utente e password non è corretta, viene generato un messaggio di errore.  
  
**NOTA**  La variabile di ambiente OSQLPASSWORD è disponibile per motivi di compatibilità con le versioni precedenti. La variabile di ambiente SQLCMDPASSWORD ha la priorità rispetto alla variabile di ambiente OSQLPASSWORD. Ora che OSQLPASSWORD non è più condivisa, è possibile usare le utilità **sqlcmd** e **osql** l'una accanto all'altra senza interferenze. Gli script usati in precedenza continueranno a funzionare.  
  
 Se si usa l'opzione **-P** in combinazione con l'opzione **-E** , viene generato un messaggio di errore.  
  
 Se l'opzione **-P** è seguita da più di un argomento, viene generato un messaggio di errore e il programma viene chiuso.  
  
 **-S** [*protocol*:]*server*[ **\\** _instance\_name_][ **,** _port_]  
 Specifica l'istanza di SQL Server alla quale connettersi. Imposta la variabile di scripting SQLCMDSERVER di **sqlcmd** .  
  
 Specificare *server_name* per connettersi all'istanza predefinita di SQL Server nel computer server. Specificare *server_name* [ **\\** _instance\_name_ ] per connettersi a un'istanza denominata di SQL Server in tale computer server. Se non si specifica alcun server, **sqlcmd** si connette all'istanza predefinita di SQL Server del computer locale. Questa opzione è necessaria per l'esecuzione di **sqlcmd** da un computer remoto in rete.  
  
 *protocol* può essere **tcp** (TCP/IP), **lpc** (memoria condivisa) o **np** (named pipe).  
  
 Se non si specifica il valore di *server_name* [ **\\** _instance\_name_ ] all'avvio di **sqlcmd**, SQL Server verifica la presenza della variabile di ambiente SQLCMDSERVER e la usa.  
  
> [!NOTE]  
>  La variabile di ambiente OSQLSERVER è disponibile per motivi di compatibilità con le versioni precedenti. La variabile di ambiente SQLCMDSERVER è prioritaria rispetto alla variabile di ambiente OSQLSERVER. Questo significa che è possibile usare **sqlcmd** e **osql** insieme senza creare conflitti e che gli script precedenti continueranno a funzionare correttamente.  
  
 **-U** _login_id_  
 È il nome di accesso o il nome utente di un database indipendente. Per gli utenti dei database indipendenti, è necessario specificare l'opzione del nome di database (-d).  
  
> [!NOTE]  
>  La variabile di ambiente OSQLUSER è disponibile per motivi di compatibilità con le versioni precedenti. La variabile di ambiente SQLCMDUSER ha la priorità rispetto alla variabile di ambiente OSQLUSER. È quindi possibile usare **sqlcmd** e **osql** insieme senza conflitti e gli script **osql** esistenti continueranno a funzionare correttamente.  
  
 Se si omettono le opzioni **-U** e **-P**, **sqlcmd** prova a connettersi usando la modalità di autenticazione di Microsoft Windows. L'autenticazione si basa sull'account Windows dell'utente che esegue **sqlcmd**.  
  
 Se si usa l'opzione **-U** in combinazione con l'opzione **-E**, descritta più avanti in questo articolo, viene generato un messaggio di errore. Se l'opzione **-U** è seguita da più di un argomento, viene generato un messaggio di errore e il programma viene chiuso.  
  
 **-z** _new_password_  
 Modifica la password:  
  
 `sqlcmd -U someuser -P s0mep@ssword -z a_new_p@a$$w0rd`  
  
 **-Z** _new_password_  
 Consente di modificare la password e di chiudere l'utilità:  
  
 `sqlcmd -U someuser -P s0mep@ssword -Z a_new_p@a$$w0rd`  
  
 **Opzioni di input/output**  
  **-f** _codepage_ | **i:** _codepage_[ **,o:** _codepage_] | **o:** _codepage_[ **,i:** _codepage_]  
 Specifica le tabelle codici di input e output. Il numero specificato per codepage è un valore numerico che indica una tabella codici di Windows installata.  
  
 Regole di conversione delle tabelle codici  
  
- Se non viene specificata alcuna tabella codici, **sqlcmd** userà la tabella codici corrente per i file sia di input che di output, a meno che il file di input non sia un file Unicode per cui non è necessaria alcuna conversione.  
  
- **sqlcmd** riconosce automaticamente file di input Unicode sia di tipo Big-Endian che di tipo Little-Endian. Se è stata specificata l'opzione **-u** , l'output sarà sempre in formato Unicode Little-Endian.  
  
- Se non viene specificato alcun file di output, la tabella codici di output sarà costituita dalla tabella codici della console. Questo approccio consente la visualizzazione corretta dell'output nella console.  
  
- Se sono disponibili più file di input, vengono considerati appartenenti alla stessa tabella codici. È possibile combinare file di input Unicode e non Unicode.  
  
 Immettere **chcp** al prompt dei comandi per verificare la tabella codici di Cmd.exe.  
  
 **-i** _input_file_[ **,** _input\_file2_...]  
 Identifica il file che include un batch di istruzioni SQL o stored procedure. È possibile specificare più file che verranno letti ed elaborati nell'ordine in cui sono stati indicati. Non utilizzare alcuno spazio tra i nomi di file. **sqlcmd** verificherà prima di tutto che tutti i file specificati esistano. Se uno o più file non esistono, l'utilità **sqlcmd** viene chiusa. Le opzioni -i e -Q/-q si escludono a vicenda.  
  
 Percorsi di esempio:  

```cmd
-i C:\<filename>  
-i \\<Server>\<Share$>\<filename>  
-i "C:\Some Folder\<file name>"  
```
  
 È necessario racchiudere tra virgolette i percorsi dei file contenenti spazi.  
  
 Questa opzione può essere usata più di una volta: **-i**_input\_file_ **-I**_I input_file._  
  
 **-o** _output_file_  
 Identifica il file che riceve l'output di **sqlcmd**.  
  
 Se si specifica **-u** , *output_file* viene archiviato in formato Unicode. Se il nome file non è valido, viene generato un messaggio di errore e l'utilità **sqlcmd** viene chiusa. **sqlcmd** non supporta la scrittura simultanea di più processi **sqlcmd** nello stesso file. L'output del file risulterà danneggiato o non corretto. L'opzione **-f** è pertinente anche per i formati di file. Se questo file non esiste, viene creato automaticamente. Un file con lo stesso nome di una sessione di **sqlcmd** precedente verrà sovrascritto. Il file specificato in questa posizione non corrisponde al file **stdout** . Se viene specificato un file **stdout**, questo file non verrà usato.  
  
 Percorsi di esempio:  

```cmd
-o C:< filename>  
-o \\<Server>\<Share$>\<filename>  
-o "C:\Some Folder\<file name>"  
 ```
 È necessario racchiudere tra virgolette i percorsi dei file contenenti spazi.  
  
 **-r**[**0** | **1**]  
 Reindirizza l'output dei messaggi di errore sullo schermo (**stderr**). Se il parametro viene omesso o si specifica **0**, vengono reindirizzati solo i messaggi di errore con livello di gravità 11 o superiore. Se si specifica **1**, viene reindirizzato l'output di tutti i messaggi di errore che includono PRINT. Non ha effetto se si utilizza -o. Per impostazione predefinita, i messaggi sono inviati a **stdout**.  
  
 **-R**  
 Fa in modo che **sqlcmd** localizzi le colonne numeriche, di valuta, di data e di ora recuperate da SQL Server in base alle impostazioni locali del client. Per impostazione predefinita, tali colonne vengono visualizzate usando le impostazioni internazionali del server.  
  
 **-u**  
 Specifica l'archiviazione di *output_file* in formato Unicode, indipendentemente dal formato di *input_file*.  
  
 **Opzioni relative all'esecuzione di query**  
  **-e**  
 Scrive gli script di input nel dispositivo di output standard (**stdout**).  
  
 **-I**  
 Imposta l'opzione di connessione SET QUOTED_IDENTIFIER su ON. Per impostazione predefinita, l'opzione è impostata su OFF. Per altre informazioni, vedere [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](~/t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
 **-q "** _cmdline query_ **"**  
 Esegue una query all'avvio di **sqlcmd** senza chiudere **sqlcmd** al termine dell'esecuzione della query. È possibile eseguire più query delimitandole con punti e virgola. Racchiudere la query tra virgolette come illustrato nell'esempio seguente.  
  
 Al prompt dei comandi digitare:  
  
 `sqlcmd -d AdventureWorks2012 -q "SELECT FirstName, LastName FROM Person.Person WHERE LastName LIKE 'Whi%';"`  
  
 `sqlcmd -d AdventureWorks2012 -q "SELECT TOP 5 FirstName FROM Person.Person;SELECT TOP 5 LastName FROM Person.Person;"`  
  
> [!IMPORTANT]  
>  Non utilizzare il carattere di terminazione GO nella query.  
  
 Se insieme a questa opzione si specifica **-b** , l'utilità **sqlcmd** viene chiusa in caso di errore. L'opzione **-b** è descritta più avanti in questo articolo.  
  
 **-Q "** _cmdline query_ **"**  
 Esegue una query all'avvio di **sqlcmd** e quindi chiude immediatamente **sqlcmd**. È possibile eseguire più query delimitandole con punti e virgola.  
  
 Racchiudere la query tra virgolette come illustrato nell'esempio seguente.  
  
 Al prompt dei comandi digitare:  
  
 `sqlcmd -d AdventureWorks2012 -Q "SELECT FirstName, LastName FROM Person.Person WHERE LastName LIKE 'Whi%';"`  
  
 `sqlcmd -d AdventureWorks2012 -Q "SELECT TOP 5 FirstName FROM Person.Person;SELECT TOP 5 LastName FROM Person.Person;"`  
  
> [!IMPORTANT]  
>  Non utilizzare il carattere di terminazione GO nella query.  
  
 Se insieme a questa opzione si specifica **-b** , l'utilità **sqlcmd** viene chiusa in caso di errore. L'opzione **-b** è descritta più avanti in questo articolo.  
  
 **-t** _query_timeout_  
 Specifica il numero di secondi prima del timeout del comando o dell'istruzione SQL. Questa opzione imposta la variabile di scripting SQLCMDSTATTIMEOUT di **sqlcmd** . Se non viene specificato un valore di *time_out* , il comando non prevede alcun timeout. Il  *time_out* di query deve essere un numero compreso tra 1 e 65534. Se il valore specificato non è numerico o non è compreso in tale intervallo, **sqlcmd** genera un messaggio di errore.  
  
> [!NOTE]  
>  Il valore di timeout effettivo può variare di diversi secondi rispetto al valore specificato per *time_out* .  
  
 **-vvar =**  _value_[ **var =** _value_...]  
 Crea una variabile di scripting di **sqlcmd** che può essere usata in uno script **sqlcmd** . Se il valore contiene spazi, racchiuderlo tra virgolette. È possibile specificare più valori _**var**_= **"** _values_ **"** . Se in uno dei valori specificati è incluso un errore, l'utilità **sqlcmd** genera un messaggio di errore e viene chiusa.  
  
 `sqlcmd -v MyVar1=something MyVar2="some thing"`  
  
 `sqlcmd -v MyVar1=something -v MyVar2="some thing"`  
  
 **-x**  
 Indica a **sqlcmd** di ignorare le variabili di scripting. Questo parametro risulta utile quando uno script contiene numerose istruzioni INSERT che possono includere stringhe con lo stesso formato di variabili normali, ad esempio $(*variable_name*).  
  
 **Opzioni di formattazione**  
  **-h** _headers_  
 Specifica il numero di righe da stampare tra le intestazioni delle colonne. Per impostazione predefinita, le intestazioni vengono stampate una volta per ogni set di risultati delle query. Questa opzione imposta la variabile di scripting SQLCMDHEADERS di **sqlcmd** . Usare **-1** per non stampare alcuna intestazione. Eventuali valori non validi comportano la generazione di un messaggio di errore e la chiusura di **sqlcmd** .  
  
 **-k** [**1** | **2**]  
 Rimuove tutti i caratteri di controllo, ad esempio i caratteri di tabulazione e nuova riga, dall'output. Questo parametro conserva la formattazione di colonna quando vengono restituiti i dati. Se si specifica 1, i caratteri di controllo vengono sostituiti da un singolo spazio. Se si specifica 2, i caratteri di controllo consecutivi vengono sostituiti da uno spazio singolo. **-k** corrisponde a **-k1**.  
  
 **-s** _col_separator_  
 Specifica il carattere separatore di colonna. L'impostazione predefinita è uno spazio vuoto. Questa opzione imposta la variabile di scripting SQLCMDCOLSEP di **sqlcmd** . Per utilizzare caratteri con un significato speciale per il sistema operativo, ad esempio il carattere e commerciale (&) o il punto e virgola (;), racchiudere il carattere specifico tra virgolette ("). Il separatore di colonna può essere un carattere a 8 bit qualsiasi.  
  
 **-w** _column_width_  
 Specifica la larghezza della schermata per l'output. Questa opzione imposta la variabile di scripting SQLCMDCOLWIDTH di **sqlcmd** . La larghezza della colonna deve essere un numero maggiore di 8 e minore di 65.536. Se la larghezza della colonna specificata non rientra in tale intervallo, **sqlcmd** genera un messaggio di errore. La lunghezza predefinita è di 80 caratteri. Se una riga di output supera la larghezza di colonna specificata, la riga viene riportata nella riga successiva.  
  
 **-W**  
 Questa opzione rimuove gli spazi finali da una colonna. Usare questa opzione in combinazione con l'opzione **-s** quando si preparano i dati per l'esportazione in un'altra applicazione. Non è possibile usare questa opzione con **-y** o **-Y** .  
  
 **-y** _variable_length_type_display_width_  
 Imposta la variabile di scripting **di** sqlcmd `SQLCMDMAXVARTYPEWIDTH`. L'impostazione predefinita è 256. Limita il numero di caratteri restituiti per i tipi di dati a lunghezza variabile di grandi dimensioni:  
  
- **ntext**  
  
- **nvarchar(max)**  
  
- **varbinary(max)**  
  
- **xml**  
  
- **UDT (tipi di dati definiti dall'utente)**  
  
- **text**  
  
- **ntext**  
  
- **image**  
  
> [!NOTE]
> I tipi definiti dall'utente (UDT) possono essere a lunghezza fissa in base all'implementazione. Se la lunghezza di un tipo definito dall'utente (UDT) a lunghezza fissa è inferiore a *display_width*, il valore del tipo definito dall'utente (UDT) restituito non viene alterato. Se invece la lunghezza è superiore a *display_width*, l'output viene troncato.  
  
> [!IMPORTANT]
>  Usare l'opzione **-y 0** con estrema cautela, poiché potrebbe causare gravi problemi a livello di prestazioni del server e della rete, a seconda delle dimensioni dei dati restituiti.  
  
 **-Y** _fixed_length_type_display_width_  
 Imposta la variabile di scripting **di** sqlcmd `SQLCMDMAXFIXEDTYPEWIDTH`. Il valore predefinito è 0 (illimitato). Limita il numero di caratteri restituiti per i tipi di dati seguenti:  
  
- **char(** _n_ **)** , dove 1<=n<=8000  
  
- **nchar(n** _n_ **)** , dove 1<=n<=4000  
  
- **varchar(n** _n_ **)** , dove 1<=n<=8000  
  
- **nvarchar(n** _n_ **)** , dove 1<=n<=4000  
  
- **varbinary(n** _n_ **)** , dove 1<=n\<=4000  
  
- **variant**  
  
 **Opzioni relative alla segnalazione degli errori**  
  **-b**  
 Specifica che in caso di errore l'utilità **sqlcmd** viene chiusa restituendo un valore DOS ERRORLEVEL. Il valore restituito alla variabile DOS ERRORLEVEL è **1** se il messaggio di errore di SQL Server ha un livello di gravità maggiore di 10. In caso contrario, il valore restituito è **0**. Se è stata impostata l'opzione **-V** in combinazione con **-b**, **sqlcmd** non segnalerà un errore se il livello di gravità è minore dei valori impostati tramite **-V**. I file batch del prompt dei comandi consentono di verificare il valore di ERRORLEVEL nonché di gestire correttamente l'errore. **sqlcmd** non segnala errori per il livello di gravità 10 (messaggi informativi).  
  
 Se lo script **sqlcmd** contiene un commento errato o un errore di sintassi o se una variabile di scripting risulta mancante, il valore di ERRORLEVEL restituito è 1.  
  
 **-m** _error_level_  
 Controlla i messaggi di errore inviati a **stdout**. Vengono inviati i messaggi a cui è associato un livello di gravità maggiore o uguale a questo livello. Quando questo valore è impostato su **-1**, vengono inviati tutti i messaggi, inclusi quelli informativi. Non sono consentiti spazi tra **-m** e **-1**. Ad esempio, **-m-1** è un formato valido, mentre **-m-1** non lo è.  
  
 Questa opzione imposta inoltre la variabile di scripting SQLCMDERRORLEVEL di **sqlcmd** . Per impostazione predefinita, il valore di questa variabile è 0.  
  
 **-V** _error_severity_level_  
 Controlla il livello di gravità utilizzato per impostare la variabile ERRORLEVEL. Tramite i messaggi di errore a cui sono associati livelli di gravità maggiori o uguali a questo valore viene impostato ERRORLEVEL. I valori minori di 0 vengono indicati come 0. È possibile utilizzare file batch e CMD per verificare il valore della variabile ERRORLEVEL.  
  
 **Opzioni varie**  
  **-a** _packet_size_  
 Richiede un pacchetto di dimensione diversa. Questa opzione imposta la variabile di scripting SQLCMDPACKETSIZE di **sqlcmd** . Il valore di *packet_size* deve essere compreso tra 512 e 32767. Il valore predefinito è 4096. Dimensioni più estese del pacchetto possono migliorare le prestazioni per l'esecuzione di script che dispongono di molte istruzioni SQL tra comandi GO. È possibile richiedere dimensioni di pacchetto superiori. Se tale richiesta viene negata, tuttavia, **sqlcmd** usa le dimensioni di pacchetto predefinite del server.  
  
 **-c** _batch_terminator_  
 Specifica il carattere di terminazione di batch. Per impostazione predefinita, i comandi vengono terminati e inviati a SQL Server tramite l'immissione della parola "GO" su una riga a sé stante. Se il carattere di terminazione di batch viene reimpostato, non utilizzare parole chiave riservate Transact-SQL o caratteri con un significato speciale per il sistema operativo anche se sono preceduti da una barra rovesciata.  
  
 **-L**[**c**]  
 Elenca i computer server configurati localmente e i nomi dei computer server che trasmettono in rete. Questo parametro non può essere utilizzato in combinazione con altri parametri. Il numero massimo di computer del server che è possibile specificare è 3000. Se l'elenco server è troncato a causa della dimensione del buffer, verrà visualizzato un messaggio di avviso.  
  
> [!NOTE]  
> A causa della natura delle trasmissioni in rete, è possibile che **sqlcmd** non riceva una risposta tempestiva da tutti i server e pertanto che l'elenco di server restituito sia diverso per ogni chiamata di questa opzione.  
  
 Se si specifica il parametro facoltativo **c** , l'output viene visualizzato senza la riga di intestazione **Servers:** e ogni riga del server viene elencata senza spazi iniziali. Questa presentazione è definita output pulito. L'output pulito consente di migliorare le prestazioni di elaborazione dei linguaggi di scripting.  
  
 **-p**[**1**]  
 Stampa le statistiche delle prestazioni per ogni set di risultati. Di seguito è riportato un esempio del formato delle statistiche delle prestazioni:  
  
 `Network packet size (bytes): n`  
  
 `x xact[s]:`  
  
 `Clock Time (ms.): total       t1  avg       t2 (t3 xacts per sec.)`  
  
 Dove:  
  
 `x` = numero di transazioni elaborate da SQL Server.  
  
 `t1` = tempo totale per tutte le transazioni.  
  
 `t2` = tempo medio per una singola transazione.  
  
 `t3` = numero medio di transazioni al secondo.  
  
 Tutti i valori relativi al tempo sono espressi in millisecondi.  
  
 Se si specifica il parametro facoltativo **1** , l'output delle statistiche è in formato separato da due punti. Questo formato può essere facilmente importato in un foglio di calcolo o elaborato da uno script.  
  
 Se il parametro facoltativo è un valore qualsiasi diverso da **1**, viene generato un messaggio di errore e l'utilità **sqlcmd** viene chiusa.  
  
 **-X**[**1**]  
 Disabilita i comandi che potrebbero pregiudicare la sicurezza del sistema quando si esegue **sqlcmd** da un file batch. I comandi disabilitati vengono comunque riconosciuti. **sqlcmd** genera un messaggio di avviso e continua l'esecuzione. Se si specifica il parametro facoltativo **1** , l'utilità **sqlcmd** genera un messaggio di errore e viene chiusa. Se si specifica l'opzione **-X** , vengono disabilitati i comandi seguenti:  
  
- **ED**  
  
- **!!** _command_  
  
 Se si specifica l'opzione **-X** , le variabili di ambiente non vengono passate a **sqlcmd**. Questa opzione impedisce inoltre che venga eseguito lo script di avvio specificato utilizzando la variabile di scripting SQLCMDINI. Per altre informazioni sulle variabili di scripting di **sqlcmd** , vedere [Utilizzo di sqlcmd con variabili di scripting](~/relational-databases/scripting/sqlcmd-use-with-scripting-variables.md).  
  
 **-?**  
 Visualizza la versione di **sqlcmd** e il riepilogo della sintassi delle opzioni di **sqlcmd** .  
  
## <a name="remarks"></a>Osservazioni

Le opzioni non devono essere necessariamente utilizzate nell'ordine illustrato nella sezione della sintassi.

Se vengono restituiti più risultati, **sqlcmd** stampa una riga vuota tra ogni set di risultati in un batch. Inoltre, il messaggio `<x> rows affected` non viene visualizzato se non si applica all'istruzione eseguita.

Per usare **sqlcmd** in modo interattivo, digitare **sqlcmd** al prompt dei comandi con una o più delle opzioni precedentemente descritte in questo articolo. Per altre informazioni, vedere [Utilizzo dell'utilità sqlcmd](~/relational-databases/scripting/sqlcmd-use-the-utility.md)

> [!NOTE]  
>  Le opzioni **-L**, **-Q**, **-Z** e **-i** causano la chiusura di **sqlcmd** dopo l'esecuzione.
  
 La lunghezza totale della riga di comando di **sqlcmd** nell'ambiente di comando (Cmd.exe), inclusi tutti gli argomenti e le variabili espanse, corrisponde alla lunghezza determinata dal sistema operativo per Cmd.exe.
  
## <a name="variable-precedence-low-to-high"></a>Precedenza delle variabili (in ordine crescente)
  
1.  Variabili di ambiente a livello di sistema.
  
2.  Variabili di ambiente a livello di utente.  
  
3.  Shell dei comandi (**SET** X=Y) impostata al prompt dei comandi prima dell'esecuzione di **sqlcmd**.  
  
4.  **sqlcmd-v** X=Y  
  
5.  **:Setvar** X Y  
  
> [!NOTE]  
>  Per visualizzare le variabili di ambiente, nel **Pannello di controllo** aprire **Sistema** quindi fare clic sulla scheda **Avanzate** .  
  
## <a name="sqlcmd-scripting-variables"></a>Variabili di scripting di sqlcmd  
  
|Variabile|Opzione correlata|L/S|Predefinito|  
|--------------|--------------------|----------|-------------|  
|SQLCMDUSER|-U|R|""|  
|SQLCMDPASSWORD|-P|--|""|  
|SQLCMDSERVER|-S|R|"DefaultLocalInstance"|  
|SQLCMDWORKSTATION|-H|R|"ComputerName"|  
|SQLCMDDBNAME|-d|R|""|  
|SQLCMDLOGINTIMEOUT|-l|L/S|"8" (secondi)|  
|SQLCMDSTATTIMEOUT|-T|L/S|"0" = attesa illimitata|  
|SQLCMDHEADERS|-H|L/S|"0"|  
|SQLCMDCOLSEP|-S|L/S|" "|  
|SQLCMDCOLWIDTH|-w|L/S|"0"|  
|SQLCMDPACKETSIZE|-a|R|"4096"|  
|SQLCMDERRORLEVEL|-M|L/S|0|  
|SQLCMDMAXVARTYPEWIDTH|-y|L/S|"256"|  
|SQLCMDMAXFIXEDTYPEWIDTH|-y|L/S|"0" = numero illimitato|  
|SQLCMDEDITOR||L/S|"edit.com"|  
|SQLCMDINI||R|""|
|SQLCMDUSEAAD  | -G | L/S | "" |  
  
 Le variabili SQLCMDUSER, SQLCMDPASSWORD e SQLCMDSERVER vengono impostate quando viene usato **:Connect**.  
  
 La lettera L indica che il valore può essere impostato una sola volta durante l'inizializzazione del programma.  
  
 L/S indica che è possibile modificare il valore tramite il comando **setvar** e che il nuovo valore influirà sui comandi successivi.  
  
## <a name="sqlcmd-commands"></a>Comandi sqlcmd  
 In aggiunta alle istruzioni Transact-SQL all'interno di **sqlcmd**, sono disponibili anche i comandi seguenti:  
  
:::row:::
   :::column span="":::
      **GO** [*count*]
   :::column-end:::
   :::column span="":::
      **:List**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [ **:** ] **RESET**
   :::column-end:::
   :::column span="":::
      **:Error**  
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [ **:** ] **ED**
   :::column-end:::
   :::column span="":::
      **:Out**  
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [**:**] **!!**
   :::column-end:::
   :::column span="":::
      **:Perftrace**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [ **:** ] **QUIT**
   :::column-end:::
   :::column span="":::
      **:Connect**  
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      [ **:** ] **EXIT**
   :::column-end:::
   :::column span="":::
      **:On Error**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      **:r**
   :::column-end:::
   :::column span="":::
      **:Help**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      **:ServerList**
   :::column-end:::
   :::column span="":::
      **:XML** [**ON** &#124; **OFF**]
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      **:Setvar**
   :::column-end:::
   :::column span="":::
      **:Listvar** 
   :::column-end:::
:::row-end:::
  
 Quando si usano comandi **sqlcmd** , tenere presente quanto segue:  
  
- Tutti i comandi **sqlcmd** , ad eccezione di GO, devono essere preceduti da due punti (:).  
  
    > [!IMPORTANT]  
    >  Per garantire la compatibilità con le versioni precedenti per gli script **osql** esistenti, alcuni comandi verranno riconosciuti anche senza i due punti, indicati da [ **:** ].
  
- I comandi **sqlcmd** vengono riconosciuti solo se si trovano all'inizio di una riga.  
  
- Tutti i comandi **sqlcmd** non fanno distinzione tra maiuscole e minuscole.  
  
- Ogni comando deve trovarsi in una riga distinta. Un comando non può essere seguito da un'istruzione Transact-SQL o da un altro comando.  
  
- I comandi vengono eseguiti immediatamente e non vengono inseriti nel buffer di esecuzione come le istruzioni Transact-SQL.  
  
 **Comandi di modifica**  
  [ **:** ] **ED**  
 Avvia l'editor di testo. Questo editor può essere utilizzato per modificare il batch Transact-SQL corrente o l'ultimo batch eseguito. Per modificare l'ultimo batch eseguito, il comando **ED** deve essere specificato subito dopo il completamento dell'esecuzione dell'ultimo batch.  
  
 L'editor di testo viene definito dalla variabile di ambiente SQLCMDEDITOR. L'editor predefinito è "Edit". Per modificare l'editor, impostare la variabile di ambiente SQLCMDEDITOR. Ad esempio, per impostare l'editor su Blocco note di Microsoft, al prompt dei comandi digitare:  
  
 `SET SQLCMDEDITOR=notepad`  
  
 [ **:** ] **RESET**  
 Cancella la cache dell'istruzione.  
  
 **:List**  
 Stampa il contenuto della cache dell'istruzione.  
  
 **Variabili**  
  **:Setvar** \<**var**> [ **"** _value_ **"** ]  
 Definisce le variabili di scripting di **sqlcmd** . Il formato delle variabili di scripting è il seguente: `$(VARNAME)`.  
  
 I nomi delle variabili non fanno distinzione tra maiuscole e minuscole.  
  
 È possibile impostare le variabili di scripting nei modi seguenti:  
  
- In modo implicito utilizzando un'opzione della riga di comando. Ad esempio, l'opzione **-l** imposta la variabile SQLCMDLOGINTIMEOUT di **sqlcmd** .  
  
- In modo esplicito utilizzando il comando **:Setvar** .  
  
- Mediante la definizione di una variabile di ambiente prima dell'esecuzione di **sqlcmd**.  
  
> [!NOTE]  
>  L'opzione **-X** impedisce la trasmissione delle variabili di ambiente a **sqlcmd**.  
  
 Se il nome di una variabile definita mediante il comando **:Setvar** e di una variabile di ambiente è lo stesso, la variabile definita con **:Setvar** ha la precedenza.  
  
 I nomi delle variabili non devono contenere spazi vuoti.  
  
 I nomi delle variabili non possono disporre dello stesso formato delle espressioni con variabili, ad esempio $(var).  
  
 Se il valore stringa della variabile di scripting contiene spazi vuoti, racchiudere il valore tra virgolette. Se per una variabile di scripting non viene specificato alcun valore, la variabile di scripting viene eliminata.  
  
 **:Listvar**  
 Visualizza l'elenco delle variabili di scripting impostate.  
  
> [!NOTE]  
>  Verranno visualizzate solo le variabili di scripting impostate da **sqlcmd** e quelle impostate usando il comando **:Setvar** .  
  
 **Comandi di output**  
  **:Error**   
 _**\<**_  _filename_  **_>|_ STDERR|STDOUT**  
 Reindirizza tutti gli output degli errori nel file specificato da *filename*, in **stderr** oppure in **stdout**. Il comando **Error** può essere utilizzato più volte in uno script. Per impostazione predefinita, l'output degli errori viene inviato in **stderr**.  
  
 *file name*  
 Crea e apre un file che riceverà l'output. Se il file esiste già, verrà troncato a zero byte. Se il file non è disponibile a causa delle autorizzazioni o per altri motivi, l'output non verrà trasferito e verrà inviato all'ultima destinazione specificata oppure alla destinazione predefinita.  
  
 **STDERR**  
 Trasferisce l'output degli errori al flusso **stderr** . Se l'output è stato reindirizzato, la destinazione alla quale il flusso è stato reindirizzato riceverà l'output degli errori.  
  
 **STDOUT**  
 Trasferisce l'output degli errori al flusso **stdout** . Se l'output è stato reindirizzato, la destinazione alla quale il flusso è stato reindirizzato riceverà l'output degli errori.  
  
 **:Out \<** _filename_ **>** | **STDERR**| **STDOUT**  
 Crea e reindirizza tutti i risultati delle query nel file specificato da *filename*, in **stderr** oppure in **stdout**. Per impostazione predefinita, l'output viene inviato in **stdout**. Se il file esiste già, viene troncato a zero byte. Il comando **Out** può essere utilizzato più volte in uno script.  
  
 **:Perftrace \<** _filename_ **>** | **STDERR**| **STDOUT**  
 Crea e reindirizza tutte le informazioni di traccia delle prestazioni nel file specificato da *filename*, in **stderr** oppure in **stdout**. Per impostazione predefinita, l'output della traccia delle prestazioni viene inviato in **stdout**. Se il file esiste già, viene troncato a zero byte. Il comando **Perftrace** può essere utilizzato più volte in uno script.  
  
 **Comandi di controllo dell'esecuzione**  
  **:On Error**[ **exit** | **ignore**]  
 Imposta l'azione da eseguire quando si verifica un errore durante l'esecuzione dello script o del batch.  
  
 Se si specifica l'opzione **exit** , l'utilità **sqlcmd** viene chiusa con il valore di errore appropriato.  
  
 Se viene usata l'opzione **ignore** , **sqlcmd** ignora l'errore e continua l'esecuzione del batch o dello script. Per impostazione predefinita viene stampato un messaggio di errore.  
  
 [ **:** ] **QUIT**  
 Provoca la chiusura di **sqlcmd** .  
  
 [ **:** ] **EXIT**[ **(** _statement_ **)** ]  
 Consente di usare il risultato di un'istruzione SELECT come valore restituito da **sqlcmd**. Se numerica, la prima colonna dell'ultima riga di risultati viene convertita in un valore integer di 4 byte (long). MS-DOS, Linux e Mac passano il byte di ordine inferiore al livello di errore del processo padre o del sistema operativo. Windows 200x passa l'intero valore intero di 4 byte. La sintassi è:  
  
 `:EXIT(query)`  
  
 Ad esempio:  
  
 `:EXIT(SELECT @@ROWCOUNT)`  
  
 È inoltre possibile includere il parametro **EXIT** in un file batch. Al prompt dei comandi, ad esempio, digitare:  
  
 `sqlcmd -Q "EXIT(SELECT COUNT(*) FROM '%1')"`  
  
 L'utilità **sqlcmd** invia i dati tra parentesi **()** al server. Se una stored procedure di sistema seleziona un set e restituisce un valore, viene restituita solo la selezione. Se non si specifica alcun elemento tra le parentesi dell'istruzione EXIT **()** , viene eseguito tutto ciò che la precede nel batch e l'operazione viene quindi terminata senza restituire alcun valore.  
  
 Se si specifica una query non corretta, l'utilità **sqlcmd** viene chiusa senza restituire alcun valore.  
  
 Di seguito è riportato l'elenco dei formati supportati dall'istruzione EXIT.  
  
- :EXIT  
  
 L'utilità non esegue il batch, quindi viene chiusa immediatamente e non restituisce alcun valore.  
  
- :EXIT( )  
  
 L'utilità esegue il batch, viene chiusa e non restituisce alcun valore.  
  
- :EXIT(query)  
  
 L'utilità esegue il batch in cui è inclusa la query e quindi viene chiusa dopo aver restituito i risultati della query.  
  
 Se in uno script **sqlcmd** si usa RAISERROR e si verifica una condizione con stato 127, l'utilità **sqlcmd** viene chiusa e restituisce al client l'ID di messaggio. Ad esempio:  
  
 `RAISERROR(50001, 10, 127)`  
  
 Questo errore comporta la chiusura dello script **sqlcmd** e la restituzione dell'ID di messaggio 50001 al client.  
  
 I valori restituiti compresi tra -1 e -99 sono riservati a SQL Server e l'utilità **sqlcmd** definisce i valori restituiti aggiuntivi elencati di seguito:  
  
|Valori restituiti|Descrizione|  
|-------------------|-----------------|  
|-100|Si è verificato un errore prima di selezionare il valore restituito.|  
|-101|Selezionando il valore restituito non si sono trovate righe.|  
|-102|Si è verificato un errore di conversione durante la selezione del valore restituito.|  
  
 **GO** [*count*]  
 GO indica sia la fine di un batch che l'esecuzione di eventuali istruzioni Transact-SQL memorizzate nella cache. Il batch viene eseguito più volte in batch separati. Non è possibile dichiarare una variabile più di una volta in un singolo batch.
  
 **Comandi vari**  
  **:r \<** _filename_ **>**  
 Analizza le istruzioni Transact-SQL e i comandi **sqlcmd** aggiuntivi del file specificato da **\<**_filename_**>** nella cache delle istruzioni.  
  
 Se il file contiene istruzioni Transact-SQL non seguite da **GO**, è necessario immettere **GO** nella riga successiva a **:r**.  
  
> [!NOTE]  
>  **\<** _filename_ **>** viene letto in relazione alla directory di avvio in cui è stata eseguita l'utilità **sqlcmd**.  
  
 Il file verrà letto ed eseguito dopo che è stato rilevato un carattere di terminazione di batch. È possibile eseguire più comandi **:r** . Il file può contenere qualsiasi comando **sqlcmd** . incluso il carattere di terminazione di batch **GO**.  
  
> [!NOTE]  
>  Il conteggio delle righe visualizzato in modalità interattiva viene incrementato di 1 per ogni comando `:r` rilevato. Il comando `:r` verrà visualizzato nell'output del comando list.  
  
 **:Serverlist**  
 Elenca i server configurati localmente e i nomi dei server che trasmettono in rete tramite broadcast.  
  
 **:Connect**  _server_name_[ **\\** _instance\_name_] [-l *timeout*] [-U *user_name* [-P *password*]]  
 Stabilisce la connessione a un'istanza di SQL Server. e inoltre chiude la connessione corrente.  
  
 Opzioni di timeout:  
  
|valore|Comportamento|  
|-|-|  
|0|attesa infinita|  
|n>0|attesa di n secondi|  
  
 La variabile di scripting SQLCMDSERVER si adatterà alla connessione attiva corrente.  
  
 Se *timeout* viene omesso, il valore predefinito corrisponde al valore della variabile SQLCMDLOGINTIMEOUT.  
  
 Se si specifica solo *user_name* , come opzione o come variabile di ambiente, all'utente verrà chiesto di immettere una password. Ciò non si verifica se è stata impostata la variabile di ambiente SQLCMDUSER o SQLCMDPASSWORD. Se non si specificano opzioni o variabili di ambiente, per la connessione verrà usata la modalità di autenticazione di Windows. Ad esempio, per connettersi a un'istanza, `instance1`, di SQL Server, `myserver`, usando la sicurezza integrata, usare il comando seguente:  
  
 `:connect myserver\instance1`  
  
 Per connettersi all'istanza predefinita di `myserver` utilizzando le variabili di scripting, specificare:  
  
 `:setvar myusername test`  
  
 `:setvar myservername myserver`  
  
 `:connect $(myservername) $(myusername)`  
  
 [ **:** ] **!!** < *comando*>  
 Esegue i comandi del sistema operativo. Per eseguire un comando del sistema operativo, digitare due punti esclamativi all'inizio della riga ( **!!** ) seguiti dal comando del sistema operativo. Ad esempio:  
  
 `:!! Dir`  
  
> [!NOTE]  
>  Il comando viene eseguito nel computer in cui è in esecuzione **sqlcmd** .  
  
 **:XML** [**ON** | **OFF**]  
 Per altre informazioni, vedere [Formato di output XML](#OutputXML) e [Formato di output JSON](#OutputJSON) in questo articolo  
  
 **:Help**  
 Elenca i comandi di **sqlcmd** con una breve descrizione di ognuno.  
  
### <a name="sqlcmd-file-names"></a>Nomi di file per sqlcmd  
 È possibile specificare i file di input di **sqlcmd** con l'opzione **-i** o il comando **:r** . I file di output possono essere specificati con l'opzione **-o** oppure con i comandi **:Error**, **:Out** e **:Perftrace** . Di seguito vengono illustrate alcune linee guida per l'utilizzo di tali file:  
  
- **:Error**, **:Out** e **:Perftrace** devono usare valori **\<**_filename_**>** distinti. Se viene usato lo stesso valore **\<**_filename_**>** , è possibile che gli input di tali comandi vengano confusi.  
  
- Se un file di input che si trova in un server remoto viene chiamato da **sqlcmd** in un computer locale e contiene un percorso di file con unità come :Out c:\OutputFile.txt, il file di output viene creato nel computer locale e non nel server remoto.  
  
- I percorsi di file validi includono `C:\<filename>`, `\\<Server>\<Share$>\<filename>` e `"C:\Some Folder\<file name>"`. Se il percorso contiene uno spazio, utilizzare le virgolette.  
  
- Ogni nuova sessione di **sqlcmd** sovrascriverà i file esistenti con gli stessi nomi.  
  
### <a name="informational-messages"></a>Messaggi informativi

**sqlcmd** stampa qualsiasi messaggio informativo inviato dal server. Nell'esempio seguente al termine dell'esecuzione delle istruzioni Transact-SQL viene stampato un messaggio informativo.
  
Al prompt dei comandi digitare il comando:

`sqlcmd`
  
Al prompt di sqlcmd digitare:

`USE AdventureWorks2012;`

`GO`

Quando si preme INVIO, viene visualizzato il messaggio informativo seguente: "Il contesto di database è stato sostituito con 'AdventureWorks2012'."  
  
### <a name="output-format-from-transact-sql-queries"></a>Formato di output delle query Transact-SQL  
 **sqlcmd** stampa prima di tutto un'intestazione di colonna contenente i nomi delle colonne specificati nell'elenco di selezione. I nomi di colonna sono delimitati tramite il carattere specificato da SQLCMDCOLSEP. Per impostazione predefinita, viene utilizzato uno spazio. Se la lunghezza del nome di colonna è minore della larghezza della colonna, nell'output vengono inseriti caratteri di riempimento fino alla colonna successiva.  
  
 La riga sarà seguita da una riga di separazione costituita da una serie di trattini. Di seguito è riportato un esempio di output.  
  
 Avviare **sqlcmd**. Al prompt dei comandi di **sqlcmd** digitare la query:  
  
 `USE AdventureWorks2012;`  
  
 `SELECT TOP (2) BusinessEntityID, FirstName, LastName`  
  
 `FROM Person.Person;`  
  
 `GO`  
  
 Quando si preme INVIO, viene restituito il set di risultati seguente.  
  
 `BusinessEntityID FirstName    LastName`  
  
 `---------------- ------------ ----------`  
  
 `285              Syed         Abbas`  
  
 `293              Catherine    Abel`  
  
 `(2 row(s) affected)`  
  
 Nonostante la larghezza della colonna `BusinessEntityID` sia soltanto di quattro caratteri, la colonna viene espansa in modo da contenere un nome di colonna più lungo. Per impostazione predefinita, l'output viene troncato a 80 caratteri. Questo valore può essere modificato usando l'opzione **-w** oppure impostando la variabile di scripting SQLCMDCOLWIDTH.  
  
###  <a name="xml-output-format"></a><a name="OutputXML"></a> Formato di output XML  
 L'output XML risultante dalla clausola FOR XML viene restituito non formattato in un flusso continuo.  
  
 Quando è previsto output XML, utilizzare il comando: `:XML ON`.  
  
> [!NOTE]  
>  **sqlcmd** restituisce messaggi di errore nel formato standard. Si noti che anche l'output dei messaggi di errore viene generato nel flusso di testo XML in formato XML. Usando `:XML ON`, **sqlcmd** non visualizza messaggi informativi.  
  
 Per disattivare la modalità XML, usare il comando: `:XML OFF`.  
  
 Il comando GO non deve essere immesso prima dell'esecuzione del comando XML OFF, poiché il comando XML OFF reimposta **sqlcmd** sull'output orientato alle righe.  
  
 Non è possibile combinare dati XML (di flusso) e dati del set di righe. Se il comando XML ON non è stato eseguito prima di un'istruzione Transact-SQL che genera flussi XML, l'output non viene visualizzato correttamente. Dopo che il comando XML ON è stato eseguito, non è possibile eseguire istruzioni Transact-SQL che generano normali set di righe come output.  
  
> [!NOTE]  
>  Il comando `:XML` non supporta l'istruzione SET STATISTICS XML.  
  
###  <a name="json-output-format"></a><a name="OutputJSON"></a> Formato di output JSON  
 Quando è previsto un output JSON, usare il comando seguente: `:XML ON`. In caso contrario, l'output includerà sia il nome di colonna che il testo JSON. Questo output non è valido per JSON.  
  
 Per disattivare la modalità XML, usare il comando: `:XML OFF`.  
  
 Per altre informazioni, vedere [Formato di output XML](#OutputXML) in questo articolo.  

### <a name="using-azure-active-directory-authentication"></a>Uso dell'autenticazione di Azure Active Directory

Esempi di uso dell'autenticazione di Azure Active Directory:

```cmd
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  -l 30
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -G -U bob@contoso.com -P MyAADPassword -l 30
```
  
## <a name="sqlcmd-best-practices"></a>Procedure consigliate per sqlcmd

Utilizzare le procedure seguenti per ottimizzare i livelli di sicurezza ed efficienza.  

- Utilizzare la sicurezza integrata.  

- Usare **-X[1]** negli ambienti automatizzati.

- Proteggere i file di input e di output utilizzando autorizzazioni del file system NTFS appropriate.

- Per incrementare le prestazioni, eseguire il maggior numero di operazioni possibile in un'unica sessione di **sqlcmd** , anziché in una serie di sessioni.

- Per l'esecuzione di batch o query, impostare valori di timeout superiori rispetto al tempo che si prevede sarà necessario per eseguire il batch o la query.

Utilizzare le procedure seguenti per ottimizzare la correttezza:

- Usare **-V16** per registrare tutti i [messaggi con livello di gravità 16](../relational-databases/errors-events/database-engine-error-severities.md#levels-of-severity).  I messaggi con gravità 16 indicano errori generali che possono essere corretti dall'utente.

- Verificare il codice di uscita e la variabile DOS ERRORLEVEL dopo che il processo è stato terminato.  **sqlcmd** restituirà 0 normalmente, in caso contrario imposterà ERRORLEVEL nel modo configurato da **-V**.  In altre parole, ERRORLEVEL non dovrà avere lo stesso valore del numero di errore segnalato da SQL Server. Il numero di errore è un valore specifico di SQL Server corrispondente alla funzione di sistema [ **@@ERROR**](../t-sql/functions/error-transact-sql.md).  ERRORLEVEL è un valore specifico di SQLCMD per indicare il motivo per cui è stato terminato (SQLCMD) e il suo valore viene influenzato specificando l'argomento della riga di comando **-b**.

L'uso di **-V16** in combinazione con il controllo del codice di uscita ed ERRORLEVEL DOS consente di rilevare gli errori negli ambienti automatizzati, in particolare i controlli di qualità prima di una versione di produzione.

## <a name="next-steps"></a>Passaggi successivi

- [Avvio dell'utilità sqlcmd](~/relational-databases/scripting/sqlcmd-start-the-utility.md)
- [Esecuzione di file script Transact-SQL mediante sqlcmd](~/relational-databases/scripting/sqlcmd-run-transact-sql-script-files.md)
- [Utilizzo dell'utilità sqlcmd](~/relational-databases/scripting/sqlcmd-use-the-utility.md)
- [Utilizzo di sqlcmd con variabili di scripting](~/relational-databases/scripting/sqlcmd-use-with-scripting-variables.md)
- [Connessione al Motore di database tramite sqlcmd](~/relational-databases/scripting/sqlcmd-connect-to-the-database-engine.md)
- [Modificare script SQLCMD con l'editor di query](~/relational-databases/scripting/edit-sqlcmd-scripts-with-query-editor.md)
- [Gestire passaggi di processo](~/ssms/agent/manage-job-steps.md)   
- [Creare un passaggio di processo CmdExec](~/ssms/agent/create-a-cmdexec-job-step.md)  

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]