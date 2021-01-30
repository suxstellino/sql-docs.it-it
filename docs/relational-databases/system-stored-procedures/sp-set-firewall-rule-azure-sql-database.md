---
description: sp_set_firewall_rule (Database di SQL Azure)
title: sp_set_firewall_rule (database SQL di Azure) | Microsoft Docs
ms.custom: ''
ms.date: 07/28/2016
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sp_set_firewall_rule
- sp_set_firewall_rule_TSQL
- sys.sp_set_firewall_rule
- sys.sp_set_firewall_rule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_set_firewall_rule
- firewall_rules, setting server rules
ms.assetid: a974a561-5382-4039-8499-3a56767bcefe
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: aa47f5578403fa6cfd2f945ebcaa5bff767d967b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184737"
---
# <a name="sp_set_firewall_rule-azure-sql-database"></a>sp_set_firewall_rule (Database di SQL Azure)
[!INCLUDE [asdb-asa](../../includes/applies-to-version/asdb-asa.md)]

  Crea o aggiorna le impostazioni del firewall a livello di server per il server del [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Questa stored procedure è disponibile solo nel database master per l'account di accesso dell'entità di livello server o assegnata Azure Active Directory entità di protezione.  
  
  
## <a name="syntax"></a>Sintassi  
  
```
sp_set_firewall_rule [@name =] 'name', 
    [@start_ip_address =] 'start_ip_address', 
    [@end_ip_address =] 'end_ip_address'
[ ; ]  
```  
  
## <a name="arguments"></a>Argomenti  
 Nella tabella seguente vengono illustrati gli argomenti e le opzioni supportati in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] .  
  
|Nome|Datatype|Descrizione|  
|----------|--------------|-----------------|  
|[ @name =]' nome '|**NVARCHAR (128)**|Nome utilizzato per descrivere e distinguere l'impostazione del firewall a livello di server.|  
|[ @start_ip_address =]' start_ip_address '|**VARCHAR (50)**|L'indirizzo IP più basso nell'intervallo dell'impostazione del firewall a livello di server. Gli indirizzi IP uguali o maggiori di questo possono tentare la connessione al server del [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. L'indirizzo IP più basso possibile è `0.0.0.0`.|  
|[ @end_ip_address =]' end_ip_address '|**VARCHAR (50)**|L'indirizzo IP più alto nell'intervallo dell'impostazione del firewall a livello di server. Gli indirizzi IP uguali o minori di questo possono tentare la connessione al server del [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. L'indirizzo IP più alto possibile è `255.255.255.255`.<br /><br /> Nota: i tentativi di connessione di Azure sono consentiti quando sia questo campo che il campo *start_ip_address* è uguale a `0.0.0.0` .|  
  
## <a name="remarks"></a>Commenti  
 I nomi delle impostazioni del firewall a livello di server devono essere univoci. Se il nome dell'impostazione fornito per la stored procedure esiste già nella tabella delle impostazioni del firewall, gli indirizzi IP iniziale e finale verranno aggiornati. In caso contrario, verrà creata una nuova impostazione del firewall a livello di server.  
  
 Quando si aggiunge un'impostazione del firewall a livello di server in cui gli indirizzi IP iniziale e finale sono uguali a `0.0.0.0`, si abilita l'accesso al server del [!INCLUDE[ssSDS](../../includes/sssds-md.md)] da Azure. Fornire un valore al parametro *Name* che consenta di ricordare l'impostazione del firewall a livello di server.  
  
 Nel [!INCLUDE[ssSDS](../../includes/sssds-md.md)] i dati dell'account di accesso necessari per autenticare una connessione e le regole del firewall a livello di server vengono memorizzati temporaneamente nella cache in ogni database. Questa cache viene aggiornata periodicamente. Per forzare un aggiornamento della cache di autenticazione e assicurarsi che un database abbia la versione più recente della tabella di account di accesso, eseguire [DBCC FLUSHAUTHCACHE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo l'account di accesso dell'entità di livello server creato dal processo di provisioning o un'entità Azure Active Directory assegnata come amministratore può creare o modificare le regole del firewall a livello di server. Per eseguire sp_set_firewall_rule, l'utente deve essere connesso al database master.  
  
## <a name="examples"></a>Esempio  
 Il codice seguente consente di creare un'impostazione del firewall a livello di server denominata `Allow Azure` che abilita l'accesso da Azure. Eseguire il comando seguente nel database master virtuale.  
  
```  
-- Enable Azure connections.  
exec sp_set_firewall_rule N'Allow Azure', '0.0.0.0', '0.0.0.0';  
  
```  
  
 Il codice seguente consente di creare un'impostazione del firewall a livello di server denominata `Example setting 1` solo per l'indirizzo IP `0.0.0.2`. Quindi, il `sp_set_firewall_rule` stored procedure viene chiamato di nuovo per aggiornare l'indirizzo IP finale a `0.0.0.4` , in tale impostazione del firewall. Viene creato un intervallo che consente agli indirizzi IP `0.0.0.2` , `0.0.0.3` , e `0.0.0.4` di accedere al server.  
  
```  
-- Create server-level firewall setting for only IP 0.0.0.2  
exec sp_set_firewall_rule N'Example setting 1', '0.0.0.2', '0.0.0.2';  
  
-- Update server-level firewall setting to create a range of allowed IP addresses
exec sp_set_firewall_rule N'Example setting 1', '0.0.0.2', '0.0.0.4';  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Firewall del database SQL di Azure](/azure/azure-sql/database/firewall-configure)   
 [Procedura: configurare le impostazioni del firewall (database SQL di Azure)](/azure/azure-sql/database/firewall-configure)   
 [sys.firewall_rules &#40;database SQL di Azure&#41;](../../relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database.md)