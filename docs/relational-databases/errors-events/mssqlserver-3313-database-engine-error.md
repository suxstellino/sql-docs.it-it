---
description: MSSQLSERVER_3313
title: MSSQLSERVER_3313 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3313 (Database Engine error)
ms.assetid: a244227b-8553-42df-9435-034f906c4c74
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 54d19e8b37d0cf4154bb14b3909c1af60afb0743
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208388"
---
# <a name="mssqlserver_3313"></a>MSSQLSERVER_3313
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|3313|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|ERR_LOG_RID1|  
|Testo del messaggio|Durante il rollforward di un'operazione registrata nel database '%.*ls' si è verificato un errore in corrispondenza dell'ID del record di log ID %S_LSN. L'errore specifico viene in genere registrato in precedenza come errore nel registro eventi di Windows. Ripristinare il database da un backup completo oppure correggere il database.|  
  
## <a name="explanation"></a>Spiegazione  
Si tratta di un errore di rollup per il recupero del rollforward. Tale errore ha determinato l'impostazione del database sullo stato SUSPECT. Il filegroup primario, e probabilmente altri filegroup, sono sospetti e possono essere danneggiati. Non è possibile recuperare il database durante l'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , pertanto non è disponibile. Per risolvere il problema, è necessario l'intervento dell'utente.  
  
Se questo errore si verifica per **tempdb**, l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene arrestata.  
  
## <a name="user-action"></a>Azione dell'utente  
Questo errore può essere causato da una condizione temporanea già presente nel sistema durante un determinato tentativo di avvio dell'istanza del server o di recupero di un database. Può inoltre essere causato da un errore permanente che si verifica ogni volta che si tenta di avviare il database. Per informazioni sulla causa, esaminare il registro eventi di Windows per cercare un errore precedente che indichi l'errore specifico.  
  
Quando viene rilevata questa condizione di errore, normalmente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera tre file nella cartella **LOG** di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Nel file SQLDump *nnnn*.txt sono contenute informazioni diagnostiche avanzate relative agli errori, inclusi i dettagli sulla transazione e la pagina in cui si è verificato il problema. In genere, tali informazioni vengono utilizzate dal team del Servizio Supporto Tecnico Clienti per analizzare il motivo dell'errore.  
  
Per informazioni sulla causa di questa occorrenza dell'errore 3313, esaminare il registro eventi di Windows per cercare un errore precedente che indichi l'errore specifico. L'azione appropriata dell'utente varia a seconda che le informazioni nel registro eventi di Windows indichino che l'errore [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è stato causato da una condizione temporanea o da un errore permanente. Per informazioni sulle azioni eseguibili dall'utente per risolvere l'errore 3313, vedere la documentazione online di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>Vedere anche  
[ALTER DATABASE &#40;Transact-SQL&#41;](~/t-sql/statements/alter-database-transact-sql-set-options.md)  
[DBCC CHECKDB &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[Ripristini di database completi &#40;modello di recupero con registrazione minima&#41;](~/relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)  
[MSSQLSERVER_824](~/relational-databases/errors-events/mssqlserver-824-database-engine-error.md)  
[sys.databases &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
  
