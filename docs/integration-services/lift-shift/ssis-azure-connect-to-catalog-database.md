---
title: Connettersi al catalogo SSIS (SSISDB) in Azure | Microsoft Docs
description: Trovare le informazioni di connessione necessarie per connettersi al catalogo SSIS (SSISDB) ospitato in un server di database SQL di Azure.
ms.date: 09/23/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: e5ce8d83ba29843cc14c6798eb4f550eab62e557
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352017"
---
# <a name="connect-to-the-ssis-catalog-ssisdb-in-azure"></a>Connettersi al catalogo SSIS (SSISDB) in Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Trovare le informazioni di connessione necessarie per connettersi al catalogo SSIS (SSISDB) ospitato in un server di database SQL di Azure. Per eseguire la connessione sono necessari gli elementi seguenti:
- nome completo del server
- nome del database
- informazioni di accesso 

> [!IMPORTANT]
> In questo momento, non è possibile creare il database del catalogo SSISDB in un database SQL di Azure se non si crea il runtime di integrazione SSIS di Azure in Azure Data Factory. Il runtime di integrazione Azure-SSIS è l'ambiente di runtime che esegue i pacchetti SSIS in Azure. Per una procedura dettagliata del processo, vedere [Distribuire ed eseguire un pacchetto SSIS in Azure](/azure/data-factory/tutorial-create-azure-ssis-runtime-portal). 

## <a name="prerequisites"></a>Prerequisiti
Prima di iniziare, verificare di avere la versione 17.2 o successiva di SQL Server Management Studio (SSMS). Se il database del catalogo SSISDB è ospitato in Istanza gestita di SQL, verificare di avere SSMS 17.6 o versioni successive. Per scaricare la versione più recente di SSMS, vedere [Scaricare SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).

## <a name="get-the-connection-info-from-the-azure-portal"></a>Ottenere le informazioni di connessione dal portale di Azure
1. Accedere al [Portale di Azure](https://portal.azure.com/).
2. Nel portale di Azure selezionare **Database SQL** dal menu a sinistra e quindi selezionare il database `SSISDB` nella pagina dei **database SQL**. 
3. Nella pagina **Panoramica** del database `SSISDB` controllare il nome completo del server come illustrato nella figura seguente. Passare il mouse sul nome del server per visualizzare l'opzione **Fare clic per copiare**.

    ![Informazioni di connessione server](media/ssis-azure-connect-to-catalog-database/server-name.png) 

4. Se non si ricordano i dati di accesso al server di database SQL, passare alla pagina del server di database SQL. Nella pagina è possibile visualizzare il nome dell'amministratore del server e, se necessario, reimpostare la password.

## <a name="connect-with-ssms"></a>Connettersi a SSMS
1. Aprire SQL Server Management Studio.

2. **Connettersi al server**. Immettere le informazioni seguenti nella finestra di dialogo **Connetti al server**:

   | Impostazione       | Valore consigliato | Descrizione | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Tipo di server** | Motore di database | Questo valore è obbligatorio. |
   | **Nome server** | Nome completo del server | Il nome deve essere nel formato **mysqldbserver.database.windows.net**. |
   | **autenticazione** | Autenticazione di SQL Server | |
   | **Accesso** | Account amministratore del server | Si tratta dell'account specificato al momento della creazione del server. |
   | **Password** | Password per l'account amministratore del server | Si tratta della password specificata al momento della creazione del server. |

    ![Connettersi al server con SSMS](media/ssis-azure-connect-to-catalog-database/ssisdb-connect-1.png)

3. **Connettersi al database SSISDB**. Selezionare **Opzioni** per espandere la finestra di dialogo **Connetti al server**. Nella finestra di dialogo **Connetti al server** espansa selezionare la scheda **Proprietà connessione**. Nel campo **Connetti al database** selezionare o immettere `SSISDB`.

    > [!IMPORTANT]
    > Se non si seleziona `SSISDB` durante la connessione, può non essere possibile visualizzare il catalogo SSIS in Esplora oggetti.

    ![Selezionare il database SSISDB per la connessione](media/ssis-azure-connect-to-catalog-database/ssisdb-connect-2.png)

4. Selezionare **Connetti**.

5. In Esplora oggetti espandere **Cataloghi di Integration Services** e quindi espandere **SSISDB** per visualizzare gli oggetti nel database del catalogo SSIS.

    ![Trovare il database SSISDB in Esplora oggetti in SQL Server Management Studio](media/ssis-azure-connect-to-catalog-database/ssisdb-connect-3.png)

## <a name="next-steps"></a>Passaggi successivi
- Distribuire un pacchetto. Per altre informazioni, vedere [Distribuire un progetto SSIS con SQL Server Management Studio (SSMS)](../ssis-quickstart-deploy-ssms.md).
- Eseguire un pacchetto. Per altre informazioni, vedere [Eseguire un pacchetto SSIS con SQL Server Management Studio (SSMS)](../ssis-quickstart-run-ssms.md).
- Pianificare un pacchetto. Per altre informazioni, vedere [Pianificare i pacchetti SSIS in Azure](ssis-azure-schedule-packages.md).