---
description: catalog.deploy_project (database SSISDB)
title: catalog.deploy_project (database SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 2e3439b4-7226-4b61-a993-7a1d161eac7e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ef918ad60dd685cf90fc043eba72dcc6dd7987ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342256"
---
# <a name="catalogdeploy_project-ssisdb-database"></a>catalog.deploy_project (database SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Viene distribuito un progetto in una cartella del catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] o viene aggiornato un progetto esistente distribuito precedentemente.  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
catalog.deploy_project [@folder_name =] folder_name   
      , [ @project_name = ] project_name   
      , [ @project_stream = ] projectstream   
    [ , [ @operation_id = ] operation_id OUTPUT ]   
```  
  
## <a name="arguments"></a>Argomenti  
 [@folder_name =] *folder_name*  
 Nome della cartella in cui è distribuito il progetto. *folder_name* è di tipo **nvarchar(128)** .  
  
 [@project_name =] *project_name*  
 Nome del progetto nuovo o aggiornato nella cartella. *project_name* è di tipo **nvarchar(128)** .  
  
 [@projectstream =] *projectstream*  
 Contenuto binario di un file di distribuzione progetto [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] (estensione ispac).  
  
 È possibile utilizzare un'istruzione SELECT con la funzione OPENROWSET e il provider BULK per set di righe per recuperare il contenuto binario del file. Per un esempio, vedere [Distribuire progetti e pacchetti di Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md). Per altre informazioni su OPENROWSET, vedere [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md).  
  
 *projectstream* è di tipo **varbinary(MAX)**  
  
 [@operation_id =] *operation_id*  
 Viene restituito l'identificatore univoco dell'operazione di distribuzione. *operation_id* è di tipo **bigint**.  
  
## <a name="return-code-value"></a>Valore del codice restituito  
 0 (esito positivo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa stored procedure è necessaria una delle autorizzazioni seguenti:  
  
-   Autorizzazioni CREATE_OBJECTS sulla cartella per distribuire un nuovo progetto o autorizzazioni MODIFY sul progetto per aggiornare un progetto  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errori e avvisi  
 Nell'elenco seguente vengono descritte alcune condizioni che possono determinare la generazione di un errore da parte della stored procedure:  
  
-   Parametro che fa riferimento a un oggetto inesistente, parametro che tenta di creare un oggetto già esistente o parametro non valido in alcuni altri modi  
  
-   Il valore del parametro *\@project_name* non corrispondente al nome del progetto nel file di distribuzione  
  
-   Utente senza autorizzazioni sufficienti.  
  
## <a name="remarks"></a>Commenti  
 Durante la distribuzione o aggiornamento di un progetto, il livello di protezione dei singoli pacchetti non viene controllato dalla stored procedure.  
  
  
