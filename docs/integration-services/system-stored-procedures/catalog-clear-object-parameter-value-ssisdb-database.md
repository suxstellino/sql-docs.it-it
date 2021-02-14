---
description: catalog.clear_object_parameter_value (database SSISDB)
title: catalog.clear_object_parameter_value (database SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: dcbbb714-a051-4805-9e2b-2c2fb647c890
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9b8b3397514fc0dd2e7a4d8b5dfa704ab14a5989
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346880"
---
# <a name="catalogclear_object_parameter_value-ssisdb-database"></a>catalog.clear_object_parameter_value (database SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Viene cancellato il valore di un parametro per un progetto o pacchetto di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] esistente archiviato nel server.  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
catalog.clear_object_parameter [ @folder_name = ] folder_name   
    , [ @project_name = ] project_name   
    , [ @object_type = ] object_type   
    , [ @object_name = ] object_name   
    , [ @parameter_ name = ] parameter_name  
```  
  
## <a name="arguments"></a>Argomenti  
 [ \@folder_name = ] *folder_name*  
 Nome della cartella in cui è contenuto il progetto. *folder_name* è di tipo **nvarchar(128)** .  
  
 [ \@project_name = ] *project_name*  
 Nome del progetto. *project_name* è di tipo **nvarchar(128)** .  
  
 [ \@object_type = ] *object_type*  
 Tipo di oggetto. Nei valori validi sono inclusi `20` per un progetto e `30` per un pacchetto. *object_type* è di tipo **smallInt**.  
  
 [ \@ object _name = ] *object _name*  
 Nome del pacchetto. *object_name* è di tipo **nvarchar(260)**.  
  
 [ \@parameter_ name = ] *parameter_name*  
 Nome del parametro. *parameter_ name* è di tipo **nvarchar(128)**.  
  
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
 Nell'elenco seguente sono descritte alcune condizioni che possono determinare la generazione di un errore da parte della stored procedure clear_object_parameter:  
  
-   Specificato tipo di oggetto non valido o impossibile trovare il nome dell'oggetto nel progetto  
  
-   Progetto inesistente, progetto non accessibile o nome del progetto non valido  
  
-   Specificato nome di parametro non valido.  
  
-   Utente senza autorizzazioni appropriate.  
  
  
