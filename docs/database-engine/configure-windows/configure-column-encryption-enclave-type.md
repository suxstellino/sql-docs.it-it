---
title: Configurare il tipo di enclave per l'opzione di configurazione del server Always Encrypted | Microsoft Docs
description: Informazioni su come abilitare o disabilitare un'enclave sicura per Always Encrypted. Informazioni su come verificare se un'enclave è stata inizializzata correttamente.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 19e7349c3b8bb9ef104f95ef0963b6d7982a3669
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534750"
---
# <a name="configure-the-enclave-type-for-always-encrypted-server-configuration-option"></a>Configurare il tipo di enclave per l'opzione di configurazione del server Always Encrypted

[!INCLUDE [sqlserver2019-windows-only](../../includes/applies-to-version/sqlserver2019-windows-only.md)]

Questo articolo descrive come abilitare o disabilitare un enclave sicuro per Always Encrypted con enclave sicuri. Per altre informazioni, vedere [Always Encrypted con le enclave sicure](../../relational-databases/security/encryption/always-encrypted-enclaves.md) e [Configurare l'enclave sicura in SQL Server](../../relational-databases/security/encryption/always-encrypted-enclaves-configure-enclave-type.md).

L'opzione di configurazione del server **column encryption enclave type** controlla il tipo di un enclave sicuro usato per Always Encrypted. L'opzione può essere impostata su uno dei valori seguenti:  
  
|valore|Descrizione|  
|-------------------|-----------------| 
|0|**No secure enclave** (Nessun enclave sicuro). [!INCLUDE[ssDE](../../includes/ssde-md.md)] non inizializza l'enclave sicuro per Always Encrypted. Di conseguenza non è disponibile la funzionalità Always Encrypted con enclave sicuri.|  
|1|**Sicurezza basata sulla virtualizzazione (VBS)** . Il [!INCLUDE[ssDE](../../includes/ssde-md.md)] proverà a inizializzare un'enclave di sicurezza basata sulla virtualizzazione.

> [!IMPORTANT]
> Le modifiche a **column encryption enclave type** non hanno effetto finché non si riavvia l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
   
È possibile controllare il valore del tipo di enclave configurato e il valore del tipo di enclave attualmente in uso con la vista [sys.configurations (Transact-SQL)](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md). 

Per verificare che un enclave del tipo (maggiore di 0) attualmente in uso sia stato inizializzato correttamente dopo l'ultimo riavvio di [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)], controllare la vista [sys.dm_column_encryption_enclave (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-column-encryption-enclave.md):
 - Se la vista contiene esattamente una riga, l'enclave è stato inizializzato correttamente. 
 - Se la vista non contiene righe, cercare gli errori di inizializzazione dell'enclave nel log degli errori di SQL Server. Vedere [Visualizzare il log degli errori di SQL Server (SQL Server Management Studio)](../../relational-databases/performance/view-the-sql-server-error-log-sql-server-management-studio.md).

Per istruzioni dettagliate su come configurare un'enclave di sicurezza basata sulla virtualizzazione, vedere [Abilitare Always Encrypted con enclave sicuri in SQL Server](../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md#step-3-enable-always-encrypted-with-secure-enclaves-in-sql-server).

## <a name="examples"></a>Esempi  
 L'esempio seguente abilita l'enclave sicuro e imposta il tipo di enclave su VBS:

```sql  
sp_configure 'column encryption enclave type', 1;  
GO  
RECONFIGURE;  
GO  
```  

L'esempio seguente disabilita l'enclave sicuro:  

```sql  
sp_configure 'column encryption enclave type', 0;  
GO  
RECONFIGURE;  
GO  
```  

La query seguente recupera il tipo di enclave configurato e il tipo di enclave attualmente in uso:

```sql  
USE [master];
GO
SELECT
[value]
, CASE [value] WHEN 0 THEN 'No enclave' WHEN 1 THEN 'VBS' ELSE 'Other' END AS [value_description]
, [value_in_use]
, CASE [value_in_use] WHEN 0 THEN 'No enclave' WHEN 1 THEN 'VBS' ELSE 'Other' END AS [value_in_use_description]
FROM sys.configurations
WHERE [name] = 'column encryption enclave type'; 
```  
## <a name="next-steps"></a>Passaggi successivi
 [Gestire le chiavi per Always Encrypted con enclave sicuri](../../relational-databases/security/encryption/always-encrypted-enclaves-manage-keys.md)

## <a name="see-also"></a>Vedere anche  
 [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [sys.configurations (Transact-SQL)](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)   
 [sys.dm_column_encryption_enclave (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-column-encryption-enclave.md)   
