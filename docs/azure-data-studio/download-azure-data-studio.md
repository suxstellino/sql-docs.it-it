---
title: Scaricare e installare Azure Data Studio
description: Scaricare e installare Azure Data Studio per Windows, macOS o Linux. Questo articolo include le date di rilascio, i numeri di versione, i requisiti di sistema e i collegamenti di download.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: overview
author: yualan
ms.author: alayu
ms.reviewer: maghan
ms.custom: seodec18
ms.date: 3/17/2021
ms.openlocfilehash: 0a88fcbdd027215b8ce1bcd6242f48833ec0c6b2
ms.sourcegitcommit: bf7577b3448b7cb0e336808f1112c44fa18c6f33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2021
ms.locfileid: "104610820"
---
# <a name="download-and-install-azure-data-studio"></a>Scaricare e installare Azure Data Studio

Azure Data Studio è uno strumento di database multipiattaforma per professionisti della gestione di dati che usano piattaforme dati locali e nel cloud in Windows, macOS e Linux.

Azure Data Studio offre un'esperienza di modifica moderna per la gestione dei dati tra più origini con funzionalità IntelliSense, frammenti di codice, integrazione del controllo del codice sorgente e un terminale integrato. Azure Data Studio è stato progettato in base alle esigenze dell'utente della piattaforma dati, con grafici predefiniti di set di risultati delle query e dashboard personalizzabili. Per altre informazioni su Azure Data Studio, vedere [Che cos'è Azure Data Studio](what-is-azure-data-studio.md).

## <a name="download-the-latest-release"></a>Scaricare la versione più recente

| Piattaforma | Download | Data di rilascio | Versione |
|----------|----------|--------------|---------|
| Windows | [Programma di installazione utente (scelta consigliata)](https://go.microsoft.com/fwlink/?linkid=2157460)<br>[Programma di installazione di sistema](https://go.microsoft.com/fwlink/?linkid=2157459)<br>[.zip](https://go.microsoft.com/fwlink/?linkid=2157458) | 17 marzo 2021 | 1.27.0 |
| macOS | [.zip](https://go.microsoft.com/fwlink/?linkid=2157456) | 17 marzo 2021 | 1.27.0 |
| Linux | [.deb](https://go.microsoft.com/fwlink/?linkid=2157352)<br>[.rpm](https://go.microsoft.com/fwlink/?linkid=2157248)<br>[.tar.gz](https://go.microsoft.com/fwlink/?linkid=2157353) | 17 marzo 2021 | 1.27.0 |

**Per informazioni dettagliate sulla versione più recente, vedere le [note sulla versione](./release-notes-azure-data-studio.md).**

## <a name="get-azure-data-studio-for-windows"></a>Ottenere Azure Data Studio per Windows

[!INCLUDE [ssms-ads-install](../includes/ssms-azure-data-studio-install.md)]

Questa versione di Azure Data Studio include un'esperienza di Windows Installer standard e un file ZIP.

Si consiglia di usare il *programma di installazione utente* perché non richiede privilegi di amministratore, il che semplifica sia le installazioni che gli aggiornamenti. Il programma di installazione utente non richiede privilegi di amministratore perché il percorso si trova nella cartella AppData locale (LOCALAPPDATA) dell'utente. Il programma di installazione utente fornisce anche un'esperienza di aggiornamento in background più fluida. Per altre informazioni, vedere l'articolo sulla [Configurazione utente per Windows](https://code.visualstudio.com/updates/v1_26#_user-setup-for-windows).

**Programma di installazione utente** (scelta consigliata)

1. Scaricare ed eseguire il [programma di installazione *utente* di Azure Data Studio per Windows](https://go.microsoft.com/fwlink/?linkid=2157460).
2. Avviare l'app Azure Data Studio.

**Programma di installazione di sistema**

1. Scaricare ed eseguire il [programma di installazione *di sistema* di Azure Data Studio per Windows](https://go.microsoft.com/fwlink/?linkid=2157459).
2. Avviare l'app Azure Data Studio.

**.zip file**

1. Scaricare il [file ZIP di Azure Data Studio per Windows](https://go.microsoft.com/fwlink/?linkid=2157458).
2. Individuare il file scaricato ed estrarlo.
3. Eseguire `\azuredatastudio-windows\azuredatastudio.exe`

## <a name="get-azure-data-studio-for-macos"></a>Ottenere Azure Data Studio per macOS

1. Scaricare [Azure Data Studio per macOS](https://go.microsoft.com/fwlink/?linkid=2157456).
2. Per espandere il contenuto del file ZIP, fare doppio clic su di esso.
3. Per rendere Azure Data Studio disponibile in *Launchpad*, trascinare *Azure Data Studio.app* nella cartella *Applicazioni*.

## <a name="get-azure-data-studio-for-linux"></a>Ottenere Azure Data Studio per Linux

1. Scaricare Azure Data Studio per Linux usando uno dei programmi di installazione o l'archivio tar.gz:
    - [.deb](https://go.microsoft.com/fwlink/?linkid=2157352)
    - [.rpm](https://go.microsoft.com/fwlink/?linkid=2157248)
    - [.tar.gz](https://go.microsoft.com/fwlink/?linkid=2157353)
1. Per estrarre il file e avviare Azure Data Studio, aprire una nuova finestra del terminale e digitare i comandi seguenti:

   **Installazione Debian:**

   ```bash
   cd ~
   sudo dpkg -i ./Downloads/azuredatastudio-linux-<version string>.deb

   azuredatastudio
   ```

   **Installazione RPM:**

   ```bash
   cd ~
   yum install ./Downloads/azuredatastudio-linux-<version string>.rpm

   azuredatastudio
   ```

   **Installazione tar.gz:**

   ```bash
   cd ~
   cp ~/Downloads/azuredatastudio-linux-<version string>.tar.gz ~ 
   tar -xvf ~/azuredatastudio-linux-<version string>.tar.gz 
   echo 'export PATH="$PATH:~/azuredatastudio-linux-x64"' >> ~/.bashrc
   source ~/.bashrc
   azuredatastudio
   ```

   > [!NOTE]
   > In Debian, Red Hat e Ubuntu potrebbero mancare alcune dipendenze. Usare i comandi seguenti per installare queste dipendenze a seconda della versione di Linux:

   **Debian:**

   ```bash
   sudo apt-get install libunwind8
   ```

   **RedHat:**

   ```bash
   yum install libXScrnSaver
   ```

   **Ubuntu:**

   ```bash
   sudo apt-get install libxss1

   sudo apt-get install libgconf-2-4

   sudo apt-get install libunwind8
   ```

## <a name="download-insiders-build-of-azure-data-studio"></a>Scaricare la build Insider di Azure Data Studio

In generale, gli utenti dovrebbero scaricare la versione stabile di Azure Data Studio precedente. Tuttavia, se si vogliono provare le funzionalità beta e inviare commenti e suggerimenti, è possibile scaricare la [build Insider di Azure Data Studio](https://github.com/microsoft/azuredatastudio#try-out-the-latest-insiders-build-from-main).

## <a name="supported-operating-systems"></a>Sistemi operativi supportati

Azure Data Studio viene eseguito in Windows, macOS e Linux ed è supportato nelle piattaforme seguenti:

### <a name="windows"></a>Windows

- Windows 10 (64 bit)
- Windows 8.1 (64 bit)
- Windows 8 (64 bit)
- Windows 7 (SP1)
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2 (64 bit)
- Windows Server 2012 (64 bit)
- Windows Server 2008 R2 (64 bit)

### <a name="macos"></a>macOS

- macOS 10.15 Catalina
- macOS 10.14 Mojave
- macOS 10.13 High Sierra
- macOS 10.12 Sierra
- macOS 11.1 Big Sur

### <a name="linux"></a>Linux

- Red Hat Enterprise Linux 7.4
- Red Hat Enterprise Linux 7.3
- SUSE Linux Enterprise Server v12 SP2
- Ubuntu 16.04

## <a name="recommended-system-requirements"></a>Requisiti di sistema consigliati

| Consigliati/Minimi | Core CPU | Memoria/RAM |
|---------------------|-----------|------------|
|     Consigliato     |     4     |   8 GB     |
|     Minima         |     2     |   4 GB     |

## <a name="check-for-updates"></a>Verificare gli aggiornamenti

Per verificare la disponibilità di aggiornamenti più recenti, selezionare l'icona a forma di ingranaggio nella parte inferiore sinistra della finestra e selezionare **Controlla aggiornamenti**.

In un ambiente offline gli aggiornamenti possono essere applicati tramite l'[installazione della versione più recente](#download-and-install-azure-data-studio) direttamente su una versione installata in precedenza. La disinstallazione delle versioni precedenti di Azure Data Studio non è necessaria. Il programma di installazione aggiorna un'applicazione installata, se presente.

## <a name="supported-sql-offerings"></a>Offerte di SQL supportate

- Questa versione di Azure Data Studio funziona con tutte le [versioni supportate [!INCLUDE [sssql14-md](../includes/sssql14-md.md)]  -  [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] di](https://support.microsoft.com/lifecycle?C2=1044) e fornisce il supporto per l'uso delle funzionalità cloud più recenti nel database SQL di Azure e nell'analisi delle sinapsi di Azure. Azure Data Studio fornisce anche il supporto in anteprima per Istanza gestita di SQL di Azure.

## <a name="move-user-settings"></a>Spostare le impostazioni utente

Se si esegue l'aggiornamento da SQL Operations Studio ad Azure Data Studio e si vogliono mantenere le impostazioni, le scelte rapide da tastiera o i frammenti di codice, seguire la procedura riportata di seguito.

*Se si ha già Azure Data Studio o non si è mai installato o personalizzato SQL Operations Studio, è possibile ignorare questa sezione.*

1. Aprire le impostazioni selezionando l'icona a forma di ingranaggio in basso a sinistra e selezionando **Impostazioni.**

   ![modificare le impostazioni in Azure Data Studio](./media/download/open-settings.png)

2. Fare clic con il pulsante destro del mouse sulla scheda **Impostazioni utente** nella parte superiore e scegliere **Visualizza in Esplora risorse**

   ![avviare Esplora risorse, che consente di passare al file system locale](./media/download/reveal-in-explorer.png)

3. Copiare tutti i file in questa cartella e salvarli in un percorso facile da trovare nell'unità locale, ad esempio la cartella Documenti.

   ![usare i file e copiarli nel percorso](./media/download/copy-settings.png)

4. Nella nuova versione di Azure Data Studio, seguire i passaggi 1-2, quindi, per il passaggio 3, incollare il contenuto salvato nella cartella. È anche possibile copiare manualmente le impostazioni, i tasti di scelta rapida o i frammenti di codice nelle rispettive posizioni.

5. Se si esegue l'override di un'installazione esistente, eliminare la vecchia directory di installazione prima dell'installazione per evitare errori di connessione all'account Azure per Resource Explorer.

## <a name="unattended-install-for-windows"></a>Installazione automatica per Windows

È anche possibile installare Azure Data Studio usando uno script del prompt dei comandi.

Se si vuole installare Azure Data Studio in background senza prompt dell'interfaccia utente grafica e si usa la piattaforma Windows, seguire questa procedura.

1. Avviare il prompt dei comandi con autorizzazioni elevate.

2. Digitare il comando seguente nel prompt dei comandi.

    ```console
    <path where the azuredatastudio-windows-user-setup-x.xx.x.exe file is located> /VERYSILENT /MERGETASKS=!runcode>
    ```

    Esempio:

    ```console
    %systemdrive%\azuredatastudio-windows-user-setup-1.24.0.exe /VERYSILENT /MERGETASKS=!runcode
    ```

    > [!Note]
    > L'esempio funziona anche con il file del programma di installazione di sistema.
    > 
    > ```console
    > <path where the azuredatastudio-windows-setup-x.xx.x.exe file is located> /VERYSILENT /MERGETASKS=!runcode>
    > ```

    È anche possibile passare */SILENT* invece di */VERYSILENT* per visualizzare l'interfaccia utente del programma di installazione.

3. Se tutto va bene, Azure Data Studio verrà installato.

## <a name="uninstall-azure-data-studio"></a>Disinstallare Azure Data Studio

Se Azure Data Studio è stato installato usando Windows Installer, disinstallarlo così come si rimuove qualsiasi applicazione Windows.

Se Azure Data Studio è stato installato con un file ZIP o con un altro archivio, eliminare i file.

## <a name="next-steps"></a>Passaggi successivi

Per iniziare, vedere uno degli argomenti di avvio rapido seguenti:

- [Informazioni su Azure Data Studio](what-is-azure-data-studio.md)
- [Note sulla versione di Azure Data Studio](release-notes-azure-data-studio.md)
- [Connettersi ed eseguire query in SQL Server](quickstart-sql-server.md)
- [Connettersi ed eseguire query nel database SQL di Azure](quickstart-sql-database.md)
- [Connettersi ed eseguire query in Azure Synapse Analytics](quickstart-sql-dw.md)

[!INCLUDE[get-help-sql-tools](../includes/paragraph-content/get-help-sql-tools.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

[Informativa sulla privacy di Microsoft](https://go.microsoft.com/fwlink/?LinkId=521839) e [Raccolta dati di utilizzo](usage-data-collection.md).
