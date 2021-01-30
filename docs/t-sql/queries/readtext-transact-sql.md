---
description: READTEXT (Transact-SQL)
title: READTEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- READTEXT_TSQL
- READTEXT
dev_langs:
- TSQL
helpviewer_keywords:
- column reading [SQL Server]
- READTEXT statement
- reading columns
ms.assetid: 91b69853-1381-4306-8343-afdb73105738
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e312bff216746345fc028b1ff581bdee56a12eca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207622"
---
# <a name="readtext-transact-sql"></a>READTEXT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Legge i valori di tipo **text**, **ntext** o **image** da una colonna di tipo **text**, **ntext** o **image**. Legge il numero di byte specificato a partire dall'offset indicato.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Usare invece la funzione [SUBSTRING](../../t-sql/functions/substring-transact-sql.md).  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
READTEXT { table.column text_ptr offset size } [ HOLDLOCK ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
_table_ **.** _column_  
Nome di una tabella e di una colonna in cui leggere i valori. I nomi delle tabelle e delle colonne devono rispettare le regole relative agli [identificatori](../../relational-databases/databases/database-identifiers.md). È necessario specificare i nomi della tabella e della colonna, mentre il nome del database e il nome del proprietario sono facoltativi.  
  
_text\_ptr_  
Puntatore di testo valido. _text\_ptr_ deve essere **binary(16)** .  
  
_offset_  
Se viene usato il tipo di dati **text** o **image**, è il numero di byte. Se viene usato il tipo di dati **ntext**, può anche essere il numero di byte di caratteri da ignorare prima di iniziare la lettura dei dati **text**, **image** o **ntext**.  
  
_size_ è il numero di byte se viene usato il tipo di dati **text** o **image**. Se viene usato il tipo di dati **ntext**, può anche essere il numero di byte di caratteri dei dati da leggere. Se _size_ è 0, vengono letti 4 KB di dati.  
  
HOLDLOCK  
Impedisce la lettura del valore del testo fino alla fine della transazione. Gli altri utenti possono leggere il valore, ma non modificarlo.  
  
## <a name="remarks"></a>Osservazioni  
Usare la funzione [TEXTPTR](../../t-sql/functions/text-and-image-functions-textptr-transact-sql.md) per ottenere un valore _text\_ptr_ valido. TEXTPTR restituisce un puntatore alla colonna di tipo **text**, **ntext** o **image** nella riga specificata. TEXTPRT può anche restituire un puntatore alla colonna di tipo **text**, **ntext** o **image** nell'ultima riga restituita dalla query, se la query restituisce più righe. Poiché TEXTPTR restituisce una stringa binaria di 16 byte, è consigliabile dichiarare una variabile locale in cui archiviare il puntatore di testo e utilizzare quindi la variabile con l'istruzione READTEXT. Per altre informazioni sulla dichiarazione di una variabile locale, vedere [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md).  
  
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] i puntatori di testo all'interno di righe possono esistere, ma possono essere non validi. Per altre informazioni sull'opzione **text in row**, vedere [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md). Per altre informazioni su come invalidare i puntatori di testo, vedere [sp_invalidate_textptr &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-invalidate-textptr-transact-sql.md).  
  
Se il valore della funzione @@TEXTSIZE è inferiore alle dimensioni specificate per READTEXT, prevale su di esse. La funzione @@TEXTSIZE specifica il limite per il numero di byte di dati restituito che viene impostato dall'istruzione SET TEXTSIZE. Per altre informazioni sull'impostazione della sessione per la funzione TEXTSIZE, vedere [SET TEXTSIZE &#40;Transact-SQL&#41;](../../t-sql/statements/set-textsize-transact-sql.md).  
  
## <a name="permissions"></a>Autorizzazioni  
Le autorizzazioni per l'istruzione READTEXT vengono assegnate per impostazione predefinita agli utenti con autorizzazioni SELECT per la tabella specificata. Le autorizzazioni sono trasferibili, ovvero vengono trasferite insieme alle autorizzazioni SELECT.  
  
## <a name="examples"></a>Esempi  
L'esempio seguente legge dal secondo al 26° carattere della colonna `pr_info` della tabella `pub_info`.  
  
> [!NOTE]  
>  Per eseguire questo esempio, è necessario installare il database di esempio [**pubs**](https://github.com/microsoft/sql-server-samples/tree/master/samples/databases).  
  
```sql
USE pubs;  
GO  
DECLARE @ptrval VARBINARY(16);  
SELECT @ptrval = TEXTPTR(pr_info)   
   FROM pub_info pr INNER JOIN publishers p  
      ON pr.pub_id = p.pub_id   
      AND p.pub_name = 'New Moon Books'  
READTEXT pub_info.pr_info @ptrval 1 25;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
[@@TEXTSIZE &#40;Transact-SQL&#41;](../../t-sql/functions/textsize-transact-sql.md)   
[UPDATETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/updatetext-transact-sql.md)   
[WRITETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/writetext-transact-sql.md)  
  
  
