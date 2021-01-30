---
description: sp_delete_firewall_rule (Database di SQL Azure)
title: sp_delete_firewall_rule
titleSuffix: Azure SQL Database
ms.date: 07/27/2016
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sp_delete_firewall_rule_TSQL
- sp_delete_firewall_rule
- sys.sp_delete_firewall_rule
- sys.sp_delete_firewall_rule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_firewall_rule procedure
ms.assetid: cf93eed1-ba97-4850-9fcc-b9c5a9317908
author: VanMSFT
ms.author: vanto
ms.custom: seo-dt-2019
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: 0908e90e8f84c98a3b4b74cc5877cc11d41d6de2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195504"
---
# <a name="sp_delete_firewall_rule-azure-sql-database"></a>sp_delete_firewall_rule (Database di SQL Azure)
[!INCLUDE [asdb-asa](../../includes/applies-to-version/asdb-asa.md)]

  Rimuove le impostazioni del firewall a livello di server dal server del [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Questa stored procedure è disponibile solo nel database master all'account di accesso principale di livello server.  

  
## <a name="syntax"></a>Sintassi  
  
```  
sp_delete_firewall_rule [@name =] 'name' 
[ ; ] 
```  
  
## <a name="arguments"></a>Argomenti  
 L'argomento della stored procedure è:  
  
 [ @name =]'*nome*'  
 Nome dell'impostazione del firewall a livello di server che verrà rimossa. *Name* è di **tipo nvarchar (128)** e non prevede alcun valore predefinito.  
  
## <a name="remarks"></a>Osservazioni  
 Nel [!INCLUDE[ssSDS](../../includes/sssds-md.md)] i dati dell'account di accesso necessari per autenticare una connessione e le regole del firewall a livello di server vengono memorizzati temporaneamente nella cache in ogni database. Questa cache viene aggiornata periodicamente. Per forzare un aggiornamento della cache di autenticazione e assicurarsi che un database abbia la versione più recente della tabella di account di accesso, eseguire [DBCC FLUSHAUTHCACHE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo l'account di accesso dell'entità di livello server creato dal processo di provisioning può eliminare le regole firewall a livello di server. Per eseguire sp_delete_firewall_rule, l'utente deve essere connesso al database master.  
  
## <a name="example"></a>Esempio  
 Nell'esempio seguente viene rimossa l'impostazione del firewall a livello di server denominata 'Impostazione di esempio 1'. Eseguire l'istruzione nel database master virtuale.  
  
```   
EXEC sp_delete_firewall_rule N'Example setting 1';   
```  
  
## <a name="see-also"></a>Vedere anche  
 [Firewall del database SQL di Azure](/azure/azure-sql/database/firewall-configure)   
 [Procedura: configurare le impostazioni del firewall (database SQL di Azure)](/azure/azure-sql/database/firewall-configure)   
 [sp_set_firewall_rule &#40;database SQL di Azure&#41;](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)   
 [sys.firewall_rules &#40;database SQL di Azure&#41;](../../relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database.md)  
  
