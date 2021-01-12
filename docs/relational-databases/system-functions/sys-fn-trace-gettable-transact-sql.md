---
description: sys.fn_trace_gettable (Transact-SQL)
title: sys.fn_trace_gettable (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_trace_gettable
- fn_trace_gettable_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- fn_trace_gettable function
- sys.fn_trace_gettable function
ms.assetid: c2590159-6ec5-4510-81ab-e935cc4216cd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1f1593e1d12621b5dbe858b0f012322f447111e4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101302"
---
# <a name="sysfn_trace_gettable-transact-sql"></a>sys.fn_trace_gettable (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce il contenuto di uno o più file di traccia in formato tabulare.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In alternativa, usare Eventi estesi.  
   
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
fn_trace_gettable ( 'filename' , number_files )  
```  
  
## <a name="arguments"></a>Argomenti  
 '*filename*'  
 Specifica il file di traccia iniziale da leggere. *filename* è di **tipo nvarchar (256)** e non prevede alcun valore predefinito.  
  
 *number_files*  
 Specifica il numero di file di rollover da leggere. Questo numero include il file iniziale specificato nel *nome* file. *number_files* è di **tipo int**.  
  
## <a name="remarks"></a>Osservazioni  
 Se *number_files* viene specificato come **valore predefinito**, **fn_trace_gettable** legge tutti i file di rollover fino a quando non raggiunge la fine della traccia. **fn_trace_gettable** restituisce una tabella con tutte le colonne valide per la traccia specificata. Per ulteriori informazioni, vedere [sp_trace_setevent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md).  
  
 Si noti che la funzione fn_trace_gettable non caricherà file di rollover (quando questa opzione viene specificata tramite l'argomento *number_files* ) in cui il nome del file di traccia originale termina con un carattere di sottolineatura e un valore numerico. Questa operazione non si applica al carattere di sottolineatura e al numero che vengono aggiunti automaticamente al rollover di un file. Come soluzione alternativa, è possibile rinominare i file di traccia per rimuovere i caratteri di sottolineatura nel nome file originale. Ad esempio, se il file originale è denominato **Trace_Oct_5. trc** e il file di rollover è denominato **Trace_Oct_5_1. trc**, è possibile rinominare i file in **TraceOct5. trc** e **TraceOct5_1. trc**.  
  
 Questa funzione consente di leggere una traccia ancora attiva nell'istanza in cui viene eseguita.  
  
## <a name="permissions"></a>Autorizzazioni  
 È necessario disporre dell'autorizzazione ALTER TRACE nel server.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-fn_trace_gettable-to-import-rows-from-a-trace-file"></a>R. Utilizzo di fn_trace_gettable per importare righe da un file di traccia  
 Nell'esempio seguente viene chiamato `fn_trace_gettable` all'interno della clausola `FROM` di un'istruzione `SELECT...INTO`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT * INTO temp_trc  
FROM fn_trace_gettable('c:\temp\mytrace.trc', default);  
GO  
```  
  
### <a name="b-using-fn_trace_gettable-to-return-a-table-with-an-identity-column-that-can-be-loaded-into-a-sql-server-table"></a>B. Utilizzo di fn_trace_gettable per restituire una tabella con una colonna IDENTITY che può essere caricata in una tabella di SQL Server  
 Nell'esempio seguente questa funzione viene chiamata all'interno di un'istruzione `SELECT...INTO` e viene restituita una tabella con una colonna `IDENTITY` che può essere caricata nella tabella `temp_trc`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT IDENTITY(int, 1, 1) AS RowNumber, * INTO temp_trc  
FROM fn_trace_gettable('c:\temp\mytrace.trc', default);  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_trace_generateevent &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setfilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)   
 [sp_trace_setstatus &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setstatus-transact-sql.md)  
  
  
