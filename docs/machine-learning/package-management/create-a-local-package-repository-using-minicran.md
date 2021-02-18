---
title: Creare un repository con miniCRAN
titleSuffix: SQL machine learning
description: Informazioni su come installare offline i pacchetti R usando miniCRAN per creare un repository locale di pacchetti e dipendenze.
ms.prod: sql
ms.technology: machine-learning
ms.date: 11/20/2019
ms.topic: how-to
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: f93a1180f25f764330db44306457e775961bc0da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350690"
---
# <a name="create-a-local-r-package-repository-using-minicran"></a>Creare un repository di pacchetti R locale usando miniCRAN
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Questo articolo descrive come installare i pacchetti R offline usando [miniCRAN](https://cran.r-project.org/web/packages/miniCRAN/index.html) per creare un repository locale di pacchetti e dipendenze. **miniCRAN** identifica e scarica pacchetti e dipendenze in un'unica cartella che è possibile copiare in altri computer per l'installazione offline di pacchetti R.

È possibile specificare uno o più pacchetti. **miniCRAN** legge in modo ricorsivo l'albero delle dipendenze e quindi scarica solo i pacchetti specificati e le relative dipendenze da CRAN o da repository simili.

Al termine, **miniCRAN** crea un repository internamente coerente costituito dai pacchetti selezionati e da tutte le dipendenze necessarie. È possibile spostare questo repository locale nel server e procedere all'installazione dei pacchetti senza una connessione Internet.

Gli utenti di R esperti cercano spesso l'elenco dei pacchetti dipendenti nel file DESCRIPTION di un pacchetto scaricato. Tuttavia, i pacchetti elencati in **Imports** potrebbero avere dipendenze di secondo livello. Per questo motivo, è consigliabile usare **miniCRAN** per assemblare la raccolta completa dei pacchetti necessari.

## <a name="why-create-a-local-repository"></a>Perché creare un repository locale

Lo scopo per cui si crea un repository di pacchetti locale è di fornire un'unica posizione che può essere usata da un amministratore del server o da altri utenti dell'organizzazione per installare nuovi pacchetti R in un server, soprattutto se questo non dispone di accesso a Internet. Dopo aver creato il repository, è possibile modificarlo aggiungendo nuovi pacchetti o aggiornando la versione dei pacchetti esistenti.

I repository di pacchetti sono utili negli scenari seguenti:

- **Sicurezza**: molti utenti di R sono abituati a scaricare e installare nuovi pacchetti R a loro discrezione, da CRAN o da uno dei suoi siti mirror. Per motivi di sicurezza, tuttavia, i server di produzione che eseguono [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] in genere non dispongono di connettività Internet.

- **Installazione offline facilitata**: per installare un pacchetto in un server offline, è necessario scaricare anche tutte le dipendenze del pacchetto. Con miniCRAN è più semplice ottenere tutte le dipendenze nel formato corretto ed evitare errori di dipendenza.

- **Gestione delle versioni migliorata**: in un ambiente multiutente è in genere opportuno evitare l'installazione senza restrizioni di più versioni di pacchetto nel server. Usare un repository locale per rendere disponibile un set coerente di pacchetti per gli utenti.

## <a name="install-minicran"></a>Installare miniCRAN

Il pacchetto stesso **miniCRAN** dipende da altri 18 pacchetti di CRAN, tra cui **RCurl**, che ha a sua volta una dipendenza di sistema dal pacchetto **curl-devel**. Analogamente, il pacchetto **XML** presenta una dipendenza da **libxml2-devel**. Per risolvere le dipendenze, è consigliabile creare inizialmente il repository locale in un computer con accesso completo a Internet.

Eseguire i comandi seguenti in un computer con un pacchetto R di base, R Tools e una connessione Internet. Si presuppone che non sia il computer di SQL Server. I comandi seguenti installano i pacchetti **miniCRAN** e **igraph**. Questo esempio controlla se il pacchetto è già installato, ma è possibile ignorare le istruzioni `if` e installare direttamente i pacchetti.

```R
if(!require("miniCRAN")) install.packages("miniCRAN") 
if(!require("igraph")) install.packages("igraph") 
library("miniCRAN")
```

## <a name="set-the-cran-mirror-and-mran-snapshot"></a>Impostare il mirror di CRAN e lo snapshot di MRAN

Specificare un sito mirror da usare per ottenere i pacchetti. È ad esempio possibile usare il sito MRAN o qualsiasi altro sito nella propria area che contiene i pacchetti necessari. Se il download non riesce, provare con un altro sito mirror.

```R
CRAN_mirror <- c(CRAN = "https://cran.cnr.berkeley.edu")
```

## <a name="create-a-local-folder"></a>Creare una cartella locale

Creare una cartella locale in cui archiviare i pacchetti raccolti. Se si ripete spesso questa operazione, può essere opportuno usare un nome descrittivo, ad esempio "PacchettiZoominiCRAN" o "PacchettoRV2miniCRAN".

Specificare la cartella come repository locale. La sintassi R richiede l'uso della barra per i nomi di percorso, diversamente dalla convenzione di Windows che prevede l'uso della barra rovesciata.

```R
local_repo <- "C:/miniCRANZooPackages"
```

## <a name="add-packages-to-the-local-repo"></a>Aggiungere pacchetti al repository locale

Dopo l'installazione e il caricamento di **miniCRAN**, creare l'elenco dei pacchetti aggiuntivi da scaricare.

**Non** aggiungere dipendenze a questo elenco iniziale. Il pacchetto **igraph** usato da **miniCRAN** genera l'elenco delle dipendenze automaticamente. Per altre informazioni su come usare il grafico delle dipendenze generato, vedere [Using miniCRAN to identify package  dependencies](https://cran.r-project.org/web/packages/miniCRAN/vignettes/miniCRAN-dependency-graph.html) (Uso di miniCRAN per identificare le dipendenze dei pacchetti).

1. Aggiungere i pacchetti di destinazione "zoo" e "forecast" a una variabile.

    ```R
    pkgs_needed <- c("zoo", "forecast")
    ```

2. Facoltativamente, tracciare il grafico delle dipendenze. Questa operazione non è necessaria, ma può essere utile a scopo informativo.

    ```R
    plot(makeDepGraph(pkgs_needed))
    ```

3. Creare il repository locale. Assicurarsi di sostituire la versione di R, se necessario, con la versione installata nell'istanza di SQL Server. Se è stato eseguito un aggiornamento del componente, la versione potrebbe essere più recente di quella originale. Per altre informazioni, vedere [Ottenere informazioni sui pacchetti R](../package-management/r-package-information.md).

    ```R
    pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);
    makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "win.binary", Rversion = "3.3");
    ```

   In base a queste informazioni, il pacchetto miniCRAN crea la struttura di cartelle di [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] in cui successivamente sarà necessario copiare i pacchetti.

A questo punto dovrebbe essere presente una cartella contenente i pacchetti necessari ed eventuali pacchetti aggiuntivi richiesti. La cartella dovrebbe contenere una raccolta di pacchetti ZIP. Non decomprimerli né rinominare i file.

Facoltativamente, eseguire il codice seguente per elencare i pacchetti contenuti nel repository miniCRAN locale.

```R
pdb <- as.data.frame(pkgAvail(local_repo, type = "win.binary", Rversion = "3.3"), stringsAsFactors = FALSE);
head(pdb);
pdb$Package;
pdb[, c("Package", "Version", "License")]
```

## <a name="add-packages-to-the-instance-library"></a>Aggiungere pacchetti alla libreria dell'istanza

Quando si ha un repository locale con i pacchetti necessari, spostarlo nel computer di SQL Server. La procedura seguente descrive come installare i pacchetti usando R Tools.

::: moniker range=">sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current"
> [!NOTE]
> Il metodo consigliato per l'installazione dei pacchetti consiste nell'usare **sqlmlutils**. Vedere [Installare nuovi pacchetti R con sqlmlutils](install-additional-r-packages-on-sql-server.md).
::: moniker-end

1. Copiare l'intera cartella contenente il repository miniCRAN nel server in cui si prevede di installare i pacchetti. La cartella ha in genere la struttura seguente: 

   `<miniCRAN root>/bin/windows/contrib/version/<all packages>`

   In questa procedura si presuppone che la cartella non si trovi nell'unità radice.

2. Aprire uno strumento R associato all'istanza. È ad esempio possibile usare Rgui.exe. Fare clic con il pulsante destro del mouse e scegliere **Esegui come amministratore** per consentire allo strumento di eseguire aggiornamenti al sistema.

   ::: moniker range="=sql-server-2016"
   - Ad esempio, il percorso di file predefinito per RGUI è `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64`.
   ::: moniker-end

   ::: moniker range="=sql-server-2017"
   - Ad esempio, il percorso di file per RGUI è `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\bin\x64`.
   ::: moniker-end

   ::: moniker range=">sql-server-2017"
   - Ad esempio, il percorso di file per RGUI è `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\R_SERVICES\bin\x64`.
   ::: moniker-end

3. Ottenere il percorso della libreria dell'istanza e aggiungerlo all'elenco dei percorsi di libreria.

   ::: moniker range="=sql-server-2016"
   Ad esempio,

   ```R
   outputlib <- "C:/Program Files/Microsoft SQL Server/MSSQL13.MSSQLSERVER/R_SERVICES/library"
   ```

   ::: moniker-end

   ::: moniker range="=sql-server-2017"
   Ad esempio,

   ```R
   outputlib <- "C:/Program Files/Microsoft SQL Server/MSSQL14.MSSQLSERVER/R_SERVICES/library"
   ```

   ::: moniker-end

   ::: moniker range=">sql-server-2017"
   Ad esempio,

   ```R
   outputlib <- "C:/Program Files/Microsoft SQL Server/MSSQL15.MSSQLSERVER/R_SERVICES/library"
   ```

   ::: moniker-end

4. Specificare il nuovo percorso nel server in cui è stato copiato il repository **miniCRAN** come `server_repo`.

    In questo esempio si presuppone che il repository sia stato copiato in una cartella temporanea sul server.

    ```R
    inputlib <- "C:/miniCRANZooPackages"
    ```

5. Poiché si sta lavorando in una nuova area di lavoro di R sul server, è necessario specificare anche l'elenco dei pacchetti da installare.

    ```R
    mypackages <- c("zoo", "forecast")
    ```

6. Installare i pacchetti, specificando il percorso della copia locale del repository miniCRAN.

    ```R
    install.packages(mypackages, repos = file.path("file://", normalizePath(inputlib, winslash = "/")), lib = outputlib, type = "win.binary", dependencies = TRUE);
    ```

7. Dalla libreria dell'istanza è possibile visualizzare i pacchetti installati usando un comando simile al seguente:

    ```R
    installed.packages()
    ```

## <a name="see-also"></a>Vedere anche

+ [Ottenere informazioni sui pacchetti R](../package-management/r-package-information.md)
+ [Esercitazioni di R](../tutorials/r-tutorials.md)