---
title: Versione autonoma di SQL Server Integration Services (SSIS) DevOps Tools | Microsoft Docs
description: Informazioni su come compilare il processo CI/CD di SSIS con la versione autonoma di SSIS DevOps Tools.
ms.date: 10/16/2020
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a445e38d796113826f45199cf19b4f59a6e9dc16
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045101"
---
# <a name="standalone-sql-server-integration-service-ssis-devops-tools-preview"></a>Versione autonoma di SQL Server Integration Services (SSIS) DevOps Tools (anteprima)

La versione autonoma di **SSIS DevOps Tools** offre un set di file eseguibili per eseguire attività CI/CD di SSIS. Senza dipendere dall'installazione di Visual Studio o dal runtime SSIS, questi file eseguibili possono essere facilmente integrati con qualsiasi piattaforma CI/CD. Gli eseguibili disponibili sono i seguenti:

- SSISBuild.exe: compila i progetti SSIS nel modello di distribuzione del progetto o nel modello di distribuzione del pacchetto.
- SSISDeploy.exe: distribuisce i file ISPAC nel catalogo SSIS o i file DTSX e le relative dipendenze in file system.

## <a name="installation"></a>Installazione

È necessario .NET Framework 4.6.2 o versione successiva.

Scaricare il programma di installazione più recente [dall'area download](https://aka.ms/AA9xp65), quindi eseguire l'installazione tramite la procedura guidata o la riga di comando:

- Eseguire l'installazione tramite la procedura guidata

Fare doppio clic sul file con estensione exe per eseguire l'installazione, quindi specificare una cartella in cui estrarre i file eseguibili e i file di dipendenza.

![Percorso di installazione](media/installation-location.png)

- Eseguire l'installazione tramite la riga di comando

```
SSISDevOpsTools.exe /Q /C /T:<full path>
```

![Riga di comando dell'installazione](media/installation-command-line.png)


## <a name="ssisbuildexe"></a>SSISBuild.exe

**Sintassi**

```
SSISBuild.exe -project|-p:<dtproj file path> [-configuration|-c:<configuration name>] [-projectPassword|-pp:<project password>] [-stripSensitive|-ss] [-output|-o:<output path>] [-log|-l:<log level>[;<log path>]] [-quiet|-q] [-help|-h|-?]
```

**Parametri**

|Parametro|Descrizione|
|---------|---------|
|-project \|-p:\<dtproj file path>|Percorso del file dtproj da compilare.|
|-configuration\|-c:\<configuration name>|Nome della configurazione di progetto da usare per la compilazione. Se non viene specificata, per impostazione predefinita viene assegnata la prima configurazione di progetto definita nel file dtproj.|
|-projectPassword\|-pp:\<project password>|Password del progetto SSIS e dei relativi pacchetti. Questo argomento è valido solo quando il livello di protezione del progetto e dei pacchetti SSIS è EncryptSensitiveWithPassword o EncryptAllWithPassword. Per il modello di distribuzione del pacchetto, tutti i pacchetti devono condividere la stessa password specificata da questo argomento.|
|-stripSensitive\|-ss|Converte il livello di protezione del progetto SSIS in DontSaveSensitve. Quando il livello di protezione è EncryptSensitiveWithPassword o EncryptAllWithPassword, è necessario che l'argomento -projectPassword sia impostato correttamente. Questa opzione è valida solo per il modello di distribuzione del progetto.|
|-output\|-o:\<output path>|Percorso di output dell'artefatto della compilazione. Il valore di questo argomento sovrascrive il percorso di output predefinito nella configurazione del progetto.|
|-log\|-l:\<log level>[;\<log path>]|Impostazioni relative al log. <li>Livello di log: solo i log con livello di registrazione uguale o superiore vengono scritti nel file di log. Esistono quattro livelli di registrazione (da basso ad alto): DIAG, INFO, WRN, ERR. Se non è specificato, il livello di registrazione predefinito è INFO. <li> Percorso log: percorso del file per il salvataggio permanente dei log. Il file di log non viene generato se non viene specificato il percorso.|
|-quiet\|-q|Nell'output standard non vengono visualizzi log.|
|-help\|-h\|-?|Vengono visualizzate informazioni dettagliate sull'utilizzo di questa utilità della riga di comando.|

**esempi**

- Compilare un file dtproj con la prima configurazione di progetto definita, non crittografata con la password:    
    ```
    SSISBuild.exe -p:"C:\projects\demo\demo.dtproj"
    ```

- Compilare un file dtproj con la configurazione "DevConfiguration" crittografata con la password e restituire gli artefatti della compilazione in una cartella specifica:
    ```
    SSISBuild.exe -p:C:\projects\demo\demo.dtproj -c:DevConfiguration -pp:encryptionpassword -o:D:\folder
    ```

- Compilare un file dtproj con la configurazione "DevConfiguration" crittografato con password, striping dei dati sensibili e livello di registrazione DIAG:
    ```
    SSISBuild.exe -p:C:\projects\demo\demo.dtproj -c:DevConfiguration -pp:encryptionpassword -ss -l:diag
    ```

## <a name="ssisdeployexe"></a>SSISDeploy.exe

**Sintassi**

```
SSISDeploy.exe -source|-s:<source path> -destination|-d:<type>;<path>[;server] [-authType|-at:<auth type name>] [-connectionStringSuffix|-css:<connection string suffix>] [-projectPassword|-pp:<project password>] [-username|-u:<username>] [-password|-p:<password>] [-log|-l:<log level>[;<log path>]] [-quiet|-q] [-help|-h|-?]
```

**Parametri**

|Parametro|Descrizione|
|---------|---------|
|-source\|-s:\<source path>|Percorso del file locale degli artefatti da distribuire. Sono consentiti ISPAC, DTSX, il percorso della cartella per DTSX e SSISDeploymentManfiest.|
|-destination\|-d:\<type>;\<path>[;server]|Tipo di destinazione, percorso della cartella di destinazione e nome del server del catalogo SSIS in cui viene distribuito il file di origine. Attualmente sono supportati i due tipi di destinazione seguenti: <li> *CATALOG*: è possibile distribuire uno o più file ISPAC nel catalogo SSIS specificato. Il percorso della destinazione CATALOG deve avere il formato seguente: <br> /SSISDB/\<folder name>[/\<project name>] <br> Il valore <project name> facoltativo è valido solo quando l'origine specifica un singolo percorso del file ISPAC. Per la destinazione CATALOG è necessario specificare il nome del server. <li> *FILE*: è possibile distribuire i pacchetti SSIS o i file specificati in uno o più file SSISDeploymentManifest nel percorso specificato del file system. Il percorso di destinazione FILE può essere un percorso di cartella locale o un percorso di cartella di rete nel formato seguente: <br>\\\\\<machine name>\\\<folder name>[\\\<sub folder name>\...]|
|-authType\|-at:\<auth type name>|Tipo di autenticazione per accedere a SQL Server. Obbligatorio per la destinazione CATALOG. Sono supportati i tipi seguenti: <li> WIN:  Autenticazione di Windows <li> SQL:  Autenticazione di SQL Server <li> ADPWD:  Active Directory - Password <li> ADINT:  Active Directory - Integrata|
|-connectionStringSuffix\|-css:\<connection string suffix> |Suffisso della stringa di connessione usata per la connessione al catalogo SSIS.|
|-projectPassword\|-pp:\<project password> |Password per decrittografare i file ISPAC o DTSX.|
|-username\|-u:\<username>  |Nome utente per accedere al file system o al catalogo SSIS specificato. Per accedere al file system è consentito il prefisso con nome di dominio.|
|-password\|-p:\<password>  |Password per accedere al file system o al catalogo SSIS specificato.|
|-log\|-l:\<log level>[;\<log path>] |Impostazioni relative al log per l'esecuzione di questa utilità. <li> Livello di log: solo i log con livello di registrazione uguale o superiore vengono scritti nel file di log. Esistono quattro livelli di registrazione (da basso ad alto): DIAG, INFO, WRN, ERR. Se non è specificato, il livello di registrazione predefinito è INFO. <li> Percorso log: percorso del file per il salvataggio permanente dei log. Il file di log non viene generato se non viene specificato il percorso.|
|-quiet\|-q |Nell'output standard non vengono visualizzi log.|
|-help\|-h\|-?  |Vengono visualizzate informazioni dettagliate sull'utilizzo di questa utilità della riga di comando.|

**esempi**

- Distribuire un singolo file ISPAC non crittografato con la password nel catalogo SSIS usando l'autenticazione di Windows.
    ```
    SSISDeploy.exe -s:D:\myfolder\demo.ispac -d:catalog;/SSISDB/destfolder;myssisserver -at:win
    ```

- Distribuire un singolo file ISPAC crittografato con la password nel catalogo SSIS usando l'autenticazione SQL e rinominare il nome del progetto.
    ```
    SSISDeploy.exe -s:D:\myfolder\test.ispac -d:catalog;/SSISDB/folder/testproj;myssisserver -at:sql -u:sqlusername -p:sqlpassword -pp:encryptionpassword
    ```

- Distribuire un singolo file SSISDeploymentManifest e i relativi file associati nella condivisione file di Azure.
    ```
    SSISDeploy.exe -s:D:\myfolder\mypackage.SSISDeploymentManifest -d:file;\\myssisshare.file.core.windows.net\destfolder -u:Azure\myssisshare -p:storagekey
    ```

- Distribuire una cartella di file DTSX in file system locali.
    ```
    SSISDeploy.exe -s:D:\myfolder -d:file;\\myssisshare\destfolder
    ```

## <a name="release-notes"></a>Note sulla versione

### <a name="version-012-preview"></a>Versione 0.1.2 - Anteprima

Data di rilascio: 14 gennaio 2021

- Correzione di un problema a causa del quale SSISBuild.exe non riesce a compilare il progetto con eccezione NullReference quando i metadati dei parametri del pacchetto nel file di progetto SSIS e nel pacchetto SSIS non corrispondono.
- Correzione di un problema a causa del quale il pacchetto non può essere eseguito con un messaggio di errore che inizia "Impossibile decrittografare il nodo XML protetto " anche se il pacchetto viene distribuito correttamente in SSISDB con SSISDeploy.exe, quando il progetto SSIS contenente il pacchetto è crittografato con EncryptSensitiveWithUserKey e il pacchetto contiene CM con dati sensibili.

### <a name="version-011-preview"></a>Version 0.1.1 (anteprima)

Data di rilascio: 11 novembre 2020

- Correzione di un problema a causa del quale SSISDeploy.exe non riesce a caricare un assembly durante la distribuzione di ispac nel catalogo SSIS.

### <a name="version-010-preview"></a>Version 0.1.0 (anteprima)

Data di rilascio: 16 ottobre 2020

Versione di anteprima iniziale autonoma di SSIS DevOps Tools.

## <a name="next-steps"></a>Passaggi successivi

- Ottenere la [versione autonoma di SSIS DevOps Tools](https://aka.ms/AA9xp65)
- Per eventuali domande, visitare [Domande e risposte](https://marketplace.visualstudio.com/items?itemName=SSIS.ssis-devops-tools&ssr=false#qna)
