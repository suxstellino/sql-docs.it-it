---
description: catalog.add_data_tap_by_guid
title: catalog.add_data_tap_by_guid | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: ed9d7fa3-61a1-4e21-ba43-1ead7dfc74eb
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 969f6975d2f73cfea80582694a3b0370a00f7264
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354910"
---
# <a name="catalogadd_data_tap_by_guid"></a>catalog.add_data_tap_by_guid 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aggiunge una scelta dei dati a uno specifico percorso del flusso di dati in un flusso di dati del pacchetto, per un'istanza dell'esecuzione.  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
catalog.add_data_tap_by_guid [ @execution_id = ] execution_id  
, [ @dataflow_task_guid = ] dataflow_task_guid   
, [ @dataflow_path_id_string = ] dataflow_path_id_string  
, [ @data_filename = ] data_filename  
, [ @max_rows = ] max_rows  
, [ @data_tap_id = ] data_tap_id  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @execution_id = ] *execution_id*  
 ID dell'esecuzione che contiene il pacchetto. *execution_id* è di tipo **bigint**.  
  
 [ @dataflow_task_guid = ] *dataflow_task_guid*  
 ID dell'attività Flusso di dati nel pacchetto che contiene il percorso del flusso di dati da scegliere. *dataflow_task_guid* è di tipo **uniqueidentifier**.  
  
 [ @dataflow_path_id_string = ] *dataflow_path_id_string*  
 Stringa di identificazione per il percorso del flusso di dati. Un percorso connette due componenti flusso di dati. La proprietà **IdentificationString** per il percorso specifica la stringa.  
  
 Per individuare la stringa di identificazione, in [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] fare clic con il pulsante destro del mouse sul percorso tra due componenti flusso dati e scegliere **Proprietà**. La proprietà **IdentificationString** viene visualizzata nella finestra **Proprietà**.  
  
 *dataflow_path_id_string* è di tipo **nvarchar(4000)** .  
  
 [ @data_filename = ] *data_filename*  
 Nome del file in cui sono archiviati i data tap. Se l'attività Flusso di dati viene eseguita in un contenitore Ciclo Foreach o Ciclo For, i data tap per ogni iterazione del ciclo vengono archiviati in file separati. Ogni file ha un prefisso dato da un numero corrispondente a un'iterazione. I file di scelta dei dati vengono scritti nella cartella " *\<SQL Server installation folder>* \130\DTS\\". *data_filename* è di tipo **nvarchar(4000)** .  
  
 [ @max_rows = ] max_rows  
 Numero di righe acquisite durante la scelta dei dati. Se questo valore non è specificato, vengono acquisite tutte le righe. Il valore max_rows è di tipo **int**.  
  
 [ @data_tap_id = ] *data_tap_id*  
 L'ID della scelta dei dati. *data_tap_id* è di tipo **bigint**.  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente viene creata una scelta dei dati nel percorso del flusso di dati `Paths[SRC DimDCVentor.OLE DB Source Output]` nell'attività Flusso di dati `{D978A2E4-E05D-4374-9B05-50178A8817E8}`. I dati scelti vengono archiviati nel file DCVendorOutput.csv.  
  
```sql
exec catalog.add_data_tap_by_guid   @execution_id,   
'{D978A2E4-E05D-4374-9B05-50178A8817E8}',   
'Paths[SRC DimDCVentor.OLE DB Source Output]',   
'D:\demos\datafiles\DCVendorOutput.csv'  
```  
  
## <a name="remarks"></a>Osservazioni  
 Per aggiungere scelte dei dati, l'istanza di esecuzione deve essere nello stato di creazione (valore 1 nella colonna **status** della vista [catalog.operations &#40;Database SSISDB&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md)). Il valore dello stato viene modificato in seguito all'esecuzione. È possibile creare un'esecuzione chiamando [catalog.create_execution &#40;Database SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md).  
  
 Di seguito sono riportate alcune considerazioni relative alla stored procedure add_data_tap_by_guid.  
  
-   Quando si aggiunge la scelta dei dati, la convalida non avviene prima dell'esecuzione del pacchetto.  
  
-   È consigliabile limitare il numero di righe acquisite durante la scelta dei dati per evitare di generare file di dati di grandi dimensioni. Se nel computer in cui viene eseguita la stored procedure si esaurisce lo spazio di archiviazione per i file di dati, l'esecuzione della stored procedure viene arrestata.  
  
-   L'esecuzione della stored procedure add_data_tap_by_guid influisce sulle prestazioni del pacchetto. È consigliabile eseguire la stored procedure solo per risolvere problemi relativi ai dati.  
  
-   Per accedere al file in cui vengono archiviati i dati scelti, è necessario disporre delle autorizzazioni di amministratore sul computer in cui viene eseguita la stored procedure o essere l'utente che ha avviato l'esecuzione che contiene il pacchetto con la scelta dei dati.  
  
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
  
## <a name="see-also"></a>Vedere anche  
 [catalog.add_data_tap](../../integration-services/system-stored-procedures/catalog-add-data-tap.md)  
  
  
