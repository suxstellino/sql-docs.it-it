---
description: Informazioni sui diversi tipi di cursore che è possibile usare per le query dell'applicazione e quando usarli.
title: Informazioni sui tipi di cursore
ms.custom: ''
ms.date: 03/22/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 4f4d3db7-4f76-450d-ab63-141237a4f034
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 77f134dc3be879586cc9e20c4e0048b88ad2e5b5
ms.sourcegitcommit: c09ef164007879a904a376eb508004985ba06cf0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/23/2021
ms.locfileid: "104890633"
---
# <a name="understanding-cursor-types"></a>Informazioni sui tipi di cursore

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Nei database relazionali le operazioni vengono eseguite su set di righe completi. Il set di righe restituito da un'istruzione SELECT include tutte le righe che soddisfano le condizioni specificate nella clausola WHERE dell'istruzione. Il set di righe completo restituito dall'istruzione è noto come set di risultati. Le applicazioni non possono sempre funzionare in modo efficace con l'intero set di risultati come unità. Queste applicazioni richiedono un modo per lavorare con una riga o un piccolo blocco di righe alla volta. I cursori sono un'estensione dei set di risultati che implementano appunto tale meccanismo.

I cursori estendono l'elaborazione del set di risultati eseguendo gli elementi seguenti:

- Consentono il posizionamento su righe specifiche del set di risultati.
- Recuperano una riga o un blocco di righe dalla posizione corrente del set di risultati.
- Supportano le modifiche ai dati della riga in corrispondenza della posizione corrente nel set di risultati.
- Supporto di livelli diversi di visibilità per le modifiche apportate ai dati del database da altri utenti presentati nel set di risultati.

> [!NOTE]
> Per una descrizione completa dei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipi di cursore, vedere [tipo di cursori](../../relational-databases/cursors.md#type-of-cursors).

La specifica JDBC fornisce supporto per cursori forward-only e scorrevoli sensibili o non sensibili alle modifiche apportate da altri processi e che possono essere di sola lettura o aggiornabili. Questa funzionalità viene fornita dalla classe [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)][SQLServerResultSet](reference/sqlserverresultset-class.md).

## <a name="remarks"></a>Osservazioni

Il driver JDBC supporta il set di risultati e i tipi di cursore seguenti insieme alle opzioni di comportamento specificate.

|Tipo di set di risultati (cursore)|Tipo di cursore di SQL Server|Caratteristiche|Select (metodo)|Buffering delle risposte|
|------------------------------------|----------------------------|---------------------|-----------------------|----------------------------|
|TYPE_FORWARD_ONLY (CONCUR_READ_ONLY)|N/D|Forward-only, di sola lettura|dirette|completi|

L'applicazione deve eseguire una singola iterazione (in avanti) nel set di risultati. Questo passaggio è il comportamento predefinito e si comporta come un cursore TYPE_SS_DIRECT_FORWARD_ONLY. Tramite il driver viene letto l'intero set di risultati dal server caricandolo in memoria durante l'esecuzione dell'istruzione.

|Tipo di set di risultati (cursore)|Tipo di cursore di SQL Server|Caratteristiche|Select (metodo)|Buffering delle risposte|
|------------------------------------|----------------------------|---------------------|-----------------------|----------------------------|
|TYPE_FORWARD_ONLY (CONCUR_READ_ONLY)|N/D|Forward-only, di sola lettura|dirette|completi|

L'applicazione deve eseguire una singola iterazione (in avanti) nel set di risultati. Questo passaggio è il comportamento predefinito e si comporta come un cursore TYPE_SS_DIRECT_FORWARD_ONLY. Tramite il driver viene letto l'intero set di risultati dal server caricandolo in memoria durante l'esecuzione dell'istruzione.

|Tipo di set di risultati (cursore)|Tipo di cursore di SQL Server|Caratteristiche|Select (metodo)|Buffering delle risposte|
|------------------------------------|----------------------------|---------------------|-----------------------|----------------------------|
|TYPE_FORWARD_ONLY (CONCUR_READ_ONLY)|N/D|Forward-only, di sola lettura|dirette|adaptive|

L'applicazione deve eseguire una singola iterazione (in avanti) nel set di risultati. Il comportamento corrisponde a quello di un cursore TYPE_SS_DIRECT_FORWARD_ONLY. Il driver legge le righe dal server mentre l'applicazione le richiede e riduce al minimo l'utilizzo della memoria sul lato client.

|Tipo di set di risultati (cursore)|Tipo di cursore di SQL Server|Caratteristiche|Select (metodo)|Buffering delle risposte|
|------------------------------------|----------------------------|---------------------|-----------------------|----------------------------|
|TYPE_FORWARD_ONLY (CONCUR_READ_ONLY)|Fast forward|Forward-only, di sola lettura|cursor|N/D|

L'applicazione deve eseguire una singola iterazione (in avanti) nel set di risultati utilizzando un cursore server. Il comportamento è analogo a quello di un cursore TYPE_SS_SERVER_CURSOR_FORWARD_ONLY.

Le righe vengono recuperate dal server in blocchi specificati dalla dimensione di recupero.

|Tipo di set di risultati (cursore)|Tipo di cursore di SQL Server|Caratteristiche|Select (metodo)|Buffering delle risposte|
|------------------------------------|----------------------------|---------------------|-----------------------|----------------------------|
|TYPE_FORWARD_ONLY (CONCUR_UPDATABLE)|Dinamico (forward-only)|Forward-only, aggiornabile|N/D|N/D|

L'applicazione deve eseguire una singola iterazione (in avanti) nel set di risultati per aggiornare una o più righe.

Le righe vengono recuperate dal server in blocchi specificati dalla dimensione di recupero.

Per impostazione predefinita, la dimensione di recupero viene fissata quando l'applicazione chiama il metodo [setFetchSize](reference/setfetchsize-method-sqlserverresultset.md) dell'oggetto [SQLServerResultSet](reference/sqlserverresultset-class.md).

> [!NOTE]
> il driver JDBC include una caratteristica di buffer adattivo che consente di recuperare i risultati dell'esecuzione di istruzioni da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] quando sono necessari per l'applicazione anziché tutti contemporaneamente. Se ad esempio l'applicazione deve recuperare una quantità di dati eccessiva per essere caricata completamente in memoria, il buffer adattivo consente all'applicazione client di recuperare tali dati sotto forma di flusso. Il comportamento predefinito del driver è "**adaptive**". Per ottenere il buffer adattivo per i set di risultati aggiornabili forward-only, l'applicazione deve tuttavia chiamare in modo esplicito il metodo [setResponseBuffering](reference/setresponsebuffering-method-sqlserverstatement.md) dell'oggetto [SQLServerStatement](reference/sqlserverstatement-class.md) offrendo un valore **Stringa** "**adaptive"**. Per un esempio di codice, vedere [Esempio di aggiornamento di dati di grandi dimensioni](updating-large-data-sample.md).

|Tipo di set di risultati (cursore)|Tipo di cursore di SQL Server|Caratteristiche|Select (metodo)|Buffering delle risposte|
|------------------------------------|----------------------------|---------------------|-----------------------|----------------------------|
|TYPE_SCROLL_INSENSITIVE|Static|Scorrevole, non aggiornabile<br /><br /> Gli aggiornamenti, gli inserimenti e le eliminazioni di righe esterne non sono visibili.|N/D|N/D|

L'applicazione richiede uno snapshot del database. Il set di risultati non è aggiornabile. È supportato solo CONCUR_READ_ONLY.  Tutti gli altri tipi di concorrenza generano un'eccezione, se utilizzati con questo tipo di cursore.

Le righe vengono recuperate dal server in blocchi specificati dalla dimensione di recupero.

|Tipo di set di risultati (cursore)|Tipo di cursore di SQL Server|Caratteristiche|Select (metodo)|Buffering delle risposte|
|------------------------------------|----------------------------|---------------------|-----------------------|----------------------------|
|TYPE_SCROLL_SENSITIVE (CONCUR_READ_ONLY)|Keyset|Scorrevole, sola lettura. Gli aggiornamenti di righe eseguiti esternamente sono visibili e le eliminazioni vengono visualizzate come dati mancanti.<br /><br /> Gli inserimenti di riga esterni non sono visibili.|N/D|N/D|

Nell'applicazione devono essere visualizzati i dati modificati solo per le righe esistenti.

Le righe vengono recuperate dal server in blocchi specificati dalla dimensione di recupero.

|Tipo di set di risultati (cursore)|Tipo di cursore di SQL Server|Caratteristiche|Select (metodo)|Buffering delle risposte|
|------------------------------------|----------------------------|---------------------|-----------------------|----------------------------|
|TYPE_SCROLL_SENSITIVE (CONCUR_UPDATABLE, CONCUR_SS_SCROLL_LOCKS, CONCUR_SS_OPTIMISTIC_CC, CONCUR_SS_OPTIMISTIC_CCVAL)|Keyset|Scorrevole, aggiornabile<br /><br /> Gli aggiornamenti delle righe esterni e interni sono visibili e le eliminazioni vengono visualizzate come dati mancanti; gli inserimenti non sono visibili.|N/D|N/D|

L'applicazione può modificare i dati nelle righe esistenti tramite l'oggetto ResultSet. L'applicazione deve inoltre essere in grado di visualizzare le modifiche apportate alle righe da altri esternamente all'oggetto ResultSet.

Le righe vengono recuperate dal server in blocchi specificati dalla dimensione di recupero.

|Tipo di set di risultati (cursore)|Tipo di cursore di SQL Server|Caratteristiche|Select (metodo)|Buffering delle risposte|
|------------------------------------|----------------------------|---------------------|-----------------------|----------------------------|
|TYPE_SS_DIRECT_FORWARD_ONLY|N/D|Forward-only, di sola lettura|N/D|full o adaptive|

Valore intero = 2003. Fornisce un cursore sul lato client di sola lettura completamente memorizzato nel buffer. Non viene creato alcun cursore server.

È supportato solo il tipo di concorrenza CONCUR_READ_ONLY. Tutti gli altri tipi di concorrenza generano un'eccezione, se utilizzati con questo tipo di cursore.

|Tipo di set di risultati (cursore)|Tipo di cursore di SQL Server|Caratteristiche|Select (metodo)|Buffering delle risposte|
|------------------------------------|----------------------------|---------------------|-----------------------|----------------------------|
|TYPE_SS_SERVER_CURSOR_FORWARD_ONLY|Fast forward|Forward-only|N/D|N/D|

Valore intero = 2004. Accede rapidamente a tutti i dati tramite un cursore del server. È aggiornabile quando viene usato con CONCUR_UPDATABLE tipo di concorrenza.

Le righe vengono recuperate dal server in blocchi specificati dalla dimensione di recupero.

Per ottenere il buffer adattivo per questo caso, l'applicazione deve chiamare in modo esplicito il metodo [setResponseBuffering](reference/setresponsebuffering-method-sqlserverstatement.md) dell'oggetto [SQLServerStatement](reference/sqlserverstatement-class.md) fornendo un valore **stringa** "**Adaptive"**. Per un esempio di codice, vedere [Esempio di aggiornamento di dati di grandi dimensioni](updating-large-data-sample.md).

|Tipo di set di risultati (cursore)|Tipo di cursore di SQL Server|Caratteristiche|Select (metodo)|Buffering delle risposte|
|------------------------------------|----------------------------|---------------------|-----------------------|----------------------------|
|TYPE_SS_SCROLL_STATIC|Static|Gli aggiornamenti di altri utenti non vengono riflessi.|N/D|N/D|

Valore intero = 1004. L'applicazione richiede uno snapshot del database. Questa opzione è il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sinonimo specifico di per il TYPE_SCROLL_INSENSITIVE JDBC e ha lo stesso comportamento dell'impostazione di concorrenza.

Le righe vengono recuperate dal server in blocchi specificati dalla dimensione di recupero.

|Tipo di set di risultati (cursore)|Tipo di cursore di SQL Server|Caratteristiche|Select (metodo)|Buffering delle risposte|
|------------------------------------|----------------------------|---------------------|-----------------------|----------------------------|
|TYPE_SS_SCROLL_KEYSET (CONCUR_READ_ONLY)|Keyset|Scorrevole, sola lettura. Gli aggiornamenti di righe eseguiti esternamente sono visibili e le eliminazioni vengono visualizzate come dati mancanti.<br /><br /> Gli inserimenti di riga esterni non sono visibili.|N/D|N/D|

Valore intero = 1005. Nell'applicazione devono essere visualizzati i dati modificati per le righe esistenti. Questa opzione è il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sinonimo specifico di per il TYPE_SCROLL_SENSITIVE JDBC e ha lo stesso comportamento dell'impostazione di concorrenza.

Le righe vengono recuperate dal server in blocchi specificati dalla dimensione di recupero.

|Tipo di set di risultati (cursore)|Tipo di cursore di SQL Server|Caratteristiche|Select (metodo)|Buffering delle risposte|
|------------------------------------|----------------------------|---------------------|-----------------------|----------------------------|
|TYPE_SS_SCROLL_KEYSET (CONCUR_UPDATABLE, CONCUR_SS_SCROLL_LOCKS, CONCUR_SS_OPTIMISTIC_CC, CONCUR_SS_OPTIMISTIC_CCVAL)|Keyset|Scorrevole, aggiornabile<br /><br /> Gli aggiornamenti delle righe esterni e interni sono visibili e le eliminazioni vengono visualizzate come dati mancanti; gli inserimenti non sono visibili.|N/D|N/D|

Valore intero = 1005. Nell'applicazione devono essere modificati i dati oppure devono essere visualizzati i dati modificati solo per le righe esistenti. Questa opzione è il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sinonimo specifico di per il TYPE_SCROLL_SENSITIVE JDBC e ha lo stesso comportamento dell'impostazione di concorrenza.

Le righe vengono recuperate dal server in blocchi specificati dalla dimensione di recupero.

|Tipo di set di risultati (cursore)|Tipo di cursore di SQL Server|Caratteristiche|Select (metodo)|Buffering delle risposte|
|------------------------------------|----------------------------|---------------------|-----------------------|----------------------------|
|TYPE_SS_SCROLL_DYNAMIC (CONCUR_READ_ONLY)|Dinamico|Scorrevole, sola lettura.<br /><br /> Gli aggiornamenti e gli inserimenti di righe eseguiti esternamente sono visibili e le eliminazioni vengono visualizzate come dati mancanti temporanei nel buffer di recupero corrente.|N/D|N/D|

Valore intero = 1006. Nell'applicazione devono essere visualizzati i dati modificati per le righe esistenti, nonché le righe inserite ed eliminate nel corso della durata del cursore.

Le righe vengono recuperate dal server in blocchi specificati dalla dimensione di recupero.

|Tipo di set di risultati (cursore)|Tipo di cursore di SQL Server|Caratteristiche|Select (metodo)|Buffering delle risposte|
|------------------------------------|----------------------------|---------------------|-----------------------|----------------------------|
|TYPE_SS_SCROLL_DYNAMIC (CONCUR_UPDATABLE, CONCUR_SS_SCROLL_LOCKS, CONCUR_SS_OPTIMISTIC_CC, CONCUR_SS_OPTIMISTIC_CCVAL)|Dinamico|Scorrevole, aggiornabile<br /><br /> Gli aggiornamenti e gli inserimenti di righe eseguiti esternamente e internamente sono visibili e le eliminazioni vengono visualizzate come dati mancanti temporanei nel buffer di recupero corrente.|N/D|N/D|

Valore intero = 1006. L'applicazione può modificare i dati per le righe esistenti oppure inserire o eliminare righe tramite l'oggetto ResultSet. L'applicazione deve inoltre essere in grado di visualizzare le modifiche, gli inserimenti e le eliminazioni apportate da altri esternamente all'oggetto ResultSet.

Le righe vengono recuperate dal server in blocchi specificati dalla dimensione di recupero.

## <a name="cursor-positioning"></a>Posizionamento del cursore

I cursori TYPE_FORWARD_ONLY, TYPE_SS_DIRECT_FORWARD_ONLY e TYPE_SS_SERVER_CURSOR_FORWARD_ONLY supportano solo il metodo di posizionamento [next](../../connect/jdbc/reference/next-method-sqlserverresultset.md).

Il TYPE_SS_SCROLL_DYNAMIC cursore non supporta i metodi [Absolute](reference/absolute-method-sqlserverresultset.md) e [GetRow](reference/getrow-method-sqlserverresultset.md) . È possibile ottenere un risultato simile a quello del metodo absolute tramite una combinazione di chiamate ai metodi [first](reference/first-method-sqlserverresultset.md) e [relative](reference/relative-method-sqlserverresultset.md) per i cursori dinamici.

Il metodo getRow è supportato solo dai cursori TYPE_FORWARD_ONLY, TYPE_SS_DIRECT_FORWARD_ONLY, TYPE_SS_SERVER_CURSOR_FORWARD_ONLY, TYPE_SS_SCROLL_KEYSET e TYPE_SS_SCROLL_STATIC. Il metodo getRow con tutti i tipi di cursore forward-only restituisce il numero di righe lette fino a quel momento tramite il cursore.

> [!NOTE]
> Quando un'applicazione esegue una chiamata non supportata ai metodi di posizionamento del cursore o al metodo getRow, viene generata un'eccezione con il messaggio "L'operazione richiesta non è supportata con questo tipo di cursore".

Solo il cursore TYPE_SS_SCROLL_KEYSET e l'equivalente TYPE_SCROLL_SENSITIVE espongono le righe eliminate. Se il cursore viene posizionato in una riga eliminata, i valori della colonna non sono disponibili e il metodo [rowDeleted](reference/rowdeleted-method-sqlserverresultset.md) restituisce "true". Le chiamate ai metodi get\<Type> generano un'eccezione con il messaggio "Impossibile ottenere un valore da una riga eliminata". Le righe eliminate non possono essere aggiornate. Se si tenta di chiamare un metodo update\<Type> per una riga eliminata, viene generata un'eccezione con il messaggio "Non è possibile aggiornare una riga eliminata". Il cursore TYPE_SS_SCROLL_DYNAMIC ha lo stesso comportamento fino a quando non viene tolto dal buffer di recupero corrente.

I cursori forward e dinamici espongono le righe eliminate in modo simile, ma solo fino a quando rimangono accessibili nel buffer di recupero. Per i cursori di avanzamento, questo comportamento è piuttosto semplice. Per i cursori dinamici, è più complessa quando la dimensione del recupero è maggiore di 1. Un'applicazione può spostare il cursore avanti e indietro nella finestra definita dal buffer di recupero, ma la riga eliminata non viene più visualizzata all'uscita dal buffer di recupero originale in cui è stata aggiornata. Se un'applicazione non desidera visualizzare le righe eliminate temporanee tramite cursori dinamici, è necessario utilizzare un oggetto fetch relativo (0).

Se i valori di chiave di una riga TYPE_SS_SCROLL_KEYSET o TYPE_SCROLL_SENSITIVE cursore vengono aggiornati con il cursore, la riga mantiene la posizione originale nel set di risultati, indipendentemente dal fatto che la riga aggiornata soddisfi i criteri di selezione del cursore. Se la riga è stata aggiornata all'esterno del cursore, nella posizione originale della riga verrà visualizzata una riga eliminata, ma la riga verrà visualizzata nel cursore solo se nel cursore era presente un'altra riga con i nuovi valori chiave, che tuttavia è stata eliminata.

Per i cursori dinamici, le righe aggiornate manterranno la propria posizione all'interno del buffer di recupero fino a quando non viene lasciata la finestra definita dal buffer di recupero. Le righe aggiornate potrebbero essere visualizzate successivamente in posizioni diverse all'interno del set di risultati o potrebbero scomparire completamente. Per le applicazioni in cui è necessario evitare le incoerenze temporanee nel set di risultati, è necessario utilizzare una dimensione di recupero pari a 1. L'impostazione predefinita è 8 righe con concorrenza CONCUR_SS_SCROLL_LOCKS e 128 righe con altre concorrenze.

## <a name="cursor-conversion"></a>Conversione del cursore

A volte, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può venire implementato un tipo di cursore diverso da quello richiesto e questo comportamento è noto come conversione implicita del cursore o degradazione del cursore.

Con [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)], quando si aggiornano i dati tramite il set di risultati ResultSet.TYPE_SCROLL_SENSITIVE e ResultSet.CONCUR_UPDATABLE, viene generata un'eccezione con un messaggio "il cursore è READ ONLY". Questa eccezione si verifica perché [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] in è stata eseguita una conversione implicita del cursore per il set di risultati e non è stato restituito il cursore aggiornabile richiesto.

Per risolvere questo problema, è possibile eseguire una delle soluzioni seguenti:

- Verificare che la tabella sottostante disponga di una chiave primaria.
- Usare [SQLServerResultSet.TYPE_SS_SCROLL_DYNAMIC](reference/type-ss-scroll-dynamic-field-sqlserverresultset.md) anziché ResultSet.TYPE_SCROLL_SENSITIVE durante la creazione di un'istruzione.

## <a name="cursor-updating"></a>Aggiornamento del cursore

Gli aggiornamenti sul posto sono supportati per i cursori se la concorrenza e il tipo di cursore supportano gli aggiornamenti. Se il cursore non è posizionato in una riga aggiornabile nel set di risultati (nessuna \<Type> chiamata al metodo Get ha avuto esito positivo), una chiamata a un metodo di aggiornamento \<Type> genererà un'eccezione con il messaggio "il set di risultati non ha una riga corrente". In base alla specifica JDBC viene generata un'eccezione quando viene chiamato un metodo di aggiornamento per una colonna di un cursore di tipo CONCUR_READ_ONLY. Nei casi in cui la riga non è aggiornabile, ad esempio a causa di un conflitto di concorrenza ottimistica come un aggiornamento o un'eliminazione in conflitto, l'eccezione potrebbe non venire generata fino a quando non viene chiamato il metodo [insertRow](reference/insertrow-method-sqlserverresultset.md), [updateRow](reference/updaterow-method-sqlserverresultset.md) o [deleteRow](reference/deleterow-method-sqlserverresultset.md).

Dopo una chiamata a update\<Type>, non è possibile accedere alla colonna interessata usando get\<Type> finché non viene chiamato il metodo updateRow o [cancelRowUpdates](reference/cancelrowupdates-method-sqlserverresultset.md). Questo comportamento consente di evitare problemi a causa del quale una colonna viene aggiornata usando un tipo diverso dal tipo restituito dal server e le chiamate al Getter successive potrebbero richiamare conversioni di tipi lato client che forniscono risultati non accurati. Le chiamate a get\<Type> generano un'eccezione con il messaggio "Impossibile accedere alle colonne aggiornate fino a quando non è stato chiamato updateRow() o cancelRowUpdates()".

> [!NOTE]
> Se il metodo updateRow viene chiamato quando non è stata aggiornata alcuna colonna, il driver JDBC genera un'eccezione con un messaggio che indica che è stato chiamato il metodo updateRow () senza che siano state aggiornate le colonne.

Dopo la chiamata a [moveToInsertRow](reference/movetoinsertrow-method-sqlserverresultset.md), viene generata un'eccezione se nel set di risultati viene chiamato qualsiasi metodo ad eccezione di get\<Type>, update\<Type>, insertRow e dei metodi di posizionamento del cursore, incluso [moveToCurrentRow](reference/movetocurrentrow-method-sqlserverresultset.md). Il metodo moveToInsertRow consente di attivare in modo efficace la modalità di inserimento per il set di risultati e i metodi di posizionamento del cursore consentono di interrompere tale modalità. Le chiamate ai metodi di posizionamento relativo del cursore consentono di spostare il cursore rispetto alla posizione in cui si trovava prima della chiamata al metodo moveToInsertRow. Dopo le chiamate ai metodi di posizionamento del cursore, la posizione di destinazione finale del cursore diventa la nuova posizione del cursore.

Se la chiamata di posizionamento del cursore eseguita in modalità di inserimento non riesce, la posizione del cursore dopo la chiamata non riuscita è la posizione del cursore originale prima della chiamata a moveToInsetRow. Se il metodo insertRow ha esito negativo, il cursore rimane nella riga di inserimento e in modalità di inserimento.

Inizialmente, le colonne nella riga di inserimento hanno uno stato non inizializzato. Le chiamate al metodo update\<Type> consentono di impostare lo stato delle colonne come inizializzato. Una chiamata al metodo get\<Type> per una colonna non inizializzata genera un'eccezione. Una chiamata al metodo insertRow ripristina lo stato non inizializzato per tutte le colonne nella riga di inserimento.

Se quando viene chiamato il metodo insertRow vi sono colonne non inizializzate, in tali colonne viene inserito il relativo valore predefinito. Se non è presente alcun valore predefinito ma la colonna ammette valori NULL, viene inserito NULL. Se non è presente alcun valore predefinito e la colonna non ammette valori null, il server restituirà un errore e verrà generata un'eccezione.

> [!NOTE]
> Le chiamate al metodo getRow eseguite in modalità di inserimento restituiscono 0.
>
> Il driver JDBC non supporta eliminazioni o aggiornamenti posizionati. In base alla specifica JDBC, il metodo [setCursorName](reference/setcursorname-method-sqlserverstatement.md) non ha effetto e il metodo [getCursorName](reference/getcursorname-method-sqlserverresultset.md) quando viene chiamato genera un'eccezione.
>
> I cursori di sola lettura e statici non sono mai aggiornabili.
>
> In SQL Server i cursori server sono limitati a un singolo set di risultati. Se un batch o una stored procedure contiene più istruzioni, è necessario utilizzare un cursore client di sola lettura forward-only.

## <a name="see-also"></a>Vedere anche

[Gestione dei set di risultati con il driver JDBC](managing-result-sets-with-the-jdbc-driver.md)  
