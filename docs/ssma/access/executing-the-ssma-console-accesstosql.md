---
description: Esecuzione della console SSMA (AccessToSQL)
title: Esecuzione della console SSMA (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: aa1bf665-8dc0-4259-b36f-46ae67197a43
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: cf369ee1104f4b686dc0e7b6ab4bbde7bab458de
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044701"
---
# <a name="executing-the-ssma-console-accesstosql"></a>Esecuzione della console SSMA (AccessToSQL)
Microsoft offre un solido set di comandi del file di script e opzioni della riga di comando per eseguire e controllare le attività SSMA. Le sezioni seguenti illustrano in dettaglio lo stesso.  
  
## <a name="project--script-file-commands"></a>Comandi del file di script di progetto  
I comandi di progetto gestiscono la creazione di progetti, l'apertura, il salvataggio e l'uscita di progetti.  
  
**Comando**  
  
Create-New-Project: crea un nuovo progetto SSMA.  
  
**Script**  
  
-   `project-folder` indica la cartella del progetto da creare.  
  
-   `project-name` indica il nome del progetto. {string}  
  
-   `overwrite-if-exists`L'attributo facoltativo indica se è necessario sovrascrivere un progetto esistente. Boolean  
  
-   `project-type` è un attributo facoltativo.  Per il tipo di progetto sono disponibili le opzioni seguenti:  
  
    -   SQL-Server-2005  
  
    -   SQL-Server-2008  
  
    -   SQL-Server-2012  
  
    -   SQL-Server-2014  
  
    -   SQL-Server-2016  
  
    -   SQL-Azure  
  
    Il valore predefinito è "SQL-Server-2008".  
  
**Esempio:**  
  
```xml  
<create-new-project  
  
  project-folder="<project-folder>"  
  
  project-name="<project-name>"  
  
  overwrite-if-exists="<true/false>"  
  
  project-type="<sql-server-2008 | sql-server-2005 | sql-server-2012 | sql-server-2014 | sql-azure>"  
  
/>  
```  
Per impostazione predefinita, l'attributo ' Overwrite-IF-EXISTS ' è **false** .  
  
Per impostazione predefinita, l'attributo ' Project-Type ' è **SQL-Server-2008** .  
  
**Comando**  
  
Apri-progetto: apre un progetto esistente.  
  
**Script**  
  
-   `project-folder` indica la cartella del progetto da creare. Il comando ha esito negativo se la cartella specificata non esiste.  {string}  
  
-   `project-name` indica il nome del progetto. Il comando ha esito negativo se il progetto specificato non esiste.  {string}  
  
**Esempio di sintassi:**  
  
```xml  
<open-project  
  
  project-folder="<project-folder>"  
  
  project-name="<project-name>"  
  
/>  
```  
**Nota:** SSMA per l'applicazione console di accesso supporta la compatibilità con le versioni precedenti. È possibile aprire i progetti creati con la versione precedente di SSMA.  
  
**Comando**  
  
Salva-progetto: Salva il progetto di migrazione.  
  
**Script**  
  
**Esempio di sintassi:**  
  
```xml  
<save-project/>  
```  
**Comando**  
  
Close-Project: chiude il progetto di migrazione.  
  
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
  
La funzionalità di **esplorazione** dell'interfaccia utente non è supportata nella console di.  
  
I parametri di autenticazione e di **porta** di **Windows** non sono applicabili quando ci si connette a SQL Azure.  
  
Per ulteriori informazioni sulla creazione di file di script, vedere [creazione di file di script &#40;AccessToSQL&#41;](../../ssma/access/creating-script-files-accesstosql.md).  
  
**Comando**
  
Connect-source-database  
  
- Esegue la connessione al database di origine e carica i metadati di livello elevato del database di origine, ma non tutti i metadati.  
  
- Se non è possibile stabilire la connessione all'origine, viene generato un errore e l'applicazione console interrompe l'esecuzione.  
  
**Script**
  
La definizione del server viene recuperata dall'attributo Name definito per ogni connessione nella sezione server del file di connessione del server o del file di script.  
  
**Esempio di sintassi:**  
  
```xml  
<connect-source-database  server="<server-unique-name>"/>  
```  
**Comando**  
  
Load-Access-database: usato per caricare i file di database di Access  
  
**Script**  
  
**Esempio di sintassi:**  
  
```xml  
<load-access-database  database-file="<Access-database>"/>  
```  
oppure  
  
```xml  
<load-access-database>  
  
  <access-database database-file="<Access-database1>"/>  
  
  <access-database database-file="<Access-database2>"/>  
  
</load-access-database>  
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
<force-load  
  
  object-name="<object-name>"  
  
  metabase="<source/target>"/>  
```  
oppure  
  
```xml  
<force-load>  
  
  <metabase-object object-name="<object-name>"/>  
  
</force-load>  
```  
**Comando**  
  
Riconnetti-origine-database  
  
- Si riconnette al database di origine, ma non carica i metadati, a differenza del comando Connect-source-database.  
  
- Se non è possibile stabilire una connessione con l'origine, viene generato un errore e l'applicazione console interrompe l'esecuzione.  
  
**Script**
  
**Esempio di sintassi:**  
  
```xml  
<reconnect-source-database  server="<server-unique-name>"/>  
```

**Comando**
  
Connect-target-database  
  
- Stabilisce la connessione al database di destinazione SQL Server o SQL Azure e carica i metadati di alto livello del database di destinazione, ma non i metadati interamente.  
  
- Se non è possibile stabilire la connessione alla destinazione, viene generato un errore e l'applicazione console interrompe l'esecuzione.  
  
**Script**
  
La definizione del server viene recuperata dall'attributo Name definito per ogni connessione nella sezione server del file di connessione del server o del file di script  
  
**Esempio di sintassi:**  
  
```xml  
<connect-target-database  server="<server-unique-name>"/>  
```  

**Comando**
  
riconnessione-destinazione-database  
  
- Si riconnette al database di destinazione, ma non carica i metadati, a differenza del comando Connect-target-database.  
  
- Se non è possibile stabilire una connessione alla destinazione, viene generato un errore e l'applicazione console interrompe l'esecuzione.  
  
**Script**
  
**Esempio di sintassi:**  
  
```xml  
<reconnect-target-database  server="<server-unique-name>"/>  
```  

## <a name="report-script-file-commands"></a>Comandi del file di script del report

I comandi del report generano report sulle prestazioni di varie attività della console SSMA

**Comando**
  
generate-assessment-report  
  
- Genera report di valutazione nel database di origine.  
  
- Se la connessione al database di origine non viene eseguita prima di eseguire questo comando, viene generato un errore e l'applicazione console viene chiusa.  
  
- Errore di connessione al server di database di origine durante l'esecuzione del comando. Inoltre, l'applicazione console viene terminata.  
  
**Script**

- `assessment-report-folder:` Specifica la cartella in cui è possibile archiviare il report di valutazione. (attributo facoltativo)  
  
- `object-name:` Specifica gli oggetti considerati per la generazione di report di valutazione. può includere nomi di oggetti singoli o un nome di oggetto gruppo.  
  
- `object-type:` Specifica il tipo dell'oggetto specificato nell'attributo nome-oggetto (se viene specificata la categoria dell'oggetto, il tipo di oggetto sarà "Category").  
  
- `assessment-report-overwrite:` Specifica se sovrascrivere la cartella dei report di valutazione, se esiste già.  
  
    **Valore predefinito:** false. (attributo facoltativo)  
  
- `write-summary-report-to:` Specifica il percorso in cui verrà generato il report.  
  
    Se viene indicato solo il percorso della cartella, il file in base al nome **AssessmentReport &lt; n &gt; . XML** creato. (attributo facoltativo)  
  
    Per la creazione di report sono presenti altre due sottocategorie:  
  
    - `report-errors` (= "true/false", con il valore predefinito "false" (attributi facoltativi))  
  
    - `verbose` (= "true/false", con il valore predefinito "false" (attributi facoltativi))  
  
**Esempio di sintassi:**  
  
```xml  
<generate-assessment-report  
  
  object-name="ssma.Procedures"  
  
  object-type="category"  
  
  write-summary-report-to="<file>"             (optional)  
  
  verbose="<true/false>"                       (optional)  
  
  report-errors="<true/false>"                 (optional)  
  
  conversion-report-folder="<folder>"          (optional)  
  
  conversion-report-overwrite="<true/false>"   (optional)  
  
/>  
```

oppure
  
```xml  
<generate-assessment-report  
  
  conversion-report-folder="<folder>"            (optional)  
  
  conversion-report-overwrite="<true/false>"     (optional)  
  
>  
    <metabase-object object-name="ssma.Procedures"  
  
        object-type="category"/>  
  
</generate-assessment-report>  
```  
  
## <a name="migration-script-file-commands"></a>Comandi del file di script di migrazione

I comandi di migrazione convertono lo schema del database di destinazione nello schema di origine ed esegue la migrazione dei dati al server di destinazione.  
  
L'impostazione di output della console predefinita per i comandi di migrazione è il report di output completo senza segnalazione errori dettagliata: solo riepilogo nel nodo radice dell'albero degli oggetti di origine.  
  
**Comando**

Convert-schema  
  
- Esegue la conversione dello schema dall'origine allo schema di destinazione.  
  
- Se la connessione al database di origine o di destinazione non viene eseguita prima di eseguire questo comando o se la connessione al server di database di origine o di destinazione ha esito negativo durante l'esecuzione del comando, viene generato un errore e l'applicazione console viene chiusa.  
  
**Script**

- `conversion-report-folder:` Specifica la cartella in cui è possibile archiviare il report di valutazione. (attributo facoltativo)  
  
- `object-name:` Specifica gli oggetti di origine considerati per la conversione dello schema. può includere nomi di oggetti singoli o un nome di oggetto gruppo.  
  
- `object-type:` Specifica il tipo dell'oggetto specificato nell'attributo nome-oggetto (se viene specificata la categoria dell'oggetto, il tipo di oggetto sarà "Category").  
  
- `conversion-report-overwrite:` Specifica se sovrascrivere la cartella dei report di valutazione, se esiste già.  
  
    **Valore predefinito:** false. (attributo facoltativo)  
  
- `write-summary-report-to:` Specifica il percorso in cui verrà generato il report.  
  
    Se viene indicato solo il percorso della cartella, il file in base al nome **SchemaConversionReport &lt; n &gt; . XML** creato. (attributo facoltativo)  
  
    Per la creazione di report sono presenti altre due sottocategorie:  
  
    - `report-errors` (= "true/false", con il valore predefinito "false" (attributi facoltativi))  
  
    - `verbose` (= "true/false", con il valore predefinito "false" (attributi facoltativi))  
  
**Esempio di sintassi:**  
  
```xml  
<convert-schema  
  
  object-name="ssma.Procedures"  
  
  object-type="category"  
  write-summary-report-to="<filepath/folder>"     (optional)  
  
  verbose="<true/false>"                          (optional)  
  
  report-errors="<true/false>"                    (optional)  
  
  conversion-report-folder="<folder>"             (optional)  
  
  conversion-report-overwrite="<true/false>"      (optional)  
  
/>  
```

oppure  
  
```xml  
<convert-schema  
  
  conversion-report-folder="<folder>"         (optional)  
  
  conversion-report-overwrite="<true/false>"  (optional)  
  
  <metabase-object object-name="ssma.Procedures"  
  
    object-type="category"/>  
  
</convert-schema>  
```

**Comando**
  
migrate-dati  
  
- Esegue la migrazione dei dati di origine alla destinazione.  
  
**Script**
  
- `object-name:` Specifica gli oggetti di origine considerati per la migrazione dei dati. può avere nomi di oggetti singoli o un nome di oggetto gruppo.  
  
- `object-type:` Specifica il tipo dell'oggetto specificato nell'attributo nome-oggetto (se viene specificata la categoria dell'oggetto, il tipo di oggetto sarà "Category").  
  
- `write-summary-report-to:` Specifica il percorso in cui verrà generato il report.  
  
    Se viene indicato solo il percorso della cartella, il file in base al nome **DataMigrationReport &lt; n &gt; . XML** creato. (attributo facoltativo)  
  
    Per la creazione di report sono presenti altre due sottocategorie:  
  
    - `report-errors` (= "true/false", con il valore predefinito "false" (attributi facoltativi))  
  
    - `verbose` (= "true/false", con il valore predefinito "false" (attributi facoltativi))  
  
**Esempio di sintassi:**  
  
```xml  
<migrate-data  
  
  write-summary-report-to="<filepath/folder>"  
  
  report-errors="true" verbose="true">  
  
    <metabase-object object-name="ssma.TT1"/>  
  
    <metabase-object object-name="ssma.TT2"/>  
  
    <metabase-object object-name="ssma.TT3"/>  
  
    <data-migration-connection  
  
      source-use-last-used="true"/source-server="<server-unique-name>"  
  
      target-use-last-used="true"/target-server="<server-unique-name>"/>  
  
</migrate-data>  
```

oppure  
  
```xml  
<migrate-data  
  
  object-name="ssma.Tables"  
  
  object-type="category"  
  
  write-summary-report-to="<filepath/folder>"  
  
  report-errors="true" verbose="true"/>  
```

**Comando**
  
link-Tables: questo comando collega la tabella di origine (accesso) alla tabella di destinazione.  
  
**Script**

**Esempio di sintassi:**  
  
```xml  
<link-tables>  
  
  <metabase-object object-name="AccessDatabase.table1" object-type="Tables"/>  
  
  <metabase-object object-name="AccessDatabase.table2" object-type="Tables"/>  
  
</link-tables>  
```

oppure  
  
```xml  
<link-tables>  
  
  <metabase-object object-name="AccessDatabase.Tables" object-type="category"/>  
  
</link-tables>  
```

**Comando**
  
unlink-Tables: questo comando scollega la tabella di origine (accesso) dalla tabella di destinazione.  
  
**Script**
  
**Esempi di sintassi:**  
  
```xml  
<unlink-tables>  
  
  <metabase-object object-name="AccessDatabase.table1" object-type="Tables"/>  
  
  <metabase-object object-name="AccessDatabase.table2" object-type="Tables"/>  
  
</unlink-tables>  
```

oppure  
  
```xml  
<unlink-tables>  
  
  <metabase-object object-name="AccessDatabase.Tables" object-type="category"/>  
  
</unlink-tables>  
```  
  
## <a name="migration-preparation-script-file-commands"></a>Comandi del file di script di preparazione della migrazione

Il comando di preparazione alla migrazione avvia il mapping dello schema tra i database di origine e di destinazione.  
  
**Comando**
  
Map-schema: mapping dello schema del database di origine allo schema di destinazione.  
  
**Script**
  
- `source-schema` Specifica lo schema di origine di cui si intende eseguire la migrazione.  
  
- `sql-server-schema` Specifica lo schema di destinazione in cui si desidera eseguire la migrazione.  
  
**Esempio di sintassi:**  
  
```xml  
<map-schema source-schema="source-schema"  
  
            sql-server-schema="target-schema"/>  
```  
  
## <a name="manageability-commands"></a>Comandi di gestibilità

I comandi di gestibilità consentono di sincronizzare gli oggetti di database di destinazione con il database di origine.  
  
L'impostazione di output della console predefinita per i comandi di migrazione è il report di output completo senza segnalazione errori dettagliata: solo riepilogo nel nodo radice dell'albero degli oggetti di origine.  
  
**Comando**

sincronizzazione-destinazione  
  
- Sincronizza gli oggetti di destinazione con il database di destinazione.  
  
- Se questo comando viene eseguito sul database di origine, si ottiene un errore.  
  
- Se la connessione al database di destinazione non viene eseguita prima di eseguire questo comando o se la connessione al server di database di destinazione non riesce durante l'esecuzione del comando, viene generato un errore e l'applicazione console viene chiusa.  
  
**Script**
  
- `object-name:` Specifica gli oggetti di destinazione considerati per la sincronizzazione con il database di destinazione. può contenere nomi di oggetti singoli o un nome di oggetto gruppo.  
  
- `object-type:` Specifica il tipo dell'oggetto specificato nell'attributo nome-oggetto (se viene specificata la categoria dell'oggetto, il tipo di oggetto sarà "Category").  
  
- `on-error:` Specifica se specificare gli errori di sincronizzazione come avvisi o errori. Opzioni disponibili per l'errore:  
  
    - report-totale-come-avviso  
  
    - report-each-As-Warning  
  
    - script di errore  
  
- `report-errors-to:` Specifica la posizione della segnalazione errori per l'operazione di sincronizzazione (attributo facoltativo) se viene specificato solo il percorso della cartella, quindi viene creato il file in base al nome **TargetSynchronizationReport.XML** .  
  
**Esempio di sintassi:**  
  
```xml  
<synchronize-target  
  
  object-name="ots_triggers.dbo"  
  
  on-error="<report-total-as-warning|  
  
             report-each-as-warning|  
  
             fail-script>"              (optional)  
  
  report-errors-to="<file-name>"        (optional)  
  
/>  
```

oppure  
  
```xml  
<synchronize-target  
  
  object-name="ssma.dbo.Procedures"  
  
  object-type="category"/>  
```

oppure  
  
```xml  
<synchronize-target>  
  
  <metabase-object object-name="ssma.dbo.TT1"/>  
  
  <metabase-object object-name="ssma.dbo.TT2"/>  
  
  <metabase-object object-name="ssma.dbo.TT3"/>  
  
</synchronize-target>  
```

**Comando**
  
aggiornamento da database  
  
- Aggiorna gli oggetti di origine dal database.  
  
- Se questo comando viene eseguito sul database di destinazione, viene generato un errore.  
  
**Script**
  
Richiede uno o più nodi della metabase come parametro della riga di comando.  
  
- `object-name:` Specifica gli oggetti di origine considerati per l'aggiornamento dal database di origine. può contenere nomi di oggetti singoli o un nome di oggetto gruppo.  
  
- `object-type:` Specifica il tipo dell'oggetto specificato nell'attributo nome-oggetto (se viene specificata la categoria dell'oggetto, il tipo di oggetto sarà "Category").  
  
- `on-error:` Specifica se specificare gli errori di aggiornamento come avvisi o errori. Opzioni disponibili per l'errore:  
  
    - report-totale-come-avviso  
  
    - report-each-As-Warning  
  
    - script di errore  
  
- `report-errors-to:` Specifica la posizione della segnalazione errori per l'operazione di aggiornamento (attributo facoltativo) se viene specificato solo il percorso della cartella, quindi viene creato il file in base al nome **SourceDBRefreshReport.XML** .  
  
**Esempio di sintassi:**  
  
```xml  
<refresh-from-database  
  
  object-name="ssma.TT1"  
  
  on-error="<report-total-as-warning|  
  
             report-each-as-warning|  
  
             fail-script>"              (optional)  
  
  report-errors-to="<file-name>"        (optional)  
  
/>  
```

oppure  
  
```xml  
<refresh-from-database  
  
  object-name="ssma.Procedures"  
  
  object-type="category"/>  
```

oppure  
  
```xml  
<refresh-from-database>  
  
  <metabase-object object-name="ssma.TT_1"/>  
  
</refresh-from-database>  
```  
  
## <a name="script-generation-script-file-commands"></a>Comandi del file script di generazione script

I comandi di generazione script consentono di salvare l'output della console in un file di script.  
  
**Comando**
  
Salva come script  
  
Usato per salvare gli script degli oggetti in un file indicato quando metabase = target, si tratta di un'alternativa al comando di sincronizzazione in cui si ottengono gli script ed eseguono gli stessi nel database di destinazione.  
  
**Script**
  
Richiede uno o più nodi della metabase come parametro della riga di comando.  
  
- `object-name:` Specifica gli oggetti i cui script devono essere salvati. (Può avere nomi di oggetti singoli o un nome di oggetto gruppo)  
  
- `object-type:` Specifica il tipo dell'oggetto specificato nell'attributo nome-oggetto (se viene specificata la categoria dell'oggetto, il tipo di oggetto sarà "Category").  
  
- `metabase:` Specifica se si tratta della metabase di origine o di destinazione.  
  
- `destination:` Specifica il percorso o la cartella in cui deve essere salvato lo script; Se al nome del file non viene assegnato un nome file nel formato (valore dell'attributo object_name). out  
  
- `overwrite:` Se true, sovrascrive se sono presenti gli stessi nomi di file. Può contenere i valori (true/false).  
  
**Esempio di sintassi:**  
  
```xml  
<save-as-script  
  
  metabase="<source/target>"  
  
  object-name="ssma.dbo.Procedures"  
  
  object-type="category"  
  
  destination="<file/folder>"  
  
  overwrite="<true/false>"             (optional)  
  
/>  
```

oppure  
  
```xml  
<save-as-script  
  
  metabase="<source/target>"  
  
  destination="<file/folder>"  
  
    <metabase-object object-name="ssma.dbo.Procedures"  
  
                     object-type="category"/>  
  
</save-as-script>  
```  
  
## <a name="next-steps"></a>Passaggi successivi

Per informazioni sulle opzioni della riga di comando, vedere [Opzioni della riga di comando nella console SSMA &#40;AccessToSQL&#41;](../../ssma/access/command-line-options-in-ssma-console-accesstosql.md) .  
  
Per informazioni sui file script della console di esempio, vedere [uso dello script della console di esempio FILESEXECUTING SSMA console &#40;AccessToSQL&#41;](../../ssma/access/working-sample-console-script-filesexecuting-ssma-console-accesstosql.md)  
  
Il passaggio successivo dipende dai requisiti del progetto:  
  
- Per specificare una password o le password di esportazione/importazione, vedere Gestione delle password [&#40;AccessToSQL&#41;](../../ssma/access/managing-passwords-accesstosql.md).  
  
- Per la generazione di report, vedere [generazione di report &#40;&#41;AccessToSQL ](../../ssma/access/generating-reports-accesstosql.md).  
  
- Per la risoluzione dei problemi nella console di, vedere [troubleshooting &#40;AccessToSQL&#41;](../../ssma/access/troubleshooting-accesstosql.md).  
