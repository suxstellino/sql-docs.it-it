---
title: Aggiornare Data Quality Services | Microsoft Docs
description: Questo articolo offre informazioni su come aggiornare l'installazione esistente di SQL Server Data Quality Services (DQS).
ms.custom: ''
ms.date: 06/29/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: f396666b-7754-4efc-9507-0fd114cc32d5
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 36d580e73939d99d8e4a08dc609a48d0390fac6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341828"
---
# <a name="upgrade-data-quality-services"></a>Aggiornare Data Quality Services

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Questo articolo offre informazioni su come aggiornare l'installazione esistente di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Data Quality Services (DQS). Come parte dell'aggiornamento di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Data Quality Server, è inoltre necessario aggiornare lo schema dei database DQS.  
  
> [!IMPORTANT]
>  -   Per evitare eventuali perdite di dati accidentali durante l'aggiornamento dello schema, è necessario eseguire il backup dei database DQS prima di aggiornare DQS. Per altre informazioni sul ripristino dei database DQS, vedere [Backup e ripristino di database DQS](../../data-quality-services/backing-up-and-restoring-dqs-databases.md).  
> -   È possibile connettersi a [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Data Quality Server usando la versione corrente o una precedente di Data Quality Client o [Trasformazione DQS Cleansing](../../integration-services/data-flow/transformations/dqs-cleansing-transformation.md) in Integration Services per eseguire le attività relative alla qualità dei dati.  
> -   Dopo l'aggiornamento di Data Quality Services e Master Data Services, tutte le versioni precedenti del componente aggiuntivo Master Data Services per Excel non funzioneranno più. È possibile scaricare la versione [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] del componente aggiuntivo Master Data Services per Excel da [qui](../../master-data-services/master-data-services-installation-and-configuration.md).  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
  
-   È necessario aver eseguito l'accesso come membro del gruppo di amministratori nel computer di [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] .  
  
-   È necessario che l'account utente di Windows sia membro del ruolo predefinito del server sysadmin nell'istanza di SQL Server in cui è installato [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] .  
  
##  <a name="upgrading-dqs"></a><a name="Upgrade"></a> Aggiornamento di DQS  
 Per aggiornare DQS:  
  
1.  Eseguire il backup dei database DQS prima di avviare il processo di aggiornamento. Per altre informazioni sul ripristino dei database DQS, vedere [Backup e ripristino di database DQS](../../data-quality-services/backing-up-and-restoring-dqs-databases.md).  
  
2.  Aggiornare l'istanza di SQL Server dove DQS è installato.  
  
    1.  Eseguire l'Installazione guidata di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .  
  
    2.  Fare clic su **Installazione** nel riquadro sinistro.  
  
    3.  Nel riquadro a destra, fare clic su **Aggiornamento da...** una versione precedente di SQL Server.  
  
    4.  Completare l'Installazione guidata.  
  
        > [!NOTE]  
        >  Verrà aggiornata l'istanza di SQL Server a [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] ; inoltre, verrà installato il client Data Quality più recente, se questo client è stato precedentemente installato nel computer. Se il client Data Quality è stato installato in altri computer, è necessario eseguire i passaggi secondari nel passaggio 2 nei computer in questione per installare la versione corrente del client Data Quality. Tramite l'Installazione guidata viene installata la versione corrente del client Data Quality, insieme alla versione esistente di questo client. Al termine dell'aggiornamento dello schema dei database DQS, è possibile connettersi alla versione [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] di Data Quality Server utilizzando la versione corrente o una precedente del client Data Quality.  
  
3.  Aggiornare lo schema dei database DQS.  
  
    1.  Avviare il prompt dei comandi come amministratore.  
  
    2.  Al prompt dei comandi impostare la directory sul percorso in cui è disponibile il file DQSInstaller.exe. Per l'istanza predefinita di SQL Server, il file DQSinstaller.exe è disponibile in C:\Programmi\Microsoft SQL Server\MSSQL[nn].MSSQLSERVER\MSSQL\Binn:  

        >[!NOTE]
        >Nel percorso della cartella, sostituire [nn] con il numero della versione di [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].
        >- Per SQL Server 2016: 13
        >- Per SQL Server 2017: 14
    
        ```console
        cd C:\Program Files\Microsoft SQL Server\MSSQL[nn].MSSQLSERVER\MSSQL\Binn  
        ```  
  
    3.  Al prompt dei comandi digitare il comando seguente e premere INVIO:  
  
        ```console
        dqsinstaller.exe -upgrade  
        ```  
  
    4.  Tramite il programma di installazione viene richiesta all'utente la conferma dell'esecuzione del backup dei database DQS prima di continuare. Se il backup dei database DQS è già stato eseguito, digitare **S** o **Sì**, quindi premere INVIO per continuare l'aggiornamento.  
  
    5.  Se l'aggiornamento dello schema dei database DQS viene terminato correttamente, viene visualizzato un messaggio di completamento.  
  
##  <a name="verifying-the-dqs-databases-schema-upgrade"></a><a name="Verify"></a> Verifica dell'aggiornamento dello schema dei database DQS  
 Per verificare che lo schema dei database DQS sia stato aggiornato correttamente, è possibile controllare la versione corrente nei database DQS_MAIN e DQS_PROJECTS eseguendo una query sulla tabella A_DB_VERSION in ogni database. A tale scopo, procedere come indicato di seguito:  
  
1.  Avviare SQL Server Management Studio e connettersi all'istanza di SQL Server contenente lo schema dei database DQS aggiornato.  
  
2.  Eseguire la query riportata di seguito:  
  
    ```sql
    SELECT * FROM DQS_MAIN.dbo.A_DB_VERSION WHERE STATUS=2;  
    SELECT * FROM DQS_PROJECTS.dbo.A_DB_VERSION WHERE STATUS=2;  
    ```  
  
3.  Nell'output verrà visualizzata una voce per ogni aggiornamento, insieme alla data di aggiornamento. Il valore massimo di VERSION_ID e ASSEMBLY_VERSION nella data più recente è la versione corrente. Un valore pari a 2 nella colonna STATUS indica l'esito positivo. Se si è verificato un errore, questo verrà visualizzato nella colonna ERROR. Esempio di output:  
  
    |ID|UPGRADE_DATE|VERSION_ID|ASSEMBLY_VERSION|USER_NAME|STATO|ERRORE|  
    |--------|-------------------|-----------------|-----------------------|----------------|------------|-----------|  
    |1000|2013-08-11 05:26:39.567|1200|11.0.3000.0|\<DOMAIN\UserName>|2||  
    |1001|2013-09-19 15:09:37.750|1600|12.0.xxxx.0|\<DOMAIN\UserName>|2||  
  
## <a name="see-also"></a>Vedere anche  
 [Installare Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md)   
 [Rimuovere oggetti server Data Quality Services](../../sql-server/install/remove-data-quality-server-objects.md)   
 [Eseguire l'aggiornamento di SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)  
  
