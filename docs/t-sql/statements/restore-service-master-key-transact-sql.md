---
description: RESTORE SERVICE MASTER KEY (Transact-SQL)
title: RESTORE SERVICE MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RESTORE SERVICE MASTER KEY
- RESTORE_SERVICE_MASTER_KEY_TSQL
- LOAD SERVICE MASTER KEY
- LOAD_SERVICE_MASTER_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- importing Service Master Keys
- copying Service Master Keys
- service master key [SQL Server], importing
- RESTORE SERVICE MASTER KEY statement
- transferring Service Master Keys
ms.assetid: a68fd0ee-70ce-4104-aca0-fcae5f41fc38
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 3954958dceda15a2eb68505ffbe12299b335c491
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184431"
---
# <a name="restore-service-master-key-transact-sql"></a>RESTORE SERVICE MASTER KEY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Importa una chiave master del servizio da un file di backup.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
RESTORE SERVICE MASTER KEY FROM FILE = 'path_to_file'   
    DECRYPTION BY PASSWORD = 'password' [FORCE]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 FILE **='** _path\_to\_file_ **'**  
 Specifica il percorso completo, nome di file incluso, della chiave master del servizio archiviata. *path_to_file* può essere un percorso locale o un percorso UNC di rete.  
  
 PASSWORD **='** _password_ **'**  
 Specifica la password necessaria per decrittografare la chiave master del servizio che verrà importata da un file.  
  
 FORCE  
 Forza la sostituzione della chiave master del servizio, anche a rischio di perdita dei dati.  
  
## <a name="remarks"></a>Commenti  
 Quando si ripristina la chiave master del servizio, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono decrittografati tutti i segreti e tutte le chiavi crittografate con la chiave master del servizio corrente. Tali elementi vengono poi crittografati nuovamente con la chiave master del servizio caricata dal file di backup.  
  
 In caso di esito negativo di una qualsiasi delle operazioni di decrittografia, il ripristino avrà esito negativo. È possibile utilizzare l'opzione FORCE per ignorare eventuali errori, ma in questo caso andranno perduti tutti i dati che non possono essere decrittografati.  
  
> [!CAUTION]  
>  La chiave master del servizio è l'elemento radice della gerarchia di crittografia di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Con la chiave master del servizio vengono protette direttamente o indirettamente tutte le altre chiavi nell'albero. Se non è possibile decrittografare una chiave dipendente durante un ripristino forzato, i dati protetti da tale chiave andranno perduti.  
  
 La rigenerazione della gerarchia di crittografia è un'operazione che utilizza molte risorse e pertanto dovrebbe essere pianificata in periodi di carico ridotto.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL SERVER per il server.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene ripristinata la chiave master del servizio da un file di backup.  
  
```sql  
RESTORE SERVICE MASTER KEY   
    FROM FILE = 'c:\temp_backups\keys\service_master_key'   
    DECRYPTION BY PASSWORD = '3dH85Hhk003GHk2597gheij4';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Chiave master del servizio](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
 [ALTER SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-service-master-key-transact-sql.md)   
 [BACKUP SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/backup-service-master-key-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)