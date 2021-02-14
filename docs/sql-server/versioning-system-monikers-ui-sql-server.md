---
description: Sistema di versioni per la documentazione SQL
title: Documentazione SQL del sistema di controllo delle versioni
ms.date: 08/12/2020
ms.prod: sql
ms.technology: release-landing
ms.topic: conceptual
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||=azuresqldb-mi-current||=azure-sqldw-latest||>=aps-pdw-2016||>=sql-server-linux-2017||>=sql-server-2016
ms.openlocfilehash: 56ef939790de90d02add427f14f8817df885383f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079752"
---
# <a name="versioning-system-for-sql-documentation"></a>Sistema di versioni per la documentazione SQL

[!INCLUDE[includes_appliesto-ss-asdb-asdw-pdw-md.md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

Questo articolo illustra il _sistema di versioni_ per la documentazione SQL. Il sistema di versioni distingue i prodotti e le relative versioni e consente di scegliere il prodotto e la versione a cui si è interessati, visualizzando la documentazione appropriata.

## <a name="applies-to-products"></a>Dicitura SI APPLICA A

Sotto il titolo della maggior parte degli articoli relativi a SQL Server è riportata la dicitura **SI APPLICA A**. Sulla stessa riga viene visualizzato un elenco di _prodotti_ SQL per indicare se l'articolo è rilevante per il prodotto. Ad esempio, il prodotto SQL Server potrebbe essere indicato come rilevante, mentre il database SQL di Azure potrebbe essere indicato come irrilevante per l'articolo.

La riga **SI APPLICA A** non distingue le _versioni_ dei prodotti. Microsoft si impegna a evitare discrepanze tra la riga **SI APPLICA A** e l'aspetto dei prodotti delle configurazioni del sistema di versioni.

## <a name="history-of-separate-file-sets"></a>Cronologia di set di file separati

Per SQL Server 2014 e versioni precedenti, ogni versione ha la propria copia separata completa dei file della documentazione. Ad esempio, la documentazione per SQL Server 2014 è iniziata come copia della documentazione per SQL Server 2012. La copia per la versione 2014 è stata quindi modificata durante il ciclo di sviluppo del prodotto.

Questo approccio adottato in precedenza implicava che, in caso di errore nella documentazione per la versione 2014, lo stesso errore poteva essere presente anche per le versioni 2012 e 2008. Ciò rendeva più difficile la correzione degli errori e la manutenzione generale.

## <a name="multiple-versions-in-the-same-files"></a>Più versioni negli stessi file

Per questo e altri motivi, i file della documentazione per SQL Server 2016 sono validi anche per le versioni 2017, 2019 e probabilmente per \<vNext\>. Questo consolidamento è stato attuato perché ora si assegnano _moniker di controllo delle versioni_ ai file della documentazione di SQL Server. I moniker di controllo delle versioni vengono assegnati, o sono incorporati in modo esplicito, a qualsiasi livello di granularità che risulti appropriato per ogni file della documentazione specificato.

## <a name="versioning-control-in-the-ui"></a>Controllo delle versioni nell'interfaccia utente

Quando si visualizza un articolo della documentazione di SQL tramite il sito Web :::no-loc text="Docs":::, il moniker di controllo delle versioni attualmente selezionato è visibile sopra il sommario. Il controllo è un elenco a discesa.

![media_versioning-control-10-sql-server-2017.png](media/versioning-control-10-sql-server-2017.png)

Se si vuole visualizzare la documentazione per un'altra versione di SQL Server, fare clic sulla freccia di espansione all'estremità del moniker di controllo delle versioni corrente. Quindi fare clic per scegliere qualsiasi combinazione di prodotto e versione desiderata. Quando si fa clic su una versione diversa, la documentazione visualizzata cambia istantaneamente per mostrare le differenze per la nuova versione scelta. Possono esserci modifiche e possono non esserci, entrambi i casi sono comuni.

![media_versioning-control-20-expanded.png](media/versioning-control-20-expanded.png)

### <a name="https-parameter-no-loc-textview"></a>Parametro HTTPS :::no-loc text="view=":::

Ogni articolo il cui indirizzo Web inizia con `https://docs.microsoft.com/sql/` ha un parametro denominato `?view=` aggiunto all'indirizzo stesso. Il valore di questo parametro è il codice del moniker di controllo delle versioni.

Il _codice_ del moniker nell'indirizzo `https` corrisponde sempre al _nome_ del moniker visualizzato nel controllo delle versioni.

## <a name="products-not-editions"></a>Prodotti, non edizioni

### <a name="editions"></a>Edizioni

Negli anni Novanta e Duemila Microsoft SQL Server aveva un solo prodotto. Erano disponibili diverse _edizioni_ di ogni versione di SQL Server, ad esempio le edizioni _Developer_ ed _Enterprise_ di SQL Server 2008. Le edizioni rappresentavano set di funzionalità leggermente diversi, ma il prodotto principale era lo stesso. Le nuove versioni di SQL Server possono avere ancora una vasta gamma di edizioni.

### <a name="products"></a>Products

Con la più recente crescita del cloud computing e di Microsoft Azure, Microsoft ha rilasciato il prodotto Database SQL di Azure. Per quanto il prodotto tradizionale SQL Server locale e il prodotto Database SQL di Azure condividano molto codice, si tratta di due prodotti effettivamente distinti.

Per SQL, i moniker di controllo delle versioni fanno distinzione tra i prodotti, ma non tra le edizioni.

#### <a name="azure-cloud-sql-products"></a>Prodotti SQL del cloud di Azure

Per quel che riguarda gli articoli i cui indirizzi Web iniziano con `https://docs.microsoft.com/sql/`, quasi tutti si applicano ad almeno una versione del prodotto denominato SQL Server. Un sottoinsieme di grandi dimensioni di questi articoli si applica anche a uno o più prodotti del servizio SQL ospitati nel cloud di Azure. Uno di questi prodotti del cloud SQL è denominato database SQL di Azure.

Naturalmente, il prodotto Database SQL di Azure ha una sola versione. Quasi tutti gli articoli che si applicano al database SQL di Azure, ma non a SQL Server, hanno indirizzi Web che iniziano con `https://docs.microsoft.com/azure/sql-database/`.

## <a name="scenarios-of-version-filtering"></a>Scenari di filtraggio della versione

Il sistema di controllo delle versioni funziona escludendo tramite filtro tutto il contenuto della documentazione che non si applica al moniker correntemente attivo. Ogni volta che si sceglie un moniker di controllo delle versioni diverso, il set di contenuto nascosto cambia. Il filtro nasconde il contenuto ai livelli seguenti:

- Sezioni o frasi in un articolo.
- Voci per gli articoli nel sommario.

Di seguito sono riportati alcuni scenari che illustrano gli effetti della scelta di un moniker diverso.

### <a name="scenario-1-within-the-current-article"></a>Scenario 1: nell'articolo corrente

Lo scenario seguente è incentrato sulle sezioni all'interno dell'articolo corrente:

1. Il moniker di controllo delle versioni corrente è **SQL Server 2017**.
2. Si sta leggendo una sezione che descrive una funzionalità aggiunta per la prima volta alla versione 2017 di SQL Server.
3. Il moniker viene modificato in **SQL Server 2016**.
4. Si noterà che la sezione che si stava leggendo non è più disponibile.
5. Il moniker viene modificato nuovamente, questa volta in **SQL Server 2019**.
6. Si noterà che la sezione 2017 che si stava leggendo è di nuovo visualizzata.

Nello scenario precedente la sezione sulla nuova funzionalità 2017 probabilmente è contrassegnata con un _intervallo di moniker_ che include il codice del moniker seguente:

- `>=sql-server-2017`

Quando si è scelto il moniker **SQL Server 2019**, il sistema di controllo delle versioni ha realizzato che 2019 è maggiore di o uguale a 2017 e ha quindi visualizzato la sezione.

### <a name="scenario-2-click-a-link-to-a-hidden-article"></a>Scenario 2: clic su un collegamento a un articolo nascosto

Lo scenario non comune seguente spiega cosa accade se si fa clic su un collegamento a un articolo attualmente nascosto dal sommario. In breve, il collegamento funziona:

1. Il moniker di controllo delle versioni corrente è **SQL Server 2017**.
2. Nell'articolo corrente :::no-loc text="A"::: si fa clic su un collegamento a un articolo :::no-loc text="B"::: che si applica solo a SQL Server 2016.
    - Prima di aver fatto clic, nel sommario la voce per l'articolo :::no-loc text="B"::: è nascosta.
3. Dopo aver fatto clic, viene visualizzato l'articolo :::no-loc text="B":::.
    - La visualizzazione dell'articolo :::no-loc text="B"::: impone al controllo delle versioni di passare al moniker **SQL Server 2016**.
    - Poiché è stato necessario abbandonare il moniker originale **SQL Server 2017**. Questo abbandono comporta la visualizzazione di un messaggio informativo nella parte superiore della pagina Web. Il [messaggio](#anchor-message-unavailable-for-moniker) spiega che è stato necessario cambiare il moniker corrente per supportare il nuovo articolo :::no-loc text="B":::.

### <a name="scenario-3-navigate-to-an-https-address"></a>Scenario 3: passaggio a un indirizzo https

L'articolo seguente è stato aggiunto nuovo per SQL Server 2017. L'articolo descrive le funzionalità aggiunte a SQL Server nella versione 2017. Tutte queste nuove funzionalità o buona parte di esse fanno parte anche della versione 2019. Ecco gli attributi dell'articolo.

| Attributo | Valore |
| :-------- | :---- |
| Titolo | Novità di SQL Server 2017 |
| intervallo di moniker | `=">= sql-server-2017"` |
| indirizzo `https` | `https://docs.microsoft.com/sql/sql-server/what-s-new-in-sql-server-2017` |
| &nbsp; | &nbsp; |

Dato l'indirizzo `https` di base, la tabella seguente illustra cosa accade quando il parametro `?view=` viene aggiunto dall'utente, con diversi valori.

| Valore di `?view=` | Comportamento della navigazione all'indirizzo `https` |
| :---------------- | :------------------------------ |
| _(Nessun parametro.)_ | Il sistema di controllo delle versioni proverà il valore del moniker predefinito. Questo valore viene in genere impostato sulla versione non di anteprima più recente di SQL Server.<br/><br/>Un valore predefinito uguale a SQL Server 2017 o 2019 soddisferà l'attributo `>= sql-server-2017`.<br/><br/>Il sistema aggiungerà il parametro all'indirizzo `https`, ad esempio nel formato `?view=sql-server-2017`.<br/>L'elenco a discesa di controllo delle versioni verrà quindi impostato sul nome del moniker corrispondente. |
| `sql-server-2016` | Il sistema di controllo delle versioni realizzerà che l'intervallo di moniker dell'articolo non include la versione 2016.<br/><br/>Il sistema sceglierà quindi uno dei moniker che soddisfa l'intervallo.<br/><br/>Come nel caso della versione 2016, verrà aggiunto il parametro `?view=` e il nome del controllo corrisponderà al valore del parametro. |
| `sql-server-2017` | Il sistema di controllo delle versioni comprende che il valore del parametro è incluso nell'intervallo di moniker dell'articolo.<br/><br/>Il controllo delle versioni verrà impostato in modo da corrispondere al valore del parametro. |
| `sql-server-2019` | Uguale al caso del valore `sql-server-2017`, ad eccezione del fatto che il parametro e il controllo sono impostati su 2019. |
| &nbsp; | &nbsp; |

### <a name="all-sql---hide-nothing-special-moniker"></a><a name="anchor-allsql-hidenothing"></a> Moniker speciale All SQL - Hide nothing (Tutto SQL - Visualizza tutto)

Esiste un moniker speciale per il nome del prodotto **All SQL** (Tutto SQL) la cui unica versione è **Hide nothing** (Visualizza tutto). Lo scopo di questo moniker è il test interno di determinate modifiche. Se usato da un cliente, è probabile che questo moniker fornisca informazioni fuorvianti.

Alcuni articoli contengono informazioni relative a più versioni di SQL Server. Ogni moniker regolare nasconde le sezioni relative alle versioni che potrebbero altrimenti visualizzare informazioni non accurate, confuse o contraddittorie per la versione del moniker. Il moniker speciale **All SQL** (Tutto SQL) visualizza invece le sezioni relative a tutte le versioni e potrebbe non essere ovvio che potrebbero essere visualizzate informazioni non accurate.

## <a name="message-the-requested-page-is-not-available-for-moniker"></a><a name="anchor-message-unavailable-for-moniker"></a> Messaggio: La pagina richiesta non è disponibile per \<moniker\>

Lo scenario seguente comporta la visualizzazione di un messaggio informativo nella parte superiore della pagina Web :::no-loc text="Docs"::::

1. Attualmente il moniker di controllo delle versioni è **SQL Server 2017**.
2. Si sta leggendo un articolo pertinente per SQL Server 2017.
    - L'articolo _non_ è pertinente per il prodotto Database SQL di Azure.
3. Si tenta di cambiare il moniker in **Azure SQL Database - current** (Database SQL di Azure - corrente).
4. Si noterà che il tentativo è stato rifiutato e viene visualizzato un messaggio.

Al termine di questo scenario, verrà visualizzato il messaggio informativo seguente nella parte superiore della pagina Web Docs:

> La pagina richiesta non è disponibile per Azure SQL Database - current (Database SQL di Azure - corrente). È stato eseguito il reindirizzamento alla versione più recente del prodotto per cui questa pagina è disponibile.

La versione _più recente_ potrebbe escludere le versioni non ancora completamente rilasciate e in stato di _anteprima_.

![media_versioning-control-30-viewfallbackfrom.png](media/versioning-control-30-viewfallbackfrom.png)

## <a name="previous-versions-of-sql-server"></a>Versioni precedenti di SQL Server

Il sistema di controllo delle versioni è completamente implementato per le versioni da SQL Server 2016 in avanti.

- _2012 e versioni precedenti:_ &nbsp; il sistema di controllo non viene usato per SQL Server 2012 o versioni precedenti.
    - Il moniker speciale **SQL Server - older** (precedente) è destinato a nascondere quasi tutti gli articoli, ad eccezione di pochi articoli di cui i clienti delle versioni precedenti potrebbero aver bisogno.
    - [Versioni precedenti di SQL Server, 2012-2005](./previous-versions-sql-server.md)

- _2014:_ &nbsp; il sistema di controllo delle versioni è implementato a metà per SQL Server 2014. È possibile scegliere SQL Server 2014 nel controllo delle versioni e funziona. Tuttavia, internamente i file per la versione 2014 sono dedicati solo alla versione 2014, allo stesso modo in cui i file per la versione 2008 sono dedicati solo alla versione 2008.
    - [Documentazione offline di SQL Server 2014](./sql-server-offline-documentation.md)

- _2016 e versioni successive:_ &nbsp; il sistema di controllo delle versioni è completamente implementato per SQL Server 2016 e versioni successive.
    - [Documentazione di SQL Server 2016 e versioni successive](./index.yml?preserve-view=true&view=sql-server-2016)
    - [Documentazione offline di SQL Server 2016](sql-server-offline-documentation.md)

## <a name="see-also"></a>Vedere anche

[Versioni precedenti di SQL Server, 2014-2005](./previous-versions-sql-server.md)  
[Guida all'esplorazione della documentazione di SQL Server](sql-docs-navigation-guide.md)  
[Come contribuire alla documentazione di SQL Server](sql-server-docs-contribute.md)