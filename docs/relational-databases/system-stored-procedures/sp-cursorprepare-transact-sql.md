---
description: sp_cursorprepare (Transact-SQL)
title: sp_cursorprepare (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursor_prepare_TSQL
- sp_cursor_prepare
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursor_prepare
ms.assetid: 6207e110-f4bf-4139-b3ec-b799c9cb3ad7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: df4b0ff31a5b8a1723b6c8f130aeab9b32b03166
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209743"
---
# <a name="sp_cursorprepare-transact-sql"></a>sp_cursorprepare (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Compila il batch o l'istruzione del cursore in un piano di esecuzione, ma non crea il cursore. L'istruzione compilata può essere utilizzata in un secondo momento da sp_cursorexecute. Questa procedura, associata a sp_cursorexecute, ha la stessa funzione di sp_cursoropen, ma è suddivisa in due fasi. sp_cursorprepare viene richiamato specificando ID = 3 in un pacchetto del flusso TDS (Tabular Data Stream).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_cursorprepare prepared_handle OUTPUT, params , stmt , options  
    [ , scrollopt[ , ccopt]]  
```  
  
## <a name="arguments"></a>Argomenti  
 *prepared_handle*  
 Identificatore dell' *handle* preparato generato da SQL Server che restituisce un valore integer.  
  
> [!NOTE]  
>  *prepared_handle* viene successivamente fornito a una procedura sp_cursorexecute per aprire un cursore. Dopo essere stato creato, un handle esiste fino a quando non si esegue la disconnessione o fino a quando non viene rimosso in modo esplicito tramite la routine sp_cursorunprepare.  
  
 *params*  
 Identifica le istruzioni con parametri. La definizione *params* delle variabili viene sostituita per i marcatori di parametro nell'istruzione. *params* è un parametro obbligatorio che richiede un valore di input **ntext**, **nchar** o **nvarchar** . Immettere un valore NULL se l'istruzione non è con parametri.  
  
> [!NOTE]  
>  Usare una stringa **ntext** come valore di input quando *stmt* è con parametri e il valore di *scrollopt* PARAMETERIZED_STMT è on.  
  
 *stmt*  
 Definisce il set di risultati del cursore. Il parametro *stmt* è obbligatorio e richiede un valore di input **ntext**, **nchar** o **nvarchar** .  
  
> [!NOTE]  
>  Le regole per la specifica del valore *stmt* sono le stesse di quelle per sp_cursoropen, con l'eccezione che il tipo di dati stringa *stmt* deve essere **ntext**.  
  
 *options*  
 Parametro facoltativo tramite cui viene restituita una descrizione delle colonne dei set di risultati del cursore. *options* richiede il valore di input **int** seguente.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|0x0001|RETURN_METADATA|  
  
 *scrollopt*  
 Opzione di scorrimento. *scrollopt* è un parametro facoltativo che richiede uno dei valori di input **int** seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|0x0001|KEYSET|  
|0x0002|DYNAMIC|  
|0x0004|FORWARD_ONLY|  
|0x0008|STATIC|  
|0x10|FAST_FORWARD|  
|0x1000|PARAMETERIZED_STMT|  
|0x2000|AUTO_FETCH|  
|0x4000|AUTO_CLOSE|  
|0x8000|CHECK_ACCEPTED_TYPES|  
|0x10000|KEYSET_ACCEPTABLE|  
|0x20000|DYNAMIC_ACCEPTABLE|  
|0x40000|FORWARD_ONLY_ACCEPTABLE|  
|0x80000|STATIC_ACCEPTABLE|  
|0x100000|FAST_FORWARD_ACCEPTABLE|  
  
 Poiché il valore richiesto potrebbe non essere appropriato per il cursore definito da *stmt*, questo parametro funge sia da input sia da output. In questi casi, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] assegna un valore appropriato.  
  
 *ccopt*  
 Opzioni del controllo della concorrenza. *ccopt* è un parametro facoltativo che richiede uno dei valori di input **int** seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|0x0001|READ_ONLY|  
|0x0002|SCROLL_LOCKS (precedentemente noto come LOCKCC)|  
|0x0004|**Ottimistica** (precedentemente nota come OPTCC)|  
|0x0008|OPTIMISTIC (precedentemente noto come OPTCCVAL)|  
|0x2000|ALLOW_DIRECT|  
|0x4000|UPDT_IN_PLACE|  
|0x8000|CHECK_ACCEPTED_OPTS|  
|0x10000|READ_ONLY_ACCEPTABLE|  
|0x20000|SCROLL_LOCKS_ACCEPTABLE|  
|0x40000|OPTIMISTIC_ACCEPTABLE|  
|0x80000|OPTIMISITC_ACCEPTABLE|  
  
 Come per *scrollpt*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può assegnare un valore diverso da quello richiesto.  
  
## <a name="remarks"></a>Commenti  
 Il parametro di stato RPC è uno degli elementi seguenti:  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|0|Operazione completata|  
|0x0001|Errore|  
|1FF6|Non è stato possibile restituire metadati.<br /><br /> Nota: il motivo è che l'istruzione non produce un set di risultati. ad esempio, si tratta di un'istruzione INSERT o DDL.|  
  
## <a name="examples"></a>Esempio  
  Di seguito è riportato un esempio di utilizzo di sp_cursorprepare e sp_cursorexecute

```sql
declare @handle int , @p5 int, @p6 int
exec sp_cursorprepare @handle OUTPUT, 
    N'@dbid int', 
    N'select * from sys.databases where database_id < @dbid',
    1,
    @p5 output,
    @p6 output


declare @p1 int  
set @P1 = @handle 
declare @p2 int   
declare @p3 int  
declare @p4 int  
set @P6 = 4 
exec sp_cursorexecute @p1, @p2 OUTPUT, @p3 output , @p4 output, @p5 OUTPUT, @p6

exec sp_cursorfetch @P2

exec sp_cursorunprepare @handle
exec sp_cursorclose @p2
```
 
 Quando *stmt* è con parametri e il valore di *scrollopt* PARAMETERIZED_STMT è on, il formato della stringa è il seguente:  
  
 { *\<local variable name>**\<data type>* } [ ,... *n* ]  
  
## <a name="see-also"></a>Vedere anche  
 [sp_cursorexecute &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursorexecute-transact-sql.md)   
 [sp_cursoropen &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [sp_cursorunprepare &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursorunprepare-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
