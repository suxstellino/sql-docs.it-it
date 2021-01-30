---
description: WAITFOR (Transact-SQL)
title: WAITFOR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WAITFOR
- WAITFOR_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- TIME option
- delaying executions [SQL Server]
- batches [SQL Server], execution times
- stored procedures [SQL Server], executing
- DELAY
- time [SQL Server], execution delays
- blocking executions
- transactions [SQL Server], execution times
- WAITFOR statement
- timing executions
ms.assetid: 8e896e73-af27-4cae-a725-7a156733f3bd
author: cawrites
ms.author: chadam
ms.openlocfilehash: 51cd5038d472ae4220d15673036dd46295887543
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211061"
---
# <a name="waitfor-transact-sql"></a>WAITFOR (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Blocca l'esecuzione di un batch, una stored procedure o una transazione fino all'ora specificata o fino a quando non è trascorso l'intervallo di tempo specificato oppure finché un'istruzione specificata non modifica o restituisce almeno una riga.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
WAITFOR   
{  
    DELAY 'time_to_pass'   
  | TIME 'time_to_execute'   
  | [ ( receive_statement ) | ( get_conversation_group_statement ) ]   
    [ , TIMEOUT timeout ]  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 DELAY  
 Periodo di tempo specificato che deve trascorrere (massimo 24 ore) prima che l'esecuzione di un batch, una stored procedure o una transazione possa continuare.  
  
 '*time_to_pass*'  
 Periodo di tempo di attesa. *time_to_pass* può essere specificato in un formato dati **datetime** o come variabile locale. Non è possibile specificare date, pertanto la parte relativa alla data del valore **datetime** non è consentita. *time_to_pass* è formattato come hh:mm[[:ss].mss].
  
 TIME  
 Ora specificata di esecuzione del batch, della stored procedure o della transazione.  
  
 '*time_to_execute*'  
 Ora in cui l'istruzione WAITFOR viene interrotta. *time_to_execute* può essere specificato in un formato dati **datetime** oppure come variabile locale. Non è possibile specificare date, pertanto la parte relativa alla data del valore **datetime** non è consentita. *time_to_execute* è formattato come hh:mm[[:ss].mss] e può includere facoltativamente la data 1900-01-01.
  
 *receive_statement*  
 Istruzione RECEIVE valida.  
  
> [!IMPORTANT]  
>  L'uso di WAITFOR con *receive_statement* è consentito solo per i messaggi di [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Per altre informazioni, vedere [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md).  
  
 *get_conversation_group_statement*  
 Istruzione GET CONVERSATION GROUP valida.  
  
> [!IMPORTANT]  
>  L'uso di WAITFOR con *get_conversation_group_statement* è consentito solo per i messaggi di [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Per altre informazioni, vedere [GET CONVERSATION GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/get-conversation-group-transact-sql.md).  
  
 TIMEOUT *timeout*  
 Specifica il periodo di tempo, espresso in millisecondi, di attesa dell'arrivo di un messaggio nella coda.  
  
> [!IMPORTANT]  
>  La specifica di WAITFOR con TIMEOUT è consentita solo per i messaggi di [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Per altre informazioni, vedere [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md) e [GET CONVERSATION GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/get-conversation-group-transact-sql.md).  
  
## <a name="remarks"></a>Osservazioni  
 Durante l'esecuzione dell'istruzione WAITFOR la transazione è in esecuzione e pertanto non possono essere eseguite altre richieste nell'ambito della stessa transazione.  
  
 Il ritardo effettivo può variare rispetto al tempo specificato in *time_to_pass*, *time_to_execute* o *timeout* e dipende dal livello di attività del server. Il contatore del tempo viene avviato alla pianificazione del thread dell'istruzione WAITFOR. Se il server è occupato, è possibile che il thread non venga pianificato immediatamente e che il ritardo risulti superiore al tempo specificato.  
  
 WAITFOR non modifica la semantica di una query. Se una query non è in grado di restituire righe, WAITFOR attenderà all'infinito oppure finché non viene raggiunto il valore dell'argomento TIMEOUT, se specificato.  
  
 Nelle istruzioni WAITFOR non è possibile aprire i cursori.  
  
 Nelle istruzioni WAITFOR non è possibile definire le viste.  
  
 Se la query supera il valore specificato dall'opzione query wait, l'argomento dell'istruzione WAITFOR può essere completato senza essere eseguito. Per altre informazioni sull'opzione di configurazione, vedere [Configurare l'opzione di configurazione del server query wait](../../database-engine/configure-windows/configure-the-query-wait-server-configuration-option.md). Per visualizzare i processi attivi e in attesa, usare [sp_who](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md).  
  
 A ogni istruzione WAITFOR è associato un thread. Se nello stesso server sono specificate numerose istruzioni WAITFOR, è possibile che numerosi thread siano in attesa dell'esecuzione di queste istruzioni. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] monitora il numero di thread dell'istruzione WAITFOR e ne seleziona casualmente alcuni per forzarne l'uscita se nel server inizia a verificarsi una condizione di "thread starvation".  
  
 L'esecuzione di una query con WAITFOR all'interno di una transazione che include anche blocchi che impediscono le modifiche al set di righe a cui accede l'istruzione WAITFOR può creare un deadlock. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identifica questi scenari e restituisce un set di risultati vuoto se esiste la probabilità che si verifichi tale deadlock.  
  
> [!CAUTION]  
>  L'inclusione di WAITFOR rallenterà il completamento del processo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e può generare un messaggio di timeout nell'applicazione. Se necessario, regolare l'impostazione di timeout per la connessione a livello di applicazione.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-waitfor-time"></a>R. Utilizzo di WAITFOR TIME  
 Nell'esempio seguente la stored procedure `sp_update_job` viene eseguita nel database msdb alle ore 22:20. (`22:20`).  
  
```sql  
EXECUTE sp_add_job @job_name = 'TestJob';  
BEGIN  
    WAITFOR TIME '22:20';  
    EXECUTE sp_update_job @job_name = 'TestJob',  
        @new_name = 'UpdatedJob';  
END;  
GO  
```  
  
### <a name="b-using-waitfor-delay"></a>B. Utilizzo di WAITFOR DELAY  
 Nell'esempio seguente la stored procedure viene eseguita dopo un ritardo di due ore.  
  
```sql  
BEGIN  
    WAITFOR DELAY '02:00';  
    EXECUTE sp_helpdb;  
END;  
GO  
```  
  
### <a name="c-using-waitfor-delay-with-a-local-variable"></a>C. Utilizzo di WAITFOR DELAY con una variabile locale  
 Nell'esempio seguente viene illustrato l'utilizzo di una variabile locale con l'opzione `WAITFOR DELAY`. Questa stored procedure rimane in attesa per un periodo di tempo variabile e quindi restituisce all'utente informazioni sul numero di ore, minuti e secondi trascorsi.  
  
```sql  
IF OBJECT_ID('dbo.TimeDelay_hh_mm_ss','P') IS NOT NULL  
    DROP PROCEDURE dbo.TimeDelay_hh_mm_ss;  
GO  
CREATE PROCEDURE dbo.TimeDelay_hh_mm_ss   
    (  
    @DelayLength char(8)= '00:00:00'  
    )  
AS  
DECLARE @ReturnInfo VARCHAR(255)  
IF ISDATE('2000-01-01 ' + @DelayLength + '.000') = 0  
    BEGIN  
        SELECT @ReturnInfo = 'Invalid time ' + @DelayLength   
        + ',hh:mm:ss, submitted.';  
        -- This PRINT statement is for testing, not use in production.  
        PRINT @ReturnInfo   
        RETURN(1)  
    END  
BEGIN  
    WAITFOR DELAY @DelayLength  
    SELECT @ReturnInfo = 'A total time of ' + @DelayLength + ',   
        hh:mm:ss, has elapsed! Your time is up.'  
    -- This PRINT statement is for testing, not use in production.  
    PRINT @ReturnInfo;  
END;  
GO  
/* This statement executes the dbo.TimeDelay_hh_mm_ss procedure. */  
EXEC TimeDelay_hh_mm_ss '00:00:10';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `A total time of 00:00:10, in hh:mm:ss, has elapsed. Your time is up.`  
  
## <a name="see-also"></a>Vedere anche  
 [Elementi del linguaggio per il controllo di flusso &#40;Transact-SQL&#41;](~/t-sql/language-elements/control-of-flow.md)   
 [datetime &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime-transact-sql.md)   
 [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)  
  
