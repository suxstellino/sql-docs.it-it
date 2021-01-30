---
description: WRITETEXT (Transact-SQL)
title: WRITETEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WRITETEXT_TSQL
- WRITETEXT
dev_langs:
- TSQL
helpviewer_keywords:
- replacing data
- WRITETEXT statement
- updating data [SQL Server]
- nonlogged updating
- minimally logged updating [SQL Server]
- overwriting data
- data updates [SQL Server], WRITETEXT statement
ms.assetid: 80c252fd-a8b8-4a2e-888a-059081ed4109
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f1793b813e763df5cb8078a73fb1cd78c7fe7a6c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193370"
---
# <a name="writetext-transact-sql"></a>WRITETEXT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Consente di eseguire l'aggiornamento interattivo con registrazione minima di una colonna di tipo **text**, **ntext** o **image** esistente. WRITETEXT sovrascrive completamente tutti i dati esistenti nella colonna interessata. Non è possibile usare WRITETEXT nelle colonne di tipo **text**, **ntext** e **image** di una vista.  
  
> [!IMPORTANT]
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare i tipi di dati per valori di grandi dimensioni e la clausola **.** WRITE dell'istruzione [UPDATE](../../t-sql/queries/update-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
WRITETEXT [BULK]  
  { table.column text_ptr }  
  [ WITH LOG ] { data }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 BULK  
 Consente agli strumenti di caricamento di caricare un flusso di dati binario. Il flusso deve essere fornito dallo strumento a livello di protocollo TDS. Se il flusso di dati non è presente, Query Processor ignora l'opzione BULK.  
  
> [!IMPORTANT]  
>  È consigliabile che l'opzione BULK non venga utilizzata nelle applicazioni basate su [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa opzione potrebbe essere modificata o rimossa in una prossima versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *table* **.column**  
 Nome della tabella e della colonna di tipo **text**, **ntext** o **image** da aggiornare. I nomi delle tabelle e delle colonne devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md). I nomi del database e del proprietario sono facoltativi.  
  
 *text_ptr*  
 Valore che contiene il puntatore nei dati di tipo **text**, **ntext** o **image**. *text_ptr* deve essere di tipo **binary(16)**. Per creare un puntatore di testo, eseguire un'istruzione [INSERT](../../t-sql/statements/insert-transact-sql.md) o [UPDATE](../../t-sql/queries/update-transact-sql.md) con dati non Null per la colonna di tipo **text**, **ntext** o **image**.  
  
 WITH LOG  
 Argomento ignorato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La registrazione è definita dal modello di recupero attivo nel database.  
  
 *data*  
 Dati **text**, **ntext** o **image** effettivi da archiviare. *data* può essere un valore letterale o un parametro. La lunghezza massima consentita per il testo che è possibile inserire in modo interattivo tramite WRITETEXT è di circa 120 KB per i dati di tipo **text**, **ntext** e **image**.  
  
## <a name="remarks"></a>Commenti  
 Usare WRITETEXT per sostituire i dati di tipo **text**, **ntext** e **image** e UPDATETEXT per modificare i dati di tipo **text**, **ntext** e **image**. UPDATETEXT risulta più flessibile poiché modifica solo una parte della colonna di tipo **text**, **ntext** o **image** anziché l'intera colonna.  
  
 Per ottimizzare le prestazioni, è consigliabile inserire o aggiornare i dati di tipo **text**, **ntext** e **image** in blocchi con dimensioni multiple di 8040 byte.  
  
 Se il modello di recupero del database è di tipo semplice o con registrazione minima delle operazioni bulk, le operazioni sui dati di tipo **text**, **ntext** e **image** in cui viene usato WRITETEXT saranno operazioni con registrazione minima in caso di inserimento o aggiunta di nuovi dati.  
  
> [!NOTE]  
>  La registrazione minima non viene utilizzata in caso di aggiornamento di valori esistenti.  
  
 Per consentire la corretta esecuzione di WRITETEXT, è necessario che la colonna includa un puntatore di testo valido.  
  
 Se la tabella non include testo all'interno delle righe, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] riduce automaticamente la quantità di spazio usato evitando l'inizializzazione delle colonne di tipo **text** in corrispondenza dell'aggiunta di valori Null impliciti o espliciti nelle colonne di tipo **text** tramite l'istruzione INSERT. Per questi valori Null non è più possibile ottenere un puntatore di testo. Per inizializzare le colonne di tipo **text** per i valori NULL, usare l'istruzione UPDATE. Se la tabella include testo all'interno delle righe, non è necessario inizializzare la colonna di tipo text per i valori Null ed è sempre possibile ottenere un puntatore di testo.  
  
 La funzione ODBC SQLPutData risulta più rapida e usa una quantità inferiore di memoria dinamica rispetto a WRITETEXT. Questa funzione è in grado di inserire fino a 2 gigabyte di dati di tipo **text**, **ntext** o **image**.  
  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è possibile che esistano ma non siano validi puntatori di testo all'interno di righe a dati di tipo **text**, **ntext** o **image**. Per informazioni sull'opzione text in row, vedere [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md). Per informazioni su come invalidare i puntatori di testo, vedere [sp_invalidate_textptr &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-invalidate-textptr-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione UPDATE per la tabella specificata. L'autorizzazione è trasferibile in caso di trasferimento dell'autorizzazione UPDATE.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente il puntatore di testo viene inserito nella variabile locale `@ptrval` e quindi `WRITETEXT` inserisce la nuova stringa di testo nella riga indicata da `@ptrval`.  
  
> [!NOTE]  
>  Per eseguire questo esempio, è necessario installare il database di esempio pubs.  
  
```sql  
USE pubs;  
GO  
ALTER DATABASE pubs SET RECOVERY SIMPLE;  
GO  
DECLARE @ptrval BINARY(16);  
SELECT @ptrval = TEXTPTR(pr_info)   
FROM pub_info pr, publishers p  
WHERE p.pub_id = pr.pub_id   
   AND p.pub_name = 'New Moon Books'  
WRITETEXT pub_info.pr_info @ptrval 'New Moon Books (NMB) has just released another top ten publication. With the latest publication this makes NMB the hottest new publisher of the year!';  
GO  
ALTER DATABASE pubs SET RECOVERY SIMPLE;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [UPDATETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/updatetext-transact-sql.md)  
  
  
