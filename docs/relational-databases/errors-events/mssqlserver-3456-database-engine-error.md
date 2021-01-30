---
description: MSSQLSERVER_3456
title: MSSQLSERVER_3456 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3456 (Database Engine error)
ms.assetid: d11b2b2c-3ae4-4023-b82f-05b561bfacce
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a4809d1ba5e22e8e8d9a9f0d574f2690731e62a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201573"
---
# <a name="mssqlserver_3456"></a>MSSQLSERVER_3456
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|3456|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|REC_REDOLSNMISMATCH|  
|Testo del messaggio|Impossibile eseguire il rollforward del record di log %S_LSN per l'ID di transazione %S_XID, pagina %S_PGID, database '%.*ls' (ID di database %d). Pagina: LSN = %S_LSN, tipo = %ld. Log: OpCode = %ld, contesto=%d, PrevPageLSN: %S_LSN. Correggere il database oppure ripristinarlo da un backup.|  
  
## <a name="explanation"></a>Spiegazione  
Durante l'operazione di ripristino non è stato possibile eseguire il rollforward del log delle transazioni. Tale errore ha determinato l'impostazione del database sullo stato SUSPECT. Il filegroup primario, e probabilmente altri filegroup, sono sospetti e possono essere danneggiati. Non è possibile recuperare il database durante l'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , pertanto non è disponibile. Per risolvere il problema, è necessario l'intervento dell'utente.  
  
Se questo errore si verifica per **tempdb**, l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene arrestata.  
  
## <a name="user-action"></a>Azione dell'utente  
Questo errore può essere causato da una condizione temporanea già presente nel sistema durante un determinato tentativo di avvio dell'istanza del server o di recupero di un database. Può inoltre essere causato da un errore permanente che si verifica ogni volta che si tenta di avviare il database. Per informazioni sulla causa, esaminare il registro eventi di Windows per cercare un errore precedente che indichi l'errore specifico.  
  
Per informazioni sulla causa di questa occorrenza dell'errore 3456, esaminare il registro eventi di Windows per cercare un errore precedente che indichi l'errore specifico. L'azione appropriata dell'utente varia a seconda che le informazioni nel registro eventi di Windows indichino che l'errore [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è stato causato da una condizione temporanea o da un errore permanente. Per informazioni sulle azioni eseguibili dall'utente per risolvere l'errore 3456, vedere la documentazione online di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="see-also"></a>Vedere anche  
[ALTER DATABASE &#40;Transact-SQL&#41;](~/t-sql/statements/alter-database-transact-sql-set-options.md)  
[DBCC CHECKDB &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[Ripristini di database completi &#40;modello di recupero con registrazione minima&#41;](~/relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)  
[MSSQLSERVER_824](~/relational-databases/errors-events/mssqlserver-824-database-engine-error.md)  
[sys.databases &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
  
