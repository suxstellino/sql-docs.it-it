---
description: sp_cursorfetch (Transact-SQL)
title: sp_cursorfetch (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursorfetch
- sp_cursorfetch_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursorfetch
ms.assetid: 14513c5e-5774-4e4c-92e1-75cd6985b6a3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bd6ad166a48d259ab925ae9369abe0d73a73f13b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99126314"
---
# <a name="sp_cursorfetch-transact-sql"></a>sp_cursorfetch (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Recupera un buffer di una o più righe dal database. Il gruppo di righe in questo buffer viene denominato *buffer di recupero* del cursore. sp_cursorfetch viene richiamato specificando ID = 7 in un pacchetto del flusso TDS (Tabular Data Stream).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_cursorfetch cursor  
    [ , fetchtype[ , rownum [ , nrows] ]]   
```  
  
## <a name="arguments"></a>Argomenti  
 *cursor*  
 Valore dell' *handle* generato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e restituito da sp_cursoropen. *Cursor* è un parametro obbligatorio che richiede un valore di input **int** . Per ulteriori informazioni, vedere la sezione Osservazioni di seguito in questo argomento.  
  
 *FetchType*  
 Specifica il buffer del cursore da recuperare. *FetchType* è un parametro facoltativo che richiede uno dei valori di input Integer seguenti.  
  
|Valore|Nome|Descrizione|  
|-----------|----------|-----------------|  
|0x0001|FIRST|Recupera il primo buffer delle righe *nrows* . Se *nrows* è uguale a 0, il cursore viene posizionato prima del set di risultati e non viene restituita alcuna riga.|  
|0x0002|NEXT|Recupera il buffer successivo di righe *nrows* .|  
|0x0004|PREV|Recupera il buffer precedente di *nrows* righe.<br /><br /> Nota: l'utilizzo di PREV per un cursore di FORWARD_ONLY restituisce un messaggio di errore perché FORWARD_ONLY supporta solo lo scorrimento in una direzione.|  
|0x0008|LAST|Recupera l'ultimo buffer di *nrows* righe. Se *nrows* è uguale a 0, il cursore viene posizionato dopo il set di risultati e non viene restituita alcuna riga.<br /><br /> Nota: l'utilizzo di LAST per un cursore FORWARD_ONLY restituisce un messaggio di errore perché FORWARD_ONLY supporta solo lo scorrimento in una direzione.|  
|0x10|ABSOLUTE|Recupera un buffer di righe *nrows* a partire dalla riga *rownum* .<br /><br /> Nota: se si utilizza ABSOLUTe per un cursore dinamico o un cursore di FORWARD_ONLY, viene restituito un messaggio di errore perché FORWARD_ONLY supporta solo lo scorrimento in una sola direzione.|  
|0x20|RELATIVE|Recupera il buffer delle righe *nrows* a partire dalla riga specificata come valore *rownum* di righe dalla prima riga nel blocco corrente. In questo caso *rownum* può essere un numero negativo.<br /><br /> Nota: l'utilizzo di relativo per un cursore di FORWARD_ONLY restituisce un messaggio di errore perché FORWARD_ONLY supporta solo lo scorrimento in una direzione.|  
|0x80|REFRESH|Reinserisce nel buffer dati delle tabelle sottostanti.|  
|0x100|INFO|Recupera informazioni sul cursore. Queste informazioni vengono restituite tramite i parametri *rownum* e *nrows* . Pertanto, quando si specifica INFO, *rownum* e *nrows* diventano parametri di output.|  
|0x200|PREV_NOADJUST|Viene utilizzato come PREV. Se tuttavia l'inizio del set di risultati viene raggiunto prima del previsto, i risultati potrebbero variare.|  
|0x400|SKIP_UPDT_CNCY|Deve essere usato con uno degli altri valori *FetchType* , ad eccezione delle informazioni.|  
  
> [!NOTE]  
>  Il valore 0x40 non è supportato.  
  
 Per ulteriori informazioni, vedere la sezione Osservazioni di seguito in questo argomento.  
  
 *rownum*  
 Parametro facoltativo utilizzato per specificare la posizione della riga per i valori ABSOLUTe e INFO *FetchType* utilizzando solo valori integer per l'input o l'output oppure entrambi. *rownum* funge da offset di riga per il valore di bit *FetchType* relativo. *rownum* viene ignorato per tutti gli altri valori. Per ulteriori informazioni, vedere la sezione Osservazioni di seguito in questo argomento.  
  
 *nRows*  
 Parametro facoltativo utilizzato per specificare il numero di righe da recuperare. Se *nrows* non è specificato, il valore predefinito è 20 righe. Per impostare la posizione senza restituire i dati, specificare un valore pari a 0. Quando *nrows* viene applicato alla query *FetchType* info, restituisce il numero totale di righe della query.  
  
> [!NOTE]  
>  *nrows* viene ignorato dal valore di bit Refresh *FetchType* .  
>   
>  Per ulteriori informazioni, vedere la sezione Osservazioni di seguito in questo argomento.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nelle tabelle seguenti vengono indicati i valori che possono essere restituiti quando si specifica il valore di bit INFO.  
  
> [!NOTE]  
>  : Se non vengono restituite righe, il contenuto del buffer rimane inalterato.  
  
|*\<rownum>*|Impostare su|  
|------------------|------------|  
|Se non aperto|0|  
|Se posizionato prima del set di risultati|0|  
|Se posizionato dopo il set di risultati|-1|  
|Per i cursori STATIC e KEYSET|Numero di riga assoluto della posizione corrente nel set di risultati|  
|Peri cursori DYNAMIC|1|  
|Per ABSOLUTE|-1 restituisce l'ultima riga di un set.<br /><br /> -2 restituisce la penultima riga di un set e così via.<br /><br /> Nota: se è necessario recuperare più di una riga in questo caso, vengono restituite le ultime due righe del set di risultati.|  
  
|*\<nrows>*|Impostare su|  
|-----------------|------------|  
|Se non aperto|0|  
|Per i cursori STATIC e KEYSET|In genere la dimensione del keyset corrente.<br /><br /> **-m** se il cursore si trova in una creazione asincrona con *m* righe trovate fino a questo punto.|  
|Peri cursori DYNAMIC|-1|  
  
## <a name="remarks"></a>Commenti  
  
## <a name="cursor-parameter"></a>Parametro cursor  
 Prima dell'esecuzione di qualsiasi operazione di recupero, il cursore precede per impostazione predefinita la prima riga del set di risultati.  
  
## <a name="fetchtype-parameter"></a>Parametro fetchtype  
 Ad eccezione di SKIP_UPD_CNCY, i valori *FetchType* si escludono a vicenda.  
  
 Quando viene specificato SKIP_UPDT_CNCY, i valori della colonna timestamp non vengono scritti nella tabella di keyset quando viene recuperata o aggiornata una riga. Se la riga di keyset viene aggiornata, i valori delle colonne timestamp rimangono inalterati. Se la riga di keyset viene inserita, i valori per le colonne timestamp non sono definiti.  
  
 Per i cursori KEYSET, ciò significa che per la tabella di keyset i valori vengono impostati durante l'ultima operazione FETCH non ignorata, se ne è stata eseguita una. In caso contrario, i valori vengono impostati durante il popolamento.  
  
 Per i cursori DYNAMIC, ciò significa che se l'operazione FETCH viene eseguita con l'aggiornamento, vengono prodotti gli stessi risultati di KEYSET. Per qualsiasi altro tipo di recupero, la tabella di keyset viene troncata. Ciò significa che è in corso l'inserimento delle righe e i valori delle colonne timestamp non vengono definiti. Pertanto, quando si esegue sp_cursorfetch per i cursori DYNAMIC, evitare di utilizzare SKIP_UPDT_CNCY per qualsiasi operazione diversa da REFRESH.  
  
 Se un'operazione di recupero non riesce perché la posizione del cursore richiesta è oltre il set di risultati, la posizione del cursore viene impostata subito dopo l'ultima riga. Se un'operazione di recupero non riesce perché la posizione del cursore richiesta è prima del set di risultati, la posizione del cursore viene impostata prima della prima riga.  
  
## <a name="rownum-parameter"></a>Parametro rownum  
 Quando si utilizza *rownum*, il buffer viene compilato iniziando con la riga specificata.  
  
 Il valore assoluto di *FetchType* fa riferimento alla posizione di *rownum* all'interno dell'intero set di risultati. Un numero negativo per ABSOLUTE specifica che l'operazione inizia a contare le righe a partire dalla fine del set di risultati.  
  
 Il valore *FETCHTYPE* relativo si riferisce alla posizione di *rownum* in relazione alla posizione del cursore all'inizio del buffer corrente. Un numero negativo per RELATIVE specifica che il cursore va all'indietro a partire dalla posizione corrente.  
  
## <a name="nrows-parameter"></a>Parametro nrows  
 I valori *FETCHTYPE* Refresh e info ignorano il parametro.  
  
 Quando si specifica un valore *FetchType* di First con un valore *nrow* pari a 0, il cursore viene posizionato prima del set di risultati privo di righe nel buffer di recupero.  
  
 Quando si specifica un valore *FetchType* di Last con un valore *nrow* pari a 0, il cursore viene posizionato dopo il set di risultati privo di righe nel buffer di recupero corrente.  
  
 Per i valori *FetchType* di Next, prev, Absolute, RELATIVE e PREV_NOADJUST, un valore *nrow* pari a 0 non è valido.  
  
## <a name="rpc-considerations"></a>Considerazioni su RPC  
 Lo stato restituito di RPC indica se il parametro di dimensione del keyset è finale oppure no, cioè se è in corso il popolamento asincrono nel keyset o nella tabella temporanea.  
  
 Il parametro di stato di RPC viene impostato su uno dei valori mostrati nella tabella seguente.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|0|La routine è stata eseguita correttamente.|  
|0x0001|La routine non è riuscita.|  
|0x0002|Un recupero in direzione negativa ha impostato la posizione del cursore all'inizio del set di risultati, laddove logicamente il recupero avrebbe dovuto trovarsi prima dei risultati.|  
|0x10|Un cursore di avanzamento è stato chiuso automaticamente.|  
  
 Le righe vengono restituite come set tipico di risultati, ovvero il formato della colonna (0x2a), le righe (0xd1), infine done (0xfd). I token dei metadati vengono inviati nello stesso formato specificato per sp_cursoropen, ovvero: 0x81, 0xa5 e 0xa4 per gli utenti di SQL Server 7.0 e così via. Gli indicatori di stato delle righe vengono inviati come colonne nascoste, analogamente alla modalità BROWSE, alla fine di ogni riga con nome di colonna rowstat e tipo di dati INT4. La colonna rowstat può avere uno dei valori mostrati nella tabella seguente:  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|0x0001|FETCH_SUCCEEDED|  
|0x0002|FETCH_MISSING|  
  
 Poiché il protocollo TDS non consente di inviare la colonna dello stato finale senza inviare le colonne precedenti, per le righe mancanti vengono inviati dati fittizi, cioè campi che ammettono i valori Null, campi a lunghezza fissa impostati su 0, vuoti oppure il valore predefinito per quella colonna, a seconda della situazione specifica.  
  
 Il conteggio delle righe per DONE sarà sempre zero. Il messaggio DONE include il conteggio delle righe del set di risultati effettivo. Tra i messaggi TDS potrebbero essere visualizzati messaggi di errore o informativi.  
  
 Per richiedere che vengano restituiti metadati sull'elenco di selezione del cursore nel flusso TDS, impostare il flag di input RPC RETURN_METADATA su 1.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-using-prev-to-change-a-cursor-position"></a>R. Utilizzo di PREV per modificare una posizione del cursore  
 Si supponga che un cursore h2 produca un set di risultati con il contenuto seguente con la posizione corrente mostrata di seguito:  
  
```  
row 1 contents      
row 2 contents  
row 3 contents  
row 4 contents  <-- current position  
row 5 contents   
row 6 contents  
```  
  
 Successivamente, un sp_cursorfetch PREV con un valore *nrows* pari a 5, il cursore viene posizionato logicamente su due righe prima della prima riga del set di risultati. In questi casi, il cursore viene regolato in modo da partire dalla prima riga e restituire il numero di righe richieste. Questo spesso significa che restituirà righe che si trovavano nel buffer di recupero di PRIOR.  
  
> [!NOTE]  
>  Si tratta esattamente del caso in cui il parametro di stato di RPC è impostato su 2.  
  
### <a name="b-using-prev_noadjust-to-return-fewer-rows-than-prev"></a>B. Utilizzo di PREV_NOADJUST per restituire un numero inferiore di righe rispetto a PREV  
 PREV_NOADJUST non include mai nel blocco di righe che restituisce le righe che si trovano in corrispondenza della posizione corrente del cursore o dopo tale posizione. Nei casi in cui PREV restituisce righe dopo la posizione corrente, PREV_NOADJUST restituisce un minor numero di righe rispetto a quelle richieste in *nrows*. Presupponendo che la posizione corrente del cursore sia quella dell'Esempio A precedente, quando viene applicato PREV, sp_cursorfetch (h2, 4, 1, 5) recupera le righe seguenti:  
  
```  
row1 contents   
row2 contents  
row3 contents  
row4 contents  
row5 contents  
```  
  
 Quando invece viene applicato PREV_NOADJUST, sp_cursorfetch (h2, 512, 6, 5) recupera solo le righe seguenti:  
  
```  
row1 contents   
row2 contents  
row3 contents   
```  
  
## <a name="see-also"></a>Vedere anche  
 [sp_cursoropen &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
