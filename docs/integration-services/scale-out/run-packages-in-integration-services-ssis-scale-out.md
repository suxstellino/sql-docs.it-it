---
title: Eseguire pacchetti in SQL Server Integration Services Scale Out (SSIS) | Microsoft Docs
description: Informazioni su come eseguire pacchetti in SQL Server Integration Services (SSIS) Scale Out usando diversi metodi.
ms.custom: performance
ms.date: 12/13/2017
ms.prod: sql
ms.technology: integration-services
ms.topic: conceptual
author: HaoQian-MS
ms.author: haoqian
ms.reviewer: maghan
f1_keywords:
- sql13.ssis.ssms.ispackageexecuteinscaleout.f1
ms.openlocfilehash: 25f550f711364760de9cc07608b46532cfa34219
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839011"
---
# <a name="run-packages-in-integration-services-ssis-scale-out"></a>Eseguire pacchetti nel servizio Integration Services (SSIS) Scale Out

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Dopo aver distribuito i pacchetti nel server Integration Services, è possibile eseguirli in Scale Out usando uno dei metodi seguenti:

-   [Finestra di dialogo Esegui pacchetto in Scale Out](#scale_out_dialog)

-   [Stored procedure](#stored_proc)

-   [SQL Server Agent - processi](#sql_agent)

## <a name="run-packages-with-the-execute-package-in-scale-out-dialog-box"></a><a name="scale_out_dialog"></a>Eseguire i pacchetti con la finestra di dialogo Esegui pacchetto in Scale Out

1. Aprire la finestra di dialogo Esegui pacchetto in Scale Out.

    In [!INCLUDE[ssManStudioFull_md](../../includes/ssmanstudiofull-md.md)]connettersi al server di Integration Services. In Esplora oggetti espandere l'albero per visualizzare il nodo in **Cataloghi di Integration Services**. Fare clic con il pulsante destro del mouse sul nodo **SSISDB** , sul progetto o sul pacchetto che si vuole eseguire e quindi fare clic su **Execute in Scale Out**(Esegui nel servizio di scalabilità orizzontale).

2. Selezionare i pacchetti e impostare le opzioni.

    Nella pagina **Selezione pacchetto** selezionare uno o più pacchetti da eseguire. Impostare ambiente, parametri, gestioni connessioni e opzioni avanzate per ogni pacchetto. Fare clic su un pacchetto per impostare queste opzioni.
    
    Nella scheda **Avanzate** impostare un'opzione di Scale Out denominata **Numero di tentativi** per specificare il numero massimo di volte in cui, in caso di errore, viene ritentata l'esecuzione di un pacchetto.

    > [!NOTE]
    > L'opzione **Dump su errori** ha effetto solo quando l'account che esegue il servizio Scale Out Worker è l'account di un amministratore del computer locale.

3. Selezionare i computer del ruolo di lavoro.

    Nella pagina **Selezione computer** selezionare i computer Scale Out Worker per l'esecuzione dei pacchetti. Per impostazione predefinita, tutti i computer possono eseguire i pacchetti. 

   > [!NOTE] 
   > I pacchetti vengono eseguiti con le credenziali degli account utente dei servizi Scale Out Worker. Verificare le credenziali nella pagina **Selezione computer**. Per impostazione predefinita, l'account è `NT Service\SSISScaleOutWorker140`.

   > [!WARNING]
   > Le esecuzioni dei pacchetti attivate da utenti diversi sullo stesso ruolo di lavoro vengono eseguite con le stesse credenziali. Non c'è alcun limite di sicurezza. 

4. Eseguire i pacchetti e visualizzare i report.

    Fare clic su **OK** per avviare le esecuzioni dei pacchetti. Per visualizzare il report di esecuzione relativo a un pacchetto, fare clic con il pulsante destro del pacchetto in Esplora oggetti, fare clic su **Report**, **Tutte le esecuzioni** e trovare l'esecuzione.
    
## <a name="run-packages-with-stored-procedures"></a><a name="stored_proc"></a> Eseguire i pacchetti con le stored procedure

1.  Creare le esecuzioni.

    Chiamare `[catalog].[create_execution]` per ogni pacchetto. Impostare il parametro **\@runinscaleout** su `True`. Se non tutti i computer Scale Out Worker sono autorizzati a eseguire il pacchetto, impostare il parametro **\@useanyworker** su `False`. Per altre informazioni su questa stored procedure e sul parametro **\@useanyworker**, vedere [catalog.create_execution](../system-stored-procedures/catalog-create-execution-ssisdb-database.md). 

2. Impostare i parametri di esecuzione.

    Chiamare `[catalog].[set_execution_parameter_value]` per ogni esecuzione.

3. Impostare le istanze di Scale Out Worker.

    Chiamare `[catalog].[add_execution_worker]`. Se tutti i computer possono eseguire il pacchetto, non è necessario chiamare questa stored procedure. 

4. Avviare le esecuzioni.

    Chiamare `[catalog].[start_execution]`. Impostare il parametro **\@retry_count** per configurare il numero di tentativi di riesecuzione di un pacchetto in caso di errore.
    
### <a name="example"></a>Esempio
Nell'esempio seguente vengono eseguiti due pacchetti, `package1.dtsx` e `package2.dtsx`, nel servizio Scale Out con un ruolo di lavoro.  

```sql
Declare @execution_id bigint
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'package1.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'folder1', @project_name=N'project1', @use32bitruntime=False, @reference_id=Null, @useanyworker=False, @runinscaleout=True
Select @execution_id
DECLARE @var0 smallint = 1
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var0
EXEC [SSISDB].[catalog].[add_execution_worker] @execution_id,  @workeragent_id=N'64c020e2-f819-4c2d-a22f-efb31a91e70a'
EXEC [SSISDB].[catalog].[start_execution] @execution_id,  @retry_count=0
GO

Declare @execution_id bigint
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'package2.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'folder2', @project_name=N'project2', @use32bitruntime=False, @reference_id=Null, @useanyworker=False, @runinscaleout=True
Select @execution_id
DECLARE @var0 smallint = 1
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var0
EXEC [SSISDB].[catalog].[add_execution_worker] @execution_id,  @workeragent_id=N'64c020e2-f819-4c2d-a22f-efb31a91e70a'
EXEC [SSISDB].[catalog].[start_execution] @execution_id,  @retry_count=0
GO
```

### <a name="permissions"></a>Autorizzazioni
Per eseguire i pacchetti in Scale Out, è necessario avere una delle autorizzazioni seguenti:

-   Appartenenza al ruolo del database **ssis_admin**  

-   Appartenenza al ruolo del database **ssis_cluster_executor**  
  
-   Appartenenza al ruolo del server **sysadmin**  

## <a name="set-default-execution-mode"></a>Impostare la modalità di esecuzione predefinita
Per impostare la modalità di esecuzione predefinita per i pacchetti su **Scale Out**, eseguire le operazioni seguenti:

1.  In SSMS in Esplora oggetti fare clic con il pulsante destro del mouse sul nodo **SSISDB** e selezionare **Proprietà**.

2.  Nella finestra di dialogo **Proprietà catalogo** impostare **Modalità di esecuzione predefinita a livello di server** su **Scale Out**.

Dopo aver impostato la modalità di esecuzione predefinita, non è più necessario specificare il parametro **\@runinscaleout** quando si chiama la stored procedure `[catalog].[create_execution]`. I pacchetti vengono eseguiti automaticamente in Scale Out. 

![Modalità file exe](media/exe-mode.PNG)

Per impostare nuovamente la modalità di esecuzione predefinita sulla modalità non Scale Out, impostare **Modalità di esecuzione predefinita a livello di server** su **Server**.

## <a name="run-package-in-sql-server-agent-job"></a><a name="sql_agent"></a>Eseguire un pacchetto in un processo di SQL Server Agent
In un processo di SQL Server Agent è possibile eseguire un pacchetto SSIS come un passaggio del processo. Per eseguire il pacchetto in Scale Out, impostare la modalità di esecuzione predefinita su **Scale Out**. Dopo aver impostato la modalità di esecuzione predefinita su **Scale Out**, i pacchetti nei processi di SQL Server Agent verranno eseguiti in modalità Scale Out.

> [!NOTE]
> Non è possibile arrestare Scale Out esecuzione del pacchetto annullando il processo di SQL Server Agent. Per arrestare Scale Out esecuzione, è consigliabile utilizzare il stored procedure catalog.stop_operation o utilizzare il riquadro **operazioni attive** . 

## <a name="next-steps"></a>Passaggi successivi
-   [Risoluzione dei problemi di scalabilità orizzontale](troubleshooting-scale-out.md)
