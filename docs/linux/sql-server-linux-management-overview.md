---
title: Gestire SQL Server in Linux
description: Questo articolo mette a disposizione collegamenti ad attività e strumenti di gestione comuni per SQL Server in esecuzione in Linux.
author: VanMSFT
ms.author: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 6bd8eb0b-593d-467e-87ea-ab1c4dbcd1ea
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
ms.openlocfilehash: 3defce833ad89868dd4fb08ba6abfcfeea4a0d46
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345624"
---
# <a name="choose-the-right-tool-to-manage-sql-server-on-linux"></a>Scegliere lo strumento più adatto per gestire SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Esistono diversi modi per gestire SQL Server in Linux. La sezione seguente offre una rapida panoramica di strumenti e tecniche di gestione diversi e riferimenti ad altre risorse.

## <a name="mssql-conf"></a>mssql-conf 

Lo strumento **mssql-conf** consente di configurare SQL Server in Linux. Per altre informazioni, vedere [Configurare SQL Server in Linux con mssql conf](sql-server-linux-configure-mssql-conf.md).

## <a name="transact-sql"></a>Transact-SQL

Quasi tutte le operazioni eseguibili in uno strumento client possono essere effettuate anche con istruzioni Transact-SQL. In SQL Server sono disponibili [DMV (Dynamic Management Views, viste a gestione dinamica)](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md), che consentono di eseguire query sullo stato e sulla configurazione di SQL Server. Sono anche disponibili comandi [Transact-SQL](../t-sql/language-reference.md) per le attività di gestione dei database. È possibile eseguire questi comandi in qualsiasi strumento client che supporti la connessione a SQL Server e l'esecuzione di query Transact-SQL, ad esempio [sqlcmd](sql-server-linux-setup-tools.md) o [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md).

## <a name="azure-data-studio"></a>Azure Data Studio

Il nuovo Azure Data Studio è uno strumento multipiattaforma per la gestione di SQL Server. Per altre informazioni, vedere [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md).

## <a name="sql-server-management-studio-on-windows"></a>SQL Server Management Studio in Windows

SQL Server Management Studio (SSMS) è un'applicazione Windows che consente la gestione di SQL Server tramite un'interfaccia utente grafica. Questa applicazione può essere eseguita solo in Windows, ma è possibile usarla per connettersi in remoto alle istanze di SQL Server in Linux. Per altre informazioni sull'uso di SSMS per gestire SQL Server, vedere [Usare SSMS per gestire SQL Server in Linux](sql-server-linux-manage-ssms.md).

## <a name="mssql-cli-preview"></a>mssql-cli (anteprima)

Microsoft ha rilasciato un nuovo strumento di scripting multipiattaforma per SQL Server, [mssql-cli](https://blogs.technet.microsoft.com/dataplatforminsider/2017/12/12/try-mssql-cli-a-new-interactive-command-line-tool-for-sql-server/). Questo strumento è attualmente disponibile in anteprima.

## <a name="powershell"></a>PowerShell

PowerShell è un ambiente avanzato da riga di comando per la gestione di SQL Server in Linux. Per altre informazioni, vedere [Usare PowerShell per gestire SQL Server in Linux](sql-server-linux-manage-powershell.md).

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su SQL Server in Linux, vedere [SQL Server in Linux](sql-server-linux-overview.md).