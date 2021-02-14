---
title: Importare ed esportare dati da SQL Server e dal database SQL di Azure
description: È possibile usare Transact-SQL, gli strumenti da riga di comando e le procedure guidate per importare ed esportare dati in SQL Server e nel database SQL di Azure in svariati formati di dati.
ms.date: 10/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 168e5e508c9228232422e54bbbe1970cdd290ec6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351326"
---
# <a name="import-and-export-data-from-sql-server-and-azure-sql-database"></a>Importare ed esportare dati da SQL Server e dal database SQL di Microsoft Azure
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
È possibile usare diversi metodi per importare ed esportare dati da SQL Server e dal database SQL di Microsoft Azure. Tali metodi includono istruzioni Transact-SQL, strumenti da riga di comando e procedure guidate.

È anche possibile importare ed esportare dati in una vasta gamma di formati. Tali formati includono file flat, Excel, database relazionali principali e vari servizi cloud.

## <a name="methods-for-importing-and-exporting-data"></a>Metodi di importazione ed esportazione dei dati

### <a name="use-transact-sql-statements"></a>Usare istruzioni Transact-SQL
È possibile importare dati con i comandi `BULK INSERT` o `OPENROWSET(BULK...)`. In genere tali comandi vengono eseguiti in SQL Server Management Studio (SSMS). Per altre informazioni, vedere [Importare dati per operazioni bulk con BULK INSERT o OPENROWSET (BULK...)](import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).

### <a name="use-bcp-from-the-command-prompt"></a>Usare BCP dal prompt dei comandi
È possibile importare ed esportare i dati con l'utilità della riga di comando BCP. Per altre informazioni, vedere [Importare ed esportare dati per operazioni bulk usando l'utilità BCP](import-and-export-bulk-data-by-using-the-bcp-utility-sql-server.md).

### <a name="use-the-import-flat-file-wizard"></a>Usare la procedura guidata Importa file flat
Se non sono necessarie tutte le opzioni di configurazione disponibili nell'Importazione/Esportazione guidata e in altri strumenti, è possibile importare un file di testo in SQL Server usando la **procedura guidata Importa file flat** in SQL Server Management Studio (SSMS). Per altre informazioni, vedere gli articoli seguenti:
- [Procedura guidata per l'importazione di file flat in SQL](import-flat-file-wizard.md)
- [What’s new in SQL Server Management Studio 17.3](https://blogs.technet.microsoft.com/dataplatforminsider/2017/10/10/whats-new-in-sql-server-management-studio-17-3/) (Novità di SQL Server Management Studio 17.3)
- [Introducing the new Import Flat File Wizard in SSMS 17.3](https://channel9.msdn.com/Shows/Data-Exposed/Introducing-the-new-Import-Flat-File-Wizard-in-SSMS-173) (Introduzione alla nuova procedura guidata Importa file flat in SSMS 17.3)

### <a name="use-the-sql-server-import-and-export-wizard"></a>Usare Importazione/Esportazione guidata SQL Server
È possibile importare o esportare dati da un'ampia gamma di origini e destinazioni con Importazione/Esportazione guidata SQL Server. Per usare la procedura guidata, è necessario avere installato SQL Server Integration Services (SSIS) o SQL Server Data Tools (SSDT). Per altre informazioni, vedere [Importare ed esportare dati con l'Importazione/Esportazione guidata SQL Server](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md).

### <a name="design-your-own-import-or-export"></a>Progettare la propria importazione o esportazione
Se si vuole progettare un'importazione di dati personalizzata, è possibile usare le funzionalità o i servizi seguenti:
-   SQL Server Integration Services. Per altre informazioni, vedere [SQL Server Integration Services](../../integration-services/sql-server-integration-services.md).
-   Azure Data Factory. Per altre informazioni, vedere [Introduzione a Azure Data Factory](/azure/data-factory/data-factory-introduction).

## <a name="data-formats-for-import-and-export"></a>Formati dei dati per importazione ed esportazione

### <a name="supported-formats"></a>Formati supportati

È possibile importare ed esportare dati, file flat o un'ampia gamma di altri formati di file, database relazionali e servizi cloud. Per altre informazioni su queste opzioni per strumenti specifici, vedere gli argomenti seguenti
-   Per l'Importazione/Esportazione guidata SQL Server, vedere [Connettersi a origini dati con Importazione/Esportazione guidata SQL Server](../../integration-services/import-export-data/connect-to-data-sources-with-the-sql-server-import-and-export-wizard.md).
-   Per SQL Server Integration Services, vedere [Connessioni in Integration Services (SSIS)](../../integration-services/connection-manager/integration-services-ssis-connections.md).
-   Per Azure Data Factory, vedere [Connettori di Azure Data Factory](/azure/data-factory/data-factory-amazon-redshift-connector).

### <a name="commonly-used-data-formats"></a>Formati di dati di uso comune

Sono disponibili esempi e considerazioni speciali per alcuni formati di dati di uso comune. Per altre informazioni su tali formati di dati, vedere gli argomenti seguenti:
-   Per Excel, vedere [Importare da Excel](import-data-from-excel-to-sql.md).
-   Per JSON, vedere [Importare documenti JSON in SQL Server](../json/import-json-documents-into-sql-server.md).
-   Per XML, vedere [Importare ed esportare documenti XML](examples-of-bulk-import-and-export-of-xml-documents-sql-server.md).
-   Per l'Archiviazione BLOB di Azure, vedere [Importare ed esportare da Archiviazione BLOB di Azure](examples-of-bulk-access-to-data-in-azure-blob-storage.md).

## <a name="next-steps"></a>Passaggi successivi
Se non si è certi su come cominciare l'attività di importazione o esportazione, valutare la possibilità di eseguire l'Importazione/esportazione guidata SQL Server. Per una breve introduzione, vedere [Iniziare con questo semplice esempio di importazione/esportazione guidata](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md).