---
description: Opzioni della riga di comando nella console SSMA (AccessToSQL)
title: Opzioni della riga di comando nella console SSMA (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 08/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: c1f3b3f0-0f3e-4e07-b745-2fbdde85c67e
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 5e091085616078a93a65922bfaa2176ae4a6b16b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076342"
---
# <a name="command-line-options-in-the-ssma-console-accesstosql"></a>Opzioni della riga di comando nella console SSMA (AccessToSQL)
Microsoft offre un solido set di opzioni della riga di comando per l'esecuzione e il controllo delle attività SSMA. Le sezioni seguenti forniscono ulteriori dettagli.  
  
## <a name="command-line-options-in-the-ssma-console"></a>Opzioni della riga di comando nella console di SSMA  
Di seguito sono descritte le opzioni del comando console.  
  
Ai fini di questa sezione, il termine "opzione" viene definito anche "switch".  
  
Le opzioni non fanno distinzione tra maiuscole e minuscole e possono iniziare con il **-** carattere '' o ' **/** '.  
  
Se si specificano le opzioni, è obbligatorio specificare i parametri di opzione corrispondenti.  
  
I parametri di opzione devono essere separati dal carattere dell'opzione per spazi vuoti.  
  
**Esempi di sintassi:**  
  
`C:\> SSMAforAccessConsole.EXE -s scriptfile`  
  
`C:\> SSMAforAccessConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\AssessmentReportGenerationSample.xml" -v "C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\VariableValueFileSample.xml" -c "C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ServersConnectionFileSample.xml"`  
  
I nomi di file o cartelle contenenti spazi devono essere specificati tra virgolette doppie.  
  
L'output delle voci della riga di comando e dei messaggi di errore è archiviato in STDOUT o in un file specificato.  
  
### <a name="script-file-option--sscript"></a>Opzione file script:-s/script  
Un Commuter obbligatorio, il nome/percorso del file di script specifica lo script delle sequenze di comandi che devono essere eseguite da SSMA.  
  
**Esempi di sintassi:**  
  
`C:\>SSMAforAccessConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml"`  
  
### <a name="variable-value-file-option--vvariable"></a>Opzione file valore variabile:-v/variabile  
Il file di valori di variabile include le variabili usate nel file di script. L'opzione è facoltativa. Se le variabili non vengono dichiarate nel file di variabile e usate nel file di script, l'applicazione genera un errore e termina l'esecuzione della console.  
  
**Esempi di sintassi:**  
  
-   Variabili definite in più file di valori di variabile, ad esempio uno con un valore predefinito e un altro con un valore specifico dell'istanza quando applicabile. L'ultimo file di variabile specificato negli argomenti della riga di comando assume la preferenza, nel caso in cui esista una duplicazione di variabili:  
  
    `C:\>SSMAforAccessConsole.EXE -s`  
  
    `"C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml" -v c:\migration`  
  
    `projects\global_variablevaluefile.xml -v "c:\migrationprojects\instance_variablevaluefile.xml"`  
  
### <a name="server-connection-file-option--cserverconnection"></a>Opzione del file di connessione del server:-c/ServerConnection  
Questo file contiene le informazioni di connessione al server per ogni server. Ogni definizione di server è identificata da un ID server univoco. Nel file di script per i comandi correlati alla connessione viene fatto riferimento agli ID server.  
  
La definizione del server può essere una parte del file di connessione al server e/o del file di script. L'ID del server nel file di script ha la precedenza sul file di connessione del server, nel caso in cui si verifichi una duplicazione dell'ID server.  
  
**Esempi di sintassi:**  
  
-   Gli ID server vengono utilizzati nel file di script. Sono definite in un file di connessione server separato. Questo file usa le variabili definite nel file di valori di variabile:  
  
    `C:\>SSMAforAccessConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -v`  
  
    `c:\SsmaProjects\myvaluefile1.xml -c`  
  
    `c:\SsmaProjects\myserverconnectionsfile1.xml`  
  
-   La definizione del server è incorporata nel file script:  
  
    `C:\>SSMAforAccessConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml"`  
  
### <a name="xml-output-option--xxmloutput-xmloutputfile"></a>Opzione di output XML:-x/XMLOutput [xmloutputfile]  
Questo comando viene usato per l'output dei messaggi di output del comando in formato XML nella console o in un file XML.  
  
Sono disponibili due opzioni per XMLOutput, vale a dire:  
  
-   Se il FilePath viene fornito dopo l'opzione XMLOutput, l'output viene reindirizzato al file.  
  
    **Esempio di sintassi:**  
  
    `C:\>SSMAforAccessConsole.EXE -s`  
  
    `"C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -x d:\xmloutput\project1output.xml`  
  
-   Se non viene fornito alcun FilePath dopo l'opzione XMLOutput, il XMLout viene visualizzato nella console.  
  
    **Esempio di sintassi:**  
  
    `C:\>SSMAforAccessConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -xmloutput`  
  
### <a name="log-file-option--llog"></a>Opzione del file di log:-l/log  
Tutte le operazioni SSMA nell'applicazione console vengono registrate in un file di log e l'opzione è facoltativa. Se un file di log e il relativo percorso vengono specificati nella riga di comando, il log viene generato nel percorso specificato. In caso contrario, viene generato nel percorso predefinito.  
  
**Esempio di sintassi:**  
  
`C:\>SSMAforAccessConsole.EXE`  
  
`"C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -l c:\SsmaProjects\migration1.log`  
  
### <a name="project-environment-folder-option--eprojectenvironment"></a>Opzione cartella ambiente progetto:-e/projectenvironment  
Questa opzione facoltativa denota la cartella delle impostazioni dell'ambiente del progetto per il progetto SSMA corrente.  
  
**Esempio di sintassi:**  
  
`C:\>SSMAforAccessConsole.EXE -s`  
  
`"C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -e c:\SsmaProjects\CommonEnvironment`  
  
### <a name="secure-password-option--psecurepassword"></a>Opzione password protetta:-p/SecurePassword  
Questa opzione indica la password crittografata per le connessioni server. Differisce da tutte le altre opzioni in quanto non esegue alcuno script o Guida in alcuna attività relativa alla migrazione, ma consente di gestire la crittografia delle password per le connessioni server usate nel progetto di migrazione.  
  
Non è possibile immettere altre opzioni o password come parametri della riga di comando. In caso contrario, viene restituito un errore. Per ulteriori informazioni, vedere la sezione [gestione delle password](managing-passwords-accesstosql.md) .  
  
Per sono supportate le sottoopzioni seguenti `-p/securepassword` :  
  
-   Per aggiungere una password o aggiornare una password esistente, ad archiviazione protetta per un ID server specificato o per tutti gli ID server definiti nel file di connessione del server:  
  
    `-p|-securepassword -a|add    {"<server_id>[, .n]"|all}``-c|-serverconnection <server-connection-file> [-v|variable <variable-value-file>]``[-o|overwrite]`  
  
    `-p|-securepassword -a|add    {"<server_id>[, .n]"|all}``-s|-script <server-connection-file> [-v|variable <variable-value-file>] [-o|overwrite]`  
  
-   Per rimuovere la password crittografata dalla risorsa di archiviazione protetta dell'ID server specificato o di tutti gli ID server:  
  
    `-p/securepassword -r/remove {<server_id> [, ...n] | all}`  
  
-   Per visualizzare un elenco di ID server per i quali la password è crittografata:  
  
    `-p/securepassword -l/list`  
  
-   Per esportare le password archiviate in un archivio protetto in un file crittografato. Questo file è crittografato con il pass-phrase specificato dall'utente.  
  
    `-p/securepassword -e/export {<server-id> [, ...n] | all} <encrypted-password -file>`  
  
-   Il file crittografato che è stato esportato in precedenza viene importato nell'archiviazione protetta locale usando la passphrase specificata dall'utente. Una volta decrittografato, il file viene archiviato in un nuovo file, che a sua volta viene crittografato nel computer locale.  
  
    `-p/securepassword -i/import {<server-id> [, ...n] | all} <encrypted-password -file>`  
  
    È possibile specificare più ID server utilizzando separatori di virgole.  
  
### <a name="help-option--help"></a>Opzione della guida:-?/Help  
Visualizza il riepilogo della sintassi delle opzioni della console SSMA:  
  
`C:\>SSMAforAccessConsole.EXE -?`  
  
Per una visualizzazione tabulare delle opzioni della riga di comando della console SSMA, fare riferimento all' [appendice-1 &#40;AccessToSQL&#41;](../../ssma/access/appendix-1-accesstosql.md).  
  
### <a name="securepassword-help-option--securepassword--help"></a>Opzione della Guida di SecurePassword:-SecurePassword-?/Help  
Visualizza il riepilogo della sintassi delle opzioni della console SSMA:  
  
`C:\>SSMAforAccessConsole.EXE -securepassword -?`  
  
Per una visualizzazione tabulare delle opzioni della riga di comando della console SSMA, fare riferimento all' [appendice-1 &#40;AccessToSQL&#41;](../../ssma/access/appendix-1-accesstosql.md)  
  
### <a name="next-steps"></a>Passaggi successivi  
Il passaggio successivo dipende dai requisiti del progetto:  
  
1.  Per specificare una password o le password di esportazione/importazione, vedere Gestione delle password [&#40;AccessToSQL&#41;](../../ssma/access/managing-passwords-accesstosql.md).  
  
2.  Per la generazione di report, vedere [generazione di report &#40;&#41;AccessToSQL ](../../ssma/access/generating-reports-accesstosql.md).  
  
3.  Per la risoluzione dei problemi nella console di, vedere [troubleshooting &#40;AccessToSQL&#41;](../../ssma/access/troubleshooting-accesstosql.md).  
  
