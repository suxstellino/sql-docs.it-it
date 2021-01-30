---
description: RESTORE MASTER KEY (Transact-SQL)
title: RESTORE MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RESTORE_MASTER_KEY_TSQL
- RESTORE MASTER KEY
- LOAD_MASTER_KEY_TSQL
- LOAD MASTER KEY
dev_langs:
- TSQL
helpviewer_keywords:
- database master key [SQL Server], importing
- encryption [SQL Server], Database Master Key
- copying Database Master Keys
- importing Database Master Keys
- cryptography [SQL Server], Database Master Key
- transferring Database Master Keys
- RESTORE MASTER KEY statement
ms.assetid: 70ceb951-31a2-4fc4-a0c1-e6c18eeb3ae7
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 66ba1a2950f25ad1e0ed5e189c091ed6074f1b4d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180111"
---
# <a name="restore-master-key-transact-sql"></a>RESTORE MASTER KEY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Importa una chiave master del database da un file di backup.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
RESTORE MASTER KEY FROM FILE = 'path_to_file'   
    DECRYPTION BY PASSWORD = 'password'  
    ENCRYPTION BY PASSWORD = 'password'  
    [ FORCE ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 FILE ='*path_to_file*'  
 Specifica il percorso completo, nome file incluso, della chiave master del database archiviata. *path_to_file* può essere un percorso locale o un percorso UNC di rete.  
  
 DECRYPTION BY PASSWORD ='*password*'  
 Specifica la password necessaria per decrittografare la chiave master del database che verrà importata da un file.  
  
 ENCRYPTION BY PASSWORD ='*password*'  
 Specifica la password utilizzata per crittografare la chiave master del database dopo averla caricata nel database.  
  
 FORCE  
 Specifica che il processo RESTORE deve continuare anche se la chiave master del database corrente non viene aperta oppure se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è in grado di decrittografare alcune delle chiavi private crittografate con la chiave master.  
  
## <a name="remarks"></a>Commenti  
 Quando si ripristina la chiave master, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono decrittografate tutte le chiavi crittografate con la chiave master attiva corrente. Tali elementi venogno poi crittografati nuovamente con la chiave master ripristinata. Si tratta di un'operazione che utilizza molte risorse e pertanto dovrebbe essere pianificata in periodi di carico ridotto. L'operazione di ripristino avrà esito negativo se la chiave master del database corrente non è aperta e non è possibile aprirla, oppure se non è possibile decrittografare le eventuali chiavi crittografate con tale chiave master.  
  
 Utilizzare l'opzione FORCE solo se la chiave master è irrecuperabile o se la decrittografia ha esito negativo. Le informazioni crittografate esclusivamente da una chiave irrecuperabile andranno perdute.  
  
 Se la chiave master è stata crittografata con la chiave master del servizio, anche la chiave master ripristinata verrà crittografata con la chiave master del servizio.  
  
 Se il database corrente non include alcuna chiave master, con l'esecuzione di RESTORE MASTER KEY verrà creata una chiave master. La nuova chiave master non verrà crittografata automaticamente con la chiave master del servizio.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione CONTROL per il database.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene ripristinata la chiave master del database `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
RESTORE MASTER KEY   
    FROM FILE = 'c:\backups\keys\AdventureWorks2012_master_key'   
    DECRYPTION BY PASSWORD = '3dH85Hhk003#GHkf02597gheij04'   
    ENCRYPTION BY PASSWORD = '259087M#MyjkFkjhywiyedfgGDFD';  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md)   
 [ALTER MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-master-key-transact-sql.md)   
 [Gerarchia di crittografia](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
