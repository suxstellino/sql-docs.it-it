---
description: sp_enclave_send_keys (Transact-SQL)
title: sp_enclave_send_keys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/19/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: reference
f1_keywords:
- sp_enclave_send_keys
- sp_enclave_send_keys_TSQL
- sys.sp_enclave_send_keys
- sys.sp_enclave_send_keys_TSQL
helpviewer_keywords:
- sp_enclave_send_keys
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 22d7f8b86d5cd247691c0d7527287c901845cb58
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99157304"
---
# <a name="sp_enclave_send_keys-transact-sql"></a>sp_enclave_send_keys (Transact-SQL)
[!INCLUDE [sqlserver2019-windows-only](../../includes/applies-to-version/sqlserver2019-windows-only.md)]

Invia le chiavi di crittografia delle colonne, definite nel database, all'enclave protetta lato server utilizzata con [Always Encrypted con enclave sicure](../security/encryption/always-encrypted-enclaves.md).

`sp_enclave_send_keys` Invia solo le chiavi che sono abilitate per l'enclave e crittografano le colonne che usano la crittografia casuale e contengono indici. Per una query utente normale, un driver client fornisce all'enclave le chiavi necessarie per i calcoli nella query. `sp_enclave_send_keys` Invia tutte le chiavi di crittografia della colonna definite nel database e utilizzate per le colonne crittografate degli indici. 

`sp_enclave_send_keys` fornisce un modo semplice per inviare chiavi all'enclave e popolare la cache della chiave di crittografia della colonna per le operazioni di indicizzazione successive. Usare `sp_enclave_send_keys` per abilitare:
- Un amministratore di database per la ricompilazione o la modifica di indici o statistiche per le colonne del database crittografato, se l'amministratore di database non ha accesso alle chiavi master della colonna. Vedere [richiamare operazioni di indicizzazione usando chiavi di crittografia di colonna memorizzate nella cache](../security/encryption/always-encrypted-enclaves-create-use-indexes.md#invoke-indexing-operations-using-cached-column-encryption-keys).
- [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] per completare il ripristino degli indici nelle colonne crittografate. Vedere [ripristino del database](../security/encryption/always-encrypted-enclaves.md#database-recovery).
- Un'applicazione che utilizza .NET Framework provider di dati per SQL Server eseguire il caricamento bulk dei dati nelle colonne crittografate.

Per richiamare correttamente `sp_enclave_send_keys` , è necessario connettersi al database con Always Encrypted e i calcoli dell'enclave abilitati per la connessione al database. È anche necessario avere accesso alle chiavi master della colonna, proteggere le chiavi di crittografia della colonna, si intende inviare ed è necessario disporre delle autorizzazioni per accedere Always Encrypted metadati della chiave nel database. 

## <a name="syntax"></a>Sintassi  
  
```
sp_enclave_send_keys
[ ;]  
```

## <a name="arguments"></a>Argomenti

Questo stored procedure non dispone di argomenti.

## <a name="return-value"></a>Valore restituito

Questo stored procedure non restituisce alcun valore.
  
## <a name="result-sets"></a>Set di risultati

Questa stored procedure non include set di risultati.
  
## <a name="permissions"></a>Autorizzazioni

 Richiedere le `VIEW ANY COLUMN ENCRYPTION KEY DEFINITION` `VIEW ANY COLUMN MASTER KEY DEFINITION` autorizzazioni e nel database.  
  
## <a name="examples"></a>Esempi  
  
```sql
EXEC sp_enclave_send_keys;  
```

## <a name="see-also"></a>Vedere anche
- [Always Encrypted con enclave sicuri](../security/encryption/always-encrypted-enclaves.md) 
 
- [Creare e usare indici in colonne usando Always Encrypted con enclave sicuri](../security/encryption/always-encrypted-enclaves-create-use-indexes.md)

- [Esercitazione: Creare e usare indici sulle colonne abilitate per l'enclave tramite la crittografia casuale](../security/tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md)
