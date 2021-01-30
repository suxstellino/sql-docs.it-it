---
description: UPDATETEXT (Transact-SQL)
title: UPDATETEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- UPDATETEXT
- UPDATETEXT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- text updates [SQL Server]
- updating data [SQL Server]
- data updates [SQL Server], UPDATETEXT statement
- UPDATETEXT statement
ms.assetid: d73c28ee-3972-4afd-af8d-ebbbd9e50793
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 5304394a75662d5cdc37196aab70444ebfbcbe41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186915"
---
# <a name="updatetext-transact-sql"></a>UPDATETEXT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aggiorna un campo **text**, **ntext**, o **image** esistente. Usare UPDATETEXT per modificare solo una parte di una colonna esistente di tipo **text**, **ntext** o **image**. Usare WRITETEXT per aggiornare e sostituire un intero campo di tipo **text**, **ntext**, o **image**.  
  
> [!IMPORTANT]
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare i tipi di dati per valori di grandi dimensioni e la clausola **.** WRITE dell'istruzione [UPDATE](../../t-sql/queries/update-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
UPDATETEXT [BULK] { table_name.dest_column_name dest_text_ptr }  
  { NULL | insert_offset }  
     { NULL | delete_length }  
     [ WITH LOG ]  
     [ inserted_data  
    | { table_name.src_column_name src_text_ptr } ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 BULK  
 Consente agli strumenti di caricamento di caricare un flusso di dati binario. Il flusso deve essere fornito dallo strumento a livello di protocollo TDS. Se il flusso di dati non è presente, Query Processor ignora l'opzione BULK.  
  
> [!IMPORTANT]  
>  È consigliabile che l'opzione BULK non venga utilizzata nelle applicazioni basate su [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa opzione potrebbe essere modificata o rimossa in una prossima versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *table_name* **.** *dest_column_name*  
 Nome della tabella e della colonna di tipo **text**, **ntext** o **image** da aggiornare. I nomi delle tabelle e delle colonne devono essere conformi alle regole per gli [identificatori](../../relational-databases/databases/database-identifiers.md). I nomi del database e del proprietario sono facoltativi.  
  
 *dest_text_ptr*  
 Valore di un puntatore di testo, restituito dalla funzione TEXTPTR, che fa riferimento ai dati di tipo **text**, **ntext** o **image** da aggiornare. *dest_text_ptr* deve essere **binary(** 16 **)**.  
  
 *insert_offset*  
 Posizione iniziale in base zero dell'aggiornamento. Per le colonne di tipo **text** o **image**, *insert_offset* rappresenta il numero di byte da ignorare a partire dall'inizio della colonna esistente prima di inserire nuovi dati. Per le colonne di tipo **ntext**, *insert_offset* è il numero di caratteri (ogni carattere **ntext** usa 2 byte). I dati di tipo **text**, **ntext**, o **image** esistenti che iniziano nella posizione iniziale in base zero specificata vengono spostati a destra per creare spazio per i nuovi dati. Il valore 0 inserisce i nuovi dati all'inizio dei dati esistenti. Il valore NULL accoda i nuovi dati al valore dei dati esistenti.  
  
 *delete_length*  
 Lunghezza dei dati da eliminare dalla colonna esistente di tipo **text**, **ntext** o **image**, a partire dalla posizione *insert_offset*. Il valore *delete_length* è espresso in byte per le colonne di tipo **text** e **image** e in caratteri per le colonne **ntext**. Ogni carattere **ntext** usa 2 byte. Il valore 0 non elimina alcun dato. Il valore NULL elimina tutti i dati a partire dalla posizione *insert_offset* fino alla fine della colonna esistente di tipo **text** o **image**.  
  
 WITH LOG  
 La registrazione è definita dal modello di recupero attivo nel database.  
  
 *inserted_data*  
 I dati da inserire nella colonna esistente di tipo **text**, **ntext** o **image** nel percorso *insert_offset*. Si tratta di un singolo valore di tipo **char**, **nchar**, **varchar**, **nvarchar**, **binary**, **varbinary**, **text**, **ntext**, o **image**. *inserted_data* può essere un valore letterale o una variabile.  
  
 *table_name.src_column_name*  
 Nome della tabella e della colonna di tipo **text**, **ntext** o **image** usata come origine dei dati inseriti. I nomi delle tabelle e delle colonne devono essere conformi alle regole per gli identificatori.  
  
 *src_text_ptr*  
 Valore di un puntatore di testo restituito dalla funzione TEXTPTR che fa riferimento a una colonna di tipo **text**, **ntext**, o **image** usata come origine dei dati inseriti.  
  
> [!NOTE]  
>  Il valore *scr_text_ptr* non deve essere identico al valore *dest_text_ptr*.  
  
## <a name="remarks"></a>Commenti  
 I dati inseriti possono essere una singola costante *inserted_data*, un nome di tabella o di colonna oppure un puntatore di testo.  
  
|Operazione di aggiornamento|Parametri di UPDATETEXT|  
|-------------------|---------------------------|  
|Sostituzione di dati esistenti|Specificare un valore *insert_offset* diverso da Null, un valore *delete_length* diverso da zero e i nuovi dati da inserire.|  
|Eliminazione di dati esistenti|Specificare un valore *insert_offset* diverso da Null e un valore *delete_length* diverso da zero. Non specificare nuovi dati da inserire.|  
|Inserimento di nuovi dati|Specificare un valore *insert_offset*, un valore *delete_length* pari a zero e i nuovi dati da inserire.|  
  
 Per ottimizzare le prestazioni, è consigliabile inserire o aggiornare i dati di tipo **text**, **ntext** e **image** in blocchi con dimensioni multiple di 8.040 byte.  
  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è possibile che esistano ma non siano validi puntatori di testo all'interno di righe a dati di tipo **text**, **ntext** o **image**. Per informazioni sull'opzione text in row, vedere [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md). Per informazioni su come invalidare i puntatori di testo, vedere [sp_invalidate_textptr &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-invalidate-textptr-transact-sql.md).  
  
 Per inizializzare le colonne di tipo **text** sul valore NULL, usare WRITETEXT; UPDATETEXT inizializza le colonne di tipo **text** su una stringa vuota.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione UPDATE per la tabella specificata.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente il puntatore di testo viene inserito nella variabile locale `@ptrval`, quindi viene eseguita l'istruzione `UPDATETEXT` per aggiornare un errore di ortografia.  
  
> [!NOTE]  
>  Per eseguire l'esempio, è necessario installare il database pubs.  
  
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
UPDATETEXT pub_info.pr_info @ptrval 88 1 'b';  
GO  
ALTER DATABASE pubs SET RECOVERY FULL;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [READTEXT &#40;Transact-SQL&#41;](../../t-sql/queries/readtext-transact-sql.md)   
 [TEXTPTR &#40;Transact-SQL&#41;](../../t-sql/functions/text-and-image-functions-textptr-transact-sql.md)   
 [WRITETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/writetext-transact-sql.md)  
  
  
