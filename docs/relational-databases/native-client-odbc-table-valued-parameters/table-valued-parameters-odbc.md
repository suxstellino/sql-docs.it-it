---
description: Parametri con valori di tabella (ODBC)
title: Parametri di Table-Valued (ODBC) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (ODBC)
- ODBC, table-valued parameters
ms.assetid: ef06cd13-18e2-4c65-8ede-c3955d820e54
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e7c561ef9b4ee7602bffe65f6a18c8a440970299
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755031"
---
# <a name="table-valued-parameters-odbc"></a>Parametri con valori di tabella (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Il supporto ODBC dei parametri con valori di tabella consente a un'applicazione client di inviare più efficientemente i dati con parametri al server, inviando più righe al server con una sola chiamata.  
  
 Per informazioni sui parametri con valori di tabella nel server, vedere [usare Table-Valued parametri &#40;motore di database&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).  
  
 In ODBC è possibile inviare parametri con valori di tabella al server in due modi:  
  
-   Tutti i dati dei parametri con valori di tabella possono essere in memoria al momento della chiamata a SQLExecDirect o SQLExecute. Se sono presenti più righe nel valore di tabella, i dati vengono archiviati in matrici.  
  
-   Un'applicazione può specificare data-at-execution per un parametro con valori di tabella quando viene chiamato SQLExecDirect o SQLExecute. In tal caso, le righe di dati per il valore di tabella possono essere fornite in batch o uno alla volta per ridurre i requisiti di memoria.  
  
 La prima opzione consente alle stored procedure di incapsulare più logica di business. Ad esempio, una singola stored procedure può incapsulare un'intera transazione di immissione ordini se gli articoli dell'ordine vengono passati come parametro con valori di tabella. Questa opzione è molto efficiente poiché è necessario un solo round trip del server. In alternativa, è possibile utilizzare altre procedure per gestire separatamente l'intestazione degli ordini e gli articoli richiedendo più codice e un contratto più complesso tra il client e il server.  
  
 Il secondo metodo fornisce un meccanismo efficiente per le operazioni bulk con quantità elevate di dati consentendo a un'applicazione di trasmettere un flusso di righe di dati al server senza doverle prima memorizzare nel buffer.  
  
 È possibile creare vincoli e chiavi primarie durante la creazione della variabile di tabella. I vincoli rappresentano un ottimo metodo per assicurarsi che i dati di una tabella soddisfino requisiti specifici.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
 [Utilizzi dei parametri con valori di tabella in ODBC](../../relational-databases/native-client-odbc-table-valued-parameters/uses-of-odbc-table-valued-parameters.md)  
 Vengono descritti gli scenari utente principali per i parametri con valori di tabella e ODBC.  
  
 [Tipo SQL ODBC per parametri con valori di tabella](../../relational-databases/native-client-odbc-table-valued-parameters/odbc-sql-type-for-table-valued-parameters.md)  
 Viene descritto il tipo SQL_SS_TABLE. Si tratta di un nuovo tipo ODBC SQL che supporta i parametri con valori di tabella.  
  
 [Campi di descrizione dei parametri con valori di tabella](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameter-descriptor-fields.md)  
 Vengono descritti i campi di descrizione che supportano i parametri con valori di tabella.  
  
 [Campi di descrizione per le colonne che costituiscono parametri con valori di tabella](../../relational-databases/native-client-odbc-table-valued-parameters/descriptor-fields-for-table-valued-parameter-constituent-columns.md)  
 Vengono descritti i campi di descrizione che hanno valenza per i parametri con valori di tabella.  
  
 [Campi dei record di diagnostica dei parametri con valori di tabella](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameter-diagnostic-record-fields.md)  
 Vengono descritti i due campi di diagnostica aggiunti ai record di diagnostica per supportare i parametri con valori di tabella.  
  
 [Attributi dell'istruzione che influiscono sui parametri con valori di tabella](../../relational-databases/native-client-odbc-table-valued-parameters/statement-attributes-that-affect-table-valued-parameters.md)  
 Viene descritto un nuovo campo di intestazione di descrizione che consente l'indirizzamento delle colonne dei parametri con valori di tabella.  
  
 [Associazione e trasferimento dati di valori di colonna e parametri con valori di tabella](../../relational-databases/native-client-odbc-table-valued-parameters/binding-and-data-transfer-of-table-valued-parameters-and-column-values.md)  
 Vengono descritti l'associazione dei parametri e il passaggio di un parametro con valori di tabella al server.  
  
 [Metadati del parametro con valori di tabella per le istruzioni preparate](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameter-metadata-for-prepared-statements.md)  
 Viene descritto il modo in cui un'applicazione può ottenere i metadati per una chiamata alla procedura preparata.  
  
 [Metadati aggiuntivi dei parametri con valori di tabella](../../relational-databases/native-client-odbc-table-valued-parameters/additional-table-valued-parameter-metadata.md)  
 Viene descritto come utilizzare SQLProcedureColumns, SQLTables e SQLColumns per recuperare i metadati per un parametro con valori di tabella.  
  
 [Conversione di dati dei parametri con valori di tabella e altri errori e avvisi](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameter-data-conversion-and-other-errors-and-warnings.md)  
 Viene descritta l'elaborazione degli errori nei valori delle colonne dei parametri con valori di tabella.  
  
 [Compatibilità tra versioni](../../relational-databases/native-client-odbc-table-valued-parameters/cross-version-compatibility.md)  
 Vengono descritti i conflitti che possono verificarsi quando i parametri con valori di tabella vengono utilizzati da un client o da un server di una versione precedente di [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].  
  
 [Riepilogo delle API dei parametri con valori di tabella ODBC](../../relational-databases/native-client-odbc-table-valued-parameters/odbc-table-valued-parameter-api-summary.md)  
 Vengono descritte le funzioni ODBC che supportano i parametri con valori di tabella.  

## <a name="see-also"></a>Vedere anche  
 [SQL Server Native Client &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)   
 [Parametri con valori di tabella &#40;SQL Server Native Client&#41;](../../relational-databases/native-client/features/table-valued-parameters-sql-server-native-client.md)  
  
