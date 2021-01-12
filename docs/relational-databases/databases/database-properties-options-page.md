---
title: Proprietà database (pagina Opzioni) | Microsoft Docs
description: Informazioni su come usare la scheda Opzioni della finestra di dialogo Proprietà database per visualizzare o modificare le regole di confronto, il modello di recupero e altre impostazioni del database.
ms.custom: ''
ms.date: 08/28/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.databaseproperties.options.f1
ms.assetid: a3447987-5507-4630-ac35-58821b72354d
author: stevestein
ms.author: sstein
ms.openlocfilehash: 87c3b10b95a37c74ea1595dca15341aeffe5c261
ms.sourcegitcommit: 4a813a0741502c56c0cd5ecaafafad2e857a9d7f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/08/2021
ms.locfileid: "98031102"
---
# <a name="database-properties-options-page"></a>Proprietà database (pagina Opzioni)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Utilizzare questa pagina per visualizzare o modificare le opzioni per il database selezionato. Per altre informazioni sulle opzioni disponibili in questa pagina, vedere [Opzioni di ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) e [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).  
  
## <a name="page-header"></a>Intestazione di pagina  
 **Regole di confronto**  
 È possibile specificare le regole di confronto del database selezionandole nell'elenco. Per altre informazioni, vedere [Set or Change the Database Collation](../../relational-databases/collations/set-or-change-the-database-collation.md).  
  
 **Modello di recupero**  
 È possibile specificare uno dei modelli di recupero del database seguenti: **Con registrazione completa**, **Con registrazione minima delle operazioni bulk** o **Con registrazione minima**. Per altre informazioni sui modelli di recupero, vedere [Modelli di recupero &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md).  
  
 **Livello di compatibilità**  
 È possibile specificare la versione più recente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supportata dal database. Per i valori possibili, vedere [Livello di compatibilità di ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md). Quando un database SQL Server viene aggiornato, il livello di compatibilità per il database viene mantenuto, se possibile. Oppure viene portato al livello minimo supportato per il nuovo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
 **Tipo di contenimento**  
 È possibile specificare nessuno o parziale per determinare se si tratta di un database indipendente. Per altre informazioni sui database indipendenti, vedere [Contained Databases](../../relational-databases/databases/contained-databases.md). La proprietà del server **Abilita database indipendenti** deve essere impostata su **TRUE** prima che un database possa essere configurato come indipendente.  
  
> [!IMPORTANT]  
>  L'abilitazione dei delegati di database parzialmente indipendenti controlla l'accesso all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per i proprietari del database. Per altre informazioni, vedere [Security Best Practices with Contained Databases](../../relational-databases/databases/security-best-practices-with-contained-databases.md).  
  
## <a name="automatic"></a>Automatico  
 **Chiusura automatica**  
 Specifica se il database viene chiuso correttamente e se le risorse corrispondenti vengono liberate dopo la disconnessione dell'ultimo utente. I valori possibili sono **True** e **False**. Quando il valore è **True**, il database viene chiuso correttamente e le relative risorse vengono liberate dopo la disconnessione dell'ultimo utente.  

 **Creazione automatica di statistiche incrementali**  
 Specificare se utilizzare l'opzione incrementale nella creazione di statistiche per partizione. Per informazioni sulle statistiche incrementali, vedere [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md).  
  
 **Creazione automatica statistiche**  
 Indica se il database crea automaticamente le statistiche di ottimizzazione mancanti. I valori possibili sono **True** e **False**. Quando il valore è **True**, le statistiche mancanti necessarie per l'ottimizzazione di una query vengono create automaticamente durante la fase di ottimizzazione. Per altre informazioni, vedere [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md).  
  
 **Compattazione automatica**  
 Indica se i file di database sono disponibili per la compattazione periodica. I valori possibili sono **True** e **False**. Per altre informazioni, vedere [Shrink a Database](../../relational-databases/databases/shrink-a-database.md).  
  
 **Aggiornamento automatico statistiche**  
 Indica se il database aggiorna automaticamente le statistiche di ottimizzazione non aggiornate. I valori possibili sono **True** e **False**. Quando il valore è **True**, tutte le statistiche non aggiornate necessarie per l'ottimizzazione di una query vengono create automaticamente durante la fase di ottimizzazione. Per altre informazioni, vedere [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md).  
  
 **Aggiornamento automatico asincrono statistiche**  
 Quando il valore è **True**, le query che avviano un aggiornamento automatico delle statistiche non aggiornate non attendono il completamento dell'aggiornamento delle statistiche prima della compilazione. Le query successive usano le statistiche aggiornate, non appena disponibili.  
  
 Quando il valore è **False**, le query che avviano un aggiornamento automatico delle statistiche non aggiornate attenderanno che le statistiche aggiornate diventino disponibili per l'uso nel piano di ottimizzazione query.  
  
 L'impostazione di questa opzione su **True** non produce effetti a meno che anche l'opzione **Aggiornamento automatico statistiche** non sia impostata su **True**.  

## <a name="azure"></a>Azure
Quando si è connessi al database SQL di Azure, questa sezione include impostazioni per controllare l'obiettivo del livello di servizio. L'obiettivo del livello di servizio predefinito per un nuovo database è Standard S2.

  **Obiettivo di livello di servizio corrente**: l'obiettivo del livello di servizio specifico da usare. I valori validi sono vincolati dall'edizione selezionata. Se il valore dell'obiettivo di livello di servizio che si vuole selezionare non è presente nell'elenco, è possibile digitarlo.

  **Edizione**: l'edizione del database SQL di Azure da usare, ad esempio Basic o Premium. Se il valore dell'edizione che si vuole selezionare non è presente nell'elenco, è possibile digitarlo ma deve corrispondere al valore usato nell'API REST di Azure.
  
  **Dimensioni massime**: le dimensioni massime del database. Se il valore delle dimensioni massime che si vuole selezionare non è presente nell'elenco, è possibile digitarlo. Lasciare vuoto il campo per le dimensioni predefinite dell'edizione e dell'obiettivo del livello di servizio specificati.
  
## <a name="containment"></a>Containment  
 In un database indipendente alcune impostazioni che in genere sono configurate a livello di server possono essere configurate a livello di database.  
  
 **LCID lingua full-text predefinita**  
 Specifica una lingua predefinita per le colonne con indicizzazione full-text. L'analisi linguistica dei dati con indicizzazione full-text dipende dalla lingua dei dati. Il valore predefinito per questa opzione corrisponde alla lingua impostata per il server. Per la lingua corrispondente all'impostazione visualizzata, vedere [sys.fulltext_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md).  
  
 **Lingua predefinita**  
 Lingua predefinita per tutti i nuovi utenti del database indipendente, salvo altrimenti specificato.  
  
 **Trigger annidati abilitati**  
 Consente l'attivazione di trigger da altri trigger. I trigger possono essere nidificati fino a un massimo di 32 livelli. Per altre informazioni, vedere la sezione relativa ai trigger annidati in [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
 **Trasforma parole non significative**  
 Evita la visualizzazione di un messaggio di errore qualora, a causa di parole non significative, un'operazione booleana su una query full-text restituisca zero righe. Per altre informazioni, vedere [transform noise words Server Configuration Option](../../database-engine/configure-windows/transform-noise-words-server-configuration-option.md).  
  
 **Cambio data per anno a due cifre**  
 Indica il numero più alto che può essere immesso come anno a due cifre. L'anno indicato e i 99 anni precedenti possono essere immessi con due cifre. Tutti gli altri anni devono essere immessi con quattro cifre.  
  
 Ad esempio, l'impostazione predefinita 2049 indica che la data '14/03/49' verrà interpretata come 14 marzo 2049, mentre la data '14/03/50' verrà interpretata come 14 marzo 1950. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server two-digit year cutoff](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).  
  
## <a name="cursor"></a>Cursore  
 **Chiusura cursori dopo commit abilitata**  
 Specifica se i cursori vengono chiusi dopo l'esecuzione del commit della transazione di apertura del cursore. I valori possibili sono **True** e **False**. Quando il valore è **True**, vengono chiusi tutti i cursori che risultano aperti al momento dell'esecuzione del commit o del rollback di una transazione. Quando il valore è **False**, tali cursori rimangono aperti quando viene eseguito il commit della transazione. Quando il valore è **False**, il rollback di una transazione determina la chiusura di tutti i cursori, ad eccezione di quelli definiti come INSENSITIVE o STATIC. Per altre informazioni, vedere [SET CURSOR_CLOSE_ON_COMMIT &#40;Transact-SQL&#41;](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md).  
  
 **Cursore predefinito**  
 Indica il comportamento del cursore predefinito. Se **True**, le dichiarazioni dei cursori vengono impostate su LOCAL per impostazione predefinita. Se **False**, i cursori [!INCLUDE[tsql](../../includes/tsql-md.md)] vengono automaticamente impostati su GLOBAL.  
  
## <a name="database-scoped-configurations"></a>Configurazioni con ambito database  
 In SQL Server 2016 e nel database SQL Azure sono presenti numerose proprietà di configurazione il cui ambito può essere limitato al livello di database. Per altre informazioni su tutte queste impostazioni, vedere [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).  
  
 **Stima di cardinalità legacy**  
 Specificare il modello di stima di cardinalità di Query Optimizer per il database primario indipendentemente dal livello di compatibilità del database. Equivale a [Flag di traccia 9481](https://support.microsoft.com/kb/2801413).  
  
 **Stima di cardinalità legacy per database secondari**  
 Specificare il modello di stima di cardinalità di Query Optimizer per gli eventuali database secondari indipendentemente dal livello di compatibilità del database. Equivale a [Flag di traccia 9481](https://support.microsoft.com/kb/2801413).  
  
 **Massimo grado di parallelismo**  
 Specificare l'impostazione [MAXDOP](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) predefinita per il database primario da usare per le istruzioni.  
  
 **Massimo grado di parallelismo per database secondario**  
 Specificare l'impostazione [MAXDOP](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) predefinita per gli eventuali database secondari da usare per le istruzioni.  
  
 **Analisi dei parametri**  
 Abilita o disabilita l'analisi dei parametri sul database primario. Equivale a [Flag di traccia 4136](https://support.microsoft.com/kb/980653).  
  
 **Analisi dei parametri per i database secondari**  
 Abilita o disabilita l'analisi dei parametri sugli eventuali database secondari. Equivale a [Flag di traccia 4136](https://support.microsoft.com/kb/980653).  
  
 **Correzioni di Query Optimizer**  
 Abilita o disabilita gli hotfix di ottimizzazione query sul database primario indipendentemente dal livello di compatibilità del database. Equivale a [Flag di traccia 4199](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md). Per altri dettagli, vedere [QUERY_OPTIMIZER_HOTFIXES](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md#qo_hotfixes).  
  
 **Correzioni di Query Optimizer per database secondari**  
 Abilita o disabilita gli hotfix di ottimizzazione query sugli eventuali database secondari indipendentemente dal livello di compatibilità del database. Equivale a [Flag di traccia 4199](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md). Per altri dettagli, vedere [QUERY_OPTIMIZER_HOTFIXES](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md#qo_hotfixes).  
  
## <a name="filestream"></a>FILESTREAM  
 **Nome di directory FILESTREAM**  
 Specifica il nome di directory per i dati FILESTREAM associati al database selezionato.  
  
 **Accesso FILESTREAM non in transazioni**  
 È possibile specificare una delle opzioni seguenti per l'accesso non transazionale tramite il file system a dati FILESTREAM archiviati in tabelle FileTable: **OFF**, **READ_ONLY** o **FULL**. Se FILESTREAM non è abilitato nel server, questo valore viene impostato su OFF ed è disabilitato. Per altre informazioni, vedere [FileTables &#40;SQL Server&#41;](../../relational-databases/blob/filetables-sql-server.md).  
  
## <a name="miscellaneous"></a>Varie  
**Consenti isolamento snapshot**  
Abilita questa funzionalità.  

 **NULL ANSI predefinito**  
 Consente l'uso di valori Null per ogni colonna o tipo di dati definito dall'utente non indicato in modo esplicito come **NOT NULL** durante un'istruzione **CREATE TABLE** o **ALTER TABLE** (stato predefinito). Per altre informazioni, vedere [SET ANSI_NULL_DFLT_ON &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md) e [SET ANSI_NULL_DFLT_OFF &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-null-dflt-off-transact-sql.md).  
  
 **NULL ANSI abilitati**  
 Indica il comportamento degli operatori di confronto Uguale a (`=`) e Diverso da (`<>`) quando vengono utilizzati con valori Null. I valori possibili sono **True** (attivato) e **False** (disattivato). Quando il valore è **True**, tutti i confronti con un valore Null restituiscono UNKNOWN. Quando il valore è **False**, i confronti di valori non UNICODE con un valore Null restituiscono **True** se entrambi i valori sono NULL. Per altre informazioni, vedere [SET ANSI_NULLS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-nulls-transact-sql.md).  
  
 **Riempimento ANSI abilitato**  
 Indica se il riempimento ANSI è attivato o disattivato. I valori consentiti sono **True** (attivato) e **False** (disattivato). Per altre informazioni, vedere [SET ANSI_PADDING &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-padding-transact-sql.md).  
  
 **Avvisi ANSI abilitati**  
 Indica il comportamento dello standard ISO per diverse condizioni di errore. Quando il valore è **True**, viene generato un messaggio di avviso se le funzioni di aggregazione, quali SUM, AVG, MAX, MIN, STDEV, STDEVP, VAR, VARP o COUNT, includono valori Null. Quando il valore è **False**, non viene generato alcun messaggio di avviso. Per altre informazioni, vedere [SET ANSI_WARNINGS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-warnings-transact-sql.md).  
  
 **Interruzione per errori aritmetici abilitata**  
 Indica se l'opzione del database relativa all'interruzione aritmetica è abilitata o disabilitata. I valori possibili sono **True** e **False**. Quando il valore è **True**, un errore di overflow o di divisione per zero determina l'interruzione della query o del batch. Se l'errore si verifica in una transazione, viene eseguito il rollback della transazione. Quando il valore è **False**, viene visualizzato un messaggio di avviso, ma l'esecuzione della query, del batch o della transazione prosegue ignorando l'errore. Per altre informazioni, vedere [SET ARITHABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-arithabort-transact-sql.md).  
  
 **Risultato Null per concatenazione di valori Null**  
 Indica il comportamento in caso di valori Null concatenati. Se il valore della proprietà è **True**, **stringa** + NULL restituisce NULL. Se il valore è **False**, il risultato è **string**. Per altre informazioni, vedere [SET CONCAT_NULL_YIELDS_NULL &#40;Transact-SQL&#41;](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).  
  
 **Concatenamento della proprietà tra database abilitato**  
 Questo valore di sola lettura indica se è abilitato il concatenamento della proprietà tra database. Quando il valore è **True**, il database può essere l'origine o la destinazione di una catena di proprietà tra database. Utilizzare l'istruzione ALTER DATABASE per impostare questa proprietà.  
  
 **Ottimizzazione di correlazione data abilitata**  
 Se **True**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mantiene statistiche di correlazione per qualsiasi coppia di tabelle nel database collegata tramite un vincolo FOREIGN KEY e con colonne di tipo **datetime** .  
  
 Se **False**, le statistiche di correlazione non vengono mantenute.  
 
 **Durabilità ritardata**  
 Abilita questa funzionalità. Per altre informazioni, vedere [Controllo della durabilità delle transazioni](../logs/control-transaction-durability.md).
 
 **È abilitato per lo snapshot Read Committed**  
 Abilita questa funzionalità.  
 
 **Interruzione per perdita di precisione numerica**  
 Indica la modalità di gestione degli errori di arrotondamento utilizzata dal database. I valori possibili sono **True** e **False**. Quando il valore è **True**, viene generato un errore se si verifica una perdita di precisione in un'espressione. Quando il valore è **False**, la perdita di precisione non determina la visualizzazione di messaggi di errore e il risultato viene arrotondato alla precisione della colonna o della variabile in cui è archiviato. Per altre informazioni, vedere [SET NUMERIC_ROUNDABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-numeric-roundabort-transact-sql.md).  
  
 **Parametrizzazione**  
 Se **SIMPLE**, le query vengono parametrizzate in base al comportamento predefinito del database. Se **FORCED**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] parametrizza tutte le query del database.  
  
 **Identificatori delimitati abilitati**  
 Indica se le parole chiave di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possono essere utilizzate come identificatori (nome di variabile o oggetto) se racchiusi tra virgolette. I valori possibili sono **True** e **False**. Per altre informazioni, vedere [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
 **Trigger ricorsivi abilitati**  
 Indica se i trigger possono essere attivati da altri trigger. I valori possibili sono **True** e **False**. Se il valore è **True**, l'attivazione ricorsiva dei trigger è abilitata. Se il valore è **False**, viene impedita solo la ricorsione diretta. Per disabilitare la ricorsione indiretta, impostare l'opzione del server nested triggers su 0 utilizzando sp_configure. Per altre informazioni, vedere [Creazione di trigger annidati](../../relational-databases/triggers/create-nested-triggers.md).  
  
 **Attendibile**  
 Quando è visualizzato il valore **True**, questa opzione di sola lettura indica che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consente l'accesso a risorse esterne al database in un contesto di rappresentazione definito all'interno del database. I contesti di rappresentazione possono essere definiti all'interno del database mediante l'istruzione utente EXECUTE AS o la clausola EXECUTE AS sui moduli di database.  
  
 Per ottenere l'accesso, il proprietario del database deve anche disporre dell'autorizzazione AUTHENTICATE SERVER a livello del server.  
  
 Questa proprietà consente inoltre la creazione e l'esecuzione di assembly di accesso esterni e non sicuri all'interno del database. Oltre a impostare questa proprietà su **True**, il proprietario del database deve disporre dell'autorizzazione EXTERNAL ACCESS ASSEMBLY o UNSAFE ASSEMBLY a livello di server.  
  
 Per impostazione predefinita, in tutti i database utente e in tutti i database di sistema, ad eccezione di **MSDB**, questa proprietà è impostata su **False**. Il valore non può essere modificato nel caso dei database **model** e **tempdb** .  
  
 TRUSTWORTHY è impostata su **False** ogniqualvolta un database è collegato al server.  
  
 La modalità consigliata per l'accesso alle risorse esterne al database in un contesto di rappresentazione consiste nell'utilizzo di certificati e firme al posto dell'opzione **Trustworthy** .  
  
 Per impostare questa proprietà, utilizzare l'istruzione ALTER DATABASE.  
  
 **Formato di archiviazione vardecimal abilitato**  
 A partire da [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], questa opzione è di sola lettura. Se impostata su **True**, per il database è abilitato il formato di archiviazione vardecimal. Questo formato non può essere disabilitato se è in uso da una o più tabelle del database. In [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive, tutti i database utente sono abilitati per il formato di archiviazione vardecimal. Questa opzione usa [sp_db_vardecimal_storage_format](../../relational-databases/system-stored-procedures/sp-db-vardecimal-storage-format-transact-sql.md).  
  
## <a name="recovery"></a>Ripristino  
 **Verifica pagina**  
 Indica l'opzione utilizzata per individuare e segnalare le transazioni di I/O incomplete causate da errori di I/O su disco. I valori possibili sono **None**, **TornPageDetection** e **Checksum**. Per altre informazioni, vedere [Gestione della tabella suspect_pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md).  
  
 **Tempo di recupero di riferimento (secondi)**  
 Specifica il limite massimo di tempo, in secondi, necessario per recuperare il database specificato in caso di un arresto anomalo del sistema. Per altre informazioni, vedere [Checkpoint di database &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md).  

## <a name="service-broker"></a>Broker di servizio  
**Broker abilitato**  
Abilita o disabilita Service Broker.  

**Rispetta priorità di Service Broker**  
Proprietà di Service Broker di sola lettura.  

**Identificatore Service Broker**  
Identificatore di sola lettura.  

## <a name="state"></a>State  
 **Database di sola lettura**  
 Indica se il database è di sola lettura. I valori possibili sono **True** e **False**. Se il valore è **True**, gli utenti possono unicamente leggere i dati contenuti nel database. Gli utenti non possono modificare i dati o gli oggetti di database. È tuttavia possibile eliminare il database usando l'istruzione `DROP DATABASE`. Il database non può essere in uso quando si specifica un nuovo valore per l'opzione **Database di sola lettura** . L'unica eccezione riguarda il database master e prevede che solo l'amministratore di sistema possa utilizzare il database master durante l'impostazione di questa opzione.  
  
 **Stato database**  
 Indica lo stato corrente del database. Non è modificabile. Per ulteriori informazioni su **Stato database**, vedere [Database States](../../relational-databases/databases/database-states.md).  

 **Crittografia abilitata**  
 Se questa opzione è impostata su **True**, la crittografia è abilitata per il database. Per la crittografia è necessaria una chiave di crittografia del database. Per altre informazioni sulla crittografia trasparente del database, vedere [Transparent Data Encryption &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md).  
 
 **Limitazione accesso**  
 Indica gli utenti autorizzati ad accedere al database. I valori possibili sono:  
  
-   **Più di uno**  
  
     Rappresenta lo stato normale per un database di produzione e consente l'accesso simultaneo di più utenti al database.  
  
-   **Singolo**  
  
     Questa impostazione viene utilizzata per operazioni di manutenzione e consente l'accesso al database di un solo utente alla volta.  
  
-   **Con restrizioni**  
  
     Solo i membri del ruolo db_owner, dbcreator o sysadmin possono utilizzare il database.  
  


## <a name="see-also"></a>Vedere anche  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md)  
