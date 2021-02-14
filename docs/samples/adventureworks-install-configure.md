---
title: Database di esempio AdventureWorks
description: Seguire queste istruzioni per scaricare e installare i database di esempio AdventureWorks in SQL Server usando Transact-SQL (T-SQL), SQL Server Management Studio (SSMS) o Azure Data Studio.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.date: 06/16/2020
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: d706b9ef0ed5d95c411f00d426a8b9daf1415074
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340030"
---
# <a name="adventureworks-sample-databases"></a>Database di esempio AdventureWorks
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Questo articolo fornisce collegamenti diretti per scaricare i database di esempio AdventureWorks, oltre a istruzioni per il ripristino in SQL Server e nel database SQL di Azure. 

Per ulteriori informazioni sugli esempi, vedere il [repository GitHub degli esempi](https://github.com/microsoft/sql-server-samples/tree/master/samples/databases). 

## <a name="prerequisites"></a>Prerequisiti

- [SQL Server](https://www.microsoft.com/evalcenter/evaluate-sql-server-2019) o [database SQL di Azure](https://azure.microsoft.com/services/sql-database/)
- [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md) o [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)


## <a name="download-backup-files"></a>Scaricare i file di backup 

Usare questi collegamenti per scaricare il database di esempio appropriato per lo scenario. 

- I dati **OLTP** sono per i carichi di lavoro di elaborazione delle transazioni online più comuni. 
- I dati **data warehouse (DW)** sono per i carichi di lavoro di data warehousing. 
- **Lightweight (LT)** data è una versione leggera e ridotta dell'esempio **OLTP** . 

Se non si è certi di cosa è necessario, iniziare con la versione di OLTP che corrisponde alla versione di SQL Server. 

|**OLTP** |**data warehouse** |**Leggerezza**|
|---------|---------|---------|
|[AdventureWorks2019. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2019.bak)|[AdventureWorksDW2019. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2019.bak)|[AdventureWorksLT2019. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2019.bak)|
|[AdventureWorks2017. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2017.bak)|[AdventureWorksDW2017. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2017.bak)|[AdventureWorksLT2017. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2017.bak)|
|[AdventureWorks2016. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2016.bak)|[AdventureWorksDW2016. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2016.bak)|[AdventureWorksLT2016. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2016.bak)|
|[AdventureWorks2016_EXT. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2016_EXT.bak)|[AdventureWorksDW2016_EXT. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2016_EXT.bak)| N/D |
|[AdventureWorks2014. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2014.bak)|[AdventureWorksDW2014. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2014.bak)|[AdventureWorksLT2014. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2014.bak)|
|[AdventureWorks2012. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2012.bak)|[AdventureWorksDW2012. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2012.bak)|[AdventureWorksLT2012. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2012.bak)|
|[AdventureWorks2008R2. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks2008r2/adventure-works-2008r2-oltp.bak)| [AdventureWorksDW2008R2. bak](https://github.com/microsoft/sql-server-samples/releases/download/adventureworks2008r2/adventure-works-2008r2-dw.bak) | N/D |

È possibile trovare file aggiuntivi direttamente su GitHub: 

- [SQL Server 2014-2019](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks)
- [SQL Server 2012](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks2012)
- [SQL Server 2008 e 2008R2](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks2008r2)


## <a name="restore-to-sql-server"></a>Ripristina SQL Server 

È possibile usare il `.bak` file per ripristinare il database di esempio nell'istanza di SQL Server. Questa operazione può essere eseguita tramite il comando [Restore (Transact-SQL)](../t-sql/statements/restore-statements-transact-sql.md) o mediante l'interfaccia grafica (GUI) in [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md) o [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md).

# <a name="sql-server-management-studio-ssms"></a>[SQL Server Management Studio (SSMS)](#tab/ssms)

Se non si ha familiarità con SQL Server Management Studio (SSMS), è possibile vedere [connetti & query](../ssms/quickstarts/ssms-connect-query-sql-server.md) per iniziare. 

Per ripristinare il database in SQL Server Management Studio, attenersi alla procedura seguente:

1. Scaricare il `.bak` file appropriato da uno dei collegamenti disponibili nella sezione [download dei file di backup](#download-backup-files) .
2. Spostare il `.bak` file nel percorso di backup SQL Server. Questo dipende dal percorso di installazione, dal nome dell'istanza e dalla versione di SQL Server. Il percorso predefinito per un'istanza predefinita di SQL Server 2019, ad esempio, è:

   `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup`. 

3. Aprire SQL Server Management Studio (SSMS) e connettersi al SQL Server in. 
4. Fare clic con il pulsante destro del mouse su **database** in **Esplora oggetti**  >  **Restore database...** per avviare la procedura guidata **Ripristina database** . 

   :::image type="content" source="media/adventureworks-install-configure/restore-db-ssms.png" alt-text="Screenshot che illustra come scegliere di ripristinare il database facendo clic con il pulsante destro del mouse su database in Esplora oggetti, quindi selezionando Ripristina database.":::


1. Selezionare **Device (dispositivo** ) e quindi fare clic sui puntini di sospensione **(...)** per scegliere un dispositivo. 
1. Selezionare **Aggiungi** , quindi scegliere il `.bak` file spostato di recente in questo percorso. Se il file è stato spostato in questo percorso, ma non è possibile visualizzarlo nella procedura guidata, questo indica in genere un problema di autorizzazioni-SQL Server o l'utente connesso SQL Server non dispone dell'autorizzazione per questo file in questa cartella. 
1. Selezionare **OK** per confermare la selezione del backup del database e chiudere la finestra **Seleziona dispositivi di backup** . 
1. Selezionare la scheda **file** per confermare che il **ripristino come** percorso e i nomi file corrispondono al percorso e ai nomi file desiderati nella procedura guidata **Ripristina database** . 
1. Selezionare **OK** per ripristinare il database. 

   :::image type="content" source="media/adventureworks-install-configure/restore-db-wizard-ssms.png" alt-text="Screenshot che mostra la finestra Ripristina database con il set di backup da ripristinare evidenziato e l'opzione OK denominata.":::

Per altre informazioni sul ripristino di un database di SQL Server, vedere [ripristinare un backup del database tramite SSMS](../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md).

# <a name="transact-sql-t-sql"></a>[Transact-SQL (T-SQL)](#tab/tsql)

È possibile ripristinare il database di esempio tramite Transact-SQL (T-SQL). Di seguito viene fornito un esempio di ripristino di AdventureWorks2019, ma il nome del database e il percorso del file di installazione possono variare a seconda dell'ambiente. 

Per ripristinare AdventureWorks2019 a **Windows**, modificare i valori in modo appropriato per l'ambiente in uso e quindi eseguire il comando Transact-SQL (T-SQL) seguente:


```sql
USE [master]
RESTORE DATABASE [AdventureWorks2019] 
FROM  DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup\AdventureWorks2019.bak' 
WITH  FILE = 1,  NOUNLOAD,  STATS = 5
GO

```

Per ripristinare AdventureWorks2019 in **Linux**, modificare il percorso del file System di Windows in Linux, quindi eseguire il comando Transact-SQL (T-SQL) seguente: 


```sql
USE [master]
RESTORE DATABASE [AdventureWorks2019]
FROM DISK = '/var/opt/mssql/backup/AdventureWorks2019.bak'
WITH MOVE 'AdventureWorks2017' TO '/var/opt/mssql/data/AdventureWorks2019.mdf',
MOVE 'AdventureWorks2017_log' TO '/var/opt/mssql/data/AdventureWorks2019_log.ldf',
FILE = 1,  NOUNLOAD,  STATS = 5
GO
```

# <a name="azure-data-studio"></a>[Azure Data Studio](#tab/data-studio)

Se non si ha familiarità con [Azure Data Studio studio](../azure-data-studio/download-azure-data-studio.md), è possibile vedere [Connetti & query](../azure-data-studio/quickstart-sql-server.md) per iniziare

Per ripristinare il database in Azure Data Studio, attenersi alla procedura seguente:

1. Scaricare il `.bak` file appropriato da uno dei collegamenti disponibili nella sezione [download dei file di backup](#download-backup-files) .
1. Spostare il `.bak` file nel percorso di backup SQL Server. Questo dipende dal percorso di installazione, dal nome dell'istanza e dalla versione di SQL Server. Il percorso predefinito per un'istanza predefinita di SQL Server 2019, ad esempio, è:

    `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup`.

1. Aprire Azure Data Studio studio e connettersi all'istanza di SQL Server.
1. Fare clic con il pulsante destro del mouse sul server e scegliere **Gestisci**.

   :::image type="content" source="media/adventureworks-install-configure/ads-manage.png" alt-text="Screenshot che mostra Azure Data Studio con l'opzione Gestisci evidenziata e richiamata.":::

1. Selezionare **Ripristina**

   :::image type="content" source="media/adventureworks-install-configure/ads-restore-database.png" alt-text="Selezionare Ripristina nel menu in alto per ripristinare il database.":::

1. Nella scheda **generale** compilare i valori elencati in **origine**.
    1. In **Ripristina da** selezionare *file di backup*.
    1. In **percorso file di backup** selezionare il percorso in cui è stato archiviato il file con estensione bak. 
    
   :::image type="content" source="media/adventureworks-install-configure/ads-source.png" alt-text="Selezionare il percorso del file di backup":::
    
    Questa operazione consente di popolare automaticamente il resto dei campi, ad esempio **database**, **database di destinazione** e **ripristino in**. 

   :::image type="content" source="media/adventureworks-install-configure/ads-destination-restore-plan.png" alt-text="Dopo aver scelto un percorso del file di backup, il resto dei campi viene popolato automaticamente":::

1. Selezionare **Ripristina** per ripristinare il database. 

   :::image type="content" source="media/adventureworks-install-configure/ads-restore.png" alt-text="Quando si è pronti, selezionare Ripristina per ripristinare il database.":::

---

## <a name="deploy-to-azure-sql-database"></a>Eseguire la distribuzione nel database SQL di Azure

Sono disponibili due opzioni per visualizzare i dati di esempio del database SQL di Azure. È possibile usare un esempio quando si crea un nuovo database oppure è possibile distribuire un database da SQL Server direttamente in Azure usando SQL Server Management Studio (SSMS).

Per ottenere i dati di esempio per Istanza gestita SQL di Azure, vedere [ripristinare le utilità di importazione universali in sql istanza gestita](/azure/azure-sql/managed-instance/restore-sample-database-quickstart). 

### <a name="deploy-new-sample-database"></a>Distribuire un nuovo database di esempio

Quando si crea un nuovo database nel database SQL di Azure, è possibile creare un database vuoto o un database di esempio. 

Per usare un database di esempio per creare un nuovo database, seguire questa procedura: 

1. Connettersi al portale di Azure.
1. Selezionare **Crea una risorsa** nella parte superiore sinistra del riquadro di spostamento. 
1. Selezionare **database e selezionare** **database SQL**. 
1. Immettere le informazioni richieste per creare il database. 
1. Nella scheda **Impostazioni aggiuntive** scegliere **campione** come dati esistenti in **origine dati**: 

   :::image type="content" source="media/adventureworks-install-configure/deploy-sample-to-azure.png" alt-text="Scegliere esempio come origine dati nella scheda Impostazioni aggiuntive nella portale di Azure durante la creazione del database SQL di Azure":::

1. Selezionare **Crea** per creare il nuovo database SQL, ovvero la copia ripristinata del database AdventureWorksLT. 


### <a name="deploy-database-from-sql-server"></a>Distribuisci database da SQL Server

SQL Server Management Studio offre la possibilità di distribuire un database direttamente nel database SQL di Azure. Questo metodo attualmente non fornisce la convalida dei dati, pertanto è destinato allo sviluppo e al test e non deve essere utilizzato per la produzione. 

Per distribuire un database di esempio da SQL Server al database SQL di Azure, seguire questa procedura:

1. Connettersi al SQL Server in SQL Server Management Studio. 
1. Se non è già stato fatto, [ripristinare il database di esempio in SQL Server](#restore-to-sql-server). 
1. Fare clic con il pulsante destro del mouse sul database ripristinato in **Esplora oggetti**  >  **attività**  >  **Distribuisci database in database SQL di Microsoft Azure...**. 

   :::image type="content" source="media/adventureworks-install-configure/deploy-db-to-azure.png" alt-text="Scegliere di distribuire il database per database SQL di Microsoft Azure facendo clic con il pulsante destro del mouse sul database e selezionando attività":::

1. Seguire la procedura guidata per connettersi al database SQL di Azure e distribuire il database. 


## <a name="creation-scripts"></a>Script di creazione

Invece di ripristinare un database, in alternativa, è possibile utilizzare gli script per creare i database AdventureWorks indipendentemente dalla versione. 

Gli script seguenti possono essere utilizzati per creare l'intero database AdventureWorks: 

- [Script OLTP AdventureWorks zip](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks-oltp-install-script.zip)
- [Script di AdventureWorks DW zip](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW-data-warehouse-install-script.zip)

Altre informazioni sull'uso degli script sono disponibili in [GitHub](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks). 

## <a name="next-steps"></a>Passaggi successivi

Dopo aver ripristinato il database di esempio, usare le esercitazioni seguenti per iniziare a usare SQL Server: 


[Esercitazioni per SQL Server motore di database](../relational-databases/database-engine-tutorials.md)   
[Connettersi ed eseguire query con SQL Server Management Studio (SSMS)](../ssms/quickstarts/ssms-connect-query-sql-server.md)   
[Connettersi ed eseguire query con Azure Data Studio](../ssms/quickstarts/ssms-connect-query-sql-server.md)