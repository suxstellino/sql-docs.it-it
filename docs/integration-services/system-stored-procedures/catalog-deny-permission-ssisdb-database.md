---
description: catalog.deny_permission (database SSISDB)
title: catalog.deny_permission (database SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: de310bac-2ddc-4ef9-8783-43dcb02a94f1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d32af051552c211592bcc4deb2bcd2c1c1c20079
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342259"
---
# <a name="catalogdeny_permission-ssisdb-database"></a>catalog.deny_permission (database SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Viene negata un'autorizzazione su un oggetto a protezione diretta nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Sintassi  
  
```sql
catalog.deny_permission [ @object_type = ] object_type  
    , [ @object_id = ] object_id  
    , [ @principal_id = ] principal_id  
    , [ @permission_type = ] permission_type  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @object_type = ] *object_type*  
 Tipo di oggetto a protezione diretta. Nei tipi di oggetti a protezione diretta sono inclusi cartelle (`1`), progetti (`2`), ambienti (`3`) e operazioni (`4`). *object_type* è di tipo **smallint** _._  
  
 [ @object_id = ] *object_id*  
 Identificatore (ID) univoco o chiave primaria dell'oggetto a protezione diretta. *object_id* è di tipo **bigint**.  
  
 [ @principal_id = ] *principal_id*  
 ID dell'entità a cui deve essere negata l'autorizzazione. *principal_id* è di tipo **int**.  
  
 [ @permission_type = ] *permission_type*  
 Tipo di autorizzazione che deve essere negata. *permission_type* è di tipo **smallint**.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (esito positivo)  
  
 1 (object_class non è valido)  
  
 2 (object_id non esiste)  
  
 3 (principal non esiste)  
  
 4 (permission non è valido)  
  
 5 (altro errore)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa stored procedure è necessaria una delle autorizzazioni seguenti:  
  
-   Autorizzazione MANAGE_PERMISSIONS sull'oggetto  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
## <a name="remarks"></a>Osservazioni  
 Questa stored procedure consente all'utente di negare i tipi di autorizzazione descritti nella tabella seguente:  
  
|Valore di permission_type|Nome dell'autorizzazione|Descrizione dell'autorizzazione|Tipi di oggetti applicabili|  
|----------------------------|---------------------|----------------------------|-----------------------------|  
|`1`|READ|Consente all'entità di leggere le informazioni considerate parte dell'oggetto, ad esempio le proprietà. Non consente all'entità di enumerare o leggere il contenuto di altri oggetti inseriti all'interno dell'oggetto.|Cartella, progetto, ambiente, operazione|  
|`2`|MODIFY|Consente all'entità di modificare le informazioni considerate parte dell'oggetto, ad esempio le proprietà. Non consente all'entità di modificare gli altri oggetti contenuti all'interno dell'oggetto.|Cartella, progetto, ambiente, operazione|  
|`3`|EXECUTE|Consente all'entità di eseguire tutti i pacchetti nel progetto.|Project|  
|`4`|MANAGE_PERMISSIONS|Consente all'entità di assegnare autorizzazioni agli oggetti.|Cartella, progetto, ambiente, operazione|  
|`100`|CREATE_OBJECTS|Consente all'entità di creare oggetti nella cartella.|Cartella|  
|`101`|READ_OBJECTS|Consente all'entità di leggere tutti gli oggetti nella cartella.|Cartella|  
|`102`|MODIFY_OBJECTS|Consente all'entità di modificare tutti gli oggetti nella cartella.|Cartella|  
|`103`|EXECUTE_OBJECTS|Consente all'entità di eseguire tutti i pacchetti di tutti i progetti contenuti nella cartella.|Cartella|  
|`104`|MANAGE_OBJECT_PERMISSIONS|Consente all'entità di gestire le autorizzazioni su tutti gli oggetti nella cartella.|Cartella|  
  
## <a name="errors-and-warnings"></a>Errori e avvisi  
 Nell'elenco seguente vengono descritte alcune condizioni che possono generare un errore o un avviso:  
  
-   Se viene specificato permission_type, la routine nega l'autorizzazione specificata assegnata in modo esplicito all'entità indicata per l'oggetto specificato. Anche se non esiste nessuna di tali istanze, tramite la routine viene restituito ancora un valore di codice con esito positivo (`0`).  
  
-   Se permission_type viene omesso, la routine nega tutte le autorizzazioni per l'entità indicata sull'oggetto specificato.  
  
  
