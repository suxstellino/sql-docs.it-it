---
title: Opzioni dell'editor Transact-SQL
description: Acquisire familiarità con le opzioni dell'editor Transact-SQL. Informazioni sulle proprietà di esecuzione delle query, sulle proprietà dei risultati delle query e su come modificare i valori.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.SqlExecutionAdvancedSettingsOption
- sql.data.tools.SqlExecutionAnsiSettingsDlg
- sql.data.tools.SqlResultToTextSettingsDlg
- sql.data.tools.SqlExecutionGeneralSettingsDlg
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_RESULTS.GENERAL
- sql.data.tools.unittesting.tsqleditor
- sql.data.tools.SqlResultsToGridSettingsDlg
- sql.data.tools.queryresults.executequeryfailure
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.GENERAL
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.EDITOR_TAB_AND_STATUS_BAR
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.GENERAL
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_RESULTS.RESULTS_TO_TEXT
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_EXECUTION.ANSI
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_EXECUTION.GENERAL
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_RESULTS.RESULTS_TO_GRID
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.ONLINE_EDITING
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_EXECUTION.ADVANCED
ms.assetid: fa9a250f-7feb-433e-91bd-a09779d74c8b
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 6ae9da5c98d625843bc01bba5d12276098e3e3f1
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98594452"
---
# <a name="transact-sql-editor-options"></a>Opzioni dell'editor Transact-SQL

In questo argomento sono contenute informazioni su alcune opzioni dell'editor Transact-SQL. Per impostare queste opzioni, passare alla finestra **Opzione** tramite il menu **Strumenti\Opzioni**.  
  
[Esecuzione di query](#QueryExecution)  
  
[Risultati query](#QueryResults)  
  
## <a name="query-execution"></a><a name="QueryExecution"></a>Esecuzione di query  
  
|Proprietà|Descrizione|  
|------------|---------------|  
|**SET ROWCOUNT**|Il valore predefinito 0 indica che SQL Server rimarrà in attesa di risultati fino a quando non verranno ricevuti tutti i risultati. Specificare un valore maggiore di 0 se si desidera che in SQL Server la query venga interrotta dopo aver ottenuto il numero di righe specificato. Per disabilitare questa opzione in modo che vengano restituite tutte le righe, specificare SET ROWCOUNT 0.|  
|**SET TEXTSIZE**|Il valore predefinito di 2.147.483.647 byte indica che in SQL Server sarà disponibile un campo dati completo fino al limite dei campi dati text, ntext, nvarchar(max) e varchar(max). Ciò non ha alcun effetto sul tipo di dati XML. Specificare un numero più piccolo per limitare i risultati in caso di valori elevati. Le colonne le cui dimensioni sono maggiori del numero specificato verranno troncate.|  
|**Timeout esecuzione**|Consente di indicare il numero di secondi di attesa prima dell'annullamento della query. Il valore 0 indica un'attesa infinita, ovvero nessun timeout.|  
|**Per impostazione predefinita, apri le nuove query in modalità SQLCMD**|Selezionare questa casella di controllo per aprire le nuove query in modalità SQLCMD. Questa casella di controllo è disponibile solo se la finestra di dialogo è stata aperta tramite il menu Strumenti.<br /><br />Quando si seleziona questa opzione, tenere presente le limitazioni seguenti:<br /><br />Nell'editor di query del motore di database, IntelliSense è disattivata.<br />- Poiché l'editor di query non è in esecuzione dalla riga di comando, non è possibile passare parametri della riga di comando, ad esempio variabili.<br />- Poiché l'editor di query non può rispondere ai prompt del sistema operativo, è necessario prestare attenzione a non eseguire istruzioni interattive.|  
|**SET NOCOUNT**|Arresta la restituzione come parte dei risultati del messaggio che indica il numero di righe interessate da un'istruzione Transact-SQL. Per ulteriori informazioni, vedere [SET NOCOUNT](../t-sql/statements/set-nocount-transact-sql.md).|  
|**SET NOEXEC**|Quando è impostata su **ON**, indica a Microsoft® SQL Server™ di compilare ogni batch di istruzioni Transact-SQL, ma di non eseguirle. Quando invece è impostata su **OFF**, indica a Microsoft® SQL Server™ di eseguire tutti i batch dopo la compilazione. Per altre informazioni, vedere [SET NOEXEC](../t-sql/statements/set-noexec-transact-sql.md).|  
|**SET PARSEONLY**|Controlla la sintassi di ogni istruzione Transact-SQL e restituisce eventuali messaggi di errore senza compilare o eseguire l'istruzione. Per ulteriori informazioni, vedere [SET PARSEONLY](../t-sql/statements/set-parseonly-transact-sql.md).|  
|**SET CONCAT_NULL_YIELDS_NULL**|Controlla se i risultati della concatenazione vengono considerati come valori stringa Null o vuoti. Per altre informazioni, vedere [SET CONCAT_NULL_YIELDS_NULL](../t-sql/statements/set-concat-null-yields-null-transact-sql.md).|  
|**SET ARITHABORT**|Interrompe una query quando si verifica un errore di divisione per zero o di overflow durante l'esecuzione della query stessa. Per altre informazioni, vedere [SET ARITHABORT](https://msdn.microsoft.com/library/aa259212(v=SQL.80).aspx).|  
|**SET SHOWPLAN_TEXT**|Impedisce l'esecuzione di istruzioni Transact-SQL di Microsoft® SQL Server™ e vengono restituite invece informazioni dettagliate sulla modalità di esecuzione delle istruzioni. Per altre informazioni, vedere [SET SHOWPLAN_TEXT](../t-sql/statements/set-showplan-text-transact-sql.md).|  
|**SET STATISTICS TIME**|Visualizza il numero di millisecondi necessari per l'analisi, la compilazione e l'esecuzione di ogni istruzione.|  
|**SET STATISTICS IO**|Determina in Microsoft® SQL Server™ la visualizzazione di informazioni sulla quantità di attività del disco generata da istruzioni Transact-SQL.|  
|**SET TRANSACTION ISOLATION LEVEL**|Controlla il comportamento predefinito di blocco delle transazioni per tutte le istruzioni **SELECT** di Microsoft® SQL Server™ eseguite da una connessione. Per ulteriori informazioni, vedere  [SET TRANSACTION ISOLATION LEVEL](../t-sql/statements/set-transaction-isolation-level-transact-sql.md).|  
|**SET LOCK_TIMEOUT**|Specifica l'intervallo in millisecondi durante il quale un'istruzione rimane in attesa del rilascio di un blocco. Per altre informazioni, vedere [SET LOCK_TIMEOUT](../t-sql/statements/set-lock-timeout-transact-sql.md).|  
|**SET QUERY_GOVERNOR_COST_LIMIT**|Esegue l'override del valore configurato attualmente per la connessione corrente. Per altre informazioni, vedere [SET QUERY_GOVERNOR_COST_LIMIT](../t-sql/statements/set-query-governor-cost-limit-transact-sql.md).|  
|**SET ANSI_DEFAULTS**|Controlla un gruppo di impostazioni di Microsoft® SQL Server™ che specificano complessivamente alcuni comportamenti standard di SQL-92. Per altre informazioni, vedere [SET ANSI_DEFAULTS](../t-sql/statements/set-ansi-defaults-transact-sql.md).|  
|**SET QUOTED_IDENTIFIER**|Impone a Microsoft® SQL Server™ di seguire le regole di SQL-92 relative alle virgolette come identificatori di delimitazione e alle stringhe letterali. Gli identificatori delimitati da virgolette doppie possono essere parole chiave riservate di Transact-SQL o possono includere caratteri in genere non consentiti in base alle regole di sintassi per gli identificatori di Transact-SQL. Per altre informazioni, vedere [SET QUOTED_IDENTIFIER](../t-sql/statements/set-quoted-identifier-transact-sql.md).|  
|**SET ANSI_NULL_DFLT_ON**|Modifica il comportamento della sessione in modo da ignorare l'impostazione predefinita relativa al supporto di valori Null ANSI per tutte le nuove colonne, quando l'opzione predefinita di database è false. Per altre informazioni, vedere [SET ANSI_NULL_DFLT_ON](../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md).|  
|**SET IMPLICIT_TRANSACTIONS**|Quando impostata su **ON**, definisce la modalità di transazione implicita per la connessione. Quando impostata su **OFF**, restituisce la modalità di transazione con autocommit per la connessione. Per altre informazioni, vedere [SET IMPLICIT_TRANSACTIONS](../t-sql/statements/set-implicit-transactions-transact-sql.md).|  
|**SET CURSOR_CLOSE_ON_COMMIT**|Controlla se il cursore non viene chiuso quando una transazione viene confermata. Per altre informazioni, vedere [SET CURSOR_CLOSE_ON_COMMIT](../t-sql/statements/set-cursor-close-on-commit-transact-sql.md).|  
|**SET ANSI_PADDING**|Controlla la modalità di archiviazione nella colonna dei valori di dimensioni inferiori alle dimensioni definite per la colonna e dei valori che includono spazi vuoti finali con tipo di dati **char**, **varchar**, **binary** e **varbinary** . Per altre informazioni, vedere [SET ANSI_PADDING](../t-sql/statements/set-ansi-padding-transact-sql.md).|  
|**SET ANSI_WARNINGS**|Specifica il comportamento standard SQL-92 per diverse condizioni di errore. Per altre informazioni, vedere [SET ANSI_WARNINGS](../t-sql/statements/set-ansi-warnings-transact-sql.md).|  
|**SET ANSI_NULLS**|Specifica la conformità del comportamento agli standard SQL-92 per gli operatori di confronto uguale a ( **=** ) e diverso da ( **<>** ) quando vengono usati con valori Null. Per altre informazioni, vedere [SET ANSI_NULLS](../t-sql/statements/set-ansi-nulls-transact-sql.md).|  
  
## <a name="query-results"></a><a name="QueryResults"></a>Risultati query  
  
|Proprietà|Descrizione|  
|------------|---------------|  
|**Includi la query nel set di risultati**|Restituisce il testo della query come parte del set di risultati.|  
|**Includi intestazioni di colonna nelle operazioni di copia o salvataggio dei risultati**|Consente di includere le intestazioni di colonna (titoli) durante la copia dei risultati negli Appunti o il salvataggio in un file. Deselezionare questa casella di controllo se si desidera che i risultati salvati o copiati contengano solo i dati senza le intestazioni di colonna.|  
|**Elimina risultati dopo l'esecuzione**|Consente di liberare memoria eliminando i risultati delle query dopo la loro visualizzazione.|  
|**Visualizza risultati in una scheda separata**|Consente di visualizzare il set dei risultati in una nuova finestra del documento anziché nella parte inferiore della finestra del documento della query.|  
|**Passa alla scheda dei risultati al termine della query**|Consente di impostare automaticamente lo stato attivo dello schermo sul set dei risultati.|  
|**Dimensioni massime caratteri recuperati**|Dati non XML:<br /><br />Consente di immettere un valore compreso tra 1 e 65535 per specificare il numero massimo di caratteri che sarà possibile visualizzare in ogni cella. **Nota:** Se si specifica un numero elevato di caratteri, i dati nel set di risultati potrebbero non essere visualizzati completamente. Il numero massimo di caratteri visualizzati in ogni cella dipende dalle dimensioni del carattere. Se si specifica un valore elevato in questa casella e vengono restituiti set di risultati di notevoli dimensioni, la memoria per SQL Server Management Studio potrebbe risultare insufficiente con effetti negativi sulle prestazioni del sistema.<br /><br />Dati XML<br /><br />Consente di selezionare i valori 1 MB, 2 MB o 5 MB. Selezionare Illimitate per recuperare tutti i caratteri.|  
|**Formato di output**|Per impostazione predefinita, l'output viene visualizzato in colonne create utilizzando gli spazi per separare i risultati. Per separare le colonne, è inoltre possibile utilizzare virgole, caratteri di tabulazione o spazi. Selezionare la casella di controllo **Delimitatore personalizzato** per specificare un carattere di delimitazione differente nella casella **Delimitatore personalizzato** .|  
|**Delimitatore personalizzato**|Consente di specificare il carattere che si desidera utilizzare per separare le colonne. Questa opzione è disponibile solo se la casella di controllo **Delimitatore personalizzato** è selezionata nella finestra **Formato di output** .|  
|**Includi intestazioni di colonna nel set di risultati**|Deselezionare questa casella di controllo se non si vuole applicare un titolo alle colonne.|  
|**Scorri i risultati ricevuti**|Selezionare questa casella di controllo per mantenere visualizzati gli ultimi record restituiti nella parte inferiore. Deselezionare la casella per mantenere visualizzata la prima riga restituita.|  
|**Allinea a destra i valori numerici**|Selezionare questa casella di controllo per allineare i valori numeri a destra nella colonna in modo da semplificare l'esame delle cifre con un numero di posizioni decimali predefinito.|  
|**Elimina risultati dopo l'esecuzione della query**|Libera la memoria eliminando i risultati della query dopo che sono stati visualizzati.|  
|**Visualizza risultati in una scheda separata**|Selezionare questa casella di controllo per visualizzare il set di risultati in una nuova finestra del documento anziché nella parte inferiore della finestra della query.|  
|**Passa alla scheda dei risultati al termine della query**|Selezionare questa casella per impostare lo stato attivo dello schermo sul set di risultati.|  
|**Numero massimo di caratteri visualizzati in ogni colonna**|Questo valore viene impostato in modo predefinito su 256. È possibile scegliere un valore superiore per visualizzare set di risultati di dimensioni maggiori senza troncamenti.|  
|**Ripristina predefiniti**|Reimposta le impostazioni predefinite originali per tutti i valori nella pagina.|  
