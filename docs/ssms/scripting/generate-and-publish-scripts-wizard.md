---
title: Genera e pubblica script
description: Informazioni su come usare la procedura guidata Genera e pubblica script per creare script per il trasferimento di un database tra istanze di database. Le istanze possono essere istanze del motore di database di SQL Server o del database SQL di Azure.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- sql9.swb.generatescriptswizard.chooseviews.f1
- sql13.swb.generatescriptswizard.manageproviders.f1
- sql9.swb.generatescriptswizard.scriptwizarddescription.f1
- sql9.swb.generatescriptswizard.choosedefaults.f1
- sql13.swb.generatescriptswizard.summarypage.f1
- sql13.swb.generatescriptswizard.providerconfiguration.f1
- sql9.swb.generatescriptswizard.chooseuddt.f1
- sql9.swb.generatescriptswizard.chooserules.f1
- sql13.swb.generatescriptswizard.introduction.f1
- sql13.swb.generatescriptswizard.setscriptingoptions.f1
- sql13.swb.generatescriptswizard.saveorpublishscripts.f1
- sql9.swb.generatescriptswizard.progress.f1
- sql9.swb.generatescriptswizard.chooseobjects.f1
- sql9.swb.generatescriptswizard.welcome.f1
- sql9.swb.generatescriptswizard.scriptfileoption.f1
- sql13.swb.generatescriptswizard.chooseobjects.f1
- sql13.swb.generatescriptswizard.advancedpublishingoptions.f1
- sql9.swb.generatescriptswizard.selectdatabase.f1
- sql9.swb.generatescriptswizard.choosetables.f1
- sql9.swb.generatescriptswizard.choosestoredprocedures.f1
- sql9.swb.generatescriptswizard.chooseobjecttypes.f1
- sql13.swb.generatescriptswizard.advancedscriptingoptions.f1
- sql9.swb.generatescriptswizard.choosescriptoptions.f1
- sql9.swb.generatescriptswizard.chooseudf.f1
helpviewer_keywords:
- databases [SQL Server], publishing
- publishing databases
- scripts [SQL Server], generating
- scripts [SQL Server], publishing
- databases [SQL Server], generating scripts
- Publish Database Wizard
ms.assetid: 5ee520ba-ec7e-4199-a441-189e9e264b37
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 04/07/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e7a24570538161a8b0196e9c7d8ee20af9e32587
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271636"
---
# <a name="generate-and-publish-scripts-wizard"></a>Genera e pubblica script

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

È possibile usare la procedura guidata **Genera e pubblica script** per creare script per il trasferimento di un database tra le istanze del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] o di [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]. È possibile generare script per un database in un'istanza del motore di database nella rete locale o da [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Gli script generati possono essere eseguiti in un'altra istanza del motore di database o in [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. È inoltre possibile usare la procedura guidata per pubblicare direttamente il contenuto di un database in un servizio Web creato tramite Database Publishing Services. È possibile creare script per un intero database o limitare la creazione a oggetti specifici.

Per un'esercitazione più dettagliata sull'uso della procedura guidata Genera e pubblica script, vedere [Esercitazione: Procedura guidata Genera script](../tutorials/scripting-ssms.md#script-databases).

## <a name="before-you-begin"></a>Prima di iniziare

I database di origine e di destinazione possono trovarsi in [!INCLUDE[ssSDS](../../includes/sssds-md.md)]o in un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] eseguita in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o versione successiva.

### <a name="publishing-to-a-hosted-service"></a><a name="PubHostSvc"></a> Pubblicazione in un servizio ospitato

Oltre a creare script, la procedura guidata **Genera e pubblica script** può essere usata per pubblicare un database in un tipo specifico di servizio Web di SQL Server ospitato. SQL Server Hosting Toolkit include Database Publishing Services come progetto di origine condiviso su CodePlex. Il progetto Database Publishing Services può essere usato dai provider di hosting Web per compilare un set di servizi Web per facilitare la distribuzione di database nel servizio Web da parte dei clienti. Per altre informazioni sul download di SQL Server Hosting Toolkit, vedere [Database Publishing Services di SQL Server](https://go.microsoft.com/fwlink/?LinkId=142025).

Per pubblicare un database in un servizio di hosting Web, selezionare l'opzione **Pubblica su servizio Web** nella pagina **Imposta opzioni di generazione script** della procedura guidata.

### <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni

L'autorizzazione minima per pubblicare un database è l'appartenenza al ruolo predefinito del database db_ddladmin per il database di origine. L'autorizzazione minima per pubblicare uno script del database in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al provider di hosting è l'appartenenza al ruolo predefinito del database db_ddladmin sul database di destinazione.

È inoltre necessario fornire un nome utente e una password per accedere all'account del provider di hosting ed eseguire la pubblicazione guidata. Il database di destinazione deve essere creato nel provider di hosting prima della pubblicazione del database di origine. La pubblicazione sovrascrive gli oggetti presenti nel database esistente.

## <a name="using-the-generate-and-publish-scripts-wizard"></a><a name="GenPubScriptWiz"></a> Utilizzo della procedura guidata Genera e pubblica script

**Per generare o pubblicare uno script**

1. In **Esplora oggetti** espandere il nodo dell'istanza contenente il database per il quale generare lo script.

2. Scegliere **Attività** e selezionare **Genera script**.

    ![Procedura guidata Genera script](media/generate-and-publish-scripts-wizard/generate-scripts.png)

3. Completare le finestre di dialogo della procedura guidata.

    - [Pagina Introduzione](#Introduction)
    - [Pagina Seleziona oggetti](#ChooseObjects)
    - [Pagina Imposta opzioni di generazione script](#SetScriptOpt)
    - [Pagina Opzioni di generazione script avanzate](#AdvScriptOpt)
    - [Pagina Riepilogo](#Summary)
    - [Pagina Salva o pubblica script](#SavePubScripts)

###  <a name="introduction-page"></a><a name="Introduction"></a> Pagina Introduzione

In questa pagina vengono descritti i passaggi per la generazione o la pubblicazione di uno script.

**Non visualizzare più questa pagina** : consente di non visualizzare più questa pagina al successivo avvio della procedura guidata **Genera e pubblica script**.

  ![Pagina Introduzione](media/generate-and-publish-scripts-wizard/intro.png)

### <a name="choose-objects-page"></a><a name="ChooseObjects"></a> Pagina Seleziona oggetti

Usare questa pagina per scegliere quali oggetti si desidera includere negli script generati dalla procedura guidata. Nella pagina successiva della procedura guidata è possibile salvare gli script nel percorso desiderato oppure usarli per pubblicare oggetti di database su un provider di hosting Web remoto in cui è installato [Database Publishing Services di SQL Server](https://go.microsoft.com/fwlink/?LinkId=142025).

**Opzione Genera script per intero database**: selezionare questa opzione per generare script per tutti gli oggetti del database e includere uno script per il database stesso.

   ![Script per tutti i database](media/generate-and-publish-scripts-wizard/script-all.png)

**Seleziona oggetti di database specifici**: selezionare questa opzione per limitare la generazione degli script solo agli oggetti specifici nel database prescelto:

- **Oggetti di database** : consente di selezionare almeno un oggetto da includere nello script.

- **Seleziona tutto** : consente di selezionare tutte le caselle di controllo disponibili.

- **Deseleziona tutto** : consente di deselezionare tutte le caselle di controllo. Per continuare, selezionare almeno un oggetto di database.

   ![Script per database specifico](media/generate-and-publish-scripts-wizard/script-specific-objects.png)

### <a name="set-scripting-options-page"></a><a name="SetScriptOpt"></a> Pagina Imposta opzioni di generazione script

Usare questa pagina per specificare se si desidera che tramite la procedura guidata gli script vengano salvati nel percorso prescelto o vengano usati per pubblicare oggetti di database in un provider di hosting Web remoto. Per eseguire la pubblicazione è necessario avere l'accesso a un servizio Web installato usando il servizio Web Database Publishing Services.

**Opzioni** : se si vuole che gli script vengano salvati in un percorso specifico, selezionare **Salva script in un percorso specifico**. È possibile eseguire in un secondo momento gli script in a un'istanza del motore di database o in [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Se si desidera pubblicare gli oggetti di database in un provider di hosting Web remoto tramite una procedura guidata, selezionare **Pubblica su servizio Web**.

**Salva script in un percorso specifico**: consente di salvare uno o più file script Transact-SQL in un percorso specificato dall'utente.

![Save as notebook (Salva come notebook)](media/generate-and-publish-scripts-wizard/save.png)

- **[Save as notebook](../../azure-data-studio/notebooks/notebooks-guidance.md)** (Salva come notebook): consente di salvare lo script in uno o più file con estensione sql. Selezionare il pulsante Sfoglia ( **…** ) per specificare il nome e il percorso del file.

- **Salva come script**: consente di salvare lo script in uno o più file con estensione sql. Selezionare il pulsante Sfoglia **(...)** per specificare il nome e il percorso del file. Selezionare la casella di controllo **Sovrascrivi file esistente** per sostituire il file se ne esiste già uno con lo stesso nome. Selezionare **Genera script in un singolo file** o **One script file per object** (Un file script per oggetto) per specificare come devono essere generati gli script. Selezionare **Testo Unicode** o **Testo ANSI** per specificare il tipo di testo che deve essere usato nello script.

- **Salva negli Appunti** : consente di salvare lo script Transact-SQL negli Appunti.

- **Apri in nuova finestra Query**: consente di generare lo script in una finestra dell'editor di query del motore di database. Se non vi sono finestre dell'editor aperte, viene aperta una nuova finestra dell'editor da usare come destinazione dello script.

- **Avanzate** : consente di visualizzare la finestra di dialogo **Opzioni di pubblicazione avanzate** in cui è possibile selezionare opzioni avanzate per la pubblicazione di script.

- **Provider** : consente di selezionare il provider che specifica le informazioni di connessione per il servizio di hosting Web che ospita il database nel quale pubblicare gli oggetti selezionati. È necessario avere almeno un provider nella finestra di dialogo **Gestisci provider** per selezionare un provider.

- **Database di destinazione** : consente di selezionare il database di destinazione nel quale pubblicare gli oggetti selezionati. È necessario selezionare un provider prima di selezionare un database di destinazione.

### <a name="advanced-scripting-options-page"></a><a name="AdvScriptOpt"></a> Pagina Opzioni di generazione script avanzate

Usare questa pagina per specificare come si desidera che vengano generati gli script tramite la procedura guidata. Sono disponibili molte opzioni diverse. Le opzioni sono disattivate se non sono supportate dalla versione di SQL Server o di [!INCLUDE[ssSDS](../../includes/sssds-md.md)] specificata in **Tipo di motore di database**.

![Opzioni avanzate](media/generate-and-publish-scripts-wizard/advanced.png)

**Opzioni** : consente di specificare le opzioni avanzate selezionando un valore tra quelli disponibili nell'elenco visualizzato a destra di ciascuna opzione.

**Generale**: le opzioni seguenti si applicano all'intero script.

- **Riempimento ANSI** : consente di includere **ANSI PADDING ON** nello script. Il valore predefinito è **True**.

- **Accoda a file** : se è impostato su **True**, questo script viene aggiunto alla fine di uno script esistente specificato nella pagina **Imposta opzioni di generazione script** . Se è impostato su **False**, il nuovo script sovrascrive uno script precedente. Il valore predefinito è **False**.

- **Verifica esistenza oggetto** - Quando **True** aggiunge la verifica dell'esistenza prima di generare l'istruzione di creazione per gli oggetti SQL. Ad esempio: tabelle, viste, funzioni o stored procedure. Per l'istruzione CREATE viene eseguito il wrapping in un'istruzione IF. Se si sa che la destinazione è pulita, lo script è molto più pulito. Se si prevede che gli oggetti NON esistano nella destinazione, si riceve un errore. Il valore predefinito è **False**.

- **Continua generazione script in caso di errore**: se è impostato su **False**, la generazione di script viene arrestata in caso di errore. Se è impostato su **True**, la generazione di script continua. Il valore predefinito è **False**.

- **Converti UDDT in tipi di base** : se è impostato su **True**, i tipi di dati definiti dall'utente (UDDT) vengono convertiti nei tipi di dati di base sottostanti che erano stati usati per crearli. Usare **True** se il tipo di dati UDDT non esiste nel database in cui viene eseguito lo script. Se è impostato su **False**, vengono usati gli UDDT. Il valore predefinito è **False**.

- **Genera script per oggetti dipendenti** : consente di generare uno script per un oggetto la cui presenza è necessaria quando viene eseguito lo script per l'oggetto selezionato. Il valore predefinito è **True**.

- **Includi intestazioni descrittive** : se è impostato su **True**, allo script vengono aggiunti commenti descrittivi dividendolo in sezioni per ogni oggetto. Il valore predefinito è **False**.

- **Includi IF NOT EXISTS** : se è impostato su **True**, lo script include un'istruzione che verifica se l'oggetto esiste già nel database e non prova a creare un nuovo oggetto se già presente. Il valore predefinito è **False**.

- **Includi nomi di vincoli di sistema**: se è impostato su **False**, i valori predefiniti dei vincoli nominati automaticamente nel database di origine vengono rinominati nel database di destinazione. Se è impostato su **True**, i vincoli hanno lo stesso nome nel database di origine e in quello di destinazione.

- **Includi istruzioni non supportate** : se è impostato su **False**, lo script non contiene istruzioni per oggetti che non sono supportati nella versione del server o nel tipo di motore selezionati. Se impostato su **Vero**, lo script contiene gli oggetti non supportati. Ogni istruzione per un oggetto non supportato è accompagnata da un commento, che specifica che è necessario modificare l'istruzione prima di poter eseguire lo script con la versione di SQL Server o il tipo di motore selezionati. Il valore predefinito è **False**.

- **Schema per qualifica dei nomi degli oggetti** : consente di includere il nome dello schema nel nome degli oggetti che vengono creati. Il valore predefinito è **True**.

- **Genera script per associazioni** : consente di generare uno script per l'associazione di oggetti predefiniti e oggetti della regola. Il valore predefinito è **False**. Per altre informazioni, vedere [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md) e [CREATE RULE &#40;Transact-SQL&#41;](../../t-sql/statements/create-rule-transact-sql.md).

- **Script per regole di confronto** : consente di includere nello script le informazioni sulle regole di confronto. Il valore predefinito è **False**. Per altre informazioni, vedere [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md).

- **Script per valori predefiniti** : consente di includere nelle colonne della tabella gli oggetti predefiniti usati per impostare i valori predefiniti. Il valore predefinito è **True**. Per altre informazioni, vedere [CREATE DEFAULT &#40;Transact-SQL&#41;](../../t-sql/statements/create-default-transact-sql.md).

- **Genera script per DROP e CREATE** : se è impostato su **Genera script per CREATE**, vengono incluse le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] per creare oggetti. Se impostato su **Genera script per DROP**, vengono incluse le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] per eliminare oggetti. Se è impostato su **Genera script per DROP e CREATE**, nello script viene inclusa l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] di eliminazione seguita dall'istruzione di creazione per ogni oggetto inserito nello script. Il valore predefinito è **Genera script per CREATE**.

- **Script per proprietà estese** : consente di includere le proprietà estese nello script, se presenti nell'oggetto. Il valore predefinito è **True**.

- **Script per il tipo di motore di database** : consente di creare uno script che può essere eseguito nel tipo selezionato di [!INCLUDE[ssSDS](../../includes/sssds-md.md)] o di un'istanza del motore di database di SQL Server. Oggetti non supportati sul tipo specificato non sono inclusi nello script. Il tipo predefinito è quello del server di origine.

- **Script per versione server** : consente di creare uno script eseguibile nella versione selezionata di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per le nuove funzionalità non è possibile generare script per le versioni precedenti. La versione predefinita è quella del server di origine.

- **Script per account di accesso** : se l'oggetto per il quale generare uno script è un utente di database, questa opzione consente di creare gli account di accesso da cui l'utente dipende. Il valore predefinito è **False**.

- **Script per autorizzazioni a livello oggetto** : consente di includere script per l'impostazione dell'autorizzazione per gli oggetti del database. Il valore predefinito è **False**.

- **Script per statistiche** : se è impostato su **Script per statistiche**, questa opzione include l'istruzione **CREATE STATISTICS** per ricreare le statistiche per l'oggetto. L'opzione **Genera script per statistiche e istogrammi** consente inoltre di creare informazioni sugli istogrammi. Il valore predefinito è **Non generare script per statistiche**. Per altre informazioni, vedere [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md).

- **Script per USE DATABASE** : consente di aggiungere l'istruzione **USE DATABASE** allo script. Per verificare che gli oggetti di database vengano creati nel database corretto, includere l'istruzione **USE DATABASE** . Se si prevede di usare lo script in un database diverso, selezionare **False** per omettere l'istruzione **USE DATABASE** . Il valore predefinito è **True**. Per altre informazioni, vedere [USE &#40;Transact-SQL&#41;](../../t-sql/language-elements/use-transact-sql.md).

- **Tipi di dati per cui generare lo script**: consente di selezionare cosa includere negli script, ovvero **Solo dati**, **Solo schema** o entrambi. Il valore predefinito è **Solo schema**.

**Opzioni tabella/vista** : le opzioni seguenti si applicano solo agli script per tabelle o viste.

- **Genera script per il rilevamento modifiche** : consente di generare script per il rilevamento delle modifiche se è abilitato nel database di origine o nelle tabelle del database di origine. Il valore predefinito è **False**. Per altre informazioni, vedere [Informazioni sul rilevamento delle modifiche &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-tracking-sql-server.md).

- **Script per vincoli CHECK**: consente di aggiungere vincoli **CHECK** allo script. Il valore predefinito è **True**. I vincoli **CHECK** richiedono che i dati immessi in una tabella rispettino una condizione specificata. Per altre informazioni, vedere [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md).

- **Genera script per le opzioni di compressione dati** : consente di includere le opzioni di compressione dati, se sono configurate nel database di origine o nelle tabelle del database di origine. Per altre informazioni, vedere [Data Compression](../../relational-databases/data-compression/data-compression.md). Il valore predefinito è **False**.

- **Script per chiavi esterne** : consente di aggiungere chiavi esterne allo script. Il valore predefinito è **True**. Le chiavi esterne indicano e impongono le relazioni tra tabelle.

- **Script per indici full-text** : consente di generare script per la creazione di indici full-text. Il valore predefinito è **False**.

- **Script per indici** : consente di generare script per la creazione di indici. Il valore predefinito è **True**. Gli indici consentono di trovare dati rapidamente.

- **Script per chiavi primarie** : consente di generare script per la creazione di chiavi primarie nelle tabelle. Il valore predefinito è **True**. Le chiavi primarie identificano in modo univoco ogni riga di una tabella.

- **Script per trigger** : consente di generare script per la creazione di trigger DML nelle tabelle. Il valore predefinito è **False**. Un trigger DML è un'azione programmata per l'esecuzione quando si verifica un evento DML (Data Manipulation Language) nel server di database. Per altre informazioni, vedere [DML Triggers](../../relational-databases/triggers/dml-triggers.md).

- **Script per chiavi univoche** : consente di generare script per la creazione di chiavi univoche nelle tabelle. Le chiavi univoche impediscono l'immissione di dati duplicati. Il valore predefinito è **True**. Per altre informazioni, vedere [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md).

### <a name="summary-page"></a><a name="Summary"></a> Pagina Riepilogo

![Riepilogo Genera script](media/generate-and-publish-scripts-wizard/summary.png)

In questa pagina vengono riepilogate le opzioni selezionate nella procedura guidata. Per modificare un'opzione, selezionare **Indietro**. Per avviare la generazione di script da salvare o pubblicare, selezionare **Avanti**.

**Verificare le selezioni** : consente di visualizzare le opzioni selezionate per ogni pagina della procedura guidata. Espandere un nodo per visualizzare le opzioni selezionate per la pagina corrispondente.

### <a name="save-or-publish-scripts-page"></a><a name="SavePubScripts"></a> Pagina Salva o pubblica script  

Usare questa pagina per monitorare lo stato di avanzamento della procedura guidata durante l'esecuzione.

**Dettagli** : consente di visualizzare la colonna **Azione** per vedere lo stato di avanzamento della procedura guidata. Dopo avere generato gli script, la procedura guidata li salva in un file o li usano per la pubblicazione su un servizio Web, a seconda delle selezioni. Al termine di ogni passaggio selezionare il valore nella colonna **Risultato** per vedere il risultato del passaggio corrispondente.

**Salva report**: selezionare questa opzione per salvare i risultati dello stato della procedura guidata in un file.

**Annulla**: selezionare questa opzione per chiudere la procedura guidata prima che venga completata o se si verifica un errore.

**Fine**: selezionare questa opzione per chiudere la procedura guidata dopo che è stata completata o se si verifica un errore.

### <a name="save-scripts"></a>Salva script

![Finish](media/generate-and-publish-scripts-wizard/save-scripts-finish.png)

Se tutte le impostazioni sono corrette, la configurazione viene completata correttamente.

## <a name="generating-scripts-on-azure-synapse-analytics"></a>Generazione di script in Azure Synapse Analytics

Se la sintassi generata tramite "Script come..." non corrisponde alla sintassi di [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] o se viene visualizzato un messaggio di errore, può essere necessario impostare le opzioni di scripting in SQL Server Management Studio su [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)].

### <a name="how-to-set-default-scripting-options-to-sql-data-warehouse"></a>Come impostare le opzioni di scripting su SQL Data Warehouse

Per generare script di oggetti con la sintassi di [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] , impostare l'opzione di scripting predefinita su [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] come indicato di seguito:

1. Selezionare **Strumenti** e quindi **Opzioni**.
2. In **Opzioni generali di scripting** impostare:
    1. Script per il tipo di motore di database: **Database SQL di Microsoft Azure**.
    2. Script per l'edizione del motore di database: **Edizione Microsoft Azure SQL Data Warehouse**.
3. Selezionare **OK**.

### <a name="how-to-generate-scripts-for-sql-data-warehouse-when-it-is-not-the-default-scripting-option"></a>Come generare script per SQL Data Warehouse quando non è l'opzione di scripting predefinita

Se si imposta [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] come opzione di scripting predefinita come illustrato in precedenza, è possibile ignorare queste istruzioni. Se tuttavia si sceglie di usare opzioni di scripting predefinite diverse, è possibile riscontrare un errore. Per evitare errori, seguire questa procedura per generare e pubblicare script per [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)]:

1. Fare clic con il pulsante destro del mouse sul database SQL Data Warehouse.
2. Selezionare **Genera script**.
3. Scegliere gli oggetti per i quali generare script.
4. In **Opzioni di scripting** selezionare **Avanzate**. In **Generale** impostare:
    1. Script per il tipo di motore di database: **Database SQL di Microsoft Azure**.
    2. Script per l'edizione del motore di database: **Edizione Microsoft Azure SQL Data Warehouse**.
5. Selezionare **Salva o pubblica script** e quindi **Fine**.

Le opzioni impostate nel passaggio 4 non vengono memorizzate. Se si vuole che queste opzioni vengano memorizzate, seguire le istruzioni in **Come impostare le opzioni di scripting su SQL Data Warehouse**.

## <a name="see-also"></a>Vedere anche

- [Installazione di SMO](../../relational-databases/server-management-objects-smo/installing-smo.md)

- [Copiare database in altri server](../../relational-databases/databases/copy-databases-to-other-servers.md)