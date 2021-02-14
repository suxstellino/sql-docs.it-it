---
description: catalog.add_data_tap
title: catalog.add_data_tap | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: a25ebcc7-535e-4619-adf6-4e2b5a62ba37
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8d188b361b32627ffac604a9b69e14f54fa26355
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339202"
---
# <a name="catalogadd_data_tap"></a>catalog.add_data_tap 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aggiunge una scelta dei dati sull'output di un componente in un flusso di dati del pacchetto, per un'istanza dell'esecuzione.  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
catalog.add_data_tap [ @execution_id = ] execution_id  
, [ @task_package_path = ] task_package_path  
, [ @dataflow_path_id_string = ] dataflow_path_id_string  
, [ @data_filename = ] data_filename  
, [ @max_rows = ] max_rows  
, [ @data_tap_id = ] data_tap_id OUTPUT  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @execution_id = ] *execution_id*  
 ID dell'esecuzione che contiene il pacchetto. *execution_id* è di tipo **bigint**.  
  
 [ @task_package_path = ] *task_package_path*  
 Percorso del pacchetto per l'attività Flusso di dati. La proprietà **PackagePath** dell'attività Flusso di dati specifica il percorso. Per il percorso viene applicata la distinzione tra maiuscole e minuscole. Per individuare il percorso del pacchetto, in [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] fare clic con il pulsante destro del mouse sull'attività Flusso di dati e scegliere **Proprietà**. La proprietà **PackagePath** viene visualizzata nella finestra **Proprietà**.  
  
 *task_package_path* è di tipo **nvarchar(max)** .  
  
 [ @dataflow_path_id_string = ] *dataflow_path_id_string*  
 Stringa di identificazione per il percorso del flusso di dati. Un percorso connette due componenti flusso di dati. La proprietà **IdentificationString** per il percorso specifica la stringa.  
  
 Per individuare la stringa di identificazione, in [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] fare clic con il pulsante destro del mouse sul percorso tra due componenti flusso dati e scegliere **Proprietà**. La proprietà **IdentificationString** viene visualizzata nella finestra **Proprietà**.  
  
 *dataflow_path_id_string* è di tipo **nvarchar(4000)** .  
  
 [ @data_filename = ] *data_filename*  
 Nome del file in cui sono archiviati i data tap. Se l'attività Flusso di dati viene eseguita in un contenitore Ciclo Foreach o Ciclo For, i data tap per ogni iterazione del ciclo vengono archiviati in file separati. Ogni file ha un prefisso dato da un numero corrispondente a un'iterazione.  
  
 Per impostazione predefinita, il file viene archiviato nella cartella \<*drive*>:\Programmi\Microsoft SQL Server\130\DTS\DataDumps.  
  
 *data_filename* è di tipo **nvarchar(4000)** .  
  
 [ @max_rows = ] *max_rows*  
 Numero di righe acquisite durante la scelta dei dati. Se questo valore non è specificato, vengono acquisite tutte le righe. *max_rows* è di tipo **int**.  
  
 [ @data_tap_id = ] *data_tap_id*  
 Restituisce l'ID della scelta dei dati. *data_tap_id* è di tipo **bigint**.  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente viene creata una scelta dei dati nel percorso del flusso di dati `'Paths[OLE DB Source.OLE DB Source Output]` nell'attività Flusso di dati `\Package\Data Flow Task`. Le scelte dei dati vengono archiviate nel file `output0.txt` nella cartella DataDumps (\<*drive*>:\Programmi\Microsoft SQL Server\130\DTS\DataDumps).  
  
```sql
Declare @execution_id bigint  
Exec SSISDB.Catalog.create_execution @folder_name='Packages',@project_name='SSISPackages', @package_name='Package.dtsx',@reference_id=Null, @use32bitruntime=False, @execution_id=@execution_id OUTPUT  
  
Exec SSISDB.Catalog.set_execution_parameter_value @execution_id,50, 'LOGGING_LEVEL', 0  
  
Exec SSISDB.Catalog.add_data_tap @execution_id, @task_package_path='\Package\Data Flow Task', @dataflow_path_id_string = 'Paths[OLE DB Source.OLE DB Source Output]', @data_filename = 'output0.txt'  
  
Exec SSISDB.Catalog.start_execution @execution_id  
```  
  
## <a name="remarks"></a>Osservazioni  
 Per aggiungere scelte dei dati, l'istanza di esecuzione deve essere nello stato di creazione (valore 1 nella colonna **status** della vista [catalog.operations &#40;Database SSISDB&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md)). Il valore dello stato viene modificato in seguito all'esecuzione. È possibile creare un'esecuzione chiamando [catalog.create_execution &#40;Database SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md).  
  
 Di seguito sono riportate alcune considerazioni relative alla stored procedure add_data_tap.  
  
-   Se un'esecuzione contiene un pacchetto padre e uno o più pacchetti figlio, è necessario aggiungere un data tap per ogni pacchetto per il quale si desidera creare data tap.  
  
-   Se un pacchetto contiene più di un'attività Flusso di dati con lo stesso nome, task_package_path identifica in modo univoco l'attività Flusso di dati che contiene l'output del componente di cui è stato creato un data tap.  
  
-   Quando si aggiunge la scelta dei dati, la convalida non avviene prima dell'esecuzione del pacchetto.  
  
-   È consigliabile limitare il numero di righe acquisite durante la scelta dei dati per evitare di generare file di dati di grandi dimensioni. Se nel computer in cui viene eseguita la stored procedure si esaurisce lo spazio di archiviazione per i file di dati, l'esecuzione del pacchetto viene arrestata e nel log viene scritto un messaggio di errore.  
  
-   L'esecuzione della stored procedure add_data_tap influisce sulle prestazioni del pacchetto. È consigliabile eseguire la stored procedure solo per risolvere problemi relativi ai dati.  
  
-   Per accedere al file in cui sono archiviati i data tap, è necessario essere un amministratore nel computer in cui viene eseguita la stored procedure. È anche necessario essere l'utente che ha avviato l'esecuzione che contiene il pacchetto con il data tap.  
  
## <a name="return-codes"></a>Codici restituiti  
 0 (esito positivo)  
  
 Quando la stored procedure ha esito negativo viene generato un errore.  
  
## <a name="result-set"></a>Set di risultati  
 nessuno  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa stored procedure è necessaria una delle autorizzazioni seguenti:  
  
-   Autorizzazione MODIFY per l'istanza di esecuzione  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errori e avvisi  
 Nell'elenco seguente vengono descritte le condizioni che causano la mancata riuscita della stored procedure.  
  
-   L'utente non dispone delle autorizzazioni MODIFY.  
  
-   Il data tap per il componente specificato, nel pacchetto specificato, è già stato aggiunto.  
  
-   Il valore specificato per il numero di righe da acquisire non è valido.  
  
## <a name="requirements"></a>Requisiti  
  
## <a name="external-resources"></a>Risorse esterne  
 Post di blog [SSIS 2012: A Peek to Data Taps](https://go.microsoft.com/fwlink/?LinkId=239983) (Scelte dei dati in SSIS 2012) nel sito rafael-salas.com.  
  
## <a name="see-also"></a>Vedere anche  
 [catalog.add_data_tap_by_guid](../../integration-services/system-stored-procedures/catalog-add-data-tap-by-guid.md)  
  
  
