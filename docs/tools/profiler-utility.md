---
title: Utilità profiler
description: L'utilità Profiler avvia lo strumento SQL Server Profiler. Gli argomenti facoltativi consentono di controllare la modalità di avvio dell'applicazione.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- command prompt utilities [SQL Server], profiler90 utility
- profiler90 utility
- Profiler [SQL Server Profiler], starting
- SQL Server Profiler, starting
- starting SQL Server Profiler
ms.assetid: e91c30a9-0d29-4f84-bcb8-e8fb62afadda
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ab40ea190dab072d7415ded1f399cfa4a4b78568
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335947"
---
# <a name="profiler-utility"></a>Utilità profiler
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  L'utilità **profiler** consente di avviare lo strumento [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] . Gli argomenti facoltativi elencati di seguito in questo argomento consentono di controllare la modalità di avvio dell'applicazione.  
  
> [!NOTE]  
>  L'utilità **profiler** non viene usata per lo scripting delle tracce. Per altre informazioni, vedere [SQL Server Profiler](../tools/sql-server-profiler/sql-server-profiler.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
profiler  
    [ /? ] |  
[  
{  
{ /U login_id [ /P password ] }  
| /E  
}  
{[ /S sql_server_name ] | [ /A analysis_services_server_name ] }  
[ /D database ]  
[ /T "template_name" ]  
[ /B { "trace_table_name" } ]  
{ [/F "filename" ] | [ /O "filename" ] }  
[ /L locale_ID ]  
[ /M "MM-DD-YY hh:mm:ss" ]  
[ /R ]  
[ /Z file_size ]  
]  
```  
  
## <a name="arguments"></a>Argomenti  
 **/?**  
 Visualizza il riepilogo della sintassi degli argomenti di **profiler** .  
  
 **/U** *login_id*  
 ID di accesso utente per l'autenticazione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Per gli ID di accesso la distinzione tra maiuscole e minuscole è rilevante.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteWinAuthentication](../includes/ssnotewinauthentication-md.md)].  
  
 **/P** *password*  
 Specifica una password definita dall'utente per l'autenticazione di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 **/E**  
 Specifica la connessione tramite l'autenticazione di Windows e le credenziali dell'utente corrente.  
  
 **/S**  *sql_server_name*  
 Specifica un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Profiler viene automaticamente connesso al server specificato usando le informazioni di autenticazione definite nelle opzioni **/U** e **/P** o **/E** . Usare **/S** *sql_server_name*\\*instance_name* per connettersi a un'istanza denominata di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 **/A**  *analysis_services_server_name*  
 Consente di specificare un'istanza di Analysis Services. Profiler viene automaticamente connesso al server specificato usando le informazioni di autenticazione definite nelle opzioni **/U** e **/P** o **/E** . Usare **/A** *analysis_services_server_name\instance_name* per connettersi a un'istanza denominata di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 **/D** *database*  
 Specifica il nome del database da utilizzare con la connessione. Se non si specifica alcun database, questa opzione selezionerà il database predefinito per l'utente specificato.  
  
 **/B "** *trace_table_name* **"**  
 Specifica una tabella di traccia da caricare all'avvio di SQL Profiler. Assieme alla tabella è necessario specificare il database, l'utente oppure lo schema.  
  
 **/T"** *template_name* **"**  
 Specifica il modello che verrà caricato per configurare la traccia. È necessario racchiudere il nome del modello tra virgolette. Il nome del modello deve trovarsi nella directory dei modelli di sistema oppure in quella dei modelli utente. Se in entrambe le directory esistono due modelli con lo stesso nome, verrà caricato il modello incluso nella directory di sistema. Se non esiste alcun modello con il nome specificato, verrà caricato il modello standard. Si noti che l'estensione di file tdf del modello non dovrà essere specificata come parte del *template_name*. Ad esempio:  
  
```  
/T "standard"  
```  
  
 **/F"** *filename* **"**  
 Specifica il percorso e il nome file di un file di traccia da caricare all'avvio di SQL Profiler. È necessario racchiudere l'intero percorso e il nome file tra virgolette. Questa opzione non può essere usata in combinazione con **/O**.  
  
 **/O "** *filename*  **"**  
 Specifica il percorso e il nome file di un file nel quale verranno scritti i risultati della traccia. È necessario racchiudere l'intero percorso e il nome file tra virgolette. Questa opzione non può essere usata in combinazione con **/F**.  
  
 **/L** *locale_ID*  
 Non disponibile.  
  
 **/M "** *MM-DD-YY hh:mm:ss* **"**  
 Specifica la data e l'ora in cui verrà arrestata la traccia. È necessario racchiudere l'ora di arresto tra virgolette. Specificare l'ora di arresto in base ai parametri descritti nella tabella seguente.  
  
|Parametro|Definizione|  
|---------------|----------------|  
|MM|Mese nel formato a 2 cifre|  
|GG|Giorno nel formato a 2 cifre|  
|YY|Anno nel formato a 2 cifre|  
|hh|Ora a 2 cifre nel formato a 24 ore|  
|MM|Minuti nel formato a 2 cifre|  
|ss|Secondi nel formato a 2 cifre|  
  
> [!NOTE]  
>  Il formato "MM-DD-YY hh:mm:ss" può essere utilizzato solo se l'opzione **Visualizza valori di data e ora in base alle impostazioni internazionali** è abilitata in [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]. Se questa opzione non è abilitata, è necessario utilizzare il formato di data e ora "YYYY-MM-DD hh:mm:ss".  
  
 **/R**  
 Abilita il rollover del file di traccia.  
  
 **/Z**  *file_size*  
 Specifica le dimensioni del file di traccia espresse in megabyte (MB). Le dimensioni predefinite sono pari a 5 MB. Se si abilita il rollover, a tutti i file di rollover verrà applicato il valore limite specificato in questo argomento.  
  
## <a name="remarks"></a>Osservazioni  
 Per avviare la traccia con un modello specifico, usare contemporaneamente le opzioni **/S** e **/T** . Per avviare una traccia utilizzando il modello Standard in MyServer\MyInstance, ad esempio, al prompt dei comandi digitare quanto segue:  
  
```  
profiler /S MyServer\MyInstance /T "Standard"  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Guida di riferimento alle utilità del prompt dei comandi &#40;motore di database&#41;](../tools/command-prompt-utility-reference-database-engine.md)  
  
  
