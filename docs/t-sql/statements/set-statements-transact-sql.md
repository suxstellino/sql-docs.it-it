---
description: Istruzioni SET (Transact-SQL)
title: Istruzioni SET (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET
- SET_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ISO SET statements
- queries [SQL Server], executing
- dates [SQL Server], SET statements
- time [SQL Server], SET statements
- SET statement, about SET statement
- SET statement
- statistical information [SQL Server], SET statements
- locking [SQL Server], SET statements
ms.assetid: f7e107f8-0fcf-408b-b30f-da2323eeb714
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azure-sqldw-latest ||= azuresqldb-current || >= sql-server-2016 || >= sql-server-linux-2017
ms.openlocfilehash: b00353ebc513ecdab6b0be57a64c05f84e1ae461
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189404"
---
# <a name="set-statements-transact-sql"></a>Istruzioni SET (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

Il linguaggio di programmazione [!INCLUDE[tsql](../../includes/tsql-md.md)] offre varie istruzioni SET per la modifica della gestione delle informazioni specifiche della sessione corrente. Le istruzioni SET sono raggruppate in categorie, descritte nella tabella seguente.  
  
Per informazioni sull'impostazione delle variabili locali mediante l'istruzione SET, vedere [SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md).  
  
|Category|Istruzioni|  
|--------------|----------------|  
|Istruzioni relative a data e ora|[SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md)<br /><br /> [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md)|  
|Istruzioni di blocco|[SET DEADLOCK_PRIORITY](../../t-sql/statements/set-deadlock-priority-transact-sql.md)<br /><br /> [SET LOCK_TIMEOUT](../../t-sql/statements/set-lock-timeout-transact-sql.md)|  
|Istruzioni varie|[SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md)<br /><br /> [SET CURSOR_CLOSE_ON_COMMIT](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md)<br /><br /> [SET FIPS_FLAGGER](../../t-sql/statements/set-fips-flagger-transact-sql.md)<br /><br /> [SET IDENTITY_INSERT](../../t-sql/statements/set-identity-insert-transact-sql.md)<br /><br /> [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md)<br /><br /> [SET OFFSETS](../../t-sql/statements/set-offsets-transact-sql.md)<br /><br /> [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md)|  
|Istruzioni per l'esecuzione di query|[SET ARITHABORT](../../t-sql/statements/set-arithabort-transact-sql.md)<br /><br /> [SET ARITHIGNORE](../../t-sql/statements/set-arithignore-transact-sql.md)<br /><br /> [SET FMTONLY](../../t-sql/statements/set-fmtonly-transact-sql.md)<br /> Nota: [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]<br /><br /> [SET NOCOUNT](../../t-sql/statements/set-nocount-transact-sql.md)<br /><br /> [SET NOEXEC](../../t-sql/statements/set-noexec-transact-sql.md)<br /><br /> [SET NUMERIC_ROUNDABORT](../../t-sql/statements/set-numeric-roundabort-transact-sql.md)<br /><br /> [SET PARSEONLY](../../t-sql/statements/set-parseonly-transact-sql.md)<br /><br /> [SET QUERY_GOVERNOR_COST_LIMIT](../../t-sql/statements/set-query-governor-cost-limit-transact-sql.md)<br /><br /> [SET RESULT SET CACHING](../../t-sql/statements/set-result-set-caching-transact-sql.md?view=azure-sqldw-latest&preserve-view=true) (anteprima)<br /> Nota:  Questa funzionalità si applica solo a [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].<br /><br /> [SET ROWCOUNT](../../t-sql/statements/set-rowcount-transact-sql.md)<br /><br /> [SET TEXTSIZE](../../t-sql/statements/set-textsize-transact-sql.md)|  
|Istruzioni per impostazioni ISO|[SET ANSI_DEFAULTS](../../t-sql/statements/set-ansi-defaults-transact-sql.md)<br /><br /> [SET ANSI_NULL_DFLT_OFF](../../t-sql/statements/set-ansi-null-dflt-off-transact-sql.md)<br /><br /> [SET ANSI_NULL_DFLT_ON](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md)<br /><br /> [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md)<br /><br /> [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md)<br /><br /> [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md)|  
|Istruzioni statistiche|[SET FORCEPLAN](../../t-sql/statements/set-forceplan-transact-sql.md)<br /><br /> [SET SHOWPLAN_ALL](../../t-sql/statements/set-showplan-all-transact-sql.md)<br /><br /> [SET SHOWPLAN_TEXT](../../t-sql/statements/set-showplan-text-transact-sql.md)<br /><br /> [SET SHOWPLAN_XML](../../t-sql/statements/set-showplan-xml-transact-sql.md)<br /><br /> [SET STATISTICS IO](../../t-sql/statements/set-statistics-io-transact-sql.md)<br /><br /> [SET STATISTICS XML](../../t-sql/statements/set-statistics-xml-transact-sql.md)<br /><br /> [SET STATISTICS PROFILE](../../t-sql/statements/set-statistics-profile-transact-sql.md)<br /><br /> [SET STATISTICS TIME](../../t-sql/statements/set-statistics-time-transact-sql.md)|  
|Istruzioni per transazioni|[SET IMPLICIT_TRANSACTIONS](../../t-sql/statements/set-implicit-transactions-transact-sql.md)<br /><br /> [SET REMOTE_PROC_TRANSACTIONS](../../t-sql/statements/set-remote-proc-transactions-transact-sql.md)<br /><br /> [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)<br /><br /> [SET XACT_ABORT](../../t-sql/statements/set-xact-abort-transact-sql.md)| 
  
## <a name="considerations-when-you-use-the-set-statements"></a>Considerazioni sull'utilizzo delle istruzioni SET  
  
- Tutte le istruzioni SET vengono eseguite in fase di esecuzione, tranne queste che vengono eseguite in fase di analisi:

  - SET FIPS_FLAGGER
  - SET OFFSETS
  - SET PARSEONLY
  - SET QUOTED_IDENTIFIER  
  
- Se un'istruzione SET viene eseguita in una stored procedure o in un trigger, il valore dell'opzione SET viene ripristinato quando la stored procedure o il trigger restituisce il controllo. Se si specifica un'istruzione SET in una stringa SQL dinamica eseguita con **sp_executesql** o EXECUTE, inoltre, il valore dell'opzione SET viene ripristinato quando il batch indicato nella stringa SQL dinamica restituisce il controllo.  
  
- Le stored procedure vengono eseguite con le impostazioni SET specificate in fase di esecuzione, ad eccezione di SET ANSI_NULLS e SET QUOTED_IDENTIFIER. Le stored procedure che specificano SET ANSI_NULLS o SET QUOTED_IDENTIFIER utilizzano l'impostazione specificata in fase di creazione della stored procedure. Se utilizzate all'interno di una stored procedure, le impostazioni SET vengono ignorate.  
  
- L'impostazione **user options** di **sp_configure** consente di specificare opzioni a livello di server e ha effetto su più database. Questa impostazione si comporta, inoltre, come un'istruzione SET esplicita, con la sola differenza che viene applicata al momento dell'accesso.  
  
- Le impostazioni di database impostate tramite ALTER DATABASE sono valide solo a livello di database e hanno effetto solo se impostate in modo esplicito. Le impostazioni di database prevalgono sulle impostazioni delle opzioni dell'istanza impostate tramite **sp_configure**.  
  
- Se un'istruzione SET usa ON e OFF, è possibile specificare una delle due impostazioni per più opzioni SET.
  
    > [!NOTE]  
    >  Questo non si applica alle opzioni SET correlate a statistiche.
  
     Ad esempio `SET QUOTED_IDENTIFIER, ANSI_NULLS ON` imposta sia QUOTED_IDENTIFIER che ANSI_NULLS su ON.  
  
- Le impostazioni delle istruzioni SET eseguono l'override delle identiche impostazioni delle opzioni di database configurate con ALTER DATABASE. Il valore specificato in un'istruzione SET ANSI_NULLS, ad esempio, prevarrà sull'impostazione di database per ANSI_NULLS. Alcune impostazioni di connessione, inoltre, vengono impostate automaticamente su ON quando un utente si connette a un database in base ai valori applicati con il precedente uso dell'impostazione **user options di sp_configure** o ai valori applicati a tutte le connessioni ODBC e OLE/DB.  
  
- Le istruzioni ALTER, CREATE e DROP DATABASE non rispettano l'impostazione di SET LOCK_TIMEOUT.  
  
- Quando un'istruzione SET globale o di scelta rapida include diverse impostazioni, eseguendola vengono ripristinate le impostazioni precedenti per tutte le opzioni interessate. Se un'opzione SET interessata da un'istruzione SET di scelta rapida viene impostata dopo l'esecuzione di tale istruzione, la singola istruzione SET eseguirà l'override delle impostazioni corrispondenti dell'istruzione di scelta rapida. Un esempio di istruzione SET di scelta rapida è SET ANSI_DEFAULTS.  
  
- Quando si usano batch, il contesto di database è determinato dal batch stabilito con l'istruzione USE. Le query non pianificate e tutte le altre istruzioni all'esterno della stored procedure e incluse in batch ereditano le impostazioni delle opzioni del database e della connessione stabilite con l'istruzione USE.  
  
- Le richieste MARS (Multiple Active Result Set) condividono uno stato globale che contiene le impostazioni delle opzioni SET della sessione più recente. Quando viene eseguita, ogni richiesta può modificare le opzioni SET. Le modifiche sono specifiche del contesto della richiesta in cui vengono impostate e non influiscono sulle altre richieste MARS simultanee. Al termine dell'esecuzione della richiesta, tuttavia, le nuove opzioni SET vengono copiate nello stato della sessione globale. Le nuove richieste che vengono eseguite nella stessa sessione dopo questa modifica utilizzeranno queste nuove impostazioni delle opzioni SET.  
  
- Una stored procedure eseguita da un batch o da un'altra stored procedure viene eseguita in base ai valori delle opzioni configurati nel database contenente la stored procedure. Se la stored procedure **db1.dbo.sp1** chiama la stored procedure **db2.dbo.sp2**, ad esempio, la stored procedure **sp1** viene eseguita con l'impostazione corrente del livello di compatibilità del database **db1** e la stored procedure **sp2** viene eseguita con l'impostazione corrente del livello di compatibilità del database **db2**.  
  
- Quando un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] riguarda oggetti che si trovano in più database, a tale istruzione si applicano il contesto di database e il contesto di connessione correnti. In questo caso, se l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] si trova in un batch, il contesto della connessione corrente è il database definito dall'istruzione USE. Se l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] si trova in una stored procedure, il contesto della connessione è il database che contiene la stored procedure.  
  
- Quando si creano e si modificano indici su colonne calcolate o viste indicizzate, è necessario impostare le opzioni SET seguenti su ON: ARITHABORT, CONCAT_NULL_YIELDS_NULL, QUOTED_IDENTIFIER, ANSI_NULLS, ANSI_PADDING e ANSI_WARNINGS. Impostare l'opzione NUMERIC_ROUNDABORT su OFF.  
  
  Se una di queste opzioni non viene impostata sui valori obbligatori, le azioni INSERT, UPDATE, DELETE, DBCC CHECKDB e DBCC CHECKTABLE sulle viste indicizzate o sulle tabelle con indici su colonne calcolate avranno esito negativo. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verrà generato un avviso contenente tutte le opzioni impostate in modo errato. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] elaborerà inoltre le istruzioni SELECT su tali tabelle o viste indicizzate come se gli indici sulle colonne calcolate o sulle viste non esistessero. 

- Quando l'istruzione SET RESULT_SET_CACHING è impostata su ON, la funzionalità di memorizzazione nella cache dei risultati per la sessione client corrente è abilitata.   Non è possibile impostare RESULT_SET_CACHING su ON per una sessione se è impostata su OFF a livello del database.    Quando l'istruzione SET RESULT_SET_CACHING è impostata su OFF, la funzionalità di memorizzazione nella cache dei set di risultati per la sessione client corrente è disabilitata. Per modificare questa impostazione è richiesta l'appartenenza al ruolo public.
Si applica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] Gen2
