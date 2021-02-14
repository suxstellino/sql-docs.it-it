---
description: Eseguire un pacchetto SSIS con SQL Server Management Studio (SSMS)
title: Eseguire un pacchetto SSIS con SSMS | Microsoft Docs
ms.date: 05/21/2018
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 07021268716c90b97e8a2e15fcc59805f5d65b15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339195"
---
# <a name="run-an-ssis-package-with-sql-server-management-studio-ssms"></a>Eseguire un pacchetto SSIS con SQL Server Management Studio (SSMS)

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


Questa guida introduttiva illustra come usare SQL Server Management Studio (SSMS) per connettersi al database del catalogo SSIS e quindi eseguire un pacchetto SSIS archiviato nel catalogo SSIS da Esplora oggetti in SSMS.

SQL Server Management Studio è un ambiente integrato per la gestione di qualsiasi infrastruttura SQL, da SQL Server al database SQL. Per altre informazioni su SSMS, vedere [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md).

## <a name="prerequisites"></a>Prerequisiti

Prima di iniziare, verificare di avere l'ultima versione di SQL Server Management Studio (SSMS). Per scaricare SSMS, vedere [Scaricare SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md).

Un server di Database SQL di Azure è in ascolto sulla porta 1433. Se si sta provando a connettersi a un server di database SQL di Azure dall'interno di un firewall aziendale, per stabilire correttamente la connessione questa porta deve essere aperta nel firewall aziendale.

## <a name="supported-platforms"></a>Piattaforme supportate

È possibile usare le informazioni di questa guida introduttiva per eseguire un pacchetto SSIS nelle piattaforme seguenti:

-   SQL Server in Windows.

-   Database SQL di Azure. Per altre informazioni sulla distribuzione e l'esecuzione di pacchetti in Azure, vedere [Spostare i carichi di lavoro di SQL Server Integration Services nel cloud](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Non è possibile usare le informazioni di questa guida introduttiva per eseguire un pacchetto SSIS in Linux. Per altre informazioni sull'esecuzione di pacchetti in Linux, vedere [Estrarre, trasformare e caricare i dati in Linux con SSIS](../linux/sql-server-linux-migrate-ssis.md).

## <a name="for-azure-sql-database-get-the-connection-info"></a>Ottenere le informazioni di connessione per il database SQL di Azure

Per eseguire il pacchetto nel database SQL di Azure, ottenere le informazioni di connessione necessarie per connettersi al database del catalogo SSIS (SSISDB). Nelle procedure che seguono sono necessari il nome completo del server e le informazioni di accesso.

1. Accedere al [Portale di Azure](https://portal.azure.com/).
2. Selezionare **Database SQL** nel menu a sinistra e quindi il database SSISDB nella pagina **Database SQL**. 
3. Nella pagina **Panoramica** del database controllare il nome completo del server. Passare il mouse sul nome del server per visualizzare l'opzione **Fare clic per copiare**. 
4. Se si dimenticano le informazioni di accesso del server di database SQL di Azure, passare alla pagina del server di database SQL per visualizzare il nome amministratore del server. Se necessario, è possibile reimpostare la password.

## <a name="connect-to-the-ssisdb-database"></a>Connettersi al database SSISDB

Usare SQL Server Management Studio per stabilire una connessione al catalogo SSIS. 

1. Aprire SQL Server Management Studio.

2. Immettere le informazioni seguenti nella finestra di dialogo **Connetti al server**:

   | Impostazione       | Valore consigliato | Altre informazioni | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Tipo di server** | Motore di database | Questo valore è obbligatorio. |
   | **Nome server** | Nome completo del server | Se si sta eseguendo la connessione a un server di database SQL di Azure, il nome è nel formato `<server_name>.database.windows.net`. |
   | **autenticazione** | Autenticazione di SQL Server | Con l'autenticazione di SQL Server è possibile connettersi a SQL Server o al database SQL di Azure. Se ci si connette a un server di database SQL di Azure, non è possibile usare l'autenticazione di Windows. |
   | **Accesso** | Account amministratore del server | Account specificato al momento della creazione del server. |
   | **Password** | Password per l'account amministratore del server | Password specificata al momento della creazione del server. |

3. Fare clic su **Connetti**. In SSMS si apre la finestra Esplora oggetti. 

4. In Esplora oggetti espandere **Cataloghi di Integration Services** e quindi espandere **SSISDB** per visualizzare gli oggetti nel database del catalogo SSIS.

## <a name="run-a-package"></a>Eseguire un pacchetto

1. In Esplora oggetti selezionare il pacchetto da eseguire.

2. Fare clic con il pulsante destro del mouse e selezionare **Esegui**. Viene visualizzata la finestra di dialogo **Esegui pacchetto**.

3.  Configurare l'esecuzione del pacchetto usando le impostazioni sulle schede **Parametri**, **Gestioni connessioni** e **Avanzate** nella finestra di dialogo Esegui pacchetto .

4.  Fare clic su OK per eseguire il pacchetto.

## <a name="next-steps"></a>Passaggi successivi
- Prendere in considerazione altri modi per eseguire un pacchetto.
    - [Eseguire un pacchetto SSIS con Transact-SQL (SSMS)](./ssis-quickstart-run-tsql-ssms.md)
    - [Eseguire un pacchetto SSIS con Transact-SQL (Visual Studio Code)](ssis-quickstart-run-tsql-vscode.md)
    - [Eseguire un pacchetto SSIS dal prompt dei comandi](./ssis-quickstart-run-cmdline.md)
    - [Eseguire un pacchetto SSIS con PowerShell](ssis-quickstart-run-powershell.md)
    - [Eseguire un pacchetto SSIS con C#](./ssis-quickstart-run-dotnet.md)