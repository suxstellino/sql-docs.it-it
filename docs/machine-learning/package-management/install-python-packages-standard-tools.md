---
title: Installare i pacchetti con gli strumenti Python
description: Informazioni su come usare gli strumenti Python standard per installare nuovi pacchetti Python in un'istanza di Machine Learning Services di SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 01/21/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2017
ms.openlocfilehash: 37d7f95f4140e0cab778ac4864757fbe75b1ff0c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082632"
---
# <a name="install-packages-with-python-tools-on-sql-server"></a>Installare i pacchetti con gli strumenti Python in SQL Server
[!INCLUDE [SQL Server 2017 only](../../includes/applies-to-version/sqlserver2017-only.md)]

Questo articolo descrive come usare gli strumenti Python standard per installare nuovi pacchetti Python in un'istanza di Machine Learning Services di SQL Server. In linea generale il processo di installazione di nuovi pacchetti è simile al processo eseguito in un ambiente Python standard. Sono tuttavia necessari alcuni passaggi aggiuntivi se il server non ha una connessione Internet.

Per altre informazioni sulla posizione dei pacchetti e sui percorsi di installazione, vedere [Ottenere informazioni sui pacchetti Python](python-package-information.md).

## <a name="prerequisites"></a>Prerequisites

+ È necessario che sia installato [Machine Learning Services per SQL Server](../install/sql-machine-learning-services-windows-install.md) con l'opzione relativa al linguaggio Python.

### <a name="other-considerations"></a>Altre considerazioni

+ I pacchetti devono essere conformi a Python 3.5 ed essere eseguiti in Windows.

+ La libreria di pacchetti Python si trova nella cartella Programmi dell'istanza di SQL Server e, per impostazione predefinita, è necessario disporre delle autorizzazioni di amministratore per eseguire installazioni in questa cartella. Per altre informazioni, vedere [Percorso della libreria dei pacchetti](../package-management/python-package-information.md#default-python-library-location).

+ L'installazione di un pacchetto viene eseguita a livello di istanza. Se si hanno più istanze di Machine Learning Services, è necessario aggiungere il pacchetto a ciascuna di esse.

+ I server di database sono spesso bloccati. In molti casi l'accesso a Internet è completamente bloccato. Per i pacchetti con un lungo elenco di dipendenze, è necessario identificare le dipendenze in anticipo e prepararsi all'installazione manuale di ogni pacchetto.

+ Prima di aggiungere un pacchetto, valutare se è adatto all'ambiente di SQL Server.

  + È consigliabile usare Python all'interno nel database per le attività che traggono vantaggio da una stretta integrazione con il motore di database, ad esempio Machine Learning, piuttosto che per le attività che eseguono semplicemente query sul database.

  + Se si aggiungono pacchetti che richiedono un utilizzo elevato delle risorse di calcolo del server, possono verificarsi problemi di prestazioni.

  + In un ambiente di SQL Server con protezione avanzata, è consigliabile evitare i tipi di pacchetti seguenti:
    + Pacchetti che richiedono l'accesso alla rete
    + Pacchetti che richiedono l'accesso al file system con privilegi elevati
    + Pacchetti usati per lo sviluppo Web o altre attività che non traggono vantaggio dall'esecuzione all'interno di SQL Server

## <a name="add-a-python-package-on-sql-server"></a>Aggiungere un pacchetto Python in SQL Server

Per installare un nuovo pacchetto Python che può essere usato in uno script in SQL Server, è necessario installare il pacchetto nell'istanza di Machine Learning Services. Se si hanno più istanze di Machine Learning Services, è necessario aggiungere il pacchetto a ciascuna di esse.

Il pacchetto installato negli esempi seguenti è [CNTK](/cognitive-toolkit/). Si tratta di un framework di Deep Learning offerto da Microsoft che supporta la personalizzazione, il training e la condivisione di diversi tipi di reti neurali.

### <a name="for-offline-install-download-the-python-package"></a>Per eseguire l'installazione offline, scaricare il pacchetto Python

Se si installano pacchetti Python in un server che non ha accesso a Internet, è necessario scaricare il file WHL da un computer con accesso a Internet, quindi copiare il file nel server.

Ad esempio, in un computer connesso a Internet è possibile scaricare il file `cntk-2.1-cp35-cp35m-win_amd64.whl` dal sito [https://cntk.ai/PythonWheel/CPU-Only](https://cntk.ai/PythonWheel/CPU-Only/cntk-2.1-cp35-cp35m-win_amd64.whl), quindi copiare il file in una cartella locale nel computer con SQL Server.

> [!IMPORTANT]
> Assicurarsi di ottenere la versione Windows del pacchetto. Se il file termina con l'estensione gz, la versione non è probabilmente corretta.

Per altre informazioni sui download del framework CNTK per più piattaforme e più versioni di Python, vedere [Installazione di CNTK nel computer](/cognitive-toolkit/Setup-CNTK-on-your-machine).

### <a name="locate-the-python-library"></a>Individuare la libreria Python

Individuare il percorso predefinito della libreria Python usato da SQL Server. Se sono state installate più istanze, individuare la cartella `PYTHON_SERVICES` per l'istanza in cui si vuole aggiungere il pacchetto.

Se ad esempio Machine Learning Services è stato installato usando le impostazioni predefinite e Machine Learning è stato abilitato nell'istanza predefinita, il percorso sarà il seguente:

```console
cd "C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES"
```

> [!TIP]
> Per il debug e il test futuri, potrebbe essere necessario configurare un ambiente Python specifico per la libreria dell'istanza.

### <a name="install-the-package-using-pip"></a>Installare il pacchetto usando pip

Usare il programma di installazione **pip** per installare nuovi pacchetti. È possibile trovare `pip.exe` nella sottocartella `Scripts` della cartella `PYTHON_SERVICES`. L'installazione di SQL Server non aggiunge la sottocartella `Scripts` al percorso di sistema. È quindi necessario specificare il percorso completo oppure è possibile aggiungere la cartella Scripts alla variabile PATH in Windows.

> [!NOTE]
> Se si usa Visual Studio 2017 o Visual Studio 2015 con le estensioni Python, è possibile eseguire `pip install` dalla finestra **Ambienti Python**. Fare clic su **Pacchetti**, nella casella di testo specificare il nome o il percorso del pacchetto da installare. Non è necessario digitare `pip install`. Viene compilato automaticamente.

+ Se il computer ha accesso a Internet, specificare il nome del pacchetto:

  ```console
  scripts\pip.exe install cntk
  ```
  È anche possibile specificare l'URL di un pacchetto e una versione specifici, ad esempio:

  ```console
  scripts\pip.exe install https://cntk.ai/PythonWheel/CPU-Only/cntk-2.1-cp35-cp35m-win_amd64.whl
  ```

+ Se il computer non ha accesso a Internet, specificare il file WHL scaricato in precedenza. Ad esempio:

  ```console
  scripts\pip.exe install C:\Downloads\cntk-2.1-cp35-cp35m-win_amd64.whl
  ```

Potrebbe essere richiesto di elevare le autorizzazioni per completare l'installazione.
Con l'avanzamento dell'installazione, è possibile visualizzare i messaggi di stato nella finestra del prompt dei comandi.

### <a name="load-the-package-or-its-functions-as-part-of-your-script"></a>Caricare il pacchetto o le relative funzioni come parte dello script

Al termine dell'installazione, è possibile iniziare immediatamente a usare il pacchetto negli script Python in SQL Server.

Per usare le funzioni del pacchetto nello script, inserire l'istruzione `import <package_name>` standard nelle righe iniziali dello script:

```sql
EXECUTE sp_execute_external_script 
  @language = N'Python', 
  @script = N'
import cntk
# Python statements ...
'
```

## <a name="see-also"></a>Vedere anche

+ [Ottenere informazioni sui pacchetti Python](python-package-information.md)
+ [Esercitazioni di Python per Machine Learning Services per SQL Server](../tutorials/python-tutorials.md)
+ [Python API for CNTK](https://cntk.ai/pythondocs/tutorials.html). (API Python per CNTK)