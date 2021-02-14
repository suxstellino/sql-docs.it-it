---
title: Importare dati da Excel a SQL Server | Microsoft Docs
description: Questo articolo descrive i metodi per importare dati da Excel in SQL Server o nel database SQL di Azure. Alcuni usano un solo passaggio, altri richiedono un file di testo intermedio.
ms.custom: sqlfreshmay19
ms.date: 09/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 84fce410f5ee03301b5ad8e5fe0aa61698b52e0f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351346"
---
# <a name="import-data-from-excel-to-sql-server-or-azure-sql-database"></a>Importare dati da Excel a SQL Server o al database SQL di Azure

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Sono disponibili vari modi per importare dati da file di Excel a SQL Server o al database SQL di Azure. Alcuni metodi consentono di importare dati in un unico passaggio direttamente dai file di Excel. Altri metodi richiedono l'esportazione dei dati di Excel in formato testo (file CSV) prima di poterli importare. Questo articolo riepiloga i metodi usati di frequente e include collegamenti a informazioni più dettagliate.

## <a name="list-of-methods"></a>Elenco di metodi

Per importare dati da Excel, è possibile usare gli strumenti seguenti:

| Esportazione prima in formato testo (SQL Server e database SQL) | Direttamente da Excel (solo SQL Server locale) |
| :------------------------------------------------- |:------------------------------------------------- |
| [Procedura guidata Importa file flat](#import-wiz)             |[Importazione/Esportazione guidata SQL Server](#wiz)        |
| Istruzione [BULK INSERT](#bulk-insert)              |[SQL Server Integration Services (SSIS)](#ssis)    |
| [BCP](#bcp)                                        |Funzione [OPENROWSET](#openrowset) <br>            |
| [Copia guidata (Azure Data Factory)](#adf-wiz)       |                                                   |
| [Azure Data Factory](#adf)                         |                                                   |
| &nbsp; | &nbsp; |

Se si vogliono importare più fogli di lavoro da una cartella di lavoro di Excel, è generalmente necessario eseguire uno di questi strumenti una volta per ogni foglio.

Una descrizione completa degli strumenti e dei servizi complessi come Azure Data Factory o SSIS esula dagli scopi di questo elenco. Per altre informazioni sulla soluzione a cui si è interessati, seguire i collegamenti indicati.

> [!IMPORTANT]
> Per informazioni dettagliate sulla connessione ai file di Excel e sulle limitazioni e i problemi noti per il caricamento di dati da o a file di Excel, vedere [Caricare i dati da o in Excel con SQL Server Integration Services (SSIS)](../../integration-services/load-data-to-from-excel-with-ssis.md).

Se SQL Server non è installato o è installato senza SQL Server Management Studio, vedere [Scaricare SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).

## <a name="sql-server-import-and-export-wizard"></a><a name="wiz"></a> Importazione/Esportazione guidata SQL Server

È possibile importare i dati direttamente dai file di Excel scorrendo le pagine dell'Importazione/Esportazione guidata SQL Server. Facoltativamente, salvare le impostazioni come pacchetto di SQL Server Integration Services (SSIS) che è possibile personalizzare e riusare in seguito.

1. In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] connettersi a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)].

2. Espandere **Database**.
3. Fare clic con il pulsante destro del mouse su un database.
4. Scegliere **Attività**.
5. Fare clic su una delle opzioni seguenti.

  - **Importazione dei dati**
  - **Esportazione dei dati**

    ![Avviare la procedura guidata da SQL Server Management Studio](../../integration-services/import-export-data/media/start-wizard-ssms.jpg)

![Connettersi a un'origine dati Excel](media/excel-connection.png)

Per un esempio di come usare la procedura guidata per l'importazione da Excel a SQL Server, vedere [Get started with this simple example of the Import and Export Wizard](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md) (Iniziare con un semplice esempio dell'Importazione/Esportazione guidata).

Per informazioni su altri modi di avviare la procedura di importazione ed esportazione guidata, vedere [Avviare l'Importazione/Esportazione guidata SQL Server](../../integration-services/import-export-data/start-the-sql-server-import-and-export-wizard.md).

## <a name="sql-server-integration-services-ssis"></a><a name="ssis"></a> SQL Server Integration Services (SSIS)

Se si ha familiarità con SSIS e si preferisce non eseguire l'Importazione/Esportazione guidata di SQL Server, creare un pacchetto SSIS che usa Excel come origine e SQL Server come destinazione nel flusso di dati.

Per altre informazioni su questi componenti di SSIS, vedere gli argomenti seguenti:

- [Origine Excel](../../integration-services/data-flow/excel-source.md)
- [Destinazione SQL Server](../../integration-services/data-flow/sql-server-destination.md)

Per istruzioni su come creare pacchetti SSIS, vedere l'esercitazione [Creazione di un pacchetto ETL](../../integration-services/ssis-how-to-create-an-etl-package.md).

![Componenti del flusso di dati](media/excel-to-sql-data-flow.png)

## <a name="openrowset-and-linked-servers"></a><a name="openrowset"></a> OPENROWSET e server collegati

> [!IMPORTANT]
> Nel database SQL di Azure non è possibile eseguire l'importazione direttamente da Excel. È necessario prima esportare i dati in un file di testo (CSV). Per alcuni esempi, vedere [Esempi](import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).

> [!NOTE]
> Il provider ACE (in precedenza provider Jet) che si connette alle origini dati di Excel è destinato all'uso interattivo sul lato client. Se si usa il provider ACE in SQL Server, in particolare in processi automatizzati o processi in esecuzione in parallelo, si possono ottenere risultati imprevisti.

### <a name="distributed-queries"></a>Query distribuite

Importare i dati direttamente in SQL Server dai file di Excel usando la funzione `OPENROWSET` o `OPENDATASOURCE` di Transact-SQL. Questo utilizzo è noto come *query distribuita*.

> [!IMPORTANT]
> Nel database SQL di Azure non è possibile eseguire l'importazione direttamente da Excel. È necessario prima esportare i dati in un file di testo (CSV). Per alcuni esempi, vedere [Esempi](import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).

Prima di eseguire una query distribuita, è necessario abilitare l'opzione di configurazione del server `ad hoc distributed queries`, come illustrato nell'esempio seguente. Per altre informazioni, vedere [Opzione di configurazione del server ad hoc distributed queries](../../database-engine/configure-windows/ad-hoc-distributed-queries-server-configuration-option.md).

```sql
sp_configure 'show advanced options', 1;
RECONFIGURE;
GO
sp_configure 'Ad Hoc Distributed Queries', 1;
RECONFIGURE;
GO
```

L'esempio di codice seguente usa `OPENROWSET` per importare i dati dal foglio di lavoro di Excel `Sheet1` in una nuova tabella di database.

```sql
USE ImportFromExcel;
GO
SELECT * INTO Data_dq
FROM OPENROWSET('Microsoft.ACE.OLEDB.12.0',
    'Excel 12.0; Database=C:\Temp\Data.xlsx', [Sheet1$]);
GO
```

Ecco lo stesso esempio con `OPENDATASOURCE`.

```sql
USE ImportFromExcel;
GO
SELECT * INTO Data_dq
FROM OPENDATASOURCE('Microsoft.ACE.OLEDB.12.0',
    'Data Source=C:\Temp\Data.xlsx;Extended Properties=Excel 12.0')...[Sheet1$];
GO
```

Per *aggiungere* i dati importati a una tabella *esistente* invece di creare una nuova tabella, usare la sintassi `INSERT INTO ... SELECT ... FROM ...` al posto della sintassi `SELECT ... INTO ... FROM ...` usata negli esempi precedenti.

Per eseguire una query sui dati di Excel senza eseguirne l'importazione, usare la sintassi standard `SELECT ... FROM ...`.

Per altre informazioni sulle query distribuite, vedere gli argomenti seguenti:

- [Query distribuite](/previous-versions/sql/sql-server-2008-r2/ms188721(v=sql.105)) Le query distribuite sono ancora supportate in SQL Server 2016, ma la documentazione relativa a questa funzionalità non è stata aggiornata.
- [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md)
- [OPENDATASOURCE](../../t-sql/functions/openquery-transact-sql.md)

### <a name="linked-servers"></a>Server collegati

È anche possibile configurare una connessione permanente da SQL Server al file di Excel come *server collegato*. L'esempio seguente importa i dati dal foglio di lavoro `Data` nel server collegato di Excel esistente `EXCELLINK` in una nuova tabella di database di SQL Server denominata `Data_ls`.

```sql
USE ImportFromExcel;
GO
SELECT * INTO Data_ls FROM EXCELLINK...[Data$];
GO
```

È possibile creare un server collegato da SQL Server Management Studio o eseguendo la stored procedure di sistema `sp_addlinkedserver`, come illustrato nell'esempio seguente.

```sql
DECLARE @RC int

DECLARE @server     nvarchar(128)
DECLARE @srvproduct nvarchar(128)
DECLARE @provider   nvarchar(128)
DECLARE @datasrc    nvarchar(4000)
DECLARE @location   nvarchar(4000)
DECLARE @provstr    nvarchar(4000)
DECLARE @catalog    nvarchar(128)

-- Set parameter values
SET @server =     'EXCELLINK'
SET @srvproduct = 'Excel'
SET @provider =   'Microsoft.ACE.OLEDB.12.0'
SET @datasrc =    'C:\Temp\Data.xlsx'
SET @provstr =    'Excel 12.0'

EXEC @RC = [master].[dbo].[sp_addlinkedserver] @server, @srvproduct, @provider,
@datasrc, @location, @provstr, @catalog
```

Per altre informazioni sui server collegati, vedere gli argomenti seguenti:

- [Creare server collegati](../../relational-databases/linked-servers/create-linked-servers-sql-server-database-engine.md)
- [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md)

Per altri esempi e informazioni sia sui server collegati che sulle query distribuite, vedere gli argomenti seguenti:

- [How to use Excel with SQL Server linked servers and distributed queries](https://support.microsoft.com/help/306397/how-to-use-excel-with-sql-server-linked-servers-and-distributed-queries) (Come usare Excel con server collegati e query distribuite di SQL Server)
- [Come importare dati da Excel a SQL Server](https://support.microsoft.com/help/321686/how-to-import-data-from-excel-to-sql-server)

## <a name="prerequisite---save-excel-data-as-text"></a><a name="prereq"></a> Prerequisito: salvare i dati di Excel come testo

Per usare i restanti metodi descritti in questa pagina, ovvero l'istruzione BULK INSERT, lo strumento BCP o Azure Data Factory, è prima di tutto necessario esportare i dati di Excel in un file di testo.

In Excel selezionare **File | Salva con nome** e quindi selezionare **Testo (delimitato da tabulazioni) (\*.txt)** o **CSV (delimitato da virgole) (\*.csv)** come tipo di file di destinazione.

Se si vuole esportare più fogli di lavoro dalla cartella di lavoro, selezionare ogni foglio e ripetere questa procedura. Il comando **Salva con nome** esporta solo il foglio attivo.

> [!TIP]
> Per ottenere risultati ottimali con gli strumenti per l'importazione dei dati, salvare fogli che contengono solo le intestazioni di colonna e le righe di dati. Se i dati salvati contengono titoli di pagina, righe vuote, note e così via, possono verificarsi risultati imprevisti in un secondo momento quando si importano i dati.

## <a name="the-import-flat-file-wizard"></a><a name="import-wiz"></a> Procedura guidata Importa file flat

È possibile importare i dati salvati come file di testo seguendo le varie pagine della procedura guidata Importa file flat.

Come descritto in precedenza nella sezione [Prerequisito](#prereq), è necessario esportare i dati Excel come testo prima di poter usare la procedura guidata Importa file flat per eseguire l'importazione.

Per altre informazioni sulla procedura guidata Importa File Flat, vedere [Procedura guidata per l'importazione di file flat in SQL](import-flat-file-wizard.md).

## <a name="bulk-insert-command"></a><a name="bulk-insert"></a> Comando BULK INSERT

`BULK INSERT` è un comando Transact-SQL che è possibile eseguire da SQL Server Management Studio. L'esempio seguente carica i dati dal file con valori delimitati da virgole `Data.csv` in una tabella di database esistente.

Come descritto in precedenza nella sezione [Prerequisito](#prereq), è necessario esportare i dati Excel come testo prima di usare l'istruzione BULK INSERT per eseguire l'importazione. L'istruzione BULK INSERT non legge direttamente i file di Excel. Con il comando BULK INSERT, è possibile importare un file CSV archiviato localmente o in Archiviazione BLOB di Azure.

```sql
USE ImportFromExcel;
GO
BULK INSERT Data_bi FROM 'C:\Temp\data.csv'
   WITH (
      FIELDTERMINATOR = ',',
      ROWTERMINATOR = '\n'
);
GO
```

Per altre informazioni ed esempi per SQL Server e il database SQL di Azure, vedere gli argomenti seguenti:

- [Importare dati per operazioni bulk usando BULK INSERT o OPENROWSET (BULK...)](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)
- [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md)

## <a name="bcp-tool"></a><a name="bcp"></a> Strumento BCP

BCP è un programma eseguibile dal prompt dei comandi. L'esempio seguente carica i dati dal file con valori delimitati da virgole `Data.csv` nella tabella di database `Data_bcp` esistente.

Come descritto in precedenza nella sezione [Prerequisito](#prereq), è necessario esportare i dati Excel come testo prima di usare BCP per eseguire l'importazione. BCP non legge direttamente i file di Excel. Usarlo per eseguire l'importazione in SQL Server o nel database SQL da un file di testo (CSV) salvato nella risorsa di archiviazione locale.

> [!IMPORTANT]
> Per un file di testo (CSV) archiviato in Archiviazione BLOB di Azure, usare BULK INSERT o OPENROWSET. Per alcuni esempi, vedere [Esempi](import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).

```console
bcp.exe ImportFromExcel..Data_bcp in "C:\Temp\data.csv" -T -c -t ,
```

Per altre informazioni su BCP, vedere gli argomenti seguenti:

- [Importare ed esportare dati per operazioni bulk usando l'utilità bcp](../../relational-databases/import-export/import-and-export-bulk-data-by-using-the-bcp-utility-sql-server.md)
- [Utilità bcp](../../tools/bcp-utility.md)
- [Preparare i dati per l'importazione o l'esportazione bulk](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)

## <a name="copy-wizard-azure-data-factory"></a><a name="adf-wiz"></a> Copia guidata (Azure Data Factory)

È possibile importare i dati salvati come file di testo seguendo le varie pagine della Copia guidata di Azure Data Factory.

Come descritto in precedenza nella sezione [Prerequisito](#prereq), è necessario esportare i dati Excel come testo prima di usare Azure Data Factory per eseguire l'importazione. Azure Data Factory non legge direttamente i file di Excel.

Per altre informazioni sulla Copia guidata, vedere gli argomenti seguenti:

- [Copia guidata di data factory](/azure/data-factory/data-factory-azure-copy-wizard)
- [Esercitazione: Creare una pipeline con l'attività di copia usando la Copia guidata di Data Factory](/azure/data-factory/data-factory-copy-data-wizard-tutorial).

## <a name="azure-data-factory"></a><a name="adf"></a> Azure Data Factory

Se si ha familiarità con Azure Data Factory e si preferisce non eseguire la Copia guidata, creare una pipeline con un'attività di copia dal file di testo a SQL Server o al database SQL di Azure.

Come descritto in precedenza nella sezione [Prerequisito](#prereq), è necessario esportare i dati Excel come testo prima di usare Azure Data Factory per eseguire l'importazione. Azure Data Factory non legge direttamente i file di Excel.

Per altre informazioni sull'uso di questi sink e origini di Data Factory, vedere gli argomenti seguenti:

- [File system](/azure/data-factory/data-factory-onprem-file-system-connector)
- [SQL Server](/azure/data-factory/data-factory-sqlserver-connector)
- [Database SQL di Azure](/azure/data-factory/data-factory-azure-sql-connector)

Per istruzioni su come copiare dati con Azure Data Factory, vedere gli argomenti seguenti:

- [Spostare dati con l'attività di copia](/azure/data-factory/data-factory-data-movement-activities)
- [Esercitazione: Creare una pipeline con l'attività di copia usando il portale di Azure](/azure/data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database)

## <a name="common-errors"></a>Errori comuni

### <a name="microsoftaceoledb120-has-not-been-registered"></a>Microsoft.ACE.OLEDB.12.0" non è stato registrato

Questo errore si verifica perché non è installato il provider OLE DB. Installarlo da [Microsoft Access Database Engine 2010 Redistributable](https://www.microsoft.com/download/details.aspx?id=13255). Assicurarsi di installare la versione a 64 bit se Windows e SQL Server sono entrambi a 64 bit.

Testo dell'errore completo:

```
Msg 7403, Level 16, State 1, Line 3
The OLE DB provider "Microsoft.ACE.OLEDB.12.0" has not been registered.
```

### <a name="cannot-create-an-instance-of-ole-db-provider-microsoftaceoledb120-for-linked-server-null"></a>Impossibile creare un'istanza del provider OLE DB "Microsoft.ACE.OLEDB.12.0" per il server collegato "(null)"

Indica che Microsoft OLE DB non è stato configurato correttamente. Eseguire il codice Transact-SQL seguente per risolvere il problema:

```sql
EXEC sp_MSset_oledb_prop N'Microsoft.ACE.OLEDB.12.0', N'AllowInProcess', 1
EXEC sp_MSset_oledb_prop N'Microsoft.ACE.OLEDB.12.0', N'DynamicParameters', 1
```

Testo dell'errore completo:

```
Msg 7302, Level 16, State 1, Line 3
Cannot create an instance of OLE DB provider "Microsoft.ACE.OLEDB.12.0" for linked server "(null)".
```

### <a name="the-32-bit-ole-db-provider-microsoftaceoledb120-cannot-be-loaded-in-process-on-a-64-bit-sql-server"></a>Impossibile caricare il provider OLE DB a 32 bit "Microsoft.ACE.OLEDB.12.0" in-process in SQL Server a 64 bit

Ciò si verifica quando si installa una versione a 32 bit del provider OLE DB con SQL Server a 64 bit. Per risolvere questo problema, disinstallare la versione a 32 bit e installare la versione a 64 bit del provider OLE DB.

Testo dell'errore completo:

```
Msg 7438, Level 16, State 1, Line 3
The 32-bit OLE DB provider "Microsoft.ACE.OLEDB.12.0" cannot be loaded in-process on a 64-bit SQL Server.
```

### <a name="the-ole-db-provider-microsoftaceoledb120-for-linked-server-null-reported-an-error-the-provider-did-not-give-any-information-about-the-error"></a>Il provider OLE DB "Microsoft.ACE.OLEDB.12.0" per il server collegato "(null)" ha segnalato un errore. Il provider non ha fornito informazioni sull'errore

### <a name="cannot-initialize-the-data-source-object-of-ole-db-provider-microsoftaceoledb120-for-linked-server-null"></a>Impossibile inizializzare l'oggetto origine dei dati del provider OLE DB "Microsoft.ACE.OLEDB.12.0" per il server collegato "(null)"

Entrambi questi errori indicano in genere un problema di autorizzazione tra il processo di SQL Server e il file. Verificare che l'account che esegue il servizio SQL Server abbia l'autorizzazione di accesso completo al file. È consigliabile evitare di cercare di importare i file dal desktop.

Testo degli errori completo:

```
Msg 7399, Level 16, State 1, Line 3
The OLE DB provider "Microsoft.ACE.OLEDB.12.0" for linked server "(null)" reported an error. The provider did not give any information about the error.
```

```
Msg 7303, Level 16, State 1, Line 3
Cannot initialize the data source object of OLE DB provider "Microsoft.ACE.OLEDB.12.0" for linked server "(null)".
```

## <a name="see-also"></a>Vedere anche

[Caricare dati da o in Excel con SQL Server Integration Services (SSIS)](../../integration-services/load-data-to-from-excel-with-ssis.md)