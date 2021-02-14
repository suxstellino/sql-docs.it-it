---
description: catalog.stop_operation (database SSISDB)
title: catalog.stop_operation (database SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 97fd9d22-03dd-4eda-8f6c-ba8b67acec68
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1d215f7d53fcc525fdc6aca43c43df25a827ed6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346745"
---
# <a name="catalogstop_operation-ssisdb-database"></a>catalog.stop_operation (database SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Arresta una convalida o un'istanza di esecuzione nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
catalog.stop_operation [ @operation_id = ] operation_id  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @operation_id = ] *operation_id*  
 Identificatore univoco della convalida o dell'istanza di esecuzione. *operation_id* è di tipo **bigint**.  
  
## <a name="return-code-value"></a>Valore del codice restituito  
 0 (esito positivo)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="permissions"></a>Autorizzazioni  
 Per questa stored procedure è necessaria una delle autorizzazioni seguenti:  
  
-   Autorizzazione READ e MODIFY sulla convalida o sull'istanza di esecuzione  
  
-   Appartenenza al ruolo del database **ssis_admin**  
  
-   Appartenenza al ruolo del server **sysadmin**  
  
## <a name="errors-and-warnings"></a>Errori e avvisi  
 Nell'elenco seguente vengono descritte alcune condizioni che possono generare un errore o un avviso:  
  
-   Utente senza autorizzazioni appropriate.  
  
-   Identificatore di operazione non valido.  
  
-   Operazione già arrestata.  
  
## <a name="remarks"></a>Osservazioni  
 Solo un utente per volta deve arrestare un'operazione nel catalogo di [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Se più utenti tentano di arrestare l'operazione, tramite la stored procedure verrà restituito l'esito positivo (valore `0`) al primo tentativo, ma verrà generato un errore ai tentativi successivi.  
  
  
