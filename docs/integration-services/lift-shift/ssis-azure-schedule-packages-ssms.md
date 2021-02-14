---
title: Pianificare i pacchetti SSIS in Azure con SQL Server Management Studio | Microsoft Docs
description: Informazioni su come pianificare i pacchetti SSIS distribuiti al database SQL di Azure tramite il comando Pianifica in SQL Server Management Studio (SSMS).
ms.date: 09/23/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 2a4460dadef71d61cb01ce67c27438cee2054857
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351983"
---
# <a name="schedule-the-execution-of-ssis-packages-deployed-in-azure-with-sql-server-management-studio-ssms"></a>Pianificare l'esecuzione di pacchetti SSIS distribuiti in Azure con SQL Server Management Studio (SSMS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



È possibile usare SQL Server Management Studio (SSMS) per pianificare i pacchetti SSIS distribuiti nel database SQL di Azure. SQL Server in locale e Istanza gestita di SQL hanno come pianificatore di processi SSIS di prima classe SQL Server Agent e Managed Instance Agent rispettivamente. Il database SQL invece non ha un pianificatore di processi SSIS di prima classe integrato. La funzionalità SSMS descritta in questo articolo offre un'interfaccia utente familiare e simile a SQL Server Agent per la pianificazione dei pacchetti distribuiti al database SQL.

Se si usa il database SQL per ospitare il catalogo SSIS `SSISDB`, è possibile usare questa funzionalità SSMS per generare le pipeline, le attività e i trigger Data Factory necessari per la pianificazione dei pacchetti SSIS. È poi possibile in seguito e facoltativamente modificare ed estendere tali oggetti in Data Factory.

Quando si usa SSMS per pianificare un pacchetto, SSIS crea automaticamente tre nuovi oggetti Data Factory, con nomi basati sul nome del pacchetto selezionato e sul timestamp. Se ad esempio il nome del pacchetto SSIS è **MyPackage**, SSMS crea nuovi oggetti Data Factory simili ai seguenti:

| Oggetto | Nome |
|---|---|
| Pipeline | **Pipeline_MyPackage_2018-05-08T09_00_00Z** |
| Attività Esegui pacchetto SSIS | **Activity_MyPackage_2018-05-08T09_00_00Z** |
| Trigger | **Trigger_MyPackage_2018-05-08T09_00_00Z** |
|||

## <a name="prerequisites"></a>Prerequisites

La funzionalità descritta in questo articolo richiede SQL Server Management Studio 17.7 o versioni successive. Per ottenere la versione più recente di SSMS, vedere [Scaricare SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).

## <a name="schedule-a-package-in-ssms"></a>Pianificare un pacchetto in SSMS

1. In Esplora oggetti di SSMS selezionare il database SSISDB, selezionare una cartella, selezionare un progetto e quindi selezionare un pacchetto. Fare clic con il pulsante destro del mouse sul pacchetto e selezionare **Pianificazione**.

    ![Selezionare un pacchetto da pianificare.](media/ssis-azure-schedule-packages-ssms/schedule-ssms-image1-schedule.png)

2. Viene visualizzata la finestra di dialogo **Nuova pianificazione**. Nella scheda **Generale** della finestra di dialogo **Nuova pianificazione** specificare un nome e descrizione per il nuovo processo pianificato.

    ![Scheda Generale della finestra di dialogo Nuova pianificazione](media/ssis-azure-schedule-packages-ssms/schedule-ssms-image2-new-schedule.png)

3. Nella scheda **Pacchetto** della finestra di dialogo **Nuova pianificazione** selezionare le impostazioni di runtime facoltative e un ambiente di runtime.

    ![Scheda Pacchetto della finestra di dialogo Nuova pianificazione](media/ssis-azure-schedule-packages-ssms/schedule-ssms-image3-new-schedule2.png)

4. Nella scheda **Pianificazione** della finestra di dialogo **Nuova pianificazione** specificare le impostazioni di pianificazione, ad esempio la frequenza, l'ora e la durata.

    ![Scheda Pianificazione della finestra di dialogo Nuova pianificazione](media/ssis-azure-schedule-packages-ssms/schedule-ssms-image4-new-schedule3.png)

5. Al termine della creazione del processo nella finestra di dialogo **Nuova pianificazione** una finestra di dialogo di conferma elenca i nuovi oggetti Data Factory che verranno creati da SQL Server Management Studio. Se si seleziona **Sì** nella finestra di dialogo di conferma, la nuova pipeline Data Factory viene aperta nel portale di Azure e consente operazioni di revisione e personalizzazione.

    ![Conferma della nuova pianificazione](media/ssis-azure-schedule-packages-ssms/schedule-ssms-image5-confirmation.png)

6. Per personalizzare il trigger di pianificazione, selezionare **Nuova/Modifica** nel menu **Trigger**.

    ![Modificare la nuova pipeline (facoltativo)](media/ssis-azure-schedule-packages-ssms/schedule-ssms-image6-edit.png)

    Viene visualizzato il pannello **Modifica trigger** nel quale è possibile personalizzare le opzioni di pianificazione.

    ![Modifica trigger](media/ssis-azure-schedule-packages-ssms/schedule-ssms-image7-edit2.png)

## <a name="next-steps"></a>Passaggi successivi

Per informazioni su altri metodi per la pianificazione di un pacchetto SSIS, vedere [Pianificare l'esecuzione di un pacchetto SSIS in Azure](ssis-azure-schedule-packages.md).

Per altre informazioni su pipeline, attività e trigger di Azure Data Factory, vedere gli articoli seguenti:
-   [Pipeline e attività in Azure Data Factory](/azure/data-factory/concepts-pipelines-activities)
-   [Esecuzione e trigger di pipeline in Azure Data Factory](/azure/data-factory/concepts-pipeline-execution-triggers)