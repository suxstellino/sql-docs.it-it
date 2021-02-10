---
title: Domande frequenti su Azure Data Studio
description: Risposte alle domande frequenti su Azure Data Studio, ad esempio "Che funzioni ha?", "Chi lo deve usare?" e "Qual è il costo?".
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: alayu, maghan
ms.custom: seodec18
ms.date: 10/28/2020
ms.openlocfilehash: 56d3f751fdae52fa0ff94aafe94d8ba955b418ec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040191"
---
# <a name="azure-data-studio-faq"></a>Domande frequenti su Azure Data Studio

## <a name="what-is-azure-data-studio"></a>Che cos'è Azure Data Studio?

[Azure Data Studio](what-is-azure-data-studio.md) è un ambiente desktop multipiattaforma open source per professionisti della gestione dei dati che usano la famiglia Azure Data di piattaforme dati locali e cloud in Windows, macOS e Linux. Precedentemente rilasciato con il nome di anteprima di SQL Operations Studio, Azure Data Studio offre un'esperienza di modifica moderna per la gestione dei dati tra più origini con funzionalità IntelliSense veloce, frammenti di codice, integrazione del controllo del codice sorgente e un terminale integrato. Azure Data Studio è stato progettato in base alle esigenze dell'utente della piattaforma dati, con grafici predefiniti di set di risultati delle query e dashboard personalizzabili.

La ricerca ha dimostrato che gli utenti dedicano alla modifica delle query molto più tempo rispetto a qualsiasi altra attività con SQL Server Management Studio. Per questo motivo, Azure Data Studio è stato progettato con una particolare attenzione alle funzionalità più usate, con esperienze aggiuntive disponibili come estensioni facoltative del prodotto. Ogni utente può personalizzare il proprio ambiente per i flussi di lavoro che usa più spesso.

## <a name="how-much-does-azure-data-studio-cost"></a>Quanto costa Azure Data Studio?

Azure Data Studio è gratuito per uso privato o commerciale.

## <a name="who-should-use-azure-data-studio"></a>Chi dovrebbe usare Azure Data Studio?

Chiunque può usare Azure Data Studio. Tuttavia, è progettato per semplificare le attività eseguite da sviluppatori di database, amministratori di database, amministratori di sistema e fornitori di software indipendenti.

## <a name="what-can-i-do-with-azure-data-studio"></a>Cosa si può fare con Azure Data Studio?

Azure Data Studio è basato su Visual Studio Code e offre un'esperienza di codifica leggera, moderna e incentrata sulla tastiera quando si usano SQL Server, il database SQL di Azure e Azure Synapse Analytics. Azure Data Studio semplifica le attività quotidiane grazie a funzionalità integrate quali finestre a più schede, un editor SQL avanzato, IntelliSense, completamento delle parole chiave, frammenti di codice, esplorazione del codice e integrazione del controllo del codice sorgente (Git e TFS). È possibile eseguire query su richiesta, visualizzare e salvare i risultati in formato testo, JSON o Excel, modificare i dati, organizzare e gestire le connessioni di database preferite e sfogliare gli oggetti di database in un'esperienza di esplorazione familiare.

Nella finestra del terminale integrato all'interno dell'interfaccia utente di Azure Data Studio si possono usare i propri strumenti da riga di comando preferiti, ad esempio Bash, PowerShell, sqlcmd, bcp, psql e ssh. È semplice generare ed eseguire script di creazione e inserimento per gli oggetti di database, per creare copie del database a scopo di sviluppo o test. I frammenti di codice intelligenti e le ricche esperienze grafiche per creare nuovi database e oggetti di database (come tabelle, viste, stored procedure, utenti, accessi, ruoli e così via), nonché aggiornare gli oggetti di database esistenti, aumentano la produttività. È possibile usare dashboard avanzati e personalizzabili per monitorare e risolvere rapidamente i colli di bottiglia delle prestazioni nei database in locale, in Azure o in qualunque cloud.

Azure Data Studio offre un'esperienza coerente per eseguire il backup e il ripristino dei database. Grazie al supporto pianificato per i gruppi di disponibilità Always On di SQL Server, è possibile configurare, monitorare e risolvere facilmente i problemi relativi ai gruppi di disponibilità per i database di SQL Server cruciali ed eseguire rapidamente il failover in un database secondario durante un'emergenza. Azure Data Studio è stato progettato per migliorare la produttività nel ciclo di vita DevOps per i database e nei sistemi operativi scelti dagli utenti. Di conseguenza, si ha sempre il pieno controllo ed è possibile ridurre i rischi, risolvere i problemi più rapidamente e fornire continuamente una soluzione di valore che supera le aspettative dei clienti.

## <a name="is-azure-data-studio-open-source"></a>Azure Data Studio è open source?

Il codice sorgente per Azure Data Studio e per i suoi provider di dati è disponibile su GitHub. Il codice sorgente per il front-end [Azure Data Studio](https://github.com/microsoft/azuredatastudio) (basato su Visual Studio Code) è disponibile in base a un contratto di licenza che concede il diritto di modificare e usare il software, ma non di ridistribuirlo o di ospitarlo in un servizio cloud. Il codice sorgente per i provider di dati è disponibile in base ai termini della licenza MIT all'indirizzo [https://github.com/Microsoft/sqltoolsservice](https://github.com/Microsoft/sqltoolsservice).

## <a name="do-we-plan-to-open-source-ssms"></a>SQL Server Management Studio verrà reso open source?

No.

Tuttavia, gli strumenti CLI e GUI di nuova generazione per più sistemi operativi sono open source. Ad esempio, l'estensione mssql per VS Code, mssql-scripter, e msql-CLI sono tutti open source su GitHub. Il codice sorgente di Azure Data Studio è disponibile su GitHub.  

## <a name="now-that-theres-azure-data-studio-does-microsoft-plan-to-deprecate-ssms-and-ssdt"></a>Ora che esiste Azure Data Studio, Microsoft prevede di deprecare SSMS e SSDT? 

No.

Gli investimenti negli strumenti principali di Windows (SSMS, SSDT, PowerShell) proseguiranno di pari passo con la nuova generazione di strumenti CLI e GUI multi-database e multi-sistema operativo. L'obiettivo è offrire ai clienti la possibilità di scegliere di usare gli strumenti che preferiscono, sulle piattaforme più idonee per i propri scenari. Azure Data Studio è maggiormente incentrato sulle esperienze di modifica delle query e sviluppo dati, che secondo la ricerca è la capacità di gran lunga più usata in SQL Server Management Studio. Come estensioni di Azure Data Studio sono anche disponibili altre funzionalità amministrative di valore elevato, ad esempio backup, ripristino, gestione dei processi agente e profilatura dei server. Azure Data Studio è multipiattaforma, per cui consente agli utenti di lavorare sulla propria piattaforma preferita. SQL Server Management Studio, tuttavia, offre ancora la più ampia gamma di funzioni amministrative e rimane lo strumento di punta per le attività di gestione della piattaforma. 

## <a name="when-should-i-use-azure-data-studio-or-sql-server-management-studio"></a>Quando è consigliabile usare Azure Data Studio o SQL Server Management Studio?

*Usare Azure Data Studio se:*

- Ci si occupa per lo più di modificare o eseguire query.
- È necessario poter creare grafici e visualizzare set di risultati rapidamente.
- Si può eseguire la maggior parte delle attività amministrative tramite il terminale integrato usando sqlcmd o PowerShell.
- Si ha una necessità minima di esperienze di tipo procedura guidata.
- Non è necessario eseguire operazioni approfondite di configurazione amministrativa o della piattaforma.
- L'ambiente di esecuzione è macOS o Linux.

*Usare SQL Server Management Studio se:*

- Si eseguono operazioni approfondite di configurazione amministrativa o della piattaforma.
- Ci si occupa di gestione della sicurezza, incluse gestione degli utenti, valutazione della vulnerabilità e configurazione delle funzionalità di sicurezza.
- È necessario usare dashboard e procedure guidate di ottimizzazione delle prestazioni.
- Si usano diagrammi di database e progettazione tabelle.
- È necessario accedere ai server registrati.
- Si usano statistiche sulle query dinamiche o statistiche client.

## <a name="feature-comparison"></a>Confronto tra le funzionalità

Per altri dettagli sulle differenze tra Azure Data Studio e SQL Server Management Studio (SSMS), vedere [Informazioni su Azure Data Studio](what-is-azure-data-studio.md#feature-comparison-with-sql-server-management-studio-ssms).


## <a name="what-if-azure-data-studio-is-missing-a-feature-that-is-in-ssmsssdt"></a>Cosa accade se in Azure Data Studio manca una funzionalità che si trova in SSMS/SSDT?

Dipende dallo scenario e dalle esigenze del cliente o dell'azienda. Per spingerne l'aggiunta, è consigliabile inviare un suggerimento o votarne uno esistente in [GitHub](https://github.com/microsoft/azuredatastudio/issues).

## <a name="i-understand-azure-data-studio-and-the-mssql-extension-for-vs-code-are-powered-by-a-new-tools-service-that-uses-smo-apis-under-the-covers-is-smo-available-on-linux-and-macos"></a>Ho compreso che Azure Data Studio e l'estensione mssql per VS Code sono basate su un nuovo servizio strumenti che, dietro le quinte, usa le API SMO. SMO è disponibile in Linux e macOS?

Le API SMO non sono ancora disponibili in Linux o macOS in un formato utilizzabile. Un subset delle API SMO necessarie per Azure Data Studio è stato convertito in .NET Core ed è prevista un'espansione nell'ambito della roadmap. Il servizio SQL Tools è su GitHub: [https://github.com/Microsoft/sqltoolsservice](https://github.com/Microsoft/sqltoolsservice).

## <a name="do-you-plan-to-port-the-dacfx-apis-andor-sqlpackageexe-andor-ssdt-to-linux-and-macos"></a>È prevista la conversione delle API DACFx e/o di sqlpackage.exe e/o di SSDT per Linux e macOS?

Sì.

[SqlPackage.exe](../tools/sqlpackage/sqlpackage-download.md) è ora disponibile in .NET Core per Windows, macOS e Linux.  La funzionalità Progetti SQL (SSDT) è abilitata in Azure Data Studio nell'[estensione dei progetti di database SQL](extensions/sql-database-project-extension.md).

## <a name="will-sql-powershell-cmdlets-be-available-on-linux-and-macos"></a>I cmdlet SQL PowerShell saranno disponibili in Linux e macOS?

SQL PowerShell è attualmente disponibile in PowerShell Gallery ed è possibile usarlo in Windows per lavorare con SQL Server in esecuzione ovunque, incluso SQL in Linux. L'offerta dei cmdlet SQL PowerShell in Linux e macOS è prevista nella roadmap. Per contribuire alla definizione delle priorità, presentare un suggerimento su [GitHub](https://github.com/microsoft/azuredatastudio/issues).

## <a name="who-usually-uses-azure-data-studio"></a>Chi usa in genere Azure Data Studio?

I normali utenti di Azure Data Studio sono sviluppatori e amministratori di database.

## <a name="does-azure-data-studio-integrate-with-azure-synapse-analytics"></a>Azure Data Studio si integra con Azure Synapse Analytics?

Sì.

Il supporto di Azure Data Studio per Azure Synapse Analytics è attualmente disponibile in anteprima, insieme a Istanza gestita di SQL di Azure e ai Big Data di SQL Server 2019.

## <a name="why-is-azure-data-studio-important-for-big-data-scenarios"></a>Perché Azure Data Studio è importante per gli scenari di Big Data?

Man mano che SQL Server estende le proprie funzionalità nello spazio dei Big Data, sono necessari nuovi strumenti per supportare questi casi d'uso. Per questo motivo, Azure Data Studio include una nuova esperienza per i Big Data di SQL Server, che comprende un'esperienza di tipo notebook nel set di strumenti SQL Server e una nuova creazione guidata di tabella esterna, che consente di accedere ai dati da istanze remote di SQL Server e Oracle in modo semplice e veloce.

## <a name="can-i-use-visual-studio-code-vs-code-extensions-with-azure-data-studio"></a>È possibile usare le estensioni di Visual Studio Code (VS Code) con Azure Data Studio?

Sì.

Tuttavia, non tutte le estensioni di VS Code hanno una controparte in Azure Data Studio.

## <a name="next-steps"></a>Passaggi successivi
- [Informazioni su Azure Data Studio](what-is-azure-data-studio.md)
- [Scaricare Azure Data Studio](download-azure-data-studio.md)