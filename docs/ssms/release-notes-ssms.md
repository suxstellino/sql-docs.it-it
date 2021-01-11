---
title: Note sulla versione di SSMS
description: Note sulla versione di SQL Server Management Studio (SSMS).
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 3dc76cc1-3b4c-4719-8296-f69ec1b476f9
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 12/17/2020
ms.openlocfilehash: 346e964b30f76bf79c20d6f348636a626a8beee2
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642382"
---
# <a name="release-notes-for-sql-server-management-studio-ssms"></a>Note sulla versione per SQL Server Management Studio (SSMS)

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Questo articolo fornisce informazioni dettagliate sugli aggiornamenti, i miglioramenti e le correzioni di bug per le versioni correnti e precedenti di SSMS.

## <a name="current-ssms-release"></a>Versione corrente di SSMS

### <a name="188"></a>18,8

![download](media/download-icon.png) [Scaricare SSMS 18.8](download-sql-server-management-studio-ssms.md)

- Numero di versione: 18.8
- Numero di build: 15.0.18369.0
- Data di rilascio: 17 dicembre 2020

[Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x40a)

SSMS 18.8 è la versione più recente di SSMS disponibile a livello generale. Se è necessaria una versione precedente, vedere [Versioni precedenti di SSMS](release-notes-ssms.md#previous-ssms-releases).

#### <a name="whats-new-in-188"></a>Novità della versione 18.8

[!INCLUDE [ssms-ads-install](../includes/ssms-azure-data-studio-install.md)]

| Nuovo elemento | Dettagli |
|----------|---------|
| Integrazione dell'installazione di Azure Data Studio | Con SSMS viene installato anche Azure Data Studio 1.25.1. |
| Analysis Services | Aggiunta del supporto per le aree di lavoro di Power BI Premium Gen2. |
| Controllo | Aggiunta del supporto per EXTERNAL_MONITOR e "controllo operatore".  Per altre informazioni, vedere la [documentazione](https://docs.microsoft.com/azure/azure-sql/managed-instance/auditing-configure) di configurazione. |
| Integration Services | Miglioramento della creazione guidata del runtime di integrazione in modo che la creazione di SSISDB sia facoltativa quando si crea SSIS IR |

#### <a name="bug-fixes-in-188"></a>Correzioni di bug nella versione 18.8

| Nuovo elemento | Dettagli |
|----------|---------|
| Accessibilità | Correzione dei problemi che interessano l'Assistente vocale nella finestra di dialogo ProgressReport |
| Monitoraggio attività | Correzione di un problema per cui il menu di scelta rapida "Modifica testo query" nella griglia "Query attive con costo elevato" non funzionava |
| Always Encrypted | Correzione di un problema per cui SSMM poteva generare un errore di runtime ("Classe non trovata") |
| Valori DPI alti/Ridimensionamento | Miglioramento della visualizzazione 4K della finestra di dialogo ProgressReport |
| Valori DPI alti/Ridimensionamento | Correzione del problema relativo al modulo "Nuova pianificazione - Pacchetto" in Integration Services |
| SQL Server Management Studio (SSMS) - Generale |  Correzione di un problema per cui SSMS generava un errore quando si tentavano di visualizzare le proprietà di una colonna con un valore di inizializzazione Identity o di incremento di identità superiore alle dimensioni di un oggetto bigint.
| SQL Server Management Studio (SSMS) - Generale | Correzione di un arresto anomalo in SSMS causato da una voce di menu "Projects Export Templates..." (Modelli di esportazione progetti) non destinata alla visualizzazione |
| SQL Server Management Studio (SSMS) - Generale | Correzione di un problema che poteva causare l'arresto anomalo di SSMS durante l'enumerazione dei backup in una cartella |
| SQL Server Management Studio (SSMS) - Generale | Correzione di un problema che causava la perdita di handle e memoria da SSMS durante la visualizzazione dei risultati delle query |
| SQL Server Management Studio (SSMS) - Generale | Correzione di un problema per cui gli utenti non erano in grado di creare un indice columnstore cluster per colonne di tipi particolari (UniqueIdentifier in SQL 2014 e versioni successive, VarCharMax/NVarCharMax/VarBinaryMax in SQL 2017 e versioni successive) |
| SQL Server Management Studio (SSMS) - Generale | Correzione di un problema rilevato da tempo per cui il riordinamento delle colonne XML nella "Griglia risultati" non funzionava. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/32890930) |
| SQL Server Management Studio (SSMS) - Generale | Correzione di un problema per cui SSMS visualizzava erroneamente un avviso relativo al mancato supporto del rilevamento delle modifiche per un database con oggetti con ottimizzazione per la memoria. |
| Procedura guidata Genera script | Correzione di un bug per cui veniva bloccata l'enumerazione degli oggetti di database in SQL Server 2014 e versioni precedenti. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/41885587). |
| Strumenti di replica | Correzione di un problema per cui Monitoraggio replica SQL non rispettava le impostazioni di connessione automatica, chiedendo all'utente di eseguire ogni volta la riconnessione manuale. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/40394743) |
| SMO/scripting | Correzione di un problema durante il tentativo di usare "Genera script..." in una vista crittografata nel database SQL di Azure |
| Interfaccia utente XEvent | Correzione di un problema che poteva causare l'arresto anomalo di SSMS durante il passaggio da altre applicazioni a SSMS. |
| Interfaccia utente XEvent | Ripristino del miglioramento dalla versione 18.7 che ha generato un bug nella creazione di XEvent a causa di virgolette aggiuntive. *Questa modifica è temporanea e il miglioramento verrà riportato in una versione successiva senza il bug associato.* |

#### <a name="known-issues-188"></a>Problemi noti (18.8)

| Nuovo elemento | Dettagli | Soluzione alternativa |
|----------|---------|------------|
| Analysis Services | Errore durante la connessione a SSAS tramite msmdpump.dll. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696). | N/D |
| Analysis Services | Quando si usa la configurazione di aggiornamento, in rari casi può essere visualizzato un errore "Oggetto non impostato su un'istanza di un oggetto" quando si tenta di aprire l'editor DAX dopo l'aggiornamento di SSMS. | Disinstallazione e reinstallazione di SQL Server Management Studio.  Se la reinstallazione non risolve il problema, chiudere tutte le istanze di SSMS, eseguire il backup e quindi rimuovere `%AppData%\Microsoft\SQL Server Management Studio` e `%LocalAppData%\Microsoft\SQL Server Management Studio`. |
| SQL Server Management Studio (SSMS) - Generale | La finestra di dialogo Nuova specifica controllo server può causare l'arresto anomalo di SSMS con un errore di violazione di accesso. | N/D |
| SQL Server Management Studio (SSMS) - Generale | Le estensioni di SSMS che usano SMO devono essere ricompilate usando come destinazione il nuovo pacchetto SMO v161 specifico di SSMS. Una versione di anteprima è disponibile all'indirizzo https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects.SSMS/ </br></br> Le estensioni compilate con le versioni 160 precedenti del pacchetto Microsoft.SqlServer.SqlManagementObjects funzioneranno ancora. | N/D |
| Integration Services | Quando si importano o esportano pacchetti in Integration Services o si esportano pacchetti in Azure-SSIS Integration Runtime, gli script vanno persi per i pacchetti che contengono attività o componenti di script. | Rimuovere la cartella "C:\Programmi (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild". |
| Integration Services | Le connessioni remote a Integration Services possono avere esito negativo e visualizzare il messaggio "Il servizio specificato non esiste come servizio installato" in un sistema operativo più recente. | Identificare il percorso del registro di sistema correlato di Integration Services in Computer\HKEY_CLASSES_ROOT\AppID & Computer\HKEY_CLASSES_ROOT\ WOW6432Node\AppID e all'interno di questi hive rinominare la chiave del registro di sistema denominata 'LocalService' in 'LocalService_A' per la versione specifica di Integration Services che si sta tentando di connettere |


È possibile fare riferimento al [sito dei commenti e suggerimenti degli utenti per SQL Server](https://feedback.azure.com/forums/908035-sql-server) per informazioni su altri problemi noti e per inviare commenti e suggerimenti al team del prodotto.

## <a name="previous-ssms-releases"></a>Versioni precedenti di SSMS

[!INCLUDE[ssms-connect-aazure-ad](../includes/ssms-connect-azure-ad.md)]

Scaricare le versioni precedenti di SSMS selezionando il collegamento per il download nella sezione correlata.

| Versione di SSMS | Numero di build | Data di rilascio |
|--------------|--------------|--------------|
| [18.7.1](#1871) | 15.0.18358.0 | 27 ottobre 2020 |
| [18,7](#187) | 15.0.18357.0 | 20 ottobre 2020 |
| [18.6](#186) | 15.0.18338.0 | 22 luglio 2020 |
| [18.5.1](#1851) | 15.0.18333.0 | 09 giugno 2020 |
| [18.5](#185) | 15.0.18330.0 | 7 aprile 2020 |
| [18.4](#184) | 15.0.18206.0 | 4 novembre 2019 |
| [18.3.1](#1831) | 15.0.18183.0 | 2 ottobre 2019 |
| [18.2](#182) | 15.0.18142.0 | 25 luglio 2019 |
| [18.1](#181) | 15.0.18131.0 | 11 giugno 2019 |
| [18.0](#180) | 15.0.18118.0 | 24 aprile 2019 |
| [17.9.1](#1791) | 14.0.17289.0 | 21 novembre 2018 |
| [16.5.3](#1653) | 13.0.16106.4 | 30 gennaio 2017 |

### <a name="1871"></a>18.7.1

![download](media/download-icon.png) [Scaricare SSMS 18.7](https://go.microsoft.com/fwlink/?linkid=2147207)

- Numero di versione: 18.7.1
- Numero di build: 15.0.18358.0
- Data di rilascio: 27 ottobre 2020

[Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x40a)

SSMS 18.7 è la versione più recente di SSMS disponibile a livello generale. Se è necessaria una versione precedente, vedere [Versioni precedenti di SSMS](release-notes-ssms.md#previous-ssms-releases).

#### <a name="whats-new-in-1871"></a>Novità della versione 18.7.1

[!INCLUDE [ssms-ads-install](../includes/ssms-azure-data-studio-install.md)]


#### <a name="bug-fixes-in-1871"></a>Correzioni di bug nella versione 18.7.1

| Nuovo elemento | Dettagli |
|----------|---------|
| Archivio query | Correzione dell'errore generato quando si fa clic con il pulsante destro del mouse sul nodo di Esplora oggetti per Query Store. |


#### <a name="known-issues-1871"></a>Problemi noti (18.7.1)

| Nuovo elemento | Dettagli | Soluzione alternativa |
|----------|---------|------------|
| Analysis Services | Errore durante la connessione a SSAS tramite msmdpump.dll. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696). | N/D |
| Analysis Services | Quando si usa la configurazione di aggiornamento, in rari casi può essere visualizzato un errore "Oggetto non impostato su un'istanza di un oggetto" quando si tenta di aprire l'editor DAX dopo l'aggiornamento di SSMS. | Per risolvere questo problema, disinstallare e reinstallare SSMS. |
| Editor di query DAX | L'apertura dell'editor di query DAX causa un errore per cui "un oggetto non viene impostato sull'istanza di un oggetto" | Disinstallazione e reinstallazione di SQL Server Management Studio.  Se la reinstallazione non risolve il problema, chiudere tutte le istanze di SSMS, eseguire il backup e quindi rimuovere `%AppData%\Microsoft\SQL Server Management Studio` e `%LocalAppData%\Microsoft\SQL Server Management Studio`. |
| SQL Server Management Studio (SSMS) - Generale | La finestra di dialogo Nuova specifica controllo server può causare l'arresto anomalo di SSMS con un errore di violazione di accesso. | N/D |
| SQL Server Management Studio (SSMS) - Generale | Le estensioni di SSMS che usano SMO devono essere ricompilate usando come destinazione il nuovo pacchetto SMO v161 specifico di SSMS. Una versione di anteprima è disponibile all'indirizzo https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects.SSMS/ </br></br> Le estensioni compilate con le versioni 160 precedenti del pacchetto Microsoft.SqlServer.SqlManagementObjects funzioneranno ancora. | N/D |
| Procedura guidata Genera script | La procedura guidata ha esito negativo quando si tenta di enumerare gli oggetti di database in SQL Server 2014 e versioni precedenti. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/41885587). | Usare SSMS 18.6 per selezionare gli oggetti nella procedura guidata di generazione script per SQL 2014 e versioni precedenti. |
| Integration Services | Quando si importano o esportano pacchetti in Integration Services o si esportano pacchetti in Azure-SSIS Integration Runtime, gli script vanno persi per i pacchetti che contengono attività o componenti di script. | Rimuovere la cartella "C:\Programmi (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild". |
| Integration Services | Le connessioni remote a Integration Services possono avere esito negativo e visualizzare il messaggio "Il servizio specificato non esiste come servizio installato" in un sistema operativo più recente. | Identificare il percorso del registro di sistema correlato di Integration Services in Computer\HKEY_CLASSES_ROOT\AppID & Computer\HKEY_CLASSES_ROOT\ WOW6432Node\AppID e all'interno di questi hive rinominare la chiave del registro di sistema denominata 'LocalService' in 'LocalService_A' per la versione specifica di Integration Services che si sta tentando di connettere |
| Esplora oggetti | Le versioni di SSMS precedenti alla 18.7 presentano una modifica sostanziale in Esplora oggetti a causa delle modifiche del motore relative al [pool SQL serverless in Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics/sql/on-demand-workspace-overview). | Per continuare a usare Esplora oggetti in SSMS con pool SQL serverless in Azure Synapse Analytics, è necessario SSMS 18.7 o versione successiva. |

### <a name="187"></a>18,7

![download](media/download-icon.png) [Scaricare SSMS 18.7](https://go.microsoft.com/fwlink/?linkid=2146265)

- Numero di versione: 18.7
- Numero di build: 15.0.18357.0
- Data di rilascio: 20 ottobre 2020

[Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x40a)

SSMS 18.7 è la versione più recente di SSMS disponibile a livello generale. Se è necessaria una versione precedente, vedere [Versioni precedenti di SSMS](release-notes-ssms.md#previous-ssms-releases).

#### <a name="whats-new-in-187"></a>Novità della versione 18.7

[!INCLUDE [ssms-ads-install](../includes/ssms-azure-data-studio-install.md)]

| Nuovo elemento | Dettagli |
|----------|---------|
| Integrazione dell'installazione di Azure Data Studio | Con SSMS viene installato anche Azure Data Studio. |
| Always Encrypted | Per riconoscere i nuovi endpoint HSM, è necessario aggiornare SSMS. Questa operazione viene eseguita utilizzando il nuovo NugetPackage del provider Azure Key Vault. |
| Importare file flat | È stato apportato un miglioramento che consente di stimare con maggior precisione i tipi di dati tramite l'apprendimento di 300 righe per impostazione predefinita. |
| Importare file flat | Viene impedito che le colonne siano dichiarate come TinyInt quando dovrebbero essere SmallInt. |
| Importare file flat | È stato apportato un miglioramento che consente di pulire correttamente le tabelle di DW in caso di errore nell'importazione dei dati. |
| Resource Governor | Aggiunta del supporto per i valori decimali. |
| Showplan | Aggiunta dell'operatore PREDICT. |
| Interfaccia utente XEvent | Aggiunta della capacità di inserire nello script gli eventi estesi usando il nome wait_type. Gli utenti richiedono l'uso del valore della colonna map_value anziché map_key nel predicato del filtro wait_type perché il valore della chiave è soggetto a modifica durante l'aggiornamento della versione. Correzione: è stata aggiunta una casella di controllo ed è stata data agli utenti la possibilità di scegliere map_value o map_key per il valore del predicato del filtro wait_type. |

#### <a name="bug-fixes-in-187"></a>Correzioni di bug nella versione 18.7

| Nuovo elemento | Dettagli |
|----------|---------|
| Accessibilità | Correzione dell'ordine di tabulazione dei pulsanti nella finestra "Le impostazioni seguenti non sono supportate dal database" visualizzata quando si esegue una query in DW. |
| Accessibilità | Importazione/esportazione guidata: il layout di pagina non è corretto nella modalità con valori DPI alti. |
| Monitoraggio attività | Correzione di un problema per cui Monitoraggio attività era in pausa quando si apriva la scheda "Processi". Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37050118). |
| Gruppo di disponibilità Always On | Correzione di un problema per cui il failover del gruppo di disponibilità con scalabilità in lettura non funzionava. |
| Analysis Services | Aggiornamento dei componenti PowerQuery alla versione 2.84.982 per gli scenari di modelli tabulari di AS. |
| Analysis Services | Correzione di un problema che avrebbe potuto causare un errore durante il tentativo di connessione a SSAS tramite msmdpump.dll. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696). |
| Backup/Ripristino | Correzione di un problema a causa del quale la selezione di "Visualizza proprietà connessione" restituiva un errore SMO nella proprietà HostDistribution mancante per SQL 2016 e versioni precedenti. |
| Progettazione database | Correzione di un problema che causava l'arresto anomalo di SSMS durante la gestione dei numeri decimali. |
| Diagrammi di database | Correzione di un problema che potrebbe causare un arresto anomalo o un blocco di SSMS quando si usano diagrammi di database in cui la finestra di dialogo "Aggiungi tabella" non è stata visualizzata correttamente. |
| Mirroring del database | Correzione di un problema che causava l'esito negativo della configurazione mirror. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/32897281). |
| SQL Server Management Studio (SSMS) - Generale | Correzione di un problema per cui il tentativo di connessione a un database SQL di Azure potrebbe richiedere alcuni secondi (accesso SQL in un database utente). |
| SQL Server Management Studio (SSMS) - Generale | Correzione di un problema per cui SSMS non gestiva/visualizzava il deadlock acquisito (file con estensione xdl). |
| SQL Server Management Studio (SSMS) - Generale | Correzione di un problema per cui il tentativo di aprire le impostazioni del log degli errori per SQL Server 2008 R2 e versioni precedenti aveva esito negativo con la proprietà ErrorLogSizeKb non trovata. |
| SQL Server Management Studio (SSMS) - Generale | Correzioni e miglioramenti generali relativi al supporto del [pool SQL serverless in Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics/sql/on-demand-workspace-overview). |
| Importare file flat | Correzione di un problema per cui la procedura guidata non rilevava il possibile utilizzo del file in un'altra applicazione generando invece un errore. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/40761574). |
| Importa/Esporta applicazione livello dati | Correzione del livello di servizio predefinito su S0 Standard durante l'importazione di un file bacpac (stesso comportamento del portale di Azure e SqlPackage.exe). |
| Importare file flat | Correzione di un problema per cui la procedura guidata non rilevava il possibile utilizzo del file in un'altra applicazione generando invece un errore. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/40761574). |
| Integration Services | Correzione di un problema per cui gli elementi ComboBox della sottoscrizione di Azure sono duplicati nella creazione guidata del runtime di integrazione e nella migrazione guidata dei processi quando sottoscrizioni diverse hanno lo stesso nome. |
| Integration Services | Correzione di un problema che in alcune situazioni rendeva impossibile l'abilitazione del pulsante Connetti nella creazione guidata del runtime di integrazione. |
| Integration Services | Correzione di un problema per cui gli elementi del controllo ComboBox "Usa variabile di ambiente" nella finestra di dialogo "Imposta valore parametro" non sono in ordine. |
| Intellisense | Correzione di un arresto anomalo in SSMS che poteva verificarsi durante l'esecuzione di una query. |
| Intellisense | Correzione di un problema per cui IntelliSense non funziona quando l'utente è connesso a un gruppo di disponibilità configurato per il routing di sola lettura. |
| Server collegati | Correzione di un problema per cui un utente con autorizzazione CONTROL SERVER (ma non nel ruolo sysadmin) non era in grado di aggiungere un server collegato. |
| Visualizzatore log | Correzione di un problema per cui un utente con autorizzazioni VIEW SERVER STATE non era in grado di visualizzare i log degli errori di SQL Server. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/32899204). |
| Esplora oggetti | Correzione di un problema per cui la selezione del menu **Avvia PowerShell** in alcuni nodi di Esplora oggetti, quali "Gestione dei criteri" ed "Eventi estesi", determinerebbe l'avvio non corretto di PowerShell. |
| Server registrati | Correzione di un problema per cui SSMS veniva arrestato in modo anomalo durante il tentativo di registrazione di un server di gestione centrale. |
| Server registrati | Correzione del problema per cui le voci di menu per l'avvio di Azure Data Studio dai server registrati risultavano mancanti. |
| Report | Correzione di un problema per cui, quando in Performance Dashboard si tenta di passare a sottocollegamenti quali **Query dispendiose**, l'operazione non riesce. Questo problema risultava comune nella maggior parte delle versioni non in lingua inglese di SSMS. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/41454499). |
| Showplan | Correzione di un problema che provocava l'arresto anomalo di SSMS quando per la ricerca di testo si usava Trova nodo. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/40421650). |
| Showplan | Aggiunto il suffisso KB nella riga della descrizione comando della concessione di memoria |
| Valutazione della vulnerabilità | Correzione di un problema che determinava la generazione di un errore SSMS durante il tentativo di impostazione delle linee di base nella valutazione della vulnerabilità. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/40578565). |
| Interfaccia utente XEvent | Correzione del problema per cui premendo il tasto F1 non si accedeva alla pagina corretta di DOCS. |
| Interfaccia utente XEvent | Correzione nel log nel visualizzatore XEvent in cui la descrizione comando non visualizzava correttamente il testo codificato tramite coppie di surrogati. |

#### <a name="known-issues-187"></a>Problemi noti (18.7)

| Nuovo elemento | Dettagli | Soluzione alternativa |
|----------|---------|------------|
| Analysis Services | Errore durante la connessione a SSAS tramite msmdpump.dll. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696). | N/D |
| Analysis Services | Quando si usa la configurazione di aggiornamento, in rari casi può essere visualizzato un errore "Oggetto non impostato su un'istanza di un oggetto" quando si tenta di aprire l'editor DAX dopo l'aggiornamento di SSMS. | Per risolvere questo problema, disinstallare e reinstallare SSMS. |
| SQL Server Management Studio (SSMS) - Generale | La finestra di dialogo Nuova specifica controllo server può causare l'arresto anomalo di SSMS con un errore di violazione di accesso. | N/D |
| SQL Server Management Studio (SSMS) - Generale | Le estensioni di SSMS che usano SMO devono essere ricompilate usando come destinazione il nuovo pacchetto SMO v161 specifico di SSMS. Una versione di anteprima è disponibile all'indirizzo https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects.SSMS/ </br></br> Le estensioni compilate con le versioni 160 precedenti del pacchetto Microsoft.SqlServer.SqlManagementObjects funzioneranno ancora. | N/D |
| Procedura guidata Genera script | La procedura guidata ha esito negativo quando si tenta di enumerare gli oggetti di database in SQL Server 2014 e versioni precedenti. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/41885587). | Usare SSMS 18.6 per selezionare gli oggetti nella procedura guidata di generazione script per SQL 2014 e versioni precedenti. |
| Integration Services | Quando si importano o esportano pacchetti in Integration Services o si esportano pacchetti in Azure-SSIS Integration Runtime, gli script vanno persi per i pacchetti che contengono attività o componenti di script. Soluzione alternativa: Rimuovere la cartella "C:\Programmi (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild". | N/D |
| Integration Services | Le connessioni remote a Integration Services possono avere esito negativo e visualizzare il messaggio "Il servizio specificato non esiste come servizio installato" in un sistema operativo più recente. Soluzione alternativa: Identificare il percorso del registro di sistema correlato di Integration Services in Computer\HKEY_CLASSES_ROOT\AppID & Computer\HKEY_CLASSES_ROOT\ WOW6432Node\AppID e all'interno di questi hive rinominare la chiave del registro di sistema denominata 'LocalService' in 'LocalService_A' per la versione specifica di Integration Services che si sta tentando di connettere | N/D |
| Esplora oggetti | Le versioni di SSMS precedenti alla 18.7 presentano una modifica sostanziale in Esplora oggetti a causa delle modifiche del motore relative al [pool SQL serverless in Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics/sql/on-demand-workspace-overview). | Per continuare a usare Esplora oggetti in SSMS con pool SQL serverless in Azure Synapse Analytics, è necessario SSMS 18.7 o versione successiva. |
| Archivio query | Il nodo di Esplora oggetti per Query Store genera un errore quando si fa clic con il pulsante destro del mouse. | Per accedere direttamente agli elementi, espandere il nodo e fare clic con il pulsante destro del mouse su singole opzioni figlio. |

### <a name="186"></a>18.6

![download](media/download-icon.png) [Scaricare SSMS 18.6](https://go.microsoft.com/fwlink/?linkid=2135491)

- Numero di versione: 18.6
- Numero di build: 15.0.18338.0
- Data di rilascio: 22 luglio 2020

[Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x40a)

#### <a name="whats-new-in-186"></a>Novità della versione 18.6

| Nuovo elemento | Dettagli |
|----------|---------|
| Analysis Services | Aggiornato alla versione più recente delle librerie client di Analysis Services. |
| Controllo | Aggiunta del supporto per l'ID azione di SENSITIVE_BATCH_COMPLETED_GROUP (stringa anziché numero). |
| Controllo | Sono stati aggiunti i campi seguenti al visualizzatore di controllo: affected_rows, response_rows, connection_id, duration_milliseconds e data_sensitivity_information. |
| Classificazione dei dati | Aggiornamento di SSMS per supportare l'importazione/esportazione dei criteri esportati tramite i cmdlet di PowerShell. |
| Importare file flat | Aggiunto il supporto per i file a larghezza fissa e il rilevamento dei tipi di file per i file CSV/TSV per assicurarsi che vengano analizzati rispettivamente come file CSV/TSV. |
| Integration Services | Aggiunta del supporto per i processi dell'agente di Istanza gestita di SQL di Azure per eseguire un pacchetto SSIS dall'archivio pacchetti in Azure-SSIS IR. |
| SMO/scripting | Aggiunta del supporto per lo scripting di Dynamic Data Masking in [Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is) (in precedenza SQL Azure DW). |
| SMO/scripting | Aggiunta del supporto per lo scripting dei criteri di sicurezza in [Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is). |

#### <a name="bug-fixes-in-186"></a>Correzioni di bug nella versione 18.6

| Nuovo elemento | Dettagli |
|----------|---------|
| Accessibilità | Modifica dei colori dei bordi per l'accessibilità nella **pagina delle proprietà generali del database** (il bordo intorno alla griglia e alla casella del nome è più scuro per impostare un contrasto > 3:1). |
| Accessibilità | Aggiunta della gestione per l'esecuzione di query per aggiornare l'Assistente vocale (richiede NetFx 4.8+ installato nel computer). |
| Always Encrypted | Correzione del problema a causa del quale nella finestra di dialogo *Nuova chiave di crittografia della colonna* viene indicato che CEK non è abilitato per l'enclave anche se CMK è abilitato per l'enclave. |
| Analysis Services | Correzione di un problema durante la visualizzazione delle partizioni di Analysis Services che potrebbe causare un'eccezione non gestita. |
| **Diagrammi di database** | Correzione di un problema di lunga data relativo ai **diagrammi di database**, che causava sia il danneggiamento dei diagrammi esistenti che l'arresto anomalo di SSMS. Se è stato creato o salvato un diagramma con una versione di SSMS dalla 18.0 alla 18.5.1 e tale diagramma include un'*annotazione di testo*, non sarà possibile aprire il diagramma in alcuna versione di SSMS. Con questa correzione, SSMS 18.6 può aprire e salvare un diagramma creato in SSMS 17.9.1 e versioni precedenti. Il diagramma può anche essere aperto in SSMS 17.9.1 e versioni precedenti dopo averlo salvato in SSMS 18.6. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37992649). |
| Classificazione dei dati | Correzione di un problema a causa del quale il nome della colonna non viene visualizzato nel pannello delle raccomandazioni del riquadro classificazione dati. |
| SQL Server Management Studio (SSMS) - Generale | Correzione di un problema a causa del quale le proprietà del database *Dimensioni* e *Spazio disponibile* hanno valori non corretti per il database SQL di Azure (livello di servizio Hyperscale). |
| SQL Server Management Studio (SSMS) - Generale | Correzione di un problema a causa del quale le proprietà del database "Dimensioni" visualizzavano le dimensioni massime invece delle dimensioni effettive del database per i database SQL di Azure (nota: per SQL Data Warehouse vengono ancora visualizzate le dimensioni massime). |
| SQL Server Management Studio (SSMS) - Generale | Sono state risolte tre origini comuni di blocchi in SSMS. |
| SQL Server Management Studio (SSMS) - Generale | Sono stati risolti alcuni problemi relativi alla finestra di dialogo di connessione di SSMS che *dimenticava* i dati (server/utente/password). Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/40256401) e questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/40015519). |
| SQL Server Management Studio (SSMS) - Generale | Correzione di un problema nella finestra di dialogo delle **proprietà per le statistiche** a causa del quale la selezione della casella di controllo **Aggiorna statistiche per le colonne selezionate** e la selezione di **OK** non producono alcun effetto. Le statistiche non vengono aggiornate e il tentativo di creare uno script per l'azione restituisce un messaggio *Nessuna azione per cui generare uno script*. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37799992). |
| SQL Server Management Studio (SSMS) - Generale | Sono stati risolti i problemi relativi a [CVE-2020-1455](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2020-1455). | 
| Importa/Esporta applicazione livello dati | Correzione di un problema a causa del quale SSMS generava un errore durante l'importazione di un file bacpac. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/40229137). |
| Integration Services | Correzione di un bug a causa del quale i clienti non possono modificare un passaggio di processo di SQL Agent quando usano SSMS versione 18.4 o precedenti per eseguire pacchetti SSIS in Istanza gestita di SQL di Azure. |
| Integration Services | Correzione di un bug a causa del quale l'opzione **Usa runtime a 32 bit** non è presente nella scheda **Opzioni di esecuzione** per eseguire un pacchetto SSIS in un passaggio di processo di SQL Agent per un'istanza locale di SQL Server. |
| IntelliSense/editor | Correzione di un problema a causa del quale è possibile che venga visualizzata una finestra di dialogo di errore quando si esegue il comando File -> Nuovo -> Query del motore di database. |
| Esplora oggetti | Correzione di un problema a causa del quale la *finestra Proprietà* per il database SQL di Azure non era disponibile facendo clic con il pulsante destro del mouse su un nodo Tabella o Indice in Esplora oggetti. |
| Esplora oggetti | Correzione di un problema a causa del quale SSMS non è in grado di espandere il nodo dei database master in Azure in presenza di un'interruzione del piano di controllo che interessa sys.database_service_objectives. |
| Report | Correzione di diversi report standard che non funzionavano in Linux </br></br> Esempio: Il report Consumo memoria non funzionava con un errore simile a "/var/opt/mssql/log/log_116.trc\log.trc' non è valido…". |
| SMO/scripting | Aggiornamento della logica per creare nuovi database SQL di Azure che usano Gen5_2 come SLO predefinito. |
| Interfaccia utente XEvent | Correzione di un problema di lunga data (introdotto in SSMS 18.0) a causa del quale "Salva in file XEL..." generava un errore. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37695592). |

#### <a name="known-issues-186"></a>Problemi noti (18.6)

| Nuovo elemento | Dettagli | Soluzione alternativa |
|----------|---------|------------|
| Analysis Services | Errore durante la connessione a SSAS tramite msmdpump.dll. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696). | N/D |
| SQL Server Management Studio (SSMS) - Generale | La finestra di dialogo Nuova specifica controllo server può causare l'arresto anomalo di SSMS con un errore di violazione di accesso. | N/D |
| SQL Server Management Studio (SSMS) - Generale | Le estensioni di SSMS che usano SMO devono essere ricompilate usando come destinazione il nuovo pacchetto SMO v161 specifico di SSMS. Una versione di anteprima è disponibile all'indirizzo https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects.SSMS/ </br></br> Le estensioni compilate con le versioni 160 precedenti del pacchetto Microsoft.SqlServer.SqlManagementObjects funzioneranno ancora. | N/D |
| Integration Services | Quando si importano o esportano pacchetti in Integration Services o si esportano pacchetti in Azure-SSIS Integration Runtime, gli script vanno persi per i pacchetti che contengono attività o componenti di script. Soluzione alternativa: Rimuovere la cartella "C:\Programmi (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild". | N/D|
| Integration Services | Le connessioni remote a Integration Services possono avere esito negativo e visualizzare il messaggio "Il servizio specificato non esiste come servizio installato" in un sistema operativo più recente. Soluzione alternativa: Identificare il percorso del registro di sistema correlato di Integration Services in Computer\HKEY_CLASSES_ROOT\AppID & Computer\HKEY_CLASSES_ROOT\ WOW6432Node\AppID e all'interno di questi hive rinominare la chiave del registro di sistema denominata 'LocalService' in 'LocalService_A' per la versione specifica di Integration Services che si sta tentando di connettere | N/D|

### <a name="1851"></a>18.5.1

![download](media/download-icon.png) [Scaricare SSMS 18.5.1](https://go.microsoft.com/fwlink/?linkid=2132606)

- Numero di versione: 18.5.1
- Numero di build: 15.0.18333.0
- Data di rilascio: 09 giugno 2020

[Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x40a)

#### <a name="bug-fixes-in-1851"></a>Correzioni di bug nella versione 18.5.1

| Nuovo elemento | Dettagli |
|----------|---------|
| Analysis Services | Miglioramento delle prestazioni quando si espande l'elenco dei database quando si è connessi ai server Azure Analysis Services o Power BI. |
| Analysis Services | Correzione di un problema a causa del quale si verificava un errore durante il tentativo di aprire la Sincronizzazione guidata database di un server Analysis Services. |
| Analysis Services | Correzione di un problema che impedisce agli utenti di eseguire query su SSAS 2017 e versioni precedenti con autorizzazioni per i dati delle celle. |
| SQL Server Management Studio (SSMS) - Generale | [Progettazione tabelle - Correzione del segnale acustico emesso quando si tenta di usare TAB in una griglia di Progettazione tabelle](https://feedback.azure.com/forums/908035/suggestions/40318435) |

#### <a name="known-issues-1851"></a>Problemi noti (18.5.1)

| Nuovo elemento | Dettagli | Soluzione alternativa |
|----------|---------|------------|
| SQL Server Management Studio (SSMS) - Generale | Esiste un bug noto con Struttura diagramma che causa il danneggiamento dei diagrammi esistenti. Ad esempio, quando si crea una Struttura diagramma con SSMS 17.9.1, quindi la si aggiorna/salva con SSMS 18.x e poi si prova ad aprirla con la versione 17.9.1. Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37992649). | N/D |
| SQL Server Management Studio (SSMS) - Generale | La finestra di dialogo Nuova specifica controllo server può causare l'arresto anomalo di SSMS con un errore di violazione di accesso. | N/D ||
| SMO/scripting | Le estensioni di SSMS che usano SMO devono essere ricompilate usando come destinazione il nuovo SMO v160. | N/D |
| Integration Services | Quando si importano o esportano pacchetti in Integration Services o si esportano pacchetti in Azure-SSIS Integration Runtime, gli script vanno persi per i pacchetti che contengono attività o componenti di script. Soluzione alternativa: | Rimuovere la cartella "C:\Programmi (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild". |

### <a name="185"></a>18.5

![download](media/download-icon.png) [Scaricare SSMS 18.5](https://go.microsoft.com/fwlink/?linkid=2125901)

- Numero di versione: 18.5
- Numero di build: 15.0.18330.0
- Data di rilascio: 7 aprile 2020

[Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x40a)

#### <a name="whats-new-in-185"></a>Novità della versione 18.5

| Nuovo elemento | Dettagli |
|----------|---------|
| Analysis Services | È stato aggiunto il supporto per l'endpoint Power BI in Analysis Services, con funzionalità corrispondenti a quelle di Azure Analysis Services. |
| Analysis Services | Profiler: è stato aggiunto il supporto per la definizione di traccia di Analysis Services 15.1. |
| Classificazione dei dati | È stato aggiunto un pulsante alla visualizzazione dei risultati dell'analisi VA, per consentire la correzione della regola di classificazione dei dati passando al riquadro classificazione dei dati. |
| Classificazione dei dati | È stato aggiunto il supporto per la classificazione della riservatezza nella classificazione dei dati. |
| Hyperscale | È stato aggiunto il supporto per *Importa applicazione livello dati* (con estensione bacpac) a SQL Azure HyperScale. |
| Integration Services | Supporto per l'esecuzione di un pacchetto SSIS da file system in un processo agente dell'istanza gestita. |
| Integration Services | Sono stati apportati miglioramenti intuitivi nella configurazione di DTExec con abilitazione per Azure per richiamare esecuzioni di pacchetti SSIS in Azure-SSIS Integration Runtime.
| Integration Services | Supporto della connessione di Azure-SSIS Integration Runtime e della gestione e dell'esecuzione di pacchetti SSIS negli archivi pacchetti.
| Integration Services | Supporto della migrazione di processi agente SSIS locale a pipeline e trigger ADF.
| Integration Services | Miglioramento all'esperienza utente per l'esportazione di progetti SSIS da un database SSIS. Rispetto al processo di esportazione precedente, che caricava e aggiornava i pacchetti nel progetto SSIS, il nuovo processo di esportazione indipendente dalla versione non carica e non aggiorna i pacchetti nel progetto SSIS ma mantiene i pacchetti all'interno dei progetti così come sono nel database SSIS, ad eccezione della modifica del livello di protezione in EncryptSensitiveWithUserKey. |
| SMO/scripting | È stata aggiunta la nuova proprietà DwMaterializedViewDistribution all'oggetto View. |
| SMO/scripting | È stato rimosso il supporto per la *restrizione delle funzionalità* (questa funzionalità di anteprima è stata rimossa da SQL Azure e da SQL locale). |
| SMO/scripting | Aggiunta di *Notebook* come destinazione per la procedura guidata Genera script. |
| SMO/scripting | Aggiunta del supporto per il *pool SQL serverless di Azure Synapse Analytics*. |
| SMO/scripting | [API Valutazione SQL](../tools/sql-assessment-api/sql-assessment-api-overview.md) - I campi Platform, Name ed engineEdition possono ora contenere normali elenchi delimitati da virgole (*platform*: \[*Windows*, *Linux*\]), non solo espressioni regolari (*platform*: *\/Windows\|Linux\/* )
| SMO/scripting | [API Valutazione SQL](../tools/sql-assessment-api/sql-assessment-api-overview.md) - Aggiunta di 13 regole di valutazione. Per altri dettagli, passare a [GitHub](https://github.com/microsoft/sql-server-samples/tree/master/samples/manage/sql-assessment-api)). |

#### <a name="bug-fixes-in-185"></a>Correzioni di bug nella versione 18.5

| Nuovo elemento | Dettagli |
|----------|---------|
| Accessibilità | SSIS ADF/Nuova pianificazione: è stato risolto un problema per cui l'ordine dello stato attivo non era logico nella modalità di analisi di Assistente vocale nella procedura guidata *Nuova pianificazione*. |
| Accessibilità | Procedura guidata Stretch Database: è stato risolto un problema per cui il lettore dello schermo non specificava il nome della tabella di query quando offriva informazioni sulla tabella. |
| Analysis Services | Correzione della connessione memorizzata nella cache quando si creano script in AS con una connessione Azure AD. |
| Always On | È stato risolto un problema a causa del quale l'aggiunta del primo database a un gruppo di disponibilità Always On non viene eseguita correttamente.
| Always On | È stato risolto un problema per cui veniva visualizzato un errore quando si tentava di visualizzare il dashboard durante la connessione a un endpoint di un cluster Big Data. |
| Controllo | È stato risolto un problema a causa del quale la finestra di unione dei log di controllo si arrestava in modo anomalo quando era presente una cartella con un nome vuoto nella cartella radice dell'account di archiviazione. |
| Controllo | È stato risolto un problema per cui la finestra di unione dei log di controllo non visualizzava tutti i server quando erano presenti troppi elementi nella radice del contenitore. |
| Classificazione dei dati | È stato risolto un problema per cui la procedura guidata *Classificazione dei dati* non si apre per i database con un numero elevato di tabelle. |
| Classificazione dei dati | Vengono ora applicati GUID diversi per ogni struttura label/infoType e di GUID nel processo di convalida. |
| Classificazione dei dati | Rimozione del processo di classificazione in SqlServer2019. |
| Classificazione dei dati | Correzione dei test di convalida precedenti (aggiunta della classificazione, rimozione della proprietà non valida *InformationTypes*) e aggiunta di nuovi test per i primi due punti. |
| Classificazione dei dati | Il pulsante immediatamente sopra la tabella delle colonne classificate riduce ora al minimo il pannello delle indicazioni, come afferma. |
| SQL Server Management Studio (SSMS) - Generale | Aggiornamento della versione dei driver MSODBC e MSOLEDB. |
| SQL Server Management Studio (SSMS) - Generale | Sono stati risolti i blocchi e gli arresti anomali di almeno due origini comuni in SSMS. |
| SQL Server Management Studio (SSMS) - Generale | È stato risolto un altro caso in cui la *finestra di dialogo Ripristina* si blocca quando si seleziona il pulsante Sfoglia. |
| SQL Server Management Studio (SSMS) - Generale | Correzione dell'*interfaccia utente grafica Nuovo database* per il pool SQL serverless di Azure Synapse Analytics. |
| SQL Server Management Studio (SSMS) - Generale | Correzione dei modelli *Nuova tabella esterna* e *Nuova origine dati esterna* per il pool SQL serverless di Azure Synapse Analytics. |
| SQL Server Management Studio (SSMS) - Generale | Correzione di proprietà del database, proprietà di connessione, report nascosti e ridenominazione per il pool SQL serverless di Azure Synapse Analytics. |
| SQL Server Management Studio (SSMS) - Generale | Always Encrypted: È stato risolto un problema per cui l'elenco a discesa del nome della chiave diventava di sola lettura quando si selezionava una nuova chiave abilitata per l'enclave. |
| SQL Server Management Studio (SSMS) - Generale | È stata pulita la griglia *Opzioni di Proprietà database*, in cui erano visualizzate due *categorie Varie*. |
| SQL Server Management Studio (SSMS) - Generale | È stato risolto un problema a causa del quale la barra di scorrimento iniziava dal centro nella griglia "Opzioni di Proprietà database". |
| SQL Server Management Studio (SSMS) - Generale | È stato risolto un problema che causava l'arresto anomalo di SSMS durante l'apertura di un file con estensione sql durante la connessione al server Analysis Services. |
| SQL Server Management Studio (SSMS) - Generale | Finestra di dialogo Connessione: è stato risolto un problema per cui la deselezione di Memorizza password non funziona. |
| SQL Server Management Studio (SSMS) - Generale | È stato risolto un problema per cui le credenziali associate a server/utenti vengono sempre memorizzate. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37875172). |
| SQL Server Management Studio (SSMS) - Generale | È stato risolto un problema per cui occasionalmente le finestre dell'editor non venivano aggiornate correttamente. Questo risultato viene ottenuto disabilitando l'accelerazione hardware in *Strumenti > Opzioni > Ambiente*. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37474042). |
| SQL Server Management Studio (SSMS) - Generale | È stato risolto un problema per cui l'autenticazione di Azure Active Directory non funzionava attraverso un proxy. |
| Valori DPI alti/Ridimensionamento | È stato risolto un problema per cui il rendering dei controlli nelle *proprietà indice* veniva eseguito in modo non corretto (pulsanti sovrapposti alla griglia). Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/36030424). |
| Valori DPI alti/Ridimensionamento | Sono stati risolti più problemi nella finestra di dialogo *Proprietà database*, che poteva visualizzare controlli tagliati sui monitor 4K. |
| Valori DPI alti/Ridimensionamento | Sono state corrette le procedure guidate di pubblicazione e sottoscrizione per i monitor 4K. |
| Valori DPI alti/Ridimensionamento | È stata eseguita una correzione secondaria nella pagina Nuova specifica controllo server. |
| Valori DPI alti/Ridimensionamento | È stato risolto un problema di visualizzazione 4K nella procedura guidata Disponibilità elevata. |
| Valori DPI alti/Ridimensionamento | È stato risolto un problema per cui l'utente non era in grado di aggiungere una destinazione in una finestra Nuova sessione di XEvent né di impostare filtri degli eventi di sessione nella procedura guidata per una sessione di XEvent quando la visualizzazione era ridimensionata al 125%. |
| Valori DPI alti/Ridimensionamento | È il corso la risoluzione di un problema per cui il rendering dei controlli nell'interfaccia utente di *backup del database in un URL* viene eseguito in modo non visibile con un ridimensionamento superiore al 100%. |
|Importa file flat | È stata aggiornata l'Importazione guidata file flat per consentire di selezionare tutto per la colonna Consenti valore Null. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/38027137). |
| Esplora oggetti | È stato risolto un problema per cui Esplora oggetti poteva visualizzare informazioni non corrette quando si usavano stringhe di connessione nella finestra di dialogo Connessione. |
| Esplora oggetti | È stato risolto un problema a causa del quale Esplora oggetti eseguiva lentamente l'espansione delle tabelle per i database con diverse migliaia di tabelle (oltre 20.000). |
| Interfaccia utente di Query Store | È stato corretto il conteggio del calcolo delle esecuzioni del report TRC (per la metrica *tempo di attesa*) come somma dei conteggi di esecuzione per ogni singola categoria di attesa, che non è corretto. Per una singola esecuzione di query, tuttavia, viene eseguita la registrazione per ogni categoria di attesa della query. Se quindi il report TRC esegue la somma attraverso la categoria di attesa, il conteggio delle esecuzioni aumenta eccessivamente. Il conteggio deve in realtà corrispondere al valore massimo di wait_category. |
| Interfaccia utente di Query Store | È stato risolto il problema per il quale la visualizzazione dettagliata TRC restituiva dati non corretti quando il set di risultati veniva filtrato in base ai primi x. Questo avveniva perché la query usa più espressioni di tabella comune, che vengono quindi unite insieme per creare il set di risultati finale. Se viene eseguito il push dei primi x nell'espressione di tabella comune, è talvolta possibile che le righe richieste vengano escluse. Questo può talvolta rendere non deterministico il set di risultati. La correzione consiste nel non eseguire il push dei primi x nelle espressioni di tabella comune. |
| Interfaccia utente di Query Store | È stato corretto il riepilogo del piano nella vista griglia o grafico. È necessario il tempo di attesa dell'ultima esecuzione della query. L'assenza di questa colonna compromette il funzionamento della query. Questo insieme di modifiche aggiunge tale colonna all'espressione di tabella comune delle statistiche di attesa. |
| Showplan | È stato migliorato il modo in cui SSMS visualizza i conteggi delle righe stimati per gli operatori con più esecuzioni: (1) *Numero stimato di righe* in SSMS è stato modificato in "Numero stimato di righe per esecuzione". (2) È stata aggiunta la nuova proprietà *Numero stimato di righe per Tutte le esecuzioni*. (3) È stata modificata la proprietà *Numero effettivo di righe* in *Numero effettivo di righe per Tutte le esecuzioni*. |
| SQL Agent | È stato risolto il problema per cui il tentativo di modificare un passaggio di un processo SQL Agent poteva causare il blocco dell'interfaccia utente di SSMS. SSMS consente ora di visualizzare (pulsante *Visualizza*) un file di output il cui nome è in formato token (almeno per le macro o i token semplici supportati da SQL Agent che non sono determinati in fase di runtime). SSMS, poi, non disabilita il pulsante "Visualizza" quando l'utente non ha accesso al file (in base alle autorizzazioni SQL). Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/39063124). |
| SQL Agent | È stato corretto l'ordine di tabulazione nella pagina Passaggio processo. |
| SQL Agent | È stata invertita la posizione dei pulsanti "Avanti" e "Indietro" nella pagina Passaggio processo perché abbiano un ordine logico. |
| SQL Agent | È stata modificata la finestra Pianificazione processo in modo che non tagli l'interfaccia utente. |
| SMO/scripting | Correzione della creazione di script del database per il pool SQL serverless di Azure Synapse Analytics. |
| SMO/scripting | Rimozione del cast sqlvariant esplicito (sintassi T-SQL non valida per SqlOnDemand). Questo corregge la creazione di script per SqlOnDemand. |
| SMO/scripting | È stato risolto un problema per cui veniva ignorata l'operazione FILLFACTOR sugli indici per SQL Azure. |
| SMO/scripting | È stato risolto un problema relativo alla creazione di script di oggetti esterni. |
| SMO/scripting | È stato risolto un problema per cui *Genera script* non consentiva di scegliere l'opzione relativa agli script per le proprietà estese per il database SQL. È stata anche corretta la creazione di script per tali proprietà estese. |
| SMO/scripting | [API Valutazione SQL](../tools/sql-assessment-api/sql-assessment-api-overview.md) - Collegamento alla Guida non corretto nella regola XTPHashAvgChainBuckets. |
| Interfaccia utente XEvent | È stato corretto un problema per cui gli elementi nella griglia venivano selezionati al passaggio del mouse. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/38262124) e questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/37873921). |

#### <a name="known-issues-185"></a>Problemi noti (18.5)

| Nuovo elemento | Dettagli | Soluzione alternativa |
|----------|---------|------------|

- Il diagramma di database creato da SSMS in esecuzione nel computer A non può essere modificato dal computer B (si verifica un arresto anomalo di SSMS). Per altri dettagli, vedere la [segnalazione di un utente 37992649 per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37992649).

- Quando si importano o esportano pacchetti in Integration Services o si esportano pacchetti in Azure-SSIS Integration Runtime, gli script vanno persi per i pacchetti che contengono attività o componenti di script. Una soluzione alternativa consiste nel rimuovere la cartella *C:\Programmi (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild*.

- La finestra di dialogo Nuova specifica controllo server può causare l'arresto anomalo di SSMS con un errore di violazione di accesso.

- È necessario ricompilare le estensioni di SSMS che usano SMO usando come destinazione il nuovo pacchetto SMO versione 160 (che sarà disponibile in nuget.org subito dopo il rilascio di SSMS 18.5).

- [Errore durante la connessione a SSAS tramite msmdpump.dll in SSMS](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696-error-when-connecting-to-ssas-via-msmdpump-dll-in).

### <a name="184"></a>18.4

![download](media/download-icon.png) [Scaricare SSMS 18.4](https://go.microsoft.com/fwlink/?linkid=2108895)

- Numero di versione: 18.4
- Numero di build: 15.0.18206.0
- Data di rilascio: 4 novembre 2019

[Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x40a)

| Nuovo elemento | Dettagli |
|----------|---------|
| Classificazione dei dati | Aggiunta del supporto dei criteri di Information Protection personalizzati per la classificazione dei dati. |
| Archivio query | Aggiunta del valore *Numero massimo di piani per query* nelle proprietà della finestra di dialogo. |
| Archivio query | Aggiunta del supporto per i nuovi criteri di acquisizione personalizzati. |
| Archivio query | Aggiunta di **Modalità di acquisizione delle statistiche di attesa** alle opzioni **Proprietà database** di **Query Store**. |
| SMO/scripting | Supporto dello script della vista materializzata in SQL DW. |
| SMO/scripting | Aggiunta del supporto per il *pool SQL serverless di Azure Synapse Analytics*. |
| SMO/scripting | [API Valutazione SQL](../tools/sql-assessment-api/sql-assessment-api-overview.md) - Aggiunta di 50 regole di valutazione (vedere i dettagli su GitHub). |
| SMO/scripting | [API Valutazione SQL](../tools/sql-assessment-api/sql-assessment-api-overview.md) - Aggiunta di espressioni matematiche di base e confronti alle condizioni delle regole. |
| SMO/scripting | [API Valutazione SQL](../tools/sql-assessment-api/sql-assessment-api-overview.md) - Aggiunta del supporto per l'oggetto RegisteredServer. |
| SMO/scripting | [API Valutazione SQL](../tools/sql-assessment-api/sql-assessment-api-overview.md) - Aggiornamento della modalità di archiviazione delle regole nel formato JSON e aggiornamento del meccanismo di applicazione delle sostituzioni/personalizzazioni. |
| SMO/scripting | [API Valutazione SQL](../tools/sql-assessment-api/sql-assessment-api-overview.md) - Aggiornamento delle regole per supportare SQL in Linux. |
| SMO/scripting | [API Valutazione SQL](../tools/sql-assessment-api/sql-assessment-api-overview.md) - Aggiornamento del formato JSON di RuleSet è aggiunta della versione SCHEMA. |
| SMO/scripting | [API Valutazione SQL](../tools/sql-assessment-api/sql-assessment-api-overview.md) - Aggiornamento dell'output dei cmdlet per migliorare la leggibilità delle raccomandazioni. |
| Profiler XEvent | Aggiunta dell'evento *error_reported* alle sessioni del profiler XEvent. |

#### <a name="bug-fixes-in-184"></a>Correzioni di bug nella versione 18.4

| Nuovo elemento | Dettagli |
|----------|---------|
| Analysis Services | È stato risolto un problema per cui l'editor di script DAX per i database multidimensionali non mostrava tabelle in IntelliSense. |
| Analysis Services | Usare il parser DAX per eseguire la conversione in una stringa del motore. Riguarda separatori internazionali, decimali e spazi vuoti. |
| Always Encrypted | È stato risolto un problema a causa del quale la *convalida delle attestazioni* non faceva *distinzione tra maiuscole e minuscole*. |
| Always Encrypted | È stato risolto un problema a causa del quale la segnalazione di errori/avvisi non funzionava correttamente. |
| Copia guidata database | Sono stati risolti vari problemi di troncamento e layout nel rendering di questa finestra di dialogo. |
| SQL Server Management Studio (SSMS) - Generale | È stato risolto un problema esistente da tempo a causa del quale SSMS non rispettava le informazioni di connessione passate dalla riga di comando quando venivano specificati anche file SQL. |
| SQL Server Management Studio (SSMS) - Generale | Correzione di un problema di arresto anomalo in SSMS durante il tentativo di visualizzare entità a protezione diretta negli oggetti "Filtro di replica". |
| SQL Server Management Studio (SSMS) - Generale | È stato risolto il problema della rimozione dell'opzione della riga di comando -P facendo in modo che SSMS esamini la cache delle credenziali: se vengono trovate le credenziali necessarie, vengono usate per stabilire la connessione. |
| Importa file flat | È stato risolto un problema per cui la funzionalità *Importa file flat* non gestiva correttamente i qualificatori di testo. |
| Esplora oggetti | È stato risolto un problema per cui l'eliminazione di un database SQL di Azure in Esplora oggetti mostrava un messaggio non corretto. |
| Risultati delle query | È stato risolto un problema introdotto in SSMS 18.3.1 a causa del quale le griglie vengono disegnate strette e con *...* alla fine della stringa più lunga di ogni colonna. |
| Strumenti di replica | È stato risolto un problema che causava la generazione di un errore dell'applicazione ("Impossibile caricare il file o l'assembly...") quando si tentava di modificare i processi di SQL Agent. |
| SMO/scripting | È stato risolto un problema per cui *Crea script per tabella…* per SQL DW con regole di confronto Japanese_BIN2 non funzionava.|
| SMO/scripting | È stato risolto un problema per cui ScriptAlter () terminava l'esecuzione delle istruzioni nel server.|
| SQL Agent | È stato risolto un problema a causa del quale l'interfaccia utente dell'operatore agente non aggiornava il nome dell'operatore quando veniva modificato nell'interfaccia utente, né veniva creato uno script. Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/32897647).|

#### <a name="known-issues-184"></a>Problemi noti (18.4)

- Il diagramma di database creato da SSMS in esecuzione nel computer A non può essere modificato dal computer B (si verifica un arresto anomalo di SSMS). Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37992649).

- Esistono problemi di aggiornamento durante il passaggio tra finestre di query diverse. Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37474042). Una soluzione alternativa per questo problema è disabilitare l'accelerazione hardware in *Strumenti > Opzioni*.

È possibile fare riferimento al [sito dei commenti e suggerimenti degli utenti per SQL Server](https://feedback.azure.com/forums/908035-sql-server) per informazioni su altri problemi noti e per inviare commenti e suggerimenti al team del prodotto.

### <a name="1831"></a>18.3.1

![download](media/download-icon.png) [Scaricare SSMS 18.3.1](https://go.microsoft.com/fwlink/?linkid=2105412)

- Numero di versione: 18.3.1
- Numero di build: 15.0.18183.0
- Data di rilascio: 2 ottobre 2019

[Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x40a)

#### <a name="whats-new-in-1831"></a>Novità della versione 18.3.1

| Nuovo elemento | Dettagli |
|----------|---------|
| Classificazione dei dati | Aggiungere le informazioni di classificazione dei dati all'interfaccia utente delle proprietà della colonna (le opzioni *Tipo di informazioni*, *ID tipo di informazioni*, *Etichetta riservatezza* e *ID etichetta di riservatezza* non sono esposte nell'interfaccia utente di SSMS). |
| IntelliSense/editor | Supporto aggiornato per le funzionalità aggiunte di recente a SQL Server 2019 (ad esempio, *ALTER SERVER CONFIGURATION*). |
| Integration Services | Aggiunta di una nuova voce del menu di selezione `Tools > Migrate to Azure > Configure Azure-enabled DTExec` per richiamare le esecuzioni del pacchetto SSIS in Azure-SSIS Integration Runtime come attività di esecuzione del pacchetto SSIS nelle pipeline ADF. |
| SMO/scripting | Aggiunta del supporto dello scripting del vincolo UNIQUE di Azure SQL Data Warehouse. |
| SMO/scripting | Classificazione dei dati </br> - Aggiunta del supporto per SQL versione 10 (SQL 2008) e versioni successive. </br> - Aggiunta del nuovo attributo di riservatezza 'rank' per SQL versione 15 (SQL 2019) e versioni successive e il database SQL di Azure. |
| SMO/scripting | [API Valutazione SQL](../tools/sql-assessment-api/sql-assessment-api-overview.md) - Aggiunta del controllo delle versioni al formato del set di regole. |
| SMO/scripting | [API Valutazione SQL](../tools/sql-assessment-api/sql-assessment-api-overview.md) - Aggiunta di nuovi controlli. |
| SMO/scripting | [API Valutazione SQL](../tools/sql-assessment-api/sql-assessment-api-overview.md) - Aggiunta del supporto per Istanza gestita di SQL di Azure. |
| SMO/scripting | [API Valutazione SQL](../tools/sql-assessment-api/sql-assessment-api-overview.md) - Aggiornamento della visualizzazione predefinita dei cmdlet per visualizzare i risultati come tabella. |

#### <a name="bug-fixes-in-1831"></a>Correzioni di bug nella versione 18.3.1

| Nuovo elemento | Dettagli |
|----------|---------|
| Analysis Services | È stato risolto un problema di ridimensionamento nell'editor di query MDX.|
| Analysis Services | È stato risolto un problema nell'interfaccia utente di XEvent che impedisce agli utenti di creare una nuova sessione. |
| Distribuzione di database in SQL Azure | Correzione di un problema (in DacFx) che causava la non disponibilità di questa funzionalità.|
| SQL Server Management Studio (SSMS) - Generale | È stato risolto un problema che causava l'arresto anomalo di SSMS quando si usa la funzionalità di ordinamento nel visualizzatore XEvent. |
| SQL Server Management Studio (SSMS) - Generale | Sono stati risolti problemi in sospeso da tempo a causa dei quali il ripristino di database in SSMS può smettere di rispondere per tempo illimitato. </br></br> Per altri dettagli, vedere queste segnalazioni degli utenti per SQL Server: </br> [Restore Database - Select Backup Devices Slow to Load](https://feedback.azure.com/forums/908035/suggestions/32899099/) (Ripristina database - Caricamento lento di Seleziona dispositivi di backup).  </br> [SSMS 2016 very slow in the database restore dialogs](https://feedback.azure.com/forums/908035/suggestions/32900767/) (SSMS 2016 è molto lento nelle finestre di dialogo per il ripristino di database). </br> [Restoring database is slow](https://feedback.azure.com/forums/908035/suggestions/32900224/) (Lentezza del ripristino di database).  </br> [Restore Database from Device HANGS on clicking "..."](https://feedback.azure.com/forums/908035/suggestions/34281658/) (Il ripristino di database dal dispositivo si BLOCCA dopo il clic su "...").  |
| SQL Server Management Studio (SSMS) - Generale | È stato risolto un problema a causa del quale veniva visualizzato l'arabo come lingua predefinita per tutti gli account di accesso. </br></br> Per altri dettagli, vedere questa segnalazione di un utente per SQL Server: [SSMS 18.2 default language display bug](https://feedback.azure.com/forums/908035/suggestions/38236363) (Bug di visualizzazione della lingua predefinita in SSMS 18.2). |
| SQL Server Management Studio (SSMS) - Generale | Correzione della finestra di dialogo difficile da visualizzare per *Opzioni query* (quando l'utente fa clic con il pulsante destro del mouse sulla finestra dell'editor T-SQL) rendendola ridimensionabile.|
| SQL Server Management Studio (SSMS) - Generale | Il messaggio *Ora di completamento* visibile nella griglia/file dei risultati (introdotto in SSMS 18.2), è ora configurabile in Strumenti > Opzioni > Esecuzione query > SQL Server > Avanzate > Mostra ora di completamento. |
| SQL Server Management Studio (SSMS) - Generale | Nella finestra di dialogo di connessione, le opzioni *Active Directory - Password* e *Active Directory - Integrata* sono state sostituite rispettivamente con *Azure Active Directory - Password* e *Azure Active Directory - Integrata*. |
| SQL Server Management Studio (SSMS) - Generale | Correzione di un problema che impedisce agli utenti di usare SSMS per configurare il controllo sulle istanze gestite di SQL Azure quando si trovano in un fuso orario con offset UTC negativo. |
| SQL Server Management Studio (SSMS) - Generale | È stato un risolto un problema nell'interfaccia utente di XEvent a causa del quale al passaggio del mouse sulla griglia vengono selezionate righe. </br></br> Per altri dettagli, vedere questa segnalazione di un utente per SQL Server: [SSMS Extended Events UI Selects Actions on Hover](https://feedback.azure.com/forums/908035/suggestions/38262124) (L'interfaccia utente degli eventi estesi di SSMS seleziona azioni al passaggio del mouse). |
| Importa file flat | È stato risolto un problema a causa del quale Importa file flat non importava tutti i dati consentendo all'utente di scegliere tra un rilevamento dei tipi di dati semplice o formattati.</br></br> Per altri dettagli, vedere questa segnalazione di un utente per SQL Server: [SSMS Import Flat File fails to import all data](https://feedback.azure.com/forums/908035/suggestions/38096989) (Importa file flat di SSMS non importa tutti i dati). |
| Integration Services | Aggiunta del nuovo tipo di operazione *StartNonCatalogExecution* per il report dell'operazione SSIS.|
| Integration Services | È stato risolto un problema nelle pipeline di Azure Data Factory generate dall'utilità `DTExec` abilitata per Azure per usare il tipo di parametro corretto. (esplicito per 18.3.1) |
| SMO/scripting | È stato risolto un problema che causava la generazione di errori da SMO durante il recupero delle proprietà quando si usava **SMO.Server.SetDefaultInitFields(true)** .|
| Interfaccia utente di Query Store | È stato risolto un problema a causa del quale l'asse Y non veniva ridimensionato con la selezione della metrica *Conteggio esecuzioni* nella visualizzazione *Query rilevate*. |
| Valutazione della vulnerabilità | Cancellazione e approvazione della baseline per database SQL di Azure disabilitata.|

#### <a name="known-issues-1831"></a>Problemi noti (18.3.1)

- Il diagramma di database creato da SSMS in esecuzione nel computer A non può essere modificato dal computer B (si verifica un arresto anomalo di SSMS). Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37992649).

- Esistono problemi di aggiornamento durante il passaggio tra finestre di query diverse. Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37474042). Una soluzione alternativa per questo problema è disabilitare l'accelerazione hardware in Strumenti > Opzioni.

È possibile fare riferimento al [sito dei commenti e suggerimenti degli utenti per SQL Server](https://feedback.azure.com/forums/908035-sql-server) per informazioni su altri problemi noti e per inviare commenti e suggerimenti al team del prodotto.

### <a name="182"></a>18.2

![download](media/download-icon.png) [Scaricare SSMS 18.2](https://go.microsoft.com/fwlink/?linkid=2099720)

- Numero di versione: 18.2
- Numero di build: 15.0.18142.0
- Data di rilascio: 25 luglio 2019

[Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x40a)

#### <a name="whats-new-in-182"></a>Novità della versione 18.2

| Nuovo elemento | Dettagli |
|----------|---------|
| Integration Services (SSIS) | Ottimizzazione delle prestazioni per l'utilità di pianificazione pacchetti SSIS in Azure. |
| IntelliSense/editor | Aggiunta del supporto per la classificazione dei dati. |
| OPTIMIZE_FOR_SEQUENTIAL_KEY | Aggiunta del supporto di IntelliSense. |
| OPTIMIZE_FOR_SEQUENTIAL_KEY | Attiva un'ottimizzazione all'interno del motore di database che contribuisce a migliorare la velocità effettiva per gli inserimenti nell'indice con un elevato grado di concorrenza. Questa opzione è destinata agli indici che sono soggetti a conflitti di inserimento dell'ultima pagina, che generalmente si verificano con gli indici con una chiave sequenziale, come una colonna Identity, una sequenza o una colonna di data/ora. Per altri dettagli, vedere [CREATE INDEX](../t-sql/statements/create-index-transact-sql.md#sequential-keys). |
| Esecuzione o risultati di query | Aggiunta di *Ora di completamento* nei messaggi per tenere traccia di quando viene completata l'esecuzione di una determinata query. |
| Esecuzione o risultati di query | È consentita la visualizzazione (risultato come testo) e l'archiviazione nelle celle (risultato come griglia) di più dati. SSMS consente ora fino a 2 M di caratteri per entrambi (rispettivamente, fino a 256 K e 64 K). In questo modo è stato risolto anche il problema a causa del quale gli utenti non erano in grado di acquisire più di 43680 caratteri dalle celle della griglia. |
| Showplan | Aggiunta di un nuovo attributo in QueryPlan quando è abilitata la [funzionalità UDF scalare inline](../relational-databases/performance/intelligent-query-processing.md#scalar-udf-inlining) (ContainsInlineScalarTsqludfs). |
| SMO | Aggiunta del supporto per l'*API Valutazione SQL*. Per altre informazioni, vedere [API Valutazione SQL](../tools/sql-assessment-api/sql-assessment-api-overview.md). |
|  |  |

#### <a name="bug-fixes-in-182"></a>Correzioni di bug nella versione 18.2

| Nuovo elemento | Dettagli |
|------------|-------|
| Accessibilità | Aggiornamento dell'interfaccia utente XEvent (la griglia) in modo che sia ordinabile premendo F3. |
| Always On | È stato risolto un problema a causa del quale SSMS generava un errore durante il tentativo di eliminare un gruppo di disponibilità |
| Always On | È stato risolto un problema a causa del quale SSMS presenta la procedura guidata di failover errata, con repliche configurate come sincrone, quando si usano gruppi di disponibilità con scalabilità in lettura (tipo cluster=NONE). SSMS presenta ora la procedura guidata per l'opzione Force_Failover_Allow_Data_Loss, che è l'unica consentita per la disponibilità con tipo di cluster NONE |
| Always On | È stato risolto un problema a causa del quale la procedura guidata limitava il numero di sincronizzazioni consentite a tre |
| Classificazione dei dati | È stato risolto un problema a causa del quale SSMS generava l'errore *L'indice deve essere maggiore o uguale a zero* durante il tentativo di visualizzare i report di classificazione dei dati nei database con livello di compatibilità minore di 150. |
| SQL Server Management Studio (SSMS) - Generale | È stato risolto un problema a causa del quale l'utente non era in grado di scorrere orizzontalmente il riquadro dei risultati tramite la rotellina del mouse. Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/34145641). |
| SQL Server Management Studio (SSMS) - Generale | Aggiornamento di *Monitoraggio attività* per ignorare i tipi di attesa benigni SQLTRACE_WAIT_ENTRIES |
| SQL Server Management Studio (SSMS) - Generale | È stato risolto un problema a causa del quale alcune opzioni relative ai colori *(Editor di testo > Scheda editor e barra di stato)* non venivano mantenute. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37924165)
| SQL Server Management Studio (SSMS) - Generale | Nella finestra di dialogo di connessione l'opzione *Active Directory - Universale con supporto MFA* è stata sostituita con *Azure Active Directory - Universale con supporto MFA* (la funzionalità è la stessa, ma si auspica generi meno confusione). |
| SQL Server Management Studio (SSMS) - Generale | Aggiornamento di SSMS per l'uso dei valori predefiniti corretti durante la creazione di un database SQL di Azure. |
| SQL Server Management Studio (SSMS) - Generale | È stato risolto un problema a causa del quale l'utente non era in grado di *avviare PowerShell* da un nodo in [Registra server](register-servers/register-servers.md) quando il server è un [contenitore SQL Linux](../linux/quickstart-install-connect-docker.md). |
| Importare file flat | È stato risolto un problema a causa del quale *Importa file flat* non funzionava dopo l'aggiornamento da SSMS 18.0 a 18.1. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37912636) |
| Importare file flat | È stato risolto un problema a causa del quale la *procedura guidata Importa file flat segnalava una colonna duplicata o non valida* in un file con estensione CSV con intestazioni con caratteri Unicode. |
| Esplora oggetti | È stato risolto un problema a causa del quale alcune voci di menu (ad esempio, *Importazione/esportazione guidata* SQL Server) risultavano mancanti o disabilitate quando connessi a SQL Express. Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37500016). |
| Esplora oggetti | È stato risolto un problema che causava l'arresto anomalo di SSMS quando un oggetto veniva trascinato da Esplora oggetti all'editor. Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37887988). |
| Esplora oggetti | È stato risolto un problema a causa del quale la ridenominazione dei database comportava la visualizzazione di nomi di database non corretti in Esplora oggetti. Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37638472). |
| Esplora oggetti | È stato risolto un problema di lunga data a causa del quale viene generato un errore quando si tenta di espandere il nodo *Tabelle* in Esplora oggetti per un database impostato per l'uso di regole di confronto non più supportate da Windows (e l'utente non può espandere le tabelle). Un esempio di tali regole di confronto è Korean_Wansung_Unicode_CI_AS. |
| [Registrazione di server](register-servers/register-servers.md) | È stato risolto un problema a causa del quale il tentativo di eseguire una query su più server (in un *gruppo* di server registrati) quando il server registrato usa *Active Directory - Integrata* o *Active Directory - Universale con supporto MFA* non funziona perché SSMS non riesce a connettersi. |
| [Registrazione di server](register-servers/register-servers.md) | È stato risolto un problema a causa del quale il tentativo di eseguire una query su più server (in un *gruppo* di server registrati) quando il server registrato usa *Active Directory - Password* o *Autenticazione SQL* e l'utente sceglie di non memorizzare la password causa l'arresto anomalo di SSMS. |
| Report | È stato risolto un problema nei report di *utilizzo del disco* che causava un errore del report quando i file di dati includevano un numero elevato di extent. |
| Strumenti di replica | È stato risolto un problema a causa del quale Monitoraggio replica non funzionava con il database di pubblicazione nel gruppo di disponibilità e con il server di distribuzione nel gruppo di disponibilità (risolto in precedenza in SSMS 17.x) |
| SQL Agent | È stato risolto un problema a causa del quale con l'aggiunta, l'inserimento, la modifica o la rimozione dei passaggi di processo lo stato attivo veniva reimpostato sulla prima riga invece della riga attiva. Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/38070892). |
| SMO/scripting | È stato risolto un problema a causa del quale *CREATE OR ALTER* non creava script per oggetti con proprietà estese. Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/37236748). |
| SMO/scripting | È stato risolto un problema a causa del quale SSMS non era in grado di inserire correttamente in uno script CREATE EXTERNAL LIBRARY. Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37868089). |
| SMO/scripting | È stato risolto un problema a causa del quale quando si tentava di *generare script* per un database con poche migliaia di tabelle la finestra di dialogo di stato sembrava bloccata. |
| SMO/scripting | È stato risolto un problema a causa del quale la creazione di script per la *tabella esterna* in SQL 2019 non funzionava. |
| SMO/scripting | È stato risolto un problema a causa del quale la creazione di script per l'*origine dati esterna* in SQL 2019 non funzionava. Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/34295080). |
| SMO/scripting | È stato risolto un problema a causa del quale le *proprietà estese* per le colonne non venivano incluse nello script quando la destinazione era il database SQL di Azure. Per altri dettagli, vedere [stackoverflow](https://stackoverflow.com/questions/56952337/how-can-i-script-the-descriptions-of-columns-in-ms-sql-server-management-studio). |
| SMO/scripting | Inserimento ultima pagina: SMO - Aggiunta della proprietà *Index.IsOptimizedForSequentialKey* |
|**Installazione di SSMS**| **È stato risolto un problema a causa del quale il programma di installazione di SSMS bloccava erroneamente l'installazione di lingue non corrispondenti per i report SSMS. Questo poteva essere un problema in alcune situazioni anomale, ad esempio un'installazione interrotta o una disinstallazione non corretta di una versione precedente di SSMS. Per altri dettagli, vedere [questa segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37483594/).** |
| Profiler XEvent | Correzione di un arresto anomalo durante la chiusura del visualizzatore. |

#### <a name="known-issues-182"></a>Problemi noti (18.2)

- Il diagramma di database creato in un'istanza di SSMS in esecuzione nel computer A non può essere modificato dal computer B (causando un arresto anomalo di SSMS). Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37992649).

- Esistono problemi di aggiornamento durante il passaggio tra finestre di query diverse. Vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37474042). Una soluzione alternativa per questo problema è disabilitare l'accelerazione hardware in **Strumenti** > **Opzioni**.

- Esiste una limitazione alle dimensioni dei dati visualizzati dai risultati di SSMS in griglia, testo o file.

- Si verifica un problema e si riceve un errore durante l'eliminazione di un database SQL di Azure in Esplora oggetti, mentre in realtà l'operazione viene eseguita correttamente.

- Come lingua predefinita per gli account di accesso SQL può essere visualizzato l'Arabo nella finestra di dialogo Proprietà account di accesso, indipendentemente dalla lingua predefinita effettiva impostata per l'account di accesso. Per visualizzare la lingua predefinita effettiva per un determinato account di accesso, usare T-SQL per selezionare la voce **default_language_name** dell'account di accesso da **master.sys.server_principles**.

### <a name="181"></a>18.1

![download](media/download-icon.png) [Scaricare SSMS 18.1](https://go.microsoft.com/fwlink/?linkid=2094583)

- Numero di versione: 18.1
- Numero di build: 15.0.18131.0
- Data di rilascio: 11 giugno 2019

[Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x40a)

#### <a name="whats-new-in-181"></a>Novità della versione 18.1

| Nuovo elemento | Dettagli |
| :-------| :------|
| Diagrammi di database | [I diagrammi di database sono stati nuovamente aggiunti a SSMS](https://feedback.azure.com/forums/908035/suggestions/37507828).
| SSBDIAGNOSE.EXE |Lo strumento da riga di comando di diagnostica di SQL Server è stato nuovamente aggiunto al pacchetto di SSMS.|
| Integration Services (SSIS) | Supporto per la pianificazione del pacchetto SSIS, che si trova nel catalogo SSIS in Azure o nel file system, in Azure. Esistono tre voci di menu per l'avvio della finestra di dialogo Nuova pianificazione: *Nuova pianificazione* che viene visualizzata quando si fa clic con il pulsante destro del mouse sul pacchetto SSIS nel catalogo SSIS in Azure, *Schedule SSIS Package in Azure* (Pianifica pacchetto SSIS in Azure) in *Esegui migrazione ad Azure* nel menu *Strumenti* e "Schedule SSIS in Azure" (Pianifica SSIS in Azure) che viene visualizzata quando si fa clic con il pulsante destro del mouse sulla cartella Processi in SQL Server Agent di Istanza gestita di SQL di Azure.|

#### <a name="bug-fixes-in-181"></a>Correzioni di bug nella versione 18.1

| Nuovo elemento | Dettagli |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Accessibilità | Migliorata l'accessibilità dell'interfaccia utente del processo di Agent. |'
| Accessibilità | Migliorata l'accessibilità nella pagina di monitoraggio di Stretch grazie all'aggiunta del nome accessibile per il pulsante *Aggiornamento automatico* e anche all'aggiunta di un nome accessibile intelligente che consentirà agli utenti di sapere su quale pulsante si trovano e quale sarà l'effetto se viene selezionato. |
| Integrazione ADS| Correzione di un possibile arresto anomalo di SSMS nel tentativo di usare i server registrati ADS.|
| Progettazione database | Aggiunto il supporto per le regole di confronto Latin1_General_100_BIN2_UTF8 (disponibile in SQL Server 2019 CTP3.0) |
| Classificazione dati | Impedisce l'aggiunta di etichette di riservatezza alle colonne nella tabella di cronologia, che non è supportata. |
| Classificazione dati | Correzione del problema relativo alla gestione non corretta del livello di compatibilità (server/database). |
| Progettazione database | Aggiunto il supporto per le regole di confronto Latin1_General_100_BIN2_UTF8 (disponibile in SQL2019 CTP3.0). |
| SQL Server Management Studio (SSMS) - Generale | Correzione di un problema per cui lo scripting di pseudocolonne in un indice non era corretto. |
| SQL Server Management Studio (SSMS) - Generale | Correzione di un problema nella pagina delle proprietà Account di accesso in cui la selezione del pulsante *Aggiungi credenziali* poteva generare un'eccezione di riferimento Null. |
| SQL Server Management Studio (SSMS) - Generale | Correzione della visualizzazione delle dimensioni della colonna Money nella pagina delle proprietà Indice. |
| SQL Server Management Studio (SSMS) - Generale | Correzione di un problema per cui SSMS non rispettava le impostazioni di Intellisense di *Strumenti/Opzioni* nella finestra dell'editor di SQL. |
| SQL Server Management Studio (SSMS) - Generale | Correzione di un problema a causa del quale SSMS non rispettava le impostazioni di IntelliSense di Guida (online/offline). |
| Valori DPI alti | Correzione del layout dei controlli nelle finestre di dialogo di errore per le opzioni di query non supportate. |
| Valori DPI alti | Correzione del layout dei controlli nella pagina *Nuovo gruppo di disponibilità* in alcune versioni localizzate di SSMS. |
| Valori DPI alti | Correzione del layout della pagina *Nuova pianificazione processo*. Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37632094). |
| Importa file flat | Correzione di un problema a causa del quale durante l'importazione era possibile che le righe venissero automaticamente perse.|
| Intellisense/editor | Ridotto il traffico delle query basato su SMO verso il database SQL di Azure per IntelliSense. |
| Intellisense/editor | Correzione dell'errore grammaticale nella descrizione comando visualizzata durante la digitazione di T-SQL per creare un utente. È stato anche corretto il messaggio di errore per evitare ambiguità tra utenti e account di accesso. |
| Visualizzatore log | Correzione di un problema a causa del quale SSMS apriva sempre il log (o l'agente) del server corrente anche in caso di doppio clic sul segno di un archivio precedente in Esplora oggetti. Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37633648). |
| Installazione di SSMS | Correzione del problema che impediva l'installazione di SSMS quando il percorso del log del programma di installazione conteneva spazi. Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37496110). |
| Installazione di SSMS | Correzione di un problema per cui SSMS si chiudeva immediatamente dopo aver visualizzato la schermata iniziale. </br> Per altri dettagli, vedere questi siti: [Commenti e suggerimenti degli utenti per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37502512), [SSMS Refuses to Start](https://dba.stackexchange.com/questions/238609/ssms-refuses-to-start) (SSMS rifiuta l'avvio) e [Database Administrators](https://dba.stackexchange.com/questions/237086/sql-server-management-studio-18-wont-open-only-splash-screen-pops-up) (Amministratori di database). |
| Esplora oggetti | Elevazione della restrizione per abilitare *Avvia Powershell* quando si è connessi a SQL in Linux. |
| Esplora oggetti | Correzione di un problema che causava l'arresto anomalo di SSMS durante il tentativo di espandere il nodo Gruppo con scalabilità orizzontale PolyBase (quando si è connessi a un nodo di calcolo). |
| Esplora oggetti | Correzione di un problema per cui la voce di menu *Disabilitato* restava abilitata anche dopo aver disabilitato un determinato indice. Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37735375). |
| Report | Correzione del report per la visualizzazione di GrantedQueryMemory in KB (report Performance Dashboard di SQL). Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37167289). |
| Report | Miglioramento della traccia del blocco del log negli scenari Always On. |
| Showplan | Aggiunta del nuovo elemento showplan *SpillOccurred* allo schema showplan. |
| Showplan | Aggiunta delle letture remote (*ActualPageServerReads*, *ActualPageServerReadAheads* *ActualLobPageServerReads*, *ActualLobPageServerReadAheads*) allo schema showplan. |
| SMO/scripting | Evita l'esecuzione di query ai vincoli di arco durante la creazione di script per tabelle non grafo. |
| SMO/scripting | Rimozione del vincolo sulla classificazione di riservatezza durante la creazione di script per colonne con *Classificazione dati*. |
| SMO/scripting | Correzione di un problema a causa del quale "Genera script" in una tabella grafo non funziona durante la generazione di dati. Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898466). |
| SMO/scripting | Correzione di un problema a causa del quale il metodo EnumObjects() non recuperava il nome dello schema per un oggetto Synonym. |
| SMO/scripting | Correzione di un problema in UIConnectionInfo.LoadFromStream() in cui la sezione *AdvancedOptions* non veniva letta (quando non era specificata una password). |
| SQL Agent | Correzione di un problema che causava l'arresto anomalo di SSMS durante l'uso della finestra Proprietà processo. Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37164244). |
| SQL Agent | Correzione di un problema per cui il pulsante "Visualizza" in *Proprietà passaggio processo* non era sempre abilitato impedendo così la visualizzazione dell'output di un passaggio di processo specifico. |
| Interfaccia utente XEvent | Aggiunta della colonna "Pacchetto" all'elenco XEvents per evitare ambiguità tra eventi con nomi identici. |
| Interfaccia utente XEvent | Aggiunta del mapping del tipo di classe "EXTERNAL LIBRARY" mancante a XEventUI. |

#### <a name="known-issues-181"></a>Problemi noti (18.1)

- Gli utenti potrebbero visualizzare un errore trascinando un oggetto tabella da Esplora oggetti nell'editor di query. Siamo a conoscenza del problema, la cui correzione è prevista nella prossima versione.

- Le opzioni colore di *Connessioni di gruppo* e *Connessioni a un unico server* in Opzioni -> Editor di testo -> Scheda editor e barra di stato -> Layout e colori barra di stato non vengono mantenute dopo la chiusura di SSMS 18.1. Alla riapertura di SSMS, l'opzione Layout e colori barra di stato ripristina il valore predefinito (bianco).

- Esiste una limitazione alle dimensioni dei dati visualizzati dai risultati di SSMS in griglia, testo o file.

- Il diagramma di database creato da SSMS in esecuzione nel computer A non può essere modificato dal computer B (si verifica un arresto anomalo di SSMS). Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37992649).

### <a name="180"></a>18.0

![download](media/download-icon.png) [Scaricare SSMS 18.0](https://go.microsoft.com/fwlink/?linkid=2088649)

- Numero di versione: 18.0  
- Numero di build: 15.0.18118.0  
- Data di rilascio: 24 aprile 2019

[Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x804)| [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x404)| [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x409)| [Francese](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x40c)| [Tedesco](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x407)| [Italiano](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x410)| [Giapponese](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x411)| [Coreano](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x412)| [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x416)| [Russo](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x419)| [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x40a)

#### <a name="whats-new-in-180"></a>Novità della versione 18.0

| Nuovo elemento| Dettagli|
| :-------| :------|
|Supporto per SQL Server 2019|SSMS 18.0 è la prima versione di SSMS che *supporta completamente* SQL Server 2019 (livello di compatibilità 150).|
|Supporto per SQL Server 2019|Supporto di "BATCH_STARTED_GROUP" e "BATCH_COMPLETED_GROUP" in SQL Server 2019 e in Istanza gestita di SQL.|
|Supporto per SQL Server 2019|SMO: aggiunta del supporto per l'inlining della funzione definita dall'utente.|
|Supporto per SQL Server 2019|GraphDB: aggiunta di un flag nello showplan per la sequenza Graph TC.|
|Supporto per SQL Server 2019|Always Encrypted: aggiunta del supporto per AEv2/Enclave.|
|Supporto per SQL Server 2019|Always Encrypted: la finestra di dialogo di connessione ha una nuova scheda "Always Encrypted" quando l'utente fa clic sul pulsante "Opzioni" per abilitare e configurare il supporto degli enclave.|
|File di download di SSMS di dimensioni più piccole| Le dimensioni correnti sono di circa 500 MB, quasi la metà del bundle SSMS 17.x.
|SSMS è basato su Visual Studio 2017 Shell (Isolated).|La nuova shell (SSMS è basato su Visual Studio 2017 15.9.11) include tutte le correzioni per l'accessibilità apportate sia in SSMS sia in Visual Studio, oltre alle ultime correzioni per la sicurezza.|
|Miglioramenti di accessibilità di SSMS| È stata dedicata un'attenzione particolare ai problemi di accessibilità in tutti gli strumenti (SSMS, DTA e Profiler)|
|SSMS può ora essere installato in una cartella personalizzata| Questa opzione è disponibile sia dalla riga di comando (utile per l'installazione automatica) che dall'interfaccia utente di installazione. Dalla riga di comando passare questo argomento aggiuntivo a SSMS-Setup-ENU.exe:   SSMSInstallRoot=C:\MySSMS18 Per impostazione predefinita, il nuovo percorso di installazione per SSMS è: %ProgramFiles(x86)%\Microsoft SQL Server Management Studio 18\Common7\IDE\ssms.exe.  Questo non significa che SSMS sia multi-istanza.|
|SSMS consente l'installazione in una lingua diversa da quella del sistema operativo|Il blocco per l'installazione in lingue miste è stato rimosso. È ad esempio possibile installare SSMS in tedesco in una versione di Windows in francese. Se la lingua del sistema operativo non corrisponde a quella di SSMS, l'utente deve cambiare la lingua in **Strumenti** > **Opzioni** > **Impostazioni internazionali**, altrimenti SSMS visualizza l'interfaccia utente in inglese.|
|SSMS non condivide più componenti con il motore SQL|È stata dedicata un'attenzione particolare a evitare la condivisione di componenti con il motore SQL. Questa condivisione generava spesso problemi di funzionalità quando venivano usati i file installati da altre applicazioni.|
|SSMS richiede NetFx 4.7.2 o versioni successive|Il requisito minimo è passato da NetFx4.6.1 a NetFx4.7.2: in tal modo è possibile trarre vantaggio dalle nuove funzionalità incluse nel nuovo framework.|
|Possibilità di eseguire la migrazione delle impostazioni di SSMS| La prima volta che SSMS 18 viene avviato, all'utente viene richiesto di eseguire la migrazione delle impostazioni della versione 17.x. I file di impostazioni dell'utente vengono ora archiviati in un file XML normale, migliorando la portabilità e possibilmente consentendo le modifiche.|
|Supporto per valori DPI alti| Il supporto per valori DPI alti è ora abilitato per impostazione predefinita.|
|SSMS viene fornito con il driver Microsoft OLE DB| Per i dettagli, vedere [Scaricare il driver Microsoft OLE DB per SQL Server](../connect/oledb/download-oledb-driver-for-sql-server.md).|
|SSMS non è supportato in Windows 8. Per Windows 10/Windows Server 2016 è richiesta la versione 1607 (10.0.14393) o successiva|A causa della nuova dipendenza da NetFx 4.7.2, SSMS 18.0 non viene installato in Windows 8, nelle versioni meno recenti di Windows 10 e in Windows Server 2016. Il programma di installazione di SSMS blocca tali sistemi. Windows 8.1 è ancora supportato.|
|SSMS non viene più aggiunto alla variabile di ambiente PATH|Il percorso di SSMS.EXE e degli strumenti in generale non viene più aggiunto al percorso. Gli utenti possono aggiungerlo direttamente o, nel caso di un computer Windows moderno, tramite il menu Start.|
|Gli ID pacchetto non sono più necessari per sviluppare le estensioni di SSMS| In passato SSMS caricava in modo selettivo solo i pacchetti noti e gli sviluppatori dovevano registrare i propri pacchetti. Questo non si verifica più.|
|SQL Server Management Studio (SSMS) - Generale|Opzione di configurazione AUTOGROW_ALL_FILES disponibile per i filegroup in SSMS.|
|SQL Server Management Studio (SSMS) - Generale|Rimozione delle opzioni a rischio 'lightweight pooling' e 'priority boost' dalla GUI di SSMS. Per i dettagli, vedere [Priority boost details - and why it's not recommended](https://deep.data.blog/2010/01/26/priority-boost-details-and-why-its-not-recommended/) (Dettagli sull'opzione di priority boost e perché non è consigliata).
|SQL Server Management Studio (SSMS) - Generale|Nuovi menu e tasti di scelta rapida per la creazione di file: **CTRL+ALT+N**. **CTRL+N** continua a funzionare per creare una nuova query.|
|SQL Server Management Studio (SSMS) - Generale|La finestra di dialogo **Nuova regola del firewall** consente ora di specificare un nome di regola invece di generarne uno automaticamente.|
|SQL Server Management Studio (SSMS) - Generale|Funzionalità IntelliSense migliorata nell'editor, in particolare per T-SQL v140 e successive.|
|SQL Server Management Studio (SSMS) - Generale|Aggiunta del supporto UTF-8 nella finestra di dialogo delle regole di confronto dell'interfaccia utente di SSMS.|
|SQL Server Management Studio (SSMS) - Generale|Passaggio a "Gestione credenziali di Windows" per le password MRU della finestra di dialogo Connessione. Risolve un problema rilevato da tempo per cui la persistenza delle password non era sempre affidabile.|
|SQL Server Management Studio (SSMS) - Generale|Supporto migliorato per i sistemi con più monitor, con un numero crescente di finestre e più finestre di dialogo ora visualizzate sul monitor previsto.|
|SQL Server Management Studio (SSMS) - Generale|Viene visualizzata la configurazione server "backup checksum default" nella nuova pagina Impostazioni database della finestra di dialogo Proprietà server. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/08035-sql-server/suggestions/34634974](https://feedback.azure.com/forums/08035-sql-server/suggestions/34634974).|
|SQL Server Management Studio (SSMS) - Generale|Dimensioni massime dei file di log degli errori incluse in "Configura log degli errori di SQL Server". Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035/suggestions/33624115](https://feedback.azure.com/forums/908035/suggestions/33624115).|
|SQL Server Management Studio (SSMS) - Generale|Aggiunta dell'opzione "Esegui migrazione ad Azure" al menu Strumenti. Sono stati integrati Database Migration Assistant e Servizio Migrazione del database per un accesso rapido e veloce e per accelerare le migrazioni ad Azure.|
|SQL Server Management Studio (SSMS) - Generale|È stata aggiunta logica per chiedere all'utente di eseguire il commit delle transazioni aperte quando si usa "Cambia connessione".|
|Integrazione di Azure Data Studio|Aggiunta di una voce di menu per avviare e scaricare Azure Data Studio.|
|Integrazione di Azure Data Studio|Aggiunta della voce di menu "Start Azure Data Studio" (Avvia Azure Data Studio) in Esplora oggetti.|
|Integrazione di Azure Data Studio|Quando si fa clic con il pulsante destro del mouse su un nodo di database in Esplora oggetti, vengono visualizzati i menu di scelta rapida per eseguire una query o per creare un nuovo notebook in Azure Data Studio.|
|Supporto di Azure SQL| Le proprietà del database SLO/Edition/MaxSize ora accettano nomi personalizzati, facilitando il supporto delle edizioni future del database SQL di Azure.|
|Supporto di Azure SQL| Aggiunto il supporto per gli SKU vCore (utilizzo generico e business critical): Gen4_24 e tutti gli SKU Gen5.|
|Istanza gestita di SQL di Azure|Aggiunta di "Accesso ad Azure AD" come nuovo tipo di account di accesso in SMO e SSMS quando ci si connette a Istanza gestita di SQL di Azure.|
|Always On|Rigenerazione hash per RTO (tempo di recupero stimato) e RPO (perdita di dati stimata) nel dashboard Always On di SSMS. Vedere la documentazione aggiornata all'indirizzo [https://docs.microsoft.com/sql/database-engine/availability-groups/windows/monitor-performance-for-always-on-availability-groups](../database-engine/availability-groups/windows/monitor-performance-for-always-on-availability-groups.md).|
|Always Encrypted| La casella di controllo Abilita Always Encrypted nella nuova scheda Always Encrypted nella finestra di dialogo Connetti al server ora consente di abilitare o disabilitare facilmente la funzionalità Always Encrypted per una connessione di database.|
|Always Encrypted con enclave sicuri| In SQL Server 2019 sono stati apportati diversi miglioramenti per il supporto della funzionalità Always Encrypted con enclave sicuri:  Un campo di testo per specificare l'URL di attestazione dell'enclave nella finestra di dialogo Connetti al server (la nuova scheda Always Encrypted).  La nuova casella di controllo nella finestra di dialogo Nuova chiave master della colonna per controllare se una nuova chiave master della colonna consente i calcoli dell'enclave.  Altre finestre di dialogo di gestione delle chiavi Always Encrypted ora visualizzano informazioni su quali chiavi master della colonna consentono i calcoli dell'enclave.|
|File di controllo|Metodo di autenticazione cambiato dal metodo basato sulla Chiave account di archiviazione all'autenticazione basata su Azure AD.|
|Classificazione dei dati| Riorganizzazione del menu attività Classificazione dati : è stato aggiunto un sottomenu al menu delle attività del database ed è stata aggiunta un'opzione per aprire il report dal menu senza dover aprire prima la finestra Classifica dati.|
|Classificazione dei dati|Aggiunta della nuova funzionalità 'Classificazione dati' in SMO. L'oggetto colonna espone nuove proprietà: SensitivityLabelName, SensitivityLabelId, SensitivityInformationTypeName, SensitivityInformationTypeId e IsClassified (sola lettura). Per altre informazioni, vedere [ADD SENSITIVITY CLASSIFICATION (Transact-SQL)](../t-sql/statements/add-sensitivity-classification-transact-sql.md)|
|Classificazione dei dati|Aggiunta della nuova voce di menu "Classification Report" (Report di classificazione) nel riquadro a comparsa "Classificazione dati".|
|Classificazione dei dati| Raccomandazioni aggiornate.|
|Aggiornamento del livello di compatibilità del database|Aggiunta di una nuova opzione **_Nome database_ *_ > _* _Attività_ *_ > _* _Aggiornamento database_ *_. Questa opzione consentirà di avviare il nuovo _* Assistente ottimizzazione query** per guidare gli utenti nel processo di: Raccolta di baseline delle prestazioni prima dell'aggiornamento del livello di compatibilità del database. Aggiornamento al livello di compatibilità del database desiderato.  Raccolta di un secondo passaggio di dati sulle prestazioni nel corso dello stesso carico di lavoro. Rilevamento delle regressioni del carico di lavoro e specifica di elementi consigliati testati per migliorare le prestazioni del carico di lavoro.  È simile al processo di aggiornamento del database documentato negli [scenari di utilizzo dell'archivio query](../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade), tranne nell'ultimo passaggio in cui l'Assistente ottimizzazione query non si basa su uno stato valido noto in precedenza per generare le raccomandazioni.|
|Importazione guidata applicazione livello dati|È stato aggiunto il supporto per l'applicazione livello dati di importazione/esportazione con tabelle grafi.|
|Procedura guidata Importa file flat|Aggiunta della logica per notificare all'utente che un'importazione può aver causato una ridenominazione delle colonne.|
|Integration Services (SSIS)|Aggiunta del supporto per consentire ai clienti di pianificare in Azure-SSIS IR i pacchetti SSIS che si trovano nel cloud di Azure per enti pubblici.|
|Integration Services (SSIS)|Quando si usa il servizio SQL Agent dell'istanza gestita di Azure SQL tramite SSMS, è possibile configurare i parametri e la gestione connessioni nel passaggio di processo dell'agente SSIS.|
|Integration Services (SSIS)|Quando ci si connette al database SQL di Azure o a Istanza gestita di SQL di Azure, è possibile effettuare la connessione con *default* come database iniziale.|
|Integration Services (SSIS)|Aggiunta della nuova voce **Try SSIS in Azure Data Factory** (Prova SSIS in Azure Data Factory) nel nodo "Cataloghi di Integration Services", che può essere usata per avviare "Integration Runtime Creation Wizard" (Creazione guidata Integration Runtime) e creare rapidamente "Azure-SSIS Integration Runtime".
|Integration Services (SSIS)|Aggiunta del pulsante **Create SSIS IR** (Crea SSIS IR) in "Catalog Creation Wizard" (Creazione guidata catalogo), che può essere usato per avviare "Integration Runtime Creation Wizard" (Creazione guidata Integration Runtime) e creare rapidamente "Azure-SSIS Integration Runtime".|
|Integration Services (SSIS)|ISDeploymentWizard supporta ora l'autenticazione SQL, nonché l'autenticazione integrata e l'autorizzazione tramite password di Azure Active Directory in modalità riga di comando.|
|Integration Services (SSIS)|La distribuzione guidata supporta ora la creazione e la distribuzione in SSIS Integration Runtime di Azure Data Factory.|
|Scripting per gli oggetti|Aggiunta di nuove voci di menu per "CREATE OR ALTER" quando si esegue lo scripting per gli oggetti.|
|Archivio query|Migliorata l'usabilità di alcuni report (Consumo complessivo risorse) grazie all'aggiunta di migliaia di separatori per i numeri visualizzati sull'asse Y dei grafici.|
|Archivio query|Aggiunto un nuovo report Statistiche di attesa query.|
|Archivio query|Aggiunta della metrica "Conteggio esecuzioni" alla vista "Tracked Query" (Query rilevata).|
|Strumenti di replica|Aggiunta del supporto per la funzionalità di specifica delle porte non predefinite in Monitoraggio replica e SSMS.|
|Showplan|Aggiunti il tempo trascorso reale e le righe effettive rispetto alle righe stimate (se disponibili) sotto il nodo dell'operatore ShowPlan. Questa modifica fa sì che il piano effettivo sia coerente con il piano Statistiche query dinamiche.|
|Showplan|Modificata la descrizione comando e aggiunto un commento quando si fa clic sul pulsante Modifica query per un elemento ShowPlan, per indicare all'utente che l'elemento ShowPlan potrebbe essere troncato dal motore SQL se la query supera i 4000 caratteri.|
|Showplan|Aggiunto codice per visualizzare "Materializer Operator (External Select)".|
|Showplan|Aggiunto un nuovo attributo showplan BatchModeOnRowStoreUsed per identificare facilmente le query che usano la funzionalità "batch-mode scan on rowstores". Ogni volta che una query esegue "batch mode scan on rowstores" (analisi in modalità batch su rowstore) viene aggiunto un nuovo attributo (BatchModeOnRowStoreUsed="true") all'elemento StmtSimple.|
|Showplan|Aggiunta del supporto di Showplan a LocalCube RelOp per DW ROLLUP e CUBE.|
|Showplan|Nuovo operatore LocalCube per la nuova funzione di aggregazione ROLLUP e CUBE in Azure Synapse Analytics.|
|SMO| Supporto SMO esteso per la creazione di indici ripristinabili.|
|SMO| Aggiunto un nuovo evento per gli oggetti SMO ("PropertyMissing") che aiuta gli autori di applicazioni a rilevare i problemi di prestazioni SMO in tempi più rapidi.|
|SMO| Nuova proprietà DefaultBackupChecksum esposta nell'oggetto Configuration, che corrisponde alla configurazione del server "backup checksum default".|
|SMO| Nuova proprietà ProductUpdateLevel inclusa nell'oggetto Server, che esegue il mapping al livello di servizio per la versione di SQL in uso (ad esempio CU12, RTM).|
|SMO| Nuova proprietà LastGoodCheckDbTime inclusa in un oggetto Database, che esegue il mapping alla proprietà del database "lastgoodcheckdbtime". Se tale proprietà non è disponibile, viene restituito il valore predefinito 1/1/1900 12:00:00 AM.|
|SMO|Percorso del file RegSrvr.xml (file di configurazione Server registrato) spostato in "%AppData%\Microsoft\SQL Server Management Studio"(senza versione, quindi può essere condiviso tra più versioni di SSMS).|
|SMO|Aggiunta di "Cloud di controllo" come nuovo tipo di quorum e nuovo tipo di risorsa.|
|SMO|Aggiunta del supporto per i "vincoli di arco" sia in SMO che in SSMS.|
|SMO|Aggiunto il supporto per l'eliminazione a catena nei "vincoli di arco" sia in SMO che in SSMS.|
|SMO|Aggiunto il supporto dell'autorizzazione di "lettura/scrittura" per la classificazione dei dati.|
|Valutazione della vulnerabilità| Abilitazione del menu delle attività Valutazione della vulnerabilità in Azure SQL DW.|
|Valutazione della vulnerabilità|Modifica del set di regole di valutazione della vulnerabilità eseguito in Istanza gestita di SQL di Azure, in modo da rendere coerenti con il database SQL di Azure i risultati dell'analisi di "Valutazione della vulnerabilità".|
|Valutazione della vulnerabilità| "Valutazione vulnerabilità" supporta ora Azure SQL Data Warehouse.|
|Valutazione della vulnerabilità|Aggiunta di una nuova funzionalità per l'esportazione in Excel dei risultati dell'analisi di Valutazione vulnerabilità.|
|Visualizzatore XEvent|Visualizzatore XEvent: abilitata la finestra di showplan per un maggior numero di XEvent.|

#### <a name="bug-fixes-in-180"></a>Correzioni di bug nella versione 18.0

| Nuovo elemento | Dettagli|
|----------|--------|
|Arresti anomali e blocchi|Correzione di un'origine di arresti anomali SSMS comuni associati agli oggetti GDI.|
|Arresti anomali e blocchi|Correzione di un'origine comune di blocchi del sistema e prestazioni insufficienti quando si seleziona "Script come Crea/Aggiorna/Rilascia" (rimosse operazioni inutili di recupero di oggetti SMO).|
|Arresti anomali e blocchi|Correzione di un problema a causa del quale il sistema smette di rispondere quando si connette a un database SQL di Azure con MFA mentre sono abilitate le tracce ADAL.|
|Arresti anomali e blocchi|Correzione di un problema a causa del quale il sistema smette di rispondere (o si verifica una condizione di blocco percepito) in Statistiche query dinamiche chiamato da Monitor attività (il problema si manifesta quando si usa l'autenticazione di SQL Server senza nessuna impostazione "Mantieni informazioni di sicurezza").|
|Arresti anomali e blocchi|Correzione di un problema a causa del quale il sistema smette di rispondere quando si seleziona "Report" in Esplora oggetti. Il problema potrebbe presentarsi in presenza di connessioni con latenza elevata o in caso di non accessibilità temporanea delle risorse.|
|Arresti anomali e blocchi|Correzione di un problema di arresto anomalo in SSMS quando si prova a usare il server di gestione centrale e i server SQL di Azure. Per informazioni dettagliate, vedere l'articolo sull'[errore e arresto anomalo dell'applicazione SMSS 17.5 quando si usa il server di gestione centrale](https://feedback.azure.com/forums/908035/suggestions/33374884).|
|Arresti anomali e blocchi|Correzione di un problema a causa del quale il sistema smette di rispondere in Esplora oggetti ottimizzando la modalità di recupero della proprietà IsFullTextEnabled.|
|Arresti anomali e blocchi|Correzione di un problema a causa del quale il sistema smette di rispondere in "Copia guidata database" evitando la compilazione di query non necessarie per recuperare le proprietà del database.|
|Arresti anomali e blocchi|Correzione di un problema a causa del quale SSMS smetteva di rispondere o subiva un arresto anomalo durante la modifica di T-SQL.|
|Arresti anomali e blocchi|Riduzione di un problema per cui SSMS non risponde durante la modifica di grandi script T-SQL.|
|Arresti anomali e blocchi|Correzione di un problema a causa del quale la memoria di SSMS si esaurisce durante la gestione dei set di dati di grandi dimensioni restituiti dalle query.|
|SQL Server Management Studio (SSMS) - Generale|Correzione di un problema per cui "ApplicationIntent" non veniva passato nelle connessioni in "Server registrati".|
|SQL Server Management Studio (SSMS) - Generale|Correzione di un problema a causa del quale il rendering del modulo dell'interfaccia utente della creazione guidata di una nuova sessione di XEvent non veniva eseguito correttamente nei monitor con valori DPI alti.|
|SQL Server Management Studio (SSMS) - Generale|Correzione di un problema che si verifica durante il tentativo di importare un file bacpac.|
|SQL Server Management Studio (SSMS) - Generale|Correzione di un problema a causa del quale veniva generato un errore di overflow aritmetico durante il tentativo di visualizzare le proprietà di un database (con FILEGROWTH > 2048 GB).|
|SQL Server Management Studio (SSMS) - Generale|Risolto un problema in cui il report Perf Dashboard segnalava attese PAGELATCH e PAGEIOLATCH che non si trovavano nei sottoreport.|
|SQL Server Management Studio (SSMS) - Generale|Un'altra serie di correzioni per fare in modo che SSMS riconosca più monitor e apra una finestra di dialogo nel monitor corretto.|
|Analysis Services|Correzione di un problema per cui "Impostazioni avanzate" per l'interfaccia utente AS Xevent viene troncata.|
|Analysis Services|Risolto un problema per cui l'analisi DAX generava l'eccezione File non trovato.|
|database SQL di Azure|Correzione di un problema per cui l'elenco di database non veniva compilato correttamente per la finestra delle query del database SQL di Azure in caso di connessione a un database utente nel database SQL di Azure anziché a un database master.|
|database SQL di Azure|Correzione di un problema per cui non era possibile aggiungere "Tabella temporale" a un database SQL di Azure.|
|database SQL di Azure|Abilitata l'opzione di sottomenu Proprietà statistiche nel menu Statistiche in Azure, che è ormai completamente supporta da molto tempo.|
|Azure SQL - Supporto generale|Correzione di problemi in un controllo comune dell'interfaccia utente di Azure UI che impedisce all'utente di visualizzare le sottoscrizioni di Azure (se il loro numero è maggiore di 50). Impostazione dell'ordinamento in base al nome anziché in base all'ID sottoscrizione. L'utente poteva trovare questa impostazione ad esempio quando provava a ripristinare un backup da un URL.|
|Azure SQL - Supporto generale|Correzione di un problema nel controllo interfaccia utente di Azure comune a causa del quale l'enumerazione di sottoscrizioni può generare un errore "L'indice non è compreso nell'intervallo. Deve essere non negativo e inferiore al numero di colonne." quando l'utente non aveva sottoscrizioni in alcuni tenant. L'utente poteva trovare questa impostazione ad esempio quando provava a ripristinare un backup da un URL.|
|Azure SQL - Supporto generale|Correzione di un problema per cui gli obiettivi del livello di servizio vengono impostati come hardcoded, rendendo più difficile per SSMS supportare gli obiettivi del livello di servizio più recenti di SQL di Azure. Ora gli utenti possono accedere ad Azure e consentire a SSMS di recuperare tutti i dati degli obiettivi del livello di servizio applicabili (Edition e Max Size)|
|Supporto per Istanza gestita di database SQL di Azure|Miglioramento del supporto di istanze gestite: disabilitazione delle opzioni non supportate nell'interfaccia utente e risoluzione di un problema di Visualizza log di controllo per la gestione della destinazione di controllo URL.|
|Supporto per Istanza gestita di database SQL di Azure|La procedura guidata "Genera e pubblica script" include clausole CREATE DATABASE non supportate.|
|Supporto per Istanza gestita di database SQL di Azure|Abilitazione di Statistiche query dinamiche per le istanze gestite.|
|Supporto per Istanza gestita di database SQL di Azure|Proprietà database -> File generava codice script errato per ALTER DB ADD FILE.|
|Supporto per Istanza gestita di database SQL di Azure|Correzione della regressione con l'utilità di pianificazione di SQL Agent in cui la pianificazione ONIDLE veniva scelta anche quando era stato scelto un altro tipo di pianificazione.|
|Supporto per Istanza gestita di database SQL di Azure|Regolazione di MAXTRANSFERRATE, MAXBLOCKSIZE per i backup in Archiviazione di Azure.|
|Supporto per Istanza gestita di database SQL di Azure|Problema per cui il backup della parte finale del log viene aggiunto allo script prima dell'operazione RESTORE (operazione non supportata in CL).|
|Supporto per Istanza gestita di database SQL di Azure|La procedura guidata Crea database non esegue correttamente lo scripting dell'istruzione CREATE DATABASE.|
|Supporto per Istanza gestita di database SQL di Azure|Gestione speciale dei pacchetti SSIS all'interno di SSMS durante la connessione a istanze gestite.|
|Supporto per Istanza gestita di database SQL di Azure|Correzione di un problema per cui veniva visualizzato un errore quando si provava a usare "Monitor attività" durante la connessione a istanze gestite.|
|Supporto per Istanza gestita di database SQL di Azure|Supporto migliorato per gli account di accesso di Azure AD (in SSMS Explorer).|
|Supporto per Istanza gestita di database SQL di Azure|Scripting degli oggetti filegroup di SMO migliorato.|
|Supporto per Istanza gestita di database SQL di Azure|Interfaccia utente migliorata per le credenziali.|
|Supporto per Istanza gestita di database SQL di Azure|Supporto aggiunto per la logica di replica.|
|Supporto per Istanza gestita di database SQL di Azure|Correzione di un problema a causa del quale se si fa clic con il pulsante destro del mouse su un database e si sceglie 'Importa applicazione livello dati', l'operazione ha esito negativo.|
|Supporto per Istanza gestita di database SQL di Azure|Correzione di un problema a causa del quale se si fa clic con il pulsante destro del mouse su "TempDB" vengono generati errori.|
|Supporto per Istanza gestita di database SQL di Azure|Correzione di un problema per cui quando si prova a creare lo script dell'istruzione ALTER DB ADD FILE in SMO, lo script T-SQL generato è vuoto.|
|Supporto per Istanza gestita di database SQL di Azure|Migliorata la visualizzazione di proprietà specifiche del server delle istanze gestite (generazione di hardware, livello di servizio, spazio di archiviazione usato e riservato).|
|Supporto per Istanza gestita di database SQL di Azure|Correzione di un problema per cui lo scripting di un database ("Script come CREATE") non includeva filegroup e file aggiuntivi. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035/suggestions/37326799](https://feedback.azure.com/forums/908035/suggestions/37326799). |
|Backup/ripristino/collegamento/scollegamento del database|Correzione di un problema per cui l'utente non è in grado di associare un database quando il nome file fisico del file con estensione mdf non corrisponde al nome file originale.|
|Backup/ripristino/collegamento/scollegamento del database|Correzione di un problema a causa del quale SSMS non trova un piano di ripristino valido o ne trova uno non ottimale. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035-sql-server/suggestions/32897752](https://feedback.azure.com/forums/908035-sql-server/suggestions/32897752). |
|Backup/ripristino/collegamento/scollegamento del database|Risolto un problema per cui nella procedura guidata "Collega database" non venivano visualizzati file secondari rinominati. Ora tali file vengono visualizzati con un commento aggiunto, ad esempio, "Non trovato". Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035/suggestions/32897434](https://feedback.azure.com/forums/908035/suggestions/32897434). |
|Copia guidata database|Il tentativo di Genera script/Trasferisci/Copia guidata database di creare una tabella con una tabella in memoria non forza ansi_padding on.|
|Copia guidata database|Attività Trasferisci database/Copia guidata database interrotte in SQL Server 2017 e SQL Server 2019.|""
|Copia guidata database|Creazione della tabella di script di Genera script/Trasferisci/Copia guidata database prima della creazione di un'origine dati esterni associata.|
|Finestra di dialogo di connessione|Abilitazione della rimozione di nomi utente dall'elenco di nomi utente precedente premendo CANC. Per informazioni dettagliate, vedere l'articolo su come [consentire l'eliminazione di utenti dalla finestra di accesso di SSMS](https://feedback.azure.com/forums/908035/suggestions/32897632).|
|Importazione guidata applicazione livello dati|Correzione di un problema a causa del quale l'importazione guidata di applicazione livello dati non funzionava se connessi tramite Azure Active Directory (Azure AD).|
|Classificazione dei dati|Correzione di un problema che si verifica quando si salvano le classificazioni nel riquadro di classificazione dei dati mentre sono aperti altri riquadri di classificazione dei dati in altri database.|
|Importazione guidata applicazione livello dati|Correzione di un problema a causa del quale l'utente non poteva importare un'applicazione livello dati (con estensione dacpac) a causa dell'accesso limitato al server (ad esempio nessun accesso a tutti i database nello stesso server).|
|Importazione guidata applicazione livello dati|Risolto un problema che rendeva l'importazione estremamente lenta quando molti database erano ospitati nello stesso server di Azure SQL.|
|Tabelle esterne|Aggiunta del supporto per Rejected_Row_Location in modelli, SMO, IntelliSense e nella griglia delle proprietà.|
|Procedura guidata Importa file flat|Correzione di un problema per cui la procedura guidata "Importa file flat" non gestiva correttamente le virgolette doppie (escape). Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035/suggestions/32897998](https://feedback.azure.com/forums/908035/suggestions/32897998). |
|Procedura guidata Importa file flat|Correzione di un problema di gestione errata dei tipi a virgola mobile (per impostazioni locali che usano un delimitatore diverso dal punto).|
|Procedura guidata Importa file flat|Correzione di un problema associato all'importazione di bit quando i valori sono 0 o 1. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035-sql-server/suggestions/32898535](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898535). |
|Procedura guidata Importa file flat|Correzione di un problema a causa del quale i valori di tipo *float* venivano immessi come *null*.|
|Procedura guidata Importa file flat|Correzione di un problema per cui l'importazione guidata non elaborava valori decimali negativi.|
|Procedura guidata Importa file flat|Correzione di un problema per cui la procedura guidata non eseguiva l'importazione da file CSV a colonna singola.|
|Procedura guidata Importa file flat|Correzione di un problema a causa del quale la procedura guidata Importa file flat non consente di cambiare la tabella di destinazione se esiste già. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035-sql-server/suggestions/32896186](https://feedback.azure.com/forums/908035-sql-server/suggestions/32896186). |
|Visualizzatore della Guida|Miglioramento della logica associata al supporto delle modalità online/offline. Alcuni problemi devono essere ancora risolti.|
|Visualizzatore della Guida|Correzione di "Visualizza Guida" per rispettare le impostazioni online/offline. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035-sql-server/suggestions/32897791](https://feedback.azure.com/forums/908035-sql-server/suggestions/32897791). |
|Disponibilità elevata e ripristino di emergenza<BR> Gruppi di disponibilità (disponibilità generale)|Correzione di un problema per cui i ruoli nella procedura guidata "Failover gruppo di disponibilità" vengono sempre visualizzati come "Risoluzione in corso".|
|Disponibilità elevata e ripristino di emergenza<BR> Gruppi di disponibilità (disponibilità generale)|Risolto un problema per cui SSMS visualizzava avvisi troncati nel dashboard del gruppo di disponibilità.|
|Integration Services|Correzione di un problema a causa del quale la distribuzione guidata non si connette a SQL Server quando nello stesso computer sono installati sia SQL Server 2019 che SSMS 18.0.|
|Integration Services|Risolto un problema per cui l'attività di pianificazione della manutenzione non poteva essere modificata al momento della progettazione della pianificazione della manutenzione.|
|Integration Services|Correzione di un problema a causa del quale la distribuzione guidata si blocca se il progetto in distribuzione viene rinominato.|
|Integration Services|Abilitata l'impostazione dell'ambiente nella funzionalità di pianificazione di Azure-SSIS IR.|
|Integration Services|Correzione di un problema per cui la creazione guidata di SSIS Integration Runtime smette di rispondere se l'account utente appartiene a più di 1 tenant.|
|Monitoraggio attività processi|Correzione di un arresto anomalo quando si usa Monitoraggio attività processi (con filtri).|
|Esplora oggetti|Correzione di un problema per cui SSMS genera un'eccezione, ad esempio perché non è possibile eseguire il cast dell'oggetto da DBNull ad altri tipi, quando si prova a espandere il nodo "Gestione" in Esplora oggetti (errore di configurazione di DataCollector).|
|Esplora oggetti|Correzione di un problema per cui Esplora oggetti non applicava escape alle virgolette prima di richiamare "Modifica le prime righe"rendendo confusa la finestra di progettazione.|
|Esplora oggetti|Correzione di un problema per cui la procedura guidata "Importare un'applicazione livello dati" non viene avviata dalla struttura di Archiviazione di Azure.|
|Esplora oggetti|Correzione di un problema per cui in "Configurazione Posta elettronica database" lo stato della casella di controllo TLS/SSL non veniva mantenuto. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035-sql-server/suggestions/32895541](https://feedback.azure.com/forums/908035-sql-server/suggestions/32895541). |
|Esplora oggetti|Correzione di un problema per cui in SSMS l'opzione Chiudi connessioni esistenti viene disattivata quando si prova a ripristinare il database con is_auto_update_stats_async_on.|
|Esplora oggetti|Correzione di un problema a causa del quale quando si faceva clic con il pulsante destro del mouse sui nodi in Esplora oggetti, ad esempio "Tabelle", e si voleva eseguire un'azione come ad esempio filtrare le tabelle selezionando Filtro > Impostazioni filtro, le impostazioni di filtro potevano essere visualizzate in un altro schermo rispetto a quello in cui SSMS era attivo. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035-sql-server/suggestions/34284106](https://feedback.azure.com/forums/908035-sql-server/suggestions/34284106). |
|Esplora oggetti|Correzione di un problema rilevato da tempo per cui CANC non funzionava in Esplora oggetti quando si tentava di rinominare un oggetto. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035-sql-server/suggestions/33073510](https://feedback.azure.com/forums/908035-sql-server/suggestions/33073510), [https://feedback.azure.com/forums/908035/suggestions/32910247](https://feedback.azure.com/forums/908035/suggestions/32910247) e altri duplicati.|
|Esplora oggetti|Quando si visualizzano le proprietà di file di database esistenti, le dimensioni vengono visualizzate in una colonna "Dimensioni (MB)" anziché "Dimensioni iniziali (MB)", che è la colonna visualizzata quando si crea un nuovo database. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035-sql-server/suggestions/32629024](https://feedback.azure.com/forums/908035-sql-server/suggestions/32629024). |
|Esplora oggetti|Disattivazione della voce di menu contestuale "Progettazione" in "Tabelle grafi" perché queste tabelle non sono supportate nella versione corrente di SSMS.|
|Esplora oggetti|Correzione di un problema per cui la finestra di dialogo "Nuova pianificazione processo" non veniva visualizzata correttamente nei monitor con valori DPI alti. Per informazioni dettagliate, vedere [https://feedback.azure.com/admin/v3/suggestions/35541262](https://feedback.azure.com/admin/v3/suggestions/35541262). |
|Esplora oggetti|Correzione del problema e ottimizzazione della visualizzazione delle dimensioni del database ("Dimensioni (MB)") in Esplora oggetti. Si usano solo 2 cifre decimali e il separatore delle migliaia. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035/suggestions/34379308](https://feedback.azure.com/forums/908035/suggestions/34379308).|
|Esplora oggetti|Correzione di un problema che impedisce la creazione di un "Indice spaziale" con un errore analogo a "Per eseguire questa azione, impostare la proprietà PartitionScheme".|
|Esplora oggetti|Miglioramenti delle prestazioni minori in Esplora oggetti per evitare di eseguire query aggiuntive, quando possibile.|
|Esplora oggetti|Estensione della logica per richiedere una conferma quando si rinomina un database per tutti gli oggetti dello schema (l'impostazione può essere configurata).|
|Esplora oggetti|Aggiunto il corretto escape nel filtro di Esplora oggetti. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035/suggestions/36678803](https://feedback.azure.com/forums/908035/suggestions/36678803). |
|Esplora oggetti|Correzione/ottimizzazione della visualizzazione in Dettagli Esplora oggetti per mostrare i numeri con i separatori corretti. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035/suggestions/32900944](https://feedback.azure.com/forums/908035/suggestions/32900944). |
|Esplora oggetti|Correzione del menu di scelta rapida nel nodo "Tabelle" per cui, quando ci si connette a SQL Express, il riquadro a comparsa "Nuovo" è mancante, le tabelle grafi vengono visualizzate in modo non corretto e la tabella System-Versioned è mancante. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035/suggestions/37245529](https://feedback.azure.com/forums/908035/suggestions/37245529). |
|Scripting per gli oggetti|Miglioramento delle prestazioni generali: la generazione di script WideWorldImporters viene eseguita in metà tempo rispetto a SSMS 17.7.|
|Scripting per gli oggetti|Durante lo scripting di oggetti la configurazione con ambito database con valori predefiniti viene omessa.|
|Scripting per gli oggetti|Evitare di generare T-SQL dinamico durante lo scripting. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035-sql-server/suggestions/32898391](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898391). |
|Scripting per gli oggetti|Omettere le sintassi dei grafi "as edge" e "as node" per lo scripting di una tabella in SQL Server 2016 e versioni precedenti.|
|Scripting per gli oggetti|Correzione di un problema a causa del quale non è possibile eseguire lo scripting degli oggetti di database quando si usa l'autenticazione MFA di Azure AD per connettersi a un database SQL di Azure.|
|Scripting per gli oggetti|Correzione di un problema per cui tentativo di eseguire lo script di un indice spaziale con GEOMETRY_AUTO_GRID/GEOGRAPHY_AUTO_GRID in un database SQL di Azure genera un errore.|
|Scripting per gli oggetti|Correzione di un problema a causa del quale gli script di un database SQL di Azure usavano sempre come destinazione un'istanza di SQL locale, anche se le impostazioni di scripting di "Esplora oggetti" erano definite in modo da corrispondere all'origine.|
|Scripting per gli oggetti|Correzione di un problema per cui durante il tentativo di creare lo script di una tabella in un database SQL Data Warehouse con indici cluster e non cluster vengono generate istruzioni T-SQL non corrette.|
|Scripting per gli oggetti|Correzione di un problema per cui durante il tentativo di creare lo script di una tabella in un database SQL Data Warehouse con indici columnstore cluster e indici cluster vengono generate istruzioni T-SQL non corrette (duplicate).|
|Scripting per gli oggetti|Correzione del problema di scripting di tabelle partizionate senza valori di intervallo (database SQL Data Warehouse).|
|Scripting per gli oggetti|Correzione di un problema per cui l'utente non è in grado di creare lo script di un controllo o di una specifica di controllo SERVER_PERMISSION_CHANGE_GROUP.|
|Scripting per gli oggetti|Correzione di un problema per cui l'utente non è in grado di creare lo script di statistiche da SQL Data Warehouse. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035-sql-server/suggestions/32897296](https://feedback.azure.com/forums/908035-sql-server/suggestions/32897296). |
|Scripting per gli oggetti|Correzione di un problema per cui la "Generazione guidata script" visualizza una tabella errata con un errore di scripting se l'opzione "Continua generazione di script in caso di errore" è impostata su False.|
|Scripting per gli oggetti|Miglioramento della generazione di script in SQL Server 2019.|
|Profiler|Aggiunta dell'evento "Aggregate Table Rewrite Query" (Aggrega query di riscrittura tabella) agli eventi Profiler.|
|Query Data Store|Correzione di un problema per cui si può generare un'eccezione "DocumentFrame (SQLEditors)".|
|Query Data Store|Correzione di un problema che si verificava nel tentativo di impostare un intervallo di tempo personalizzato nei report predefiniti di Query Store. L'utente non poteva selezionare AM o PM nell'intervallo iniziale/finale.|
|Griglia dei risultati|Correzione di un problema in modalità Contrasto elevato (i numeri di riga selezionati non erano visibili).|
|Griglia dei risultati|Correzione di un problema che generava un'eccezione "Indice fuori intervallo" quando si faceva clic sulla griglia.|
|Griglia dei risultati|Risolto un problema in cui è il colore di sfondo della griglia risultati veniva ignorato. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035/suggestions/32895916](https://feedback.azure.com/forums/908035/suggestions/32895916). |
|Showplan|Vengono visualizzate le proprietà del nuovo operatore di concessione memoria quando esiste più di un thread.|
|Showplan|Aggiungere i seguenti 4 attributi in RunTimeCountersPerThread del piano di esecuzione xml: HpcRowCount (numero di righe elaborate da un dispositivo *hpc*), HpcKernelElapsedUs (tempo di attesa scaduto per l'esecuzione del kernel in uso), HpcHostToDeviceBytes (byte trasferiti dall'host al dispositivo) e HpcDeviceToHostBytes (byte trasferiti dal dispositivo all'host).|
|Showplan|Correzione di un problema per cui i nodi di piano simili vengono evidenziati nella posizione errata.|
|SMO|Correzione di un problema per cui SMO/ServerConnection non gestisce correttamente le connessioni basate su SqlCredential. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035-sql-server/suggestions/33698941](https://feedback.azure.com/forums/908035-sql-server/suggestions/33698941). |
|SMO|Correzione di un problema per cui un'applicazione scritta con SQL Server Management Objects (SMO) genera un errore se si tenta di enumerare i database dallo stesso server in più thread, anche se usa istanze di SqlConnection diverse in ogni thread.|
|SMO|Correzione della regressione delle prestazioni durante il trasferimento da tabelle esterne.|
|SMO|Correzione del problema nel thread safety ServerConnection a causa del quale l'interfaccia SMO perdeva istanze di SqlConnection con la destinazione Azure.|
|SMO|Correzione di un problema che causava un errore StringBuilder.FormatError durante il tentativo di ripristinare un database con parentesi graffe nel nome.|
|SMO|Correzione di un problema per cui i database di Azure in SMO usano per impostazione predefinita le regole di confronto senza distinzione tra maiuscole e minuscole per tutti i confronti tra stringhe invece di usare le regole di confronto specificate per il database.|
|Editor SSMS|Correzione di un problema a causa del quale nella "Tabella di sistema SQL" il ripristino dei colori predefiniti impostava il colore verde limone invece del verde predefinito, rendendo difficile la lettura su uno sfondo bianco. Per informazioni dettagliate, vedere l'articolo sul [ripristino del colore predefinito errato per la tabella di sistema SQL](https://feedback.azure.com/forums/908035-sql-server/suggestions/32896906). Questo problema persiste ancora nelle versioni di SSMS non in lingua inglese.|
|Editor SSMS|Correzione di un problema a causa del quale IntelliSense non funzionava quando si usava l'autenticazione di Azure Active Directory (Azure AD) per connettersi ad Azure SQLDW.|
|Editor SSMS|Correzione di un problema di IntelliSense in Azure quando l'utente non ha accesso al database **master**.|
|Editor SSMS|Corretti frammenti di codice per la creazione di "tabelle temporanee" che venivano interrotti quando le regole di confronto del database di destinazione facevano distinzione tra maiuscole e minuscole.|
|Editor SSMS|Nuova funzione TRANSLATE ora riconosciuta da IntelliSense. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035-sql-server/suggestions/32898430](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898430). |
|Editor SSMS|Funzionalità IntelliSense migliorata nella funzione FORMAT predefinita. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035-sql-server/suggestions/32898676](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898676). |
|Editor SSMS|LAG e LEAD vengono ora riconosciuti come funzioni predefinite. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035-sql-server/suggestions/32898757](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898757). |
|Editor SSMS|Correzione di un problema per cui IntelliSense genera un avviso quando si usa "ALTER TABLE...ADD CONSTRAINT...WITH(ONLINE=ON)".|
|Editor SSMS|Correzione di un problema a causa del quale varie viste di sistema e funzioni con valore di tabella non erano colorate correttamente.|
|Editor SSMS|Correzione di un problema per cui quando si fa clic su una scheda dell'editor, questa si chiude invece di ricevere lo stato attivo. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035/suggestions/37291114](https://feedback.azure.com/forums/908035/suggestions/37291114). |
|Opzioni SSMS|Correzione di un problema per cui la pagina **Strumenti** > **Opzioni** > **Esplora oggetti di SQL Server** > **Comandi** non veniva ridimensionata correttamente.|
|Opzioni SSMS|Per impostazione predefinita, SSMS ora disabilita il download automatico di DTD nell'editor XMLA. L'editor di script XMLA usa il servizio di linguaggio XML e per impostazione predefinita impedisce ora di scaricare automaticamente DTD per file XMLA potenzialmente dannosi. Questo comportamento viene controllato disattivando l'impostazione "Scarica automaticamente le DTD e gli schemi" in **Strumenti** > **Opzioni** > **Ambiente** > **Editor di testo** > **XML** > **Varie**.|
|Opzioni SSMS|Ripristinata la scelta rapida da tastiera **CTRL+D**, disponibile nella versione precedente di SSMS. Per informazioni dettagliate, vedere [https://feedback.azure.com/forums/908035/suggestions/35544754](https://feedback.azure.com/forums/908035/suggestions/35544754). |
|Progettazione tabelle|Correzione di un arresto anomalo in "Modifica 200 righe".|
|Progettazione tabelle|Correzione di un problema per cui la finestra di progettazione consente l'aggiunta di una tabella durante la connessione a un database Azure SQL.|
|Valutazione della vulnerabilità|Correzione di un problema per cui i risultati dell'analisi non vengono caricati correttamente.|
|XEvent|Aggiunte due colonne "action_name" e "class_type_desc" che visualizzano i campi ID azione e tipo classe come stringhe leggibili.|
|XEvent|Rimosso il limite massimo di 1.000.000 di eventi per il visualizzatore XEvent.|
|Profiler XEvent|Correzione di un problema per cui il profiler XEvent non veniva avviato se connesso a un server SQL Server a 96 nuclei.|
|Visualizzatore XEvent|Correzione di un problema per cui il visualizzatore XEvent si arresta in modo anomalo quando si prova a raggruppare gli eventi usando le opzioni della barra degli strumenti Eventi estesi.|

#### <a name="deprecated-and-removed-features-in-180"></a>Funzionalità deprecate e rimosse nella versione 18.0

Di seguito sono indicate le funzionalità deprecate e rimosse da SSMS versione 18.0.

- Debugger Transact-SQL
- Diagrammi di database
- Gli strumenti seguenti non vengono più installati con SSMS:
  - OSQL.EXE
  - DReplay.exe
  - SQLdiag.exe
  - SSBDiagnose.exe
  - bcp.exe
  - sqlcmd.exe
- Strumenti di Configuration Manager:
  - Gestione configurazione SQL Server e Gestione configurazione server di report non sono più inclusi nell'installazione di SSMS.
- Criteri standard DMF
  - I criteri non vengono più installati con SSMS. Vengono spostati in Git. Gli utenti possono contribuire a svilupparli e scaricarli/installarli in base alle esigenze.
- Opzione della riga di comando -P di SSMS rimossa
  - A causa di problemi di sicurezza, l'opzione per specificare password non crittografate nella riga di comando è stata rimossa.
- Rimozione dell'opzione Genera script > Pubblica su servizio Web
  - Questa funzionalità deprecata è stata rimossa dall'interfaccia utente di SSMS.
- Rimozione del nodo "Manutenzione > Legacy" di Esplora oggetti.
  - I nodi obsoleti "Piani di manutenzione del database" e "SQL Mail" non saranno più accessibili. I nodi recenti "Posta elettronica database" e "Piani di manutenzione" funzionano come di consueto.

#### <a name="known-issues-180"></a>Problemi noti (18.0)

- Si potrebbe verificare un problema durante l'installazione della versione 18.0, per cui non è possibile eseguire SQL Server Management Studio. Se si verifica questo problema, seguire la procedura descritta nell'articolo [SSMS2018: installato, ma non viene eseguito](https://feedback.azure.com/forums/908035-sql-server/suggestions/37502512-ssms2018-installed-but-will-not-run).

- Esiste una limitazione alle dimensioni dei dati visualizzati dai risultati di SSMS in griglia, testo o file

- Esistono problemi di aggiornamento durante il passaggio tra finestre di query diverse. Per altri dettagli, vedere questa [segnalazione di un utente per SQL Server](https://feedback.azure.com/forums/908035/suggestions/37474042). Una soluzione alternativa per questo problema è disabilitare l'accelerazione hardware in Strumenti > Opzioni.

### <a name="1791"></a>17.9.1

![download](media/download-icon.png) [Scaricare SSMS 17.9.1](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x409)

- Numero di versione: 17.9.1
- Numero di build: 14.0.17289.0
- Data di rilascio: 21 novembre 2018

#### <a name="bug-fixes-in-1791"></a>Correzioni di bug nella versione 17.9.1

- Correzione di un problema a causa del quale la connessione degli utenti veniva chiusa e riaperta ogni volta che veniva chiamata una query durante l'uso dell'autenticazione "Azure Active Directory - Universale con supporto MFA" con l'editor di query SQL. Gli effetti collaterali della chiusura della connessione includevano il rilascio imprevisto delle tabelle temporanee e talvolta l'assegnazione di un nuovo SPID alla connessione.
- Risolto un problema che persisteva da tempo per cui il piano di ripristino poteva non trovare o generare un piano di ripristino inefficiente in determinate condizioni.
- Correzione di un problema nella procedura guidata "Importa applicazione livello dati" che poteva causare un errore durante la connessione a un database SQL di Azure.

> [!NOTE]
> Le versioni di SSMS 17.x localizzate in lingue diverse dall'inglese richiedono il [pacchetto di aggiornamento della sicurezza KB 2862966](https://support.microsoft.com/kb/2862966) se sono installate in Windows 8, Windows 7, Windows Server 2012 e Windows Server 2008 R2.

[Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x804)| [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x404)| [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x409)| [Francese](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x40c)| [Tedesco](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x407)| [Italiano](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x410)| [Giapponese](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x411)| [Coreano](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x412)| [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x416)| [Russo](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x419)| [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x40a)

#### <a name="uninstall-and-reinstall-ssms-17x"></a>Disinstallare e reinstallare SSMS 17.x

Se si verificano problemi durante l'installazione di SSMS che non vengono risolti con una disinstallazione e reinstallazione standard, è possibile provare a [ripristinare](https://support.microsoft.com/help/4028054/windows-10-repair-or-remove-programs) Visual Studio 2015 Isolated Shell. Se il ripristino di Visual Studio 2015 Isolated Shell non risolve il problema, la procedura seguente consente di risolvere molti problemi casuali:

1. Disinstallare SSMS nello stesso modo in cui si disinstalla qualsiasi applicazione, usando *App e funzionalità*, *Programmi e funzionalità*, a seconda della versione di Windows in uso.

2. Disinstallare Visual Studio 2015 Isolated Shell **da un prompt dei comandi con privilegi elevati**:

    ```PUSHD "C:\ProgramData\Package Cache\FE948F0DAB52EB8CB5A740A77D8934B9E1A8E301\redist"```

    ```vs_isoshell.exe /Uninstall /Force /PromptRestart```

3. Disinstallare Microsoft Visual C++ 2015 Redistributable nello stesso modo in cui si disinstalla un'applicazione qualsiasi. Disinstallare sia la versione x86 che la x64 se sono entrambe presenti nel computer.

4. Reinstallare Visual Studio 2015 Isolated Shell **da un prompt dei comandi con privilegi elevati**:  

    ```PUSHD "C:\ProgramData\Package Cache\FE948F0DAB52EB8CB5A740A77D8934B9E1A8E301\redist"```  

    ```vs_isoshell.exe /PromptRestart```

5. Reinstallare SSMS.

6. Eseguire l'aggiornamento alla [versione più recente di Visual C++ 2015 Redistributable](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) se non è già stato fatto.

### <a name="1653"></a>16.5.3

![download](media/download-icon.png) [Scaricare SSMS 16.5.3](https://go.microsoft.com/fwlink/?LinkID=840946)

- Numero di versione: 16.5.3  
- Numero di build: 13.0.16106.4  
- Data di rilascio: 30 gennaio 2017

[Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x804)| [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x404)| [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x409)| [Francese](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x40c)| [Tedesco](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x407)| [Italiano](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x410)| [Giapponese](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x411)| [Coreano](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x412)| [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x416)| [Russo](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x419)| [Spagnolo](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x40a)

#### <a name="bug-fixes-in-1653"></a>Correzioni di bug nella versione 16.5.3

- Correzione di un problema introdotto in SSMS 16.5.2 che causava l'espansione del nodo 'Tabella' quando la tabella aveva più di una colonna di tipo sparse.

- Gli utenti possono distribuire pacchetti SSIS contenenti Gestione connessione OData per connettere una risorsa Microsoft Dynamics AX/CRM Online al catalogo SSIS. Per altre informazioni dettagliate, vedere [Gestione connessione OData](../integration-services/connection-manager/odata-connection-manager.md).

- La configurazione di Always Encrypted per una tabella esistente ha esito negativo con errori per gli oggetti correlati. [ID Connect 3103181](https://connect.microsoft.com/SQLServer/feedback/details/3103181/setting-up-always-encrypted-on-an-existing-table-fails-with-errors-on-unrelated-objects)

- La configurazione di Always Encrypted per un database esistente con più schemi non funziona. [ID Connect 3109591](https://connect.microsoft.com/SQLServer/feedback/details/3109591/sql-server-2016-always-encrypted-against-existing-database-with-multiple-schemas-doesnt-work)

- La procedura guidata Always Encrypted, Colonna crittografata ha esito negativo a causa del database che contiene viste che fanno riferimento a viste di sistema. [ID Connect 3111925](https://connect.microsoft.com/SQLServer/feedback/details/3111925/sql-server-2016-always-encrypted-encrypted-column-wizard-failed-task-failed-due-to-following-error-cannot-save-package-to-file-the-model-has-build-blocking-errors)

- Durante la crittografia con Always Encrypted, gli errori derivanti dall'aggiornamento dei moduli dopo la crittografia non vengono gestiti in modo corretto.

- Il menu *Apri recenti* non mostra i file salvati di recente. [ID Connect 3113288](https://connect.microsoft.com/SQLServer/feedback/details/3113288/ssms-2016-open-recent-menu-doesnt-show-recently-saved-files)

- SSMS è lento quando si fa clic con il pulsante destro del mouse su un indice per una tabella (tramite una connessione Internet remota). [ID Connect 3114074](https://connect.microsoft.com/SQLServer/feedback/details/3114074/ssms-slow-when-right-clicking-an-index-for-a-table-over-a-remote-internet-connection)

- Risolto un problema con la barra di scorrimento di SQL Designer. [ID Connect 3114856](https://connect.microsoft.com/SQLServer/feedback/details/3114856/bug-in-scrollbar-on-sql-desginer-in-ssms-2016)

- Il menu di scelta rapida per le tabelle smette temporaneamente di rispondere

- SSMS in alcuni casi genera eccezioni in Monitoraggio attività e subisce un arresto anomalo. [ID Connect 697527](https://connect.microsoft.com/SQLServer/feedback/details/697527/)

- Si verifica un arresto anomalo di SSMS 2016 con l'errore "Il processo è stato terminato a causa di un errore interno del runtime .NET in IP 71AF8579 (71AE0000) con codice di uscita 80131506"

## <a name="additional-downloads"></a>Download aggiuntivi

Per un elenco di tutti i download di SQL Server Management Studio, vedere [Area download Microsoft](https://www.microsoft.com/download/search.aspx?q=sql%20server%20management%20studio&p=0&r=10&t=&s=Relevancy~Descending).  
  
Per la versione più recente di SQL Server Management Studio, vedere [Scaricare SQL Server Management Studio &#40;SSMS&#41;](../ssms/download-sql-server-management-studio-ssms.md).