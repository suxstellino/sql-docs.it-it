---
description: Esecuzione della console SSMA (MySQLToSQL)
title: Esecuzione della console SSMA (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Script file commands, Database connection commands
- Script file commands, Manageability commands
- Script file commands, Migration commands
- Script file commands, Migration preparation commands
- Script file commands, project commands
- Script file commands, Report commands
- Script file commands, Script generation commands
ms.assetid: e3e9f7e4-0619-4861-a202-3d5d39953b26
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 9f9fa74533d7887f968d45dde715b191ac00be88
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015176"
---
# <a name="executing-the-ssma-console-mysqltosql"></a>Esecuzione della console SSMA (MySQLToSQL)
Microsoft offre un solido set di comandi di file script per eseguire e controllare le attività SSMA.  
  
L'applicazione console USA determinati comandi del file di script standard come enumerati in questa sezione.  
  
## <a name="project--script-file-commands"></a>Comandi del file di script di progetto  
**Comando**  
  
Creazione-nuovo-progetto:   
                   Consente di creare un nuovo progetto SSMA.  
  
I comandi di progetto gestiscono la creazione di progetti, l'apertura, il salvataggio e l'uscita di progetti.  
  
**Script**  
  
1.  `project-folder` indica la cartella del progetto da creare.  
  
2.  `project-name` indica il nome del progetto. {string}  
  
3.  `overwrite-if-exists`L'attributo facoltativo indica se è necessario sovrascrivere un progetto esistente. Boolean  
  
4.  `project-type:`Attributo facoltativo. Indica il tipo di progetto, ad esempio il progetto "SQL-Server-2005" o "SQL-Server-2008" oppure il progetto "SQL-Server-2012" o "SQL-Server-2014" o "SQL-Azure". Il valore predefinito è "SQL-Server-2008".  
  
**Esempio di sintassi:**  
  
```xml  
<create-new-project  
  
   project-folder="<project-folder>"  
  
   project-name="<project-name>"  
  
   overwrite-if-exists="<true/false>"   (optional)  
  
   project-type=="<sql-server-2008 | sql-server-2005 | sql-server-2012 | sql-server-2014 | sql-azure>"   (optional)  
  
/>  
```  
Per impostazione predefinita, l'attributo ' Overwrite-IF-EXISTS ' è **false** .  
  
Per impostazione predefinita, l'attributo ' Project-Type ' è **SQL-Server-2008** .  
  
**Comando**  
  
Apri progetto:   
                  Apre un progetto esistente.  
  
**Script**  
  
1.  `project-folder` indica la cartella del progetto da creare. Il comando ha esito negativo se la cartella specificata non esiste.  {string}  
  
2.  `project-name` indica il nome del progetto. Il comando ha esito negativo se il progetto specificato non esiste.  {string}  
  
**Esempio di sintassi:**  
  
```xml  
<open-project  
  
   project-folder="<project-folder>"  
  
   project-name="<project-name>"  
  
/>  
```  
> [!IMPORTANT]  
> SSMA per l'applicazione console MySQL supporta la compatibilità con le versioni precedenti. Sarà possibile aprire i progetti creati con la versione precedente di SSMA.  
  
**Comando**  
  
Salva-progetto: Salva il progetto di migrazione.  
  
**Script**  
  
**Esempio di sintassi:**  
  
```xml  
<save-project/>  
```  
**Comando**  
  
Chiudi progetto  
                  : Chiude il progetto di migrazione.  
  
**Script**  
  
**Esempio di sintassi:**  
  
```xml  
<save-project/>  
```  
**Comando**  
  
Chiudi progetto  
                  : Chiude il progetto di migrazione.  
  
**Script**  
  
**Esempio di sintassi:**  
  
```xml  
<close-project  
  
   if-modified="<save/error/ignore>"   (optional)  
  
/>  
```  
L'attributo ' If-Modified ' è facoltativo, **Ignora** per impostazione predefinita.  
  
## <a name="database-connection-script-file-commands"></a>Comandi del file di script di connessione del database  
I comandi di connessione al database consentono di connettersi al database.  
  
1.  La funzionalità di **esplorazione** dell'interfaccia utente non è supportata nella console di.  
  
2.  I parametri di autenticazione e di **porta** di **Windows** non sono applicabili quando ci si connette a SQL Azure.  
  
3.  Per ulteriori informazioni sulla creazione di file di script, vedere [creazione di file di script &#40;MySQLToSQL&#41;](../../ssma/mysql/creating-script-files-mysqltosql.md).  
  
**Comando**  
  
Connect-source-database  
  
-   Esegue la connessione al database di origine e carica i metadati di livello elevato del database di origine, ma non tutti i metadati.  
  
-   Se non è possibile stabilire la connessione all'origine, viene generato un errore e l'applicazione console interrompe l'esecuzione.  
  
**Script**  
  
La definizione del server viene recuperata dall'attributo Name definito per ogni connessione nella sezione server del file di connessione del server o del file di script.  
  
**Esempio di sintassi:**  
  
```xml  
<connect-source-database  server="<server-unique-name>"/>  
```  
**Comando**  
  
Force-Load-source/target-database  
  
-   Carica i metadati di origine.  
  
-   Utile per lavorare al progetto di migrazione offline.  
  
-   Se non è possibile stabilire la connessione all'origine/destinazione, viene generato un errore e l'applicazione console interrompe l'esecuzione.  
  
**Script**  
  
Richiede uno o più nodi della metabase come parametro della riga di comando.  
  
**Esempio di sintassi:**  
  
```xml  
<force-load metabase="<source/target>"  
  
      <metabase-object object-name="<object-name>"/>  
  
</force-load>  
```  
**Comando**  
  
Riconnetti-origine-database  
  
1.  Si riconnette al database di origine, ma non carica i metadati, a differenza del comando Connect-source-database.  
  
2.  Se non è possibile stabilire una connessione con l'origine, viene generato un errore e l'applicazione console interrompe l'esecuzione.  
  
**Script**  
  
**Esempio di sintassi:**  
  
```xml  
<reconnect-source-database  server="<server-unique-name>"/>  
```  
**Comando**  
  
Connect-target-database  
  
1.  Si connette al SQL Server di destinazione o al database SQL di Azure e carica i metadati di alto livello del database di destinazione, ma non i metadati interamente.  
  
2.  Se non è possibile stabilire la connessione alla destinazione, viene generato un errore e l'applicazione console interrompe l'esecuzione.  
  
**Script**  
  
La definizione del server viene recuperata dall'attributo Name definito per ogni connessione nella sezione server del file di connessione del server o del file di script  
  
**Esempio di sintassi:**  
  
```xml  
<connect-target-database  server="<server-unique-name>"/>  
```  
**Comando**  
  
riconnessione-destinazione-database  
  
1.  Si riconnette al database di destinazione, ma non carica i metadati, a differenza del comando Connect-target-database.  
  
2.  Se non è possibile stabilire una connessione alla destinazione, viene generato un errore e l'applicazione console interrompe l'esecuzione.  
  
**Script**  
  
**Esempio di sintassi:**  
  
```xml  
<reconnect-target-database  server="<server-unique-name>"/>  
```  
  
## <a name="report-script-file-commands"></a>Comandi del file di script del report  
I comandi del report generano report sulle prestazioni di varie attività della console di SSMA.  
  
**Comando**  
  
generate-assessment-report  
  
1.  Genera report di valutazione nel database di origine.  
  
2.  Se la connessione al database di origine non viene eseguita prima di eseguire questo comando, viene generato un errore e l'applicazione console viene chiusa.  
  
3.  Errore durante la connessione al server di database di origine durante l'esecuzione del comando, comporta anche la terminazione dell'applicazione console.  
  
**Script**  
  
1.  `assessment-report-folder:` Specifica la cartella in cui è archiviato il report di valutazione. (attributo facoltativo)  
  
2.  `object-name:` Specifica gli oggetti considerati per la generazione di report di valutazione. può includere nomi di oggetti singoli o un nome di oggetto gruppo.  
  
3.  `object-type:` Specifica il tipo dell'oggetto specificato nell'attributo nome-oggetto (se viene specificata la categoria dell'oggetto, il tipo di oggetto sarà "Category").  
  
4.  `assessment-report-overwrite:` Specifica se sovrascrivere la cartella dei report di valutazione, se esiste già.  
  
    **Valore predefinito:** false. (attributo facoltativo)  
  
5.  `write-summary-report-to:` Specifica il percorso in cui verrà generato il report di riepilogo.  
  
    Se viene indicato solo il percorso della cartella, il file in base al nome **AssessmentReport &lt; n &gt; . XML** creato. (attributo facoltativo)  
  
    Per la creazione di report sono presenti due sottocategorie:  
  
    -   `report-errors` (= "true/false", con il valore predefinito "false" (attributi facoltativi))  
  
    -   `verbose` (= "true/false", con il valore predefinito "false" (attributi facoltativi))  
  
**Esempio di sintassi:**  
  
```xml  
<generate-assessment-report  
  
   object-name="<object-name>"  
  
   object-type="<object-category>"  
  
   write-summary-report-to="<file-name/folder-name>"   (optional)  
  
   verbose="<true/false>"   (optional)  
  
   report-errors="<true/false>"   (optional)  
  
   conversion-report-folder="<folder-name>"   (optional)  
  
   conversion-report-overwrite="<true/false>"   (optional)  
  
/>  
```  
oppure  
  
```xml  
<generate-assessment-report  
  
   conversion-report-folder="<folder-name>"   (optional)  
  
   conversion-report-overwrite="<true/false>"   (optional)  
  
>  
  
      <metabase-object object-name="<object-name>"  
  
         object-type="<object-category>"/>  
  
</generate-assessment-report>  
```  
  
## <a name="migration--script-file-commands"></a>Comandi del file di script di migrazione  
I comandi di migrazione convertono lo schema del database di destinazione nello schema di origine ed esegue la migrazione dei dati al server di destinazione.  
  
L'impostazione di output della console predefinita per i comandi di migrazione è il report di output completo senza segnalazione errori dettagliata: solo riepilogo nel nodo radice dell'albero degli oggetti di origine.  
  
**Comando**  
  
Convert-schema  
  
1.  Esegue la conversione dello schema dall'origine allo schema di destinazione.  
  
2.  Se la connessione al database di origine o di destinazione non viene eseguita prima di eseguire questo comando o se la connessione al server di database di origine o di destinazione ha esito negativo durante l'esecuzione del comando, viene generato un errore e l'applicazione console viene chiusa.  
  
**Script**  
  
1.  `conversion-report-folder:` Specifica la cartella in cui è archiviato il report di valutazione. (attributo facoltativo)  
  
2.  `object-name:` Specifica gli oggetti considerati per la conversione dello schema. può contenere nomi di oggetti singoli o un nome di oggetto gruppo.  
  
3.  `object-type:` Specifica il tipo dell'oggetto specificato nell'attributo nome-oggetto (se viene specificata la categoria dell'oggetto, il tipo di oggetto sarà "Category").  
  
4.  `conversion-report-overwrite:` Specifica se sovrascrivere la cartella dei report di valutazione, se esiste già.  
  
    **Valore predefinito:** false. (attributo facoltativo)  
  
5.  `write-summary-report-to:` Specifica il percorso in cui verrà generato il report di riepilogo.  
  
    Se viene indicato solo il percorso della cartella, il file in base al nome **SchemaConversionReport &lt; n &gt; . XML** creato. (attributo facoltativo)  
  
    La creazione di report di riepilogo presenta due sottocategorie aggiuntive:  
  
    -   `report-errors` (= "true/false", con il valore predefinito "false" (attributi facoltativi))  
  
    -   `verbose` (= "true/false", con il valore predefinito "false" (attributi facoltativi))  
  
**Esempio di sintassi:**  
  
```xml  
<convert-schema  
  
   object-name="<object-name>"  
  
   object-type="<object-category>"  
  
   write-summary-report-to="<file-name/folder-name>"   (optional)  
  
   verbose="<true/false>"   (optional)  
  
   report-errors="<true/false>"   (optional)  
  
   conversion-report-folder="<folder-name>"   (optional)  
  
   conversion-report-overwrite="<true/false>"   (optional)  
  
/>  
```  
oppure  
  
```xml  
<convert-schema  
  
   conversion-report-folder="<folder-name>"   (optional)  
  
   conversion-report-overwrite="<true/false>"   (optional)  
  
      <metabase-object object-name="<object-names>"  
  
         object-type="<object-category>"/>  
  
</convert-schema>  
```  
**Comando**  
  
migrate-dati  
  
1.  Esegue la migrazione dei dati di origine alla destinazione.  
  
**Script**  
  
1.  `object-name:` Specifica gli oggetti di origine considerati per la migrazione dei dati. può avere nomi di oggetti singoli o un nome di oggetto gruppo.  
  
2.  `object-type:` Specifica il tipo dell'oggetto specificato nell'attributo nome-oggetto (se viene specificata la categoria dell'oggetto, il tipo di oggetto sarà "Category").  
  
3.  `write-summary-report-to:` Specifica il percorso in cui verrà generato il report di riepilogo.  
  
    Se viene indicato solo il percorso della cartella, il file in base al nome **DataMigrationReport &lt; n &gt; . XML** creato. (attributo facoltativo)  
  
    Per la creazione di report sono presenti due sottocategorie:  
  
    -   `report-errors` (= "true/false", con il valore predefinito "false" (attributi facoltativi))  
  
    -   `verbose` (= "true/false", con il valore predefinito "false" (attributi facoltativi))  
  
**Esempio di sintassi:**  
  
```xml  
<migrate-data  
  
   write-summary-report-to="<file-name/folder-name>"  
  
   report-errors="true" verbose="true">  
  
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
  
   report-errors="true" verbose="true"/>  
```  
  
## <a name="migration-preparation-script-file-command"></a>Comando file script di preparazione della migrazione  
Il comando di preparazione alla migrazione avvia il mapping dello schema tra i database di origine e di destinazione.  
  
**Comando**  
  
Mappa-schema  
  
Mapping dello schema del database di origine allo schema di destinazione.  
  
**Script**  
  
1.  `source-schema` Specifica lo schema di origine di cui si intende eseguire la migrazione.  
  
2.  `sql-server-schema` Specifica lo schema di destinazione in cui si desidera eseguire la migrazione.  
  
**Esempio di sintassi:**  
  
```xml  
<map-schema  
  
   source-schema="<source-schema>"  
  
   sql-server-schema="<target-schema>"/>  
```  
  
## <a name="manageability-script-file-commands"></a>Comandi del file di script di gestibilità  
I comandi di gestibilità consentono di sincronizzare gli oggetti di database di destinazione con il database di origine.  
  
> [!NOTE]  
> L'impostazione di output della console predefinita per i comandi di migrazione è il report di output completo senza segnalazione errori dettagliata: solo riepilogo nel nodo radice dell'albero degli oggetti di origine.  
  
**Comando**  
  
sincronizzazione-destinazione  
  
1.  Sincronizza gli oggetti di destinazione con il database di destinazione.  
  
2.  Se questo comando viene eseguito sul database di origine, viene visualizzato un errore.  
  
3.  Se la connessione al database di destinazione non viene eseguita prima di eseguire questo comando o se la connessione al server di database di destinazione non riesce durante l'esecuzione del comando, viene generato un errore e l'applicazione console viene chiusa.  
  
**Script**  
  
1.  `object-name:` Specifica gli oggetti considerati per la sincronizzazione con il database di destinazione (può avere singoli nomi di oggetto o un nome di oggetto gruppo).  
  
2.  `object-type:` Specifica il tipo dell'oggetto specificato nell'attributo nome-oggetto (se viene specificata la categoria dell'oggetto, il tipo di oggetto sarà "Category").  
  
3.  `on-error:` Specifica se specificare gli errori di sincronizzazione come avvisi o errori. Opzioni disponibili per l'errore:  
  
    -   report-totale-come-avviso  
  
    -   report-each-As-Warning  
  
    -   script di errore  
  
4.  `report-errors-to:` Specifica la posizione della segnalazione errori per l'operazione di sincronizzazione (attributo facoltativo) se viene specificato solo il percorso della cartella, quindi viene creato il file in base al nome **TargetSynchronizationReport.XML** .  
  
**Esempio di sintassi:**  
  
```xml  
<synchronize-target  
  
   object-name="<object-name>"  
  
   on-error="<report-total-as-warning/  
  
               report-each-as-warning/  
  
               fail-script>"   (optional)  
  
   report-errors-to="<file-name>"   (optional)  
  
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
**Comando**  
  
aggiornamento da database  
  
1.  Aggiorna gli oggetti di origine dal database.  
  
2.  Se questo comando viene eseguito sul database di destinazione, viene generato un errore.  
  
**Script**  
  
1.  `object-name:` Specifica gli oggetti di origine considerati per l'aggiornamento dal database di origine. può contenere nomi di oggetti singoli o un nome di oggetto gruppo.  
  
2.  `object-type:` Specifica il tipo dell'oggetto specificato nell'attributo nome-oggetto (se viene specificata la categoria dell'oggetto, il tipo di oggetto sarà "Category").  
  
3.  `on-error:` Specifica se specificare gli errori di sincronizzazione come avvisi o errori. Opzioni disponibili per l'errore:  
  
    -   report-totale-come-avviso  
  
    -   report-each-As-Warning  
  
    -   script di errore  
  
4.  `report-errors-to:` Specifica la posizione della segnalazione errori per l'operazione di sincronizzazione (attributo facoltativo) se viene specificato solo il percorso della cartella, quindi viene creato il file in base al nome **SourceDBRefreshReport.XML** .  
  
Richiede uno o più nodi della metabase come parametro della riga di comando.  
  
**Esempio di sintassi:**  
  
```xml  
<refresh-from-database  
  
   object-name="<object-name>"  
  
   on-error="<report-total-as-warning/  
  
               report-each-as-warning/  
  
               fail-script>"   (optional)  
  
   report-errors-to="<file-name>"   (optional)  
  
/>  
```  
oppure  
  
```xml  
<refresh-from-database  
  
   object-name="<object-name>"  
  
   object-type="<object-category>"/>  
```  
oppure  
  
```xml  
<refresh-from-database>  
  
   <metabase-object object-name="<object-name>"/>  
  
</refresh-from-database>  
```  
  
## <a name="script-generation-script-file-commands"></a>Comandi del file script di generazione script  
I comandi di generazione script eseguono due attività: consentono di salvare l'output della console in un file di script. e registrare l'output T-SQL nella console o in un file in base al parametro specificato.  
  
**Comando**  
  
Salva come script  
  
Usato per salvare gli script degli oggetti in un file indicato quando metabase = target, si tratta di un'alternativa al comando di sincronizzazione in cui si ottengono gli script ed eseguono gli stessi nel database di destinazione.  
  
**Script**  
  
Richiede uno o più nodi della metabase come parametro della riga di comando.  
  
1.  `object-name:` Specifica gli oggetti i cui script devono essere salvati. (Può avere nomi di oggetti singoli o un nome di oggetto gruppo)  
  
2.  `object-type:` Specifica il tipo dell'oggetto specificato nell'attributo nome-oggetto (se viene specificata la categoria dell'oggetto, il tipo di oggetto sarà "Category").  
  
3.  `metabase:` Specifica se si tratta della metabase di origine o di destinazione.  
  
4.  `destination:` Specifica il percorso o la cartella in cui deve essere salvato lo script, se il nome file non è specificato, un nome file nel formato (object_name valore dell'attributo). out  
  
5.  `overwrite:` Se true, sovrascrive se esiste lo stesso nome file. Può contenere i valori (true/false).  
  
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
**Comando**  
  
Convert-SQL-statement  
  
1.  `context` Specifica il nome dello schema.  
  
2.  `destination` Specifica se l'output deve essere archiviato in un file.  
  
    Se questo attributo non viene specificato, l'istruzione T-SQL convertita viene visualizzata nella console. (attributo facoltativo)  
  
3.  `conversion-report-folder` Specifica la cartella in cui è archiviato il report di valutazione. (attributo facoltativo)  
  
4.  `conversion-report-overwrite` Specifica se sovrascrivere la cartella dei report di valutazione, se esiste già.  
  
    **Valore predefinito:** false. (attributo facoltativo)  
  
5.  `write-converted-sql-to` Specifica il percorso della cartella del file (o) in cui deve essere archiviata l'istruzione T-SQL convertita. Quando si specifica un percorso di cartella insieme all' `sql-files` attributo, per ogni file di origine sarà presente un file T-SQL di destinazione corrispondente creato nella cartella specificata. Quando si specifica un percorso di cartella insieme all' `sql` attributo, l'istruzione T-SQL convertita viene scritta in un file denominato Result. out nella cartella specificata.  
  
6.  `sql` Specifica le istruzioni SQL MySQL da convertire, una o più istruzioni possono essere separate mediante ";"  
  
7.  `sql-files` Specifica il percorso dei file SQL che devono essere convertiti nel codice T-SQL.  
  
8.  `write-summary-report-to` Specifica il percorso in cui verrà generato il report di riepilogo. Se viene indicato solo il percorso della cartella, viene creato il file in base al nome **ConvertSQLReport.XML** . (attributo facoltativo)  
  
    La creazione di report include 2 sottocategorie aggiuntive, vale a dire...:  
  
    -   Report-Errors (= "true/false", con il valore predefinito "false" (attributi facoltativi)).  
  
    -   Verbose (= "true/false", con il valore predefinito "false" (attributi facoltativi)).  
  
**Script**  
  
Richiede uno o più nodi della metabase come parametro della riga di comando.  
  
**Esempio di sintassi:**  
  
```  
<convert-sql-statement  
  
   context="<schema-name>"  
  
   conversion-report-folder="<folder-name>"  
  
   conversion-report-overwrite="<true/false>"  
  
   write-summary-report-to="<file-name/folder-name>"   (optional)  
  
   verbose="<true/false>"   (optional)  
  
   report-errors="<true/false>"   (optional)  
  
   destination="stdout/file"   (optional)  
  
   write-converted-sql-to="<file-name/folder-name>"  
  
   sql="SELECT 1 FROM DUAL;">  
  
      <output-window suppress-messages="<true/false>" />  
  
</convert-sql-statement>  
```  
oppure  
  
```  
<convert-sql-statement  
  
   context="<schema-name>"  
  
   conversion-report-folder="<folder-name>"  
  
   conversion-report-overwrite="<true/false>"  
  
   write-summary-report-to="<file-name/folder-name>"   (optional)  
  
   verbose="<true/false>"   (optional)  
  
   report-errors="<true/false>"   (optional)  
  
   destination="<stdout/file>"   (optional)  
  
   write-converted-sql-to="<file-name/folder-name>"  
  
   sql-files="<folder-name>\*.sql"  
  
/>  
```  
oppure  
  
```  
<convert-sql-statement  
  
   context="<schema-name>"  
  
   conversion-report-folder="<folder-name>"  
  
   conversion-report-overwrite="<true/false>"  
  
   sql-files="<folder-name>\*.sql"  
  
/>  
```  
  
## <a name="next-step"></a>passaggio successivo  
Per informazioni sulle opzioni della riga di comando, vedere [Opzioni della riga di comando nella console SSMA &#40;MySQLToSQL&#41;](../../ssma/mysql/command-line-options-in-ssma-console-mysqltosql.md) .  
  
Per altre informazioni sui file script della console di esempio, vedere [uso dei file script della console di esempio &#40;MySQLToSQL&#41;](../../ssma/mysql/working-with-the-sample-console-script-files-mysqltosql.md)  
  
Il passaggio successivo dipende dai requisiti del progetto:  
  
1.  Per specificare una password o le password di esportazione/importazione, vedere Gestione delle password [&#40;MySQLToSQL&#41;](../../ssma/mysql/managing-passwords-mysqltosql.md).  
  
2.  Per la generazione di report, vedere [generazione di report &#40;&#41;MySQLToSQL ](../../ssma/mysql/generating-reports-mysqltosql.md).  
  
3.  Per la risoluzione dei problemi nella console di, vedere [troubleshooting &#40;MySQLToSQL&#41;](../../ssma/mysql/troubleshooting-mysqltosql.md).  
  
