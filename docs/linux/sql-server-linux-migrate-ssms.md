---
title: Esportare e importare un database in Linux
description: Questo articolo illustra come usare SQL Server Management Studio e SqlPackage.exe per esportare e importare un database in SQL Server in Linux.
author: VanMSFT
ms.author: vanto
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 2210cfc3-c23a-4025-a551-625890d6845f
ms.openlocfilehash: f33a5a6890d12628faf3144a0ca6e8471a15c73d
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/16/2020
ms.locfileid: "97559283"
---
# <a name="export-and-import-a-database-on-linux-with-ssms-or-sqlpackageexe-on-windows"></a>Esportare e importare un database in Linux con SSMS o SqlPackage.exe in Windows

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo illustra come usare [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) e [SqlPackage.exe](../tools/sqlpackage/sqlpackage.md) per esportare e importare un database in SQL Server in Linux. Poiché SSMS e SqlPackage.exe sono applicazioni Windows, usare questa tecnica se si ha un computer Windows in grado di connettersi a un'istanza di SQL Server in Linux remota.

È consigliabile installare e usare sempre la versione più recente di SQL Server Management Studio (SSMS) come descritto in [Usare SSMS in Windows per connettersi a SQL Server in Linux](sql-server-linux-manage-ssms.md)

> [!NOTE]
> Se si esegue la migrazione di un database da un'istanza di SQL Server a un'altra, è consigliabile usare [Backup e ripristino](sql-server-linux-migrate-restore-database.md).

## <a name="export-a-database-with-ssms"></a>Esportare un database con SSMS

1. Avviare SSMS digitando **Microsoft SQL Server Management Studio** nella casella di ricerca di Windows e quindi fare clic sull'applicazione desktop.

    ![SQL Server Management Studio](./media/sql-server-linux-manage-ssms/ssms.png) 

2. Connettersi al database di origine in Esplora oggetti. Il database di origine può trovarsi in Microsoft SQL Server in esecuzione in locale o nel cloud, in Linux, Windows o Docker e nel database SQL di Azure o in Azure Synapse Analytics.

3. Fare clic con il pulsante destro del mouse su Esplora oggetti, scegliere **Attività** e quindi fare clic su **Esporta l'applicazione livello dati...**

4. Nell'esportazione guidata fare clic su **Avanti** e quindi nella scheda **Impostazioni** configurare l'esportazione in modo da salvare il file BACPAC in un percorso del disco locale o in un BLOB di Azure.

5. Per impostazione predefinita, vengono esportati tutti gli oggetti nel database. Fare clic sulla **scheda Avanzate** e scegliere gli oggetti di database da esportare.

6. Fare clic su **Avanti** , quindi su **Fine**.

Il file con estensione BACPAC è stato creato nel percorso scelto. È ora il momento di importarlo in un database di destinazione.

## <a name="import-a-database-with-ssms"></a>Importare un database con SSMS

1. Avviare SSMS digitando **Microsoft SQL Server Management Studio** nella casella di ricerca di Windows e quindi fare clic sull'applicazione desktop.

    ![SQL Server Management Studio](./media/sql-server-linux-manage-ssms/ssms.png) 

2. Connettersi al server di destinazione in Esplora oggetti. Il server di destinazione può trovarsi in Microsoft SQL Server in esecuzione in locale o nel cloud, in Linux, Windows o Docker e nel database SQL di Azure o in Azure Synapse Analytics.

3. Fare clic con il pulsante destro del mouse sulla cartella **Database** in Esplora oggetti e fare clic su **Importa applicazione livello dati...**

4. Per creare il database nel server di destinazione, specificare un file BACPAC dal disco locale o selezionare l'account di archiviazione di Azure e il contenitore in cui è stato caricato il file BACPAC.

5. Specificare un nome per il nuovo database. Se si sta importando un database nel database SQL di Azure, impostare l'edizione del database SQL di Microsoft Azure (livello di servizio), le dimensioni massime del database e l'obiettivo di servizio (livello di prestazioni).

6. Fare clic su **Avanti** e quindi su **Fine** per importare il file BACPAC in un nuovo database nel server di destinazione.

Il file BACPAC verrà importato per creare un nuovo database nel server di destinazione specificato.

## <a name="sqlpackage-command-line-option"></a><a id="sqlpackage"></a> Opzione della riga di comando SqlPackage

Per esportare e importare file BACPAC è anche possibile usare lo strumento da riga di comando di SQL Server Data Tools (SSDT), [SqlPackage.exe](../tools/sqlpackage/sqlpackage.md).

Il comando di esempio seguente esporta un file BACPAC:

```bash
SqlPackage.exe /a:Export /ssn:tcp:<your_server> /sdn:<your_database> /su:<username> /sp:<password> /tf:<path_to_bacpac>
```

Usare il comando seguente per importare lo schema del database e i dati utente da un file con estensione BACPAC:

```bash
SqlPackage.exe /a:Import /tsn:tcp:<your_server> /tdn:<your_database> /tu:<username> /tp:<password> /sf:<path_to_bacpac>

```

## <a name="see-also"></a>Vedere anche
Per altre informazioni sull'uso di SSMS, vedere [Usare SQL Server Management Studio](../ssms/sql-server-management-studio-ssms.md). Per altre informazioni su SqlPackage.exe, vedere la [documentazione di riferimento di SqlPackage](../tools/sqlpackage/sqlpackage.md).