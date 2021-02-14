---
description: catalog.move_project (SSISDB Database)
title: catalog.move_project (database SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: ef3b0325-d8e9-472b-bf11-7d3efa6312ff
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4c7cf1e3c23eacbabfd59ba3b8948740dc1a0543
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342189"
---
# <a name="catalogmove_project---ssisdb-database"></a>catalog.move_project (database SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Viene spostato un progetto da una cartella a un'altra all'interno del catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
catalog.move_project [ @source_folder = ] source_folder  
    , [ @project_name = ] project_name  
    , [ @destination_folder = ] destination_folder  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @source_folder = ] *source_folder*  
 Nome della cartella di origine, in cui si trova il progetto prima dello spostamento. *source_folder* è di tipo **nvarchar(128)**.  
  
 [ @project_name = ] *project_name*  
 Nome del progetto che deve essere spostato. *project_name* è di tipo **nvarchar(128)** .  
  
 [ @destination_folder = ] *destination_folder*  
 Nome della cartella di destinazione, in cui si trova il progetto dopo lo spostamento. *destination_folder* è di tipo **nvarchar(128)**.  
  
## <a name="return-code-value"></a>Valore del codice restituito  
 0 (esito positivo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa stored procedure è necessaria una delle autorizzazioni seguenti:  
  
-   Autorizzazioni READ e MODIFY sul progetto che si desidera spostare e autorizzazione CREATE_OBJECTS sulla cartella di destinazione  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errori e avvisi  
 Nell'elenco seguente vengono descritte alcune condizioni che possono determinare la generazione di un errore da parte della stored procedure:  
  
-   Progetto inesistente  
  
-   Cartella di origine inesistente  
  
-   Cartella di destinazione inesistente o progetto con lo stesso nome già presente nella cartella di destinazione  
  
-   Utente senza autorizzazioni appropriate.  
  
## <a name="remarks"></a>Commenti  
 Quando un progetto viene spostato da una cartella di origine a una di destinazione, il progetto nella cartella di origine e i riferimenti all'ambiente corrispondenti vengono eliminati. Nella cartella di destinazione vengono creati un progetto e riferimenti all'ambiente identici. I riferimenti all'ambiente relativi verranno risolti in una cartella diversa dopo lo spostamento. I riferimenti assoluti verranno risolti nella stessa cartella dopo lo spostamento.  
  
> [!NOTE]  
>  Un progetto può disporre di riferimenti all'ambiente relativi o assoluti. I riferimenti relativi fanno riferimento all'ambiente in base al nome. Per tali riferimenti è necessario che l'ambiente si trovi nella stessa cartella del progetto. I riferimenti assoluti fanno riferimento all'ambiente in base al nome e alla cartella. Tali riferimenti fanno riferimento agli ambienti che si trovano in una cartella diversa da quella del progetto.  
  
  
