---
description: Esecuzione della console SSMA (SybaseToSQL)
title: Esecuzione della console SSMA (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/27/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Sybase Console,Database Connection Commands
- Sybase Console,Manageability Commands
- Sybase Console,Migration Commands
- Sybase Console,Migration Preparation Command
- Sybase Console,Project Commands
- Sybase Console,Report Commands
- Sybase Console,Script File Commands
- Sybase Console,Script Generation Commands
ms.assetid: ea8950b7-fabc-4aa4-89f8-9573a2617d70
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a7eed4ea61ada5979dbecdb106e5954620ae9c81
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079372"
---
# <a name="executing-the-ssma-console-sybasetosql"></a>Esecuzione della console SSMA (SybaseToSQL)
Microsoft offre un solido set di comandi di file script per eseguire e controllare le attività SSMA. Le sezioni seguenti illustrano in dettaglio lo stesso.  
  
## <a name="script-file-commands"></a>Comandi del file di script  
L'applicazione console USA determinati comandi del file di script standard come enumerati in questa sezione.  
  
## <a name="project-commands"></a>Comandi di progetto  
I comandi di progetto gestiscono la creazione di progetti, l'apertura, il salvataggio e l'uscita di progetti.  
  
### <a name="create-new-project"></a>create-new-project  
Questo comando crea un nuovo progetto SSMA.  
  
-   `project-folder` indica la cartella del progetto da creare.  
  
-   `project-name` indica il nome del progetto. {string}  
  
-   `overwrite-if-exists`Attributo facoltativo indica se è necessario sovrascrivere un progetto esistente. Boolean  
  
-   `project-type:`Attributo facoltativo. Indica il tipo di progetto, ovvero il progetto "SQL-Server-2005" o il progetto "SQL-Server-2008" o il progetto "SQL-Server-2012" oppure il progetto "SQL-Server-2014" o "SQL-Azure". Il valore predefinito è "SQL-Server-2008".  
  
**Esempio di sintassi:**  
  
```xml  
<create-new-project  
  
  project-folder="<project-folder>"  
  
  project-name="<project-name>"  
  
  overwrite-if-exists="<true/false>" (optional)  
  
   project-type="<sql-server-2008/sql-server-2005/sql-server-2012/sql-server-2014/sql-azure>"  
/>  
```  
Per impostazione predefinita, l'attributo ' Overwrite-IF-EXISTS ' è **false** .  
  
Per impostazione predefinita, l'attributo ' Project-Type ' è **SQL-Server-2008** .  
  
### <a name="open-project"></a>Apri progetto  
Questo comando apre il progetto.

-   `project-folder` indica la cartella del progetto da creare. Il comando ha esito negativo se la cartella specificata non esiste.  {string}  
  
-   `project-name` indica il nome del progetto. Il comando ha esito negativo se il progetto specificato non esiste.  {string}  
  
**Esempio di sintassi:**  
  
```xml  
<open-project  
  
  project-folder="<project-folder>"  
  
  project-name="<project-name>"  
  
/>  
```  
> [!NOTE]  
> L'applicazione console SSMA per SAP ASE supporta la compatibilità con le versioni precedenti. È possibile usarlo per aprire i progetti creati con la versione precedente di SSMA.  
  
### <a name="save-project"></a>save-project  
Questo comando Salva il progetto di migrazione.  
  
**Esempio di sintassi:**  
  
```xml  
<save-project/>  
```  
  
### <a name="close-project"></a>Chiudi progetto  
Questo comando chiude il progetto di migrazione.  
  
**Esempio di sintassi:**  
  
```xml  
<close-project   
  if-modified="<save/error/ignore>"   (optional)  
/>  
```  
L'attributo ' If-Modified ' è facoltativo, **Ignora** per impostazione predefinita.  
  
## <a name="database-connection-commands"></a>Comandi di connessione al database  
I comandi di connessione al database consentono di connettersi al database.  
  
> [!NOTE]  
> - La funzionalità di **esplorazione** dell'interfaccia utente non è supportata nella console di.  
> - Per ulteriori informazioni sulla creazione di file di script, vedere [creazione di file di script &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-script-files-sybasetosql.md).  
  
### <a name="connect-source-database"></a>Connect-source-database  
Questo comando esegue la connessione al database di origine e carica i metadati di livello elevato del database di origine, ma non tutti i metadati.
  
Se non è possibile stabilire la connessione all'origine, viene generato un errore e l'applicazione console interrompe l'esecuzione.
  
La definizione del server viene recuperata dall'attributo Name definito per ogni connessione nella sezione server del file di connessione del server o del file di script.  
  
**Esempio di sintassi:**  
  
```xml  
<connect-source-database  server="<server-unique-name>"/>  
```  
  
### <a name="force-load-sourcetarget-database"></a>Force-Load-source/target-database  
Questo comando carica i metadati di origine ed è utile per lavorare al progetto di migrazione offline.  
  
Se non è possibile stabilire la connessione all'origine/destinazione, viene generato un errore e l'applicazione console interrompe l'esecuzione.  
  
Questo comando richiede uno o più nodi della metabase come parametro della riga di comando.  
  
**Esempio di sintassi:**  
  
```xml  
<force-load metabase="<source/target>" >  
  
  <metabase-object object-name="<object-name>"/>  
  
</force-load>  
```  
  
### <a name="reconnect-source-database"></a>Riconnetti-origine-database  
Questo comando si riconnette al database di origine, ma non carica i metadati, a differenza del comando Connect-source-database.  
  
Se non è possibile stabilire una connessione con l'origine, viene generato un errore e l'applicazione console interrompe l'esecuzione.  
  
**Esempio di sintassi:**  
  
```xml  
<reconnect-source-database  server="<server-unique-name>"/>  
```  
  
### <a name="connect-target-database"></a>Connect-target-database  
Questo comando esegue la connessione al database di SQL Server di destinazione e carica i metadati di alto livello del database di destinazione, ma non i metadati interamente.  
  
Se non è possibile stabilire la connessione alla destinazione, viene generato un errore e l'applicazione console interrompe l'esecuzione.  
  
La definizione del server viene recuperata dall'attributo Name definito per ogni connessione nella sezione server del file di connessione del server o del file di script.  
  
**Esempio di sintassi:**  
  
```xml  
<connect-target-database  server="<server-unique-name>"/>  
```  
  
### <a name="reconnect-target-database"></a>riconnessione-destinazione-database  
  
Questo comando si riconnette al database di destinazione, ma non carica i metadati, a differenza del comando Connect-target-database.  
  
Se non è possibile stabilire una connessione alla destinazione, viene generato un errore e l'applicazione console interrompe l'esecuzione.  
  
**Esempio di sintassi:**  
  
```xml  
<reconnect-target-database  server="<server-unique-name>"/>  
```  
  
## <a name="report-commands"></a>Comandi del report  
I comandi del report generano report sulle prestazioni di varie attività della console di SSMA.  
  
### <a name="generate-assessment-report"></a>generate-assessment-report  
  
Questo comando genera i report di valutazione nel database di origine.  
  
Se la connessione al database di origine non viene eseguita prima di eseguire questo comando, viene generato un errore e l'applicazione console viene chiusa.  
  
Errore durante la connessione al server di database di origine durante l'esecuzione del comando, comporta anche la terminazione dell'applicazione console.  
  
-   `conversion-report-folder:` Specifica la cartella in cui è possibile archiviare il report di valutazione. (attributo facoltativo)  
  
-   `object-name:` Specifica gli oggetti considerati per la generazione di report di valutazione (supporta singoli nomi di oggetti o un nome di oggetto di gruppo).  
  
-   `object-type:` Specifica il tipo dell'oggetto denominato nell'attributo Object-Name (se viene specificata la categoria dell'oggetto, il tipo di oggetto sarà "Category").  
  
-   `conversion-report-overwrite:` Specifica se sovrascrivere la cartella dei report di valutazione, se esiste già.  
  
    **Valore predefinito:** false. (attributo facoltativo)  
  
-   `write-summary-report-to:` Specifica il percorso in cui verrà generato il report.  
  
    Se viene indicato solo il percorso della cartella, il file in base al nome **AssessmentReport &lt; n &gt; . XML** creato. (attributo facoltativo)  
  
    Per la creazione di report sono presenti altre due sottocategorie:  
  
    -   `report-errors` (= "true/false", con il valore predefinito "false" (attributi facoltativi))  
  
    -   `verbose` (= "true/false", con il valore predefinito "false" (attributi facoltativi))  
  
**Esempio di sintassi:**  
  
```xml  
<generate-assessment-report  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"  
  
  write-summary-report-to="<file-name/folder-name>"             (optional)  
  
  verbose="<true/false>"                       (optional)  
  
  report-errors="<true/false>"                 (optional)  
  
  assessment-report-folder="<folder-name>"          (optional)  
  
  conversion-report-overwrite="<true/false>"   (optional)  
  
/>  
```  
oppure  
  
```xml  
<generate-assessment-report  
  
  assessment-report-folder="<folder-name>"            (optional)  
  
  conversion-report-overwrite="<true/false>"     (optional)  
  
>  
<metabase-object object-name="<object-name>"  
  
        object-type="<object-category>"/>  
  
</generate-assessment-report>  
```  
  
## <a name="migration-commands"></a>Comandi di migrazione  
I comandi di migrazione convertono lo schema del database di destinazione nello schema di origine e migrano i dati nel server di destinazione.  
  
### <a name="convert-schema"></a>Convert-schema  
Questo comando esegue la conversione dello schema dall'origine allo schema di destinazione.  
  
Se la connessione al database di origine o di destinazione non viene eseguita prima di eseguire questo comando o se la connessione al server di database di origine o di destinazione ha esito negativo durante l'esecuzione del comando, viene generato un errore e l'applicazione console viene chiusa.  
  
-   `conversion-report-folder:` Specifica la cartella in cui è possibile archiviare il report di valutazione. (attributo facoltativo)  
  
-   `object-name:` Specifica gli oggetti di origine considerati per la conversione dello schema (supporta singoli nomi di oggetti o un nome di oggetto di gruppo).  
  
-   `object-type:` Specifica il tipo dell'oggetto denominato nell'attributo Object-Name (se viene specificata la categoria dell'oggetto, il tipo di oggetto sarà "Category").  
  
-   `conversion-report-overwrite:` Specifica se sovrascrivere la cartella dei report di valutazione, se esiste già.  
  
    **Valore predefinito:** false. (attributo facoltativo)  
  
-   `write-summary-report-to:` Specifica il percorso in cui verrà generato il report di riepilogo.  
  
    Se viene indicato solo il percorso della cartella, il file in base al nome **SchemaConversionReport &lt; n &gt; . XML** creato. (attributo facoltativo)  
  
    Per la creazione di report sono presenti altre due sottocategorie:  
  
    -   `report-errors` (= "true/false", con il valore predefinito "false" (attributi facoltativi))  
  
    -   `verbose` (= "true/false", con il valore predefinito "false" (attributi facoltativi))  
  
**Esempio di sintassi:**  
  
```xml  
<convert-schema  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"  
  write-summary-report-to="<file-name/folder-name>"     (optional)  
  
  verbose="<true/false>"                          (optional)  
  
  report-errors="<true/false>"                    (optional)  
  
  conversion-report-folder="<folder-name>"             (optional)  
  
  conversion-report-overwrite="<true/false>"      (optional)  
  
/>  
```  
oppure  
  
```xml  
<convert-schema  
  
  conversion-report-folder="<folder-name>"         (optional)  
  
  conversion-report-overwrite="<true/false>"> (optional)  
  
  <metabase-object object-name="<object-name>"  
  
    object-type="<object-category>"/>  
  
</convert-schema>  
```  
  
### <a name="migrate-data"></a>migrate-dati  
Questo comando esegue la migrazione dei dati di origine alla destinazione.  
  
-   `object-name:` Specifica gli oggetti di origine considerati per la migrazione dei dati (supporta i singoli nomi di oggetti o il nome di un oggetto gruppo).  
  
-   `object-type:` Specifica il tipo dell'oggetto denominato nell'attributo Object-Name (se viene specificata la categoria dell'oggetto, il tipo di oggetto sarà "Category").  
  
-   `write-summary-report-to:` Specifica il percorso in cui verrà generato il report.  
  
    Se viene indicato solo il percorso della cartella, il file in base al nome **DataMigrationReport &lt; n &gt; . XML** creato. (attributo facoltativo)  
  
    Per la creazione di report sono presenti altre due sottocategorie:  
  
    -   `report-errors` (= "true/false", con il valore predefinito "false" (attributi facoltativi))  
  
    -   `verbose` (= "true/false", con il valore predefinito "false" (attributi facoltativi))  
  
**Esempio di sintassi:**  
  
```xml  
<migrate-data  
  
  write-summary-report-to="<file-name/folder-name>"  
  
  report-errors="<true/false>" verbose="<true/false>">  
  
    <metabase-object object-name="<object-name>"/>  
  
    <metabase-object object-name="<object-name>"/>  
  
    <metabase-object object-name="<object-name>"/>  
  
    <data-migration-connection  
  
      source-use-last-used="true"/source-server="<server-unique-name>"  
  
      target-use-last-used="true"/target-server="<server-unique-name>"/>  
  
</migrate-data>  
```  
oppure  
  
```xml  
<migrate-data  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"  
  
  write-summary-report-to="<file-name/folder-name>"  
  
  report-errors="<true/false>" verbose="<true/false>"/>  
```  
  
## <a name="migration-preparation-command"></a>Comando di preparazione alla migrazione  
Il comando di preparazione alla migrazione avvia il mapping dello schema tra i database di origine e di destinazione.  
  
> [!NOTE]  
> L'impostazione di output della console predefinita per i comandi di migrazione è il report di output completo senza segnalazione errori dettagliata: solo riepilogo nel nodo radice dell'albero degli oggetti di origine.  
  
### <a name="map-schema"></a>Mappa-schema  
Questo comando consente di eseguire il mapping dello schema del database di origine allo schema di destinazione.  
  
-   `source-schema` Specifica lo schema di origine di cui eseguire la migrazione.  
  
-   `sql-server-schema` Specifica lo schema di destinazione in cui verrà eseguita la migrazione dello schema di origine.  
  
**Esempio di sintassi:**  
  
```xml  
<map-schema source-schema="<source-schema>"  
  
sql-server-schema="<target-schema>"/>  
```  
  
## <a name="manageability-commands"></a>Comandi di gestibilità  
I comandi di gestibilità consentono di sincronizzare gli oggetti di database di destinazione con il database di origine.  
  
> [!NOTE]  
> L'impostazione di output della console predefinita per i comandi di migrazione è il report di output completo senza segnalazione errori dettagliata: solo riepilogo nel nodo radice dell'albero degli oggetti di origine.  
  
### <a name="synchronize-target"></a>sincronizzazione-destinazione  
Questo comando Sincronizza gli oggetti di destinazione con il database di destinazione.  
 
Se questo comando viene eseguito sul database di origine, viene visualizzato un errore.  
  
Se la connessione al database di destinazione non viene eseguita prima di eseguire questo comando o se la connessione al server di database di destinazione non riesce durante l'esecuzione del comando, viene generato un errore e l'applicazione console viene chiusa.  
  
-   `object-name:` Specifica gli oggetti di destinazione considerati per la sincronizzazione con il database di destinazione (supporta singoli nomi di oggetti o un nome di oggetto di gruppo).  
  
-   `object-type:` Specifica il tipo dell'oggetto denominato nell'attributo Object-Name (se viene specificata la categoria dell'oggetto, il tipo di oggetto sarà "Category").  
  
-   `on-error:` Specifica se specificare gli errori di sincronizzazione come avvisi o errori. Opzioni disponibili per l'errore:  
  
    -   report-totale-come-avviso  
  
    -   report-each-As-Warning  
  
    -   script di errore  
  
-   `report-errors-to:` Specifica la posizione della segnalazione errori per l'operazione di sincronizzazione (attributo facoltativo). Se viene specificato solo il percorso della cartella, viene creato il file in base al nome **TargetSynchronizationReport.XML** .  
  
**Esempio di sintassi:**  
  
```xml  
<synchronize-target  
  
object-name="<object-name>"  
  
on-error="<report-total-as-warning/  
  
report-each-as-warning/  
  
fail-script>" (optional)  
  
  report-errors-to="<file-name/folder-name>"        (optional)  
  
/>  
```  
oppure  
  
```xml  
<synchronize-target  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"/>  
```  
oppure  
  
```xml  
<synchronize-target>  
  
  <metabase-object object-name="<object-name>"/>  
  
  <metabase-object object-name="<object-name>"/>  
  
  <metabase-object object-name="<object-name>"/>  
  
</synchronize-target>  
```  
  
### <a name="refresh-from-database"></a>aggiornamento da database  
Questo comando Aggiorna gli oggetti di origine dal database.  
  
Se questo comando viene eseguito sul database di destinazione, viene generato un errore.  
  
Questo comando richiede uno o più nodi della metabase come parametro della riga di comando.  
  
-   `object-name:` Specifica gli oggetti di origine considerati per l'aggiornamento dal database di origine (supporta singoli nomi di oggetti o un nome di oggetto di gruppo).  
  
-   `object-type:` Specifica il tipo dell'oggetto specificato nell'attributo nome-oggetto (se viene specificata la categoria dell'oggetto, il tipo di oggetto sarà "Category").  
  
-   `on-error:` Specifica se chiamare gli errori di aggiornamento come avvisi o errori. Opzioni disponibili per l'errore:  
  
    -   report-totale-come-avviso  
  
    -   report-each-As-Warning  
  
    -   script di errore  
  
-   `report-errors-to:` Specifica la posizione della segnalazione errori per l'operazione di aggiornamento (attributo facoltativo). Se viene specificato solo il percorso della cartella, viene creato il file in base al nome **SourceDBRefreshReport.XML** .  
  
**Esempio di sintassi:**  
  
```xml  
<refresh-from-database  
  
  object-name="<object-name>"  
  
  on-error="<report-total-as-warning/  
  
             report-each-as-warning/  
  
             fail-script>"              (optional)  
  
  report-errors-to="<file-name/folder-name>"        (optional)  
  
/>  
```  
oppure  
  
```xml  
<refresh-from-database  
  
  object-name="<object-name>"  
  
  object-type="<object-category>" />  
```  
oppure  
  
```xml  
<refresh-from-database>  
  
  <metabase-object object-name="<object-name>"/>  
  
</refresh-from-database>  
```  
  
## <a name="script-generation-commands"></a>Comandi di generazione script  
I comandi di generazione script eseguono due attività: consentono di salvare l'output della console in un file di script e di registrare l'output T-SQL nella console o in un file in base al parametro specificato.  
  
### <a name="save-as-script"></a>Salva come script  
Questo comando viene usato per salvare gli script degli oggetti in un file indicato quando metabase = target. Si tratta di un'alternativa al comando di sincronizzazione, che consente di ottenere gli script ed eseguire lo stesso nel database di destinazione.  
  
Questo comando richiede uno o più nodi della metabase come parametro della riga di comando.  
  
-   `object-name:` Specifica gli oggetti i cui script devono essere salvati (supporta singoli nomi di oggetti o un nome di oggetto di gruppo).  
  
-   `object-type:` Specifica il tipo dell'oggetto denominato nell'attributo Object-Name (se viene specificata la categoria dell'oggetto, il tipo di oggetto sarà "Category").  
  
-   `metabase:` Specifica se si tratta della metabase di origine o di destinazione.  
  
-   `destination:` Specifica il percorso o la cartella in cui deve essere salvato lo script. Se il nome file non viene specificato, viene specificato un nome file nel formato (object_name valore dell'attributo). out verrà fornito.
  
-   `overwrite:` Se true, sovrascrive lo stesso nome file, se esistente. Può contenere i valori (true/false).  
  
**Esempio di sintassi:**  
  
```xml  
<save-as-script  
  
  metabase="<source/target>"  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"  
  
  destination="<file-name/folder-name>"  
  
  overwrite="<true/false>"   (optional)  
  
/>  
```  
oppure  
  
```xml  
<save-as-script  
  
  metabase="<source/target>"  
  
  destination="<file-name/folder-name>"  
  
    <metabase-object object-name="<object-name>"  
  
                     object-type="<object-category>"/>  
  
</save-as-script>  
```  
  
### <a name="convert-sql-statement"></a>Convert-SQL-statement
Questo comando converte l'istruzione SQL.  
  
-   `context` Specifica il nome dello schema.  
  
-   `destination` Specifica se l'output deve essere archiviato in un file.  
  
    Se questo attributo non viene specificato, l'istruzione T-SQL convertita viene visualizzata nella console. (attributo facoltativo)  
  
-   `conversion-report-folder` Specifica la cartella in cui è possibile archiviare il report di valutazione. (attributo facoltativo)  
  
-   `conversion-report-overwrite` Specifica se sovrascrivere la cartella dei report di valutazione, se esiste già.  
  
    **Valore predefinito:** false. (attributo facoltativo)  
  
-   `write-converted-sql-to` consente di specificare il percorso della cartella del file (o) in cui deve essere archiviata l'istruzione T-SQL convertita. Quando si specifica un percorso di cartella insieme all' `sql-files` attributo, per ogni file di origine è stato creato un file T-SQL di destinazione corrispondente nella cartella specificata. Quando si specifica un percorso di cartella insieme all' `sql` attributo, l'istruzione T-SQL convertita viene scritta in un file denominato Result. out nella cartella specificata.  
  
-   `sql` Specifica le istruzioni SQL di Sybase da convertire, una o più istruzioni possono essere separate mediante ";"  
  
-   `sql-files` Specifica il percorso dei file SQL che devono essere convertiti nel codice T-SQL.  
  
-   `write-summary-report-to` Specifica il percorso in cui verrà generato il report di riepilogo. Se viene indicato solo il percorso della cartella, viene creato il file in base al nome **ConvertSQLReport.XML** . (attributo facoltativo)  
  
    La creazione di report di riepilogo presenta due sottocategorie, ovvero:  
  
    -   Report-Errors (= "true/false", con il valore predefinito "false" (attributi facoltativi)).  
  
    -   Verbose (= "true/false", con il valore predefinito "false" (attributi facoltativi)).  
  
Questo comando richiede uno o più nodi della metabase come parametro della riga di comando.  
  
**Esempio di sintassi:**  
  
```  
<convert-sql-statement  
  
       context="<database-name>.<schema-name>"  
  
        conversion-report-folder="<folder-name>"  
  
        conversion-report-overwrite="<true/false>"  
  
        write-summary-report-to="<file-name/folder-name>"   (optional)  
  
        verbose="<true/false>"   (optional)  
  
        report-errors="<true/false>"   (optional)  
  
        destination="<stdout/file>"   (optional)  
  
        write-converted-sql-to ="<file-name/folder-name>"  
  
        sql="SELECT 1 FROM DUAL;">  
  
    <output-window suppress-messages="<true/false>" />  
  
</convert-sql-statement>  
```  
oppure  
  
```  
<convert-sql-statement  
  
         context="<database-name>.<schema-name>"  
  
         conversion-report-folder="<folder-name>"  
  
         conversion-report-overwrite="<true/false>"  
  
         write-summary-report-to="<file-name/folder-name>"   (optional)  
  
         verbose="<true/false>"   (optional)  
  
         report-errors="<true/false>"   (optional)  
  
         destination="<stdout/file>"   (optional)  
  
         write-converted-sql-to ="<file-name/folder-name>"  
  
         sql-files="<folder-name>\*.sql"  
  
/>  
```  
oppure  
  
```  
<convert-sql-statement  
  
         context="<database-name>.<schema-name>"  
  
         conversion-report-folder="<folder-name>"  
  
         conversion-report-overwrite="<true/false>"  
  
         sql-files="<folder-name>\*.sql"  
  
/>  
```  
  
## <a name="next-steps"></a>Passaggi successivi  
Per informazioni sulle opzioni della riga di comando, vedere [Opzioni della riga di comando nella console SSMA (AccessToSQL)](../access/command-line-options-in-ssma-console-accesstosql.md).  
  
Per informazioni su un file script della console di esempio, vedere [uso dei file script della console di esempio &#40;SybaseToSQL&#41;](../../ssma/sybase/working-with-the-sample-console-script-files-sybasetosql.md)  
  
Il passaggio successivo dipende dai requisiti del progetto:  
  
-   Per specificare una password o le password di esportazione/importazione, vedere Gestione delle password [&#40;SybaseToSQL&#41;](../../ssma/sybase/managing-passwords-sybasetosql.md).  
  
-   Per la generazione di report, vedere [generazione di report &#40;&#41;SybaseToSQL ](../../ssma/sybase/generating-reports-sybasetosql.md).  
  
-   Per la risoluzione dei problemi nella console di, vedere [troubleshooting &#40;SybaseToSQL&#41;](../../ssma/sybase/troubleshooting-sybasetosql.md).  
  
