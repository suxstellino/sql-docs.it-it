---
description: catalog.set_object_parameter_value (database SSISDB)
title: catalog.set_object_parameter_value (database SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: fb887543-f92f-404d-9495-a1dd23a6716e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: bdc1a5ff8944a29d7e2a2fcdeec297973c9849f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355300"
---
# <a name="catalogset_object_parameter_value-ssisdb-database"></a>catalog.set_object_parameter_value (database SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Viene impostato il valore di un parametro nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Viene associato il valore a una variabile di ambiente o viene assegnato un valore letterale che viene usato per impostazione predefinita nel caso non siano assegnati altri valori.  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
catalog.set_object_parameter_value [ @object_type = ] object_type   
    , [ @folder_name = ] folder_name   
    , [ @project_name = ] project_name   
    , [ @parameter_name = ] parameter_name   
    , [ @parameter_value = ] parameter_value   
 [  , [ @object_name = ] object_name ]  
 [  , [ @value_type = ] value_type ]  
```  
  
## <a name="arguments"></a>Argomenti  
 [@object_type =] *object_type*  
 Tipo di parametro. Utilizzare il valore `20` per indicare un parametro del progetto o il valore `30` per indicare un parametro del pacchetto. *object_type* è di tipo **smallInt**.  
  
 [@folder_name =] *folder_name*  
 Nome della cartella in cui è contenuto il parametro. *folder_name* è di tipo **nvarchar(128)** .  
  
 [@project_name =] *project_name*  
 Nome del progetto in cui è contenuto il parametro. *project_name* è di tipo **nvarchar(128)** .  
  
 [@parameter_name =] *parameter_name*  
 Nome del parametro. *parameter_name* è di tipo **nvarchar(128)**.  
  
 [@parameter_value =] *parameter_value*  
 Valore del parametro. *parameter_value* è di tipo **sql_variant**.  
  
 [@object_name =] *object_name*  
 Nome del pacchetto. Questo argomento è necessario quando il parametro è un parametro del pacchetto. *object_name* è di tipo **nvarchar(260)**.  
  
 [@value_type =] *value_type*  
 Tipo di valore del parametro. Usare il carattere `V` per indicare che *parameter_value* è un valore letterale che viene usato per impostazione predefinita quando non viene assegnato nessun altro valore prima dell'esecuzione. Usare il carattere `R` per indicare che *parameter_value* è un valore di riferimento ed è stato impostato sul nome di una variabile di ambiente. Questo argomento è facoltativo. Per impostazione predefinita, viene utilizzato il carattere `V`. *value_type* è di tipo **char(1)**.  
  
## <a name="return-code-value"></a>Valore del codice restituito  
 0 (esito positivo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa stored procedure è necessaria una delle autorizzazioni seguenti:  
  
-   Autorizzazioni READ e MODIFY sul progetto  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errori e avvisi  
 Nell'elenco seguente vengono descritte alcune condizioni che possono determinare la generazione di un errore da parte della stored procedure:  
  
-   Tipo di parametro non valido  
  
-   Nome del progetto non valido  
  
-   Per parametri del pacchetto, nome del pacchetto non valido  
  
-   Tipo di valore non valido  
  
-   Utente senza autorizzazioni appropriate.  
  
## <a name="remarks"></a>Osservazioni  
  
-   Se *value_type* non viene specificato, per impostazione predefinita viene usato un valore letterale per *parameter_value*. Quando viene usato un valore letterale, il parametro *value_set* nella vista [object_parameters](../../integration-services/system-views/catalog-object-parameters-ssisdb-database.md) viene impostato su `1`. Un valore di parametro NULL non è consentito.  
  
-   Se *value_type* contiene il carattere `R`, che indica un valore a cui si fa riferimento, *parameter_value* fa riferimento al nome di una variabile di ambiente.  
  
-   Il valore `20` può essere usato per il parametro *object_type* per indicare un parametro del progetto. In questo caso, un valore per *object_name* non è necessario e qualsiasi valore specificato per *object_name* verrà ignorato. Questo valore viene utilizzato quando l'utente desidera impostare un parametro del progetto.  
  
-   Il valore `30` può essere usato per il parametro *object_type* per indicare un parametro del pacchetto. In questo caso, un valore di *object_name* viene usato per indicare il pacchetto corrispondente. Se *object_name* non è specificato, la stored procedure restituisce un errore e viene terminata.  
  
  
