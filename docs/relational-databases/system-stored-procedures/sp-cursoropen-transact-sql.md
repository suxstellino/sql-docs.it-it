---
description: sp_cursoropen (Transact-SQL)
title: sp_cursoropen (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursoropen
- sp_cursoropen_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursoropen
ms.assetid: 16462ede-4393-4293-a598-ca88c48ca70b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1e8e4025a31c92ec001bcb45bee4f8d26f3dc24e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206273"
---
# <a name="sp_cursoropen-transact-sql"></a>sp_cursoropen (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Apre un cursore. sp_cursoropen definisce l'istruzione SQL associata al cursore e alle opzioni del cursore, quindi popola il cursore. sp_cursoropen equivale alla combinazione delle [!INCLUDE[tsql](../../includes/tsql-md.md)] istruzioni DECLARE_CURSOR e Open. Questa routine viene richiamata specificando ID = 2 in un pacchetto del flusso TDS.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_cursoropen cursor OUTPUT, stmt  
    [, scrollopt[ OUTPUT ] [ , ccopt[ OUTPUT ]  
    [ ,rowcount OUTPUT [ ,boundparam][,...n]]] ]]  
```  
  
## <a name="arguments"></a>Argomenti  
 *cursor*  
 Identificatore del cursore generato da SQL Server. *Cursor* è un valore dell' *handle* che deve essere fornito in tutte le procedure successive che coinvolgono il cursore, ad esempio sp_cursorfetch. *Cursor* è un parametro obbligatorio con un valore restituito **int** .  
  
 il *cursore* consente l'attivazione di più cursori in una singola connessione al database.  
  
 *stmt*  
 Parametro obbligatorio che definisce il set di risultati del cursore. Qualsiasi stringa di query valida (sintassi e associazione) di qualsiasi tipo stringa (indipendentemente da Unicode, dimensioni e così via) può essere utilizzata come tipo di valore *stmt* valido.  
  
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
  
 A causa della possibilità che il valore richiesto non sia appropriato per il cursore definito da *stmt*, questo parametro funge sia da input sia da output. In questi casi, SQL Server assegna un valore appropriato.  
  
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
  
 Come per *scrollopt*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può eseguire l'override dei valori *ccopt* richiesti.  
  
 *RowCount*  
 Numero di righe del buffer di recupero da utilizzare con AUTO_FETCH. Il valore predefinito è 20 righe. il *conteggio delle righe* si comporta in modo diverso quando viene assegnato come valore di input rispetto a un valore restituito.  
  
|Come valore di input|Come valore restituito|  
|--------------------|---------------------|  
|Quando viene specificato il valore di AUTO_FETCH *scrollopt* , il *conteggio* delle righe rappresenta il numero di righe da inserire nel buffer di recupero.<br /><br /> Nota: >0 è un valore valido quando si specifica AUTO_FETCH, ma in caso contrario viene ignorato.|Rappresenta il numero di righe nel set di risultati, tranne quando viene specificato il valore *scrollopt* AUTO_FETCH.|  
  
-  
  
 *boundparam*  
 Indica l'utilizzo di parametri aggiuntivi. *boundparam* è un parametro facoltativo che deve essere specificato se il valore di PARAMETERIZED_STMT *scrollopt* è impostato su on.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Se non viene generato alcun errore, sp_cursoropen restituisce uno dei valori seguenti.  
  
 0  
 La routine è stata effettuata correttamente.  
  
 0x0001  
 Si verificato un errore durante l'esecuzione (un errore minore, non abbastanza grave da compromettere l'operazione).  
  
 0x0002  
 È in corso un'operazione asincrona.  
  
 0x0002  
 È in corso l'elaborazione di un'operazione FETCH.  
  
 A  
 Questo cursore è stato deallocato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e non è disponibile.  
  
 Quando viene generato un errore, è possibile che i valori restituiti siano incoerenti. L'accuratezza non può pertanto essere garantita.  
  
 Quando il parametro *RowCount* viene specificato come valore restituito, viene generato il set di risultati seguente.  
  
 -1  
 Valore restituito se il numero di righe è sconosciuto o non applicabile.  
  
 -n  
 Valore restituito quando un popolamento asincrono è attivo. Rappresenta il numero di righe inserite nel buffer di recupero quando viene specificato il valore *scrollopt* AUTO_FETCH.  
  
 Se RPC è in uso, i valori restituiti sono come segue.  
  
 0  
 La routine è stata eseguita correttamente.  
  
 1  
 La routine non è riuscita.  
  
 2  
 È in corso la generazione asincrona di un cursore keyset.  
  
 16  
 Un cursore di avanzamento rapido è stato chiuso automaticamente.  
  
> [!NOTE]  
>  Se la sp_cursoropen procedura viene eseguita correttamente, vengono inviati i parametri RPC return e un set di risultati con le informazioni sul formato della colonna TDS (messaggi 0XA0 & 0xA1 messages). Se non riesce, vengono inviati uno o più messaggi di errore TDS. In entrambi i casi, non verrà restituito alcun dato di riga e il numero di messaggi *eseguiti* sarà zero. Se si utilizza una versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] precedente a 7.0, vengono restituiti i messaggi 0xa0, 0xa1 (standard per le istruzioni SELECT) insieme ai flussi di token 0xa5 e 0xa4. Se si utilizza [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0, viene restituito 0x81 (standard per le istruzioni SELECT) insieme ai flussi di token 0xa5 e 0xa4.  
  
## <a name="remarks"></a>Commenti  
  
## <a name="stmt-parameter"></a>Parametro stmt  
 Se *stmt* specifica l'esecuzione di una stored procedure, i parametri di input possono essere definiti come costanti come parte della stringa *stmt* o specificati come argomenti *boundparam* . È possibile passare variabili dichiarate come parametri associati in questo modo.  
  
 I contenuti consentiti del parametro *stmt* dipendono dal fatto che il valore restituito da *ccopt* ALLOW_DIRECT sia stato collegato da o al resto dei valori *ccopt* , ad esempio:  
  
-   Se ALLOW_DIRECT viene omesso, è [!INCLUDE[tsql](../../includes/tsql-md.md)] necessario utilizzare un'istruzione SELECT o Execute che chiama per un stored procedure contenente una singola istruzione SELECT. L'istruzione SELECT deve inoltre essere qualificata come cursore, ovvero non può contenere le parole chiave SELECT INTO o FOR BROWSE.  
  
-   Se viene specificato ALLOW_DIRECT, è possibile che vengano eseguite una o più istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)], incluse quelle che, a loro volta, eseguono altre stored procedure con più istruzioni. Le istruzioni non SELECT o qualsiasi istruzione SELECT che contenga le parole chiave SELECT INTO o FOR BROWSE verranno semplicemente eseguite e non comporteranno la creazione di un cursore. Questo vale per qualsiasi istruzione SELECT inclusa in un batch di più istruzioni. Nei casi in cui un'istruzione SELECT contiene clausole che riguardano solo i cursori, tali clausole vengono ignorate. Ad esempio, quando il valore di *ccopt* è 0x2002, si tratta di una richiesta per:  
  
    -   Un cursore con blocchi di scorrimento, se una sola istruzione SELECT è qualificata come cursore oppure  
  
    -   Un'esecuzione diretta di istruzioni in caso di presenza di più istruzioni, di una sola istruzione non SELECT o di un'istruzione SELECT non qualificata come cursore.  
  
## <a name="scrollopt-parameter"></a>Parametro scrollopt  
 I primi cinque valori *scrollopt* (KEYSEY, DYNAMIC, FORWARD_ONLY, STATIC e FAST_FORWARD) si escludono a vicenda.  
  
 PARAMETERIZED_STMT e CHECK_ACCEPTED_TYPES possono essere collegati tramite OR a uno dei primi cinque valori.  
  
 AUTO_FETCH e AUTO_CLOSE possono essere collegati tramite OR a FAST_FORWARD.  
  
 Se CHECK_ACCEPTED_TYPES è ON, è necessario che sia ON almeno uno degli ultimi cinque valori *scrollopt* (KEYSET_ACCEPTABLE `,` DYNAMIC_ACCEPTABLE, FORWARD_ONLY_ACCEPTABLE, STATIC_ACCEPTABLE o FAST_FORWARD_ACCEPTABLE).  
  
 I cursori STATIC sono sempre aperti come READ_ONLY. Ciò significa che non è possibile aggiornare la tabella sottostante tramite questo cursore.  
  
## <a name="ccopt-parameter"></a>Parametro ccopt  
 I primi quattro valori *ccopt* (READ_ONLY, SCROLL_LOCKS e entrambi i valori ottimistici) si escludono a vicenda.  
  
> [!NOTE]  
>  La scelta di uno dei primi quattro valori di *ccopt* determina se il cursore è di sola lettura o se vengono utilizzati metodi di blocco o ottimistico per evitare gli aggiornamenti persi. Se non si specifica un valore *ccopt* , il valore predefinito è ottimistico.  
  
 ALLOW_DIRECT e CHECK_ACCEPTED_TYPES possono essere collegati tramite OR a uno dei primi quattro valori.  
  
 UPDT_IN_PLACE può essere collegato tramite OR a READ_ONLY, SCROLL_LOCKS o a uno dei valori OPTIMISTIC.  
  
 Se CHECK_ACCEPTED_TYPES è ON, è necessario che sia ON almeno uno degli ultimi quattro valori *ccopt* (READ_ONLY_ACCEPTABLE, SCROLL_LOCKS_ACCEPTABLE e uno dei valori OPTIMISTIC_ACCEPTABLE).  
  
 È possibile eseguire le funzioni di aggiornamento e eliminazione posizionate solo all'interno del buffer di recupero e solo se il valore *ccopt* è uguale SCROLL_LOCKS o ottimistico. Se SCROLL_LOCKS è il valore specificato, la riuscita dell'operazione è garantita. Se OPTIMISTIC è il valore specificato, l'operazione non riuscirà se la riga è stata modificata successivamente all'ultimo recupero.  
  
 Il motivo di questo errore è che, quando il valore specificato è OPTIMISTIC, viene eseguita una funzione di controllo della concorrenza ottimistica mediante il confronto di timestamp o valori di checksum, come determinato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se una delle righe non corrisponde, l'operazione non riesce.  
  
 Specificando UPDT_IN_PLACE come valore restituito, si ottengono i risultati seguenti:  
  
-   Se non viene impostato durante l'esecuzione di un aggiornamento posizionato in una tabella con un indice univoco, il cursore elimina la riga dalla rispettiva tabella di lavoro e la inserisce alla fine di una delle colonne chiave utilizzate dal cursore, modificando in questo modo le colonne.  
  
-   Se impostato su ON, il cursore semplicemente aggiornerà le colonne chiave nella riga originale della tabella di lavoro.  
  
## <a name="bound_param-parameter"></a>Parametro bound_param  
 Il nome del parametro deve essere *ParamDef* quando si specifica PARAMETERIZED_STMT, in base al messaggio di errore nel codice. Quando non è specificato PARAMETERIZED_STMT, nel messaggio di errore non è specificato alcun nome.  
  
## <a name="rpc-considerations"></a>Considerazioni su RPC  
 Per richiedere che vengano restituiti metadati sull'elenco di selezione del cursore nel flusso TDS, è possibile impostare il flag di input RPC RETURN_METADATA su 0x0001.  
  
## <a name="examples"></a>Esempio  
  
### <a name="bound_param-parameter"></a>Parametro bound_param  
 I parametri dopo il quinto vengono passati insieme sul piano dell'istruzione come parametri di input. Il primo parametro di questo tipo deve essere una stringa nel formato:  
  
 *{tipo di dati nome variabile locale} [,... n*  
  
 I parametri successivi vengono utilizzati per passare i valori da sostituire con il *nome della variabile locale* nell'istruzione.  
  
## <a name="see-also"></a>Vedere anche  
 [sp_cursorfetch &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cursorfetch-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
