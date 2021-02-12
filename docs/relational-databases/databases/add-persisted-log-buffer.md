---
description: Aggiungere un buffer di log persistente a un database
title: Aggiungere un buffer di log persistente a un database
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- PMEM
- persistent memory
- persisted log buffer
- add log file
- create log buffer
- remove log buffer
ms.assetid: 8ead516a-1334-4f40-84b2-509d0a8ffa45
author: briancarrig
ms.author: brcarrig
manager: amitban
ms.openlocfilehash: ab2554561abb1590db33249a0ade4e8458b0fd54
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272372"
---
# <a name="add-persisted-log-buffer-to-a-database"></a>Aggiungere un buffer di log persistente a un database
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Questo argomento descrive come aggiungere un buffer di log persistente a un database in [!INCLUDE[sqlv15](../../includes/sssql19-md.md)] usando [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="permissions"></a>Autorizzazioni

È richiesta l'autorizzazione ALTER per il database.  

## <a name="configure-persistent-memory-device-linux"></a>Configurare un dispositivo con memoria persistente (Linux)

Per configurare un dispositivo con memoria persistente in Linux, vedere [questo articolo](../../linux/sql-server-linux-configure-pmem.md).

## <a name="configure-persistent-memory-device-windows"></a>Configurare un dispositivo con memoria persistente (Windows)

Per configurare un dispositivo con memoria persistente in Windows, vedere [questo articolo](/windows-server/storage/storage-spaces/deploy-pmem/).
  
## <a name="add-a-persisted-log-buffer-to-a-database"></a>Aggiungere un buffer di log persistente a un database  

Nell'esempio seguente viene aggiunto un buffer di log persistente.

```sql
ALTER DATABASE <MyDB> 
  ADD LOG FILE 
  (
    NAME = <DAXlog>, 
    FILENAME = '<Filepath to DAX Log File>', 
    SIZE = 20MB
  );
```

Il volume o il montaggio del nuovo file di log deve essere formattato con DAX (NTFS) o montato con l'opzione DAX (XFS/EXT4).

## <a name="remove-a-persisted-log-buffer"></a>Rimuovere un buffer di log persistente

Per rimuovere in modo sicuro un buffer di log persistente, è necessario che il database sia impostato in modalità utente singolo per consentire lo svuotamento del buffer.

Nell'esempio seguente viene rimosso un buffer di log persistente.

```sql
ALTER DATABASE <MyDB> SET SINGLE_USER;
ALTER DATABASE <MyDB> REMOVE FILE <DAXlog>;
ALTER DATABASE <MyDB> SET MULTI_USER;
```

## <a name="limitations"></a>Limitazioni

La crittografia [TDE (Transparent Data Encryption)](../security/encryption/transparent-data-encryption.md) non è compatibile con il buffer di log persistente.

I [gruppi di disponibilità](../../t-sql/statements/create-availability-group-transact-sql.md) possono usare questa funzionalità solo nelle repliche secondarie a causa della necessità di una normale semantica di scrittura del log nel database primario. È tuttavia necessario creare il file di log di piccole dimensioni in tutti i nodi (preferibilmente su volumi o montaggi DAX).

## <a name="backup-and-restore-operations"></a>Operazioni di backup e ripristino

Si applicano le normali condizioni di ripristino. Se il buffer di log persistente viene ripristinato in un volume o un montaggio DAX, continuerà a funzionare. In caso contrario, può essere rimosso senza alcun rischio.
  
## <a name="next-steps"></a>Passaggi successivi

- [Funzionamento (è solo più veloce): Memorizzazione su moduli NVDIMM della coda del log di SQL Server su memoria non volatile](/archive/blogs/bobsql/how-it-works-it-just-runs-faster-non-volatile-memory-sql-server-tail-of-log-caching-on-nvdimm)
- [Dati esposti: Latenza e durabilità con SQL Server 2016](https://channel9.msdn.com/Shows/Data-Exposed/Latency-and-Durability-with-SQL-Server-2016)
- [Accelerazione della latenza di commit delle transazioni con Storage Class Memory in Windows Server 2016/SQL Server 2016 SP1](/archive/blogs/sqlserverstorageengine/transaction-commit-latency-acceleration-using-storage-class-memory-in-windows-server-2016sql-server-2016-sp1)