---
description: sp_cursoroption (Transact-SQL)
title: sp_cursoroption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursoroption_TSQL
- sp_cursoroption
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursoroption
ms.assetid: 88fc1dba-f4cb-47c0-92c2-bf398f4a382e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 787c54be4c1030f9c091dbfead90c569a63bb270
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210480"
---
# <a name="sp_cursoroption-transact-sql"></a>sp_cursoroption (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Imposta le opzioni del cursore o restituisce informazioni sul cursore create dalla sp_cursoropen stored procedure. sp_cursoroption viene richiamato specificando ID = 8 in un pacchetto del flusso TDS (Tabular Data Stream).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_cursoroption cursor, code, value  
```  
  
## <a name="arguments"></a>Argomenti  
 *cursor*  
 Valore dell' *handle* generato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e restituito dal sp_cursoropen stored procedure. il *cursore* richiede un valore di input **int** per l'esecuzione.  
  
 *code*  
 Consente di specificare i vari fattori dei valori restituiti del cursore. il *codice* richiede uno dei valori di input **int** seguenti:  
  
|Valore|Nome|Descrizione|  
|-----------|----------|-----------------|  
|0x0001|TEXTPTR_ONLY|Restituisce il puntatore di testo, anziché i dati effettivi, per determinate colonne di tipo text o image designate.<br /><br /> TEXTPTR_ONLY consente l'utilizzo di puntatori di testo come *handle* per gli oggetti BLOB che possono essere recuperati o aggiornati in un secondo momento tramite le [!INCLUDE[tsql](../../includes/tsql-md.md)] funzionalità o DBLIB, ad esempio [!INCLUDE[tsql](../../includes/tsql-md.md)] READTEXT o DBLIB DBWRITETEXT.<br /><br /> Se viene assegnato il valore "0", tutte le colonne di tipo text e image nell'elenco di selezione restituiranno puntatori di testo anziché dati.|  
|0x0002|CURSOR_NAME|Assegna il nome specificato in *valore* al cursore. Questo, a sua volta, consente a ODBC di utilizzare [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzioni Update/Delete posizionate sui cursori aperti tramite sp_cursoropen.<br /><br /> La stringa può essere specificata come qualsiasi tipo di dati Unicode o character.<br /><br /> Poiché le [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzioni Update/Delete posizionate operano per impostazione predefinita sulla prima riga in un cursore FAT, è necessario utilizzare sp_cursor seposition per posizionare il cursore prima di eseguire l'istruzione UPDATE/DELETE posizionata.|  
|0x0003|TEXTDATA|Restituisce i dati effettivi, anziché il puntatore di testo, per determinate colonne di tipo text o image in recuperi successivi, ovvero annulla l'effetto di TEXTPTR_ONLY.<br /><br /> Se per una colonna specifica è abilitato TEXTDATA, la riga viene nuovamente recuperata o aggiornata e può quindi essere nuovamente impostata su TEXTPTR_ONLY. Analogamente a quanto accade per TEXTPTR_ONLY, il parametro di valore è un intero che specifica il numero di colonna e un valore zero restituisce tutte le colonne di tipo text o image.|  
|0x0004|SCROLLOPT|Opzione di scorrimento. Per ulteriori informazioni, vedere "Valori dei codici restituiti" più avanti in questo argomento.|  
|0x0005|CCOPT|Opzioni del controllo della concorrenza. Per ulteriori informazioni, vedere "Valori dei codici restituiti" più avanti in questo argomento.|  
|0x0006|ROWCOUNT|Numero di righe correntemente nel set di risultati.<br /><br /> Nota: è possibile che il conteggio delle righe sia stato modificato rispetto al valore restituito da sp_cursoropen se viene utilizzato il popolamento asincrono. Se il numero di righe è sconosciuto, viene restituito il valore-1.|  
  
 *value*  
 Definisce il valore restituito dal *codice*. il *valore* è un parametro obbligatorio che richiede un valore di input di *codice* 0x0001, 0x0002 o 0x0003.  
  
> [!NOTE]  
>  Un valore di *codice* 2 è un tipo di dati String. Qualsiasi altro valore del *codice* di input o restituito per *valore* è un numero intero.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Il parametro *value* può restituire uno dei valori di *codice* seguenti.  
  
|Valore restituito|Descrizione|  
|------------------|-----------------|  
|0x0004|SCROLLOPT|  
|0X0005|CCOPT|  
|0X0006|ROWCOUNT|  
  
 Il parametro *value* restituisce uno dei valori scrollopt seguenti.  
  
|Valore restituito|Descrizione|  
|------------------|-----------------|  
|0x0001|KEYSET|  
|0x0002|DYNAMIC|  
|0x0004|FORWARD_ONLY|  
|0x0008|STATIC|  
  
 Il parametro *value* restituisce uno dei valori CCOPT seguenti.  
  
|Valore restituito|Descrizione|  
|------------------|-----------------|  
|0x0001|READ_ONLY|  
|0x0002|SCROLL_LOCKS|  
|0x0004 o 0x0008|OPTIMISTIC|  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sp_cursor &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursor-transact-sql.md)   
 [sp_cursoropen &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)  
  
  
