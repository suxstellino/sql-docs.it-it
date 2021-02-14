---
description: catalog.rename_environment (database SSISDB)
title: catalog.rename_environment (database SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: c73d7452-31c5-4f4e-afcc-e9eca760c826
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5cff0cdf01fbfbb0af768a18ab54614c2a46f5bb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346509"
---
# <a name="catalogrename_environment-ssisdb-database"></a>catalog.rename_environment (database SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Viene rinominato un ambiente nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
catalog.rename_environment [ @folder_name = ] folder_name  
    , [ @environment_name = ] environment_name  
    , [ @new_environment_name= ] new_environment_name  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @folder_name = ] *folder_name*  
 Nome della cartella in cui è contenuto l'ambiente. *folder_name* è di tipo **nvarchar(128)** .  
  
 [ @environment_name = ] *environment_name*  
 Nome originale dell'ambiente. *environment_name* è di tipo **nvarchar(128)** .  
  
 [ @new_environment_name = ] *new_environment_name*  
 Nuovo nome dell'ambiente. *new_environment_name* è di tipo **nvarchar(128)**.  
  
## <a name="return-code-value"></a>Valore del codice restituito  
 0 (esito positivo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa stored procedure è necessaria una delle autorizzazioni seguenti:  
  
-   Autorizzazioni MODIFY sull'ambiente  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errori e avvisi  
 Nell'elenco seguente vengono descritte alcune condizioni che possono generare un errore o un avviso:  
  
-   Nome originale dell'ambiente non valido  
  
-   Nome nuovo già utilizzato in un ambiente esistente  
  
## <a name="remarks"></a>Commenti  
 I riferimenti all'ambiente dai progetti non vengono aggiornati automaticamente quando si rinomina l'ambiente, pertanto è necessario aggiornarli di conseguenza. Questa stored procedure verrà completata anche se i riferimenti all'ambiente vengono interrotti modificando il nome dell'ambiente. I riferimenti all'ambiente devono essere aggiornati dopo il completamento di questa stored procedure.  
  
> [!NOTE]  
>  Quando un riferimento all'ambiente non è valido, la convalida e l'esecuzione dei pacchetti corrispondenti in cui vengono utilizzati tali riferimenti non verranno completate.  
  
  
