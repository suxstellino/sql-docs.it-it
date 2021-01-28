---
title: Utilità bcp
description: L'utilità bcp, o programma per la copia bulk, crea copie bulk di dati tra un'istanza di SQL Server e un file di dati in un formato specificato dall'utente.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- bcp utility [SQL Server]
- exporting data
- row exporting [SQL Server]
- copying data [SQL Server], bcp utility
- command prompt utilities [SQL Server], bcp
- tables [SQL Server], importing data
- column importing [SQL Server]
- bcp utility [SQL Server], command options
- file exporting [SQL Server]
- bulk copy [SQL Server]
- bcp utility [SQL Server], about bcp utility
- tables [SQL Server], exporting data
- row importing [SQL Server]
- importing data, bcp utility
- file importing [SQL Server]
- column exporting [SQL Server]
ms.assetid: c0af54f5-ca4a-4995-a3a4-0ce39c30ec38
author: markingmyname
ms.author: maghan
ms.reviewer: v-daenge
ms.custom: seo-lt-2019
ms.date: 09/11/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 2be72374a13dbedb444b2661cf0e53a0d555d98c
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688977"
---
# <a name="bcp-utility"></a>Utilità bcp

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

> Per l'uso di bcp in Linux, vedere le [istruzioni per l'installazione di sqlcmd e bcp in Linux](../linux/sql-server-linux-setup-tools.md).
>
> Per informazioni dettagliate sull'uso di bcp con Azure Synapse Analytics, vedere [Caricare dati con bcp](/azure/sql-data-warehouse/sql-data-warehouse-load-with-bcp).

L'utilità del **p** rogramma di **c** opia **b** ulk (**bcp**) esegue operazioni di copia bulk di dati tra un'istanza di [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] e un file di dati in un formato specificato dall'utente. L'utilità **bcp** può essere usata per importare un numero elevato di nuove righe nelle tabelle di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] o per esportare dati dalle tabelle in file di dati. Ad eccezione del caso in cui venga usata con l'opzione **queryout** , l'utilità non richiede alcuna conoscenza di [!INCLUDE[tsql](../includes/tsql-md.md)]. Per importare dati in una tabella, è necessario utilizzare un file di formato creato per la tabella specifica oppure conoscere approfonditamente la struttura della tabella e i tipi di dati validi per le relative colonne.  

![Icona di collegamento all'argomento](../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") Per informazioni sulle convenzioni adottate per la sintassi di **bcp**, vedere [Convenzioni della sintassi Transact-SQL &#40;Transact-SQL&#41;](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  

> [!NOTE]
> Se si esegue il backup dei dati con **bcp** , creare un file di formato per registrare il formato dei dati. I file di dati di **bcp** **non includono** alcuna informazione sullo schema o sul formato. Di conseguenza, se si elimina una tabella o una vista e non è disponibile un file di formato, può non essere possibile importare i dati.

## <a name="download-the-latest-version-of-bcp-utility"></a>Scaricare la versione più recente della utilità bcp

**[![Download bcp per x64](../ssdt/media/download.png) Download di Microsoft Command Line Utilities 15 per SQL Server (x64)](https://go.microsoft.com/fwlink/?linkid=2142258)**
<br>**[![Download bcp per x86](../ssdt/media/download.png) Download di Microsoft Command Line Utilities 15 per SQL Server (x86)](https://go.microsoft.com/fwlink/?linkid=2142257)**

Gli strumenti da riga di comando dipendono dalla disponibilità generale (GA), ma vengono rilasciati con il pacchetto di installazione per [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)].

### <a name="version-information"></a>Informazioni sulla versione

Numero di versione: 15.0.2 <br>
Numero di build: 15.0.2000.5<br>
Data di rilascio: 11 settembre 2020

La nuova versione di SQLCMD supporta l'autenticazione di Azure AD e include anche il supporto di Multi-Factor Authentication (MFA) per il database SQL, Azure Synapse Analytics e Always Encrypted.
Il nuovo BCP supporta l'autenticazione di Azure AD e include anche il supporto di Multi-Factor Authentication (MFA) per il database SQL e Azure Synapse Analytics.

### <a name="system-requirements"></a>Requisiti di sistema

Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Server 2008, Windows Server 2008 R2, Windows Server 2008 R2 SP1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Server 2019

Per questo componente sono necessari sia [Windows Installer 4.5](https://www.microsoft.com/download/details.aspx?id=8483) che [Microsoft ODBC Driver 17 for SQL Server](../connect/odbc/download-odbc-driver-for-sql-server.md).

Per controllare la versione di BCP, eseguire il comando `bcp /v` e verificare che sia in uso la versione 15.0.2000.5 o successiva.

<table><th>Sintassi</th><tr><td><pre>
bcp [<a href="#db_name">database_name.</a>] <a href="#schema">schema</a>.{<a href="#tbl_name">table_name</a> | <a href="#vw_name">view_name</a> | <a href="#query">"query"</a>}
    {<a href="#in">in</a> <a href="#data_file">data_file</a> | <a href="#out">out</a> <a href="#data_file">data_file</a> | <a href="#qry_out">queryout</a> <a href="#data_file">data_file</a> | <a href="#format">format</a> <a href="#format">nul</a>}
<a>                                                                                                         </a>
    [<a href="#a">-a packet_size</a>]
    [<a href="#b">-b batch_size</a>]
    [<a href="#c">-c</a>]
    [<a href="#C">-C { ACP | OEM | RAW | code_page } </a>]
    [<a href="#d">-d database_name</a>]
    [<a href="#D">-D</a>]
    [<a href="#e">-e err_file</a>]
    [<a href="#E">-E</a>]
    [<a href="#f">-f format_file</a>]
    [<a href="#F">-F first_row</a>]
    [<a href="#G">-G Azure Active Directory Authentication</a>]
    [<a href="#h">-h"hint [,...n]"</a>]
    [<a href="#i">-i input_file</a>]
    [<a href="#k">-k</a>]
    [<a href="#K">-K application_intent</a>]
    [<a href="#l">-l login_timeout</a>]
    [<a href="#L">-L last_row</a>]
    [<a href="#m">-m max_errors</a>]
    [<a href="#n">-n</a>]
    [<a href="#N">-N</a>]
    [<a href="#o">-o output_file</a>]
    [<a href="#P">-P password</a>]
    [<a href="#q">-q</a>]
    [<a href="#r">-r row_term</a>]
    [<a href="#R">-R</a>]
    [<a href="#S">-S [server_name[\instance_name]</a>]
    [<a href="#t">-t field_term</a>]
    [<a href="#T">-T</a>]
    [<a href="#U">-U login_id</a>]
    [<a href="#v">-v</a>]
    [<a href="#V">-V (80 | 90 | 100 | 110 | 120 | 130 ) </a>]
    [<a href="#w">-w</a>]
    [<a href="#x">-x</a>]
</pre></td></tr></table>

## <a name="arguments"></a>Argomenti

 _**data\_file**_<a name="data_file"></a>  
 Percorso completo del file di dati. Quando si esegue un'importazione bulk dei dati in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], il file di dati include i dati da copiare nella tabella o nella vista specificata. Quando si esegue un'esportazione bulk dei dati da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], il file di dati include i dati copiati dalla tabella o dalla vista. Il percorso può essere costituito da un numero di caratteri compreso tra 1 e 255. Il file di dati può contenere un massimo di 2^63 - 1 righe.  

 _**database\_name**_<a name="db_name"></a>  
 Nome del database in cui si trova la tabella o la vista specificata. Se tale valore non è specificato, il database è quello predefinito dell'utente.  

 È anche possibile specificare in modo esplicito il nome del database con **-d**.  

 **in** *data_file* \| **out** *data_file* \| **queryout** *data_file* \| **format nul**  
 Specifica la direzione della copia bulk nel modo seguente:  
  
- **in**<a name="in"></a> esegue la copia da un file nella tabella o nella vista del database.  
  
- **out**<a name="out"></a> esegue la copia dalla tabella o dalla vista del database in un file. Se si specifica un file esistente, il file viene sovrascritto. Durante l'estrazione dei dati, l'utilità **bcp** rappresenta una stringa vuota come Null e una stringa Null come stringa vuota.  
  
- **queryout**<a name="qry_out"></a> esegue la copia da una query e deve essere specificata solo in caso di copia bulk di dati da una query.  
  
- **format**<a name="format"></a> crea un file di formato basato sull'opzione specificata ( **-n**, **-c**, **-w** o **-N**) e sui delimitatori della tabella o della vista. Durante la copia bulk dei dati, il comando **bcp** può fare riferimento a un file di formato e si può quindi evitare di immettere nuovamente le informazioni sul formato in modo interattivo. L'opzione **format** richiede l'opzione **-f** . Se si crea un file di formato XML, è necessaria anche l'opzione **-x** . Per altre informazioni, vedere [Creazione di un file di formato &#40;SQL Server&#41;](../relational-databases/import-export/create-a-format-file-sql-server.md). È necessario specificare **nul** come valore (**format nul**).  
  
 _**owner**_<a name="schema"></a>  
 Nome del proprietario della tabella o della vista. *owner* è facoltativo se l'utente che esegue l'operazione è il proprietario della tabella o della vista specificata. Se *owner* non viene specificato e l'utente che esegue l'operazione non è il proprietario della tabella o della vista specificata, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] restituisce un messaggio di errore e l'operazione viene annullata.  
  
**"** _**query**_ **"** <a name="query"></a> Query [!INCLUDE[tsql](../includes/tsql-md.md)] che restituisce un set di risultati. Se la query restituisce più set di risultati, nel file di dati viene copiato solo il primo set, mentre quelli successivi vengono ignorati. Racchiudere la query tra virgolette doppie e utilizzare le virgolette singole per altri elementi inclusi nella query. L'opzione **queryout** deve essere specificata solo in caso di copia bulk di dati da una query.  
  
 La query può fare riferimento a una stored procedure a condizione che tutte le tabelle cui viene fatto riferimento nella stored procedure siano disponibili prima dell'esecuzione dell'istruzione bcp. Se, ad esempio, la stored procedure genera una tabella temporanea, l'istruzione **bcp** ha esito negativo poiché la tabella è disponibile solo in fase di esecuzione e non nel momento in cui viene eseguita l'istruzione. In questo caso, è opportuno inserire i risultati della stored procedure in una tabella e usare **bcp** per copiare i dati dalla tabella in un file di dati.  
  
 _**table\_name**_<a name="tbl_name"></a>  
 Nome della tabella di destinazione per l'importazione di dati in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (**in**) e della tabella di origine per l'esportazione di dati da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (**out**).  
  
 _**view\_name**_<a name="vw_name"></a>   
 Nome della vista di destinazione per la copia di dati in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (**in**) e della vista di origine per la copia di dati da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (**out**). È possibile utilizzare come viste di destinazione solo quelle in cui tutte le colonne fanno riferimento alla stessa tabella. Per altre informazioni sulle restrizioni per la copia di dati nelle viste, vedere [INSERT &#40;Transact-SQL&#41;](../t-sql/statements/insert-transact-sql.md).  
  
 **-a** _**packet\_size**_<a name="a"></a>  
 Specifica il numero di byte inviati al e dal server per ogni pacchetto di rete. È possibile impostare un'opzione di configurazione del server usando [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] o la stored procedure di sistema **sp_configure** . Questa opzione, tuttavia, ha la precedenza sull'opzione di configurazione del server. Il valore di *packet_size* può essere compreso tra 4096 byte e 65535 byte. Il valore predefinito è 4096.  
  
 Le prestazioni delle operazioni di copia bulk migliorano con l'aumentare delle dimensioni del pacchetto. Se è richiesta una dimensione del pacchetto maggiore di quella consentita, viene utilizzato il valore predefinito. Le statistiche sulle prestazioni generate dall'utilità **bcp** indicano le dimensioni del pacchetto in uso.  
  
 **-b** _**batch\_size**_<a name="b"></a>  
 Specifica il numero di righe per ogni batch di dati importati. Ogni batch viene importato e registrato come transazione distinta che importa l'intero batch prima del commit. Per impostazione predefinita, tutte le righe incluse nel file di dati vengono importate come un unico batch. Per distribuire le righe tra più batch, specificare un valore di *batch_size* inferiore al numero di righe presenti nel file di dati. Se la transazione per un batch non viene completata correttamente, viene eseguito il rollback solo degli inserimenti dal batch corrente. I batch già importati dalle transazioni di cui è stato eseguito il commit non sono influenzati in caso di esito negativo.  
  
 Non usare questa opzione in combinazione con l'opzione **-h "** ROWS_PER_BATCH **=** _bb_ **"** .  
 
 **-c**<a name="c"></a>  
 Esegue l'operazione utilizzando un tipo di dati carattere. Questa opzione non determina la visualizzazione di una richiesta per ogni campo, ma usa **char** come tipo di archiviazione, senza prefissi e con il carattere di tabulazione ( **\t** ) come separatore di campo e il carattere di nuova riga ( **\r\n** ) come carattere di terminazione della riga. **-c** non è compatibile con **-w**.  
  
 Per altre informazioni, vedere [Usare il formato carattere per importare o esportare dati &#40;SQL Server&#41;](../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md).  
  
 **-C** { **ACP** \| **OEM** \| **RAW** \| *code_page* }<a name="C"></a>   
 Specifica la tabella codici dei dati contenuti nel file di dati. *code_page* è pertinente solo se i dati contengono colonne di tipo **char**, **varchar** o **text** con valori di carattere maggiori di 127 o minori di 32.  
  
> [!NOTE]
> È consigliabile specificare un nome regole di confronto per ogni colonna in un file di formato tranne quando si vuole assegnare all'opzione 65001 la priorità sulla specifica delle regole di confronto o della tabella codici.
  
|Valore tabella codici|Descrizione|  
|---------------------|-----------------|  
|ACP|[!INCLUDE[vcpransi](../includes/vcpransi-md.md)]/Microsoft Windows (ISO 1252).|  
|OEM|Tabella codici predefinita utilizzata dal client. Si tratta della tabella codici predefinita usata se non si specifica **-C** .|  
|RAW|Non vengono eseguite conversioni tra tabelle codici. Per questo motivo, si tratta dell'opzione più rapida.|  
|*code_page*|Numero di tabella codici specifico, ad esempio 850.<br /><br /> Le versioni precedenti alla 13 ([!INCLUDE[ssSQL15](../includes/sssql16-md.md)]) non supportano la tabella codici 65001 (codifica UTF-8). Le versioni a partire dalla 13 possono importare la codifica UTF-8 per le versioni precedenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].|  
|||
  
 **-d** _**database\_name**_<a name="d"></a>   
 Specifica il database al quale connettersi. Per impostazione predefinita, bcp.exe si connette al database predefinito dell'utente. Se si specificano -d database_name e un nome in tre parti (database_name.schema.table, passato come primo parametro a bcp.exe), si verificherà un errore poiché non è possibile specificare due volte il nome del database. Se *database_name* inizia con un trattino (-) o una barra (/), non aggiungere uno spazio tra **-d** e il nome del database.  

**-D**<a name="D"></a>  
Indica che il valore passato all'opzione di `bcp` `-S` deve essere interpretato come nome dell'origine dati (DSN). Può essere utile usare un DSN per incorporare le opzioni del driver. Sarà così possibile semplificare le righe di comando, applicare opzioni del driver altrimenti inaccessibili dalla riga di comando, ad esempio MultiSubnetFailover, o impedire che credenziali sensibili siano individuabili come argomenti della riga di comando. Per altre informazioni, vedere _Supporto di DSN in sqlcmd e bcp_ in [Connessione con sqlcmd](../connect/odbc/linux-mac/connecting-with-sqlcmd.md).


 **-e** _**err\_file**_<a name="e"></a>  
 Specifica il percorso completo di un file di errori usato per archiviare le eventuali righe che l'utilità **bcp** non è in grado di trasferire dal file al database. I messaggi di errore generati dal comando **bcp** vengono inviati alla workstation dell'utente. Se questa opzione non viene utilizzata, non viene creato alcun file degli errori.  
  
 Se *err_file* inizia con un segno meno (-) o una barra (/), non includere uno spazio tra **-e** e il valore *err_file* .  
  
**-E**<a name="E"></a>

Specifica che il valore o i valori Identity presenti nel file di dati importato devono essere usati per la colonna Identity. Se si omette l'opzione **-E** , i valori Identity della colonna nel file di dati da importare vengono ignorati e [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] assegna automaticamente valori univoci in base ai valori di inizializzazione e incremento specificati durante la creazione della tabella.  Per altre informazioni, vedere [DBCC CHECKIDENT](../t-sql/database-console-commands/dbcc-checkident-transact-sql.md).
  
 Se il file di dati non contiene valori per la colonna Identity della tabella o della vista, utilizzare un file di formato per specificare che durante l'importazione di dati la colonna Identity della tabella o della vista deve essere ignorata. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] assegna automaticamente valori univoci alla colonna.
  
 L'opzione **-E** è caratterizzata da requisiti di autorizzazione speciali. Per altre informazioni, vedere la sezione "[Osservazioni](#remarks)" di seguito in questo argomento.  
   
 **-f** _**format\_file**_<a name="f"></a>  
 Specifica il percorso completo di un file di formato. Il significato di questa opzione dipende dall'ambiente in cui viene utilizzata, come descritto di seguito:  
  
- Se l'opzione **-f** viene usata con l'opzione **format** , per la tabella o per la vista indicata verrà creato il file *format_file* specificato. Per creare un file di formato XML, è necessario specificare anche l'opzione **-x** . Per altre informazioni, vedere [Creazione di un file di formato &#40;SQL Server&#41;](../relational-databases/import-export/create-a-format-file-sql-server.md).  
  
- Se si usa con l'opzione **in** o **out** , **-f** richiede un file di formato esistente.  
  
    > [!NOTE]
    > L'uso di un file di formato con l'opzione **in** o **out** è facoltativo. Se l'opzione **-f** viene omessa e non si specifica **-n**, **-c**, **-w** o **-N** , il comando richiede l'immissione di informazioni sul formato e consente di salvare le risposte in un file di formato il cui nome predefinito è Bcp.fmt.
  
 Se *format_file* inizia con un segno meno (-) o una barra (/), non includere uno spazio tra **-f** e il valore *format_file* .  
  
**-F** _**first\_row**_<a name="F"></a>  
 Specifica il numero della prima riga da esportare da una tabella o da importare da un file di dati. Per questo parametro è necessario specificare un valore maggiore di (>) 0, ma minore (<) o uguale (=) al numero totale di righe. Se il parametro viene omesso, l'impostazione predefinita è la prima riga del file.  
  
 *first_row* può essere un numero intero positivo con valore massimo pari a 2^63-1. **-F** *first_row* è in base 1.  

**-G**<a name="G"></a>

 Questa opzione viene usata dal client durante la connessione al database SQL di Azure o ad Azure Synapse Analytics per specificare che l'utente deve essere autenticato tramite l'autenticazione di Azure Active Directory. L'opzione -G richiede la [versione 14.0.3008.27 o una versione successiva](https://go.microsoft.com/fwlink/?LinkID=825643). Per determinare la versione, eseguire bcp-v. Per altre informazioni, vedere [Usare l'autenticazione di Azure Active Directory per l'autenticazione a un database SQL o ad Azure Synapse Analytics](/azure/sql-database/sql-database-aad-authentication). 

> [!IMPORTANT]
> L'opzione **-G** si applica solo al database SQL di Azure e ad Azure Data Warehouse.
> L'autenticazione integrata e interattiva di AAD non è attualmente supportata in Linux o macOS.

> [!TIP]
> Per controllare se la versione di bcp in uso include il supporto per l'autenticazione di Azure Active Directory (AAD), digitare **bcp --** (bcp\<space>\<dash>\<dash>) e verificare che -G sia presente nell'elenco degli argomenti disponibili.

- **Nome utente e password di Azure Active Directory:** 

    Quando si vuole usare nome utente e password di Azure Active Directory, è possibile fornire l'opzione **-G** e anche usare nome utente e password fornendo le opzioni **-U** e **-P** . 

    Nell'esempio seguente vengono esportati i dati usando nome utente e password di Azure AD, corrispondenti alle credenziali di AAD. L'esempio esporta la tabella `bcptest` del database `testdb` dal server di Azure `aadserver.database.windows.net` e archivia i dati nel file `c:\last\data1.dat`:

    ```cmd
    bcp bcptest out "c:\last\data1.dat" -c -t -S aadserver.database.windows.net -d testdb -G -U alice@aadtest.onmicrosoft.com -P xxxxx
    ```

    Nell'esempio seguente vengono importati i dati usando nome utente e password di Azure AD, corrispondenti alle credenziali di AAD. L'esempio importa i dati dal file `c:\last\data1.dat` nella tabella `bcptest` per il database `testdb` sul server di Azure `aadserver.database.windows.net` usando utente e password di Azure AD:

    ```cmd
    bcp bcptest in "c:\last\data1.dat" -c -t -S aadserver.database.windows.net -d testdb -G -U alice@aadtest.onmicrosoft.com -P xxxxx
    ```

- **Integrazione con Azure Active Directory**

    Per l'autenticazione integrata di Azure Active Directory, fornire l'opzione **-G** senza nome utente o password. Questa configurazione presuppone che l'account utente di Windows corrente, ovvero l'account con cui viene eseguito il comando bcp, sia federato con Azure AD:

    Nell'esempio seguente i dati vengono esportati usando l'account integrato in Azure AD. L'esempio esporta la tabella `bcptest` del database `testdb` dal server di Azure `aadserver.database.windows.net` usando un account integrato in Azure AD e archivia i dati nel file `c:\last\data2.dat`:

    ```cmd
    bcp bcptest out "c:\last\data2.dat" -S aadserver.database.windows.net -d testdb -G -c -t
    ```

    Nell'esempio seguente i dati vengono importati usando l'autorizzazione integrata di Azure AD. L'esempio importa i dati dal file `c:\last\data2.txt` nella tabella `bcptest` per il database `testdb` sul server di Azure `aadserver.database.windows.net` usando l'autorizzazione integrata di Azure AD:

    ```cmd
    bcp bcptest in "c:\last\data2.dat" -S aadserver.database.windows.net -d testdb -G -c -t
    ```

- **Azure Active Directory Interactive**  

   L'autenticazione interattiva di Azure AD per il database SQL di Azure e Azure Synapse Analytics consente di usare un metodo interattivo che supporta l'autenticazione a più fattori. Per altre informazioni, vedere l'articolo sull'[autenticazione interattiva di Active Directory](../ssdt/azure-active-directory.md#active-directory-interactive-authentication).

   La modalità interattiva di Azure AD richiede **bcp** [versione 15.0.1000.34](#download-the-latest-version-of-bcp-utility) o successiva, nonché [ODBC 17.2 o versioni successive](../connect/odbc/download-odbc-driver-for-sql-server.md).  

   Per abilitare l'autenticazione interattiva, specificare l'opzione-G solo con il nome utente (-U) senza una password.

   Nell'esempio seguente vengono esportati i dati usando la modalità interattiva di Azure AD e indicando il nome utente che rappresenta un account AAD. Si tratta dello stesso esempio usato nella sezione precedente: *Nome utente e password di Azure Active Directory*.  

   Per la modalità interattiva è necessario immettere manualmente una password o, per gli account con l'autenticazione a più fattori abilitata, completare il metodo di autenticazione a più fattori configurato.

   ```cmd
   bcp bcptest out "c:\last\data1.dat" -c -t -S aadserver.database.windows.net -d testdb -G -U alice@aadtest.onmicrosoft.com
   ```

   Se un utente di Azure AD fa parte di un dominio federato che usa un account Windows, il nome utente richiesto nella riga di comando contiene il relativo account di dominio, ad esempio joe@contoso.com, vedere di seguito:

   ```cmd
   bcp bcptest out "c:\last\data1.dat" -c -t -S aadserver.database.windows.net -d testdb -G -U joe@contoso.com
   ```

   Se in un'istanza specifica di Azure AD esistono utenti guest che fanno parte di un gruppo presente nel database SQL con autorizzazioni di database che consentono di eseguire il comando bcp, viene usato l'alias utente guest, ad esempio *keith0@adventureworks.com* .
  
**-h** _**"load hints**_[ ,... *n*] **"** <a name="h"></a> Specifica l'hint o gli hint da usare durante un'importazione in blocco di dati in una tabella o una vista.  
  
* **ORDER**(**_column_[ASC | DESC] [** , **..._n_])**  
Tipo di ordinamento dei dati nel file di dati. Le prestazioni dell'importazione bulk sono migliori se i dati da importare vengono ordinati in base all'indice cluster della tabella, se disponibile. Se il file di dati viene ordinato in modo diverso rispetto all'ordine di una chiave di indice cluster o se la tabella non include un indice cluster, la clausola ORDER viene ignorata. I nomi di colonna specificati devono corrispondere a nomi di colonna validi nella tabella di destinazione. Per impostazione predefinita, **bcp** presuppone che il file di dati non sia ordinato. Per garantire l'ottimizzazione dell'importazione bulk, in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] viene inoltre verificato che i dati importati siano ordinati.  
  
* **ROWS_PER_BATCH** **=** _**bb**_  
Numero di righe di dati per batch (come *bb*). Viene usato quando non si specifica **-b** e quindi l'intero file di dati viene inviato al server come singola transazione. Il server ottimizza il caricamento bulk in base al valore *bb*. Per impostazione predefinita, il valore ROWS_PER_BATCH è sconosciuto.  
  
* **KILOBYTES_PER_BATCH** **=** _**cc**_  
Numero approssimativo di kilobyte di dati per batch (come *cc*). Per impostazione predefinita, il valore KILOBYTES_PER_BATCH è sconosciuto.  
  
* **TABLOCK**  
Specifica che viene acquisito un blocco a livello di tabella per l'aggiornamento bulk per la durata dell'operazione di caricamento bulk. In caso contrario, viene acquisito un blocco di riga. Questo hint migliora significativamente le prestazioni, in quanto il mantenimento di un blocco per la durata dell'operazione di copia bulk riduce la contesa dei blocchi per la tabella. Una tabella può essere caricata simultaneamente da più client se non include indici e si specifica **TABLOCK** . Per impostazione predefinita, la modalità di blocco è determinata dall'opzione **table lock on bulkload** della tabella.  
  
  > [!NOTE]
  > Se la tabella di destinazione è l'indice columnstore cluster, l'hint TABLOCK non è necessario per il caricamento simultaneo da più client poiché ogni thread simultaneo usa un gruppo di righe separato all'interno dell'indice per caricare i dati. Per informazioni dettagliate, fare riferimento alle informazioni relative all'indice columnstore,
  
  **CHECK_CONSTRAINTS**  
  Specifica che tutti i vincoli sulla tabella o sulla vista di destinazione devono essere controllati durante l'operazione di importazione bulk. Se non si specifica l'hint CHECK_CONSTRAINTS, i vincoli CHECK e FOREIGN KEY vengono ignorati e al termine dell'operazione il vincolo sulla tabella viene contrassegnato come non attendibile.  
  
  > [!NOTE]
  > I vincoli UNIQUE, PRIMARY KEY e NOT NULL vengono sempre applicati.
  
  In un determinato momento sarà necessario controllare i vincoli sull'intera tabella. Se prima dell'operazione di importazione bulk la tabella non è vuota, il costo per la riconvalida del vincolo potrebbe essere superiore a quello correlato all'applicazione dei vincoli CHECK ai dati incrementali. È pertanto consigliabile abilitare in genere il controllo dei vincoli durante un'importazione bulk incrementale.  
  
  Una situazione in cui può essere necessario disabilitare i vincoli (comportamento predefinito) è rappresentata dal caso in cui i dati di input contengono righe che violano i vincoli. Se i vincoli CHECK sono disabilitati, è possibile importare i dati e quindi utilizzare le istruzioni [!INCLUDE[tsql](../includes/tsql-md.md)] per rimuovere i dati non validi.  
  
  > [!NOTE]
  > L'utilità **bcp** ora esegue la convalida e i controlli dei dati che potrebbero causare la mancata esecuzione degli script quando questi vengono eseguiti su dati non validi inclusi in un file di dati.

  > [!NOTE]
  > L'opzione **-m** *max_errors* non è valida per il controllo dei vincoli.
  
* **FIRE_TRIGGERS**  
Se specificato con l'argomento **in** , determina l'esecuzione dei trigger di inserimento definiti nella tabella di destinazione durante l'operazione di copia bulk. Se non si specifica FIRE_TRIGGERS, non viene eseguito alcun trigger di inserimento. FIRE_TRIGGERS viene ignorato per gli argomenti **out**, **queryout** e **format** .  
  
**-i** _**input\_file**_<a name="i"></a>  
Specifica il nome di un file di risposta contenente le risposte alle domande del prompt dei comandi per ogni campo dati quando si esegue una copia bulk in modalità interattiva, ovvero senza indicare **-n**, **-c**, **-w** o **-N** .  
  
Se *input_file* inizia con un segno meno (-) o una barra (/), non includere uno spazio tra **-i** e il valore *input_file* .  
  
**-k**<a name="k"></a>  
Specifica che durante l'operazione il valore delle colonne vuote deve essere Null, ovvero che non verranno inseriti valori predefiniti in tali colonne. Per altre informazioni, vedere [Mantenere i valori Null o usare i valori predefiniti durante l'importazione in blocco &#40;SQL Server&#41;](../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md).  
  
**-K** _**application\_intent**_<a name="K"></a>   
Dichiara il tipo di carico di lavoro dell'applicazione in caso di connessione a un server. L'unico valore consentito è **ReadOnly**. Se l'opzione **-K** non è specificata, l'utilità bcp non supporterà la connettività a una replica secondaria in un gruppo di disponibilità Always On. Per altre informazioni, vedere [Repliche secondarie attive: Repliche secondarie leggibili &#40;Gruppi di disponibilità Always On&#41;](../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).  
  
**-l** _**login\_timeout**_<a name="l"></a>  
Specifica un timeout accesso. L'opzione -l specifica il numero di secondi che devono trascorrere prima che si verifichi il timeout di un accesso a SQL Server quando si tenta la connessione a un server. Il timeout di accesso predefinito è 15 secondi. Il valore del timeout deve essere un numero compreso tra 0 e 65534. Se il valore specificato non è numerico o non è compreso in tale intervallo, bcp genera un messaggio di errore. Il valore 0 specifica un timeout infinito.
  
**-L** _**last\_row**_<a name="L"></a>  
Specifica il numero dell'ultima riga da esportare da una tabella o da importare da un file di dati. Per questo parametro è necessario specificare un valore maggiore di (>) 0, ma minore (<) o uguale (=) al numero dell'ultima riga. Se il parametro viene omesso, l'impostazione predefinita è l'ultima riga del file.  
  
*last_row* può essere un numero intero positivo con valore massimo pari a 2^63-1.  
  
**-m** _**max\_errors**_<a name="m"></a>  
Specifica il numero massimo di errori di sintassi che possono verificarsi prima dell'annullamento dell'operazione **bcp** . Un errore di sintassi implica un errore di conversione dei dati nel tipo di dati di destinazione. Nel valore totale restituito da *max_errors* sono esclusi tutti gli errori che possono essere rilevati solo a livello del server, ad esempio le violazioni dei vincoli.  
  
 Una riga che non può essere copiata dall'utilità **bcp** viene ignorata e conteggiata come errore. Se l'opzione viene omessa, il valore predefinito è 10.  
  
> [!NOTE]
> L'opzione **-m** non è valida per la conversione dei tipi di dati **money** o **bigint** .
  
**-n**<a name="n"></a>  
Esegue l'operazione di copia bulk utilizzando i tipi di dati nativi del database. Con questa opzione non viene visualizzata una richiesta per ogni campo, ma vengono utilizzati i valori nativi.  
  
Per altre informazioni, vedere [Usare il formato nativo per importare o esportare dati &#40;SQL Server&#41;](../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md).  
  
**-N**<a name="N"></a>  
Esegue l'operazione di copia bulk utilizzando i tipi di dati nativi del database per i dati non di tipo carattere e i caratteri Unicode per i dati di tipo carattere. Questa opzione rappresenta un'alternativa, con migliori prestazioni, all'opzione **-w** e può essere usata per trasferire dati tra istanze di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con un file di dati. Non viene visualizzata una richiesta per ogni campo. Utilizzare questa opzione per trasferire dati contenenti caratteri ANSI estesi se si desidera sfruttare le prestazioni della modalità nativa.  
  
 Per altre informazioni, vedere [Usare il formato Unicode nativo per importare o esportare dati &#40;SQL Server&#41;](../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md).  
  
 Se si esportano e successivamente si importano i dati nello stesso schema di tabella usando bcp.exe con **-N**, può essere visualizzato un avviso di troncamento se è presente una colonna contenente caratteri non Unicode a lunghezza fissa, ad esempio **char(10)** .  
  
 L'avviso può essere ignorato. Per evitare la visualizzazione dell'avviso, usare **-n** invece di **-N**.  
  
 **-o** _**output\_file**_<a name="o"></a>  
 Specifica il nome di un file in cui viene reindirizzato l'output dal prompt dei comandi.  
  
 Se *output_file* inizia con un segno meno (-) o una barra (/), non includere uno spazio tra **-o** e il valore *output_file* .  
  
 **-P** _**password**_<a name="P"></a>  
 Specifica la password per l'ID di accesso. Se si omette questa opzione, il comando **bcp** richiede una password. Se l'opzione viene specificata alla fine del prompt dei comandi senza indicare una password, **bcp** userà la password predefinita (NULL).  
  
> [!IMPORTANT]
> [!INCLUDE[ssNoteStrongPass](../includes/ssnotestrongpass-md.md)]
  
 Per nascondere la password, non specificare l'opzione **-P** in combinazione con l'opzione **-U** . Dopo aver specificato il comando **bcp** con **-U** e altre opzioni (non specificare **-P**) premere INVIO. Il comando richiederà l'immissione di una password. Questo metodo garantisce che la password venga nascosta durante l'immissione.  
  
 Se *password* inizia con un segno meno (-) o una barra (/), non aggiungere uno spazio tra **-P** e il valore *password* .  
  
 **-q**<a name="q"></a>  
 Esegue l'istruzione SET QUOTED_IDENTIFIERS ON durante la connessione tra l'utilità **bcp** e un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Questa opzione consente di specificare il nome di un database, di un proprietario, di una tabella o di una vista che include uno spazio o una virgoletta singola. Racchiudere tra virgolette doppie (" ") l'intero nome in tre parti della tabella o della vista.  
  
 Per specificare un nome di database contenente uno spazio o una virgoletta singola, è necessario usare l'opzione **-q**.  
  
 **-q** non è applicabile ai valori passati a **-d**.  
  
 Per altre informazioni, vedere la sezione [Osservazioni](#remarks)di seguito in questo argomento.  
  
 **-r** _**row\_term**_<a name="r"></a>  
 Specifica il carattere di terminazione della riga. Il valore predefinito è **\n** (carattere di nuova riga). Utilizzare questo parametro per specificare un carattere di terminazione della riga diverso da quello predefinito. Per altre informazioni, vedere [Impostazione dei caratteri di terminazione del campo e della riga &#40;SQL Server&#41;](../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).  
  
 Se si specifica il carattere di terminazione della riga in notazione esadecimale nel comando bcp.exe, il valore verrà troncato in corrispondenza di 0x00. Se ad esempio si specifica 0x410041, verrà utilizzato 0x41.  
  
 Se *row_term* inizia con un segno meno (-) o una barra (/), non includere uno spazio tra **-r** e il valore *row_term* .  
  
 **-R**<a name="R"></a>  
 Specifica che la copia bulk dei dati relativi a valuta, data e ora verrà eseguita in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilizzando il formato definito per le impostazioni locali del computer client. Per impostazione predefinita, le impostazioni internazionali vengono ignorate.  
  
 **-S** _**server\_name**_ [\\_**instance\_name**_]<a name="S"></a> Specifica l'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] cui connettersi. Se non si specifica alcun server, l'utilità **bcp** si connette all'istanza predefinita di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] del computer locale. È necessario specificare questa opzione se un comando **bcp** viene eseguito da un computer remoto in rete o in un'istanza denominata locale. Per connettersi all'istanza predefinita di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] in un server, specificare solo *server_name*. Per connettersi a un'istanza denominata di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], specificare _server\_name_ **\\** _instance\_name_.  
  
 **-t** _**field\_term**_<a name="t"></a>  
 Specifica il carattere di terminazione del campo. Il valore predefinito è **\t** (carattere di tabulazione). Utilizzare questo parametro per specificare un carattere di terminazione del campo diverso da quello predefinito. Per altre informazioni, vedere [Impostazione dei caratteri di terminazione del campo e della riga &#40;SQL Server&#41;](../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).  
  
 Se si specifica il carattere di terminazione del campo in notazione esadecimale nel comando bcp.exe, il valore verrà troncato in corrispondenza di 0x00. Se ad esempio si specifica 0x410041, verrà utilizzato 0x41.  
  
 Se *field_term* inizia con un segno meno (-) o una barra (/), non includere uno spazio tra **-t** e il valore *field_term* .  
  
 **-T**<a name="T"></a>  
 Specifica che l'utilità **bcp** si connette a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con una connessione trusted che usa la sicurezza integrata. Non è necessario specificare le credenziali di sicurezza dell'utente di rete, ovvero *login_id* e *password* . Se non si specifica **-T** , è necessario specificare **-U** e **-P** per eseguire correttamente l'accesso.

> [!IMPORTANT]
> Quando l'utilità **bcp** si connette a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tramite una connessione trusted con sicurezza integrata, usare l'opzione **-T** (connessione trusted) invece della combinazione di *user name* e *password* . Quando l'utilità **bcp** si connette al database SQL o ad Azure Synapse Analytics tramite l'autenticazione di Windows o l'autenticazione di Azure Active Directory, l'opzione non è supportata. Usare le opzioni **- U** e **-P** . 
  
 **-U** _**login\_id**_<a name="U"></a>  
 Specifica l'ID di accesso utilizzato per connettersi a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]
> Quando l'utilità **bcp** si connette a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tramite una connessione trusted con sicurezza integrata, usare l'opzione **-T** (connessione trusted) invece della combinazione di *user name* e *password* . Quando l'utilità **bcp** si connette al database SQL o ad Azure Synapse Analytics tramite l'autenticazione di Windows o l'autenticazione di Azure Active Directory, l'opzione non è supportata. Usare le opzioni **- U** e **-P** .
  
 **-v**<a name="v"></a>  
 Visualizza il numero di versione e le informazioni sul copyright per l'utilità **bcp** .  
  
 **-V** (**80** \| **90** \| **100** \| **110** \| **120** \| **130**)<a name="V"></a>  
 Esegue l'operazione di copia bulk utilizzando i tipi di dati di una versione precedente di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Con questa opzione non viene visualizzata una richiesta per ogni campo, ma vengono utilizzati i valori predefiniti.  
  
 **80** = [!INCLUDE[ssVersion2000](../includes/ssversion2000-md.md)]  
  
 **90** = [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)]  
  
 **100** = [!INCLUDE[ssKatmai](../includes/sskatmai-md.md)] e [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]  
  
 **110** = [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]  
  
 **120** = [!INCLUDE[ssSQL14](../includes/sssql14-md.md)]  
  
 **130** = [!INCLUDE[ssSQL15](../includes/sssql16-md.md)]  
  
 Per generare ad esempio dati per tipi non supportati in [!INCLUDE[ssVersion2000](../includes/ssversion2000-md.md)], ma introdotti in versioni successive di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], utilizzare l'opzione -V80.  
  
 Per altre informazioni, vedere [Importare dati in formato nativo e carattere da versioni precedenti di SQL Server](../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md).  
  
 **-w**<a name="w"></a>  
 Esegue l'operazione di copia bulk utilizzando caratteri Unicode. Questa opzione non visualizza una richiesta per ogni campo, ma usa **nchar** come tipo di archiviazione, il carattere di tabulazione ( **\t** ) come separatore dei campi e il carattere di nuova riga ( **\n** ) come carattere di terminazione della riga e non usa alcun prefisso. **-w** non è compatibile con **-c**.  
  
 Per altre informazioni, vedere [Usare il formato carattere Unicode per importare o esportare dati &#40;SQL Server&#41;](../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md).  
  
 **-x**<a name="x"></a>  
 Se usato con le opzioni **format** e **-f** *format_file*, genera un file di formato basato su XML anziché un file di formato predefinito non XML. L'opzione **-x** non può essere usata per l'importazione o l'esportazione dei dati. Genera un errore se viene usata senza **format** e **-f** *format_file*.  

## <a name="remarks"></a>Osservazioni<a name="remarks"></a>

- L'utilità **bcp** 13.0 viene installato durante l'installazione degli strumenti di [!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] . Se gli strumenti vengono installati sia per [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] che per una versione precedente di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], a seconda del valore della variabile di ambiente PATH, è possibile che venga usato il client **bcp** precedente anziché il client **bcp** 13.0. La variabile di ambiente definisce il set di directory utilizzato in Windows per la ricerca di file eseguibili. Per determinare la versione in uso, eseguire il comando **bcp /v** o **bcp -v** al prompt dei comandi di Windows. Per informazioni su come impostare il percorso di comando nella variabile di ambiente PATH, vedere [Variabili di ambiente](/windows/win32/shell/user-environment-variables) o cercare le variabili di ambiente nella Guida di Windows.

    Per assicurarsi che sia in esecuzione la versione più recente dell'utilità bcp, è necessario rimuovere le versioni precedenti dell'utilità bcp.

    Per determinare la posizione di installazione di tutte le versioni dell'utilità bcp, digitare nel prompt dei comandi:

    ```cmd
    where bcp.exe
    ```

- L'utilità bcp può anche essere scaricata separatamente dal [Microsoft SQL Server 2016 Feature Pack](https://www.microsoft.com/download/details.aspx?id=56833).  Selezionare `ENU\x64\MsSqlCmdLnUtils.msi` o `ENU\x86\MsSqlCmdLnUtils.msi`.

- I file di formato XML sono supportati solo quando gli strumenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] vengono installati insieme a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Native Client.

- Per informazioni sulla posizione e sulle modalità di esecuzione dell'utilità **bcp** e sulle convenzioni di sintassi per le utilità della riga di comando, vedere [Guida di riferimento alle utilità del prompt dei comandi &#40;Motore di database&#41;](../tools/command-prompt-utility-reference-database-engine.md).

- Per informazioni sulla preparazione dei dati per le operazioni di importazione o esportazione in blocco, vedere [Preparare i dati per l'importazione o l'esportazione in blocco &#40;SQL Server&#41;](../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md).

- Per informazioni sui casi in cui le operazioni di inserimento di righe eseguite durante l'importazione in blocco vengono registrate nel log delle transazioni, vedere [Prerequisiti per la registrazione minima nell'importazione in blocco](../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md).

- [Uso di caratteri speciali aggiuntivi](/windows-server/administration/windows-commands/set_1#remarks)

    I caratteri <, >, |, &, ^ sono caratteri speciali della shell dei comandi e devono essere preceduti dal carattere di escape (^) o racchiusi tra virgolette quando vengono usati nella stringa, ad esempio "StringaContenenteSimbolo&". Se si usano le virgolette per racchiudere una stringa contenente uno dei caratteri speciali, le virgolette vengono impostate come parte del valore della variabile di ambiente.

## <a name="native-data-file-support"></a>Supporto per file di dati nativi

 In [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)], l'utilità **bcp** supporta i file di dati nativi compatibili con [!INCLUDE[ssVersion2000](../includes/ssversion2000-md.md)], [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)], [!INCLUDE[ssKatmai](../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]e [!INCLUDE[ssSQL11](../includes/sssql11-md.md)].  

## <a name="computed-columns-and-timestamp-columns"></a>Colonne calcolate e colonne timestamp

 I valori per le colonne calcolate o **timestamp** nel file di dati importato vengono ignorati e [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] assegna automaticamente nuovi valori. Se il file di dati non contiene valori per le colonne calcolate o **timestamp** della tabella, usare un file di formato per specificare che le colonne calcolate o **timestamp** della tabella dovranno essere ignorate durante l'importazione dei dati. I valori per la colonna verranno assegnati automaticamente da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 Le copie bulk di colonne calcolate e **timestamp** da [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a un file di dati vengono eseguite con le modalità consuete.  

## <a name="specifying-identifiers-that-contain-spaces-or-quotation-marks"></a>Definizione di identificatori contenenti spazi o virgolette

 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] possono includere caratteri quali spazi incorporati e virgolette. Tali identificatori possono essere utilizzati nei modi seguenti:  
  
- Quando al prompt dei comandi si specifica un identificatore o un nome di file che include uno spazio o una virgoletta singola, racchiuderlo tra virgolette doppie ("").  

    Il comando `bcp out` seguente, ad esempio, consente di creare un file di dati denominato `Currency Types.dat`:  

    ```cmd
    bcp AdventureWorks2012.Sales.Currency out "Currency Types.dat" -T -c  
    ```
  
- Per specificare un nome di database contenente uno spazio o una virgoletta singola, è necessario usare l'opzione **-q** .  
  
- Per i nomi di proprietario, di tabella o di vista contenenti spazi incorporati o virgolette singole, è possibile effettuare le operazioni seguenti:  
  
    - Specificare l'opzione **-q** oppure  
  
    - Racchiudere il nome del proprietario, della tabella o della vista tra parentesi quadre ([]) all'interno di virgolette.  

## <a name="data-validation"></a>Convalida dei dati

 L'utilità **bcp** ora esegue la convalida e i controlli dei dati che potrebbero causare la mancata esecuzione degli script quando questi vengono eseguiti su dati non validi inclusi in un file di dati. L'utilità **bcp** , ad esempio, verifica quanto segue:  
  
- Validità delle rappresentazioni native dei tipi di dati float o real.  
  
- Lunghezza in byte pari dei dati Unicode.  
  
 Il caricamento di alcuni tipi di dati non validi di cui può essere eseguita l'importazione bulk nelle versioni precedenti di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] potrebbe avere esito negativo in questa versione. Nelle versioni precedenti l'errore si verifica solo quando un client tenta di accedere ai dati non validi. La convalida aggiuntiva riduce il rischio di errori durante l'esecuzione di query sui dati in seguito al caricamento bulk.  

## <a name="bulk-exporting-or-importing-sqlxml-documents"></a>Esportazione o importazione bulk di documenti SQLXML

Per l'esportazione o l'importazione bulk di dati SQLXML, utilizzare uno dei tipi di dati seguenti nel file di formato.  
  
|Tipo di dati|Effetto|  
|---------------|------------|  
|SQLCHAR o SQLVARYCHAR|I dati vengono inviati nella tabella codici del client o nella tabella codici implicita delle regole di confronto. L'effetto equivale a quello ottenuto specificando l'opzione **-c** senza definire un file di formato.|  
|SQLNCHAR o SQLNVARCHAR|I dati vengono inviati in formato Unicode. L'effetto equivale a quello ottenuto specificando l'opzione **-w** senza definire un file di formato.|  
|SQLBINARY o SQLVARYBIN|I dati vengono inviati senza conversione.|  

## <a name="permissions"></a>Autorizzazioni

 Un'operazione **bcp out** richiede l'autorizzazione SELECT per la tabella di origine.  
  
 Un'operazione **bcp in** richiede almeno l'autorizzazione SELECT/INSERT per la tabella di destinazione. È inoltre richiesta l'autorizzazione ALTER TABLE se si verifica uno dei casi seguenti:  
  
- Sono presenti alcuni vincoli e l'hint CHECK_CONSTRAINTS non è specificato.  
  
    > [!NOTE]
    > Per impostazione predefinita, i vincoli sono disabilitati. Per abilitarli in modo esplicito, usare l'opzione **-h** con l'hint CHECK_CONSTRAINTS.
  
- Sono presenti alcuni trigger e l'hint FIRE_TRIGGER non è specificato.  
  
    > [!NOTE]
    > Per impostazione predefinita, i trigger non sono attivati. Per attivarli in modo esplicito, usare l'opzione **-h** con l'hint FIRE_TRIGGERS.
  
- Usare l'opzione **-E** per importare valori Identity da un file di dati.  
  
> [!NOTE]
> Il requisito relativo all'autorizzazione ALTER TABLE per la tabella di destinazione è una caratteristica introdotta in [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)]. Questo nuovo requisito può causare l'esito negativo degli script di **bcp** che non consentono l'applicazione di trigger e controlli dei vincoli se l'account utente non dispone delle autorizzazioni ALTER TABLE per la tabella di destinazione.

## <a name="character-mode--c-and-native-mode--n-best-practices"></a>Procedure consigliate relative alla modalità carattere (-c) e alla modalità nativa (-n)

 In questa sezione sono presenti indicazioni relative alla modalità carattere (-c) e alla modalità nativa (-n).  
  
- (Amministratore/utente) Quando possibile, utilizzare il formato nativo (-n) per evitare i problemi relativi al separatore. Utilizzare il formato nativo per esportare e importare tramite [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Se i dati saranno importati in un database non [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , esportarli da[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilizzando l'opzione -c o -w.  
  
- (Amministratore) Verificare i dati in caso di utilizzo di BCP OUT. Ad esempio, quando si utilizza BCP OUT, BCP IN, quindi BCP OUT, verificare che i dati vengano esportati correttamente e che i valori del carattere di terminazione non siano utilizzati in alcuni valori di dati. Considerare di eseguire l'override dei caratteri di terminazione predefiniti (usando le opzioni -t e -r) con valori esadecimali casuali per evitare conflitti tra i valori del carattere di terminazione e i valori dei dati.  
  
- (Utente) Utilizzare un carattere di terminazione lungo e univoco (qualsiasi sequenza di byte o caratteri) per ridurre la possibilità di un conflitto con il valore stringa effettivo. Questa operazione può essere effettuata utilizzando le opzioni -t e -r.  

## <a name="examples"></a>Esempi

Questa sezione contiene gli esempi seguenti:

R. Identificare la versione dell'utilità **bcp**

B. Copia delle righe di tabella in un file di dati (con connessione trusted)

C. Copia delle righe di tabella in un file di dati (con autenticazione in modalità mista)

D. Copia di dati da un file a una tabella

E. Copia di una colonna specifica in un file di dati

F. Copia di una riga specifica in un file di dati

G. Copia di dati da una query a un file di dati

H. Creazione dei file di formato

I. Uso di un file di formato per l'importazione bulk con **bcp**

J. Definizione di una tabella codici

### <a name="example-test-conditions"></a>**Condizioni di test di esempio**

Gli esempi seguenti usano il database di esempio `WideWorldImporters` per SQL Server (a partire dal 2016) e il database SQL di Azure.  `WideWorldImporters` può essere scaricato da [https://github.com/Microsoft/sql-server-samples/releases/tag/wide-world-importers-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/wide-world-importers-v1.0).  Per la sintassi da usare per ripristinare il database di esempio, vedere [RESTORE (Transact-SQL)](../t-sql/statements/restore-statements-transact-sql.md) .  Se non specificato diversamente, si presuppone che l'utente usi l'autenticazione di Windows e abbia una connessione trusted all'istanza del server in cui viene eseguito il comando **bcp** .  Una directory denominata `D:\BCP` verrà usata nella maggior parte degli esempi.

Lo script seguente crea una copia vuota della tabella `WideWorldImporters.Warehouse.StockItemTransactions` e quindi aggiunge un vincolo di chiave primaria.  Eseguire lo script T-SQL seguente in SQL Server Management Studio (SSMS)

```sql  
USE WideWorldImporters;  
GO  

SET NOCOUNT ON;

IF NOT EXISTS (SELECT * FROM sys.tables WHERE name = 'Warehouse.StockItemTransactions_bcp')
BEGIN
    SELECT * INTO WideWorldImporters.Warehouse.StockItemTransactions_bcp
    FROM WideWorldImporters.Warehouse.StockItemTransactions  
    WHERE 1 = 2;  

    ALTER TABLE Warehouse.StockItemTransactions_bcp 
    ADD CONSTRAINT PK_Warehouse_StockItemTransactions_bcp PRIMARY KEY NONCLUSTERED 
    (StockItemTransactionID ASC);
END
```

> [!NOTE]
> Troncare la tabella `StockItemTransactions_bcp` in base alle esigenze.
>
> TRUNCATE TABLE WideWorldImporters.Warehouse.StockItemTransactions_bcp;

### <a name="a--identify-bcp-utility-version"></a>R.  Identificare la versione dell'utilità **bcp**

Al prompt dei comandi immettere il comando seguente:

```cmd
bcp -v
```
  
### <a name="b-copying-table-rows-into-a-data-file-with-a-trusted-connection"></a>B. Copia delle righe di tabella in un file di dati (con connessione trusted)

Gli esempi seguenti illustrano l'uso dell'opzione **out** nella tabella `WideWorldImporters.Warehouse.StockItemTransactions`.

- **Base** Questo esempio crea un file di dati denominato `StockItemTransactions_character.bcp` in cui vengono copiati i dati della tabella usando il formato **carattere**.

  Al prompt dei comandi immettere il comando seguente:

  ```cmd
  bcp WideWorldImporters.Warehouse.StockItemTransactions out D:\BCP\StockItemTransactions_character.bcp -c -T
  ```

- **Espanso** Questo esempio crea un file di dati denominato `StockItemTransactions_native.bcp` e vi copia i dati della tabella nel formato **nativo**.  L'esempio inoltre: specifica il numero massimo di errori di sintassi, un file di errore e un file di output.

    Al prompt dei comandi immettere il comando seguente:

    ```cmd
    bcp WideWorldImporters.Warehouse.StockItemTransactions OUT D:\BCP\StockItemTransactions_native.bcp -m 1 -n -e D:\BCP\Error_out.log -o D:\BCP\Output_out.log -S -T
    ```

Esaminare `Error_out.log` e `Output_out.log`.  `Error_out.log` deve essere vuoto.  Confrontare le dimensioni dei file tra `StockItemTransactions_character.bcp` e `StockItemTransactions_native.bcp`.

### <a name="c-copying-table-rows-into-a-data-file-with-mixed-mode-authentication"></a>C. Copia delle righe di tabella in un file di dati (con autenticazione in modalità mista)

L'esempio seguente illustra l'uso dell'opzione **out** nella tabella `WideWorldImporters.Warehouse.StockItemTransactions`  Questo esempio crea un file di dati denominato `StockItemTransactions_character.bcp` in cui vengono copiati i dati della tabella usando il formato **carattere** .  
  
 Si presuppone che l'utente usi l'autenticazione in modalità mista, quindi l'opzione **-U** è necessaria per specificare l'ID di accesso. A meno che non venga eseguita la connessione all'istanza predefinita di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sul computer locale, usare l'opzione **-S** per specificare il nome di sistema e, facoltativamente, il nome di un'istanza.  

Al prompt dei comandi immettere il comando seguente: \(Verrà richiesto di inserire la password.\)

```cmd
bcp WideWorldImporters.Warehouse.StockItemTransactions out D:\BCP\StockItemTransactions_character.bcp -c -U<login_id> -S<server_name\instance_name>
```

### <a name="d-copying-data-from-a-file-to-a-table"></a>D. Copia di dati da un file a una tabella

Gli esempi seguenti illustrano l'opzione **in** nella tabella `WideWorldImporters.Warehouse.StockItemTransactions_bcp` tramite i file creati in precedenza.

- **Base** Questo esempio usa il file di dati `StockItemTransactions_character.bcp` creato in precedenza.

  Al prompt dei comandi immettere il comando seguente:

  ```cmd
  bcp WideWorldImporters.Warehouse.StockItemTransactions_bcp IN D:\BCP\StockItemTransactions_character.bcp -c -T
  ```

- **Esteso** Questo esempio usa il file di dati `StockItemTransactions_native.bcp` creato in precedenza.  L'esempio inoltre: usa l'hint **TABLOCK**, specifica la dimensione del batch, il numero massimo di errori di sintassi, un file di errore e un file di output.
  
Al prompt dei comandi immettere il comando seguente:

```cmd
bcp WideWorldImporters.Warehouse.StockItemTransactions_bcp IN D:\BCP\StockItemTransactions_native.bcp -b 5000 -h "TABLOCK" -m 1 -n -e D:\BCP\Error_in.log -o D:\BCP\Output_in.log -S -T
```

  Esaminare `Error_in.log` e `Output_in.log`.

### <a name="e-copying-a-specific-column-into-a-data-file"></a>E. Copia di una colonna specifica in un file di dati

Per copiare una colonna specifica, è possibile usare l'opzione **queryout** .  Nell'esempio seguente viene copiata in un file di dati solo la colonna `StockItemTransactionID` della tabella `Warehouse.StockItemTransactions` .
  
Al prompt dei comandi immettere il comando seguente:

```cmd
bcp "SELECT StockItemTransactionID FROM WideWorldImporters.Warehouse.StockItemTransactions WITH (NOLOCK)" queryout D:\BCP\StockItemTransactionID_c.bcp -c -T
```

### <a name="f-copying-a-specific-row-into-a-data-file"></a>F. Copia di una riga specifica in un file di dati

Per copiare una riga specifica, è possibile usare l'opzione **queryout** . Nell'esempio seguente solo la riga dell'utente denominato `Amy Trefl` viene copiata dalla tabella `WideWorldImporters.Application.People` in un file di dati `Amy_Trefl_c.bcp`.  Nota: l'opzione **-d** viene usata per identificare il database.
  
Al prompt dei comandi immettere il comando seguente:

```cmd
bcp "SELECT * from Application.People WHERE FullName = 'Amy Trefl'" queryout D:\BCP\Amy_Trefl_c.bcp -d WideWorldImporters -c -T
```

### <a name="g-copying-data-from-a-query-to-a-data-file"></a>G. Copia di dati da una query a un file di dati

Per copiare il set di risultati da un'istruzione Transact-SQL a un file di dati, usare l'opzione **queryout** .  Nell'esempio seguente vengono copiati i nomi dalla tabella `WideWorldImporters.Application.People` nel file di dati `People.txt` , ordinandoli in base al nome completo.  Nota: l'opzione **-t** viene usata per creare un file delimitato da virgole.

Al prompt dei comandi immettere il comando seguente:

```cmd
bcp "SELECT FullName, PreferredName FROM WideWorldImporters.Application.People ORDER BY FullName" queryout D:\BCP\People.txt -t, -c -T
```

### <a name="h-creating-format-files"></a>H. Creazione dei file di formato

Nell'esempio seguente vengono creati tre diversi file di formato per la tabella `Warehouse.StockItemTransactions` nel database `WideWorldImporters` .  Esaminare il contenuto di ogni file creato.

Al prompt dei comandi immettere i comandi seguenti:

```cmd
REM non-XML character format
bcp WideWorldImporters.Warehouse.StockItemTransactions format nul -f D:\BCP\StockItemTransactions_c.fmt -c -T 

REM non-XML native format
bcp WideWorldImporters.Warehouse.StockItemTransactions format nul -f D:\BCP\StockItemTransactions_n.fmt -n -T

REM XML character format
bcp WideWorldImporters.Warehouse.StockItemTransactions format nul -f D:\BCP\StockItemTransactions_c.xml -x -c -T
```

> [!NOTE]
> Per usare l'opzione **-x** , è necessario avere un client **bcp** 9.0. Per informazioni sull'uso del client **bcp** 9.0, vedere la sezione "[Osservazioni](#remarks)".
  
 Per altre informazioni, vedere [File in formato non XML &#40;SQL Server&#41;](../relational-databases/import-export/non-xml-format-files-sql-server.md) e [File in formato XML &#40;SQL Server&#41;](../relational-databases/import-export/xml-format-files-sql-server.md).
  
### <a name="i-using-a-format-file-to-bulk-import-with-bcp"></a>I. Utilizzo di un file di formato per l'importazione bulk con bcp

Per usare un file di formato creato in precedenza durante l'importazione di dati in un'istanza di [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], specificare **-f** con l'opzione **in** .  Il comando seguente, ad esempio, consente di eseguire la copia bulk del contenuto di un file di dati, `StockItemTransactions_character.bcp`, in una copia della tabella `Warehouse.StockItemTransactions_bcp` usando il file di formato creato in precedenza, `StockItemTransactions_c.xml`.  Nota: l'opzione **-L** viene usata per importare solo i primi 100 record.

Al prompt dei comandi immettere il comando seguente:

```cmd
bcp WideWorldImporters.Warehouse.StockItemTransactions_bcp in D:\BCP\StockItemTransactions_character.bcp -L 100 -f D:\BCP\StockItemTransactions_c.xml -T
```

> [!NOTE]
> I file di formato risultano particolarmente utili quando i campi dei file di dati sono diversi dalle colonne della tabella, ad esempio per numero, ordine o tipi di dati. Per altre informazioni, vedere [File di formato per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  

### <a name="j-specifying-a-code-page"></a>J. Definizione di una tabella codici

Il codice parziale riportato di seguito rappresenta l'importazione bcp quando si specifica una tabella codici 65001:

```cmd
bcp.exe MyTable in "D:\data.csv" -T -c -C 65001 -t , ...  
```

## <a name="additional-examples"></a>Esempi aggiuntivi

|Gli argomenti seguenti contengono esempi relativi all'uso di bcp: |
|---|
|Formati di dati per l'importazione o l'esportazione bulk (SQL Server)<br />&emsp;&#9679;&emsp;[Usare il formato nativo per importare o esportare dati (SQL Server)](../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Usare il formato carattere per importare o esportare dati (SQL Server)](../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Usare il formato nativo Unicode per importare o esportare dati (SQL Server)](../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Usare il formato carattere Unicode per importare o esportare dati (SQL Server)](../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)<br /><br />[Impostazione dei caratteri di terminazione del campo e della riga (SQL Server)](../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)<br /><br />[Mantenimento dei valori Null o utilizzo dei valori predefiniti durante un'importazione bulk (SQL Server)](../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)<br /><br />[Mantenere i valori Identity durante l'importazione bulk dei dati (SQL Server)](../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)<br /><br />File di formato per l'importazione o l'esportazione di dati (SQL Server)<br />&emsp;&#9679;&emsp;[Creare un file di formato (SQL Server)](../relational-databases/import-export/create-a-format-file-sql-server.md)<br />&emsp;&#9679;&emsp;[Usare un file di formato per l'importazione bulk dei dati (SQL Server)](../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Usare un file di formato per ignorare una colonna di una tabella (SQL Server)](../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)<br />&emsp;&#9679;&emsp;[Usare un file di formato per ignorare un campo dati (SQL Server)](../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)<br />&emsp;&#9679;&emsp;[Usare un file di formato per eseguire il mapping tra le colonne della tabella e i campi del file di dati (SQL Server)](../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)<br /><br />[Esempi di importazione ed esportazione bulk di documenti XML (SQL Server)](../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)<br /><p>  </p>|

## <a name="considerations-and-limitations"></a>Considerazioni e limiti

- L'utilità bcp presenta una limitazione per cui il messaggio di errore visualizza solo 512 byte di caratteri. Vengono visualizzati solo i primi 512 byte del messaggio di errore.

## <a name="next-steps"></a>Passaggi successivi

- [Preparare i dati per l'importazione o l'esportazione in blocco &#40;SQL Server&#41;](../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)
- [BULK INSERT &#40;Transact-SQL&#41;](../t-sql/statements/bulk-insert-transact-sql.md)
- [OPENROWSET &#40;Transact-SQL&#41;](../t-sql/functions/openrowset-transact-sql.md)
- [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../t-sql/statements/set-quoted-identifier-transact-sql.md)
- [sp_configure &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sp_tableoption &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md)
- [File di formato per l'importazione o l'esportazione di dati &#40;SQL Server&#41;](../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]