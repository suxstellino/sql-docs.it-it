---
description: catalog.set_environment_reference_type (SSISDB Database)
title: catalog.set_environment_reference_type (database SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: b79e3a06-22c0-40e5-8933-1b3414db3329
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9948b4cdc9c1c9fc6467ba718f3a1a7c59b78e54
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273212"
---
# <a name="catalogset_environment_reference_type-ssisdb-database"></a>catalog.set_environment_reference_type (SSISDB Database)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Vengono impostati il tipo di riferimento e il nome dell'ambiente associato a un riferimento all'ambiente esistente per un progetto nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
catalog.set_environment_reference_location [ @reference_id = reference_id  
    , [ @reference_type = ] reference_type  
 [  , [ @environment_folder_name = ] environment_folder_name ]  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @reference_id = ] *reference_id*  
 Identificatore univoco del riferimento all'ambiente che deve essere aggiornato. *reference_id* è di tipo **bigint**.  
  
 [ @reference_type = ] *reference_type*  
 Viene indicato se l'ambiente può essere individuato nella stessa cartella del progetto (riferimento relativo) o in una cartella diversa (riferimento assoluto). Utilizzare il valore `R` per indicare un riferimento relativo. Utilizzare il valore `A` per indicare un riferimento assoluto. *reference_type* è di tipo **char(1)** .  
  
 [ @environment_folder_name = ] *environment_folder_name*  
 Cartella in cui viene individuato l'ambiente. Questo valore è richiesto per i riferimenti assoluti. *environment_folder_name* è di tipo **nvarchar(128)** .  
  
## <a name="return-code-value"></a>Valore del codice restituito  
 0 (esito positivo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa stored procedure è necessaria una delle autorizzazioni seguenti:  
  
-   Autorizzazioni READ e MODIFY sul progetto e autorizzazione READ sull'ambiente  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errori e avvisi  
 Nell'elenco seguente vengono descritte alcune condizioni che possono generare un errore o un avviso:  
  
-   Nome della cartella, nome dell'ambiente o ID riferimento non valido  
  
-   Utente senza autorizzazioni appropriate  
  
-   Un riferimento assoluto è specificato tramite il carattere `A` nel parametro *reference_location*, ma il nome della cartella non è stato specificato con il parametro *environment_folder_name*.  
  
## <a name="remarks"></a>Osservazioni  
 Un progetto può disporre di riferimenti all'ambiente relativi o assoluti. I riferimenti relativi fanno riferimento all'ambiente in base al nome. Per tali riferimenti è necessario che l'ambiente si trovi nella stessa cartella del progetto. I riferimenti assoluti fanno riferimento all'ambiente in base al nome e alla cartella e possono fare riferimento ad ambienti che si trovano in una cartella di destinazione diversa da quella del progetto. Un progetto può fare riferimento a più ambienti.  
  
> [!IMPORTANT]  
>  Se viene specificato un riferimento relativo, il valore del parametro *environment_folder_name* non viene usato e il nome della cartella dell'ambiente viene impostato automaticamente su **NULL**. Se viene specificato un riferimento assoluto, il nome della cartella dell'ambiente deve essere fornito nel parametro *environment_folder_name*.  
  
  
