---
description: catalog.restore_project (database SSISDB)
title: catalog.restore_project (database SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 8adee525-579b-4d2f-b807-e2ecc07fb2e9
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1b6f8e1fedd485cd48dc916c75cc0634c477a5c8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355326"
---
# <a name="catalogrestore_project-ssisdb-database"></a>catalog.restore_project (database SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Viene ripristinata la versione precedente di un progetto nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
catalog.restore_project [ @folder_name = ] folder_name  
    , [ @project_name = ] project _name  
    , [ @object_version_lsn = ] object_version_lsn  
  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @folder_name = ] *folder_name*  
 Nome della cartella in cui è contenuto il progetto. *folder_name* è di tipo **nvarchar(128)** .  
  
 [ @project _name = ] *project_name*  
 Nome del progetto. *project_name* è di tipo **nvarchar(128)** .  
  
 [ @object_version_lsn = ] *object_version_lsn*  
 Versione del progetto. *object_version_lsn* è di tipo **bigint**.  
  
## <a name="return-code-value"></a>Valore del codice restituito  
 0 (esito positivo)  
  
## <a name="result-sets"></a>Set di risultati  
 I dettagli del progetto vengono restituiti come **varbinary(MAX)** come parte del set di risultati se viene trovato *project_name*.  
  
 **NO RESULT SET** viene restituito se non è possibile ripristinare il progetto nella cartella specificata.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa stored procedure è necessaria una delle autorizzazioni seguenti:  
  
-   Autorizzazioni READ e MODIFY sul progetto  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errori e avvisi  
 Nell'elenco seguente vengono descritte alcune condizioni che possono generare un errore o un avviso:  
  
-   Versione del progetto inesistente o non corrispondente al nome del progetto  
  
-   Progetto inesistente  
  
-   Utente senza autorizzazioni appropriate.  
  
## <a name="remarks"></a>Osservazioni  
 Quando viene ripristinato un progetto, a tutti i parametri vengono assegnati i valori predefiniti mentre tutti i riferimenti all'ambiente rimangono invariati. Il numero massimo di versioni del progetto mantenute nel catalogo è determinato dalla proprietà del catalogo **MAX_VERSIONS_PER_PROJECT**, come mostrato nella vista [catalog_property](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md).  
  
> [!WARNING]  
>  I riferimenti all'ambiente potrebbero non essere più validi dopo il ripristino di un progetto.  
  
  
