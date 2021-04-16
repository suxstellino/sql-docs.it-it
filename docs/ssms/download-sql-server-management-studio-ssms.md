---
title: Scaricare SQL Server Management Studio (SSMS)
description: Scaricare la versione più recente di SQL Server Management Studio (SSMS).
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
keywords:
- installazione di ssms, download di ssms, ssms più recente
- SQL Server Management Studio
- ssms.exe
- sql man studio
- sql management studio
- installazione di sql management studio
- scaricare sql management studio
- ms sql management studio
- installare sql management studio
- ssms download
- sql server ssms
- ssms express
ms.assetid: adafeeef-4255-4924-8042-02f503d599ca
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 4/15/2021
ms.openlocfilehash: 2f51332f7aed45a6e53b7b765525457bb315713b
ms.sourcegitcommit: 233be9adaee3d19b946ce15cfcb2323e6e178170
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2021
ms.locfileid: "107561019"
---
# <a name="download-sql-server-management-studio-ssms"></a>Scaricare SQL Server Management Studio (SSMS)

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

SQL Server Management Studio (SSMS) è un ambiente integrato per la gestione di qualsiasi infrastruttura SQL, da SQL Server al database SQL di Azure. SSMS offre gli strumenti per configurare, monitorare e amministrare le istanze di SQL Server e i database. Usare SSMS per distribuire, monitorare e aggiornare i componenti del livello dati usati dalle applicazioni, nonché per creare query e script.

È possibile usare SSMS per eseguire query, progettare e gestire database e data warehouse in qualsiasi posizione, nel computer locale o nel cloud.

## <a name="download-ssms"></a>Scaricare SSMS

:::image type="icon" source="media/download-icon.png" border="false"::: **[Scaricare SQL Server Management Studio (SSMS)](https://aka.ms/ssmsfullsetup)**

SSMS 18.9 è la versione più recente a disponibilità generale di SSMS. Se è installata una versione ga precedente di SSMS 18, l'installazione di SSMS 18.9 la aggiorna alla versione 18.9.

[!INCLUDE [ssms-ads-install](../includes/ssms-azure-data-studio-install.md)]

- Numero di versione: 18.9
- Numero di build: 15.0.18382.0
- Data di rilascio: 15 aprile 2021

Per commenti, suggerimenti o per segnalare problemi, il canale migliore per contattare il team SSMS è la pagina dei [commenti e suggerimenti degli utenti per SQL Server](https://aka.ms/sqlfeedback).

L'installazione di SSMS 18.x non aggiorna o sostituisce SSMS 17.x o le versioni precedenti. SSMS 18.x viene installato side-by-side con le versioni precedenti, in modo che entrambe le versioni siano disponibili per l'uso. Tuttavia, se è installata *una* versione di anteprima di SSMS 18.x, è necessario disinstallarla prima di installare SSMS 18.9. Per verificare se è presente la versione di anteprima, passare alla finestra **Guida > Informazioni su**.

Se un computer contiene installazioni affiancate di SQL Server Management Studio, assicurarsi di avviare la versione corretta per le specifiche esigenze. La versione più recente è denominata **Microsoft SQL Server Management Studio 18**.


## <a name="available-languages"></a>Lingue disponibili

Questa versione di SSMS può essere installata nelle lingue seguenti:

SQL Server Management Studio 18.9:  
[Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2160964&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2160964&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2160964&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2160964&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2160964&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2160964&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2160964&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2160964&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2160964&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2160964&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2160964&clcid=0x40a)


> [!TIP]
> Se si accede a questa pagina da una versione non in lingua inglese e si vuole visualizzare il contenuto più aggiornato, visitare la [versione in inglese del sito](https://aka.ms/downloadssmsusenglish). È possibile scaricare una versione del sito diversa da quella in lingua inglese selezionando [available languages](#available-languages) (lingue disponibili).

> [!NOTE]
> Il modulo SQL Server PowerShell è ora un'installazione separata tramite PowerShell Gallery. Per altre informazioni, vedere [Scaricare il modulo SQL Server PowerShell](../powershell/download-sql-server-ps-module.md).

## <a name="whats-new"></a>Novità

Per informazioni dettagliate e altre informazioni sulle novità di questa versione, vedere [Note sulla versione di SSMS](release-notes-ssms.md).

Sono presenti alcuni [problemi noti](release-notes-ssms.md#known-issues-189) in questa versione.

## <a name="previous-versions"></a>Versioni precedenti

Questo articolo è solo per la versione più recente di SSMS. Per scaricare le versioni precedenti di SSMS, vedere [Versioni precedenti di SSMS](../ssms/release-notes-ssms.md#previous-ssms-releases).

[!INCLUDE[ssms-connect-azure-ad](../includes/ssms-connect-azure-ad.md)]

## <a name="unattended-install"></a>Installazione automatica

È anche possibile installare SSMS usando uno script del prompt dei comandi.

Se si vuole installare SSMS in background senza richieste dell'interfaccia utente grafica, seguire questa procedura.

1. Avviare il prompt dei comandi con autorizzazioni elevate.

2. Digitare il comando seguente nel prompt dei comandi.

    ```console
    start "" /w <path where SSMS-Setup-ENU.exe file is located> /Quiet SSMSInstallRoot=<path where you want to install SSMS>
    ```

    Esempio:

    ```console
    start "" /w %systemdrive%\SSMSfrom\SSMS-Setup-ENU.exe /Quiet SSMSInstallRoot=%systemdrive%\SSMSto
    ```

    È anche possibile passare */Passive* invece di */Quiet* per visualizzare l'interfaccia utente del programma di installazione.

3. Se l'operazione ha esito positivo, SSMS viene installato in %systemdrive%\SSMSto\Common7\IDE\Ssms.exe in base all'esempio. Se si verificano errori, è possibile verificare il codice di errore restituito nel file di log in %TEMP%\SSMSSetup.

## <a name="installation-with-azure-data-studio"></a>Installazione con Azure Data Studio

- A partire da SSMS 18.7, SSMS installa una versione di sistema di Azure Data Studio per impostazione predefinita.  Se nella workstation è già presente una versione di sistema di Azure Data Studio (versione stabile o build Insider) uguale o successiva rispetto alla versione inclusa di Azure Data Studio, SSMS ignorerà l'installazione di Azure Data Studio. La versione di Azure Data Studio è disponibile nelle note sulla versione.
- Il programma di installazione del sistema di Azure Data Studio richiede gli stessi diritti di sicurezza del programma di installazione di SSMS.
- L'installazione di Azure Data Studio viene completata con le opzioni di installazione predefinite di Azure Data Studio. È necessario creare una cartella del menu Start e aggiungere Azure Data Studio al percorso. Non viene creato un collegamento sul desktop e Azure Data Studio non viene registrato come editor predefinito per nessun tipo di file.
- La localizzazione di Azure Data Studio viene eseguita usando le estensioni dei language pack. Per localizzare Azure Data Studio, scaricare il language pack corrispondente dal [marketplace delle estensioni](../azure-data-studio/extensions/add-extensions.md).
- A questo punto, l'installazione di Azure Data Studio può essere ignorata avviando il programma di installazione di SSMS con il flag della riga di comando `DoNotInstallAzureDataStudio=1`.

## <a name="uninstall"></a>Disinstallare

Sono presenti componenti condivisi che rimangono installati dopo la disinstallazione di SSMS.

I componenti condivisi che rimangono installati sono:

- Azure Data Studio
- Microsoft .NET Framework 4.7.2
- Driver Microsoft OLE DB per SQL Server
- Microsoft ODBC Driver 17 per SQL Server
- Microsoft Visual C++ 2013 Redistributable (x86)
- Microsoft Visual C++ 2017 Redistributable (x86)
- Microsoft Visual C++ 2017 Redistributable (x64)
- Microsoft Visual Studio Tools for Applications 2017

Questi componenti non vengono disinstallati perché possono essere condivisi con altri prodotti. In caso di disinstallazione si potrebbe correre il rischio di disabilitare altri prodotti.

## <a name="supported-sql-offerings"></a>Offerte di SQL supportate

- Questa versione di SSMS funziona con tutte le [versioni supportate di SQL Server 2008 - [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]](https://support.microsoft.com/lifecycle?C2=1044) e offre il massimo livello di supporto per l'uso con le funzionalità cloud più recenti nel database SQL di Azure e in Azure Synapse Analytics.
- È supportata anche l'installazione side-by-side di SSMS 18.x con SSMS 17.x, SSMS 16.x o SQL Server 2014 SSMS e versioni precedenti.
- SQL Server Integration Services (SSIS) - SQL Server Management Studio versione 17.x o successive non supporta la connessione al servizio legacy SQL Server Integration Services. Per connettersi a una versione precedente di Integration Services, usare la versione di SQL Server Management Studio allineata alla versione di SQL Server. Ad esempio, usare SQL Server Management Studio 16.x per connettersi al servizio SQL Server 2016 Integration Services. SSMS 17.x e SSMS 16.x possono essere installati affiancati nello stesso computer. A partire dalla versione di SQL Server 2012, è consigliabile usare il database Catalogo SSIS, SSISDB, per memorizzare, gestire, eseguire e monitorare i pacchetti Integration Services. Per informazioni dettagliate, vedere [Catalogo SSIS](../integration-services/catalog/ssis-catalog.md).

## <a name="ssms-system-requirements"></a>Requisiti di sistema per SSMS

Se usata con l'ultimo Service Pack disponibile, la versione corrente di SSMS supporta le piattaforme a 64 bit seguenti:

Sistemi operativi supportati:

- Windows 10 (64 bit) versione 1607 (10.0.14393) o successiva
- Windows 8.1 (64 bit)
- Windows Server 2019 (64 bit)
- Windows Server 2016 (64 bit)
- Windows Server 2012 R2 (64 bit)
- Windows Server 2012 (64 bit)
- Windows Server 2008 R2 (64 bit)

Hardware supportato:

- Processore x86 (Intel, AMD) da 1,8 GHz o più veloce. Dual-core o superiore (opzione consigliata)
- 2 GB di RAM, 4 GB di RAM (opzione consigliata). Minimo 2,5 GB se in esecuzione in una macchina virtuale
- Spazio su disco: Minimo 2 GB e fino a 10 GB di spazio disponibile

> [!NOTE]
> SSMS è disponibile solo come applicazione a 32-bit per Windows. Se è necessario uno strumento da eseguire su sistemi operativi diversi da Windows, si consiglia Azure Data Studio. Azure Data Studio è uno strumento multipiattaforma che può essere eseguito su macOS, Linux e Windows. Per informazioni dettagliate, vedere [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md).

[!INCLUDE[get-help-sql-tools](../includes/paragraph-content/get-help-sql-tools.md)]

## <a name="next-steps"></a>Passaggi successivi

- [Strumenti SQL](../tools/overview-sql-tools.md)
- [Documentazione di SQL Server Management Studio](sql-server-management-studio-ssms.md)
- [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md)
- [Scaricare la versione più recente di SQL Server Data Tools](../ssdt/download-sql-server-data-tools-ssdt.md)
- [Aggiornamenti più recenti](../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)
- [Guida all'architettura dei dati di Azure](/azure/architecture/data-guide/)
- [Blog di SQL Server](https://cloudblogs.microsoft.com/sqlserver/)

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
