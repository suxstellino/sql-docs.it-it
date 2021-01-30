---
description: CREATE MASTER KEY (Transact-SQL)
title: CREATE MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_MASTER_KEY_TSQL
- MASTER_KEY_TSQL
- MASTER KEY
- CREATE MASTER KEY
dev_langs:
- TSQL
helpviewer_keywords:
- encryption [SQL Server], Database Master Key
- database master key [SQL Server]
- CREATE MASTER KEY statement
- cryptography [SQL Server], Database Master Key
- database master key [SQL Server], creating
ms.assetid: 1710a305-1a4f-48ec-836c-11ffd0356d76
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b77069fd4665ea72988f3eba5aee6c790c6c181e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188594"
---
# <a name="create-master-key-transact-sql"></a>CREATE MASTER KEY (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Crea una chiave master del database nel database master.

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintassi

```syntaxsql
CREATE MASTER KEY [ ENCRYPTION BY PASSWORD ='password' ]
[ ; ]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

PASSWORD ='*password*' Password usata per crittografare la chiave master nel database. *password* deve soddisfare i requisiti per i criteri password di Windows del computer che sta eseguendo l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *password* è facoltativo in [!INCLUDE[ssSDS](../../includes/sssds-md.md)] e [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)].

## <a name="remarks"></a>Commenti

La chiave master del database è una chiave simmetrica utilizzata per proteggere le chiavi private dei certificati e le chiavi asimmetriche presenti nel database. Al momento della creazione, la chiave master viene crittografata con l'algoritmoAES_256 e una password specificata dall'utente. In [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] viene utilizzato l'algoritmo Triple DES. Per consentire la decrittografia automatica della chiave master, una copia della chiave viene crittografata con la chiave master del servizio e archiviata sia nel database che nel database master. La copia archiviata nel database master viene generalmente aggiornata in modo automatico in seguito a ogni modifica della chiave master. È possibile modificare questa impostazione predefinita usando l'opzione DROP ENCRYPTION BY SERVICE MASTER KEY dell'istruzione [ALTER MASTER KEY](../../t-sql/statements/alter-master-key-transact-sql.md). Per aprire una chiave master non crittografata con la chiave master del servizio, è necessario usare l'istruzione [OPEN MASTER KEY](../../t-sql/statements/open-master-key-transact-sql.md) e una password.

La colonna is_master_key_encrypted_by_server della vista del catalogo sys.databases nel database master indica se la chiave master del database è crittografata con la chiave master del servizio.

Le informazioni sulla chiave master del database sono visibili nella vista del catalogo sys.symmetric_keys.

Per SQL Server e Parallel Data Warehouse, la chiave master in genere è protetta dalla chiave master del servizio e da almeno una password. Nel caso in cui il database venga fisicamente spostato in un server diverso (log shipping, ripristino di backup e così via), il database conterrà una copia della chiave master crittografata dalla chiave master del servizio server originale (a meno che la crittografia non sia stata rimossa in modo esplicito tramite `ALTER MASTER KEY DDL`) e una copia crittografata da ogni password specificata durante le operazioni `CREATE MASTER KEY` o `ALTER MASTER KEY DDL` successive. Per ripristinare la chiave master e tutti i dati crittografati usando la chiave master come radice della gerarchia chiavi dopo lo spostamento del database, l'utente dovrà usare l'istruzione `OPEN MASTER KEY` con una delle password usate per proteggere la chiave master, ripristinare un backup della chiave master o ripristinare un backup della chiave master originale del servizio nel nuovo server.

Per [!INCLUDE[ssSDS](../../includes/sssds-md.md)] e [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)], la protezione con password non viene considerata come un meccanismo di sicurezza per evitare uno scenario di perdita di dati in situazioni in cui il database potrebbe essere spostato da un server a un altro, poiché la protezione della chiave master del servizio è gestita dalla piattaforma Microsoft Azure. La password della chiave master è quindi facoltativa in [!INCLUDE[ssSDS](../../includes/sssds-md.md)] e [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)].

> [!IMPORTANT]
> È consigliabile eseguire il backup della chiave master usando l'istruzione [BACKUP MASTER KEY](../../t-sql/statements/backup-master-key-transact-sql.md) e archiviarlo in un percorso esterno sicuro.

Le chiavi master del servizio e del database vengono protette mediante l'algoritmo AES-256.

## <a name="permissions"></a>Autorizzazioni

È richiesta l'autorizzazione CONTROL per il database.

## <a name="examples"></a>Esempi

Usare l'esempio seguente per creare una chiave master del database nel database `master`. La chiave viene crittografata con la password `23987hxJ#KL95234nl0zBe`.

```sql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '23987hxJ#KL95234nl0zBe';
GO
```

## <a name="see-also"></a>Vedere anche

- [sys.symmetric_keys &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)
- [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [OPEN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/open-master-key-transact-sql.md)
- [ALTER MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-master-key-transact-sql.md)
- [DROP MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-master-key-transact-sql.md)
- [CLOSE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/close-master-key-transact-sql.md)
- [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)
