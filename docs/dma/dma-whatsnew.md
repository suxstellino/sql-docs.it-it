---
title: Novità di Data Migration Assistant (SQL Server) | Microsoft Docs
description: Informazioni sulle nuove funzionalità di ogni versione di Data Migration Assistant per SQL Server e per il database SQL di Azure.
ms.custom: ''
ms.date: 11/05/2019
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, new features
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.openlocfilehash: a8f7e74a44b489f22827c42bab2bd5ee60a7658a
ms.sourcegitcommit: 038a3a730cf088a64d55501d9605f2f6b31d573d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2021
ms.locfileid: "105616670"
---
# <a name="whats-new-in-data-migration-assistant"></a>Novità di Data Migration Assistant

Questo articolo elenca le aggiunte in ogni versione di Data Migration Assistant.

## <a name="data-migration-assistant-v-53"></a>Data Migration Assistant v 5,3

La versione v 5.3 del Data Migration Assistant fornisce supporto per:
- Valutazione della query ad hoc per l'applicazione esterna nella riga di comando. 
- Consente agli utenti di personalizzare il parametro timeout del comando nel file di configurazione Dma.exe.config.
- File di esempio di configurazione della valutazione e script di esempio di PowerShell, che spiegano come usare il file di configurazione per eseguire la riga di comando DMA.
- Operazioni di backup e ripristino nella migrazione locale senza limitazione del timeout.

Inoltre, questa versione di Data Migration Assistant è stata aggiornata a .NET 4,8 e fornisce correzioni di bug per i problemi seguenti:
- Non è possibile eseguire la valutazione o connettersi al database SQL di Azure senza l'autorizzazione sysadmin nella fase di migrazione.
- Gli utenti non possono eseguire la valutazione senza autorizzazione sysadmin per i test di Servizi Desktop remoto.
- Sono stati rilevati problemi durante il caricamento dei report di valutazione JSON in Azure Migrate.
- Impossibile eseguire la migrazione degli oggetti contenenti caratteri cinesi.

## <a name="data-migration-assistant-v-52"></a>Data Migration Assistant v 5,2

La versione 5.2 del Data Migration Assistant fornisce supporto per:
- Il caricamento di valutazioni per Azure Migrate con supporto per Azure per enti pubblici e cloud nazionali (offerta sovrana).  Questa funzionalità consente a di valutare l'idoneità della migrazione di SQL Server Data estate a SQL di Azure.
- Supporto della riga di comando per il caricamento di valutazioni per Azure Migrate con supporto per Azure per enti pubblici e cloud nazionali.  A questo punto, è possibile automatizzare completamente il caricamento delle valutazioni nel progetto di Azure migrate per ottenere un report di conformità SQL di Azure consolidato. 

## <a name="data-migration-assistant-v-50"></a>Data Migration Assistant v 5,0

La versione 5.0 del Data Migration Assistant fornisce supporto per:

- SQL Server 2019 per Windows e SQL Server 2019 per Linux come destinazioni per la valutazione e l'aggiornamento.
- Salvataggio e caricamento delle valutazioni, incluso il supporto per il salvataggio e il caricamento di valutazioni create nelle versioni precedenti del Data Migration Assistant.
- Valutazione dei progetti di SQL Server Integration Services (SSIS) ospitati in pacchetti SSISDB e SSIS ospitati nell'archivio pacchetti. Il Migration Assistant di database rileva le funzionalità non supportate, parzialmente supportate o deprecate e i problemi di compatibilità utilizzati nei pacchetti di origine e fornisce indicazioni utili per risolvere tali problemi.
- Valutazione delle query SQL da un'applicazione esterna, ad esempio query SQL nel codice sorgente C#. Gli utenti possono usare Data Access Migration Toolkit per generare un report JSON completo per le query SQL usate nel codice sorgente C# e quindi caricare il report in Data Migration Assistant.

Inoltre, questa versione di Data Migration Assistant fornisce ulteriori miglioramenti e correzioni di bug e lo strumento è stato aggiornato a 4.7.2 .NET.

## <a name="data-migration-assistant-v45"></a>Data Migration Assistant v 4.5

La versione 4.5 di Data Migration Assistant fornisce supporto per la valutazione della migrazione dei pacchetti di SQL Server Integration Services (SSIS) ospitati nel file System al database SQL di Azure o a SQL Istanza gestita.

## <a name="data-migration-assistant-v44"></a>Data Migration Assistant v 4.4

La versione 4.4 di Data Migration Assistant fornisce supporto per il caricamento di valutazioni in Azure Migrate.

## <a name="data-migration-assistant-v43"></a>Data Migration Assistant v 4.3

La versione 4.3 di Data Migration Assistant offre supporto per:

- Raccomandazioni per gli SKU per Istanza gestita SQL di Azure in base alla valutazione del carico di lavoro.
- RDS SQL Server come origine per le valutazioni.
- Valutazioni dei processi dell'agente per il Istanza gestita SQL di Azure come destinazione.
- Possibilità di ignorare determinate regole di valutazione; l'elenco dei codici di errore specificati nella proprietà' ignoreErrorCodes ' configurata in DMA non verrà visualizzato nei risultati della valutazione DMA.
- Valutazione delle query T-SQL nei passaggi delle attività dei processi e fornire consigli appropriati
- Valutazioni degli eventi estesi (anteprima pubblica).

Inoltre, questa versione di DMA fornisce prestazioni migliorate per la gestione di un numero elevato di oggetti dello schema nei database, oltre a correzioni di bug relative a:

- Procedure compilate con la compilazione nativa, in alcuni casi.
- Schemi di database complicati.

## <a name="data-migration-assistant-v42"></a>Data Migration Assistant v 4.2

La versione 4.2 di Data Migration Assistant fornisce supporto della riga di comando per la valutazione della conformità della destinazione per una o più istanze del server quando si esegue la migrazione da SQL Server locale a una Istanza gestita SQL. I clienti possono ora usare la riga di comando Data Migration Assistant per raccogliere i metadati relativi allo schema del database, rilevare i blocchi e ottenere informazioni sulle funzionalità parzialmente supportate o non supportate che influiscono sulla migrazione a una Istanza gestita SQL. I risultati possono quindi essere sottoposti a rendering usando il modello di Power BI fornito.

## <a name="data-migration-assistant-v41"></a>Data Migration Assistant v 4.1

La versione 4.1 di Data Migration Assistant introduce il supporto per la valutazione completa dei database SQL Server locali che eseguono la migrazione a SQL Istanza gestita.

Il flusso di lavoro di valutazione consente di rilevare i seguenti problemi che possono influire sulla migrazione a SQL Istanza gestita:

- **Funzionalità non supportate o parzialmente supportate**. Data Migration Assistant valuta il database SQL Server di origine per le funzionalità in uso che sono parzialmente supportate o non supportate nella Istanza gestita SQL di destinazione. Lo strumento offre quindi un set completo di indicazioni, approcci alternativi disponibili in Azure e procedure di mitigazione in modo che i clienti possano prendere in considerazione queste informazioni durante la pianificazione dei progetti di migrazione.

- **Problemi di compatibilità**. Data Migration Assistant inoltre identifica i problemi di compatibilità correlati alle aree seguenti:

  - Modifiche di rilievo: gli oggetti dello schema specifici che potrebbero compromettere la migrazione della funzionalità al database di destinazione.  Si consiglia di correggere questi oggetti dello schema dopo la migrazione del database.
  - Modifiche comportamentali: gli oggetti dello schema segnalati possono continuare a funzionare, ma possono presentare un comportamento diverso, ad esempio una riduzione delle prestazioni.
  - Problemi informativi: questi oggetti non avranno alcun effetto sulla migrazione, ma potrebbero essere stati deprecati dalla funzionalità SQL Server versioni.

Al termine della valutazione, usare il [servizio migrazione del database di Azure](https://azure.microsoft.com/services/database-migration/) (DMS) per eseguire la migrazione dei database di SQL Server a SQL istanza gestita.  DMS supporta le migrazioni di database sia [offline](/azure/dms/tutorial-sql-server-to-managed-instance) (monouso) che [online](/azure/dms/tutorial-sql-server-managed-instance-online) (minimo tempo di inattività) a SQL istanza gestita.

## <a name="data-migration-assistant-v40"></a>Data Migration Assistant v 4.0

La versione 4.0 di Data Migration Assistant introduce la funzionalità di consigli per lo SKU del database SQL di Azure, che consente agli utenti di identificare la SKU minima consigliata del database SQL di Azure in base ai contatori delle prestazioni raccolti dai computer che ospitano i database. Questa funzionalità fornisce consigli relativi al piano tariffario, al livello di calcolo e alle dimensioni massime dei dati, oltre ai costi stimati al mese. Offre inoltre la possibilità di eseguire il provisioning di tutti i database in Azure in blocco.

> [!NOTE]
> Questa funzionalità è attualmente disponibile solo tramite l'interfaccia della riga di comando (CLI).

Per altri dettagli, vedere l'articolo [identificare lo SKU del database SQL di Azure appropriato per il database locale](dma-sku-recommend-sql-db.md).

## <a name="data-migration-assistant-v36"></a>Data Migration Assistant v 3.6

La versione 3.6 di Data Migration Assistant introduce la correzione automatica per gli oggetti dello schema interessati dai blocchi di migrazione più comuni.

Questa versione fornisce Autofix per il blocco di migrazione e i problemi di modifica del comportamento seguenti:

- Oggetti dello schema che usano la sintassi di join non qualificata.
- Oggetti dello schema che usano l'istruzione RAISEERROR legacy.
- Istruzioni SQL che usano order by Integer Literal.

Data Migration Assistant esegue la conversione automatica dello schema per gli oggetti interessati dai problemi elencati e chiede conferma all'utente prima di procedere con la conversione dello schema. Gli utenti possono esaminare le modifiche del codice suggerite e quindi accettare o rifiutare tutte le conversioni per un determinato oggetto di database.

Data Migration Assistant usa la tecnologia Microsoft Program Synthesis (prosa) per suggerire le correzioni del codice. Scopri di più su [prosa](https://microsoft.github.io/prose/).

## <a name="data-migration-assistant-v35"></a>Data Migration Assistant v 3.5

La versione 3.5 di Data Migration Assistant include le aggiunte seguenti:

- Miglioramenti significativi delle prestazioni per la migrazione al database SQL di Azure (i test di benchmark indicano che il processo è quattro volte più veloce rispetto alle versioni precedenti di Data Migration Assistant).
- Il footprint di memoria è ulteriormente ottimizzato per migliorare la stabilità del flusso di lavoro di migrazione.
- Possibilità di ignorare le valutazioni durante le migrazioni dello schema e dei dati (se la valutazione è già stata eseguita e sono stati risolti tutti gli oggetti dello schema di rilievo prima della migrazione).
- Correzione per risolvere un problema con lo strumento che si arresta in modo anomalo quando viene fornito un percorso di condivisione di rete non valido per i file di backup, quando si esegue un aggiornamento di una versione legacy di SQL Server locale a una versione successiva o di SQL Server in macchine virtuali di Azure.

## <a name="data-migration-assistant-v34"></a>Data Migration Assistant v 3.4

La versione 3.4 di Data Migration Assistant include le aggiunte seguenti:

- Supporto per SQL Server 2017 come origine per le migrazioni nel database SQL di Azure.
- Miglioramenti alla correttezza delle regole di stabilità, prestazioni e valutazione.

## <a name="data-migration-assistant-v33"></a>Data Migration Assistant v 3.3

La versione 3.3 di Data Migration Assistant consente la migrazione di un'istanza di SQL Server locale alla nuova versione di SQL Server 2017, sia in Windows che in Linux. Anche se il flusso di lavoro di migrazione complessivo per Windows e Linux è lo stesso, il passaggio a SQL Server 2017 per Linux richiede alcune considerazioni aggiuntive.

### <a name="specifying-the-back-up-path"></a>Specifica del percorso di backup

Linux e Windows usano formati di percorso diversi. Di conseguenza, per eseguire la migrazione a SQL Server 2017 in Linux è necessario che l'utente fornisca le versioni Windows e Linux del percorso al percorso del file fisico. È possibile specificare entrambe le versioni del percorso in modi diversi a seconda della posizione del file fisico.
Se il file di backup fisico si trova in un computer che esegue:

- Linux, usare una condivisione "Samba" per condividere il file con altri computer della rete.
- Usare il comando "mnt" per montare la condivisione nel computer che esegue Linux.

> [!NOTE]
> I dettagli dell'uso di una condivisione ' Samba ' o del comando ' mnt ' esulano dall'ambito di questo articolo.

### <a name="migrating-windows-logins"></a>Migrazione degli account di accesso di Windows

Sebbene la migrazione degli account di accesso di Active Directory (AD) sia ufficialmente supportata da SQL Server 2017 in Linux, è necessario che la configurazione aggiuntiva funzioni correttamente. Per informazioni dettagliate sulla configurazione degli account di accesso Active Directory in SQL Server 2017 in Linux, vedere l'articolo [Active Directory autenticazione con SQL Server in Linux](../linux/sql-server-linux-active-directory-authentication.md) . Dopo aver eseguito la configurazione richiesta, l'installazione è stata completata ed è possibile eseguire la migrazione di Active Directory accessi come di consueto. L'autenticazione SQL standard funziona come previsto senza alcuna configurazione aggiuntiva.

## <a name="data-migration-assistant-v32"></a>Data Migration Assistant v 3.2

La versione 3.2 di Data Migration Assistant include le aggiunte seguenti:

- La migrazione dello schema e dei dati è abilitata dai database SQL Server locali al database SQL di Azure con un nuovo flusso di lavoro di migrazione.
- Durante la migrazione dello schema al database SQL di Azure, DMA esegue lo script degli oggetti di database di origine, fornisce informazioni aggiuntive su come risolvere i potenziali problemi di compatibilità e quindi distribuisce lo schema in Azure.

## <a name="data-migration-assistant-v31"></a>Data Migration Assistant v 3.1

La versione 3.1 di Data Migration Assistant include le aggiunte seguenti:

- Suggerimenti per la valutazione migliorati per il database SQL di Azure in termini di regole di confronto del database, uso di stored procedure di sistema non supportate e di oggetti CLR.
- Guida alla valutazione per i livelli di compatibilità 130, 120, 110 e 100 durante la migrazione al database SQL di Azure.

## <a name="data-migration-assistant-v30"></a>Data Migration Assistant v 3.0

La versione 3.0 di Data Migration Assistant estende la valutazione del database SQL di Azure per fornire consigli completi per la risoluzione dei problemi relativi a:

- Problemi di blocco della migrazione.
- Funzionalità e funzioni parzialmente o non supportate.

## <a name="data-migration-assistant-v21"></a>Data Migration Assistant v 2.1

La versione 2.1 di Data Migration Assistant include le aggiunte seguenti:

- Supporto della riga di comando per l'esecuzione di valutazioni in modalità automatica, che consente di eseguire valutazioni su larga scala. Per altri dettagli, vedere l'articolo [eseguire Data Migration Assistant dalla riga di comando](dma-commandline.md).
- Miglioramenti delle prestazioni quando gli utenti avviano e chiudono DMA.
- Possibilità di configurare il timeout della connessione SQL. Per ulteriori dettagli, vedere l'articolo [relativo alle impostazioni di configurazione per Data Migration Assistant](dma-configurationsettings.md).

## <a name="data-migration-assistant-v20"></a>Data Migration Assistant v 2.0

La versione 2.0 di Data Migration Assistant include consigli sulle funzionalità di stretch database migliorati per fornire tabelle con priorità appropriate per ottimizzare il risparmio di spazio di archiviazione.

## <a name="data-migration-assistant-v10"></a>Data Migration Assistant v 1.0

La versione 1.0 di Data Migration Assistant è la versione iniziale e fornisce:

- Individuazione di problemi che possono influire sull'aggiornamento a una versione locale di SQL Server. I risultati vengono descritti come problemi di compatibilità e sono suddivisi in categorie nelle aree seguenti:
  - Modifiche che causano un'interruzione
  - Modifiche del comportamento
  - Funzionalità deprecate
- Individuazione di nuove funzionalità nella piattaforma SQL Server di destinazione di cui il database può trarre vantaggio dopo un aggiornamento. I risultati sono descritti come raccomandazioni sulle funzionalità e sono suddivisi in categorie nelle aree seguenti:
  - Prestazioni
  - Sicurezza
  - Archiviazione
- Esperienza utente moderna per eseguire valutazioni.

## <a name="see-also"></a>Vedi anche

[Panoramica di Data Migration Assistant](../dma/dma-overview.md)
