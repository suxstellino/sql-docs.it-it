---
description: Uso dei file script della console di esempio (DB2ToSQL)
title: Uso dei file script della console di esempio (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 5c3080c3-d074-4f99-a5f5-219ebeddc474
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: e9c19e60d2b4b30724dc750f3a99a604334a2155
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069236"
---
# <a name="working-with-the-sample-console-script-files-db2tosql"></a>Uso dei file script della console di esempio (DB2ToSQL)
Sono stati forniti alcuni file di esempio insieme al prodotto per il riferimento utente e l'utilizzo. In questa sezione viene descritto come personalizzare facilmente questi script in base alle esigenze dell'utente finale.  
  
## <a name="sample-console-script-files"></a>File script della console di esempio  
Per informazioni di riferimento sull'utente sono stati forniti i file script della console di esempio seguenti relativi a diversi scenari:  
  
-   ServersConnectionFileSample.xml  
  
-   VariableValueFileSample.xml  
  
-   AssessmentReportGenerationSample.xml  
  
-   SqlStatementConversionSample.xml  
  
-   ConversionAndDataMigrationSample.xml  
  
1.  **ServersConnectionFileSample.xml:**  
  
    -   In questo esempio vengono fornite le diverse modalità di connessione disponibili per il database di origine e di destinazione e l'utente può selezionare qualsiasi modalità in base al requisito. Questo esempio contiene le definizioni del server.  
  
    -   L'utente può connettersi al database necessario semplicemente modificando i valori delle definizioni del server di origine e di destinazione richieste. Nell'esempio, tutti i valori sono stati forniti come valori variabili disponibili nell' **VariableValueFileSample.xml**.  Tutti gli altri parametri di connessione possono essere rimossi dal file di connessione del server funzionante dell'utente.  
  
    -   Per ulteriori informazioni sulla connessione al server di origine e di destinazione, vedere [creazione dei file di connessione al server &#40;DB2ToSQL&#41;](../../ssma/db2/creating-the-server-connection-files-db2tosql.md) .  
  
2.  **VariableValueFileSample.xml:** Tutte le variabili usate nei file script della console di esempio e sono state confrontate `ServersConnectionFileSample.xml` in questo file. Per eseguire gli script della console di esempio, l'utente deve semplicemente sostituire i valori delle variabili di esempio con quelli definiti dall'utente e passare il file come argomento della riga di comando aggiuntivo insieme al file di script.  
  
    Per altre informazioni sul file di valori di variabile, vedere [creazione di file di valori di variabile &#40;DB2ToSQL&#41;](../../ssma/db2/creating-variable-value-files-db2tosql.md).  
  
3.  **AssessmentReportGenerationSample.xml:** Questo esempio consente all'utente di generare un report di valutazione XML che può essere utilizzato dall'utente per l'analisi prima di iniziare la conversione e la migrazione dei dati.  
  
    Nel `generate-assessment-report` comando l'utente deve obbligatoriamente modificare il valore della variabile (fare riferimento **VariableValueFileSample.xml**) nell' `object-name` attributo al nome del database usato dall'utente. A seconda del tipo di oggetto specificato, `object-type` sarà necessario modificare anche il valore.  
  
    Se l'utente deve valutare più oggetti/database, può specificare più `metabase-object` nodi, come illustrato nell' `generate-assessment-report` esempio di comando 4 del file script della console di esempio.  
  
    Per ulteriori informazioni sulla generazione di report, vedere [generazione di report &#40;DB2ToSQL&#41;](../../ssma/db2/generating-reports-db2tosql.md).  
  
    **Note:**  
  
    Verificare che l'argomento della riga di comando file valore variabile venga passato all'applicazione console e VariableValueFileSample.xml venga aggiornato con i valori specificati dall'utente.  
  
    Verificare che l'argomento della riga di comando del file di connessione del server venga passato all'applicazione console e che il ServersConnectionFileSample.xml venga aggiornato con i valori dei parametri del server corretti.  
  
4.  **SqlStatementConversionSample.xml:** Questo esempio consente all'utente di generare lo `t-sql` script corrispondente per il comando del database di origine `sql` fornito come input.  
  
    Nel `convert-sql-statement` comando l'utente deve obbligatoriamente modificare il valore della variabile (fare riferimento **VariableValueFileSample.xml**) nell' `context` attributo al nome del database in uso da parte dell'utente. L'utente dovrà anche modificare il `sql` valore dell'attributo con il comando del database di origine `sql` che richiede la conversione.  
  
    L'utente può inoltre fornire i file SQL da convertire. Questo è stato illustrato nell' `convert-sql-statement` esempio 4 del comando del file script della console di esempio.  
  
    > [!NOTE]  
    > Verificare che l'argomento della riga di comando file valore variabile venga passato all'applicazione console e VariableValueFileSample.xml venga aggiornato con i valori specificati dall'utente.  
  
5.  **ConversionAndDataMigrationSample.xml:** Questo esempio consente all'utente di eseguire una migrazione end-to-end dalla conversione alla migrazione dei dati. L'elenco dei valori di attributo obbligatori che dovranno essere modificati è elencato di seguito:  
  
    |Nome comando|Descrizione|Attributo|  
    |----------------|---------------|-------------|  
    |`map-schema`|Mapping dello schema del database di origine allo schema di destinazione.|`source-schema:` Specifica il database di origine che richiede la conversione.<br /><br />`sql-server-schema`: Specifica il database di destinazione di cui eseguire la migrazione|  
    |`convert-schema`|Esegue la conversione dello schema dall'origine allo schema di destinazione.<br /><br />Se l'utente deve valutare più oggetti/database, può specificare più `metabase-object` nodi, come illustrato nell' `convert-schema` esempio di comando 4 del file script della console di esempio.|`object-name`: Specificare il nome del database o dell'oggetto di origine che richiede la conversione. Verificare che il corrispondente `object-type` venga modificato in base al tipo di oggetto specificato nel parametro `object-name`|  
    |`synchronize-target`|Sincronizza gli oggetti di destinazione con il database di destinazione.<br /><br />Se l'utente deve valutare più oggetti/database, può specificare più `metabase-object` nodi, come illustrato nell' `synchronize-target` esempio 3 del file di script della console di esempio.|`object-name:` Specificare il nome del database/oggetto di SQL Server che deve essere creato. Verificare che il corrispondente `object-type` venga modificato in base al tipo di oggetto specificato nel parametro `object-name`|  
    |`migrate-data`|Esegue la migrazione dei dati di origine alla destinazione.<br /><br />Se l'utente deve valutare più oggetti/database, può specificare più `metabase-object` nodi, come illustrato nell' `migrate-data` esempio 2 del file di script della console di esempio.|`object-name:` Specifica il nome del database o delle tabelle di origine che richiede la migrazione. Verificare che il corrispondente `object-type` venga modificato in base al tipo di oggetto specificato nel parametro `object-name`|  
  
## <a name="see-also"></a>Vedere anche  
[Creazione di file di valori di variabile &#40;DB2ToSQL&#41;](../../ssma/db2/creating-variable-value-files-db2tosql.md)  
[Creazione dei file di connessione del server &#40;DB2ToSQL&#41;](../../ssma/db2/creating-the-server-connection-files-db2tosql.md)  
[Generazione di report &#40;DB2ToSQL&#41;](../../ssma/db2/generating-reports-db2tosql.md)  
  
