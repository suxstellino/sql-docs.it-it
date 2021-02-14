---
title: Eseguire i pacchetti SSIS in Azure | Microsoft Docs
description: Panoramica dei metodi disponibili per l'esecuzione di pacchetti SSIS distribuiti nel database SQL di Azure.
ms.date: 05/29/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 2c7209d43628296169e973012ea459296acc3549
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351969"
---
# <a name="run-sql-server-integration-services-ssis-packages-deployed-in-azure"></a>Eseguire i pacchetti SQL Server Integration Services (SSIS) distribuiti in Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



È possibile eseguire i pacchetti SSIS distribuiti nel catalogo SSISDB in un server di database SQL di Azure scegliendo uno dei metodi descritti in questo articolo. Un pacchetto può essere eseguito direttamente oppure nell'ambito di una pipeline di Azure Data Factory. Per una panoramica su SSIS in Azure, vedere [Migrazione lift-and-shift dei carichi di lavoro di SQL Server Integration Services nel cloud](ssis-azure-lift-shift-ssis-packages-overview.md).

- Eseguire un pacchetto direttamente

  - [Esecuzione con SSMS](#ssms)

  - [Esecuzione con stored procedure](#sproc)

  - [Esecuzione tramite script o codice](#script)

- Eseguire un pacchetto nell'ambito di una pipeline di Azure Data Factory

  - [Esecuzione con l'attività Esegui pacchetto SSIS](#exec_activity)

  - [Esecuzione con l'attività stored procedure](#sproc_activity)

> [!NOTE]
> L'esecuzione di un pacchetto con `dtexec.exe` non è stata testata con i pacchetti distribuiti in Azure.

## <a name="run-a-package-with-ssms"></a><a name="ssms"></a> Eseguire un pacchetto con SSMS

In SQL Server Management Studio (SSMS) è possibile fare clic con il pulsante destro del mouse su un pacchetto distribuito nel database del catalogo SSIS, SSISDB, e scegliere **Esegui** per aprire la finestra di dialogo **Esegui pacchetto**. Per altre informazioni, vedere [Eseguire un pacchetto SSIS con SQL Server Management Studio (SSMS)](../ssis-quickstart-run-ssms.md).

## <a name="run-a-package-with-stored-procedures"></a><a name="sproc"></a> Eseguire un pacchetto con le stored procedure

In qualsiasi ambiente dal quale sia possibile connettersi al database SQL di Azure ed eseguire codice Transact-SQL, è possibile eseguire un pacchetto chiamando le stored procedure seguenti:

1. **[catalog].[create_execution]** . Per altre informazioni, vedere [catalog.create_execution](../system-stored-procedures/catalog-create-execution-ssisdb-database.md).

2. **[catalog].[set_execution_parameter_value]** . Per altre informazioni, vedere [catalog.set_execution_parameter_value](../system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md).

3. **[catalog].[start_execution]** . Per altre informazioni, vedere [catalog.start_execution](../system-stored-procedures/catalog-start-execution-ssisdb-database.md).

Per altre informazioni, vedere gli esempi seguenti:

- [Eseguire un pacchetto SSIS da SSMS con Transact-SQL](../ssis-quickstart-run-tsql-ssms.md)

- [Eseguire un pacchetto SSIS da Visual Studio Code con Transact-SQL](../ssis-quickstart-run-tsql-vscode.md)

## <a name="run-a-package-with-script-or-code"></a><a name="script"></a> Eseguire un pacchetto tramite script o codice

In qualsiasi ambiente di sviluppo dal quale sia possibile chiamare un'API gestita, è possibile eseguire un pacchetto chiamando il metodo `Execute` dell'oggetto `Package` nello spazio dei nomi `Microsoft.SQLServer.Management.IntegrationServices`.

Per altre informazioni, vedere gli esempi seguenti:

- [Eseguire un pacchetto SSIS con PowerShell](../ssis-quickstart-run-powershell.md)

- [Run an SSIS package with C# code in a .NET app](../ssis-quickstart-run-dotnet.md) (Eseguire un pacchetto SSIS con codice C# in un'app .NET)

## <a name="run-a-package-with-the-execute-ssis-package-activity"></a><a name="exec_activity"></a> Eseguire un pacchetto con l'attività Esegui pacchetto SSIS

Per altre informazioni, vedere [Eseguire un pacchetto SSIS tramite l'attività Esegui pacchetto SSIS in Azure Data Factory](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

## <a name="run-a-package-with-the-stored-procedure-activity"></a><a name="sproc_activity"></a> Eseguire un pacchetto con l'attività stored procedure

Per altre informazioni, vedere [Eseguire un pacchetto SSIS usando l'attività stored procedure in Azure Data Factory](/azure/data-factory/how-to-invoke-ssis-package-stored-procedure-activity).

## <a name="next-steps"></a>Passaggi successivi

Ottenere informazioni sulle opzioni per la pianificazione dei pacchetti SSIS distribuiti in Azure. Per altre informazioni, vedere [Pianificare i pacchetti SSIS in Azure](ssis-azure-schedule-packages.md).