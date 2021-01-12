---
description: Feature Pack di Integration Services (SSIS) per Azure
title: Feature Pack di Integration Services (SSIS) per Azure | Microsoft Docs
ms.custom: ''
ms.date: 12/24/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.SSIS.AZURE.F1
- SQL14.SSIS.AZURE.F1
ms.assetid: 31de555f-ae62-4f2f-a6a6-77fea1fa8189
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b22910f0a794925d07358fa73f3bd6d7a7c6e15a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091661"
---
# <a name="azure-feature-pack-for-integration-services-ssis"></a>Feature Pack di Integration Services (SSIS) per Azure

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


Il Feature Pack di SQL Server Integration Services (SSIS) per Azure è un'estensione che fornisce i componenti elencati in questa pagina per consentire a SSIS di connettersi ai servizi Azure, trasferire i dati tra Azure e le origini dati locali ed elaborare i dati archiviati in Azure.

[![Scaricare il Feature Pack di SSIS per Azure](/analysis-services/analysis-services/media/download.png)](https://www.microsoft.com/download/details.aspx?id=100430) **Scarica**

- Per SQL Server 2019 - [Microsoft SQL Server 2019 Integration Services Feature Pack per Azure](https://www.microsoft.com/download/details.aspx?id=100430)
- Per SQL Server 2017 - [Microsoft SQL Server 2017 Integration Services Feature Pack per Azure](https://www.microsoft.com/download/details.aspx?id=54798)
- Per SQL Server 2016 - [Microsoft SQL Server 2016 Integration Services Feature Pack per Azure](https://www.microsoft.com/download/details.aspx?id=49492)
- Per SQL Server 2014 - [Microsoft SQL Server 2014 Integration Services Feature Pack per Azure](https://www.microsoft.com/download/details.aspx?id=47366)
- Per SQL Server 2012 - [Microsoft SQL Server 2012 Integration Services Feature Pack per Azure](https://www.microsoft.com/download/details.aspx?id=47367)

Le pagine di download includono anche informazioni sui prerequisiti. Assicurarsi di installare SQL Server prima di installare il Feature Pack di Azure in un server. In caso contrario i componenti del Feature Pack potrebbero non essere disponibili quando si distribuiscono pacchetti al database del catalogo SSIS (SSISDB) nel server.

## <a name="components-in-the-feature-pack"></a>Componenti del Feature Pack
-   Gestioni connessioni

    -   [Gestione connessioni di Azure Data Lake Analytics](connection-manager/azure-data-lake-analytics-connection-manager.md)

    -   [Gestione connessioni di Azure Data Lake Store](../integration-services/connection-manager/azure-data-lake-store-connection-manager.md)
    
    -   [Gestione connessione Azure HDInsight](../integration-services/connection-manager/azure-hdinsight-connection-manager.md)

    -   [Gestione connessione Azure Resource Manager](../integration-services/connection-manager/azure-resource-manager-connection-manager.md)
    
    -   [Gestione connessione dell'archiviazione di Azure](../integration-services/connection-manager/azure-storage-connection-manager.md)

    -   [Gestione connessione della sottoscrizione di Azure](../integration-services/connection-manager/azure-subscription-connection-manager.md)
    
-   Attività

    -   [Attività di download BLOB di Azure](../integration-services/control-flow/azure-blob-download-task.md)

    -   [Attività di caricamento BLOB di Azure](../integration-services/control-flow/azure-blob-upload-task.md)

    -   [Attività Azure Data Lake Analytics](control-flow/azure-data-lake-analytics-task.md)

    -   [Attività File system di Azure Data Lake Store](../integration-services/control-flow/azure-data-lake-store-file-system-task.md)

    -   [Attività di creazione cluster di Azure HDInsight](../integration-services/control-flow/azure-hdinsight-create-cluster-task.md)

    -   [Attività di eliminazione cluster di Azure HDInsight](../integration-services/control-flow/azure-hdinsight-delete-cluster-task.md)
    
    -   [Attività Hive di Azure HDInsight](../integration-services/control-flow/azure-hdinsight-hive-task.md)

    -   [Attività Pig di Azure HDInsight](../integration-services/control-flow/azure-hdinsight-pig-task.md)

    -   [Attività di caricamento di Azure SQL DW](../integration-services/control-flow/azure-sql-dw-upload-task.md)

    -   [Attività File flessibili](../integration-services/control-flow/flexible-file-task.md)

-   Componenti del flusso di dati

    -   [Origine BLOB di Azure](../integration-services/data-flow/azure-blob-source.md)

    -   [Destinazione BLOB di Azure](../integration-services/data-flow/azure-blob-destination.md)
    
    -   [Origine di Azure Data Lake Store](../integration-services/data-flow/azure-data-lake-store-source.md)
    
    -   [Destinazione di Azure Data Lake Store](../integration-services/data-flow/azure-data-lake-store-destination.md)

    -   [Origine di File flessibili](../integration-services/data-flow/flexible-file-source.md)

    -   [Destinazione di File flessibili](../integration-services/data-flow/flexible-file-destination.md)

-   Blob di Azure, Azure Data Lake Store ed Enumeratore file di Data Lake Store Gen2. Vedere [Contenitore Ciclo Foreach](../integration-services/control-flow/foreach-loop-container.md).

## <a name="use-tls-12"></a>Usare TLS 1.2

La versione di TLS usata dal Feature Pack di Azure dipende dalle impostazioni di sistema di .NET Framework.
Per usare TLS 1.2, aggiungere un valore `REG_DWORD` denominato `SchUseStrongCrypto` con dati `1` nelle due chiavi del Registro di sistema seguenti.

1. `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319`
2. `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319`

## <a name="dependency-on-java"></a>Dipendenza da Java

Java è necessario per usare i formati di file ORC/Parquet con i connettori di File flessibili/Azure Data Lake Store.  
L'architettura (a 32 o a 64 bit) della build di Java deve corrispondere a quella del runtime di SSIS da usare.
Sono state sottoposte a test le build di Java seguenti.

- [OpenJDK 8u192 di Zulu](https://www.azul.com/downloads/zulu/zulu-windows/)
- [Oracle Java SE Runtime Environment 8u192](https://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8-2177648.html)

### <a name="set-up-zulus-openjdk"></a>Configurare OpenJDK di Zulu

1. Scaricare ed estrarre il pacchetto di installazione con estensione zip.
2. Dal prompt dei comandi, eseguire `sysdm.cpl`.
3. Nella scheda **Avanzate** selezionare **Variabili di ambiente**.
4. Nella sezione **Variabili di sistema** selezionare **Nuova**.
5. Immettere `JAVA_HOME` in **Nome variabile**.
6. Selezionare **Sfoglia directory**, passare alla cartella estratta e selezionare la sottocartella `jre`.
   Selezionare quindi **OK** e **Valore variabile** verrà popolato automaticamente.
7. Selezionare **OK** per chiudere la finestra di dialogo **Nuova variabile di sistema**.
8. Selezionare **OK** per chiudere la finestra di dialogo **Variabili di ambiente**.
9. Selezionare **OK** per chiudere la finestra di dialogo **Proprietà del sistema**.

> [!TIP]
> Se si usa il formato Parquet e si verifica l'errore "An error occurred when invoking java, message: **java.lang.OutOfMemoryError:Java heap space**" (Errore durante la chiamata di Java, messaggio: java.lang.OutOfMemoryError: spazio dell'heap di Java), è possibile aggiungere una variabile di ambiente *`_JAVA_OPTIONS`* per modificare le dimensioni dell'heap min/max per JVM.
>
>![heap JVM](media/azure-feature-pack-jvm-heap-size.png)
>
> Esempio: impostare la variabile *`_JAVA_OPTIONS`* con il valore *`-Xms256m -Xmx16g`* . Il flag Xms specifica il pool di allocazione della memoria iniziale per Java Virtual Machine (JVM), mentre Xmx specifica il pool di allocazione della memoria massima. JVM verrà quindi avviato con una quantità di memoria pari a *`Xms`* e potrà usare una quantità massima di memoria pari a *`Xmx`* . Per impostazione predefinita, viene usata una quantità di memoria minima pari a 64 MB e una quantità di memoria massima pari a 1 GB.

### <a name="set-up-zulus-openjdk-on-azure-ssis-integration-runtime"></a>Configurare OpenJDK di Zulu in Azure-SSIS Integration Runtime

Questa operazione deve essere eseguita tramite l'[interfaccia di configurazione personalizzata](/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup) per Azure-SSIS Integration Runtime.
Si supponga di usare `zulu8.33.0.1-jdk8.0.192-win_x64.zip`.
Il contenitore BLOB può essere organizzato come indicato di seguito.

~~~
main.cmd
install_openjdk.ps1
zulu8.33.0.1-jdk8.0.192-win_x64.zip
~~~

Come punto di ingresso, `main.cmd` attiva l'esecuzione dello script di PowerShell `install_openjdk.ps1` che a sua volta estrae `zulu8.33.0.1-jdk8.0.192-win_x64.zip` e imposta `JAVA_HOME` di conseguenza.

**main.cmd**

~~~
powershell.exe -file install_openjdk.ps1
~~~

> [!TIP]
> Se si usa il formato Parquet e si verifica l'errore "An error occurred when invoking java, message: **java.lang.OutOfMemoryError:Java heap space**" (Errore durante la chiamata di Java, messaggio: java.lang.OutOfMemoryError: spazio dell'heap di Java), è possibile aggiungere comando in *`main.cmd`* per modificare le dimensioni dell'heap min/max per JVM. Esempio:
> ~~~
> setx /M _JAVA_OPTIONS "-Xms256m -Xmx16g"
> ~~~
> Il flag Xms specifica il pool di allocazione della memoria iniziale per Java Virtual Machine (JVM), mentre Xmx specifica il pool di allocazione della memoria massima. JVM verrà quindi avviato con una quantità di memoria pari a *`Xms`* e potrà usare una quantità massima di memoria pari a *`Xmx`* . Per impostazione predefinita, viene usata una quantità di memoria minima pari a 64 MB e una quantità di memoria massima pari a 1 GB.

**install_openjdk.ps1**

~~~
Expand-Archive zulu8.33.0.1-jdk8.0.192-win_x64.zip -DestinationPath C:\
[Environment]::SetEnvironmentVariable("JAVA_HOME", "C:\zulu8.33.0.1-jdk8.0.192-win_x64\jre", "Machine")
~~~

### <a name="set-up-oracles-java-se-runtime-environment"></a>Configurare Oracle Java SE Runtime Environment

1. Scaricare ed eseguire il programma di installazione con estensione exe.
2. Seguire le istruzioni del programma di installazione per completare l'installazione.

## <a name="scenario-processing-big-data"></a>Scenario: Elaborazione di Big Data
 Usare il connettore di Azure per completare l'elaborazione di Big Data:

1.  Usare l'attività di caricamento BLOB di Azure per caricare i dati di input nell'archivio BLOB di Azure.

2.  Usare l'attività di creazione cluster di Azure HDInsight per creare un cluster di Azure HDInsight. Questo passaggio è facoltativo se si vuole usare un cluster personale.

3.  Usare l'attività Hive o Pig di Azure HDInsight per richiamare un processo Pig o Hive nel cluster di Azure HDInsight.

4.  Usare l'attività di eliminazione cluster di Azure HDInsight per eliminare il cluster di HDInsight dopo l'uso se è stato creato un cluster di HDInsight su richiesta nel passaggio 2.

5.  Usare l'attività di download BLOB di Azure HDInsight per scaricare i dati di output Pig/Hive dall'archivio BLOB di Azure.

![Screenshot che mostra lo scenario di Big Data del connettore Azure per SSIS.](../integration-services/media/ssis-azureconnector-bigdatascenario.png)
 
## <a name="scenario-managing-data-in-the-cloud"></a>Scenario: Gestione di dati nel cloud
 Usare la destinazione BLOB di Azure in un pacchetto SSIS per scrivere i dati di output in un archivio BLOB di Azure oppure usare l'origine BLOB di Azure per leggere i dati da un archivio BLOB di Azure.

![Screenshot che mostra il flusso di dati dall'origine OLE DB alla destinazione BLOB di Azure.](../integration-services/media/ssis-azureconnector-cloudarchive-1.png)
 
 ![Screenshot che mostra il flusso di dati dall'origine BLOB di Azure alla destinazione OLE DB.](../integration-services/media/ssis-azureconnector-cloudarchive-2.png)

 Usare il contenitore Ciclo Foreach con l'enumeratore BLOB di Azure per elaborare i dati in più file BLOB.

![Screenshot che mostra il Contenitore Ciclo Foreach del flusso di controllo.](../integration-services/media/ssis-azureconnector-cloudarchive-3.png)

## <a name="release-notes"></a>Note sulla versione

### <a name="version-1190"></a>Versione 1.19.0

#### <a name="improvements"></a>Miglioramenti

1. Aggiunta del supporto per l'autenticazione della firma di accesso condiviso alla gestione connessione di Archiviazione di Azure.

### <a name="version-1180"></a>Versione 1.18.0

#### <a name="improvements"></a>Miglioramenti

1. Per le attività File flessibili sono stati introdotti tre miglioramenti: (1) è stato aggiunto il supporto dei caratteri jolly per le operazioni di copia/eliminazione. (2) l'utente può abilitare o disabilitare la ricerca ricorsiva per l'operazione di eliminazione; e (3) il nome file della destinazione per l'operazione di copia può essere vuoto per mantenere il nome del file di origine.

### <a name="version-1170"></a>Versione 1.17.0

Si tratta di una versione di hotfix rilasciata solo per SQL Server 2019.

#### <a name="bugfixes"></a>Correzioni di bug

1. Quando si esegue in Visual Studio 2019 con destinazione SQL Server 2019, l'attività dei file flessibili/origine/destinazione potrebbe non riuscire con il messaggio di errore `Attempted to access an element as a type incompatible with the array.`
1. Quando si esegue in Visual Studio 2019 con destinazione SQL Server 2019, origine/destinazione file flessibili con il formato ORC/Parquet potrebbe non riuscire con il messaggio di errore `Microsoft.DataTransfer.Common.Shared.HybridDeliveryException: An unknown error occurred. JNI.JavaExceptionCheckException.`

### <a name="version-1160"></a>Versione 1.16.0

#### <a name="bugfixes"></a>Correzioni di bug

1. In alcuni casi, durante l'esecuzione del pacchetto viene visualizzato il messaggio: "Errore: Non è stato possibile caricare il file o l'assembly 'Newtonsoft.Json, Version=11.0.0.0, Culture=neutral, PublicKeyToken=30ad4fe6b2a6aeed' o una delle relative dipendenze."

### <a name="version-1150"></a>Versione 1.15.0

#### <a name="improvements"></a>Miglioramenti

1. Aggiunta dell'operazione di eliminazione cartella/file ad Attività File flessibili
1. Aggiunta della funzione di conversione di un tipo di dati esterno/output in Origine di File flessibili

#### <a name="bugfixes"></a>Correzioni di bug

1. In alcuni casi, la connessione di test per Data Lake Storage Gen2 non funziona con il messaggio di errore "Tentativo di accedere a un elemento come tipo incompatibile con la matrice"
1. Ripristino del supporto per l'emulatore di archiviazione di Azure