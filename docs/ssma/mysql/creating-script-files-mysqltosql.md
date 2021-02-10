---
description: Creazione di file di script (MySQLToSQL)
title: Creazione di file di script (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Creating script files, configuring console settings
- Creating script files, Script commands
- Creating script files, script file validation
- Creating script files, server connection parameters
ms.assetid: b4608fe7-c777-4ba5-b853-4402f02109e3
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 310bcd0f3cd8db8a0c46f278d2b18bdcef2c42a9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016661"
---
# <a name="creating-script-files-mysqltosql"></a>Creazione di file di script (MySQLToSQL)
Il primo passaggio prima di avviare l'applicazione console SSMA consiste nel creare il file di script e, se necessario, creare il file di valore della variabile e il file di connessione del server.  
  
Il file di script può essere suddiviso in tre sezioni, vale a dire...:  
  
1.  **configurazione:** Consente all'utente di impostare i parametri di configurazione per l'applicazione console.  
  
2.  **Server:** Consente all'utente di impostare le definizioni del server di origine/destinazione. Può inoltre trovarsi in un file di connessione server separato.  
  
3.  **script-comandi:** Consente all'utente di eseguire i comandi del flusso di lavoro SSMA.  
  
Ogni sezione è descritta in dettaglio di seguito:  
  
## <a name="configuring-mysql-console-settings"></a>Configurazione delle impostazioni della console MySQL  
Le configurazioni di uno script vengono visualizzate nel file di script della console.  
  
Se uno degli elementi viene specificato nel nodo di configurazione, questi vengono impostati come impostazione globale, ovvero sono applicabili a tutti i comandi di script. Questi elementi di configurazione possono essere impostati anche all'interno di ogni comando della sezione script-command se l'utente vuole eseguire l'override dell'impostazione globale.  
  
Le opzioni configurabili dall'utente includono:  
  
1.  **Provider finestra di output:** Se l'attributo non è impostato su' true ', i messaggi specifici del comando non vengono visualizzati nella console. La descrizione degli attributi è riportata di seguito:  
  
    -   destinazione: specifica se l'output deve essere stampato in un file o in stdout. Per impostazione predefinita, questo valore è false.  
  
    -   nome file: il percorso del file (facoltativo).  
  
    -   Ometti-messaggi: disattiva i messaggi sulla console. Per impostazione predefinita, è "false".  
  
    **Esempio:**  
  
    ```xml  
    <output-providers>  
  
      <output-window  
  
        suppress-messages="<true/false>"   (optional)  
  
        destination="<file/stdout>"        (optional)  
  
        file-name="<file-name>"            (optional)  
  
       />  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <...All commands...>  
  
      <output-window  
  
         suppress-messages="<true/false>"   (optional)  
  
         destination="<file/stdout>"        (optional)  
  
         file-name="<file-name>"            (optional)  
  
       />  
  
    </...All commands...>  
    ```  
  
2.  **Provider connessione migrazione dati:** Consente di specificare il server di origine/destinazione da considerare per la migrazione dei dati.  Source-Use-last-used indica che l'ultimo server di origine usato viene usato per la migrazione dei dati. Analogamente a target-use-last-used indica che l'ultimo server di destinazione usato viene usato per la migrazione dei dati. L'utente può anche specificare il server (origine o destinazione) usando gli attributi Source-Server o target-server.  
  
    È possibile utilizzare solo uno o l'altro attributo specificato, ad esempio:  
  
    - Source-Use-last-used = "true" (impostazione predefinita) o Source-Server = "source_servername"  
  
    - target-use-last-used = "true" (impostazione predefinita) o target-server = "target_servername"  
  
    **Esempio:**  
  
    ```xml  
    <output-providers>  
  
      <data-migration-connection  source-use-last-used="true"  
  
                                  target-server="<target-server-unique-name>"/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <migrate-data>  
  
      <data-migration-connection   source-server="<source-server-unique-name>"  
  
                                   target-use-last-used="true"/>  
  
    </migrate-data>  
    ```  
  
3.  **Popup input utente:** In questo modo è possibile gestire gli errori quando gli oggetti vengono caricati dal database. L'utente fornisce le modalità di input e, in caso di errore, la console continua come specificato dall'utente.  
  
    Le modalità includono:  
  
    -   **Ask-utente-** Richiede all'utente di continuare (' Sì') o di errore (' No ').  
  
    -   **errore-** Nella console viene visualizzato un errore e l'esecuzione viene interrotta.  
  
    -   **continua-** La console continua con l'esecuzione.  
  
    La modalità predefinita è **Error**.  
  
    **Esempio:**  
  
    ```xml  
    <output-providers>  
  
      <user-input-popup mode="<ask-user/continue/error>"/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <!-- Connect to target database -->  
  
    <connect-target-database server="<target-server-unique-name>">  
  
      <user-input-popup mode="<ask-user/continue/error>"/>  
  
    </connect-target-database>  
    ```  
  
4.  **Riconnetti provider:** Ciò consente all'utente di impostare le impostazioni di riconnessione in caso di errori di connessione. Questo può essere impostato per i server di origine e di destinazione.  
  
    Le modalità di riconnessione sono:  
  
    -   Riconnetti a ultimo server usato: se la connessione non è attiva, tenta di riconnettersi all'ultimo server usato al massimo 5 volte.  
  
    -   generate-an-error: se la connessione non è attiva, viene generato un errore.  
  
    La modalità predefinita è **generate-an-error**.  
  
    **Esempio:**  
  
    ```xml  
    <output-providers>  
  
    <reconnect-manager  on-source-reconnect="<reconnect-to-last-used-server/generate-an-error>"  
  
                        on-target-reconnect="<reconnect-to-last-used-server/generate-an-error>"/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <!--synchronization-->  
  
    <synchronize-target>  
  
      <reconnect-manager on-target-reconnect="reconnect-to-last-used-server"/>  
  
    </synchronize-target>  
    ```  
    *or*  
  
    ```xml  
    <!--data migration-->  
  
    <migrate-data server="target-server-unique-name">  
  
      <reconnect-manager  
  
        on-source-reconnect="reconnect-to-last-used-server"  
  
        on-target-reconnect="generate-an-error"/>  
  
    </migrate-data>  
    ```  
  
5.  **Provider sovrascrittura convertitore:** Ciò consente all'utente di gestire gli oggetti già presenti nella metabase di destinazione. Le azioni possibili includono:  
  
    -   errore: nella console viene visualizzato un errore e l'esecuzione viene interrotta.  
  
    -   Sovrascrivi: sovrascrive i valori degli oggetti esistenti. Questa azione viene eseguita per impostazione predefinita.  
  
    -   Skip: la console ignora gli oggetti già esistenti nel database  
  
    -   Ask-user: richiede l'input dell'utente (' Sì'/' No ')  
  
    **Esempio:**  
  
    ```xml  
    <output-providers>  
  
      <object-overwrite action="<error/skip/overwrite/ask-user>"/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <convert-schema object-name="<object-name>">  
  
      <object-overwrite action="<error/skip/overwrite/ask-user>"/>  
  
    </convert-schema>  
    ```  
  
6.  **Provider prerequisiti non riuscito:** Ciò consente all'utente di gestire tutti i prerequisiti necessari per l'elaborazione di un comando. Per impostazione predefinita, la modalità Strict è "false". Se è impostato su "true", viene generata un'eccezione in caso di errore che soddisfi i prerequisiti.  
  
    **Esempio:**  
  
    ```xml  
    <output-providers>  
  
      <prerequisites strict-mode="<true/false>"/>  
  
    </output-providers>  
    ```  
  
7.  **Interrompi operazione:** Durante l'operazione intermedia, se l'utente vuole arrestare l'operazione, è possibile usare il tasto di scelta rapida **"CTRL + C"** . SSMA per la console MySQL attende il completamento dell'operazione e termina l'esecuzione della console.  
  
    Se l'utente vuole arrestare immediatamente l'esecuzione, è possibile premere di nuovo il tasto di scelta rapida **"CTRL + C"** per terminare bruscamente l'applicazione console SSMA  
  
8.  **Provider di stato:** Informa lo stato di avanzamento di ogni comando della console. Questa opzione è disabilitata per impostazione predefinita. Gli attributi di report di stato comprendono:  
  
    -   spento  
  
    -   ogni-1%  
  
    -   ogni-2%  
  
    -   ogni-5%  
  
    -   ogni-10%  
  
    -   ogni 20%  
  
    **Esempio:**  
  
    ```xml  
    <output-providers>  
  
      <progress-reporting enable="<true/false>"          (optional)  
  
                         report-messages="<true/false>"  (optional)  
  
                         report-progress="<every-1%/every-2%/every-5%/every-10%/every-20%/off>" (optional)/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <...All commands...>  
  
      <progress-reporting  
  
        enable="<true/false>"              (optional)  
  
        report-messages="<true/false>"     (optional)  
  
        report-progress="<every-1%/every-2%/every-5%/every-10%/every-20%/off>"     (optional)/>  
  
    </...All commands...>  
    ```  
  
9. Livello di **dettaglio del logger:** Imposta il livello di dettaglio del log. Corrisponde all'opzione tutte le categorie nell'interfaccia utente. Per impostazione predefinita, il livello di dettaglio del log è "Error".  
  
    Le opzioni a livello di logger includono:  
  
    -   errore irreversibile: vengono registrati solo i messaggi di errore irreversibile.  
  
    -   errore: vengono registrati solo i messaggi di errore e di errore irreversibile.  
  
    -   avviso: vengono registrati tutti i livelli eccetto i messaggi di debug e di informazioni.  
  
    -   Info: vengono registrati tutti i livelli eccetto i messaggi di debug.  
  
    -   debug: tutti i livelli dei messaggi registrati.  
  
    > [!NOTE]  
    > I messaggi obbligatori vengono registrati a qualsiasi livello.  
  
    **Esempio:**  
  
    ```xml  
    <output-providers>  
  
      <log-verbosity level="<fatal-error/error/warning/info/debug>"/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <...All commands...>  
  
      <log-verbosity level="<fatal-error/error/warning/info/debug>"/>  
  
    </...All commands...>  
    ```  
  
10. **Sostituisci password crittografata:** Se è impostata su' true ', la password non crittografata specificata nella sezione relativa alla definizione del server del file di connessione del server o nel file di script sostituisce la password crittografata archiviata in un archivio protetto, se esistente. Se non viene specificata alcuna password in testo non crittografato, all'utente viene richiesto di immettere la password.  
  
    Si verificano due casi:  
  
    1.  Se l'opzione di override è **false**, l'ordine di ricerca sarà protetto da file di script di archiviazione- &gt; server di connessione file- &gt; server &gt; .  
  
    2.  Se l'opzione di override è impostata su **true**, l'ordine di ricerca sarà l'utente del prompt dei file di connessione file- &gt; server &gt; .  
  
    **Esempio:**  
  
    ```xml  
    <output-providers>  
  
      <encrypted-password override="<true/false>"/>  
  
    </output-providers>  
    ```  
  
L'opzione non configurabile è:  
  
-   **Numero massimo di tentativi di riconnessione:** Quando si verifica un timeout o si interrompe una connessione stabilita a causa di un errore di rete, è necessario riconnettere il server. I tentativi di riconnessione sono consentiti fino a un massimo di **5** tentativi dopo i quali la console esegue automaticamente la riconnessione. La funzionalità di riconnessione automatica riduce il lavoro richiesto per la riesecuzione dello script.  
  
## <a name="server-connection-parameters"></a>Parametri di connessione del server  
È possibile definire i parametri di connessione del server nel file di script o nel file di connessione del server. Per altri dettagli, vedere la sezione [creazione dei file di connessione Server &#40;MySQLToSQL&#41;](../../ssma/mysql/creating-the-server-connection-files-mysqltosql.md) .  
  
## <a name="script-commands"></a>Comandi script  
Il file di script contiene una sequenza di comandi del flusso di lavoro di migrazione nel formato XML. L'applicazione console SSMA elabora la migrazione in base all'ordine dei comandi visualizzati nel file di script.  
  
Ad esempio, una tipica migrazione dei dati di una tabella specifica in un database MySQL segue la gerarchia di: database- &gt; Table.  
  
Quando tutti i comandi nel file di script vengono eseguiti correttamente, l'applicazione console SSMA viene chiusa e restituisce il controllo all'utente. Il contenuto di un file di script è più o meno statico con le informazioni sulle variabili contenute in un file di valori di [variabile](creating-variable-value-files-mysqltosql.md) o in una sezione separata all'interno del file di script per i valori delle variabili.  
  
**Esempio:**  
  
```xml  
<!--Sample of script file commands -->  
  
<ssma-script-file>  
  
  <script-commands>  
  
    <create-new-project project-folder="<project-folder>"  
  
                        project-name="<project-name>"  
  
                        overwrite-if-exists="<true/false>"/>  
  
    <connect-source-database server="<source-server-unique-name>"/>  
  
    <save-project/>  
  
    <close-project/>  
  
  </script-commands>  
  
</ssma-script-file>  
```  
I modelli costituiti da 3 file di script (per l'esecuzione di diversi scenari), da un file di valori di variabile e da un file di connessione del server sono disponibili nella cartella script della console di esempio della directory del prodotto:  
  
-   AssessmentReportGenerationSample.xml  
  
-   ConversionAndDataMigrationSample.xml  
  
-   SqlStatementConversionSample.xml  
  
-   VariableValueFileSample.xml  
  
-   ServersConnectionFileSample.xml  
  
È possibile eseguire i modelli (file) dopo aver modificato i parametri visualizzati per rilevarli.  
  
L'elenco completo dei comandi di script è disponibile nell' [esecuzione della console SSMA &#40;MySQLToSQL&#41;](../../ssma/mysql/executing-the-ssma-console-mysqltosql.md)  
  
## <a name="script-file-validation"></a>Convalida file script  
L'utente può convalidare facilmente il proprio file di script in base al file di definizione dello schema **' 2ssconsolescriptschema. xsd '** disponibile nella cartella ' schemi '.  
  
## <a name="next-step"></a>passaggio successivo  
Il passaggio successivo per la gestione della console consiste nella [creazione di file di valori di variabile &#40;MySQLToSQL&#41;](../../ssma/mysql/creating-variable-value-files-mysqltosql.md).  
  
## <a name="see-also"></a>Vedere anche  
[Creazione di file di valori di variabile &#40;MySQLToSQL&#41;](../../ssma/mysql/creating-variable-value-files-mysqltosql.md)  
  
