---
title: Note sulla versione di Azure Data Studio
description: Questo articolo contiene le note sulla versione per le versioni di Azure Data Studio da novembre 2017 a oggi. Per molti dei problemi riportati in modo sintetico sono disponibili collegamenti a ulteriori dettagli.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan
ms.custom: seodec18
ms.date: 2/18/2021
ms.openlocfilehash: c666ae644200c1b2beefb7f73c8a819294831985
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836028"
---
# <a name="release-notes-for-azure-data-studio"></a>Note sulla versione per Azure Data Studio

**[Scarica e installa l'ultima versione](./download-azure-data-studio.md)**

## <a name="february-2021"></a>2021 febbraio

18 febbraio 2021 &nbsp;  /  &nbsp; versione: 1.26.0

&nbsp;

| Modifica | Dettagli |
| ------ | ------- |
| Correzioni di bug | Per un elenco completo delle correzioni, vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22February+2021+Release%22+is%3Aclosed). |
| Aggiornamento delle estensioni | [Dacpac](extensions/sql-server-dacpac-extension.md) <br/> [Kusto (KQL)](extensions/kusto-extension.md) </br> [MachineLearning](extensions/machine-learning-extension.md) </br> [Profiler](extensions/sql-server-profiler-extension.md) </br> [SchemaCompare](extensions/schema-compare-extension.md) </br> [SQLDatabaseProjects](extensions/sql-database-project-extension.md) |
| Nuove funzionalità di Azure Arc | Sono ora supportati più controller dati <br/> Nuove opzioni della finestra di dialogo di connessione come il file di configurazione Kube <br/> Miglioramenti del dashboard Postgres |
| Nuove funzionalità del notebook | Miglioramento del tempo di avvio del server Jupyter del 50% per Windows <br/> Aggiunta del supporto per la modifica dei libri Jupyter tramite clic con il pulsante destro del mouse <br/> Aggiunta del supporto per la parametrizzazione del notebook URI e [aggiunta della relativa documentazione](https://docs.microsoft.com/sql/azure-data-studio/notebooks/notebooks-parameterization) |

## <a name="december-2020-hotfix"></a>2020 dicembre (hotfix)

10 febbraio 2021 &nbsp;  /  &nbsp; versione: 1.25.3

| Modifica | Dettagli |
| ------ | ------- |
| Correzione [#13899](https://github.com/microsoft/azuredatastudio/issues/13899) bug | Scorrimento ai collegamenti di riferimento incrociato appropriati nei notebook |
| Aggiornare l'elettrone per incorporare correzioni di bug importanti| N/D |

## <a name="december-2020"></a>Dicembre 2020

9 dicembre 2020 &nbsp; / &nbsp; versione: 1.25.0

&nbsp;

| Modifica | Dettagli |
| ------ | ------- |
| Correzioni di bug | Per un elenco completo delle correzioni, vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%22December+2020+Release%22). |
| Aggiornamento dell'estensione progetti di database | Aggiunta di aree di lavoro e miglioramento della barra laterale |

## <a name="november-2020"></a>Novembre 2020

12 November 2020 &nbsp; / &nbsp; versione: 1.24.0

&nbsp;

| Modifica | Dettagli |
| ------ | ------- |
| Correzioni di bug | Per un elenco completo delle correzioni, vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22November+2020+Release%22+is%3Aclosed). |
| Finestra di dialogo di connessione | Aggiunta della nuova scheda Sfoglia per la finestra di dialogo di connessione. |
| Aggiornamento delle estensioni | Rilascio dell'aggiornamento nell'estensione Postgres. |
| Nuove funzionalità dei notebook | Aggiunta di nuove funzionalità a SQL per il supporto di notebook. <br/> Aggiunta di nuove funzionalità al supporto per la parametrizzazione di notebook. <br/>  Aggiunta di nuove funzionalità al flusso dei risultati per i notebook SQL. |
| Installazione di Python | Rimozione del pacchetto PROSE dall'installazione predefinita di Python. |

### <a name="known-issues-1240"></a>Problemi noti (1.24.0)

| Nuovo elemento | Dettagli | Soluzione alternativa |
|----------|---------|------------|
| Estensione Azure Arc | [Problema noto:](https://github.com/microsoft/azuredatastudio/issues/13319) il pulsante "Genera script nel notebook" per le distribuzioni di Arc per MIAA e PG non esegue la convalida dei campi prima di generare script per il notebook. Ciò significa che se gli utenti immettono una password errata negli input di conferma della password, potrebbero avere come risultato un notebook con il valore errato per la password.| Il pulsante "Distribuisci" funziona come previsto, gli utenti possono usare questo. |
| Esplora oggetti | Le versioni di ADS precedenti alla 1.24.0 presentano una modifica sostanziale in Esplora oggetti a causa delle modifiche del motore relative al [pool SQL serverless in Azure Synapse Analytics](/azure/synapse-analytics/sql/on-demand-workspace-overview). | Per continuare a usare Esplora oggetti in Azure Data Studio con il pool SQL serverless in Azure Synapse Analytics, è necessario usare Azure Data Studio 1.24.0 o versione successiva. |

È possibile fare riferimento al [sito dei commenti e suggerimenti per Azure Data Studio](https://github.com/microsoft/azuredatastudio) per informazioni su altri problemi noti e per inviare commenti e suggerimenti al team del prodotto.

## <a name="october-2020"></a>Ottobre 2020

14 ottobre 2020 &nbsp; / &nbsp; versione: 1.23.0

&nbsp;

| Modifica | Dettagli |
| ------ | ------- |
| SQL Edge di Azure | Supporto per gli oggetti SQL Edge di Azure. |
| Correzioni di bug | Per un elenco completo delle correzioni, vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is:issue+milestone:%22October+2020+Release%22+is:closed). |
| Database| Supporto per lo stesso riferimento a database. |
| Aggiornamenti delle estensioni | [Azure Arc](extensions/azure-arc-extension.md)</br>[azdata](../azdata/install/deploy-install-azdata.md)</br>[Machine Learning](extensions/machine-learning-extension.md)</br>[Kusto (KQL)](extensions/kusto-extension.md)</br>[Confronto schema](extensions/schema-compare-extension.md)</br>SQL Assessment</br>[Progetti di database SQL](extensions/sql-database-project-extension.md)</br>[SQL Server Import](extensions/sql-server-import-extension.md) |
| Nuove funzionalità di distribuzione | Aggiunta di distribuzioni di macchine virtuali e database SQL di Azure. |
| PowerShell | Aggiunta del supporto per lo streaming dei risultati del kernel PowerShell. |

## <a name="september-2020-hotfix"></a>Settembre 2020 (hotfix)

30 settembre 2020 &nbsp; / &nbsp; versione: 1.22.1

&nbsp;

| Modifica | Dettagli |
| ------ | ------- |
| Bug e problemi risolti | Per un elenco completo delle correzioni, vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/releases/tag/1.22.1). |

## <a name="september-2020"></a>Settembre 2020

22 settembre 2020 &nbsp; / &nbsp; versione: 1.22.0

&nbsp;

| Modifica | Dettagli |
| ------ | ------- |
| Nuove funzionalità dei notebook | <br/> &bull; &nbsp; Supporta un nuovissimo strumento di modifica delle celle di testo basato sulla formattazione RTF e la conversione trasparente in markdown, nota anche come barra degli strumenti WYSIWYG (What You See Is What You Get) <br/> &bull; &nbsp; Supporta il kernel Kusto <br/> &bull; &nbsp; Supporta l'aggiunta di notebook <br/> &bull; &nbsp; Aggiunto il supporto per la nuova versione dei book di Jupyter <br/> &bull; &nbsp; Miglioramento dei collegamenti di Jupyter <br/> &bull; &nbsp; Introdotti miglioramenti del caricamento delle prestazioni |
| Estensione progetti di database SQL | L'estensione progetti di database SQL consente lo sviluppo di database basati su progetto per Azure Data Studio. In questa versione di anteprima, i progetti SQL possono essere creati e pubblicati in Azure Data Studio. |
| Estensione Kusto (KQL) | Offre esperienze Kusto native in Azure Data Studio per l'esplorazione e l'analisi dei dati rispetto a una grande quantità di dati in streaming in tempo reale archiviati in Esplora dati di Azure. Questa versione di anteprima supporta la connessione e l'esplorazione dei cluster Esplora dati di Azure, la scrittura di query KQL e la creazione di notebook con il kernel Kusto. |
| Estensione Azure Arc | Gli utenti possono provare l'anteprima pubblica di Azure Arc con Azure Data Studio. ad esempio: <br/> &bull; &nbsp; Distribuire un controller dati <br/> &bull; &nbsp; Distribuire Postgres <br/> &bull; &nbsp; Distribuire Istanza gestita per Azure Arc <br/> &bull; &nbsp; Eseguire la connessione al controller dati <br/> &bull; &nbsp; Accedere ai dashboard del servizio dati <br/> &bull; &nbsp; Book di Jupyter Azure Arc |
| Opzioni di distribuzione | <br/> &bull; &nbsp; Database SQL Edge di Azure <br/> (Edge richiederà l'estensione per la distribuzione di SQL Edge di Azure) |
| Disponibilità generale dell'estensione SQL Server Import | Con l'annuncio della disponibilità generale dell'estensione SQL Server Import, le funzionalità non sono più disponibili in anteprima. Questa estensione semplifica l'importazione dei file CSV/TXT. Per altre informazioni sull'estensione, vedere [questo articolo](./extensions/sql-server-import-extension.md). |
| Bug e problemi risolti | Per un elenco completo delle correzioni, vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22September+2020+Release%22+is%3Aclosed). |

## <a name="august-2020"></a>Agosto 2020

12 agosto 2020 &nbsp; / &nbsp; versione: 1.21.0

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Nuove funzionalità dei notebook | &bull; &nbsp; Spostamento delle posizioni delle celle <br/> &bull; &nbsp; Conversione delle celle in cella di testo o cella di codice
| Selezione di book di Jupyter | Gli utenti possono ora scegliere book di Jupyter dalle release di GitHub e aprirli senza problemi in Azure Data Studio |
| Aggiunta della ricerca al viewlet di Notebooks | Gli utenti possono eseguire con facilità ricerche nel contenuto dei notebook e dei book di Jupyter |
| Bug e problemi risolti | Per un elenco completo delle correzioni, vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22August+2020+Release%22+is%3Aclosed). |
| &nbsp; | &nbsp; |

## <a name="july-2020-hotfix"></a>Luglio 2020 (hotfix)

17 luglio 2020 &nbsp; / &nbsp; versione: 1.20.1

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Correzione del bug 11372 relativo al wrapping non corretto dei nomi di tabella dopo il trascinamento delle tabelle in Esplora oggetti | [11372](https://github.com/microsoft/azuredatastudio/issues/11372) |
| Correzione del bug 11356 relativo al tema scuro impostato come predefinito | [11356](https://github.com/microsoft/azuredatastudio/issues/11356) |
| &nbsp; | &nbsp; |

### <a name="known-issue"></a>Problema noto

- Alcuni utenti hanno segnalato errori di connessione dal nuovo Microsoft.Data.SqlClient v2.0.0 incluso in questa versione. Gli utenti sono riusciti a connettersi correttamente [seguendo queste istruzioni](https://github.com/microsoft/azuredatastudio/issues/11367#issuecomment-659614111)

## <a name="july-2020"></a>Luglio 2020

15 luglio 2020 &nbsp; / &nbsp; versione: 1.20.0

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Aggiunta della presentazione delle nuove funzionalità | Dalla home page e dal riquadro comandi, gli utenti possono ora avviare una presentazione che illustra in modo dettagliato le funzionalità di uso comune, tra cui il viewlet Connessioni, il viewlet Notebooks e il Marketplace Estensioni |
| Nuove funzionalità dei notebook | &bull; &nbsp; Supporto per le intestazione nella barra degli strumenti di Markdown<br/> &bull; &nbsp; Anteprima di Markdown affiancata nelle celle di testo
| Trascinare colonne e tabelle nell'editor di query | Gli utenti possono ora trascinare colonne e tabelle direttamente dal viewlet Connessioni all'editor di query |
| Aggiunta dell'icona Account Azure alla barra delle attività | Gli utenti possono ora visualizzare facilmente la posizione da cui accedere ad Azure |
| Bug e problemi risolti | Per un elenco completo delle correzioni, vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22July+2020+Release%22+is%3Aclosed). |
| &nbsp; | &nbsp; |

## <a name="june-2020"></a>Giugno 2020

15 giugno 2020 &nbsp; / &nbsp; versione: 1.19.0

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Aggiunta di Azure Data Studio all'integrazione del portale di Azure | Gli utenti possono ora passare direttamente al portale di Azure da una connessione del database SQL di Azure, da Azure Postgres e da altre posizioni. |
| Nuove funzionalità dei notebook | &bull; &nbsp; Nuova barra degli strumenti Notebook <br/> &bull; &nbsp; Nuova barra degli strumenti Modifica cella <br/> &bull; &nbsp; Aggiornamenti dell'esperienza utente della procedura guidata delle dipendenze Python <br/> &bull; &nbsp; Spaziatura migliorata tra notebook |
| Annuncio dell'estensione API Valutazione SQL | Questa estensione aggiunge la valutazione basata su procedure consigliate SQL Server in Sicurezza dei dati avanzata. Espone l'API Valutazione SQL, che in precedenza era disponibile solo per l'uso nel modulo SqlServer di PowerShell e in SMO, per consentire la valutazione delle istanze di SQL Server e la ricezione di consigli inviati dal team di SQL Server. Altre informazioni sull'API Valutazione SQL e sulle sue funzionalità sono disponibili [in questo articolo.](../tools/sql-assessment-api/sql-assessment-api-overview.md) |
| [Miglioramenti all'estensione Machine Learning](./extensions/machine-learning-extension.md) | Supporto di Istanza gestita di SQL di Azure. |
| Miglioramenti dell'estensione di virtualizzazione dei dati | Supporto di MongoDB e Teradata |
| Correzione di bug dell'estensione Postgres | Correzione di Azure MFA |
| Bug e problemi risolti | Per un elenco completo delle correzioni, vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22June+2020+Release%22+is%3Aclosed). |
| &nbsp; | &nbsp; |

## <a name="may-2020-hotfix"></a>Maggio 2020 (hotfix)

27 maggio 2020 &nbsp; / &nbsp; versione: 1.18.1

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Correzione del bug 10538 relativo al tasto di scelta rapida "Esegui query corrente" che non si comportava più come previsto | [10538](https://github.com/microsoft/azuredatastudio/issues/10538)  |
| Correzione del bug 10537 relativo all'impossibilità di aprire file sql nuovi o esistenti nella versione 1.18 | [10537](https://github.com/microsoft/azuredatastudio/issues/10537)  |
| &nbsp; | &nbsp; |

## <a name="may-2020"></a>Maggio 2020

20 maggio 2020 &nbsp; / &nbsp; versione: 1.18.0

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Annuncio dell'estensione Redgate SQL Prompt | Questa estensione consente di gestire gli stili di formattazione direttamente all'interno di Azure Data Studio, in modo che sia possibile creare e modificare gli stili senza uscire dall'ambiente IDE. |
| Annuncio dell'estensione Machine Learning | Questa estensione consente di: <br/> &bull; &nbsp; Gestire pacchetti Python e R con Machine Learning Services per SQL Server e Azure Data Studio.<br/> &bull; &nbsp; Usare il modello ONNX per eseguire stime in SQL Edge di Azure.<br/> &bull; &nbsp; Visualizzare modelli ONNX in un database SQL Edge di Azure. <br/> &bull; &nbsp; Importare modelli ONNX da un file o da Azure Machine Learning nel database SQL Edge di Azure. <br/> &bull; &nbsp; Creare un notebook per eseguire esperimenti. |
| Nuove funzionalità dei notebook | &bull; &nbsp; Aggiunta della nuova procedura guidata dipendenze Python per semplificare l'installazione delle dipendenze Python <br/> &bull; &nbsp; Aggiunta del supporto della sottolineatura per la barra degli strumenti Markdown |
| Parametrizzazione per Always Encrypted | Consente di eseguire query che eseguono inserimenti, aggiornamenti o filtri in base a colonne di database crittografate.|
| Bug e problemi risolti | Per un elenco completo delle correzioni, vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22May+2020+Release%22+is%3Aclosed). |
| &nbsp; | &nbsp; |

## <a name="april-2020-hotfix"></a>Aprile 2020 (hotfix)

30 aprile 2020 &nbsp; / &nbsp; versione: 1.17.1

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Correzione del bug n. 10197 Non è possibile connettersi tramite MFA | [10197](https://github.com/microsoft/azuredatastudio/issues/10197)  |
| &nbsp; | &nbsp; |

## <a name="april-2020"></a>Aprile 2020

27 aprile 2020 &nbsp; / &nbsp; versione: 1.17.0

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Pagina iniziale migliorata | Aggiornamento dell'interfaccia utente nella pagina iniziale per semplificare la visualizzazione delle azioni comuni e mettere in evidenza le estensioni. |
| Nuove funzionalità dei notebook | &bull; &nbsp; Aggiunta della barra degli strumenti Markdown durante la modifica delle celle di testo per facilitare la scrittura con Markdown <br/> &bull; &nbsp; Riprogettazione del viewlet Book di Jupyter per trasformarlo in un viewlet di Notebooks in cui è possibile gestire insieme Book e notebook di Jupyter <br/>&bull; &nbsp; Aggiunta del supporto per salvare in modo permanente i grafici durante il salvataggio di un notebook <br/> &bull; &nbsp; Aggiunta del supporto per KQL Magic nei notebook Python|
| Dashboard migliorati | I dashboard in tutto Azure Data Studio sono stati aggiornati con i modelli di progettazione più recenti, inclusa una barra degli strumenti delle azioni. Questo vale anche per molte estensioni. |
| Aggiunta dell'integrazione di Cloud Shell nella visualizzazione Azure. | |
| Supporto di Always Encrypted e Always Encrypted con enclave sicuri. | |
| Bug e problemi risolti | Per un elenco completo delle correzioni, vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%22April+2020+Release%22). |
| &nbsp; | &nbsp; |
| Bug e problemi risolti | Per un elenco completo delle correzioni, vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%22April+2020+Release%22). |
| &nbsp; | &nbsp; |

## <a name="march-2020"></a>Marzo 2020

18 marzo 2020 &nbsp; / &nbsp; versione: 1.16.0 

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Aggiunta del supporto per la creazione di grafici nei notebook SQL | Quando si esegue una query SQL in una cella di codice, gli utenti possono ora creare e salvare grafici. |
| Aggiunta dell'esperienza di creazione di book Jupyter | Gli utenti possono ora creare book Jupyter personalizzati usando un notebook. |
| Aggiunta del supporto di Azure AD per l'estensione Postgres | |
| Correzione di molti bug di accessibilità | [Elenco dei bug di accessibilità](https://github.com/microsoft/azuredatastudio/issues?page=1&q=is%3Aissue+is%3Aclosed+milestone%3A%22S360+-+Accessibility%22+label%3AA11y_AzureDataStudio) |
| Merge di VS Code in 1.42 | Questa versione include gli aggiornamenti per VS Code dalle 3 versioni precedenti di VS Code. Per altre informazioni, [leggere le note sulla versione](https://code.visualstudio.com/updates/v1_42). |
| Bug e problemi risolti | Per un elenco completo delle correzioni, vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22March+2020%22+is%3Aclosed). |
| &nbsp; | &nbsp; |

## <a name="february-hotfix"></a>Febbraio (hotfix)

19 febbraio 2020 &nbsp; / &nbsp; versione: 1.15.1

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Correzione del bug 9149 Visualizzare le connessioni attive | [9149](https://github.com/microsoft/azuredatastudio/issues/9149)  |
| Correzione de bug 9061 La griglia di modifica dei dati non viene ridimensionata correttamente quando si visualizza o nasconde il riquadro SQL | [9061](https://github.com/microsoft/azuredatastudio/issues/9061)  |
| &nbsp; | &nbsp; |

## <a name="february-2020"></a>Febbraio 2020

13 febbraio 2020 &nbsp; / &nbsp; versione: 1.15.0

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Nuovo miglioramento dell'accesso ad Azure | È stata aggiunta un'esperienza di accesso di Azure migliorata, inclusa la rimozione del passaggio di copia/incolla del codice del dispositivo per offrire un'esperienza di connessione più lineare. |
| Supporto della ricerca nei notebook | Gli utenti possono ora usare CTRL+F all'interno di un notebook. Il supporto della ricerca nei notebook consente di eseguire ricerche riga per riga sia nel codice che nelle celle di testo. |
| Merge di VS Code da 1.38 a 1.42 | Questa versione include gli aggiornamenti per VS Code dalle 3 versioni precedenti di VS Code. Per altre informazioni, [leggere le note sulla versione](https://code.visualstudio.com/updates/v1_42). |
| Correzione per il problema di ["schermo bianco/vuoto"](https://github.com/microsoft/azuredatastudio/issues/8775) segnalato da molti utenti. | |
| Bug e problemi risolti | Per un elenco completo delle correzioni, vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%22February+2020%22). |
| &nbsp; | &nbsp; |

### <a name="known-issue"></a>Problema noto

- Gli utenti in macOS Catalina dovranno fare clic con il pulsante destro del mouse su Azure Data Studio e quindi scegliere Apri.

## <a name="december-2019-hotfix"></a>Dicembre 2019 (hotfix)

26 dicembre 2019 &nbsp; / &nbsp; versione: 1.14.1

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Correzione del bug #8747, espansione OE impossibile | [#8747](https://github.com/microsoft/azuredatastudio/issues/8747)  |
| &nbsp; | &nbsp; |

## <a name="december-2019"></a>Dicembre 2019

19 dicembre 2019 &nbsp; / &nbsp; versione: 1.14.0 

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Modificato l'elenco a discesa Collega a connessione nei notebook in modo che sia indicata solo la connessione attualmente attiva | [#8129](https://github.com/microsoft/azuredatastudio/issues/8129) |
| Aggiunta l'impostazione bigdatacluster.ignoreSslVerification per consentire l'esclusione degli errori di verifica TLS/SSL durante la connessione a un servizio BDC | [#8582](https://github.com/microsoft/azuredatastudio/pull/8582) |
| Consentita la modifica della versione del linguaggio predefinito per gli editor di query offline | [#8419](https://github.com/microsoft/azuredatastudio/pull/8419) |
| Stato di disponibilità generale per le funzionalità di cluster Big Data e SQL 2019 | [#8269](https://github.com/microsoft/azuredatastudio/issues/8269) |
| Bug e problemi risolti | Per un elenco completo delle correzioni, vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/milestone/44?closed=1). |
| &nbsp; | &nbsp; |


## <a name="november-2019-hotfix"></a>Novembre 2019 (hotfix)

15 novembre 2019 &nbsp; / &nbsp; versione: 1.13.1

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Correzione del bug #8210, risultati di copia/incolla non ordinati |  |
| &nbsp; | &nbsp; |

## <a name="november-2019"></a>Novembre 2019

4 novembre 2019 &nbsp; / &nbsp; versione: 1.13.0 

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Nuovo supporto per SQL Server 2019 | &bull; &nbsp; Distribuzione di un cluster Big Data di SQL Server 2019 con la distribuzione guidata del servizio di integrazione applicativa dei dati <br/>&bull; &nbsp; Gestione dell'integrità del cluster con il dashboard del controller <br/>&bull; &nbsp; Gestione degli elenchi di controllo di accesso HDFS usando la finestra di dialogo degli elenchi di controllo di accesso di sicurezza <br/> &bull; &nbsp; Aggiunta di montaggi usando la finestra di dialogo per la suddivisione in livelli HDFS <br/> &bull; &nbsp; Risoluzione dei problemi usando il Jupyter Book incorporato: SQL Server 2019 guide (Guida a SQL Server 2019) <br/> &bull; &nbsp; Estensione SQL vNext rinominata estensione di virtualizzazione dei dati <br/> &bull; &nbsp; Aggiunta del supporto per Teradata e Mongo nella procedura guidata Tabella esterna|
| Nuove funzionalità dei notebook | &bull; &nbsp; Annuncio dei notebook di PowerShell <br/> &bull; &nbsp; Annuncio delle celle di codice comprimibili <br/>&bull; &nbsp; Miglioramenti delle prestazioni nei notebook <br/> &bull; &nbsp; L'elenco completo dei miglioramenti è disponibile [qui](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22November+2019+Release%22+is%3Aclosed+label%3A%22Area+-+Notebooks%22) |
| Annuncio dei Jupyter Book  | I Jupyter Book sono costituiti da una raccolta di notebook e file Markdown organizzati in un sommario. |
| Nuova distribuzione guidata di SQL Server  | Include ora il supporto per la distribuzione dei prodotti seguenti: <br/> &bull; &nbsp; SQL Server 2019 in Windows <br/> &bull; &nbsp; SQL Server 2017 in Windows <br/> &bull; &nbsp; SQL Server 2019 in Docker <br/> &bull; &nbsp; SQL Server 2017 in Docker |
| Annuncio della disponibilità generale dell'estensione Confronto schema| &bull; &nbsp; Modalità SQLCMD <br/> &bull; &nbsp; Supporto per la localizzazione <br/> &bull; &nbsp; Correzioni di accessibilità <br/> &bull; &nbsp; Bug di sicurezza  |
| Annuncio della disponibilità generale dell'estensione SQL Server dacpac| <br/> &bull; &nbsp; Supporto per la localizzazione <br/> &bull; &nbsp; Correzioni di accessibilità <br/> &bull; &nbsp; Bug di sicurezza |
| Annuncio dell'estensione Visual Studio IntelliCode | Visual Studio IntelliCode supporta ora SQL, che consente di inviare suggerimenti più intelligenti sulle parole chiave riservate. |
| Bug e problemi risolti | Per un elenco completo delle correzioni, vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22November+2019+Release%22+is%3Aclosed). |
| &nbsp; | &nbsp; |

## <a name="october-2019-hotfix-2"></a>Ottobre 2019 (hotfix 2)

11 ottobre 2019 &nbsp; / &nbsp; versione: 1.12.2

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Disabilitare l'avvio automatico dell'EH in modalità controllo |  |
| &nbsp; | &nbsp; |

## <a name="october-2019-hotfix"></a>Ottobre 2019 (hotfix)

8 ottobre 2019 versione &nbsp; / &nbsp;: 1.12.1

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Correzione del problema relativo alle virgolette e alle barre rovesciate in Notebooks per eseguire correttamente escape. |  |
| &nbsp; | &nbsp; |

## <a name="october-2019"></a>Ottobre 2019

2 ottobre 2019 &nbsp; / &nbsp; versione: 1.12.0

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Rilascio dell'estensione per la cronologia delle query | L'estensione per la cronologia delle query SQL salva tutte le query precedenti eseguite in una sessione di Azure Data Studio e le elenca in ordine di esecuzione. Gli utenti possono aprire, eseguire o eliminare una query, sospendere la cronologia delle query o eliminare tutte le voci della cronologia. |
| Nuove modalità per copiare e incollare i risultati | Sono disponibili nuove modalità per copiare e incollare i risultati dalla griglia dei risultati. |
| Aggiornamento dell'estensione PowerShell |  |
| Bug e problemi risolti | Per un elenco completo delle correzioni, vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/milestone/42?closed=1). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problemi noti

- Notebook
  - [7080](https://github.com/microsoft/azuredatastudio/issues/7080) Caso raro in cui il notebook viene serializzato in modo non corretto

## <a name="september-2019"></a>Settembre 2019

10 settembre 2019 &nbsp; / &nbsp; versione: 1.11.0 

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Abilitare la modalità SQLCMD | L'editor di query supporta ora l'attivazione/disattivazione della modalità SQLCMD per scrivere e modificare query come script SQLCMD |
| Estensione della community: Query Editor Boost | Query Editor Boost è un'estensione open source incentrata sul miglioramento dell'editor di query di Azure Data Studio per gli utenti che scrivono spesso query. &bull; &nbsp; Salvare la query corrente come frammento di codice <br/>&bull; &nbsp; Cambiare database con CTRL+U <br/> &bull; &nbsp; Nuova query dal modello <br/> &bull; &nbsp; L'elenco completo dei miglioramenti è disponibile [qui](https://github.com/dzsquared/query-editor-boost) |
| Miglioramenti ai notebook | &bull; &nbsp; Miglioramenti delle prestazioni per il supporto dei file di notebook di dimensioni più elevate <br/> &bull; &nbsp; L'elenco completo dei miglioramenti è disponibile [qui](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22September+2019+Release%22+label%3A%22Area%3A+Notebooks%22+is%3Aclosed) |
| Visual Studio Code versione di agosto Merge 1.38 | I miglioramenti più recenti sono disponibili [qui](https://code.visualstudio.com/updates/v1_38). |
| Bug e problemi risolti | Per un elenco completo delle correzioni, vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/milestone/39?closed=1). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problemi noti

- Notebook
  - [7080](https://github.com/microsoft/azuredatastudio/issues/7080) Caso raro in cui il notebook viene serializzato in modo non corretto

## <a name="august-2019"></a>Agosto 2019

15 agosto 2019 &nbsp; / &nbsp; versione: 1.10.0 

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Rilascio dell'estensione SandDance 1.3.1 | &bull; &nbsp; Rilevamento intelligente dei grafici <br/>&bull; &nbsp; Visualizzazioni 3D <br/> &bull; &nbsp; Filtro dei dati |
| Miglioramenti ai notebook | &bull; &nbsp; Aggiunta di codice o di una cella di testo inline <br/>&bull; &nbsp; Aggiunta la possibilità di fare clic con il pulsante destro del mouse sulla griglia dei risultati SQL per salvare i risultati come CSV, JSON e così via <br/> &bull; &nbsp; Miglioramento delle prestazioni di caricamento del notebook per velocizzare il caricamento di JSON <br/> &bull; &nbsp; L'elenco completo dei miglioramenti è disponibile [qui](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+label%3A%22Area%3A+Notebooks%22+milestone%3A%22August+2019+Release%22+is%3Aclosed) |
| Supporto di SQL Server 2019 |  Questa versione include il supporto per altre funzionalità per cluster Big Data di SQL Server 2019, tra cui: <br/> &bull; &nbsp; Riduzione del tempo necessario per caricare le informazioni relative a tabelle e colonne nella pagina di mapping degli oggetti. <br/> &bull; &nbsp; Correzione di un bug relativo al caricamento delle credenziali con ambito database esistenti nella pagina Dettagli connessione. <br/> &bull; &nbsp; Aumento delle dimensioni predefinite del campione usato per l'analisi PROSE. | 
| L'estensione Dacpac ora supporta Azure AD | 
| Visual Studio Code versione di luglio Merge 1.37 | I miglioramenti più recenti sono disponibili [qui](https://code.visualstudio.com/updates/v1_37). |
| Bug e problemi risolti | Per un elenco completo delle correzioni, vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/milestone/39?closed=1). |
| &nbsp; | &nbsp; |

## <a name="july-2019"></a>Luglio 2019

11 luglio 2019 &nbsp; / &nbsp; versione: 1.9.0 

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Rilascio dell'estensione SentryOne Plan Explorer | Il partner Microsoft SentryOne fornirà l' [estensione SentryOne Plan Explorer per Azure Data Studio](https://www.sentryone.com/products/sentryone-plan-explorer-extension-azure-data-studio). <br> Si tratta di un'estensione gratuita, che fornisce diagrammi avanzati dei piani per le query eseguite in Azure Data Studio, con algoritmi di layout ottimizzati e codifica a colori intuitiva, che consentono di identificare rapidamente gli operatori più costosi che incidono sulle prestazioni delle query. Per altre informazioni sull'estensione, vedere il post di blog di SentryOne [qui.](https://sqlperformance.com/2019/07/sentryone/plan-explorer-extension-azure-data-studio) |
| Nuove funzionalità disponibili in Schema Compare | &bull; &nbsp; Supporto dei file di confronto schema (SCMP) <br/>&bull; &nbsp; Supporto dell'annullamento del confronto schema <br/>&bull; &nbsp; L'elenco completo delle modifiche è disponibile [qui](https://github.com/microsoft/azuredatastudio/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A%22July+2019+Release%22+label%3A%22Area%3A+Schema+Compare%22+is%3Aclosed+)|
| Miglioramenti ai notebook | &bull; &nbsp; Supporto di Plotly Python <br/>&bull; &nbsp; Apertura di notebook dal browser <br/> &bull; &nbsp; Finestra di dialogo di gestione pacchetti Python <br/> &bull; &nbsp; Miglioramenti delle prestazioni e del Markdown <br/> &bull; &nbsp; Aggiornamento delle scelte rapide da tastiera <br/>  &bull; &nbsp; Le correzioni di bug e le funzionalità secondarie sono disponibili [qui](https://github.com/microsoft/azuredatastudio/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A%22July+2019+Release%22+is%3Aclosed+label%3A%22Area%3A+Notebooks%22+) |
| Supporto di SQL Server 2019 |  Questa versione include il supporto per altre funzionalità per cluster Big Data di SQL Server 2019, tra cui: <br/> &bull; &nbsp; Tabella degli endpoint di servizio nel dashboard di gestione in cui sono elencati tutti i servizi principali del cluster. <br/> &bull; &nbsp; Notebook Stato cluster, che spiega come eseguire query e risolvere i problemi relativi allo stato del cluster in tutti i servizi e i pod.| 
| Disponibilità di Language Pack aggiornati| Nel marketplace di Gestione estensioni sono ora disponibili 10 Language Pack. È sufficiente cercare la lingua specifica usando il marketplace delle estensioni e installarla. Una volta installata la lingua selezionata, Azure Data Studio chiederà di riavviare con la nuova lingua. |
| Aggiornamento di SQL Server Profiler | L'estensione SQL Server Profiler è stata aggiornata per includere nuove funzionalità, tra cui: <br/> &bull; &nbsp; Filtro in base al nome del database <br/> &bull; &nbsp; Supporto per copia e incolla <br/> &bull; &nbsp; Salvataggio/caricamento dei filtri <br/>Un elenco completo dei miglioramenti per l'estensione SQL Server Profiler è disponibile [qui.](https://github.com/microsoft/azuredatastudio/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aclosed+milestone%3A%22July+2019+Release%22+label%3A%22Area%3A+SQL+Profiler%22+)  |
| Visual Studio Code versione di maggio Merge 1.35 | I miglioramenti più recenti sono disponibili [qui](https://code.visualstudio.com/updates/v1_35). |
| Bug e problemi risolti | Nelle versioni precedenti di Azure Data Studio, se veniva selezionato un database utente eseguendo la connessione tramite la finestra di dialogo di connessione, la voce Esplora oggetti risultante aveva come ambito solo tale database. A partire da questa versione, questo comportamento è stato modificato in modo che in Esplora oggetti vengano visualizzate anche le proprietà a livello di server. <br/> Per un elenco completo delle correzioni, vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/milestone/35?closed=1). |
| &nbsp; | &nbsp; |

## <a name="june-2019"></a>Giugno 2019

6 giugno 2019 &nbsp; / &nbsp; versione: 1.8.0

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Rilascio dell'estensione Server di gestione centrale (CMS) | Nei server di gestione centrale è archiviato un elenco di istanze di SQL Server organizzato in uno o più gruppi di server di gestione centrale. Gli utenti possono connettersi ai propri server CMS esistenti e gestire i propri server, ad esempio aggiungendoli e rimuovendoli. Per altre informazioni, vedere [qui](../relational-databases/administer-multiple-servers-using-central-management-servers.md) |
| Rilascio dell'estensione Database Administration Tool per Windows | Questa estensione avvia due delle esperienze più usate in SQL Server Management Studio da Azure Data Studio. Gli utenti possono fare clic con il pulsante destro del mouse su molti oggetti diversi, ad esempio database, tabelle, colonne, viste e altro ancora, e scegliere Proprietà per visualizzare la finestra di dialogo delle proprietà di SSMS per l'oggetto. Inoltre, gli utenti possono fare clic con il pulsante destro del mouse su un database e scegliere Genera script per avviare la nota procedura guidata di generazione script di SSMS. 
| Miglioramenti a Confronto schema | &bull; &nbsp; Aggiunta delle opzioni Escludi/Includi <br/>&bull; &nbsp; Genera script apre lo script dopo la generazione <br/>&bull; &nbsp; Rimozione delle barre di scorrimento doppie  <br/>&bull; &nbsp; Miglioramenti alla formattazione e al layout <br/>&bull; &nbsp; L'elenco completo delle modifiche è disponibile [qui](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22June+2019+Release%22+label%3A%22Area%3A+Schema+Compare%22+is%3Aclosed)|
| Sezione messaggi spostata in una scheda personalizzata | Quando gli utenti eseguivano query SQL, i risultati e i messaggi si trovavano in pannelli in pila. Ora si trovano in schede separate in un solo pannello, come in SSMS. |
| Miglioramenti ai notebook SQL | &bull; &nbsp; Gli utenti possono ora scegliere di usare le proprie installazioni di Python 3 o Anaconda nei notebook <br/>&bull; &nbsp; Più correzioni per stabilità e aspetto/finitura <br/> &bull; &nbsp; L'elenco completo dei miglioramenti è disponibile [qui](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22June+2019+Release%22+is%3Aclosed+label%3A%22Area%3A+Notebooks%22)|
| Visual Studio Code versione di aprile Merge 1.34 | I miglioramenti più recenti sono disponibili [qui](https://code.visualstudio.com/updates/v1_34) |
| Bug e problemi risolti. | Vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/milestone/32?closed=1). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problemi noti

- Estensione Database Administration Tool per Windows
    - Non è possibile avviare le proprietà da un nodo server disconnesso
    - Non è possibile avviare le proprietà per i server di Azure
    - Non tutti gli oggetti hanno finestre di dialogo delle proprietà
    - L'avvio delle finestre di dialogo richiede molto tempo
    - Errori di avvio dei server con alcuni tipi di connessioni, ad esempio Azure AD
- Notebook
    - [5838](https://github.com/microsoft/azuredatastudio/issues/5838) Consentire agli utenti di usare la versione di sistema di Python per i notebook
- Confronto schema
    - [5804](https://github.com/microsoft/azuredatastudio/issues/5804) Le attività di confronto schemi mostrano un menu di scelta rapida Annulla predefinito che non esegue alcuna operazione

## <a name="may-2019"></a>Maggio 2019

8 maggio 2019 &nbsp; / &nbsp; versione: 1.7.0 

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Rilascio dell'estensione Schema Compare | Confronto schema è una nota funzionalità di SQL Server Data Tools (SSDT) il cui caso d'uso principale è quello di confrontare e visualizzare le differenze tra database e file con estensione dacpac e di eseguire azioni per renderli uguali. |
| Spostamento della visualizzazione attività nella finestra di output | Gli utenti possono ora vedere lo stato delle attività con esecuzione prolungata come backup, ripristino e confronto schema nella visualizzazione attività all'interno della finestra di output
| Aggiunta della pagina di benvenuto | &bull; &nbsp; Collegamenti ad azioni comuni come Nuova query, Nuovo file, Nuovo notebook <br/>&bull; &nbsp; Collegamenti alla documentazione e a GitHub |
| Miglioramenti ai notebook SQL | &bull; &nbsp; Miglioramenti al rendering del Markdown, incluso un supporto migliore per note e tabelle <br/>&bull; &nbsp; Miglioramenti all'usabilità della barra degli strumenti <br/>&bull; &nbsp; I collegamenti al Markdown per i notebook attendibili non richiedono più di premere CMD/CTRL+clic e possono essere selezionati direttamente <br/>&bull; &nbsp; Miglioramenti nella pulizia dei processi Jupyter dopo la chiusura dei notebook e riduzione degli errori nei casi di avvio simultaneo di più notebook <br/>&bull; &nbsp; Miglioramenti delle connessioni a notebook SQL per garantire che non si verifichino errori durante l'esecuzione di 2 notebook sullo stesso database <br/>&bull; &nbsp; Miglioramenti allo scorrimento automatico del notebook verso la cella attualmente in esecuzione quando si fa clic sul pulsante Run Cells (Esegui celle) della barra degli strumenti <br/>&bull; &nbsp; Miglioramenti generali della stabilità e delle prestazioni |
| Bug e problemi risolti. | Vedere i [bug e i problemi su GitHub](https://github.com/microsoft/azuredatastudio/milestone/31?closed=1). |
| &nbsp; | &nbsp; |

## <a name="april-2019"></a>Aprile 2019

18 aprile 2019 &nbsp; / &nbsp; versione: 1.6.0 

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Scheda **Server** rinominata in **Connessioni** | |
| Spostamento di Azure Resource Explorer come viewlet di Azure in Connessioni | Gli utenti possono ora vedere le istanze SQL di Azure tramite il viewlet Azure nella visualizzazione Connessioni ed espandere per vedere gli oggetti in ogni server o database.|
| Miglioramenti ai notebook SQL | &bull; &nbsp; Aggiunta di un pulsante alla barra degli strumenti per cancellare l'output per tutte le celle <br/>&bull; &nbsp; Aggiunta di un pulsante alla barra degli strumenti per eseguire tutte le celle <br/>&bull; &nbsp; Correzione di nome della connessione anziché nome del server (se impostato) nell'elenco a discesa Collega a <br/>&bull; &nbsp; Correzione del mancato rendering delle immagini nel markdown quando si usano percorsi relativi per le immagini <br/>&bull; &nbsp; Miglioramento delle funzionalità nelle griglie dei notebook mediante l'aggiunta del doppio clic per il ridimensionamento automatico delle colonne e il supporto della rotella del mouse <br/>&bull; &nbsp; Miglioramenti alla gestione degli errori e alla resilienza dell'installazione di Python durante l'installazione di Python con i notebook <br/>&bull; &nbsp; Miglioramenti alla funzionalità "Seleziona tutto" quando si selezionano le celle del notebook <br/>&bull; &nbsp; Miglioramenti alle connessioni a notebook per impedire che la chiusura di un notebook incida su e una connessione di Esplora oggetti <br/>&bull; &nbsp; Miglioramenti all'esperienza notebook per visualizzare un messaggio all'utente quando il notebook è disconnesso ed è necessaria una connessione per l'esecuzione delle celle<br/>&bull; &nbsp; Miglioramento del supporto per i notebook non salvati per riattivarli in ADS al riavvio di ADS |
| Bug e problemi risolti. | Vedere i [bug e i problemi su GitHub](https://github.com/Microsoft/azuredatastudio/milestone/26?closed=1). |
| &nbsp; | &nbsp; |

## <a name="march-2019-hotfix"></a>Marzo 2019 (Hotfix)

22 marzo 2019 &nbsp; / &nbsp; versione: 1.5.2 &nbsp; / &nbsp; Versione hotfix

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Sono stati corretti alcuni problemi individuati nella versione 1.5.1. | Vedere la pagina sulla [versione hotfix di marzo su GitHub](https://github.com/Microsoft/azuredatastudio/milestone/28).<br/> <br/>&bull; &nbsp; Correzione del problema per cui l'utente non riusciva a chiudere il notebook aperto dall'attività di apertura notebook nel dashboard <br/>&bull; &nbsp; Correzione del problema per cui JSON del notebook conteneva un simbolo } extra dopo il salvataggio <br/>&bull; &nbsp; Correzione del problema per cui le griglie dei notebook non rispondevano alle modifiche del tema <br/>&bull; &nbsp; Correzione del problema per cui nell'intestazione della scheda veniva visualizzato il percorso completo del notebook. Ora viene visualizzato solo il nome file. |
| &nbsp; | &nbsp; |

## <a name="march-2019"></a>Marzo 2019

18 marzo 2019 &nbsp; / &nbsp; versione: 1.5.1

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Aggiunta dell'[estensione PostgreSQL per Azure Data Studio](./extensions/postgres-extension.md) | Funzionalità supportate: <br/>&bull; &nbsp; Finestra di dialogo di connessione <br/>&bull; &nbsp; Esplora oggetti <br/>&bull; &nbsp; Editor query <br/>&bull; &nbsp; Creazione di grafici <br/>&bull; &nbsp; Dashboard <br/>&bull; &nbsp; Frammenti di codice <br/>&bull; &nbsp; Modifica dei dati <br/>&bull; &nbsp; Notebook |
| Aggiunta di notebook SQL | Aggiunta del supporto del kernel SQL al visualizzatore notebook integrato: <br/>&bull; &nbsp; Supporta T-SQL <br/>&bull; &nbsp; Supporta PGSQL |
| Aggiunta dell'estensione PowerShell | Offre l'esperienza dell'[estensione PowerShell](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell) da VS Code.  |
| Aggiunta dell'estensione SQL Server dacpac  | Sposta la procedura guidata di applicazione livello dati da SQL Server Import in una nuova estensione.  |
| Aggiunta dell'estensione della community QueryPlan.show | Aggiunge il supporto dell'integrazione per visualizzare i piani di query  |
| Aggiornamento dell'estensione SQL Server 2019 (anteprima) | &bull; &nbsp; Il supporto di Jupyter Notebook, in particolare il supporto per i kernel Python3 e Spark, è stato spostato nello strumento principale Azure Data Studio. <br/>&bull; &nbsp; Correzioni di bug alla procedura guidata dati esterni |
| Bug e problemi risolti. | Vedere i [bug e i problemi su GitHub](https://github.com/Microsoft/azuredatastudio/milestone/25?closed=1). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problemi noti
- [4427](https://github.com/Microsoft/azuredatastudio/issues/4427): facendo clic su Esegui nella cella prima che il kernel sia pronto per Spark, si verifica un errore irreversibile **Soluzione alternativa:** attendere il caricamento dei kernel prima di eseguire celle
- [4493](https://github.com/Microsoft/azuredatastudio/issues/4493): se avviato da SSMS utilizzando l'autenticazione SQL, ADS chiede all'utente di inserire la password **Soluzione alternativa:** usare l'autenticazione di Windows per il momento. 
- [4494](https://github.com/Microsoft/azuredatastudio/issues/4494): impossibile installare la funzionalità dei notebook SQL <br/>
**Soluzione alternativa:** seguire la indicata [qui](https://github.com/Microsoft/azuredatastudio/issues/4494#issuecomment-473043832). 
- [4503](https://github.com/Microsoft/azuredatastudio/issues/4503): non è possibile aprire Azure Data Studio direttamente dalla cartella dei download (Mac) <br />
**Soluzione alternativa:** riavviare il computer dopo aver decompresso l'app. Verrà analizzato. 
- [4539](https://github.com/Microsoft/azuredatastudio/issues/4539):  salvando il notebook con nome si perde il contesto di connessione <br />
**Soluzione alternativa:** verrà risolto nella prossima versione. 
- [4458](https://github.com/Microsoft/azuredatastudio/issues/4458): Dacpac Extract causa l'arresto anomalo di SqlToolsService se viene usata una versione non valida <br/>
**Soluzione alternativa:** riavviare Azure Data Studio e verificare che venga usata la versione corretta.
- Le icone di nuovo notebook e apertura notebook vanno perdute <br/>
**Soluzione alternativa:** Il tipo di connessione legacy è deprecato. Si consiglia di connettersi all'endpoint SQL Server per ottenere tutte le azioni (nuovo notebook, processo Spark) come previsto. 

## <a name="february-2019"></a>Febbraio 2019

13 febbraio 2019 &nbsp; / &nbsp; versione: 1.4.5

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Aggiunta del pacchetto di estensioni **Admin Pack per SQL Server**. | Semplifica l'installazione delle estensioni correlate all'amministrazione di SQL Server. ad esempio:<br/>&bull; &nbsp; [SQL Server Agent](./extensions/sql-server-agent-extension.md)<br/>&bull; &nbsp; [SQL Server Profiler](./extensions/sql-server-profiler-extension.md)<br/>&bull; &nbsp; [SQL Server Import](./extensions/sql-server-import-extension.md) |
| Aggiunta del supporto per l'applicazione di filtri agli eventi estesi nell'estensione Profiler. | &nbsp; |
| Aggiunta della funzionalità Salva come XML per salvare i risultati T-SQL in formato XML. | &nbsp; |
| Aggiunta di miglioramenti alla procedura guidata di applicazione livello dati. | &bull; &nbsp; Aggiunta del pulsante Genera script<br/>&bull; &nbsp; Aggiunta di una visualizzazione per presentare avvisi della possibile perdita di dati durante la distribuzione. |
| Aggiornamenti all'estensione SQL Server 2019 (anteprima). | Vedere [Estensione di virtualizzazione dei dati](./extensions/data-virtualization-extension.md). |
| Streaming dei risultati abilitato per impostazione predefinita per le query con esecuzione prolungata. | &nbsp; |
| Bug e problemi risolti. | Vedere i [bug e i problemi su GitHub](https://github.com/Microsoft/azuredatastudio/milestone/23?closed=1). |
| &nbsp; | &nbsp; |

## <a name="january-2019-hotfix"></a>Gennaio 2019 (Hotfix)

16 gennaio 2019 &nbsp; / &nbsp; versione: 1.3.9 &nbsp; / &nbsp; Versione hotfix

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Sono stati corretti alcuni problemi individuati nella versione 1.3.8. | Vedere la pagina sulla [versione hotfix di gennaio su GitHub](https://github.com/Microsoft/azuredatastudio/milestone/24?closed=1).<br/><br/>Per informazioni dettagliate, vedere:<br/>&bull; &nbsp; [Il log delle modifiche su GitHub](https://github.com/Microsoft/azuredatastudio/blob/main/CHANGELOG.md).<br/>&bull; &nbsp; [La pagina delle versioni su GitHub](https://github.com/Microsoft/azuredatastudio/releases). |
| &nbsp; | &nbsp; |

## <a name="january-2019"></a>Gennaio 2019

9 gennaio 2019 &nbsp; / &nbsp; versione: 1.3.8

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Aggiunta di un nuovo programma di installazione utente per Windows. | A differenza del programma di installazione di sistema esistente, il nuovo programma di installazione utente non richiede privilegi di amministratore. Inoltre, semplifica l'esperienza di aggiornamento per i non amministratori. |
| Aggiunta del supporto dell'autenticazione di Azure Active Directory. | &nbsp; |
| Annuncio di Idera SQL DM Performance Insights (anteprima). | &nbsp; |
| Supporto della procedura guidata di applicazione livello dati nell'estensione SQL Server Import. | &nbsp; |
| Aggiornamento dell'estensione SQL Server 2019 (anteprima). | Vedere [Estensione di virtualizzazione dei dati](./extensions/data-virtualization-extension.md). |
| Miglioramenti a SQL Server Profiler. | &nbsp; |
| Streaming dei risultati per query di grandi dimensioni (anteprima). | &nbsp; |
| Estensioni della community: sp_executesql to SQL e New Database. | &nbsp; |
| Bug e problemi risolti. | Vedere i [bug e i problemi su GitHub](https://github.com/Microsoft/azuredatastudio/milestone/19?closed=1). |
| &nbsp; | &nbsp; |

## <a name="november-2018"></a>Novembre 2018

6 novembre 2018 &nbsp; / &nbsp; versione: 1.2.4

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Aggiornamento dell'estensione SQL Server 2019 (anteprima). | Vedere [Estensione di virtualizzazione dei dati](./extensions/data-virtualization-extension.md). |
| Introduzione dell'estensione Paste the Plan. | &nbsp; |
| Introduzione dell'estensione High Color queries, incluso il tema dell'editor SSMS. | &nbsp; |
| Correzioni nelle estensioni SQL Server Agent, Profiler e Import. | &nbsp; |
| Correzione del problema relativo all'opzione KeepAlive nei socket .NET Core che causa l'eliminazione delle connessioni inattive in macOS. | &nbsp; |
| Aggiornamento del servizio SQL Tools a .NET Core 2.2 Preview 3 (per l'eventuale supporto di Azure AD). | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-november-2018"></a>Correzioni di bug, novembre 2018

- Correzione del [problema 2933](https://github.com/Microsoft/azuredatastudio/issues/2933): perdita di connessione al database SQL di Azure
- Correzione del [problema 2914](https://github.com/Microsoft/azuredatastudio/issues/2914): eccezione "Argomento non valido" espandendo un nodo di database in Esplora oggetti
- Correzione del [problema 2935](https://github.com/Microsoft/azuredatastudio/pull/2935): visualizzazione corretta dei messaggi su più righe nei risultati di query
- Correzione del [problema 2906](https://github.com/Microsoft/azuredatastudio/pull/2906): errore nel nome del documento aperto in Modifica dati quando il nome della tabella contiene caratteri speciali
- Correzione del [problema 2929](https://github.com/Microsoft/azuredatastudio/issues/2929): il log delle modifiche predefinito delle estensioni indica di controllare le modifiche nelle note di rilascio di VS Code
- Correzione del [problema 2719](https://github.com/Microsoft/azuredatastudio/issues/2719): icone doppie/triple del tema a contrasto elevato
- Correzione del [problema 3047](https://github.com/Microsoft/azuredatastudio/pull/3047): aggiungere un'interfaccia della riga di comando per la connessione a SQL Server
- Correzione del [problema 3031](https://github.com/Microsoft/azuredatastudio/pull/3031): aggiungere il supporto di temi per il piano di query

## <a name="october-2018"></a>Ottobre 2018

29 ottobre 2018 &nbsp; / &nbsp; versione: 1.1.4

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Introduzione di Azure Resource Explorer per esplorare i database SQL di Azure. | &nbsp; |
| Miglioramento dell'affidabilità delle connessioni dell'editor di query e di Esplora oggetti. | &nbsp; |
| Miglioramenti alle estensioni SQL Agent. | &nbsp; |
| Aggiornamento dell'estensione SQL Server 2019 (anteprima). | Vedere [Estensione di virtualizzazione dei dati](./extensions/data-virtualization-extension.md). |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-october-2018"></a>Correzioni di bug, ottobre 2018

- Correzione del [problema 2717](https://github.com/Microsoft/azuredatastudio/issues/2717): perdita di formattazione facendo clic sui risultati di una colonna XML
- Correzione del [problema 2993](https://github.com/Microsoft/azuredatastudio/issues/2993): larghezza incompleta della finestra dei risultati
- Correzione del [problema 2999](https://github.com/Microsoft/azuredatastudio/issues/2999): impossibile caricare il file System.Diagnostics.Tracing in Mac durante la connessione al database
- Correzione del [problema 2851](https://github.com/Microsoft/azuredatastudio/issues/2851): il rendering del grafico TimeSeries non viene eseguito correttamente
- Correzione del [problema 2996](https://github.com/Microsoft/azuredatastudio/issues/2996): perdita della tabella temporanea a causa del cambio improvviso di sessione

Per informazioni dettagliate, vedere il [log delle modifiche](https://github.com/Microsoft/azuredatastudio/blob/main/CHANGELOG.md) e la pagina delle [versioni](https://github.com/Microsoft/azuredatastudio/releases).

## <a name="september-2018-ga-release"></a>Settembre 2018 (versione disponibile a livello generale)

24 settembre 2018 &nbsp; / &nbsp; versione: 1.0 &nbsp; / &nbsp; Versione disponibile a livello generale

Versione disponibile a livello generale di Azure Data Studio (in precedenza SQL Operations Studio).

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Miglioramento delle prestazioni della griglia dei risultati della query e miglioramenti dell'esperienza utente per un numero elevato di set di risultati. | &nbsp; |
| Aggiornamento del codice sorgente di Visual Studio Code da 1.23 a 1.26.1 con editor delle impostazioni migliorato (anteprima) e layout griglia. | &nbsp; |
| Miglioramenti dell'accessibilità per utilità per la lettura dello schermo, navigazione tramite tastiera e contrasto elevato. | &nbsp; |
| Aggiunta dell'opzione `Connection name` per fornire un nome visualizzato alternativo nel viewlet Server. | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="announcing-the-sql-server-2019-preview-extension"></a>Annuncio dell'estensione SQL Server 2019 (anteprima)

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Supporto delle funzionalità SQL Server 2019 (anteprima), incluso il supporto dei [cluster Big Data](../big-data-cluster/big-data-cluster-overview.md). | Capacità di connessione al gateway HDFS/Spark fornita con SQL Server 2019 (anteprima).<br/><br/>Possibilità di esplorare Hadoop Distributed File System (HDFS), caricare file, salvare file e avviare azioni utili come l'analisi nei notebook per i file CSV.<br/><br/>Invio di processi Spark dal dashboard o facendo clic con il pulsante destro del mouse su una connessione HDFS/Spark in Esplora oggetti. |
| Notebook di Azure Data Studio. | Possibilità di creare o aprire notebook usando un visualizzatore notebook integrato. In questa versione il visualizzatore notebook supporta solo la connessione ai kernel locali e al cluster Big Data di SQL Server 2019.<br/><br/>Uso delle librerie dell'acceleratore di codice PROSE nei notebook per apprendere il formato file e i tipi di dati per la preparazione rapida dei dati. |
| Azure Resource Explorer. | La visualizzazione Azure Resource Explorer consente di esplorare gli endpoint correlati ai dati per gli account Azure e di creare connessioni a tali endpoint in Esplora oggetti. In questa versione, il database SQL di Azure è supportato. |
| Procedura guidata di creazione tabella esterna in SQL Server PolyBase. | Creazione di una tabella esterna e delle strutture di metadati di supporto con una procedura guidata di facile utilizzo. In questa versione sono supportati i server remoti SQL Server e Oracle. |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-september-2018"></a>Correzioni di bug, settembre 2018

- Correzione del [problema 2647](https://github.com/Microsoft/azuredatastudio/issues/143): i grafici hanno compiuto un passo indietro significativo.
- Correzione del [problema 2648](https://github.com/Microsoft/azuredatastudio/issues/143): SELECT che restituisce un JSON trasforma in collegamenti ipertestuali i record dell'intera colonna.

Per informazioni dettagliate, vedere il [log delle modifiche](https://github.com/Microsoft/azuredatastudio/blob/main/CHANGELOG.md) e la pagina delle [versioni](https://github.com/Microsoft/azuredatastudio/releases).

## <a name="august-2018"></a>Agosto 2018

30 agosto 2018 &nbsp; / &nbsp; versione: 0.32.8 &nbsp; / &nbsp; Anteprima pubblica

L'*anteprima pubblica di agosto* è incentrata sulla correzione dei bug, la stabilizzazione del prodotto e il superamento delle lacune negli scenari esistenti.

_La versione 0.32.8 contiene correzioni per un paio di regressioni individuata nella 0.32.7 ([1971](https://github.com/Microsoft/azuredatastudio/issues/1971), [2372](https://github.com/Microsoft/azuredatastudio/issues/2372))_

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Annuncio dell'estensione SQL Server Import. | &nbsp; |
| Gestione sessioni di SQL Server Profiler. | &nbsp; |
| Supporto del modelli di sessione di SQL Server Profiler. | &nbsp; |
| Miglioramenti a SQL Server Agent. | &nbsp; |
| Nuova estensione della community: First Responder Kit. | &nbsp; |
| Miglioramenti Quality of Life: Stringhe di connessione | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-august-2018"></a>Correzioni di bug, agosto 2018

- Analisi SQL in una finestra dell'editor di query tramite il comando `Parse Syntax`.
- Correzione del [problema 143](https://github.com/Microsoft/azuredatastudio/issues/143): con il doppio clic non viene selezionato il simbolo @ nel nome variabile.
- Correzione del [problema 387](https://github.com/Microsoft/azuredatastudio/issues/387): l'icona delle schede dei database SQL è rossa.
- Correzione del [problema 825](https://github.com/Microsoft/azuredatastudio/issues/825): Richiesta: connessione automatica al server corrente dopo il comando Script come... 
- Correzione del [problema 1278](https://github.com/Microsoft/azuredatastudio/issues/1278): sqlops.desktop [Desktop Entry] - valore ridondante per Name e Comment.
- Correzione del [problema 1285](https://github.com/Microsoft/azuredatastudio/issues/1285): l'aggiornamento causa la rimozione o la sostituzione dell'icona dell'applicazione in Windows.
- Correzione del [problema 1317](https://github.com/Microsoft/azuredatastudio/issues/1317): correggere il separatore decimale.
- Correzione del [problema 1474](https://github.com/Microsoft/azuredatastudio/issues/1474): l'annullamento di Cambia connessione disconnette la connessione corrente.
- Correzione del [problema 1497](https://github.com/Microsoft/azuredatastudio/issues/1497): le opzioni di visualizzazione come grafico sono troncate in basso.
- Correzione del [problema 1524](https://github.com/Microsoft/azuredatastudio/issues/1524): shell/dashboard: le icone del viewlet principale sono trascinabili e possono causare l'arresto anomalo dell'app.
- Correzione del [problema 1578](https://github.com/Microsoft/azuredatastudio/issues/1578): non è possibile espandere o comprimere la cartella del visualizzatore file remoto facendo clic sul nome.
- Correzione del [problema 1620](https://github.com/Microsoft/azuredatastudio/issues/1620): Suggerimento di funzionalità: ottenere la stringa di connessione per la connessione esistente.
- Correzione del [problema 1624](https://github.com/Microsoft/azuredatastudio/issues/1624): la casella di selezione non cambia colore se disabilitata.
- Correzione del [problema 1728](https://github.com/Microsoft/azuredatastudio/issues/1728): il salvataggio come JSON/EXCEL/CSV non funziona.
- Correzione del [problema 1744](https://github.com/Microsoft/azuredatastudio/issues/1744): il riquadro dei risultati perde le posizioni di scorrimento quando si passa da una scheda all'altra.
- Correzione del [problema 1748](https://github.com/Microsoft/azuredatastudio/issues/1748): messaggio di errore durante il salvataggio di un file di Excel la seconda volta (e le successive).
- Correzione del [problema 1782](https://github.com/Microsoft/azuredatastudio/issues/1782): modifica dei dati: la cella non torna al valore originale premendo il tasto ESC.
- Correzione del [problema 1836](https://github.com/Microsoft/azuredatastudio/issues/1836): file con estensione .sql non associati a SQL Operations Studio.
- Correzione del [problema 1850](https://github.com/Microsoft/azuredatastudio/issues/1850): la digitazione di N'' viene completata automaticamente in N'''.
- Correzione del [problema 1985](https://github.com/Microsoft/azuredatastudio/issues/1985): la copia dalla griglia dei risultati della query è spostata di una colonna.
- Correzione del [problema 1998](https://github.com/Microsoft/azuredatastudio/pull/1998): aggiunta la versione di VS Code alla finestra di dialogo Informazioni su.
- Correzione del [problema 2042](https://github.com/Microsoft/azuredatastudio/pull/2042): Agente: abilitato il pulsante per l'importazione di query da file SQL.
- Correzione del [problema 2091](https://github.com/Microsoft/azuredatastudio/issues/2091): non è possibile usare la scelta rapida da tastiera CTRL+C per copiare dal riquadro risultati.
- Correzione del [problema 2099](https://github.com/Microsoft/azuredatastudio/pull/2099): sono state aggiunte altre opzioni di salvataggio come CSV.
- Correzione del [problema 2107](https://github.com/Microsoft/azuredatastudio/issues/2107): aggiornamento dell'icona documento per i documenti del dashboard e del Profiler.
- Correzione del [problema 2129](https://github.com/Microsoft/azuredatastudio/pull/2129): salvataggio della posizione di scorrimento in modifica dati quando si cambia scheda.
- Correzione del [problema 2152](https://github.com/Microsoft/azuredatastudio/issues/2152): l'indicatore riga della griglia parte da zero.

### <a name="known-issues-august-2018"></a>Problemi noti, agosto 2018

- [Problema 2371:](https://github.com/Microsoft/azuredatastudio/issues/2371) salvando in formato Excel viene salvata solo la prima riga di dati
- [Problema 2150](https://github.com/Microsoft/azuredatastudio/issues/2150): in Ubuntu 16.04 non è possibile connettersi a SQL in un contenitore

## <a name="july-2018"></a>Luglio 2018

19 luglio 2018 &nbsp; / &nbsp; versione: 0.31.4 &nbsp; / &nbsp; Anteprima pubblica

L'*Anteprima pubblica di luglio* è incentrata sugli elementi seguenti:

- Versione iniziale degli scenari di configurazione di SQL Server Agent.
- Miglioramenti ai modelli di sessione e di vista di SQL Server Profiler.
- Correzioni di bug continuative per i problemi segnalati dai clienti su GitHub.

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Miglioramenti all'[estensione SQL Server Agent per SQL Operations Studio](./extensions/sql-server-agent-extension.md). | Aggiunta della visualizzazione di avvisi, operatori e proxy e di icone nel riquadro sinistro.<br/><br/>Aggiunta delle finestre di dialogo Nuovo processo, Nuovo passaggio di processo, Nuovo avviso e Nuovo operatore.<br/><br/>Aggiunta di Elimina processo, Elimina avviso e Elimina operatore (clic con il pulsante destro del mouse).<br/><br/>Aggiunta della possibilità di vedere le esecuzioni precedenti.<br/><br/>Aggiunta di filtri per ogni nome di colonna. |
| Miglioramenti all'[estensione SQL Server Profiler per SQL Operations Studio](./extensions/sql-server-profiler-extension.md). | Aggiunta di 5 modelli predefiniti per visualizzare gli eventi estesi.<br/><br/>Aggiunta del nome della connessione al server o al database.<br/><br/>Aggiunta del supporto di istanze di database SQL di Azure.<br/><br/>Aggiunta del suggerimento per uscire da Profiler alla chiusura della scheda è chiusa quando Profiler è ancora in esecuzione. |
| Rilascio dell'estensione Combine Scripts. | &nbsp; |
| Aggiunta di punti di estendibilità a procedure guidate e finestre di dialogo per gli autori di estensioni. | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-july-2018"></a>Correzioni di bug, luglio 2018

- Correzione del [problema 728](https://github.com/Microsoft/azuredatastudio/issues/728): nessuna risposta al comando Aggiungi connessione in macOS
- Correzione del [problema 1612](https://github.com/Microsoft/azuredatastudio/issues/1612): La visualizzazione del testo della griglia dei risultati è resa confusa da caratteri internazionali
- Correzione del [problema 1693](https://github.com/Microsoft/azuredatastudio/issues/1693): finestra di dialogo Backup: l'interfaccia utente del visualizzatore file è interrotta
- Correzione del [problema 1713](https://github.com/Microsoft/azuredatastudio/issues/1713): numero di righe interessate
- Correzione del [problema 1718](https://github.com/Microsoft/azuredatastudio/issues/1718): non è possibile connettersi a un'origine dati
- Correzione del [problema 1719](https://github.com/Microsoft/azuredatastudio/issues/1719): errore di tipo durante la connessione a server
- Correzione del [problema 1724](https://github.com/Microsoft/azuredatastudio/issues/1724): le finestre di dialogo dell'estensione hanno smesso di funzionare
- Correzione del [problema 1749](https://github.com/Microsoft/azuredatastudio/issues/1749): BUG: i dati HTML in una colonna vengono interpretati
- Correzione del [problema 1789](https://github.com/Microsoft/azuredatastudio/issues/1789): estendibilità: se si aggiunge un provider di connessione, disinstallando l'estensione non viene rimosso dall'elenco
- Correzione del [problema 1791](https://github.com/Microsoft/azuredatastudio/issues/1791): Estensioni Sqlops: queryeditor.connect() si connette al database di destinazione, ma l'interfaccia utente non mostra che l'editor è connesso
- Correzione del [problema 1799](https://github.com/Microsoft/azuredatastudio/issues/1799): il grafico dei primi 10 database di dimensioni maggiori non funziona nelle istanze con distinzione tra maiuscole e minuscole
- Correzione del [problema 1814](https://github.com/Microsoft/azuredatastudio/issues/1814): un errore di digitazione in sqlops.d.ts causa la definizione implicita del tipo 'any'
- Correzione del [problema 1817](https://github.com/Microsoft/azuredatastudio/issues/1817): errore di ortografia
- Correzione del [problema 1830](https://github.com/Microsoft/azuredatastudio/issues/1830): impostando iconPath in ButtonComponent dopo la chiamata di component(), l'icona non viene modificata
- Correzione del [problema 1843](https://github.com/Microsoft/azuredatastudio/issues/1843): organizzazione migliore delle tabelle

## <a name="june-2018"></a>Giugno 2018

20 giugno 2018 &nbsp; / &nbsp; versione: 0.30.6 &nbsp; / &nbsp; Anteprima pubblica

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Versione iniziale dell'estensione **SQL Server Profiler per SQL Operations Studio _(anteprima)_** | &nbsp; |
| La nuova estensione **SQL Data Warehouse** include widget del dashboard avanzati e personalizzabili che consentono di ottenere informazioni dettagliate sul data warehouse. | Questo abilita scenari chiave per la gestione e la messa a punto dei data warehouse, per assicurarsi che siano ottimizzati per prestazioni coerenti. |
| Supporto di **filtri e ordinamento durante la modifica dei dati**. | &nbsp; |
| Miglioramenti all'estensione **SQL Server Agent per SQL Operations Studio _(anteprima)_** per le visualizzazioni Processi e Cronologia processo. | &nbsp; |
| Miglioramento delle API di estendibilità del **framework del generatore di interfaccia utente per procedure guidate e finestre di dialogo**. | &nbsp; |
| Aggiornamento del codice sorgente della piattaforma VS Code. | Sono state integrate le versioni seguenti:<br/>&bull; &nbsp; [Marzo 2018 (1.22)](https://code.visualstudio.com/updates/v1_22)<br/>&bull; &nbsp; [Aprile 2018 (1.23)](https://code.visualstudio.com/updates/v1_23) |
| &nbsp; | &nbsp; |

### <a name="github-issues-fixes-june-2018"></a>Correzioni dei problemi GitHub, giugno 2018

- Richiesta di funzionalità ([problema 1204](https://github.com/Microsoft/azuredatastudio/issues/1204)): fare in modo che nella griglia dei risultati la larghezza delle colonne si adatti automaticamente ai dati e ricordare le modifiche manuali se la stessa query viene eseguita di nuovo.
- Correzione del [problema 1398](https://github.com/Microsoft/azuredatastudio/issues/1398): quando l'account collegato è vuoto devono comparire il messaggio di aggiunta e il pulsante Aggiungi account.
- Correzione del [problema 1399](https://github.com/Microsoft/azuredatastudio/issues/1399): la scheda account dell'account collegato è interrotta quando la visualizzazione è compressa.
- Correzione del [problema 1374](https://github.com/Microsoft/azuredatastudio/issues/1374): il servizio SQL Tools si arresta in modo anomalo quando si apre il file SQL dal disco.
- Correzione del [problema 1372](https://github.com/Microsoft/azuredatastudio/issues/1372): mancanza della parola chiave SQL "BETWEEN".
- Correzione del [problema 1395](https://github.com/Microsoft/azuredatastudio/issues/1395): la parola chiave MATCH causa l'arresto anomalo del servizio SQL Tools.
- Correzione del [problema 1496](https://github.com/Microsoft/azuredatastudio/issues/1496): l'opzione "New Profiler" (Nuovo Profiler) del menu di scelta rapida Esplora oggetti non esegue operazioni.
- Correzione del [problema 1495](https://github.com/Microsoft/azuredatastudio/issues/1495): l'opzione "Spiega" per il piano di query nell'editor di query non funziona correttamente.

## <a name="may-2018"></a>Maggio 2018

7 maggio 2018 &nbsp; / &nbsp; versione: 0.29.3 &nbsp; / &nbsp; Anteprima pubblica

L'*Anteprima pubblica di maggio* è incentrata sulla stabilizzazione e sulle correzioni di bug.

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Annuncio dell'estensione Redgate SQL Search disponibile in Gestione estensioni. | &nbsp; |
| Localizzazione della community disponibile per 10 lingue. | Tedesco, spagnolo, francese, italiano, giapponese, coreano, portoghese, russo, cinese semplificato e cinese tradizionale. |
| Modifiche alla raccolta di dati di telemetria. | &bull; &nbsp; Riduzione della raccolta di dati di telemetria.<br/>&bull; &nbsp; Miglioramento dell'esperienza di rifiuto esplicito.<br/>&bull; &nbsp; Collegamenti nel prodotto all'Informativa sulla privacy. |
| Miglioramento dell'esperienza del marketplace in Gestione estensioni. | Individuazione più semplice delle estensioni della community. |
| Estensione SQL Agent. | &bull; &nbsp; Processi.<br/>&bull; &nbsp; Miglioramento della visualizzazione della cronologia processi. |
| Aggiornamenti per le estensioni whoisactive e Server Reports. | &nbsp; |
| Scorrimento migliorato della gestione delle proprietà del dashboard. | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="fix-github-issues"></a>Correzione di problemi GitHub

- Correzione del [problema 703](https://github.com/Microsoft/azuredatastudio/issues/703): l'immissione di testo di tipo HTML nell'area di modifica dati causa la visualizzazione non corretta del valore fino all'aggiornamento
- Correzione del [problema 821](https://github.com/Microsoft/azuredatastudio/issues/821): dipendenza del pacchetto azuredatastudio.deb
- Correzione del [problema 1260](https://github.com/Microsoft/azuredatastudio/issues/1260): parola chiave 'distinct' non evidenziata
- Correzione del [problema 1332](https://github.com/Microsoft/azuredatastudio/issues/1332): il ripristino riga in modifica dati non funziona
- Correzione del [problema 1215](https://github.com/Microsoft/azuredatastudio/issues/1215): estensione SQL Agent e barra di stato
- Correzione del [problema 1316](https://github.com/Microsoft/azuredatastudio/issues/1316): SQL Agent non viene ridimensionato dopo la modifica delle dimensioni della finestra

## <a name="april-2018"></a>Aprile 2018

25 aprile 2018 &nbsp; / &nbsp; versione: 0.28.6 &nbsp; / &nbsp; Anteprima pubblica

L'*Anteprima pubblica di aprile* contiene correzioni di bug e miglioramenti.

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Miglioramenti dell'estensione SQL Agent (anteprima): | &nbsp; |
| &nbsp; &nbsp; &nbsp; Supporto migliorato per i file. | &bull; &nbsp; File di grandi dimensioni.<br/>&bull; &nbsp; File protetti, per il salvataggio di file protetti da un amministratore.<br/>&bull; &nbsp; Archiviazione di file \>256 M all'interno di SQL Operations Studio. |
| &nbsp; &nbsp; &nbsp; Suddivisione del terminale integrato. | Uso simultaneo di più terminali aperti. |
| &nbsp; &nbsp; &nbsp; Installazioni e tempi di avvio più rapidi. | Riduzione del footprint del numero di file su disco dell'installazione. |
| &nbsp; | &nbsp; |

### <a name="fix-github-issues-april-2018"></a>Correzione di problemi GitHub, aprile 2018

- Correzione del [problema 37](https://github.com/Microsoft/azuredatastudio/issues/37): quando il visualizzatore grafico genera un errore, si verifica un comportamento imprevisto.
- Correzione del [problema 462](https://github.com/Microsoft/azuredatastudio/issues/462): Richiesta di funzionalità: opzione per espandere i gruppi di server per impostazione predefinita.
- Correzione del [problema 606](https://github.com/Microsoft/azuredatastudio/issues/606): IntelliSense - suggerimento non valido per il comando 'update'.
- Correzione del [problema 967](https://github.com/Microsoft/azuredatastudio/issues/967): quando si seleziona Showplan XML, nella griglia dei risultati deve comparire il piano di query.
- Correzione del [problema 1023](https://github.com/Microsoft/azuredatastudio/issues/1023): aggiunta di parentesi quadre per la chiamata a ms_foreachdb da flyfishingdba.
- Correzione del [problema 1048](https://github.com/Microsoft/azuredatastudio/issues/1048): errore di handshake SSL/TLS in fase di pre-accesso.
- Correzione del [problema 1050](https://github.com/Microsoft/azuredatastudio/issues/1050): cancellare la visualizzazione delle informazioni dettagliate prima di visualizzare errori.
- Correzione del [problema 1057](https://github.com/Microsoft/azuredatastudio/issues/1057): le azioni Ripristina e Nuova query nel widget Explorer non funzionano.
- Correzione del [problema 1068](https://github.com/Microsoft/azuredatastudio/issues/1068): viene visualizzata la finestra di output del dashboard con un messaggio di errore per il database SQL di Azure.
- Correzione del [problema 1069](https://github.com/Microsoft/azuredatastudio/issues/1069): alla prima visualizzazione, la finestra di dialogo della connessione mostra l'errore Server obbligatorio.
- Correzione del [problema 1070](https://github.com/Microsoft/azuredatastudio/issues/1070): ora per espandere i gruppi di server occorre un doppio clic.
- Correzione del [problema 1072](https://github.com/Microsoft/azuredatastudio/issues/1072): lo sfondo del controllo di selezione è semitrasparente.
- Correzione del [problema 1115](https://github.com/Microsoft/azuredatastudio/issues/1115): risoluzione di tutti i problemi di accessibilità relativi al contrasto elevato in SQL Operations Studio.
- Correzione del [problema 1101](https://github.com/Microsoft/azuredatastudio/issues/1101): il collegamento per il download manuale delle estensioni conduce a un percorso errato.
- Correzione del [problema 1103](https://github.com/Microsoft/azuredatastudio/issues/1103): scorrimento verticale non funzionante nella scheda Home.
- Correzione del [problema 1104](https://github.com/Microsoft/azuredatastudio/issues/1104): le schede dell'estensione SQL hanno smesso di funzionare.

### <a name="visual-studio-code-121-platform"></a>Piattaforma Visual Studio Code 1.21

Un aspetto chiave dell'Anteprima pubblica di aprile è l'aggiornamento del codice sorgente per la piattaforma Visual Studio Code 1.21. Questo introduce svariati aggiornamenti all'editor principale e all'area di lavoro rispetto al punto di sincronizzazione 1.19 precedente. Ecco alcuni esempi:

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| [Nuova interfaccia utente per le notifiche](https://code.visualstudio.com/updates/v1_21#_new-notifications-ui). | Gestione e revisione semplificate delle notifiche di SQL Operations Studio. |
| [Suddivisione del terminale integrato](https://code.visualstudio.com/updates/v1_21#_split-terminals). | Uso contemporaneo di più terminali aperti. |
| [Salvataggio di file di grandi dimensioni e protetti](https://code.visualstudio.com/updates/v1_20#_save-files-that-need-admin-privileges). | Salvataggio di file protetti dall'amministratore e file \>256 M all'interno di SQL Operations Studio. |
| [Miglioramento del supporto per file di grandi dimensioni](https://code.visualstudio.com/updates/v1_21#_text-buffer-improvements). | Ottimizzazioni del buffer di testo per file di grandi dimensioni. |
| [Miglioramento della ricerca impostazioni](https://code.visualstudio.com/updates/v1_20#_settings-search). | Individuazione semplice delle impostazioni corrette con la ricerca in linguaggio naturale. |
| [Frammenti di codice globali](https://code.visualstudio.com/updates/v1_20#_global-snippets). | Creazione di frammenti di codice che è possibile usare in tutti i tipi di file. |
| [Selezione multipla in Explorer](https://code.visualstudio.com/updates/v1_20#_multi-select-in-the-explorer). | Esecuzione di azioni su più file contemporaneamente. |
| [Errori e avvisi in Explorer](https://code.visualstudio.com/updates/v1_20#_error-indicators-in-the-explorer). | Passaggio rapido agli errori nella base di codice. |
| [Trascinamento della selezione e copia e incolla tra finestre](https://code.visualstudio.com/updates/v1_21#_better-drag-and-drop-support). | Spostamento di file tra le finestre aperte di SQL Operations Studio. |
| [Supporto del sottomodulo Git](https://code.visualstudio.com/updates/v1_20#_git-submodules). | Esecuzione di operazioni Git nei repository Git annidati. |
| [Supporto dell'utilità per la lettura dello schermo nel terminale](https://code.visualstudio.com/updates/v1_20#_screen-reader-support). | Ora il terminale integrato offre la modalità **ottimizzata per l'utilizzo con un'utilità per la lettura dello schermo**. |
| [Layout dell'editor centrato](https://code.visualstudio.com/updates/v1_21#_centered-editor-layout). | Ingrandimento della schermata di visualizzazione del codice. |
| [Risultati della ricerca orizzontali (anteprima](https://code.visualstudio.com/updates/v1_21#_horizontal-search)). | Ora è possibile visualizzare i risultati della ricerca in un pannello orizzontale. |
| &nbsp; | &nbsp; |

Per altre informazioni, vedere le [Note sulla versione di febbraio di Visual Studio Code](https://code.visualstudio.com/updates/v1_21) e le [Note sulla versione di gennaio di Visual Studio Code](https://code.visualstudio.com/updates/v1_20).

Per altre informazioni, vedere il [log delle modifiche](https://github.com/Microsoft/azuredatastudio/blob/main/CHANGELOG.md).

## <a name="march-2018"></a>Marzo 2018

28 marzo 2018 &nbsp; / &nbsp; versione: 0.27.3 &nbsp; / &nbsp; Anteprima pubblica

L'*Anteprima pubblica di marzo* continua a risolvere i principali problemi di GitHub ed è incentrata sul miglioramento dell'estendibilità, in particolare con l'abilitazione di Gestione estensioni, il miglioramento della gestione del dashboard e l'offerta delle estensioni SQL Agent e Insights. Questa versione include i miglioramenti seguenti:

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Miglioramento del modello di estendibilità del dashboard per supportare le informazioni dettagliate a schede e i riquadri di configurazione. | Gestione estensioni consente l'acquisizione semplice di estensioni.<br/><br/>Estensioni del dashboard per sp\_whoisactive da [whoisactive.com](http://www.whoisactive.com).<br/><br/>Per informazioni dettagliate, vedere [Estendere le funzionalità di SQL Operations Studio](./extensions/add-extensions.md). |
| Aggiunta di altre [API di estendibilità per la gestione delle connessioni e di Esplora oggetti](https://github.com/Microsoft/azuredatastudio/wiki/Extensibility-API). | &nbsp; |
| Correzione di importanti [problemi GitHub](https://github.com/Microsoft/azuredatastudio/issues) che incidono sui clienti. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="february-2018"></a>Febbraio 2018

15 febbraio 2018 &nbsp; / &nbsp; versione: 0.26.7 &nbsp; / &nbsp; Anteprima pubblica

L'*Anteprima pubblica di febbraio* include alcuni suggerimenti di funzionalità e correzioni di bug ad alta priorità. Questa versione include i miglioramenti seguenti:

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Introduzione della funzionalità di installazione automatica degli aggiornamenti automatico, che fornisce una notifica quando una nuova versione è disponibile per il download. | &nbsp; |
| Il campo **Database** della finestra di dialogo di connessione è ora un elenco a discesa popolato dinamicamente che conterrà un elenco di database popolati dal server specificato. | &nbsp; |
| Introduzione all'API di estendibilità della connessione. | &nbsp; |
| Integrazione dell'editor VS Code 1.19. | &nbsp; |
| Aggiornamento del componente JustinPealing/html-query-plan per implementare diversi miglioramenti del visualizzatore del piano di query. | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="fixed-issues-february-2018"></a>Problemi risolti, febbraio 2018

- Correzione del [problema 6](https://github.com/Microsoft/azuredatastudio/issues/6): mantenere la connessione e il database selezionati all'apertura di nuove schede di query.
- Correzione del [problema 22](https://github.com/Microsoft/azuredatastudio/issues/22): "Nome server" e "Nome database": possono essere elenchi a discesa anziché caselle di testo?
- Correzione del [problema 549](https://github.com/Microsoft/azuredatastudio/issues/549): l'installazione con l'opzione Silent/Verysilent produce l'apertura dell'applicazione dopo l'installazione.
- Correzione del [problema 481](https://github.com/Microsoft/azuredatastudio/issues/481): aggiunta dell'opzione "Verifica disponibilità aggiornamenti".
- Correzioni per la colorazione e il completamento automatico dell'editor SQL:
  - Correzione del [problema 584](https://github.com/Microsoft/azuredatastudio/issues/584): parola chiave "FULL" non evidenziata da IntelliSense.
  - Correzione del [problema 345](https://github.com/Microsoft/azuredatastudio/issues/345): colorazione delle funzioni SQL all'interno dell'editor.
  - Correzione del [problema 300](https://github.com/Microsoft/azuredatastudio/issues/300): [#tempData] latest "] visualizzerà il colore verde.
  - Correzione del [problema 225](https://github.com/Microsoft/azuredatastudio/issues/225): il colore delle parole chiave non corrisponde.
  - Correzione del [problema 60](https://github.com/Microsoft/azuredatastudio/issues/60): evidenziazione non valida del colore della sintassi SQL quando si usa l'inner join di tabella temporanea da una clausola.

## <a name="january-2018"></a>Gennaio 2018

17 gennaio 2018 &nbsp; / &nbsp; versione: 0.25.4 &nbsp; / &nbsp; Anteprima pubblica

L'*Anteprima pubblica di gennaio* include alcuni suggerimenti di funzionalità e correzioni di bug ad alta priorità. Questa versione include i miglioramenti seguenti:

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Le connessioni al server salvate sono disponibili nella finestra di dialogo di connessione. | &nbsp; |
| Abilitazione Hot Exit. Per impostazione predefinita, la funzionalità Hot Exit è disattivata. Per abilitarla, vedere [Impostazione di Hot Exit](settings.md#hot-exit). | &nbsp; |
| Colorazione delle schede basata sul gruppo di server. La colorazione delle schede è disattivata per impostazione predefinita. Per abilitarla, vedere [Impostazione del colore delle schede](settings.md#tab-color). | &nbsp; |
| Modifica di *Nome server* in *Server* nella finestra di dialogo di connessione. | &nbsp; |
| Correzione del comando *Run Current Query* (Esegui query corrente) non funzionante. | &nbsp; |
| Correzione del bug per cui il trascinamento della selezione interrompe le operazioni di scripting. | &nbsp; |
| Correzione della posizione errata dell'icona del menu Start. | &nbsp; |
| Correzione dell'icona di personalizzazione dell'account Azure mancante. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="december-2017"></a>Dicembre 2017

19 dicembre 2017 &nbsp; / &nbsp; versione: 0.24.1 &nbsp; / &nbsp; Anteprima pubblica

L'*Anteprima pubblica di dicembre* include diverse correzioni di bug in tutte le aree di funzionalità, oltre ai miglioramenti seguenti:

&nbsp;

| Modifica | Dettagli |
| :----- | :------ |
| Ora è disponibile una finestra di dialogo per la creazione di una nuova regola del firewall per facilitare la connessione al database SQL di Azure e ad Azure Synapse Analytics. | &nbsp; |
| Aggiunta dei pacchetti di installazione Windows, Linux DEB e RPM. | &nbsp; |
| Editor del layout visivo Manage Dashboard (Gestisci dashboard). | &nbsp; |
| Comandi *Script come Alter* e *Script come Execute*. | &nbsp; |
| Comando *Run Current Query with Actual Plan* (Esegui query corrente con piano effettivo). | &nbsp; |
| Integrazione della piattaforma dell'editor VS Code 1.18.1. | &nbsp; |
| Abilitazione del trasferimento locale dei file di estensione VSIX. | &nbsp; |
| Supporto della sintassi di iterazione batch "GO N". | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="november-2017"></a>Novembre 2017

15 novembre 2017 &nbsp; / &nbsp; versione: 0.23.6

- Versione iniziale di Azure Data Studio.

## <a name="next-steps"></a>Passaggi successivi

Per iniziare, vedere uno degli argomenti di avvio rapido seguenti:

- [Connettersi ed eseguire query in SQL Server](quickstart-sql-server.md)
- [Connettersi ed eseguire query nel database SQL di Azure](quickstart-sql-database.md)
- [Connettersi ed eseguire query in Azure Synapse Analytics](quickstart-sql-dw.md)

Contribuire ad Azure Data Studio:

- [https://github.com/Microsoft/azuredatastudio](https://github.com/Microsoft/azuredatastudio)
