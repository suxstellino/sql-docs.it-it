---
description: sp_trace_create (Transact-SQL)
title: sp_trace_create (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_trace_create_TSQL
- sp_trace_create
dev_langs:
- TSQL
helpviewer_keywords:
- sp_trace_create
ms.assetid: f3a43597-4c5a-4520-bcab-becdbbf81d2e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c63a724ad0b82a1a11145cc559ac52fc2d6f9cc3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200619"
---
# <a name="sp_trace_create-transact-sql"></a>sp_trace_create (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea la definizione di una nuova traccia nello stato arrestato.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare Eventi estesi.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_trace_create [ @traceid = ] trace_id OUTPUT   
          , [ @options = ] option_value   
          , [ @tracefile = ] 'trace_file'   
     [ , [ @maxfilesize = ] max_file_size ]  
     [ , [ @stoptime = ] 'stop_time' ]  
     [ , [ @filecount = ] 'max_rollover_files' ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @traceid = ] trace_id` Numero assegnato da [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alla nuova traccia. Qualsiasi input dell'utente verrà ignorato. *trace_id* è di **tipo int** e il valore predefinito è null. L'utente utilizza il valore *trace_id* per identificare, modificare e controllare la traccia definita da questo stored procedure.  
  
`[ @options = ] option_value` Specifica le opzioni impostate per la traccia. *option_value* è di **tipo int** e non prevede alcun valore predefinito. Gli utenti possono impostare una combinazione di queste opzioni specificando la somma dei valori delle opzioni scelte. Ad esempio, per attivare entrambe le opzioni TRACE_FILE_ROLLOVER e SHUTDOWN_ON_ERROR, specificare **6** per *option_value*.  
  
 Nella tabella seguente sono incluse le opzioni e le descrizioni, accompagnate dai relativi valori.  
  
|Nome opzione|Valore opzione|Descrizione|  
|-----------------|------------------|-----------------|  
|TRACE_FILE_ROLLOVER|**2**|Specifica che quando viene raggiunto il *max_file_size* , il file di traccia corrente viene chiuso e viene creato un nuovo file. in cui verranno scritti tutti i nuovi record. Il nome del nuovo file è uguale a quello del file precedente, ma è seguito da un numero intero a indicare la sequenza. Se, ad esempio, il nome del file di traccia originale è nomefile.trc, i successivi file di traccia verranno denominati nomefile_1.trc, nomefile_2.trc e così via.<br /><br /> Man mano che vengono creati nuovi file di traccia di rollover, viene incrementato in modo sequenziale il numero aggiunto nel nome del file.<br /><br /> SQL Server usa il valore predefinito di *max_file_size* (5 MB) se questa opzione viene specificata senza specificare un valore per *max_file_size*.|  
|SHUTDOWN_ON_ERROR|**4**|Specifica che se non è possibile scrivere nel file per un qualsiasi motivo, SQL Server viene arrestato. Questa opzione risulta utile quando si eseguono tracce di controllo della sicurezza.|  
|TRACE_PRODUCE_BLACKBOX|**8**|Specifica che il server salverà un record contenente gli ultimi 5 MB di informazioni di traccia generate dal server. TRACE_PRODUCE_BLACKBOX è incompatibile con tutte le altre opzioni.|  
  
`[ @tracefile = ] 'trace_file'` Specifica il percorso e il nome file in cui verrà scritta la traccia. *trace_file* è di **tipo nvarchar (245)** e non prevede alcun valore predefinito. *trace_file* può essere una directory locale (ad esempio n'c:\MSSQL\Trace\traccia.trc ') o un percorso UNC per una condivisione o un percorso (n' \\ \\ *nomeserver* \\ *ShareName* \\ *directory*\Trace.trc ').  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aggiungerà un'estensione **TRC** a tutti i nomi dei file di traccia. Se si specificano l'opzione TRACE_FILE_ROLLOVER e un *max_file_size* , [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in viene creato un nuovo file di traccia quando le dimensioni massime del file di traccia originale aumentano. Il nuovo file ha lo stesso nome del file originale, ma viene aggiunto _ *n* per indicare la sequenza, a partire da **1**. Se, ad esempio, il primo file di traccia è denominato **filename. trc**, il secondo file di traccia è denominato **filename_1. trc**.  
  
 Se si utilizza l'opzione TRACE_FILE_ROLLOVER, si consiglia di non utilizzare caratteri di sottolineatura nel nome del file di traccia originale. Se vengono utilizzati caratteri di sottolineatura, si verifica il comportamento seguente:  
  
-   [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] non carica automaticamente o richiede di caricare i file di rollover (se una di queste opzioni di rollover dei file è configurata).  
  
-   La funzione fn_trace_gettable non carica i file di rollover (se specificati tramite l'argomento *number_files* ) in cui il nome del file originale termina con un carattere di sottolineatura e un valore numerico. Ciò non vale per il carattere di sottolineatura e il numero che vengono aggiunti automaticamente all'esecuzione del rollover di un file.  
  
> [!NOTE]  
>  Come soluzione alternativa a entrambi questi comportamenti, è possibile rinominare i file in modo da rimuovere i caratteri di sottolineatura nel nome del file originale. Ad esempio, se il file originale è denominato **my_trace. trc** e il file di rollover è denominato **my_trace_1. trc**, è possibile rinominare i file in **Trace. trc** e **Mytrace_1. trc** prima di aprire i file in [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] .  
  
 non è possibile specificare *trace_file* quando viene utilizzata l'opzione TRACE_PRODUCE_BLACKBOX.  
  
`[ @maxfilesize = ] max_file_size` Specifica la dimensione massima in megabyte (MB) che un file di traccia può aumentare. *max_file_size* è di tipo **bigint** e il valore predefinito è **5**.  
  
 Se questo parametro viene specificato senza l'opzione TRACE_FILE_ROLLOVER, la traccia arresta la registrazione nel file quando lo spazio su disco utilizzato supera la quantità specificata da *max_file_size*.  
  
`[ @stoptime = ] 'stop_time'` Specifica la data e l'ora in cui la traccia verrà arrestata. *stop_time* è di tipo **DateTime** e il valore predefinito è null. Se il valore è NULL, la traccia viene eseguita fino a quando non viene arrestata in modo manuale o fino all'arresto del server.  
  
 Se vengono specificati sia *stop_time* che *max_file_size* e TRACE_FILE_ROLLOVER non è specificato, la traccia viene ripartita quando viene raggiunta l'ora di arresto specificata o la dimensione massima del file. Se vengono specificati *stop_time*, *max_file_size* e TRACE_FILE_ROLLOVER, la traccia viene arrestata in corrispondenza dell'ora di arresto specificata, presupponendo che la traccia non riempia l'unità.  
  
`[ @filecount = ] 'max_rollover_files'` Specifica il numero massimo di file di traccia da mantenere con lo stesso nome file di base. *MAX_ROLLOVER_FILES* è di **tipo int**, maggiore di uno. Questo parametro è valido solo se è specificata l'opzione TRACE_FILE_ROLLOVER. Quando si specifica *MAX_ROLLOVER_FILES* , [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tenta di gestire non più di *MAX_ROLLOVER_FILES* file di traccia eliminando il file di traccia meno recente prima di aprire un nuovo file di traccia. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene tenuto traccia dell'età dei file di traccia aggiungendo un numero al nome del file di base.  
  
 Ad esempio, quando il parametro *trace_file* viene specificato come "c:\mytrace", un file denominato "c:\ mytrace_123. trc" è più vecchio di un file denominato "c:\ mytrace_124. trc". Se *MAX_ROLLOVER_FILES* è impostato su 2, SQL Server Elimina il file "c:\ mytrace_123. trc" prima di creare il file di traccia "c:\ mytrace_125. trc".  
  
 Si noti che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tenta di eliminare un file una sola volta e non può eliminare un file se questo è utilizzato da un altro processo. Se, pertanto, un'altra applicazione sta utilizzando i file di traccia mentre è in esecuzione la traccia, è possibile che tali file vengano lasciati nel file system da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nella tabella seguente vengono descritti i possibili valori di codice visualizzati al completamento della stored procedure.  
  
|Codice restituito|Descrizione|  
|-----------------|-----------------|  
|0|Nessun errore.|  
|1|Errore sconosciuto.|  
|10|Opzioni non valide. Restituito quando le opzioni specificate sono incompatibili.|  
|12|Creazione del file non riuscita.|  
|13|Memoria insufficiente. Restituito quando la quantità di memoria disponibile non è sufficiente per eseguire l'azione specificata.|  
|14|Ora di arresto non valida. Restituito quando l'ora specificata è già trascorsa.|  
|15|Parametri non validi. Restituito quando l'utente specifica parametri incompatibili.|  
  
## <a name="remarks"></a>Commenti  
 **sp_trace_create** è un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stored procedure che esegue molte delle azioni eseguite in precedenza da **xp_trace_ \* *_ stored procedure estese disponibili nelle versioni precedenti di SQL Server. Usare _* sp_trace_create** invece di:  
  
-   **xp_trace_addnewqueue**  
  
-   **xp_trace_setqueuecreateinfo**  
  
-   **xp_trace_setqueuedestination**  
  
 **sp_trace_create** crea solo una definizione di traccia. non per avviare o modificare una traccia.  
  
 I parametri di tutte le stored procedure di traccia SQL (**sp_trace_xx**) sono fortemente tipizzati. Se questi parametri non vengono chiamati con i tipi di dati corretti per i parametri di input, come indicato nella descrizione dell'argomento, la stored procedure restituirà un errore.  
  
 Per **sp_trace_create**, l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] account del servizio deve disporre dell'autorizzazione di scrittura per la cartella dei file di traccia. Se l'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è un amministratore nel computer in cui si trova il file di traccia, è necessario concedere esplicitamente l'autorizzazione di scrittura all'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  È possibile caricare automaticamente il file di traccia creato con **sp_trace_create** in una tabella utilizzando la funzione di sistema **fn_trace_gettable** . Per informazioni sull'utilizzo di questa funzione di sistema, vedere [sys.fn_trace_gettable &#40;&#41;Transact-SQL ](../../relational-databases/system-functions/sys-fn-trace-gettable-transact-sql.md).  
  
 Per un esempio dell'uso di stored procedure relative alla traccia, vedere [Creare una traccia &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md).  
  
 **Trace_produce_blackbox** presenta le caratteristiche seguenti:  
  
-   È costituito da una traccia di rollover. Il *file_count* predefinito è 2, ma può essere sostituito dall'utente tramite l'opzione *FileCount* .  
  
-   Il *file_size* predefinito come con altre tracce è 5 MB e può essere modificato.  
  
-   Non è possibile specificare alcun nome di file. Il file verrà salvato come: **N'%SQLDIR%\MSSQL\DATA\blackbox.trc '**  
  
-   Nella traccia saranno contenuti solo gli eventi seguenti e le relative colonne:  
  
    -   **Avvio RPC**  
  
    -   **Avvio batch**  
  
    -   **Eccezione**  
  
    -   **Attenzione**  
  
-   Non è possibile aggiungere o rimuovere eventi o colonne da questa traccia.  
  
-   Non è possibile specificare i filtri per questa traccia.  
  
## <a name="permissions"></a>Autorizzazioni  
 L'utente deve disporre delle autorizzazioni ALTER TRACE.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_trace_generateevent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setfilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)   
 [sp_trace_setstatus &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setstatus-transact-sql.md)   
 [Traccia SQL](../../relational-databases/sql-trace/sql-trace.md)  
  
  
