---
title: Impostazioni del progetto di database
description: Informazioni sulle impostazioni del progetto di database. Scoprire come usare queste impostazioni per controllare gli aspetti delle configurazioni relative a database, debug e compilazione.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.DebugProperties
- sql.data.tools.dacsettings.dialog
- sql.data.tools.dbsqlclrlanguagevb
- SQL.DATA.TOOLS.DBADVANCEDBUILDSETTINGSCS
- sql.data.tools.dbadvancedbuildsettingscs
- ql.data.tools.criticalerror.dialog
- sql.data.tools.dbassemblysigningchangekeypassword
- sql.data.tools.advanceddeploymentsystemdefined.dialog
- sql.data.tools.advanceddatabasesettings.dialog
- sql.data.tools.extendedpropertiesvalueeditor.dialog
- sql.data.tools.dbsqlclrlanguagecs
- ql.data.tools.dbassemblysigningpasswordneeded
- sql.data.tools.vbadvancedsettings.dialog
- sql.data.tools.dbsqlclr
- sql.data.tools.SqlCmdVariablesProperties
- sql.data.tools.dbassemblysigning
- sql.data.tools.advanceddeploymentsettings.dialog
- sql.data.tools.advancedpublishsettings.dialog
- sql.data.tools.BuildActionProperties
- sql.data.tools.serverselectionpolicy.dialog
- sql.data.tools.dbadvancedbuildsettingsvb
- sql.data.tools.dbprojectwizard.buildeventcommandline
- sql.data.tools.dbreferencepaths
- sql.data.tools.GeneralProperties
- sql.data.tools.BuildProperties
- sql.data.tools.DeployProperties
- sql.data.tools.csadvancedsettings.dialog
- sql.data.tools.dbassemblyinfo
- sql.data.tools.extendedpropertieseditor.dialog
ms.assetid: 34418730-1aaa-4948-aee2-8f1e62cda85c
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 99552883fb06c01a862ddc973ff3747d11a89ee8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079552"
---
# <a name="database-project-settings"></a>Impostazioni del progetto di database

Le impostazioni del progetto di database vengono usate per controllare gli aspetti delle configurazioni relative a database, debug e compilazione. Tali impostazioni sono suddivise nelle categorie seguenti.  
  
-   [Impostazioni progetto](#bkmk_proj_settings)  
  
-   [Verifica Transact-SQL estesa](#bkmk_evf)  
  
-   [SQLCLR](#bkmk_sqlclr)  
  
-   [SQLCLR e Compilazione SQLCLR](#bkmk_sqlclr_sqlclrbuild)  
  
-   [Build](#bkmk_build)  
  
-   [Variabili SQLCMD](#bkmk_sqlcmd_variables)  
  
-   [Eventi di compilazione](#bkmk_build_events)  
  
-   [Eseguire il debug](#bkmk_debug)  
  
-   [Percorsi riferimento](#bkmk_ref_paths)  
  
-   [Analisi codice](#bkmk_code_analysis)  
  
### <a name="to-configure-properties-for-your-database-project"></a>Per configurare le proprietà del progetto di database  
  
1.  In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto di database per il quale si vuole configurare le proprietà, quindi scegliere **Proprietà**.  
  
    In alternativa, fare doppio clic sul nodo **Proprietà** del progetto in **Esplora soluzioni**.  
  
2.  Verrà visualizzato il foglio delle proprietà per il progetto di database.  
  
3.  Fare clic sulla scheda **Impostazioni progetto** . Ora sarà possibile configurare le proprietà generali del progetto di database. Si noti la disponibilità di varie schede (rappresentanti diverse categorie) nel riquadro sinistro.  
  
## <a name="project-settings"></a><a name="bkmk_proj_settings"></a>Impostazioni progetto  
Le impostazioni della tabella seguente si applicano a tutte le configurazioni di questo progetto di database.  
  
|Campo|Valore predefinito|Descrizione|  
|---------|-----------------|---------------|  
|Piattaforma di destinazione|Microsoft SQL Server 2012|Consente di specificare la versione di SQL Server per il progetto di database in questione.|  
|Abilita verifica di Transact\-SQL estesa per oggetti comuni.|Non abilitato quando si crea un nuovo progetto.<br /><br />Abilitato quando si crea un progetto da Esplora oggetti di SQL Server connesso a SQL Azure, si importa un database SQL di Azure nel progetto o si modifica la piattaforma di destinazione di un progetto impostandola su SQL Azure.|Quando questa opzione è abilitata, vengono segnalati gli errori rilevati nel progetto la cui verifica da parte del compilatore di SQL Server non è stata completata. Se si imposta la piattaforma di destinazione su SQL Azure, viene abilitata la verifica estesa. L'opzione non verrà deselezionata se si modifica la piattaforma di destinazione.<br /><br />È possibile abilitare questa opzione per altre versioni di SQL Server, tuttavia la convalida è limitata ai database parzialmente indipendenti di Microsoft SQL Server 2012 e a SQL Azure. Non tutta la sintassi Transact\-SQL è supportata da ogni versione di SQL Server.<br /><br />Per informazioni più dettagliate, vedere [Verifica Transact-SQL estesa](#bkmk_evf) più avanti in questo argomento|  
|Tipi di output|||  
|Applicazione livello dati (file dacpac)|Abilitato e bloccato. Tramite l'output di compilazione di un progetto di database viene sempre generato un pacchetto con estensione dacpac durante la compilazione del progetto.|Se si usa la versione di SQL Server Data Tools (SSDT) con l'opzione per creare il file aggiuntivo di una versione precedente (v2.0) con estensione dacpac, selezionare tale opzione se si vuole che il pacchetto sia compatibile con SQL Server Management Studio o il portale di gestione di SQL Azure. È possibile distribuire un pacchetto con estensione dacpac direttamente da SSDT. Tuttavia, al momento del rilascio di SQL Server Data Tools, è possibile distribuire solo un file con estensione dacpac della versione 2.0 tramite SQL Server Management Studio.|  
|Crea script (file sql)||Consente di specificare se uno script CREATE con estensione sql completo viene generato per tutti gli oggetti del progetto e inserito nella cartella bin\debug quando viene compilato il progetto. È possibile creare uno script di aggiornamento incrementale utilizzando il comando **Progetto (Pubblicazione)** o l'utilità SQL Compare.|  
|Generico|||  
|Schema predefinito|dbo|Specifica lo schema predefinito in cui vengono creati sia oggetti SQLCLR che oggetti Transact\-SQL. È possibile eseguire l'override di questa impostazione specificando lo schema direttamente negli oggetti.|  
|Includi nome dello schema nel nome file|no|Consente di specificare se nei nomi file è incluso lo schema come prefisso, ad esempio, dbo.Products.table.sql. Se questa casella di controllo è deselezionata, il formato dei nomi file per gli oggetti è NomeOggetto.TipoOggetto.sql, ad esempio Products.table.sql.|  
|Convalida utilizzo di maiuscole e minuscole negli identificatori|sì|Consente di specificare se l'utilizzo di maiuscole e minuscole negli identificatori degli oggetti SQL del progetto viene convalidato durante la compilazione del progetto. Questa opzione si applica ai progetti di database tramite cui vengono specificate le regole di confronto con distinzione tra maiuscole e minuscole per il database.|  
|Impostazioni database|Impostazioni predefinite basate sulle impostazioni di configurazione standard per un database.|Negli esempi di impostazioni che è possibile specificare sono incluse quelle relative al metodo delle regole di confronto e al livello di database per un database di SQL Server.|  
  
## <a name="extended-transact-sql-verification"></a><a name="bkmk_evf"></a>Verifica Transact-SQL estesa  
  
> [!IMPORTANT]  
> La funzionalità Verifica Transact-SQL estesa verrà rimossa a partire dalla successiva versione della funzionalità di SQL Server Data Tools e dalla successiva versione principale di Visual Studio.  
  
La verifica Transact-SQL estesa è una funzionalità disponibile nel sistema del progetto di database che consente agli sviluppatori di inviare il proprio progetto di database a Servizio di compilazione Transact-SQL in fase di compilazione per la convalida del codice rispetto al parser e all'interprete del motore di SQL Server.  
  
### <a name="transact-sql-compiler-service"></a>Transact-SQL Compiler Service  
Il Servizio di compilazione Transact-SQL è un componente basato sul motore di database di Microsoft SQL Server 2012. Questo servizio consente di convalidare la sintassi e la semantica delle istruzioni DDL con la stessa affidabilità del motore di database di Microsoft SQL Server 2012. Ciò significa implicitamente che il Servizio di compilazione non supporta la sintassi o le funzionalità deprecate in Microsoft SQL Server 2012. Per ulteriori informazioni sulle funzionalità deprecate, vedere [Funzionalità del Motore di database non più utilizzate in SQL Server 2012](../database-engine/discontinued-database-engine-functionality-in-sql-server.md).  
  
Ai fini della convalida del progetto di database, il Servizio compilazione crea un database parzialmente indipendente e simula l'esecuzione delle istruzioni DDL sul database. Per ulteriori informazioni, vedere i concetti relativi ai database parzialmente indipendenti nella pagina [Database indipendenti](/previous-versions/sql/sql-server-2012/ff929071(v=sql.110)).  
  
Il Servizio compilazione presenta limitazioni suddivise in due categorie.  
  
Funzionalità che richiedono la configurazione di un database o un'istanza, ad esempio:  
  
-   Riferimenti a oggetti in tre o quattro parti  
  
-   FileTable  
  
-   Rilevamento modifiche  
  
-   Funzioni per i set di righe: OPENROWSET, OPENQUERY, OPENDATASOURCE  
  
-   Ricerca semantica full-text  
  
Funzionalità attualmente non supportate per la convalida, ad esempio:  
  
-   Broker di servizio  
  
-   Schema partizionato con filegroup definiti dall'utente  
  
-   Regole di confronto dei metadati di SQL Azure (il Servizio compilazione utilizza un database parzialmente indipendente di SQL Server 2012, Latin1_General_100_CI_AS_KS_WS_SC)  
  
### <a name="enablingdisabling-extended-verification"></a>Abilitazione/disabilitazione della verifica estesa  
La verifica Transact-SQL estesa è abilitata per impostazione predefinita in un progetto di database creato direttamente da un database SQL di Azure o un progetto la cui piattaforma di destinazione è impostata su SQL Azure. È consigliabile utilizzare la verifica estesa quando si sviluppa per SQL Azure o un database con ambito di applicazione per SQL Server 2012. Per ulteriori informazioni sui database con ambito di applicazione, vedere [Database indipendenti](/previous-versions/sql/sql-server-2012/ff929071(v=sql.110)).  
  
È anche possibile usare la funzionalità di verifica estesa quando si sviluppa un database con ambito di applicazione per SQL Server 2008/R2 per ottenere la compatibilità con Microsoft SQL Server 2012 e SQL Azure.  
  
##### <a name="to-enable-or-disable-extended-verification-at-the-project-level"></a>Per abilitare o disabilitare la verifica estesa a livello di progetto.  
  
1.  In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul file di progetto, quindi scegliere **Proprietà**.  
  
2.  In **Impostazioni progetto** selezionare o deselezionare **Abilita verifica Transact-SQL estesa per oggetti comuni** in **Piattaforma di destinazione**.  
  
##### <a name="to-disable-extended-verification-at-the-file-level"></a>Per disabilitare la verifica estesa a livello di file.  
  
1.  In **Esplora soluzioni** fare clic con il pulsante destro del mouse su un file con estensione sql.  
  
    > [!NOTE]  
    > Per disabilitare la funzionalità di verifica Transact\-SQL estesa a livello di file, è necessario che la proprietà **Azione di compilazione** del file sia impostata su **Compilazione**.  
  
2.  In **Proprietà** modificare la proprietà **Verifica T-SQL estesa** impostandola su **False**.  
  
    ![Proprietà file](../ssdt/media/ssdt-evf.gif "Proprietà file")  
  
### <a name="special-considerations-for-collations"></a>Considerazioni speciali per le regole di confronto  
Per ulteriori informazioni riguardanti le regole di confronto nei database parzialmente indipendenti, vedere [Regole di confronto dei database indipendenti](/previous-versions/sql/sql-server-2012/ff929080(v=sql.110)).  
  
## <a name="sqlclr"></a><a name="bkmk_sqlclr"></a>SQLCLR  
Per informazioni sulle opzioni dell'assembly, vedere [Finestra di dialogo Informazioni assembly](/visualstudio/ide/reference/assembly-information-dialog-box?queryresult=true).  
  
Per informazioni sulla firma, vedere la sezione **Firma degli assembly** dell'argomento [Pagina firma, Progettazione progetti](/visualstudio/ide/reference/signing-page-project-designer?queryresult=true) .  
  
## <a name="sqlclr-and-sqlclr-build"></a><a name="bkmk_sqlclr_sqlclrbuild"></a>SQLCLR e Compilazione SQLCLR  
Nelle pagine delle proprietà **SQLCLR** e **Compilazione SQLCLR** sono contenute molte impostazioni per l'utilizzo di oggetti CLR SQL nel progetto in uso. In particolare, nella pagina delle proprietà **SQLCLR** è disponibile un'impostazione a livello di autorizzazione con cui è possibile impostare le autorizzazioni per l'assembly SQLCLR. È anche disponibile l'impostazione "Genera DDL" per controllare se vengono generate istruzioni DDL (Dynamic Data Language) per gli oggetti SQLCLR che sono stati aggiunti al progetto. Nella pagina delle proprietà **Compilazione** SQLCLR sono contenute tutte le opzioni del compilatore che è possibile impostare per configurare la compilazione di codice SQLCLR nel progetto.  
  
Nella pagina delle proprietà **Compilazione SQLCLR** sono contenute le impostazioni di compilazione avanzate per compilare oggetti CLR SQL nel progetto in uso. Vengono fornite diverse opzioni basate sul linguaggio (VB o C#) utilizzato per codificare oggetti SQL CLR.  
  
1.  Se l'oggetto viene scritto in C#, è possibile accedere alle opzioni facendo clic sul pulsante **Avanzate** nella pagina delle proprietà **Compilazione SQLCLR**. Le descrizioni delle opzioni di C# si trovano in [Finestra di dialogo Impostazioni di compilazione avanzate (C#)](/visualstudio/ide/reference/advanced-build-settings-dialog-box-csharp).  
  
2.  Se l'oggetto viene scritto in VB, è possibile scegliere VB nell'elenco a discesa **Linguaggio** e fare clic su **Avanzate** . Le descrizioni delle opzioni di VB si trovano in [Finestra di dialogo Impostazioni del compilatore avanzate (Visual Basic)](/visualstudio/ide/reference/advanced-compiler-settings-dialog-box-visual-basic)  
  

## <a name="build"></a><a name="bkmk_build"></a>Compilazione  
È possibile scegliere una configurazione della compilazione per ogni progetto di database nella soluzione. Per impostazione predefinita è presente una sola configurazione, tuttavia è possibile aggiungere configurazioni personalizzate. Ad esempio, se si desidera una configurazione personalizzata nella quale sia sempre possibile eliminare e ricreare il database. Nelle soluzioni che contengono diversi tipi di progetto si può creare una configurazione personalizzata della soluzione che contiene una specifica configurazione della compilazione per ogni progetto.  
  
#### <a name="to-specify-a-build-configuration-for-a-solution"></a>Per specificare una configurazione della compilazione per una soluzione  
  
1.  In **Esplora soluzioni** fare clic sul nodo della soluzione per la quale si vuole specificare una configurazione della compilazione.  
  
2.  Scegliere **Configuration Manager** dal menu **Compila**. Verrà visualizzata finestra di dialogo **Gestione configurazione**.  
  
    Specificare le impostazioni di configurazione che si desidera utilizzare per ogni progetto nella soluzione.  
  
#### <a name="to-specify-a-build-configuration-for-a-database-project"></a>Per specificare una configurazione della compilazione per un progetto di database  
  
1.  In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto di database per il quale si vuole specificare una configurazione della build e scegliere **Proprietà**.  
  
2.  Nella scheda **Compila** usare l'elenco a discesa **Configurazione** per specificare le impostazioni di configurazione che si vuole usare per questo progetto.  
  
Le impostazioni della tabella seguente si applicano a tutte le configurazioni della compilazione di questo progetto di database.  
  
|Campo|Valore predefinito|Descrizione|  
|---------|-----------------|---------------|  
|Percorso dell'output di compilazione|bin\Debug\|Specifica dove viene generato l'output di compilazione quando si compila o si distribuisce il progetto di database. Se si specifica un percorso relativo, è necessario correlarlo al percorso del progetto di database. Se il percorso non esiste, viene creato.|  
|Nome file di output di compilazione|*DatabaseProjectName*|Consente di specificare il nome che si desidera assegnare all'output che viene generato quando si compila il progetto di database.|  
|Considera avvisi Transact\-SQL come errori|No|Specifica se un avviso di Transact\-SQL deve causare l'annullamento del processo di compilazione e distribuzione. Se questa casella di controllo è deselezionata, verranno visualizzati gli avvisi ma continuerà il processo di compilazione e distribuzione. Si tratta di un'impostazione specifica del progetto, non dell'utente, e viene archiviata nel file con estensione sqlproj.|  
|Non visualizzare avvisi Transact\-SQL|Vuoto|Consente di specificare un elenco dei numeri degli avvisi, delimitati da virgole o da punti e virgola, tramite cui è possibile identificare gli avvisi annullati.<br /><br />Gli avvisi annullati non vengono visualizzati nella finestra **Elenco errori** e non influiscono sul completamento della compilazione, anche se si seleziona la casella di controllo **Considera avvisi Transact\-SQL come errori**.|  
  
## <a name="sqlcmd-variables"></a><a name="bkmk_sqlcmd_variables"></a>Variabili SQLCMD  
Le variabili SQLCMD possono essere utilizzate nei progetti di database di SQL Server per fornire sostituzioni dinamiche utilizzabili per il debug o la pubblicazione. Si immettono il nome della variabile e i valori e, durante la compilazione, questi ultimi verranno sostituiti. Se non sono presenti valori locali, verrà utilizzato il valore predefinito. Se si immettono queste variabili nelle proprietà del progetto, esse verranno utilizzate automaticamente nella pubblicazione e vengono archiviate nei profili di pubblicazione. È possibile effettuare il pull nei valori del progetto delle variabili nella pubblicazione tramite il pulsante Carica valori.  
  
Assicurarsi che siano state immesse le variabili corrette nelle proprietà del progetto, dal momento che queste variabili non vengono convalidate in uno script del progetto, né si tratta delle variabili utilizzate nello script popolato automaticamente.  
  
Inoltre, la pubblicazione sulla riga di comando consente di eseguire l'override di questi valori sulla riga di comando o mediante un profilo.  
  
## <a name="build-events"></a><a name="bkmk_build_events"></a>Eventi di compilazione  
È possibile utilizzare queste impostazioni per specificare una riga di comando da eseguire prima dell'inizio dell'operazione di compilazione e un'altra da eseguire dopo il completamento dell'operazione di compilazione.  
  
|Campo|Valore predefinito|Descrizione|  
|---------|-----------------|---------------|  
|Riga di comando eventi pre-compilazione|nessuno|Specifica la riga di comando da eseguire prima della compilazione del progetto. Scegliere **Modifica pre-compilazione** per modificare la riga di comando.|  
|Riga di comando eventi post-compilazione|nessuno|Consente di specificare la riga di comando da eseguire dopo la compilazione del progetto. Scegliere **Modifica post-compilazione** per modificare la riga di comando.|  
|Esegui evento post-compilazione|On successful build|Consente di specificare se la riga di comando post-compilazione deve essere eseguita sempre, solo a compilazione completata o solo quando la compilazione aggiorna l'output del progetto (script di compilazione).|  
  
## <a name="debug"></a><a name="bkmk_debug"></a>Debug  
È possibile utilizzare queste impostazioni per controllare il debug del progetto di database.  
  
|Campo|Valore predefinito|Descrizione|  
|---------|-----------------|---------------|  
|Azione di avvio|nessuno|Consente di specificare uno script o un programma esterno da eseguire durante il debug del progetto in uso.|  
|Stringa di connessione di destinazione|Data Source=(localdb)\\*NomeSoluzione*;Initial Catalog=*NomeProgettoDatabase*;Integrated Security=True;Pooling=False;Connect Timeout=30|Consente di specificare le informazioni di connessione per il server di database da utilizzare come destinazione per la configurazione della compilazione specificata. La stringa di connessione predefinita si trova in un'istanza del database locale di SQL Server creata dinamicamente e in un database.|  
|Distribuisci proprietà database|Sì|Specifica se le impostazioni DatabaseProperties. DatabaseProperties vengono distribuite o aggiornate quando si distribuisce il progetto di database.|  
|Ricrea sempre database|No|Consente di specificare se eliminare e ricreare il database anziché eseguire un aggiornamento incrementale. Può essere necessario selezionare questa casella di controllo se, ad esempio, si vuole eseguire gli unit test del database per una distribuzione pulita del database. Se questa casella di controllo è deselezionata, il database esistente verrà aggiornato, non eliminato e ricreato.|  
|Blocca distribuzione incrementale in caso di perdita di dati|Sì|Consente di specificare se la distribuzione viene arrestata nel caso in cui un aggiornamento possa provocare una perdita di dati. Se viene selezionata questa casella di controllo, le modifiche che causerebbero una perdita di dati comportano l'arresto della distribuzione e la visualizzazione di un errore, pertanto i dati non vengono persi. Ad esempio la distribuzione viene arrestata se una colonna `varchar(50)` viene modificata in `varchar(30)`.<br /><br />**NOTA** La distribuzione viene bloccata solo se le tabelle in cui può verificarsi la perdita di dati contengono dati. Se non si perde alcun dato, la distribuzione continua.|  
|Esegui istruzioni DROP su oggetti nel database di destinazione ma non nel progetto|No|Consente di specificare se gli oggetti presenti nel database di destinazione, ma non nel progetto di database, devono essere eliminati come parte dello script di distribuzione. È possibile escludere alcuni file nel progetto per rimuoverli temporaneamente dallo script di compilazione. Tuttavia, potrebbe essere necessario lasciare le versioni esistenti di tali oggetti nel database di destinazione. Questa casella di controllo non ha effetto se è selezionata la casella di controllo **Ricrea sempre database**, perché il database verrà eliminato.|  
|Non utilizzare le istruzioni ALTER ASSEMBLY per aggiornare i tipi CLR|No|Consente di specificare se vengono utilizzate le istruzioni ALTER ASSEMBLY per aggiornare i tipi CLR (Common Language Runtime) o se l'oggetto che consente di creare istanze del tipo CLR verrà invece eliminato e ricreato quando vengono distribuite le modifiche.|  
|Avanzate|No|Pulsante di comando che consente di specificare le opzioni tramite cui vengono controllati eventi e comportamenti per la distribuzione.|  
  
## <a name="reference-paths"></a><a name="bkmk_ref_paths"></a>Percorsi riferimento  
È possibile utilizzare questa pagina per definire le variabili di server e database associate a un riferimento tra database. Inoltre, è possibile specificare i valori di tali variabili. Per altre informazioni, vedere [Uso di riferimenti in progetti di database](/previous-versions/visualstudio/visual-studio-2010/bb386242(v=vs.100)).  
  
## <a name="code-analysis"></a><a name="bkmk_code_analysis"></a>Analisi codice  
È possibile utilizzare l'analisi codice per individuare potenziali problemi negli script, ad esempio problemi di progettazione, denominazione e di prestazioni. Le regole per i progetti di database sono organizzate in set di regole predefiniti destinati ad aree specifiche. Ogni singola regola può essere abilitata o disabilitata nella scheda **Analisi codice** della pagina delle proprietà **Proprietà progetto** . Nella stessa scheda è possibile specificare che l'analisi codice venga eseguita automaticamente ogni volta che viene compilato un progetto o nel caso in cui gli avvisi vengano considerati come errori.  
  
Per usare l'analisi codice manualmente, fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Esegui analisi del codice**. Nella finestra **Elenco errori** vengono visualizzati gli avvisi dell'analisi codice. È possibile fare doppio clic su un avviso per passare al codice di origine contenente il problema. Inoltre, si possono visualizzare informazioni aggiuntive e le possibili correzioni per un avviso usando il menu contestuale **Mostra guida errore**. Per altre informazioni sull'analisi codice, vedere [Analisi del codice di database per migliorare la qualità del codice](/previous-versions/visualstudio/visual-studio-2010/dd172133(v=vs.100)).  
