---
title: Scaricare e installare sqlpackage
description: Scaricare e installare sqlpackage per Windows, macOS o Linux
ms.custom: tools|sos
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 10/02/2020
ms.openlocfilehash: d664c9b5d87e8a9358fc63418f3223a062e5d7bf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061036"
---
# <a name="download-and-install-sqlpackage"></a>Scaricare e installare sqlpackage

sqlpackage viene eseguito in Windows, macOS e Linux.

Scaricare e installare la versione più recente per .NET Framework e le anteprime per macOS e Linux:

|Piattaforma|Download|Data di rilascio|Versione|Compilare
|:---|:---|:---|:---|:---|
|[Windows](#get-sqlpackage-for-windows)|[Programma di installazione MSI](https://go.microsoft.com/fwlink/?linkid=2143544)|18 settembre 2020| 18.6 | 15.0.4897.1 |
|[macOS .NET Core](#get-sqlpackage-net-core-for-macos) |[.zip file](https://go.microsoft.com/fwlink/?linkid=2143659)|18 settembre 2020| 18.6| 15.0.4897.1 |
|[Linux .NET Core](#get-sqlpackage-net-core-for-linux) |[.zip file](https://go.microsoft.com/fwlink/?linkid=2143497)|18 settembre 2020| 18.6| 15.0.4897.1 |
|[Windows .NET Core](#get-sqlpackage-net-core-for-windows) |[.zip file](https://go.microsoft.com/fwlink/?linkid=2143496)|18 settembre 2020| 18.6| 15.0.4897.1 |

Per informazioni dettagliate sulla versione più recente, vedere le [note sulla versione](release-notes-sqlpackage.md). Per scaricare altre lingue, vedere la sezione [Lingue disponibili](#available-languages).


È disponibile un collegamento Evergreen ( [https://aka.ms/sqlpackage-linux](https://aka.ms/sqlpackage-linux) ) che punta alla versione corrente di SqlPackage per [Linux .NET Core](#get-sqlpackage-net-core-for-linux), che può essere usata in automazione di ambienti con la versione più recente di SqlPackage.

## <a name="dacfx"></a>DacFx
SqlPackage è un'interfaccia della riga di comando per il framework DacFx, che espone alcune delle API DacFx pubbliche. DacServices ([Microsoft.SqlServer.Dac](/dotnet/api/microsoft.sqlserver.dac.dacservices)) è un meccanismo correlato per l'integrazione della distribuzione di database nella pipeline dell'applicazione.  L'API DacServices è disponibile in un pacchetto tramite NuGet, [Microsoft.SqlServer.DACFx](https://www.NuGet.org/packages/Microsoft.SqlServer.DACFx).  La versione corrente di DacFx è 150.4897.1.

L'installazione del pacchetto NuGet tramite l'interfaccia della riga di comando di .NET viene eseguita con questo comando:

```cmd
dotnet add package Microsoft.SqlServer.DACFx
```

>[!NOTE]
> Sono stati pubblicati altri pacchetti NuGet con il nome DacFx: "Microsoft.SqlServer.DacFx.x64" e "Microsoft.SqlServer.DacFx.x86". Il supporto per entrambe le piattaforme è disponibile tramite il pacchetto "Microsoft.SqlServer.DACFx". I nuovi riferimenti devono essere creati per questo pacchetto e non per le varianti x64 o x86.

## <a name="get-sqlpackage-for-windows"></a>Ottenere sqlpackage per Windows

Questa versione di sqlpackage include un'esperienza di installazione Windows standard e un file ZIP: 

1. Scaricare ed eseguire il [programma di installazione DacFramework.msi per Windows](https://go.microsoft.com/fwlink/?linkid=2143544).
2. Aprire una nuova finestra del prompt dei comandi ed eseguire sqlpackage.exe
    - sqlpackage viene installato nella cartella ```C:\Program Files\Microsoft SQL Server\150\DAC\bin```

## <a name="get-sqlpackage-net-core-for-windows"></a>Ottenere sqlpackage .NET Core per Windows

1. Scaricare [sqlpackage per Windows](https://go.microsoft.com/fwlink/?linkid=2143496).
2. Per estrarre il file, fare clic con il pulsante destro del mouse sul file in Esplora risorse, scegliere "Estrai tutto" e selezionare la directory di destinazione.
3. Aprire una nuova finestra del terminale ed eseguire CD per passare alla posizione in cui è stato estratto sqlpackage:

   ```cmd
   > sqlpackage
   ```

## <a name="get-sqlpackage-net-core-for-macos"></a>Ottenere sqlpackage .NET Core per macOS

1. Scaricare [sqlpackage per macOS](https://go.microsoft.com/fwlink/?linkid=2143659).
2. Per estrarre il file e avviare sqlpackage, aprire una nuova finestra del terminale e digitare i comandi seguenti:

   ```bash
   $ mkdir sqlpackage
   $ unzip ~/Downloads/sqlpackage-osx-<version string>.zip -d ~/sqlpackage 
   $ echo 'export PATH="$PATH:~/sqlpackage"' >> ~/.bash_profile
   $ source ~/.bash_profile
   $ sqlpackage
   ```

   > [!NOTE]
   > Le impostazioni di sicurezza possono richiedere una modifica per eseguire sqlpackage in macOS. Usare i comandi seguenti per interagire con Gatekeeper dalla riga di comando.

   **Prima dell'esecuzione di sqlpackage:**
   ```bash
   $ sudo spctl --master-disable
   ```

   **Dopo l'esecuzione di sqlpackage:**
   ```bash
   $ sudo spctl --master-enable
   ```

## <a name="get-sqlpackage-net-core-for-linux"></a>Ottenere sqlpackage .NET Core per Linux

1. Scaricare [SqlPackage per Linux](https://go.microsoft.com/fwlink/?linkid=2143497) usando uno dei programmi di installazione o l'archivio tar. gz.
2. Per estrarre il file e avviare sqlpackage, aprire una nuova finestra del terminale e digitare i comandi seguenti:

   ```bash
   $ cd ~
   $ mkdir sqlpackage
   $ unzip ~/Downloads/sqlpackage-linux-<version string>.zip -d ~/sqlpackage 
   $ echo "export PATH=\"\$PATH:$HOME/sqlpackage\"" >> ~/.bashrc
   $ chmod a+x ~/sqlpackage/sqlpackage
   $ source ~/.bashrc
   $ sqlpackage
   ```

   > [!NOTE]
   > In Debian, Red Hat e Ubuntu potrebbero mancare alcune dipendenze. Usare i comandi seguenti per installare queste dipendenze a seconda della versione di Linux:

   **Debian:**

   ```bash
   $ sudo apt-get install libunwind8
   ```

   **RedHat:**

   ```bash
   $ yum install libunwind
   $ yum install libicu
   ```

   **Ubuntu:**

   ```bash
   $ sudo apt-get install libunwind8

   # install the libicu library based on the Ubuntu version
   $ sudo apt-get install libicu52      # for 14.x
   $ sudo apt-get install libicu55      # for 16.x
   $ sudo apt-get install libicu57      # for 17.x
   $ sudo apt-get install libicu60      # for 18.x
   ```

## <a name="uninstall-sqlpackage"></a>Disinstallare sqlpackage

Se sqlpackage è stato installato usando il programma di installazione di Windows, disinstallarlo così come si rimuove qualsiasi applicazione Windows.

Se sqlpackage è stato installato con un file con estensione zip o con un altro archivio, eliminare i file.

## <a name="supported-operating-systems"></a>Sistemi operativi supportati

sqlpackage può essere eseguito in Windows, macOS e Linux ed è compilato con .NET Core 3.1.  I [requisiti del sistema operativo per .NET Core 3.1](https://github.com/dotnet/core/blob/master/release-notes/3.1/3.1-supported-os.md) si applicano a sqlpackage.

### <a name="windows-x64"></a>Windows (x64)

- Windows 10 (1607 e successive)
- Windows 8.1
- Windows 7 SP1
- Componenti di base di Windows Server
- Windows Server 2012 R2
- Windows Server 2016
- Windows Server 2019

### <a name="macos"></a>macOS

- macOS 10.15 "Catalina"
- macOS 10.14 "Mojave"
- macOS 10.13 "High Sierra"

### <a name="linux-x64"></a>Linux (x64)

- Red Hat Enterprise Linux 7 e successive
- SUSE Linux Enterprise Server v12 SP2 e successive
- Ubuntu 16.04 e successive

## <a name="available-languages"></a>Lingue disponibili

Questa versione di sqlpackage può essere installata nelle lingue seguenti:

sqlpackage Windows:  
[Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x40a)

sqlpackage .NET Core Windows:  
[Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x40a)

sqlpackage .NET Core macOS:  
[Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x40a)

sqlpackage .NET Core Linux:  
[Cinese (semplificato)](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x804) | [Cinese (tradizionale)](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x404) | [Inglese (Stati Uniti)](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x409) | [Francese](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x40c) | [Tedesco](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x410) | [Giapponese](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x412) | [Portoghese (Brasile)](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x416) | [Russo](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x419) | [Spagnolo](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x40a)


## <a name="next-steps"></a>Passaggi successivi

- Altre informazioni su [sqlpackage](sqlpackage.md)

[Informativa sulla privacy di Microsoft](https://go.microsoft.com/fwlink/?LinkId=521839)