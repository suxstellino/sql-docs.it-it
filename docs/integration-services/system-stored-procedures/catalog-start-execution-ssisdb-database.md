---
description: catalog.start_execution (database SSISDB)
title: catalog.start_execution (database SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: f8663ff3-aa98-4dd8-b850-b21efada0b87
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e98650614583eab2b40e6433721a7cf2d73b1a6d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342174"
---
# <a name="catalogstart_execution-ssisdb-database"></a>catalog.start_execution (database SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Viene avviata un'istanza di esecuzione nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
catalog.start_execution [ @execution_id = ] execution_id [, [ @retry_count = ] retry_count]  
```  
  
## <a name="arguments"></a>Argomenti  
 [@execution_id =] *execution_id*  
 Identificatore univoco per l'istanza di esecuzione. *execution_id* è di tipo **bigint**.
 
 [@retry_count =] *retry_count*  
 Numero di tentativi in caso di esecuzione non riuscita. Ha effetto solo se l'esecuzione è in Scale Out. Questo parametro è facoltativo e, Se non specificato, il valore viene impostato su 0. *retry_count* è di tipo **int**.
  
## <a name="remarks"></a>Osservazioni  
 Un'esecuzione viene usata per specificare i valori del parametro usati da un pacchetto durante una singola istanza di esecuzione del pacchetto. Dopo che è stata creata un'istanza di esecuzione, prima che venga avviata, il progetto corrispondente potrebbe essere ridistribuito. In questo caso, l'istanza di esecuzione fa riferimento a un progetto obsoleto. Questo riferimento non valido impedisce il completamento della stored procedure.  
  
> [!NOTE]  
>  Le esecuzioni possono essere avviate solo una volta. Per avviare un'istanza di esecuzione, questa deve essere nello stato di creazione (valore `1` nella colonna **status** della vista [catalog.operations](../../integration-services/system-views/catalog-operations-ssisdb-database.md)).  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente viene chiamato catalog.create_execution per creare un'istanza di esecuzione del pacchetto Child1.dtsx. Il pacchetto è incluso in Project1 di Integration Services. Nell'esempio viene chiamato catalog.set_execution_parameter_value per impostare i valori per i parametri Parameter1, Parameter2 e LOGGING_LEVEL. Inoltre viene chiamato catalog.start_execution per avviare un'istanza di esecuzione.  
  
```sql
Declare @execution_id bigint  
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Child1.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'TestDeply4', @project_name=N'Integration Services Project1', @use32bitruntime=False, @reference_id=Null  
Select @execution_id  
DECLARE @var0 sql_variant = N'Child1.dtsx'  
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id, @object_type=20, @parameter_name=N'Parameter1', @parameter_value=@var0  
DECLARE @var1 sql_variant = N'Child2.dtsx'  
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id, @object_type=20, @parameter_name=N'Parameter2', @parameter_value=@var1  
DECLARE @var2 smallint = 1  
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id, @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var2  
EXEC [SSISDB].[catalog].[start_execution] @execution_id  
GO  
```  
  
## <a name="return-code-value"></a>Valore del codice restituito  
 0 (esito positivo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa stored procedure è necessaria una delle autorizzazioni seguenti:  
  
-   Autorizzazioni READ e MODIFY sull'istanza di esecuzione, autorizzazioni READ e EXECUTE sul progetto e, se applicabile, autorizzazioni per la lettura sull'ambiente a cui si fa riferimento  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errori e avvisi  
 Nell'elenco seguente vengono descritte alcune condizioni che possono generare un errore o un avviso:  
  
-   Utente senza autorizzazioni appropriate.  
  
-   Identificatore di esecuzione non valido  
  
-   Esecuzione già avviata o già completata. Le esecuzioni possono essere avviate solo una volta  
  
-   Riferimento all'ambiente associato al progetto non valido  
  
-   Valori del parametro obbligatori non impostati  
  
-   Versione del progetto associata all'istanza di esecuzione obsoleta. Può essere eseguita solo la versione più corrente di un progetto.  
  
  
