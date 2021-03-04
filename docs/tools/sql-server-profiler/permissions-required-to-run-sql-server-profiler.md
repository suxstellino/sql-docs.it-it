---
title: Autorizzazioni necessarie
titleSuffix: SQL Server Profiler
description: Informazioni sulle autorizzazioni necessarie per eseguire SQL Server Profiler e riprodurre le tracce e sui controlli eseguiti durante le riproduzioni.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: profiler
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 4c264805e9733f4842f97d945a4807865c8a535a
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839434"
---
# <a name="permissions-required-to-run-sql-server-profiler"></a>Autorizzazioni necessarie per l'esecuzione di SQL Server Profiler

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Per impostazione predefinita, l'esecuzione di [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] richiede le stesse autorizzazioni utente necessarie per le stored procedure Transact-SQL utilizzate per la creazione di tracce. Per eseguire [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], è necessario che agli utenti venga concessa l'autorizzazione ALTER TRACE. Per altre informazioni, vedere [GRANT - autorizzazioni per server &#40;Transact-SQL&#41;](../../t-sql/statements/grant-server-permissions-transact-sql.md).

> [!IMPORTANT]
> I piani di query e il testo della query, acquisiti dalla traccia SQL e con altri mezzi, ad esempio, viste a gestione dinamica e funzioni (DMV, DMF), eventi estesi possono contenere informazioni riservate. Pertanto, le autorizzazioni ALTER TRACE, SHOWPLAN e lo stato del SERVER di visualizzazione delle autorizzazioni di copertura devono essere concesse solo agli utenti che ne hanno bisogno per soddisfare le proprie funzioni di lavoro, in base al principio del privilegio minimo.
>
> È inoltre consigliabile salvare file Showplan o file di traccia che contengono eventi correlati a Showplan in un percorso che utilizza il file system NTFS e limitare l'accesso agli utenti autorizzati a visualizzare informazioni potenzialmente riservate.

> [!IMPORTANT]
> Traccia SQL e [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] sono deprecati. Anche lo spazio dei nomi *Microsoft.SqlServer.Management.Trace* che contiene gli oggetti Trace e Replay di Microsoft SQL Server è deprecato.
> [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]
> In alternativa, usare Eventi estesi. Per altre informazioni sugli [Eventi estesi](../../relational-databases/extended-events/extended-events.md), vedere [Avvio rapido: Eventi estesi in SQL Server](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md) e [Profiler XEvent di SQL Server Management Studio](../../relational-databases/extended-events/use-the-ssms-xe-profiler.md).

> [!NOTE]
> Sono supportati i carichi di lavoro [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] per Analysis Services.

> [!NOTE]
> Quando si tenta di connettersi a un database di SQL Azure da SQL Server Profiler, viene erroneamente generato un messaggio di errore fuorviante come segue:
>
> - Per eseguire una traccia su SQL Server, è necessario essere un membro del ruolo predefinito del server sysadmin o disporre dell'autorizzazione ALTER TRACE.
>
> Il messaggio dovrebbe spiegare che il database SQL di Azure non è supportato da SQL Server Profiler.

## <a name="permissions-used-to-replay-traces"></a>Autorizzazioni per la riproduzione di tracce  
Per riprodurre le tracce, è necessario che l'utente che desidera eseguire questa operazione disponga dell'autorizzazione ALTER TRACE.  

Durante la riproduzione, in [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] viene tuttavia utilizzato il comando EXECUTE AS, se nella traccia riprodotta viene rilevato un evento Audit Login. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] usa il comando EXECUTE AS per rappresentare l'utente associato all'evento di accesso.  

Se [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] rileva un evento di accesso in una traccia che si desidera riprodurre, verranno eseguite le verifiche delle autorizzazioni seguenti:

1. Utente1, che dispone dell'autorizzazione ALTER TRACE, inizia la riproduzione di una traccia.

2. Nella traccia riprodotta viene rilevato un evento di accesso per Utente2.

3. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] usa il comando EXECUTE AS per rappresentare Utente2.

4. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prova a eseguire l'autenticazione di Utente2 e, in base ai risultati, si verifica una delle situazioni seguenti:

    1. Se non è possibile eseguire l'autenticazione di Utente2, [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] restituisce un errore e continua la riproduzione della traccia come Utente1.
  
    2. Se l'autenticazione di Utente2 viene eseguita, la riproduzione della traccia continua come Utente2.
  
5. Vengono controllate le autorizzazioni di Utente2 nel database di destinazione e, in base ai risultati, si può verificare una delle situazioni seguenti:
  
    1. Se Utente2 dispone delle autorizzazioni per il database di destinazione, la rappresentazione ha esito positivo e la riproduzione della traccia viene eseguita come Utente2.
  
    2. Se Utente2 non dispone delle autorizzazioni per il database di destinazione, il server verifica se esiste un utente Guest nel database.

6. Viene verificata l'esistenza di un utente Guest nel database di destinazione e, in base ai risultati, si può verificare una delle situazioni seguenti:
 
    1.  Se esiste un account Guest, la riproduzione della traccia viene eseguita come account Guest.
  
    2.  Se nel database di destinazione non esiste un account Guest, viene restituito un errore e la riproduzione della traccia viene eseguita come Utente1.
 
Nella figura seguente viene illustrato il processo di verifica delle autorizzazioni per la riproduzione delle tracce:

![SQL Server Profiler le autorizzazioni di traccia di riproduzione.](../../tools/sql-server-profiler/media/replaytracedecisiontree.gif)

## <a name="see-also"></a>Vedere anche
- [Stored procedure di SQL Server Profiler &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-profiler-stored-procedures-transact-sql.md)
- [Riprodurre le tracce](../../tools/sql-server-profiler/replay-traces.md)
- [Creare una traccia &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-trace-sql-server-profiler.md)
- [Riprodurre una tabella di traccia &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/replay-a-trace-table-sql-server-profiler.md)
- [Riprodurre un file di traccia &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/replay-a-trace-file-sql-server-profiler.md)
