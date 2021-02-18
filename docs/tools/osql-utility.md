---
title: Utilità osql
description: In SQL Server l'utilità osql consente di immettere istruzioni Transact-SQL, procedure di sistema e file di script. Osql usa ODBC per comunicare con il server.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- statements [SQL Server], command prompt
- QUIT command
- Transact-SQL statements, command prompt
- EXIT command
- operating systems [SQL Server], commands
- osql utility [SQL Server]
- stored procedures [SQL Server], command prompt
- scripts [SQL Server], command prompt
- RESET command
- GO command
- command prompt utilities [SQL Server], osql
- CTRL+C command
ms.assetid: cf530d9e-0609-4528-8975-ab8e08e40b9a
author: markingmyname
ms.author: maghan
ms.manageR: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/16/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 41c47f76dd0a4b81d1ccc337b65bf1c8c157b770
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335987"
---
# <a name="osql-utility"></a>Utilità osql

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

L'utilità **osql** consente di immettere istruzioni [!INCLUDE[tsql](../includes/tsql-md.md)] , procedure di sistema e file script. Questa utilità comunica con il server tramite ODBC.  
  
> [!IMPORTANT]  
>  Questa funzionalità verrà rimossa nelle versioni future di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Evitare pertanto di utilizzarla in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni che attualmente utilizzano questa funzionalità. In alternativa, usare **sqlcmd** . Per altre informazioni, vedere [sqlcmd Utility](../tools/sqlcmd-utility.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
osql  
[-?] |  
[-L] |  
[  
  {  
     {-Ulogin_id [-Ppassword]} | -E }  
     [-Sserver_name[\instance_name]] [-Hwksta_name] [-ddb_name]  
     [-ltime_out] [-ttime_out] [-hheaders]  
     [-scol_separator] [-wcolumn_width] [-apacket_size]  
     [-e] [-I] [-D data_source_name]  
     [-ccmd_end] [-q "query"] [-Q"query"]  
     [-n] [-merror_level] [-r {0 | 1}]  
     [-iinput_file] [-ooutput_file] [-p]  
     [-b] [-u] [-R] [-O]  
]  
```  
  
## <a name="arguments"></a>Argomenti  
 **-?**  
 Visualizza il riepilogo della sintassi delle opzioni di **osql** .  
  
 **-L**  
 Elenca i server configurati localmente e i nomi dei server che trasmettono in rete tramite broadcast.  
  
> [!NOTE]  
>  A causa della natura delle trasmissioni in rete, è possibile che **osql** non riceva una risposta tempestiva da tutti i server e pertanto che l'elenco di server restituito sia diverso per ogni chiamata di questa opzione.  
  
 **-U** _login_id_  
 ID di accesso dell'utente. Negli ID di accesso viene fatta distinzione tra maiuscole e minuscole.  
  
 **-P** _password_  
 Password specificata dall'utente. Se si omette l'opzione **-P** , **osql** richiede una password. Se l'opzione **-P** viene specificata alla fine del prompt dei comandi senza indicare una password, **osql** usa la password predefinita (NULL).  
  
> [!IMPORTANT]  
>  Non usare una password vuota. Usare una password complessa. Per altre informazioni, vedere [Strong Passwords](../relational-databases/security/strong-passwords.md).  
  
 Per le password viene fatta distinzione tra maiuscole e minuscole.  
  
 La variabile di ambiente OSQLPASSWORD consente di impostare una password predefinita per la sessione corrente. Non è pertanto necessario specificare una password a livello di codice nei file batch.  
  
 Se si specifica l'opzione **-P** e si omette la password, **osql** verifica innanzitutto la variabile OSQLPASSWORD. Se non viene impostato alcun valore, **osql** usa la password predefinita NULL. Nell'esempio seguente viene impostata la variabile OSQLPASSWORD al prompt dei comandi e quindi si accede all'utilità **osql** :  
  
```  
C:\>SET OSQLPASSWORD=abracadabra  
C:\>osql   
```  
  
> [!IMPORTANT]  
>  Per nascondere la password, non specificare l'opzione **-P** in combinazione con l'opzione **-U** . Dopo aver specificato il comando **osql** con **-U** e altre opzioni (non specificare **-P**), premere Invio. Il comando **osql** richiederà l'immissione di una password. Questo metodo garantisce che la password venga nascosta durante l'immissione.  
  
 **-E**  
 Utilizza una connessione trusted anziché richiedere una password.  
  
 **-S** _server\_name_[ **\\** _instance\_name_]  
 Specifica l'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] alla quale connettersi. Specificare il *nome_server* per connettersi all'istanza predefinita di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] su tale server. Specificare _server\_name_ **\\** _instance\_name_ per connettersi a un'istanza denominata di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] su tale server. Se non si specifica alcun server, **osql** si connette all'istanza predefinita di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] nel computer locale. Questa opzione è obbligatoria per l'esecuzione di **osql** da un computer remoto sulla rete.  
  
 **-H** _wksta_name_  
 Nome della workstation. Il nome della workstation viene archiviato in **sysprocesses.hostname** ed è visualizzato tramite **sp_who**. Se omesso, viene utilizzato il nome del computer corrente.  
  
 **-d** _db_name_  
 Esegue un'istruzione USE *db_name* all'avvio di **osql**.  
  
 **-l** _time_out_  
 Specifica il numero di secondi prima del timeout di accesso a **osql** . Il valore predefinito per il timeout di accesso a **osql** è otto secondi.  
  
 **-t** _time_out_  
 Specifica il numero di secondi prima del timeout del comando. Se per *time_out* non viene specificato alcun valore, ai comandi non viene associato alcun timeout.  
  
 **-h** _headers_  
 Specifica il numero di righe da stampare tra le intestazioni delle colonne. Per impostazione predefinita, le intestazioni vengono stampate una volta per ogni set di risultati delle query. Utilizzare -1 per non stampare alcuna intestazione. Se si usa -1, non inserire spazi tra il parametro e l'impostazione ( **-h-1** e non **-h -1**).  
  
 **-s** _col_separator_  
 Specifica il carattere separatore di colonne che, per impostazione predefinita, è uno spazio vuoto. Per utilizzare caratteri con un significato speciale per il sistema operativo (ad esempio, | ; & < >), racchiudere il carattere tra virgolette doppie (").  
  
 **-w** _column_width_  
 Consente di impostare la larghezza della schermata per l'output. Il valore predefinito è 80 caratteri. Se una riga di output raggiunge la larghezza massima della schermata, viene suddivisa su più righe.  
  
 **-a** _packet_size_  
 Consente di richiedere un pacchetto di dimensioni diverse. I valori validi per *packet_size* sono compresi tra 512 e 65535. Il valore predefinito per **osql** è il valore predefinito del server. Aumentando le dimensioni del pacchetto si possono ottenere miglioramenti delle prestazioni di esecuzione di script di grandi dimensioni, che includono numerose istruzioni SQL tra i comandi GO. [!INCLUDE[msCoName](../includes/msconame-md.md)] indicano che, per le operazioni di copia bulk, l'impostazione che garantisce le prestazioni più veloci è in genere 8.192. È possibile richiedere una dimensione maggiore del pacchetto, ma se questa non è disponibile **osql** usa il valore predefinito del server.  
  
 **-e**  
 Esegue l'eco dell'input.  
  
 **-I**  
 Attiva l'opzione di connessione QUOTED_IDENTIFIER.  
  
 **-D** _data_source_name_  
 Stabilisce una connessione a un'origine dei dati ODBC definita mediante il driver ODBC per [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. La connessione **osql** usa le opzioni specificate nell'origine dei dati.  
  
> [!NOTE]  
>  Tale opzione non può essere utilizzata con origini dei dati definite per altri driver.  
  
 **-c** _cmd_end_  
 Specifica il carattere di terminazione del comando. Per impostazione predefinita, i comandi vengono terminati e inviati a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tramite l'immissione di GO su una riga a sé stante. Se si reimposta il carattere di terminazione del comando, non utilizzare parole riservate di [!INCLUDE[tsql](../includes/tsql-md.md)] o caratteri con un significato speciale per il sistema operativo, indipendentemente dal fatto che siano preceduti da una barra rovesciata.  
  
 **-q "** _query_ **"**  
 Esegue una query all'avvio di **osql** senza uscire da **osql** al termine della query. Si noti che l'istruzione della query non dovrebbe includere l'istruzione GO. Se si esegue una query da un file batch, è possibile utilizzare variabili in formato %variabile o variabili di ambiente in formato %variabile%. Ad esempio:  
  
```  
SET table=sys.objects  
osql -E -q "select name, object_id from %table%"  
```  
  
 Racchiudere la query tra virgolette doppie e utilizzare le virgolette singole per altri elementi inclusi nella query.  
  
 **-Q"** _query_ **"**  
 Esegue una query ed esce immediatamente da **osql**. Racchiudere la query tra virgolette doppie e utilizzare le virgolette singole per altri elementi inclusi nella query.  
  
 **-n**  
 Rimuove la numerazione e il simbolo del prompt (>) dalle righe di input.  
  
 **-m** _error_level_  
 Personalizza la visualizzazione dei messaggi di errore. Per gli errori con livello di gravità pari o superiore a quello specificato vengono visualizzati il numero, lo stato e il livello di errore del messaggio. Per gli errori con livelli di gravità inferiori a quello specificato non viene visualizzato nulla. Usare **-1** per specificare che con i messaggi, anche quelli informativi, devono essere restituite anche tutte le rispettive intestazioni. Se si usa **-1**, non inserire spazi tra il parametro e l'impostazione ( **-m-1** e non **-m -1**).  
  
 **-r** { **0**| **1**}  
 Reindirizza l'output dei messaggi sullo schermo (**stderr**). Se il parametro viene omesso o si specifica **0**, vengono reindirizzati solo i messaggi di errore con gravità pari o superiore a 11. Se si specifica **1**, viene reindirizzato l'output di tutti i messaggi (incluso quello dell'istruzione "print").  
  
 **-i** _input_file_  
 Identifica il file che include un batch di istruzioni SQL o stored procedure. Anziché **\<** -i **, è possibile utilizzare l'operatore di confronto minore di (** ).  
  
 **-o** _output_file_  
 Identifica il file che riceve l'output di **osql**. Anziché **>** -o **, è possibile usare l'operatore di confronto maggiore di (** ).  
  
 Se *input_file* non è un file Unicode e l'opzione **-u** non è specificata, *output_file* viene archiviato in formato OEM. Se *input_file* è un file Unicode o l'opzione **-u** è specificata, *output_file* viene archiviato in formato Unicode.  
  
 **-p**  
 Stampa le statistiche sulle prestazioni.  
  
 **-b**  
 Specifica che, in caso di errore, **osql** termini, restituendo un valore DOS ERRORLEVEL. Il valore restituito alla variabile DOS ERRORLEVEL è 1 se al messaggio di errore di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] è associato un livello di gravità maggiore o uguale a 11. In caso contrario, il valore restituito è 0. [!INCLUDE[msCoName](../includes/msconame-md.md)] I file batch MS-DOS consentono di verificare il valore di DOS ERRORLEVEL, nonché di gestire correttamente l'errore.  
  
 **-u**  
 Specifica l'archiviazione di *output_file* in formato Unicode, indipendentemente dal formato di *input_file*.  
  
 **-R**  
 Specifica che il driver ODBC di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilizza le impostazioni del client per convertire i dati relativi a valuta, data e ora in dati di tipo carattere.  
  
 **-O**  
 Disattiva determinate caratteristiche di **osql** in modo che il funzionamento dell'utilità corrisponda a quello delle precedenti versioni di **isql**. Vengono disattivate le funzionalità seguenti:  
  
-   Elaborazione batch EOF  
  
-   Adattamento automatico della larghezza della console  
  
-   Messaggi estesi  
  
 L'opzione imposta inoltre il valore predefinito di DOS ERRORLEVEL su -1.  
  
> [!NOTE]  
>  Le opzioni **-n**, **-O** e **-D** non sono più supportate da **osql**.  
  
## <a name="remarks"></a>Osservazioni  
 L'utilità **osql** viene avviata direttamente dal sistema operativo con le opzioni elencate di seguito per le quali la distinzione tra maiuscole e minuscole è rilevante. Dopo l'avvio, **osql** accetta istruzioni SQL e le invia a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in modo interattivo. I risultati vengono formattati e visualizzati sullo schermo (**stdout**). Per uscire da **osql** usare QUIT o EXIT.  
  
 Se all'avvio di **osql** non si specifica un nome utente, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] verifica le variabili di ambiente e usa tali valori, ad esempio **osqluser=(** _utente_ **)** o **osqlserver=(** _server_ **)** . Se non sono state impostate variabili di ambiente, viene utilizzato il nome utente della workstation. Se non si specifica un server, viene utilizzato il nome della workstation.  
  
 Se non si specifica l'opzione **-U** né l'opzione **-P** , [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] cerca di stabilire la connessione tramite la modalità di autenticazione di [!INCLUDE[msCoName](../includes/msconame-md.md)] Windows. L'autenticazione si basa sull'account di [!INCLUDE[msCoName](../includes/msconame-md.md)] Windows dell'utente che esegue **osql**.  
  
 L'utilità **osql** usa l'API ODBC. Le impostazioni predefinite del driver ODBC di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] per le opzioni di connessione ISO di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Per ulteriori informazioni, vedere Effects of ANSI Options (informazioni in lingua inglese).  
  
> [!NOTE]  
>  L'utilità **osql** non supporta i tipi di dati CLR definiti dall'utente. Per elaborare questi tipi di dati, è necessario usare l'utilità **sqlcmd** . Per altre informazioni, vedere [sqlcmd Utility](../tools/sqlcmd-utility.md).  
  
## <a name="osql-commands"></a>Comandi OSQL  
 Oltre alle istruzioni [!INCLUDE[tsql](../includes/tsql-md.md)] all'interno di **osql**, sono disponibili anche i comandi seguenti.  
  
|Comando|Descrizione|  
|-------------|-----------------|  
|GO|Esegue tutte le istruzioni immesse dopo l'ultimo comando GO.|  
|RESET|Cancella tutte le istruzioni immesse.|  
|QUIT o EXIT( )|Consente di uscire da **osql**.|  
|CTRL+C|Termina una query senza uscire da **osql**.|  
  
> [!NOTE]  
>  I comandi !! ed ED non sono più supportati da **osql**.  
  
 I caratteri di terminazione dei comandi GO (predefinito), RESET, EXIT, QUIT e CTRL+C vengono riconosciuti solo se specificati all'inizio della riga, subito dopo il prompt **osql** .  
  
 GO indica sia la fine di un batch che l'esecuzione di eventuali istruzioni [!INCLUDE[tsql](../includes/tsql-md.md)] memorizzate nella cache. Quando si preme INVIO alla fine di ogni riga di input, **osql** memorizza nella cache le istruzioni della riga. Quando si preme INVIO dopo aver digitato GO, tutte le istruzioni memorizzate nella cache vengono inviate in batch a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 La versione corrente dell'utilità **osql** funziona come se alla fine di tutti gli script eseguiti fosse presente un comando GO implicito. Di conseguenza vengono eseguite tutte le istruzioni dello script.  
  
 Terminare un comando digitando una riga che inizia con un carattere di terminazione del comando. Per specificare quante volte eseguire il comando, digitare un numero dopo il carattere di terminazione. Ad esempio, per eseguire il comando seguente 100 volte, digitare:  
  
```  
SELECT x = 1  
GO 100  
```  
  
 I risultati vengono stampati solo una volta al termine dell'esecuzione. **osql** non accetta più di 1.000 caratteri per riga. Le istruzioni di grandi dimensioni devono essere suddivise su più righe.  
  
 Le funzionalità per la chiamata di comandi di Windows consentono di richiamare e modificare le istruzioni **osql** . Per cancellare il buffer di query esistente digitare RESET.  
  
 Durante l'esecuzione di stored procedure, **osql** stampa una riga vuota tra ogni set di risultati di un batch. Inoltre, il messaggio "Righe interessate: 0" non viene visualizzato se non è riferibile all'istruzione eseguita.  
  
## <a name="using-osql-interactively"></a>Uso interattivo di osql  
 Per usare **osql** interattivamente, al prompt dei comandi digitare il comando **osql** e le opzioni desiderate.  
  
 Per leggere un file contenente una query, ad esempio Stores.qry, da eseguire con **osql** , digitare un comando analogo al seguente:  
  
```  
osql -E -i stores.qry  
```  
  
 Per leggere un file contenente una query, ad esempio Titles.qry, e indirizzare i risultati a un altro file, digitare un comando simile al seguente:  
  
```  
osql -E -i titles.qry -o titles.res  
```  
  
> [!IMPORTANT]  
>  Se possibile, usare l'opzione **-E**(connessione trusted).  
  
 Quando si usa **osql** interattivamente, è possibile leggere un file del sistema operativo nel buffer dei comandi mediante **:r**_file\_name_. In questo modo lo script SQL in *file_name* viene inviato direttamente al server come batch singolo.  
  
> [!NOTE]  
>  Quando si usa **osql**, in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] la presenza del separatore di batch GO in un file script SQL viene considerata un errore di sintassi.  
  
## <a name="inserting-comments"></a>Inserimento di commenti  
 L'utilità [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] osql **consente di inserire commenti in un'istruzione Transact-SQL inviata a**. Sono supportati due tipi di indicatori di commento: `--` e `/*...*/`.  
  
## <a name="using-exit-to-return-results-in-osql"></a>Utilizzo di EXIT per la restituzione dei risultati in osql  
 Il risultato di un'istruzione SELECT può essere usato come valore restituito da **osql**. Se numerica, l'ultima colonna dell'ultima riga di risultati viene convertita in un valore integer di 4 byte (long). MS-DOS passa il byte di ordine inferiore al processo padre o al livello di errore del sistema operativo. Windows passa l'intero valore intero di 4 byte. La sintassi è:  
  
```  
EXIT ( < query > )  
```  
  
 Ad esempio:  
  
```  
EXIT(SELECT @@ROWCOUNT)  
```  
  
 È inoltre possibile includere il parametro EXIT in un file batch. Ad esempio:  
  
```  
osql -E -Q "EXIT(SELECT COUNT(*) FROM '%1')"  
```  
  
 L'utilità **osql** passa al server tutti gli elementi racchiusi tra parentesi **()** senza modificarli. Se una stored procedure di sistema seleziona un set e restituisce un valore, viene restituita solo la selezione. Se non si specifica alcun elemento tra le parentesi dell'istruzione EXIT **()** , viene eseguito tutto ciò che la precede nel batch e l'operazione viene quindi terminata senza restituire alcun valore.  
  
 L'istruzione EXIT può avere quattro formati:  
  
-   EXIT  
  
> [!NOTE]  
>  L'utilità non esegue il batch, viene chiusa immediatamente e non restituisce alcun valore.  
  
-   EXIT **()**  
  
> [!NOTE]  
>  L'utilità esegue il batch, viene chiusa e non restituisce alcun valore.  
  
-   EXIT **(** _query_ **)**  
  
> [!NOTE]  
>  L'utilità esegue il batch, inclusa la query, quindi viene chiusa dopo aver restituito i risultati della query.  
  
-   RAISERROR con stato 127.  
  
> [!NOTE]  
>  Se in uno script **osql** si usa RAISERROR e viene generato un errore con stato 127, l'utilità **osql** viene chiusa e al client viene restituito l'ID di messaggio. Ad esempio:  
  
```  
RAISERROR(50001, 10, 127)  
```  
  
 Questo errore provoca l'interruzione dello script **osql** e la restituzione dell'ID di messaggio 50001 al client.  
  
 I valori restituiti compresi tra -1 e -99 sono riservati a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. In **osql** vengono usati i valori seguenti:  
  
-   -100  
  
     Si è verificato un errore prima di selezionare il valore restituito.  
  
-   -101  
  
     Selezionando il valore restituito non si sono trovate righe.  
  
-   -102  
  
     Si è verificato un errore di conversione durante la selezione del valore restituito.  
  
## <a name="displaying-money-and-smallmoney-data-types"></a>Visualizzazione dei tipi di dati money e smallmoney  
 **osql** visualizza i tipi di dati **money** e **smallmoney** con due cifre decimali sebbene in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] il valore venga archiviato internamente con quattro cifre decimali. Si consideri l'esempio seguente:  
  
```  
SELECT CAST(CAST(10.3496 AS money) AS decimal(6, 4))  
GO  
```  
  
 Questa istruzione restituisce il risultato `10.3496`, che indica che il valore viene archiviato con tutte le cifre decimali.  
  
## <a name="see-also"></a>Vedere anche  
 [Commento &#40;MDX&#41;](../mdx/comment-mdx.md)   
 [-- &#40;Comment&#41; &#40;MDX&#41;](../mdx/comment-mdx-operator-reference.md)   
 [CAST e CONVERT &#40;Transact-SQL&#41;](../t-sql/functions/cast-and-convert-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../t-sql/language-elements/raiserror-transact-sql.md)  
  
  
