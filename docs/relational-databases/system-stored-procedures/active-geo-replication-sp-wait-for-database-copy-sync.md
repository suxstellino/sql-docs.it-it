---
description: Geo-Replication attivo-sp_wait_for_database_copy_sync
title: sp_wait_for_database_copy_sync
titleSuffix: Azure SQL Database
ms.date: 03/03/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sp_wait_for_database_copy_sync_TSQL
- sp_wait_for_database_copy_sync
dev_langs:
- TSQL
helpviewer_keywords:
- sp_wait_for_database_copy_sync
ms.assetid: 7068da7f-cb74-47f2-b064-eb076a0d3885
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 3651d23116aa12e25e7f6f1441a432fdeaafe4fb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180249"
---
# <a name="active-geo-replication---sp_wait_for_database_copy_sync"></a>Geo-Replication attivo-sp_wait_for_database_copy_sync
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Questa procedura è determinata da una relazione della [!INCLUDE[ssGeoDR](../../includes/ssgeodr-md.md)] tra un database primario e uno secondario. La chiamata del **sp_wait_for_database_copy_sync** fa sì che l'applicazione attenda finché tutte le transazioni di cui è stato eseguito il commit non vengono replicate e riconosciute dal database secondario attivo. Eseguire **sp_wait_for_database_copy_sync** solo sul database primario.  
  
||  
|-|  
|**Si applica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|  
  
## <a name="syntax"></a>Sintassi  
  
```  
sp_wait_for_database_copy_sync [ @target_server = ] 'server_name'   
     , [ @target_database = ] 'database_name'  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @target_server =]' server_name '  
 Nome del server di database SQL che ospita il database secondario attivo. server_name è di tipo sysname e non prevede alcun valore predefinito.  
  
 [ @target_database = ] 'database_name'  
 Nome del database secondario attivo. database_name è di tipo sysname e non prevede alcun valore predefinito.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Restituisce 0 per l'esito positivo o un numero di errore per l'esito negativo.  
  
 Le condizioni di errore più probabili sono:  
  
-   Il nome del server o il nome del database non è specificato.  
  
-   Il collegamento al nome del server o al database specificato non viene trovato.  
  
-   La connettività dell'interlink viene persa. **sp_wait_for_database_copy_sync** restituirà dopo il timeout della connessione.  
  
## <a name="permissions"></a>Autorizzazioni  
 Qualsiasi utente nel database primario può chiamare questa stored procedure di sistema. L'account di accesso deve essere un utente in entrambi i database primario e secondario attivo.  
  
## <a name="remarks"></a>Commenti  
 Tutte le transazioni di cui è stato eseguito il commit prima di una chiamata **sp_wait_for_database_copy_sync** vengono inviate al database secondario attivo.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene richiamato **sp_wait_for_database_copy_sync** per assicurarsi che venga eseguito il commit di tutte le transazioni nel database primario, DB0, inviato al relativo database secondario attivo nel server di destinazione ubfyu5ssyt.  
  
```  
USE db0;  
GO  
EXEC sys.sp_wait_for_database_copy_sync @target_server = N'ubfyu5ssyt1', @target_database = N'db0';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.dm_continuous_copy_status &#40;database SQL di Azure&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-continuous-copy-status-azure-sql-database.md)   
 [Viste a gestione dinamica (DMV) con replica geografica e funzioni &#40;database SQL di Azure&#41;](../../relational-databases/system-dynamic-management-views/geo-replication-dynamic-management-views-and-functions-azure-sql-database.md)   
 [sys.dm_geo_replication_link_status](../system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md)
  
  
