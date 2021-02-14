---
description: sys.dm_clr_properties (Transact-SQL)
title: sys.dm_clr_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_clr_properties
- sys.dm_clr_properties_TSQL
- dm_clr_properties_TSQL
- dm_clr_properties
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_properties dynamic management view
ms.assetid: 220d062f-d117-46e7-a448-06fe48db8163
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fbf92690e35a065afc9aab936e5a8e37ef59f6f6
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/13/2021
ms.locfileid: "100352690"
---
# <a name="sysdm_clr_properties-transact-sql"></a>sys.dm_clr_properties (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Restituisce una riga per ogni proprietà associata all'integrazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con Common Language Runtime (CLR), inclusi la versione e lo stato del CLR hosted. Il CLR hosted viene inizializzato eseguendo le istruzioni [create assembly](../../t-sql/statements/create-assembly-transact-sql.md), [ALTER ASSEMBLY](../../t-sql/statements/alter-assembly-transact-sql.md)o [Drop assembly](../../t-sql/statements/drop-assembly-transact-sql.md) oppure eseguendo qualsiasi routine, tipo o trigger CLR. La vista **sys.dm_clr_properties** non specifica se l'esecuzione del codice CLR utente è stata abilitata nel server. L'esecuzione del codice CLR utente viene abilitata utilizzando la [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) stored procedure con l'opzione [clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) impostata su 1.  
  
 La vista **sys.dm_clr_properties** contiene le colonne **nome** e **valore** . Ogni riga della vista include dettagli su una proprietà del CLR hosted. È possibile utilizzare questa vista per raccogliere informazioni sul CLR hosted, ad esempio la directory di installazione di CLR, la versione di CLR e lo stato corrente del CLR hosted. La vista consente inoltre di determinare se il codice dell'integrazione con CLR non funziona a causa di problemi relativi all'installazione di CLR nel computer server.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**nome**|**nvarchar(128)**|Nome della proprietà.|  
|**value**|**nvarchar(128)**|Valore della proprietà.|  
  
## <a name="properties"></a>Proprietà  
 La proprietà **directory** indica la directory in cui è stato installato il .NET Framework nel server. Nel computer server possono essere presenti più installazioni di .NET Framework. Il valore di questa proprietà identifica l'installazione utilizzata da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 La proprietà **Version** indica la versione del .NET Framework e del CLR ospitato nel server.  
  
 La **sys.dm_clr_properties** vista gestita dinamica può restituire sei valori diversi per la proprietà **state** , che riflette lo stato del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CLR hosted. Ad esempio:  
  
-   Mscoree is not loaded.  
  
-   Mscoree is loaded.  
  
-   Locked CLR version with mscoree.  
  
-   CLR is initialized.  
  
-   CLR initialization permanently failed.  
  
-   CLR is stopped.  
  
 Il **mscoree non è caricato** e gli stati **mscoree sono caricati** indica l'avanzamento dell'inizializzazione CLR ospitata all'avvio del server e non è probabile che vengano visualizzati.  
  
 È possibile che la **versione CLR bloccata con** lo stato Mscoree sia visibile laddove il CLR ospitato non è in uso e pertanto non è ancora stato inizializzato. Il CLR hosted viene inizializzato alla prima esecuzione di un'istruzione DDL, ad esempio [CREATE ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md), o un oggetto di database gestito.  
  
 Lo stato di **CLR inizializzato** indica che il CLR hosted è stato inizializzato correttamente. Si noti che questo stato non indica se l'esecuzione del codice CLR utente è stata abilitata. Se l'esecuzione del codice CLR utente viene prima abilitata e quindi disabilitata utilizzando il [!INCLUDE[tsql](../../includes/tsql-md.md)] [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) stored procedure, il valore di stato sarà ancora **CLR inizializzato**.  
  
 Lo stato di **inizializzazione in modo permanente per l'inizializzazione CLR** indica che l'inizializzazione CLR ospitata Le possibili cause sono la scarsa disponibilità di memoria o un errore nell'handshake host tra [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e il CLR. In questo caso verrà generato il messaggio di errore 6512 o 6513.  
  
 Lo **stato di CLR arrestato** viene visualizzato solo quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in corso l'arresto.  
  
## <a name="remarks"></a>Commenti  
 Le proprietà e i valori di questa visualizzazione potrebbero cambiare in una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a causa dei miglioramenti della funzionalità di integrazione con CLR.  
  
## <a name="permissions"></a>Autorizzazioni  
  
In è [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] richiesta l' `VIEW SERVER STATE` autorizzazione.   
Negli obiettivi dei Servizi Basic, S0 e S1 del database SQL e per i database in pool elastici, è necessario l'account [amministratore del server](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o l'account [amministratore Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Per tutti gli altri obiettivi del servizio di database SQL, `VIEW DATABASE STATE` è necessaria l'autorizzazione nel database.   

## <a name="examples"></a>Esempio  
 L'esempio seguente recupera informazioni relative al CLR hosted:  
  
```  
SELECT name, value   
FROM sys.dm_clr_properties;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni e viste a gestione dinamica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Viste a gestione dinamica relative a Common Language Runtime &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
  
