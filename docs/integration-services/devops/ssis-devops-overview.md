---
title: Panoramica di SQL Server Integration Services DevOps | Microsoft Docs
description: Informazioni su come compilare il processo CI/CD di SSIS con SSIS DevOps Tools.
ms.date: 4/21/2021
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 54b94201d6e77f2d51e711129e0aabc7a59bd6c0
ms.sourcegitcommit: 241b503472b01ed0119f13c578b0c32c39f5e07c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/20/2021
ms.locfileid: "107754978"
---
# <a name="sql-server-integration-services-ssis-devops-tools-azure-devops-extension"></a>Estensione SQL Server Integration Services (SSIS) DevOps Tools in Azure DevOps

L'estensione [SSIS DevOps Tools](https://marketplace.visualstudio.com/items?itemName=SSIS.ssis-devops-tools) è disponibile nel marketplace di **Azure DevOps**.

Se non si dispone di un'organizzazione **Azure DevOps**, per prima cosa iscriversi ad [Azure Pipelines](/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops&preserve-view=true) e quindi aggiungere l'estensione **SSIS DevOps Tools** seguendo [questa procedura](/azure/devops/marketplace/overview?tabs=browser&view=azure-devops&preserve-view=true#add-an-extension).

**SSIS DevOps Tools include** l'attività **di compilazione SSIS,** l'attività di rilascio distribuzione **SSIS** e l'attività **di configurazione del catalogo SSIS**.

- L'attività di **[compilazione SSIS](#ssis-build-task)** supporta la compilazione di file con estensione dtproj nel modello di distribuzione del progetto o nel modello di distribuzione del pacchetto.

- L'attività di **[distribuzione SSIS](#ssis-deploy-task)** supporta la distribuzione di uno o più file con estensione ispac nel catalogo SSIS locale e in Azure-SSIS IR o di file SSISDeploymentManifest e relativi file associati in una condivisione file locale o di Azure.

- L'attività di **[configurazione del catalogo SSIS](#ssis-catalog-configuration-task)** supporta la configurazione della cartella, del progetto o dell'ambiente del catalogo SSIS con un file di configurazione in formato JSON. Questa attività supporta gli scenari seguenti:
    - Cartella
        - Creare una cartella.
        - Aggiornare la descrizione della cartella.
    - Project
        - Configurare il valore dei parametri. Sono supportati sia il valore letterale che il valore di riferimento.
        - Aggiungere i riferimenti all'ambiente.
    - Environment
        - Creare l'ambiente.
        - Aggiornare la descrizione dell'ambiente.
        - Creare o aggiornare la variabile di ambiente.

## <a name="ssis-build-task"></a>Attività SSIS Build (Compilazione SSIS)

![attività di compilazione](media/ssis-build-task.png)

### <a name="properties"></a>Proprietà

#### <a name="project-path"></a>Percorso progetto

Percorso della cartella o del file di progetto da compilare. Se viene specificato un percorso di cartella, l'attività di compilazione SSIS cercherà in modo ricorsivo tutti i file dtproj in questa cartella e li compilerà tutti.

Il percorso del progetto non può essere *vuoto*, deve essere impostato come **.** per eseguire la compilazione dalla cartella radice del repository.

#### <a name="project-configuration"></a>Configurazione del progetto

Nome della configurazione di progetto da usare per la compilazione. Se non viene specificata, per impostazione predefinita viene assegnata la prima configurazione di progetto definita in ogni file dtproj.

#### <a name="output-path"></a>Percorso di output

Percorso di una cartella separata in cui salvare i risultati della compilazione, che può essere pubblicata come artefatto di compilazione tramite l'attività [Pubblica artefatti della compilazione](/azure/devops/pipelines/tasks/utility/publish-build-artifacts?view=azure-devops&preserve-view=true).

### <a name="limitations-and-known-issues"></a>Limitazioni e problemi noti

- L'attività di compilazione SSIS si basa su Visual Studio e Progettazione SSIS, obbligatoria negli agenti di compilazione. Per eseguire l'attività di compilazione SSIS nella pipeline, quindi, è necessario scegliere **vs2017-win2016** per gli agenti ospitati da Microsoft oppure installare Visual Studio e Progettazione SSIS (VS2017 + SSDT2017 o VS2019 + estensione Progetti SSIS) negli agenti self-hosted.

- Per compilare progetti SSIS usando i componenti predefiniti (incluso SSIS Azure Feature Pack e altri componenti di terze parti), i componenti predefiniti devono essere installati nel computer in cui è in esecuzione l'agente pipeline.  Per l'agente ospitato da Microsoft, l'utente può aggiungere un'[attività di script di PowerShell](/azure/devops/pipelines/tasks/utility/powershell?view=azure-devops&preserve-view=true) o un'[attività script da riga di comando](/azure/devops/pipelines/tasks/utility/command-line?view=azure-devops&preserve-view=true) per scaricare e installare i componenti prima dell'esecuzione dell'attività di compilazione SSIS. Di seguito è riportato lo script di esempio di PowerShell per installare il Feature Pack di Azure: 

```powershell
wget -Uri https://download.microsoft.com/download/E/E/0/EE0CB6A0-4105-466D-A7CA-5E39FA9AB128/SsisAzureFeaturePack_2017_x86.msi -OutFile AFP.msi

start -Wait -FilePath msiexec -Args "/i AFP.msi /quiet /l* log.txt"

cat log.txt
```

- Nell'attività di compilazione SSIS non sono supportati i livelli di protezione **EncryptSensitiveWithPassword** e **EncryptAllWithPassword**. Verificare che nessuno dei progetti SSIS presenti nella codebase usi questi due livelli di protezione. In caso contrario, l'attività di compilazione SSIS non risponderà e genererà un errore di timeout durante l'esecuzione.

## <a name="ssis-build-task-version-1-preview"></a>Attività di compilazione SSIS versione 1.* (anteprima)

Miglioramenti nella versione 1.*:

- Rimuovere la dipendenza da progettazione Visual Studio progettazione SSIS. L'attività di compilazione può essere eseguita nell'agente ospitato da Microsoft o nell'agente self-hosted con il sistema operativo Windows e .NET Framework 4.6.2 o versione successiva.

- Non è necessario installare componenti di tipo out-of-box.

- Supporto del livello di protezione EncryptionWithPassword e EncryptionAllWithPassword.

### <a name="version-1-only-properties"></a>Proprietà solo versione 1.*

#### <a name="project-password"></a>Project Password

Password del progetto SSIS e dei relativi pacchetti. Questo argomento è valido solo quando il livello di protezione del progetto e dei pacchetti SSIS è EncryptSensitiveWithPassword o EncryptAllWithPassword. Per il modello di distribuzione del pacchetto, tutti i pacchetti devono condividere la stessa password specificata da questo argomento.

#### <a name="strip-sensitive-data"></a>Rimuovere dati sensibili

Convertire il livello di protezione del progetto SSIS in DontSaveSensitve se questo valore è true. Quando il livello di protezione è EncryptSensitiveWithPassword o EncryptAllWithPassword, l'argomento Project Password deve essere impostato correttamente. Questa opzione è valida solo per il modello di distribuzione del progetto.

## <a name="ssis-deploy-task"></a>Attività SSIS Deploy (Distribuzione SSIS)

![attività di distribuzione](media/ssis-deploy-task.png)

### <a name="properties"></a>Proprietà

#### <a name="source-path"></a>Percorso di origine

Percorso dei file di origine ISPAC o SSISDeploymentManifest da distribuire. Questo percorso può essere un percorso di cartella o un percorso di file.

#### <a name="destination-type"></a>Tipo destinazione

Tipo della destinazione. L'attività di distribuzione SSIS supporta attualmente due tipi di destinazione:

- *File System*: Distribuire i file SSISDeploymentManifest e i file associati in un file system specificato. Sono supportate sia la condivisione file locale che quella di Azure.
- *SSISDB*: Distribuire i file ISPAC in un catalogo SSIS specificato, che può essere ospitato in un database SQL Server locale o in Azure-SSIS Integration Runtime.

#### <a name="destination-server"></a>Server di destinazione

Nome del server SQL di destinazione. Può corrispondere al nome di un database di SQL Server locale, di un database SQL di Azure o di Istanza gestita di SQL di Azure. Questa proprietà è visibile solo se il tipo di destinazione è SSISDB.

#### <a name="destination-path"></a>Percorso di destinazione

Percorso della cartella di destinazione in cui verrà distribuito il file di origine. Ad esempio:

- /SSISDB/\<folderName\>
- \\\\\<machineName\>\\\<shareFolderName\>\\\<optionalSubfolderName\>

L'attività di distribuzione SSIS creerà la cartella e la sottocartella, se non esistono già.

#### <a name="authentication-type"></a>Tipo di autenticazione

Tipo di autenticazione per accedere al server di destinazione specificato. Questa proprietà è visibile solo se il tipo di destinazione è SSISDB. In generale, sono supportati i tipi di autenticazione seguenti:

- Autenticazione di Windows
- Autenticazione di SQL Server
- Active Directory - Password
- Active Directory - Integrata

Il supporto di un determinato tipo di autenticazione dipende tuttavia dal tipo di server di destinazione e dal tipo di agente. Per una matrice di supporto dettagliata, vedere la tabella seguente.

|Tipo server di destinazione|Pool ospitato da Microsoft|Agente self-hosted|
|---------|---------|---------|
|SQL Server in locale o macchina virtuale |N/D|Autenticazione di Windows|
|SQL di Azure|Autenticazione di SQL Server <br> Active Directory - Password|Autenticazione di SQL Server <br> Active Directory - Password <br> Active Directory - Integrata|

#### <a name="domain-name"></a>Nome di dominio

Nome di dominio per accedere al file system specificato. Questa proprietà è visibile solo se il tipo di destinazione è File System.
È possibile lasciarla vuota se all'account utente che esegue l'agente self-hosted è stato concesso l'accesso in lettura/scrittura al percorso di destinazione specificato.

#### <a name="username"></a>Username

Nome utente per accedere al file system o al database SSISDB specificato. Questa proprietà è visibile se il tipo di destinazione è File System o il tipo di autenticazione è Autenticazione SQL Server o Active Directory - Password.
È possibile lasciarla vuota se il tipo di destinazione è File System e se all'account utente che esegue l'agente self-hosted è stato concesso l'accesso in lettura/scrittura al percorso di destinazione specificato.

#### <a name="password"></a>Password

Password per accedere al file system o al database SSISDB specificato. Questa proprietà è visibile se il tipo di destinazione è File System o il tipo di autenticazione è Autenticazione SQL Server o Active Directory - Password.
È possibile lasciarla vuota se il tipo di destinazione è File System e se all'account utente che esegue l'agente self-hosted è stato concesso l'accesso in lettura/scrittura al percorso di destinazione specificato.

#### <a name="overwrite-existing-projects-or-ssisdeploymentmanifest-files-of-the-same-names"></a>Overwrite existing projects or SSISDeploymentManifest files of the same names (Sovrascrivi progetti esistenti o file SSISDeploymentManifest con lo stesso nome)

Consente di specificare se sovrascrivere progetti esistenti o file SSISDeploymentManifest con gli stessi nomi. Se 'No', l'attività di distribuzione SSIS ignorerà la distribuzione di questi file o progetti.

#### <a name="continue-deployment-when-error-occurs"></a>Continue deployment when error occurs (Continua la distribuzione in caso di errore)

Consente di specificare se continuare la distribuzione dei progetti o dei file rimanenti in caso di errore. Se 'No', l'attività di distribuzione SSIS si interromperà immediatamente in caso di errore.

### <a name="limitations-and-known-issues"></a>Limitazioni e problemi noti

L'attività di distribuzione SSIS non supporta attualmente gli scenari seguenti:

- Configurare l'ambiente nel catalogo SSIS.
- Distribuire ispac in SQL Server di Azure o in un'istanza gestita di SQL di Azure che consente solo l'autenticazione a più fattori.
- Distribuire i pacchetti in MSDB o nell'archivio pacchetti SSIS.

## <a name="ssis-deploy-task-version-1-preview"></a>Attività di distribuzione SSIS versione 1.* (anteprima)

Miglioramenti nella versione 1.*:

- Supporto del livello di protezione EncryptionWithPassword e EncryptionAllWithPassword.

### <a name="version-1-only-properties"></a>Proprietà solo versione 1.*

#### <a name="project-password"></a>Project Password

Password per decrittografare i file ISPAC o DTSX. Questo argomento è valido solo quando il livello di protezione è EncryptSensitiveWithPassword o EncryptAllWithPassword.

## <a name="ssis-catalog-configuration-task"></a>Attività di configurazione del catalogo SSIS

![Attività di configurazione del catalogo](media/ssis-catalog-configuation-task.png)

### <a name="properties"></a>Proprietà

#### <a name="configuration-file-source"></a>Configuration file source (Origine del file di configurazione)

Origine del file JSON di configurazione del catalogo SSIS. Può essere "Percorso file" o "Inline".

Vedere le informazioni dettagliate su come [definire il file JSON di configurazione](#define-configuration-json):

- Vedere [un file JSON di configurazione inline di esempio](#a-sample-inline-configuration-json).
- Controllare lo [schema JSON](#json-schema).

#### <a name="configuration-json-file-path"></a>Configuration JSON file path (Percorso del file JSON di configurazione)

Percorso del file JSON di configurazione del catalogo SSIS. Questa proprietà è visibile solo quando si seleziona "Percorso file" come origine del file di configurazione.

Per usare le [variabili della pipeline](/azure/devops/pipelines/process/variables) nel file JSON di configurazione, è necessario aggiungere un'[attività di trasformazione file](/azure/devops/pipelines/tasks/utility/file-transform?view=azure-devops&preserve-view=true) prima di questa attività per sostituire i valori di configurazione con le variabili della pipeline. Per altre informazioni, vedere [Sostituzione di variabili JSON](/azure/devops/pipelines/tasks/transforms-variable-substitution?tabs=Classic&view=azure-devops&preserve-view=true#json-variable-substitution).

#### <a name="inline-configuration-json"></a>Inline configuration JSON (Percorso del file JSON di configurazione)

File JSON inline della configurazione del catalogo SSIS. Questa proprietà è visibile solo quando si seleziona "Inline" come origine del file di configurazione. È possibile usare direttamente le variabili della pipeline.

#### <a name="roll-back-configuration-when-error-occurs"></a>Roll back configuration when error occurs (Esegui il rollback della configurazione quando si verifica un errore)

Indica se eseguire il rollback della configurazione effettuata da questa attività quando si verifica un errore.

#### <a name="target-server"></a>Server di destinazione

Nome dell'istanza di SQL Server di destinazione. Può corrispondere al nome di un database di SQL Server locale, di un database SQL di Azure o di Istanza gestita di SQL di Azure.

#### <a name="authentication-type"></a>Tipo di autenticazione

Tipo di autenticazione per accedere al server di destinazione specificato. In generale, sono supportati i tipi di autenticazione seguenti:

- Autenticazione di Windows
- Autenticazione di SQL Server
- Active Directory - Password
- Active Directory - Integrata

Il supporto di un determinato tipo di autenticazione dipende tuttavia dal tipo di server di destinazione e dal tipo di agente. Per una matrice di supporto dettagliata, vedere la tabella seguente.

|Tipo server di destinazione|Pool ospitato da Microsoft|Agente self-hosted|
|---------|---------|---------|
|SQL Server in locale o macchina virtuale |N/D|Autenticazione di Windows|
|SQL di Azure|Autenticazione di SQL Server <br> Active Directory - Password|Autenticazione di SQL Server <br> Active Directory - Password <br> Active Directory - Integrata|

#### <a name="username"></a>Username

Nome utente per accedere all'istanza di SQL Server di destinazione. Questa proprietà è visibile solo se il tipo di autenticazione è Autenticazione SQL Server o Active Directory - Password.

#### <a name="password"></a>Password

Password per accedere all'istanza di SQL Server di destinazione. Questa proprietà è visibile solo se il tipo di autenticazione è Autenticazione SQL Server o Active Directory - Password.

### <a name="define-configuration-json"></a>Definire il file JSON di configurazione

Lo schema JSON di configurazione ha tre livelli:

- catalogo
- folder
- progetto e ambiente

![Schema della configurazione del catalogo](media/catalog-configuration-schema.png)

#### <a name="a-sample-inline-configuration-json"></a>File JSON di configurazione inline di esempio

```json
{
  "folders": [
    {
      "name": "devopsdemo",
      "description": "devops demo folder",
      "projects": [
        {
          "name": "catalog devops",
          "parameters": [
            {
              "name": "password",
              "container": "Package.dtsx",
              "value": "passwd",
              "valueType": "referenced"
            },
            {
              "name": "serverName",
              "container": "catalog devops",
              "value": "localhost",
              "valueType": "literal"
            }
          ],
          "references": [
            {
              "environmentName": "test",
              "environmentFolder": "devopsdemo"
            },
            {
              "environmentName": "test",
              "environmentFolder": "."
            }
          ]
        }
      ],
      "environments": [
        {
          "name": "test",
          "description": "test",
          "variables": [
            {
              "name": "passwd",
              "type": "string",
              "description": "",
              "value": "$(SSISDBServerAdminPassword)",
              "sensitive": true
            },
            {
              "name": "serverName",
              "type": "string",
              "description": "",
              "value": "$(TargetServerName)",
              "sensitive": false
            }
          ]
        }
      ]
    }
  ]
}
```

#### <a name="json-schema"></a>Schema JSON

##### <a name="catalog-attributes"></a>Attributi del catalogo

|Proprietà  |Descrizione  |Note  |
|---------|---------|---------|
|cartelle  |Matrice di oggetti folder. Ogni oggetto contiene informazioni di configurazione per una cartella del catalogo.|Per lo schema di un oggetto cartella, vedere *Attributi della cartella*.|

##### <a name="folder-attributes"></a>Attributi della cartella

|Proprietà  |Descrizione  |Note  |
|---------|---------|---------|
|name  |Nome della cartella del catalogo.|La cartella verrà creata se non esiste.|
|description|Descrizione della cartella del catalogo.|Il valore *null* verrà ignorato.|
|projects|Matrice di oggetti project. Ogni oggetto contiene informazioni di configurazione per un progetto.|Per lo schema di un oggetto progetto, vedere *Attributi del progetto*.|
|environments|Matrice di oggetti environment. Ogni oggetto contiene informazioni di configurazione per un ambiente.|Per lo schema di un oggetto ambiente, vedere *Attributi dell'ambiente*.|

##### <a name="project-attributes"></a>Attributi del progetto

|Proprietà  |Descrizione  |Note  |
|---------|---------|---------|
|name|Nome del progetto. |L'oggetto progetto verrà ignorato se il progetto non esiste nella cartella padre.|
|parametri|Matrice di oggetti parametro. Ogni oggetto contiene informazioni di configurazione per un parametro.|Per lo schema di un oggetto parametro, vedere *Attributi del parametro*.|
|references|Matrice di oggetti reference. Ogni oggetto rappresenta un riferimento dell'ambiente al progetto di destinazione.|Per lo schema di un oggetto riferimento, vedere *Attributi del riferimento*.|

##### <a name="parameter-attributes"></a>Attributi del parametro

|Proprietà  |Descrizione  |Note  |
|---------|---------|---------|
|name|Nome del parametro.|<li>Il parametro può essere un parametro di progetto o un parametro di pacchetto. <li>Se non esiste, il parametro viene ignorato. <li>Se il parametro è una proprietà di gestione connessione, il nome deve essere nel formato **CM.\<Connection Manager Name>.\<Property Name>** . |
|contenitore|Contenitore del parametro.|<li>Se il parametro è un parametro progetto, il *contenitore* deve corrispondere al nome del progetto. <li>Se è un parametro pacchetto, il *contenitore* deve corrispondere al nome del pacchetto con estensione **dtsx**.|
|Valore|Valore del parametro.|<li>Quando *valueType* è *referenced*: il valore è un riferimento a una variabile di ambiente nel tipo *string*. <li> Quando *valueType* è *literal*: questo attributo supporta qualsiasi valore JSON di tipo *boolean*, *number* e *string*. <li> Il valore verrà convertito nel tipo di parametro di destinazione. Se non è possibile convertirlo, si verificherà un errore.<li> Il valore *null* non è valido. L'attività ignorerà questo oggetto parametro e visualizzerà un avviso.|
|valueType|Tipo di valore del parametro.|I tipi validi sono: <br> *literal*: l'attributo *value* rappresenta un valore letterale. <br> *referenced*: l'attributo *valore* rappresenta un riferimento a una variabile di ambiente.|

##### <a name="reference-attributes"></a>Attributi del riferimento

|Proprietà  |Descrizione  |Note  |
|---------|---------|---------|
|environmentFolder|Nome cartella dell'ambiente.|La cartella verrà creata se non esiste. <br> Il valore può essere ".", che rappresenta la cartella padre del progetto, che fa riferimento all'ambiente.|
|environmentName|Nome dell'ambiente con riferimenti.|L'ambiente specificato verrà creato se non esiste.|

##### <a name="environment-attributes"></a>Attributi dell'ambiente

|Proprietà  |Descrizione  |Note  |
|---------|---------|---------|
|name|Nome dell'ambiente.|L'ambiente verrà creato se non esiste.|
|description|Descrizione dell'ambiente.|Il valore *null* verrà ignorato.|
|variables|Matrice di oggetti variable.|Ogni oggetto contiene informazioni di configurazione per una variabile di ambiente. Per lo schema di un oggetto variabile, vedere *Attributi della variabile*.|

##### <a name="variable-attributes"></a>Attributi della variabile

|Proprietà  |Descrizione  |Note  |
|---------|---------|---------|
|name|Nome della variabile di ambiente.|La variabile di ambiente verrà creata se non esiste.|
|type|Tipo di dati della variabile di ambiente.|I tipi validi sono: <br> *boolean* <br> *byte* <br> *datetime* <br> decimal <br> *double* <br> *int16* <br> *int32* <br> *int64* <br> *sbyte* <br> *single* <br> *string* <br> *uint32* <br> *uint64*|
|description|Descrizione della variabile di ambiente.|Il valore *null* verrà ignorato.|
|Valore|Valore della variabile di ambiente.|Questo attributo supporta qualsiasi valore JSON di tipo boolean, number e string.<br> Il valore verrà convertito nel tipo specificato dall'attributo **type**. Si verificherà un errore se la conversione non riesce.<br>Il valore *null* non è valido. L'attività ignorerà questo oggetto variabile di ambiente e visualizzerà un avviso.|
|sensitive|Indica se il valore della variabile di ambiente è sensibile.|Gli input validi sono: <br> *true* <br> *false*|

## <a name="release-notes"></a>Note sulla versione

### <a name="version-104"></a>Versione 1.0.4

Data di rilascio: 21 aprile 2021

- Attività di compilazione SSIS versione 1.* (anteprima)
    - Rimuovere la dipendenza da Visual Studio progettazione SSIS. L'attività di compilazione può essere eseguita in un agente ospitato da Microsoft o in un agente self-hosted con il sistema operativo Windows e .NET Framework 4.6.2 o versione successiva.
    - Non è necessario installare componenti di tipo out-of-box.
    - Supporto del livello di protezione EncryptionWithPassword e EncryptionAllWithPassword.
- Attività di distribuzione SSIS versione 1.* (anteprima)
    - Supporto del livello di protezione EncryptionWithPassword e EncryptionAllWithPassword.

### <a name="version-103"></a>Versione 1.0.3

Data di rilascio: 21 ottobre 2020

- Consente di specificare il suffisso della stringa di connessione per l'attività di distribuzione SSIS e l'attività di configurazione del catalogo SSIS.

### <a name="version-102"></a>Versione 1.0.2

Data di rilascio: 26 maggio 2020

- Correzione di un problema per cui l'attività di configurazione del catalogo SSIS potrebbe non riuscire in alcuni casi al termine della configurazione.

### <a name="version-101"></a>Versione 1.0.1

Data di rilascio: 9 maggio 2020

- È stato risolto un problema per cui l'attività di compilazione SSIS compila sempre l'intera soluzione, anche se è stato specificato come percorso del progetto un singolo file con estensione dtproj.

### <a name="version-100"></a>Versione 1.0.0

Data di rilascio: 8 maggio 2020

- Versione disponibile a livello generale.
- Aggiunta di una restrizione per la versione minima di .NET Framework per l'agente. Attualmente la versione minima di .NET Framework è 4.6.2.
- Miglioramento della descrizione delle attività SSIS Build (Compilazione SSIS) e SSIS Deploy (Distribuzione SSIS).

### <a name="version-020-preview"></a>Versione 0.2.0 Preview

Data di rilascio: 31 marzo 2020

- Aggiunta dell'attività di configurazione del catalogo SSIS.

### <a name="version-013-preview"></a>Versione 0.1.3 - Anteprima

Data di rilascio: 19 gennaio 2020

- È stato risolto un problema che impediva la distribuzione di ispac in caso di modifica del nome di file originale.

### <a name="version-012-preview"></a>Versione 0.1.2 - Anteprima

Data di rilascio: 13 gennaio 2020

- Aggiunte informazioni più dettagliate sull'eccezione nel log dell'attività di distribuzione SSIS quando il tipo di destinazione è SSISDB.
- Corretto il percorso di destinazione di esempio nel testo della Guida relativo al percorso di destinazione della proprietà dell'attività di distribuzione SSIS.

### <a name="version-011-preview"></a>Version 0.1.1 (anteprima)

Data di rilascio: 6 gennaio 2020

- Aggiunta la limitazione del requisito di versione minima dell'agente. Attualmente la versione minima dell'agente di questo prodotto è 2.144.0.
- Corretto testo errato visualizzato per l'attività di distribuzione SSIS.
- Perfezionati alcuni messaggi di errore.

### <a name="version-010-preview"></a>Version 0.1.0 (anteprima)

Data di rilascio: 5 dicembre 2019

Versione iniziale di SSIS DevOps Tools. Si tratta di una versione di anteprima.

## <a name="next-steps"></a>Passaggi successivi

- Ottenere [Estensione SSIS DevOps](https://marketplace.visualstudio.com/items?itemName=SSIS.ssis-devops-tools)
