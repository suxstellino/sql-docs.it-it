---
title: Estrarre, trasformare e caricare i dati in Linux con SSIS
description: Informazioni su come eseguire pacchetti di SQL Server Integration Services (SSIS) in Linux. Informazioni anche su dove trovare altre informazioni sulle funzionalità di SSIS.
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 01/09/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: e21864da00069c6a41138a8506c98ab49e07bf3b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339125"
---
# <a name="extract-transform-and-load-data-on-linux-with-ssis"></a>Estrarre, trasformare e caricare i dati in Linux con SSIS

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo descrive come eseguire pacchetti di SQL Server Integration Services (SSIS) in Linux. SSIS risolve problemi di integrazione dei dati complessi estraendo i dati da più origini e formati, trasformando e pulendo i dati e caricando i dati in più destinazioni. 

I pacchetti SSIS in esecuzione in Linux possono connettersi a Microsoft SQL Server in esecuzione in Windows in locale o nel cloud, in Linux oppure in Docker. Possono anche connettersi a database SQL di Azure, Azure Synapse Analytics, origini dati ODBC, file flat e altre origini dati, tra cui origini ADO.NET, file XML e servizi OData.

Per altre informazioni sulle funzionalità di SSIS, vedere [SQL Server Integration Services](../integration-services/sql-server-integration-services.md).

## <a name="prerequisites"></a>Prerequisiti

Per eseguire i pacchetti SSIS in un computer Linux, è prima di tutto necessario installare SQL Server Integration Services. SSIS non è incluso nell'installazione di SQL Server per computer Linux. Per le istruzioni di installazione, vedere [Installare SQL Server Integration Services](sql-server-linux-setup-ssis.md).

È necessario anche un computer Windows per creare e gestire i pacchetti. Gli strumenti di progettazione e gestione di SSIS sono applicazioni Windows che attualmente non sono disponibili per i computer Linux. 

## <a name="run-an-ssis-package"></a>Eseguire un pacchetto SSIS

Per eseguire un pacchetto SSIS in un computer Linux, seguire questa procedura:

1.  Copiare il pacchetto SSIS nel computer Linux.
2.  Eseguire il comando seguente:
    ```
    $ dtexec /F \<package name \> /DE <protection password>
    ```

## <a name="run-an-encrypted-password-protected-package"></a>Eseguire un pacchetto crittografato (protetto da password)
Esistono tre modi per eseguire un pacchetto SSIS crittografato con una password:

1.  Impostare il valore della variabile di ambiente `SSIS_PACKAGE_DECRYPT`, come illustrato nell'esempio seguente:

    ```
    SSIS_PACKAGE_DECRYPT=test /opt/ssis/bin/dtexec /f package.dtsx
    ```

2.  Specificare l'opzione `/de[crypt]` per immettere la password in modo interattivo, come illustrato nell'esempio seguente:

    ```
    /opt/ssis/bin/dtexec /f package.dtsx /de
    
    Enter decryption password:
    ```

3.  Specificare l'opzione `/de` per immettere la password nella riga di comando, come illustrato nell'esempio seguente. Questo metodo non è consigliato perché archivia la password di decrittografia con il comando nella cronologia dei comandi.

    ```
    opt/ssis/bin/dtexec /f package.dtsx /de test
    
    Warning: Using /De[crypt] <password> may store decryption password in command history.
    
    You can use /De[crypt] instead to enter interactive mode,
    or use environment variable SSIS_PACKAGE_DECRYPT to set decryption password.
    ```

## <a name="design-packages"></a>Progettare pacchetti

**Connettersi alle origini dati ODBC**. Con SSIS in Linux CTP 2.1 Refresh e versioni successive, i pacchetti SSIS possono usare le connessioni ODBC in Linux. Questa funzionalità è stata testata con i driver ODBC di SQL Server e MySQL, ma è previsto che funzioni anche con i driver ODBC Unicode che osservano la specifica ODBC. In fase di progettazione è possibile specificare un DSN o una stringa di connessione per la connessione ai dati ODBC. È anche possibile usare l'autenticazione di Windows. Per altre informazioni, vedere il [post di blog che annuncia il supporto ODBC in Linux](https://blogs.msdn.microsoft.com/ssis/2017/06/16/odbc-is-supported-in-ssis-on-linux-ssis-helsinki-ctp2-1-refresh/).

**Percorsi**. Fornire percorsi di tipo Windows nei pacchetti SSIS. SSIS in Linux non supporta i percorsi di tipo Linux, ma esegue il mapping dei percorsi di tipo Windows ai percorsi di tipo Linux in fase di esecuzione, quindi SSIS in Linux, ad esempio, esegue il mapping del percorso di tipo Windows `C:\test` al percorso di tipo Linux `/test`.

## <a name="deploy-packages"></a>Distribuire i pacchetti
In questa versione è possibile archiviare solo i pacchetti nel file system in Linux. Il database del catalogo SSIS e il servizio SSIS legacy non sono disponibili in Linux per la distribuzione e l'archiviazione dei pacchetti.

## <a name="schedule-packages"></a>Pianificare i pacchetti
È possibile usare gli strumenti di pianificazione di sistema Linux, ad esempio `cron`, per definire la pianificazione dei pacchetti. Non è possibile usare SQL Agent in Linux per pianificare l'esecuzione di pacchetti in questa versione. Per altre informazioni, vedere [Definire la pianificazione per i pacchetti SSIS in Linux con cron](sql-server-linux-schedule-ssis-packages.md).

## <a name="limitations-and-known-issues"></a>Limitazioni e problemi noti

Per informazioni dettagliate sulle limitazioni e sui problemi noti di SSIS in Linux, vedere [Limitazioni e problemi noti di SSIS in Linux](sql-server-linux-ssis-known-issues.md).

## <a name="more-info-about-ssis-on-linux"></a>Altre informazioni su SSIS in Linux

Per altre informazioni su SSIS in Linux, vedere i post di blog seguenti:

-   [SSIS on Linux is available in SQL Server CTP2.1](https://blogs.msdn.microsoft.com/ssis/2017/05/17/ssis-helsinki-is-available-in-sql-server-vnext-ctp2-1/) (SSIS in Linux è disponibile in SQL Server CTP2.1)
-   [ODBC is supported in SSIS on Linux (SQL Server CTP 2.1 refresh)](https://blogs.msdn.microsoft.com/ssis/2017/06/16/odbc-is-supported-in-ssis-on-linux-ssis-helsinki-ctp2-1-refresh/) (ODBC è supportato in SSIS in Linux - SQL Server CTP 2.1 Refresh)

## <a name="more-info-about-ssis"></a>Altre informazioni su SSIS

Microsoft SQL Server Integration Services (SSIS) è una piattaforma per la compilazione di soluzioni di integrazione dei dati dalle prestazioni elevate, in cui sono incluse funzionalità per l'estrazione, la trasformazione e il caricamento (ETL) di pacchetti per il data warehousing. Per altre informazioni su SSIS, vedere [SQL Server Integration Services](../integration-services/sql-server-integration-services.md).

SSIS include le funzionalità seguenti:
- Strumenti grafici e procedure guidate per la compilazione e il debug di pacchetti in Windows
- Varie attività per l'esecuzione di funzioni del flusso di lavoro, ad esempio operazioni FTP, esecuzione di istruzioni SQL e invio di messaggi di posta elettronica
- Varie origini e destinazioni dati per l'estrazione e il caricamento dei dati
- Varie trasformazioni per la pulizia, l'aggregazione, l'unione e la copia dei dati
- API (Application Programming Interface) per l'estensione di SSIS con componenti e script personalizzati

Per iniziare a usare SSIS, scaricare la versione più recente di [SQL Server Data Tools (SSDT)](../integration-services/ssis-how-to-create-an-etl-package.md).

Per altre informazioni su SSIS, vedere gli articoli seguenti:
- [Altre informazioni su SQL Server Integration Services](../integration-services/sql-server-integration-services.md)
- [Strumenti di gestione e sviluppo di SQL Server Integration Services (SSIS)](../integration-services/integration-services-ssis-development-and-management-tools.md)
- [Esercitazioni su SQL Server Integration Services](../integration-services/integration-services-tutorials.md)

## <a name="related-content-about-ssis-on-linux"></a>Contenuto correlato su SSIS in Linux
-   [Installare SQL Server Integration Services (SSIS) in Linux](sql-server-linux-setup-ssis.md)
-   [Configurare SQL Server Integration Services in Linux con ssis-conf](sql-server-linux-configure-ssis.md)
-   [Limitazioni e problemi noti di SSIS in Linux](sql-server-linux-ssis-known-issues.md)
-   [Pianificare l'esecuzione del pacchetto SQL Server Integration Services in Linux con cron](sql-server-linux-schedule-ssis-packages.md)