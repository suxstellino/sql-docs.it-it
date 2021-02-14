---
title: Catalogo di database OLAP WideWorldImporters-SQL | Microsoft Docs
description: Informazioni sugli schemi, le tabelle e le stored procedure utilizzati per l'elaborazione analitica e di data warehousing nel database WideWorldImportersDW.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 08/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||>=aps-pdw-2016||=azuresqldb-mi-current'
ms.openlocfilehash: 4481a0c96a8f4a7f2f82e8babc5d8dce5ea97e92
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077813"
---
# <a name="wideworldimportersdw-database-catalog"></a>Catalogo di database WideWorldImportersDW
[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]
Spiegazioni per gli schemi, le tabelle e le stored procedure nel database WideWorldImportersDW. 

Il database WideWorldImportersDW viene utilizzato per l'elaborazione analitica e di data warehousing. I dati transazionali sulle vendite e sugli acquisti vengono generati nel database di WideWorldImporters e caricati nel database WideWorldImportersDW usando un **processo ETL giornaliero**.

I dati in WideWorldImportersDW riflettono quindi i dati in WideWorldImporters, ma le tabelle sono organizzate in modo diverso. Mentre WideWorldImporters dispone di uno schema normalizzato tradizionale, WideWorldImportersDW usa l'approccio con [schema a stella](https://wikipedia.org/wiki/Star_schema) per la progettazione delle tabelle. Oltre alle tabelle dei fatti e delle dimensioni, nel database sono incluse diverse tabelle di gestione temporanea utilizzate nel processo ETL.

## <a name="schemas"></a>Schemi

I diversi tipi di tabelle sono organizzati in tre schemi.

|SCHEMA|Descrizione|
|-----------------------------|---------------------|
|Dimension|Tabelle delle dimensioni.|
|Fact| Tabelle dei fatti.|  
|Integrazione|Tabelle di staging e altri oggetti necessari per ETL.|  

## <a name="tables"></a>Tabelle

Di seguito sono elencate le tabelle delle dimensioni e dei fatti. Le tabelle nello schema di integrazione vengono utilizzate solo per il processo ETL e non sono elencate.

### <a name="dimension-tables"></a>Tabelle delle dimensioni

WideWorldImportersDW include le tabelle delle dimensioni seguenti. La descrizione include la relazione con le tabelle di origine nel database WideWorldImporters.

|Tabella|Tabelle di origine|
|-----------------------------|---------------------|
|City|`Application.Cities`, `Application.StateProvinces`, `Application.Countries`.|
|Customer|`Sales.Customers`, `Sales.BuyingGroups`, `Sales.CustomerCategories`.|
|Data|Nuova tabella con informazioni sulle date, incluso l'anno finanziario, in base al 1 ° novembre per l'anno finanziario.|
|Dipendente|`Application.People`.|
|StockItem|`Warehouse.StockItems`, `Warehouse.Colors`, `Warehouse.PackageType`.|
|Fornitore|`Purchasing.Suppliers`, `Purchasing.SupplierCategories`.|
|PaymentMethod|`Application.PaymentMethods`.|
|TransactionType|`Application.TransactionTypes`.|

### <a name="fact-tables"></a>Tabelle dei fatti

In WideWorldImportersDW sono presenti le tabelle dei fatti seguenti. La descrizione include la relazione con le tabelle di origine nel database WideWorldImporters, nonché le classi di query di analisi e Reporting, in genere ogni tabella dei fatti viene utilizzata con.

|Tabella|Tabelle di origine|Analisi di esempio|
|-----------------------------|---------------------|---------------------|
|JSON|`Sales.Orders` e `Sales.OrderLines`|Addetti alle vendite, produttività di selezione e Packer e tempo di selezione degli ordini. Inoltre, le situazioni di stock ridotte portano a back Orders.|
|Sale|`Sales.Invoices` e `Sales.InvoiceLines`|Date di vendita, date di consegna, redditività nel tempo, redditività per venditore.|
|Acquisto|`Purchasing.PurchaseOrderLines`|Tempo reale previsto rispetto ai lead|
|Transazione|`Sales.CustomerTransactions` e `Purchasing.SupplierTransactions`|Misurazione delle date del problema rispetto alle date finali e agli importi.|
|Movement|`Warehouse.StockTransactions`|Spostamenti nel tempo.|
|Holding azionaria|`Warehouse.StockItemHoldings`|Livelli e valore delle scorte in mano.|

## <a name="stored-procedures"></a>Stored procedure

Le stored procedure vengono utilizzate principalmente per il processo ETL e per scopi di configurazione.

Tutte le estensioni dell'esempio sono consigliate per l'uso dello `Reports` schema per Reporting Services report e dello `PowerBI` schema per l'accesso a Power bi.

### <a name="application-schema"></a>Schema applicazione

Queste procedure vengono utilizzate per configurare l'esempio. Vengono usati per applicare le funzionalità di Enterprise Edition alla versione Standard Edition dell'esempio, aggiungono la polibase e il reseeding ETL.

|Procedura|Scopo|
|-----------------------------|---------------------|
|Configuration_ApplyPartitionedColumnstoreIndexing|Applica sia il partizionamento sia gli indici columnstore per le tabelle dei fatti.|
|Configuration_ConfigureForEnterpriseEdition|Applica il partizionamento, l'indicizzazione columnstore e in memoria.|
|Configuration_EnableInMemory|Sostituisce le tabelle di staging dell'integrazione con SCHEMA_ONLY tabelle ottimizzate per la memoria per migliorare le prestazioni ETL.|
|Configuration_ApplyPolyBase|Configura un'origine dati esterna, un formato di file e una tabella.|
|Configuration_PopulateLargeSaleTable|Applica le modifiche dell'edizione Enterprise, quindi popola una quantità maggiore di dati per l'anno di calendario 2012 come cronologia aggiuntiva.|
|Configuration_ReseedETL|Rimuove i dati esistenti e riavvia i semi ETL. In questo modo è possibile ripopolare il database OLAP affinché corrisponda alle righe aggiornate nel database OLTP.|

### <a name="integration-schema"></a>Schema di integrazione

Le procedure utilizzate nel processo ETL rientrino nelle categorie seguenti:
- Procedure di supporto per il pacchetto ETL: tutte le procedure Get *.
- Procedure utilizzate dal pacchetto ETL per la migrazione dei dati di gestione temporanea nelle tabelle DW-tutte le procedure di migrazione *.
- `PopulateDateDimensionForYear` -Richiede un anno e garantisce che tutte le date dell'anno siano popolate nella `Dimension.Date` tabella.

### <a name="sequences-schema"></a>Schema sequences

Procedure per la configurazione delle sequenze nel database.

|Procedura|Scopo|
|-----------------------------|---------------------|
|ReseedAllSequences|Chiama la routine `ReseedSequenceBeyondTableValue` per tutte le sequenze.|
|ReseedSequenceBeyondTableValue|Utilizzato per riposizionare il successivo valore di sequenza oltre il valore in una tabella che utilizza la stessa sequenza. (Ad esempio `DBCC CHECKIDENT` per le colonne Identity equivalenti per le sequenze ma per le tabelle potenzialmente multiple).|
