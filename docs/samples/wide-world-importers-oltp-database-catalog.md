---
title: Catalogo del database OLTP WideWorldImporters-SQL | Microsoft Docs
description: Comprendere gli schemi, le tabelle, le stored procedure e le considerazioni di progettazione per il catalogo del database di esempio WideWorldImporters.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 04/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 991f001ea8fd43298cefe1f9ba542472de6935ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354091"
---
# <a name="wideworldimporters-database-catalog"></a>Catalogo di database WideWorldImporters
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
Il database WideWorldImporters contiene tutte le informazioni sulle transazioni e i dati giornalieri per le vendite e gli acquisti, oltre ai dati dei sensori per i veicoli e le stanze fredde.

## <a name="schemas"></a>Schemi

WideWorldImporters utilizza schemi per scopi diversi, ad esempio l'archiviazione dei dati, la definizione del modo in cui gli utenti possono accedere ai dati e la fornitura di oggetti per data warehouse lo sviluppo e l'integrazione.

### <a name="data-schemas"></a>Schemi di dati

Questi schemi contengono i dati. Per tutti gli altri schemi sono necessarie diverse tabelle e si trovano nello schema dell'applicazione.

|SCHEMA|Descrizione|
|-----------------------------|---------------------|
|Applicazione|Utenti, contatti e parametri a livello di applicazione. Contiene anche tabelle di riferimento con dati utilizzati da più schemi|
|Acquisto|Acquisti di articoli azionari da fornitori e informazioni dettagliate sui fornitori.|  
|Sales|Vendite di articoli azionari ai clienti finali e informazioni dettagliate sui clienti e sui venditori. |  
|Warehouse|Inventario e transazioni di articoli azionari.|  

### <a name="secure-access-schemas"></a>Schemi di accesso sicuro

Questi schemi vengono utilizzati per le applicazioni esterne che non possono accedere direttamente alle tabelle di dati. Contengono viste e stored procedure utilizzate da applicazioni esterne.

|SCHEMA|Descrizione|
|-----------------------------|---------------------|
|Sito Web|Tutti gli accessi al database dal sito Web della società sono tramite questo schema.|
|Report|Tutti gli accessi al database da Reporting Services report si riferiscono a questo schema.|
|PowerBI|Tutti gli accessi al database dal dashboard Power BI tramite il gateway aziendale sono tramite questo schema.|

Si noti che i report e gli schemi Power BI non vengono utilizzati nella versione iniziale del database di esempio. Tuttavia, tutti i Reporting Services e Power BI esempi basati su questo database sono invitati a utilizzare questi schemi.

### <a name="development-schemas"></a>Schemi di sviluppo

Schemi per scopi specifici

|SCHEMA|Descrizione|
|-----------------------------|---------------------|
|Integrazione|Oggetti e procedure necessari per l'integrazione di data warehouse, ad esempio la migrazione dei dati al database WideWorldImportersDW.|
|Sequenze|Include le sequenze utilizzate da tutte le tabelle nell'applicazione.|

## <a name="tables"></a>Tabelle

Tutte le tabelle del database si trovano negli schemi di dati.

### <a name="application-schema"></a>Schema applicazione

Dettagli dei parametri e delle persone (utenti e contatti), insieme alle tabelle di riferimento comuni (comuni a più schemi diversi).

|Tabella|Descrizione|
|-----------------------------|---------------------|
|SystemParameters|Contiene parametri configurabili a livello di sistema.|
|Persone|Contiene i nomi utente, le informazioni di contatto, per tutti gli utenti che usano l'applicazione e per gli utenti che hanno a che fare con le organizzazioni del cliente. Sono inclusi il personale, i clienti, i fornitori e altri contatti. Per gli utenti a cui è stata concessa l'autorizzazione a utilizzare il sistema o il sito Web, le informazioni includono i dettagli di accesso.|
|Cities|Nel sistema sono archiviati molti indirizzi, per gli utenti, per gli indirizzi di recapito dell'organizzazione cliente, per gli indirizzi di ritiro dei fornitori e così via. Ogni volta che viene archiviato un indirizzo, in questa tabella è presente un riferimento a una città. Esiste anche una posizione spaziale per ogni città.|
|StateProvinces|Le città fanno parte di Stati o province. Questa tabella contiene i dettagli di questi elementi, inclusi i dati spaziali che descrivono i limiti di ogni stato o provincia.|
|Paesi|Stati o province fanno parte di paesi. Questa tabella contiene i dettagli di questi elementi, inclusi i dati spaziali che descrivono i limiti di ogni paese.|
|DeliveryMethods|Opzioni per la distribuzione di articoli di stock, ad esempio Truck/Van, post, pickup, Courier e così via.|
|PaymentMethods|Opzioni per l'esecuzione dei pagamenti (ad esempio, contanti, check, EFT e così via)|
|TransactionTypes|Tipi di cliente, fornitore o transazione azionaria (ad esempio, fattura, nota di credito e così via)|

### <a name="purchasing-schema"></a>Schema di acquisto

Dettagli dei fornitori e degli acquisti di articoli azionari.

|Tabella|Descrizione|
|-----------------------------|---------------------|
|Fornitori|Tabella principale delle entità per i fornitori (organizzazioni)|
|SupplierCategories|Categorie per i fornitori (ad esempio, novità, giocattoli, abbigliamento, creazione di pacchetti e così via)|
|SupplierTransactions|Tutte le transazioni finanziarie correlate ai fornitori (fatture, pagamenti)|
|PurchaseOrders|Dettagli degli ordini di acquisto del fornitore|
|PurchaseOrderLines|Righe di dettaglio dagli ordini di acquisto dei fornitori|

 
### <a name="sales-schema"></a>Schema vendite

Dettagli relativi a clienti, venditori e vendite di articoli azionari.

|Tabella|Descrizione|
|-----------------------------|---------------------|
|Clienti|Principali tabelle di entità per i clienti (organizzazioni o individui)|
|CustomerCategories|Categorie per i clienti (ad esempio, negozi di novità, supermercati e così via)|
|BuyingGroups|Le organizzazioni del cliente possono far parte di gruppi che impiegano una maggiore potenza di acquisto|
|CustomerTransactions|Tutte le transazioni finanziarie correlate ai clienti (fatture, pagamenti)|
|SpecialDeals|Prezzo speciale. Questo può includere prezzi fissi, sconto in dollari o percentuale di sconto.|
|Orders|Dettagli degli ordini dei clienti|
|OrderLines|Righe di dettaglio dagli ordini dei clienti|
|Fatture|Dettagli delle fatture dei clienti|
|InvoiceLines|Righe di dettaglio dalle fatture dei clienti|

### <a name="warehouse-schema"></a>Schema warehouse

Dettagli degli articoli azionari, delle relative operazioni di mantenimento e delle transazioni.

|Tabella|Descrizione|
|-----------------------------|---------------------|
|StockItems|Tabella principale delle entità per gli elementi azionari|
|StockItemHoldings|Colonne non temporali per gli elementi azionari. Si tratta di colonne aggiornate di frequente.|
|StockGroups|Gruppi per la categorizzazione di elementi azionari (ad esempio, novità, giocattoli, novità commestibili e così via)|
|StockItemStockGroups|Quali elementi azionari sono in cui i gruppi azionari (molti a molti)|
|Colori|Gli elementi azionari possono (facoltativamente) avere colori|
|PackageTypes|Modi in cui è possibile confezionare gli elementi azionari, ad esempio box, carton, pallet, kg e così via.|
|StockItemTransactions|Transazioni che coprono tutti i movimenti di tutti gli elementi azionari (ricevuta, vendita, scrittura)|
|VehicleTemperatures|Temperature registrate regolarmente per i refrigeratori dei veicoli|
|ColdRoomTemperatures|Temperature registrate regolarmente dei chiller della stanza fredda|


## <a name="design-considerations"></a>Considerazioni sulla progettazione

La progettazione del database è soggettiva e non esiste una soluzione corretta o errata per progettare un database. Gli schemi e le tabelle di questo database mostrano le idee per la progettazione di un database personalizzato.

### <a name="schema-design"></a>Progettazione dello schema

WideWorldImporters utilizza un numero ridotto di schemi per semplificare la comprensione del sistema di database e la dimostrazione dei principi del database.  

Laddove possibile, il database colloca le tabelle normalmente sottoposte a query nello stesso schema per ridurre al minimo la complessità dei join.

Lo schema del database è stato generato dal codice in base a una serie di tabelle di metadati in un altro database WWI_Preparation. Ciò offre a WideWorldImporters un livello molto elevato di coerenza della progettazione, coerenza dei nomi e completezza. Per informazioni dettagliate sul modo in cui lo schema è stato generato, vedere il codice sorgente: [Wide-World-Importers/overstate-database-scripts](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/sample-scripts)

### <a name="table-design"></a>Progettazione della tabella

- Tutte le tabelle includono chiavi primarie a colonna singola per la semplicità di join.
- Tutti gli schemi, le tabelle, le colonne, gli indici e i vincoli check hanno una proprietà estesa Description che può essere utilizzata per identificare lo scopo dell'oggetto o della colonna. Le tabelle con ottimizzazione per la memoria rappresentano un'eccezione a questo dato che attualmente non supportano le proprietà estese.
- Tutte le chiavi esterne vengono indicizzate automaticamente a meno che non esista un altro indice non cluster con lo stesso componente a sinistra.
- La numerazione automatica nelle tabelle è basata sulle sequenze. Queste sequenze sono più facili da usare tra server collegati e ambienti simili rispetto alle colonne IDENTITY. Le tabelle con ottimizzazione per la memoria usano le colonne IDENTITY perché non supportano in SQL Server 2016.
- Per queste tabelle viene utilizzata una singola sequenza (ID transazione): CustomerTransactions, SupplierTransactions e StockItemTransactions. Viene illustrato il modo in cui un set di tabelle può avere una singola sequenza.
- Alcune colonne hanno valori predefiniti appropriati.

### <a name="security-schemas"></a>Schemi di sicurezza

Per la sicurezza, WideWorldImporters non consente alle applicazioni esterne di accedere direttamente agli schemi di dati. Per isolare l'accesso, WideWorldImporters utilizza schemi di accesso alla sicurezza che non contengono dati, ma che contengono viste e stored procedure. Le applicazioni esterne utilizzano gli schemi di sicurezza per recuperare i dati che sono autorizzati a visualizzare.  In questo modo, gli utenti possono eseguire le viste e le stored procedure solo negli schemi di accesso sicuro

Ad esempio, questo esempio include Power BI Dashboard. Un'applicazione esterna accede a questi dashboard Power BI dal gateway Power BI come utente che dispone dell'autorizzazione di sola lettura per lo schema Power bi.  Per l'autorizzazione di sola lettura, l'utente deve avere solo l'autorizzazione SELECT ed EXECUTE per lo schema Power bi. Un amministratore di database alla prima guerra assegna queste autorizzazioni in base alle esigenze.

## <a name="stored-procedures"></a>Stored procedure

Le stored procedure sono organizzate in schemi. La maggior parte degli schemi viene utilizzata a scopo di configurazione o di campionamento.

Lo `Website` schema contiene le stored procedure che possono essere utilizzate da un front-end Web.

Gli `Reports` `PowerBI` schemi e sono destinati a Reporting Services e a scopi Power bi. Tutte le estensioni dell'esempio sono consigliate per l'utilizzo di questi schemi per la creazione di report.

### <a name="website-schema"></a>Schema sito Web

Queste sono le procedure usate da un'applicazione client, ad esempio un front-end Web.

|Procedura|Scopo|
|-----------------------------|---------------------|
|ActivateWebsiteLogon|Consente a una persona (da `Application.People` ) di accedere al sito Web.|
|ChangePassword|Modifica la password di un utente (per gli utenti che non usano meccanismi di autenticazione esterni).|
|InsertCustomerOrders|Consente l'inserimento di uno o più ordini cliente (incluse le righe di ordine).|
|InvoiceCustomerOrders|Accetta un elenco di ordini per la fatturazione ed elabora le fatture.|
|RecordColdRoomTemperatures|Accetta un elenco di dati del sensore come parametro con valori di tabella (TVP) e applica i dati alla `Warehouse.ColdRoomTemperatures` tabella temporale.|
|RecordVehicleTemperature|Accetta una matrice JSON e la usa per aggiornare `Warehouse.VehicleTemperatures` .|
|SearchForCustomers|Cerca i clienti in base al nome o a una parte del nome, ovvero il nome della società o il nome della persona.|
|SearchForPeople|Cerca persone per nome o parte del nome.|
|SearchForStockItems|Cerca gli elementi azionari per nome o parte del nome o dei commenti di marketing.|
|SearchForStockItemsByTags|Cerca gli elementi azionari in base ai tag.|
|SearchForSuppliers|Cerca i fornitori in base al nome o a una parte del nome (nome della società o nome della persona).|

### <a name="integration-schema"></a>Schema di integrazione

Le stored procedure in questo schema vengono utilizzate dal processo ETL. Ottengono i dati necessari da diverse tabelle per l'intervallo di tempo richiesto dal [pacchetto ETL](wide-world-importers-perform-etl.md).

### <a name="dataloadsimulation-schema"></a>Schema DataLoadSimulation

Simula un carico di lavoro che inserisce vendite e acquisti. Il stored procedure principale è `PopulateDataToCurrentDate` , utilizzato per inserire i dati di esempio fino alla data corrente.

|Procedura|Scopo|
|-----------------------------|---------------------|
|Configuration_ApplyDataLoadSimulationProcedures|Ricrea le procedure necessarie per la simulazione del caricamento dei dati. Questa operazione è necessaria per portare i dati alla data corrente.|
|Configuration_RemoveDataLoadSimulationProcedures|In questo modo, le procedure vengono rimosse al termine della simulazione.|
|DeactivateTemporalTablesBeforeDataLoad|Rimuove la natura temporale di tutte le tabelle temporali e, ove applicabile, applica un trigger in modo che le modifiche possano essere apportate come se fossero applicate a una data precedente rispetto a quelle consentite dalle tabelle sys-temporali.|
|PopulateDataToCurrentDate|Utilizzato per portare i dati alla data corrente. Deve essere eseguito prima di qualsiasi altra opzione di configurazione dopo il ripristino del database da un backup iniziale.|
|ReactivateTemporalTablesAfterDataLoad|Ristabilisce le tabelle temporali, inclusa la verifica della coerenza dei dati. Rimuove i trigger associati.|


### <a name="application-schema"></a>Schema applicazione

Queste procedure vengono utilizzate per configurare l'esempio. Vengono utilizzati per applicare le funzionalità di Enterprise Edition alla versione Standard Edition dell'esempio e per aggiungere il controllo e l'indicizzazione full-text.

|Procedura|Scopo|
|-----------------------------|---------------------|
|AddRoleMemberIfNonexistant|Aggiunge un membro a un ruolo se il membro non è già incluso nel ruolo|
|Configuration_ApplyAuditing|Aggiunge il controllo. Il controllo del server viene applicato per i database Standard Edition; per Enterprise Edition è stato aggiunto il controllo aggiuntivo del database.|
|Configuration_ApplyColumnstoreIndexing|Applica l'indicizzazione columnstore a `Sales.OrderLines` e `Sales.InvoiceLines` e reindicizzare in modo appropriato.|
|Configuration_ApplyFullTextIndexing|Applica indici full-text a `Application.People` , `Sales.Customers` , `Purchasing.Suppliers` e `Warehouse.StockItems` . Sostituisce `Website.SearchForPeople` , `Website.SearchForSuppliers` , `Website.SearchForCustomers` , `Website.SearchForStockItems` , `Website.SearchForStockItemsByTags` con procedure di sostituzione che utilizzano l'indicizzazione full-text.|
|Configuration_ApplyPartitioning|Applica il partizionamento delle tabelle a `Sales.CustomerTransactions` e `Purchasing.SupplierTransactions` e riorganizza gli indici da adattare.|
|Configuration_ApplyRowLevelSecurity|Applica la sicurezza a livello di riga per filtrare i clienti in base ai ruoli correlati al territorio di vendita.|
|Configuration_ConfigureForEnterpriseEdition|Applica indicizzazione columnstore, full-text, in memoria, polibase e partizionamento.|
|Configuration_EnableInMemory|Aggiunge un filegroup ottimizzato per la memoria (quando non funziona in Azure), sostituisce `Warehouse.ColdRoomTemperatures` `Warehouse.VehicleTemperatures` con gli equivalenti in memoria ed esegue la migrazione dei dati, ricrea i `Website.OrderIDList` tipi di `Website.OrderList` tabella,, e `Website.OrderLineList` `Website.SensorDataList` con equivalenti ottimizzati per la memoria, Elimina e ricrea le routine `Website.InvoiceCustomerOrders` , `Website.InsertCustomerOrders` e `Website.RecordColdRoomTemperatures` che utilizzano questi tipi di tabella.|
|Configuration_RemoveAuditing|Rimuove la configurazione di controllo.|
|Configuration_RemoveRowLevelSecurity|Rimuove la configurazione di sicurezza a livello di riga, necessaria per le modifiche alle tabelle associate.|
|CreateRoleIfNonExistant|Crea un ruolo del database, se non esiste già.|


### <a name="sequences-schema"></a>Schema sequences

Procedure per la configurazione delle sequenze nel database.

|Procedura|Scopo|
|-----------------------------|---------------------|
|ReseedAllSequences|Chiama la stored procedure ReseedSequenceBeyondTableValue per tutte le sequenze.|
|ReseedSequenceBeyondTableValue|Utilizzato per riposizionare il successivo valore di sequenza oltre il valore in una tabella che utilizza la stessa sequenza. (Ad esempio, un DBCC CHECKIDENT per le colonne Identity equivalenti per le sequenze, ma in più tabelle potenzialmente diverse).|
