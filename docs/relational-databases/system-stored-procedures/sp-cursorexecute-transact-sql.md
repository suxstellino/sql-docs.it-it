---
description: sp_cursorexecute (Transact-SQL)
title: sp_cursorexecute (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursorexecute
- sp_cursorexecute_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursor_execute
ms.assetid: 6a204229-0a53-4617-a57e-93d4afbb71ac
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6aab1670ae9d517ec60d3af572d15eaf12432f7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205160"
---
# <a name="sp_cursorexecute-transact-sql"></a>sp_cursorexecute (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea e popola un cursore in base al piano di esecuzione creato da sp_cursorprepare. Questa procedura, associata a sp_cursorprepare, ha la stessa funzione di sp_cursoropen, ma è suddivisa in due fasi. sp_cursorexecute viene richiamato specificando ID = 4 in un pacchetto del flusso TDS (Tabular Data Stream).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_cursorexecute prepared_handle, cursor  
    [ , scrollopt[ OUTPUT ]  
    [ , ccopt[ OUTPUT ]  
    [ ,rowcount OUTPUT [ ,bound param][,...n]]]]]  
```  
  
## <a name="arguments"></a>Argomenti  
 *prepared_handle*  
 Valore dell' *handle* dell'istruzione preparata restituito da sp_cursorprepare. *prepared_handle* è un parametro obbligatorio che richiede un valore di input **int** .  
  
 *cursor*  
 Identificatore del cursore generato da SQL Server. *Cursor* è un parametro obbligatorio che deve essere fornito in tutte le procedure successive che agiscono sul cursore, ad esempio sp_cursorfetch  
  
 *scrollopt*  
 Opzione di scorrimento. *scrollopt* è un parametro facoltativo che richiede un valore di input **int** . Il parametro sp_cursorexecute *scrollopt* ha le stesse opzioni di valore di quelle per sp_cursoropen.  
  
> [!NOTE]  
>  Il valore PARAMETERIZED_STMT non è supportato.  
  
> [!IMPORTANT]  
>  Se non si specifica un valore *scrollopt* , il valore predefinito è KEYSET indipendentemente dal valore di *scrollopt* specificato in sp_cursorprepare.  
  
 *ccopt*  
 Opzione del controllo della valuta. *ccopt* è un parametro facoltativo che richiede un valore di input **int** . Il parametro sp_cursorexecute *ccopt* ha le stesse opzioni di valore di quelle per sp_cursoropen.  
  
> [!IMPORTANT]  
>  Se non si specifica un valore *ccopt* , il valore predefinito è ottimistico indipendentemente dal valore di *ccopt* specificato in sp_cursorprepare.  
  
 *RowCount*  
 Parametro facoltativo che indica il numero di righe del buffer di recupero da utilizzare con AUTO_FETCH. Il valore predefinito è 20 righe. il *conteggio delle righe* si comporta in modo diverso quando viene assegnato come valore di input rispetto a un valore restituito.  
  
|Come valore di input|Come valore restituito|  
|--------------------|---------------------|  
|Quando AUTO_FETCH viene specificato con FAST_FORWARD cursori *RowCount* rappresenta il numero di righe da inserire nel buffer di recupero.|Rappresenta il numero di righe nel set di risultati. Quando viene specificato il valore *scrollopt* AUTO_FETCH, *RowCount* restituisce il numero di righe recuperate nel buffer di recupero.|  
  
 *bound_param*  
 Indica l'utilizzo facoltativo di parametri aggiuntivi.  
  
> [!NOTE]  
>  I parametri dopo il quinto vengono passati insieme sul piano dell'istruzione come parametri di input.  
  
## <a name="code-return-value"></a>Valore restituito del codice  
 *RowCount* può restituire i valori seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|-1|Numero di righe non note.|  
|-n|Un popolamento asincrono è attivo.|  
  
## <a name="remarks"></a>Commenti  
  
## <a name="scrollopt-and-ccopt-parameters"></a>Parametri scrollopt e ccopt  
 *scrollopt* e *ccopt* sono utili quando i piani memorizzati nella cache vengono interrotti per la cache del server, vale a dire che l'handle preparato che identifica l'istruzione deve essere ricompilato. I valori dei parametri *scrollopt* e *ccopt* devono corrispondere ai valori inviati nella richiesta originale a sp_cursorprepare.  
  
> [!NOTE]  
>  PARAMETERIZED_STMT non devono essere assegnati a *scrollopt*.  
  
 Se non si riesce a fornire valori corrispondenti, i piani verranno ricompilati impedendo le operazioni di preparazione ed esecuzione.  
  
## <a name="rpc-and-tds-considerations"></a>Considerazioni su RPC e TDS  
 Per richiedere che vengano restituiti metadati sull'elenco di selezione del cursore nel flusso TDS, è possibile impostare il flag di input RPC RETURN_METADATA su 1.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_cursoropen &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [sp_cursorfetch &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursorfetch-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
