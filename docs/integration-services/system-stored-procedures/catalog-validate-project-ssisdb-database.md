---
description: catalog.validate_project (database SSISDB)
title: catalog.validate_project (database SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 5270689a-46d4-4847-b41f-3bed1899e955
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f57af2d5c29af5d3fd904c04637c0ba585b107a0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355290"
---
# <a name="catalogvalidate_project-ssisdb-database"></a>catalog.validate_project (database SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Viene convalidato in modo asincrono un progetto nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Sintassi  
  
```sql
catalog.validate_project [ @folder_name = ] folder_name  
    , [ @project_name = ] project_name  
    , [ @validate_type = ] validate_type  
    , [ @validation_id = ] validation_id OUTPUT  
 [  , [ @use32bitruntime = ] use32bitruntime ]  
 [  , [ @environment_scope = ] environment_scope ]  
 [  , [ @reference_id = ] reference_id ]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @folder_name = ] *folder_name*  
 Nome di una cartella in cui è contenuto il progetto. *folder_name* è di tipo **nvarchar(128)** .  
  
 [ @project_name = ] *project_name*  
 Nome del progetto. *project_name* è di tipo **nvarchar(128)** .  
  
 [ @validate_type = ] *validate_type*  
 Viene indicato il tipo di convalida da eseguire. Utilizzare il carattere `F` per eseguire una convalida completa. Questo parametro è facoltativo. Per impostazione predefinita, verrà utilizzato il carattere `F`. *validate_type* è di tipo **char(1)**.  
  
 [ @validation_id = ] *validation_id*  
 Viene restituito l'identificatore (ID) univoco della convalida. *validation_id* è di tipo **bigint**.  
  
 [ @use32bitruntime = ] *use32bitruntime*  
 Viene indicato se il runtime a 32 bit deve essere utilizzato per eseguire il pacchetto in un sistema operativo a 64 bit. Usare il valore `1` per eseguire il pacchetto con il runtime a 32 bit quando in esecuzione in un sistema operativo a 64 bit. Utilizzare il valore pari a `0` per eseguire il pacchetto con il runtime a 64 bit quando in esecuzione in un sistema operativo a 64 bit. Questo parametro è facoltativo e, *use32bitruntime* è di tipo **bit**.  
  
 [ @environment_scope = ] *environment_scope*  
 Vengono indicati i riferimenti all'ambiente considerati dalla convalida. Quando il valore è `A`, tutti i riferimenti all'ambiente associati al progetto sono inclusi nella convalida. Quando il valore è `S`, è incluso solo un singolo riferimento all'ambiente. Quando il valore è `D`, non è incluso alcun riferimento all'ambiente e ogni parametro deve disporre di un valore predefinito letterale per passare la convalida. Questo parametro è facoltativo. Per impostazione predefinita, verrà utilizzato il carattere `D`. *environment_scope* è di tipo **char(1)** .  
  
 [ @reference_id = ] *reference_id*  
 ID univoco del riferimento all'ambiente. Questo parametro è richiesto solo quando un singolo riferimento all'ambiente è incluso nella convalida, quando *environment_scope* è di tipo `S`. *reference_id* è di tipo **bigint**.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (esito positivo)  
  
## <a name="result-sets"></a>Set di risultati  
 L'output dei passaggi di convalida viene restituito sotto forma di sezioni diverse del set di risultati.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa stored procedure è necessaria una delle autorizzazioni seguenti:  
  
-   Autorizzazioni READ sul progetto e, se applicabile, autorizzazioni READ su ambienti a cui si fa riferimento  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errori e avvisi  
 Nell'elenco seguente vengono descritte alcune condizioni che possono generare un errore o un avviso:  
  
-   Convalida non riuscita per uno o più pacchetti nel progetto  
  
-   Convalida non riuscita se in uno o più ambienti utilizzati come riferimento inclusi nella convalida non sono contenute variabili di riferimento  
  
-   Tipo di convalida specificato non valido  
  
-   Nome del progetto o ID di riferimento all'ambiente non valido  
  
-   Utente senza autorizzazioni appropriate.  
  
## <a name="remarks"></a>Commenti  
 La convalida consente di identificare i problemi che impediscono il completamento dell'esecuzione dei pacchetti nel progetto. Usare la vista [catalog.validations](../../integration-services/system-views/catalog-validations-ssisdb-database.md) o [catalog.operations](../../integration-services/system-views/catalog-operations-ssisdb-database.md) per monitorare lo stato della convalida.  
  
 Solo ambienti che sono accessibili dall'utente possono essere utilizzati nella convalida. L'output della convalida viene inviato al client come set di risultati.  
  
 In questa versione la convalida del progetto non supporta la convalida della dipendenza.  
  
 Tramite la convalida completa viene confermato che tutte le variabili di ambiente a cui si fa riferimento si trovano all'interno degli ambienti di riferimento inclusi nella convalida. Tramite la convalida completa viene generato un elenco di riferimenti all'ambiente che non sono validi e variabili di ambiente a cui si fa riferimento che non sono state trovate in alcun ambiente di riferimento incluso nella convalida.  
  
  
