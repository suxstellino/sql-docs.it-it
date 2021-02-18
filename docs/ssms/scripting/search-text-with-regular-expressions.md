---
title: Testo di ricerca con espressioni regolari
description: Informazioni su come usare un'espressione regolare nel campo "Trova" di una finestra di dialogo Trova e sostituisci per specificare un criterio per la corrispondenza.
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vs.regularexpressionbuilder
helpviewer_keywords:
- regular expressions [SQL Server Management Studio]
- Query Editor [SQL Server Management Studio], regular expression searches
- searches [SQL Server Management Studio], regular expressions
ms.assetid: a057690c-d118-4159-8e4d-2ed5ccfe79d3
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f09834f72bbcf5932961045a803c57dc9702a242
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344175"
---
# <a name="search-text-with-regular-expressions"></a>Testo di ricerca con espressioni regolari

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Le espressioni regolari costituiscono un metodo di notazione conciso e flessibile per la ricerca e la sostituzione di testo che soddisfa determinati criteri. È possibile usare un set specifico di espressioni regolari nel campo **Trova** della finestra di dialogo **Trova e sostituisci** di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].

## <a name="find-using-regular-expressions"></a>Per eseguire la ricerca usando espressioni regolari
1. Per consentire l'uso di espressioni regolari nel campo **Trova** durante le operazioni **Ricerca rapida**, **FindinFiles** (Cerca nei file), **Sostituzione veloce** o **Replace in Files** (Sostituisci nei file), selezionare l'opzione **Usa** in **Opzioni di ricerca** e scegliere **Espressioni regolari**.

2. Accanto al campo **Trova** viene reso disponibile il pulsante triangolare per **l'elenco dei riferimenti**. Fare clic su questo pulsante per visualizzare un elenco delle espressioni regolari più comuni. Ogni elemento selezionato in Generatore di espressioni viene inserito nella stringa **Trova** .

> [!NOTE]
> Le espressioni regolari che possono essere utilizzate nelle stringhe **Trova** presentano differenze sintattiche rispetto a quelle valide nella programmazione in [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework. Poiché ad esempio in **Trova e sostituisci** la notazione con parentesi graffe {} viene usata per le espressioni con tag, l'espressione "zo{1}" consente di trovare tutte le occorrenze di "zo" seguite dal tag 1, come in "Pranzo1" e "Gonzo1". In .NET Framework, tuttavia, la notazione {} viene usata per i quantificatori. Pertanto l'espressione "zo{1}" consente di trovare tutte le occorrenze di "z" seguite da una e una sola "o", come in "zona", ma non in "zoo".

Nella seguente tabella vengono descritte le espressioni regolari disponibili nell'**elenco dei riferimenti**.

|Expression|Sintassi|Descrizione|
|----------------|------------|-----------------|
|Qualsiasi carattere|.|Consente di ricercare un carattere qualsiasi, ad eccezione del carattere di interruzione riga.|
|Zero o più|*|Consente di ricercare zero o più occorrenze dell'espressione precedente, con il maggior numero di caratteri corrispondenti possibile.|
|Uno o più|+|Consente di ricercare almeno un'occorrenza dell'espressione precedente.|
|Inizio riga|^|Consente di vincolare la ricerca all'inizio di una riga.|
|Fine riga|$|Consente di vincolare la ricerca alla fine di una riga.|
|Inizio parola|\<|Consente di trovare la corrispondenza solo quando una parola inizia nel punto specificato del testo.|
|Fine parola|>|Consente di trovare la corrispondenza solo quando una parola finisce nel punto specificato del testo.|
|Interruzione di riga|\n|Consente di ricercare un'interruzione di riga, indipendentemente dalla piattaforma. In un'espressione di sostituzione, inserisce un'interruzione di riga.|
|Qualsiasi carattere del set|[]|Consente di ricercare qualsiasi carattere racchiuso tra []. Per specificare un intervallo di caratteri, immettere il carattere iniziale e quello finale separati da un trattino (-), ad esempio [a-z].|
|Qualsiasi carattere esterno al set|[^...]|Consente di ricercare qualsiasi carattere non compreso nel set di caratteri che seguono il simbolo ^.|
|Oppure|&#124;|Cerca l'espressione specificata prima o dopo il simbolo OR (&#124;). In genere viene utilizzata all'interno di un gruppo. Ad esempio, (caffè&#124;latte) macchiato corrisponde a "caffè macchiato" e "latte macchiato".|
|Carattere speciale di escape|\|Cerca il carattere che segue la barra rovesciata (\\) come valore letterale. Ciò consente di trovare caratteri utilizzati nella notazione delle espressioni regolari, quali { e ^. Ad esempio, \\^ consente di cercare il carattere ^.|
|Espressione tag|{}|Consente di ricercare il testo con tag nell'espressione tra parentesi.|
|Identificatore C/C++|:i|Corrisponde all'espressione ([a-zA-Z_$][a-zA-Z0-9_$]*).|
|Stringa tra virgolette|:q|Cerca l'espressione (("[^"]*")&#124;('[^']\*')).|
|Spazio o tabulazione|:b|Consente di ricercare il carattere spazio o tabulazione.|
|Integer|:z|Corrisponde all'espressione ([0-9]+).|

Nell'**elenco dei riferimenti** non è possibile visualizzare tutte le espressioni regolari valide per le operazioni di **ricerca e sostituzione**. In una stringa **Trova** è possibile inserire anche le seguenti espressioni regolari:

|Expression|Sintassi|Descrizione|
|----------------|------------|-----------------|
|Minimo tra zero o più occorrenze|@|Consente di ricercare zero o più occorrenze dell'espressione precedente, con il minor numero di caratteri corrispondenti possibile.|
|Minimo tra una o più occorrenze|#|Consente di ricercare una o più occorrenze dell'espressione precedente, con il minor numero di caratteri corrispondenti possibile.|
|n ripetizioni|^n|Consente di ricercare n occorrenze dell'espressione precedente. Ad esempio, [0-9]^4 consente di ricercare qualsiasi sequenza di quattro cifre.|
|Raggruppamento|()|Consente di raggruppare una sottoespressione.|
|ennesimo testo con tag|\n|In un'espressione **Trova o Sostituisci** indica il testo corrispondente all'ennesima espressione con tag, dove n è un numero da 1 a 9.<br /><br /> In un'espressione di **sostituzione** \0 inserisce l'intero testo corrispondente.|
|Campo giustificato a destra|\\(w,n)|In un'espressione di **sostituzione** giustifica a destra l'ennesima espressione con tag in un campo di dimensioni di almeno *w* caratteri.|
|Campo giustificato a sinistra|\\(-w,n)|In un'espressione di **sostituzione** giustifica a sinistra l'ennesima espressione con tag in un campo di dimensioni di almeno *w* caratteri.|
|Impedisci corrispondenza|~(X)|Consente di escludere una corrispondenza quando X si trova nel punto specificato dell'espressione. Ad esempio, real~(tà) corrisponde a "real" in "realmente" e "realistico", ma non a "real" in "realtà".|
|Carattere alfanumerico|:a|Corrisponde all'espressione ([a-zA-Z0-9]).|
|Carattere alfabetico|:c|Corrisponde all'espressione ([a-zA-Z]).|
|Cifra decimale|:d|Corrisponde all'espressione ([0-9]).|
|Cifra esadecimale|:h|Corrisponde all'espressione ([0-9a-fA-F]+).|
|Numero razionale|:n|Cerca l'espressione (([0-9]+.[0-9]*)&#124;([0-9]\*.[0-9]+)&#124;([0-9]+)).|
|Stringa alfabetica|:w|Corrisponde all'espressione ([a-zA-Z]+).|
|Carattere speciale di escape|\e|Carattere Unicode U+001B.|
|Campanello|\g|Carattere Unicode U+0007.|
|Backspace|\h|Carattere Unicode U+0008.|
|Scheda|\t|Consente di ricercare un carattere di tabulazione, Unicode U+0009.|
|carattere Unicode|\x#### oppure \u####|Consente di ricercare un carattere corrispondente a un valore Unicode, dove #### è una cifra esadecimale. È possibile specificare un carattere non incluso nel Basic Multilingual Plane, ovvero un surrogato, tramite il punto di codice ISO 10646 o due punti di codice Unicode che forniscono i valori della coppia surrogato.|

Nella tabella seguente viene descritta la sintassi per stabilire una corrispondenza attraverso le proprietà dei caratteri Unicode standard. Le abbreviazioni di due lettere corrispondono a quelle indicate nel database delle proprietà dei caratteri Unicode e possono essere specificate come parte di un set di caratteri. Ad esempio, l'espressione [:Nd:Nl:No] corrisponde a qualsiasi tipo di cifra.

|Expression|Sintassi|Descrizione|
|----------------|------------|-----------------|
|Lettera maiuscola|:Lu|Consente di ricercare una qualsiasi lettera maiuscola. Ad esempio, :Luli corrisponde a "Gli" ma non a "gli".|
|Lettera minuscola|:Ll|Consente di ricercare una qualsiasi lettera minuscola. Ad esempio, :Llli corrisponde a "gli" ma non a "Gli".|
|Iniziale maiuscola|:Lt|Consente di ricercare le stringhe con una lettera maiuscola e una lettera minuscola, quali Nj e Dz.|
|Lettera modificatore|:Lm|Consente di ricercare lettere o punteggiatura, ad esempio virgole, accenti incrociati e doppi apici, utilizzati per indicare modifiche alla lettera precedente.|
|Altra lettera|:Lo|Consente di ricercare altre lettere, ad esempio la lettera gotica ahsa.|  
|Cifra decimale|:Nd|Consente di ricercare cifre decimali, ad esempio 0-9, e i relativi equivalenti a larghezza intera.|
|Cifra in lettere|:Nl|Consente di ricercare cifre in lettere, ad esempio i numeri romani e il numero zero ideografico.|
|Altra cifra|:No|Consente di ricercare altre cifre come numeri romani.|
|Punteggiatura di apertura|:Ps|Consente di ricercare segni di punteggiatura di apertura, ad esempio parentesi quadre aperte e parentesi graffe aperte.|
|Punteggiatura di chiusura|:Pe|Consente di ricercare segni di punteggiatura di chiusura, ad esempio parentesi quadre chiuse e parentesi graffe chiuse.|
|Virgolette iniziali|:Pi|Consente di ricercare virgolette doppie di apertura.|
|Virgolette finali|:Pf|Consente di ricercare virgolette singole e virgolette doppie di chiusura.|
|Trattino|:Pd|Consente di ricercare il carattere trattino.|
|Punteggiatura di collegamento|:Pc|Consente di ricercare il carattere di sottolineatura.|
|Altra punteggiatura|:Po|Cerca (,), ?, ", !, @, #, %, &, *, \\, (:), (;), ' e /.|
|Spazio separatore|:Zs|Consente di ricercare il carattere spazio.|
|Separatore di riga|:Zl|È consentito ricercare il carattere Unicode U+2028.|
|Separatore di paragrafo|:Zp|Consente di ricercare il carattere U+2029.|
|Accenti e segni diacritici|:Mn|Consente di ricercare accenti e segni diacritici.|
|Segno di combinazione|:Mc|Consente di ricercare segni di combinazione.|
|Segno di inclusione|:Me|Consente di ricercare segni di inclusione.|
|Simbolo matematico|:Sm|Consente di cercare +, =, ~, &#124;, \<, and >.|
|Simbolo di valuta|:Sc|Consente di ricercare il simbolo $ e altri simboli di valuta.|
|Simbolo modificatore|:Sk|Consente di ricercare simboli modificatori, ad esempio accenti circonflessi, accenti gravi e macron.|
|Altro simbolo|:So|Consente di ricercare altri simboli, ad esempio il simbolo di copyright, il simbolo di paragrafo e quello di grado.|
|Altro controllo|:Cc|Consente di ricercare il carattere di fine riga.|
|Altro formato|:Cf|Consente di ricercare caratteri di controllo di formattazione, ad esempio i caratteri di controllo bidirezionali.|
|Surrogato|:Cs|Consente di ricercare una metà di una coppia surrogato.|
|Altri caratteri privati|:Co|Consente di ricercare qualsiasi carattere appartenente all'area privata.|
|Altri caratteri non assegnati|:Cn|Consente di ricercare caratteri senza mapping a un carattere Unicode.|
|Specificare il numero di occorrenze del gruppo o del carattere precedente. Per altre informazioni, vedere [Trova la corrispondenza esatta n volte](/dotnet/standard/base-types/quantifiers-in-regular-expressions#match-exactly-n-times-n).|{n}, dove 'n' è il numero di occorrenze|`x(ab){2}x` corrisponde a "xababx"<br/>`x(ab){2,3}x` corrisponde a "xababx" e a "xabababx", ma non a "xababababx"|

Oltre alle proprietà dei caratteri Unicode standard, è possibile specificare come parte di un set di caratteri le proprietà aggiuntive elencate di seguito.

|Expression|Sintassi|Descrizione|
|----------------|------------|-----------------|
|Alfa|:Al|Consente di ricercare qualsiasi carattere. Ad esempio, :Alli consente di trovare parole come "Gli", "giglio" e "foglio".|
|Numeric|:Nu|Consente di ricercare un numero o una cifra.|
|Punteggiatura|:Pu|Consente di ricercare qualsiasi segno di punteggiatura, ad esempio ?, @, ' e così via.|
|Spazi vuoti|:Wh|Consente di ricercare tutti i tipi di spazi, inclusi gli spazi di impaginazione e quelli ideografici.|
|Bidirezionale|:Bi|Consente di ricercare caratteri appartenenti a lingue con scrittura da destra a sinistra, come l'arabo e l'ebraico.|
|Hangul|:Ha|Consente di ricercare caratteri Hangul (coreano) e jamo combinati.|
|Hiragana|:Hi|Consente di ricercare caratteri hiragana.|
|Katakana|:Ka|Consente di ricercare caratteri katakana.|
|Ideogramma/Han/Kanji|:Id|Consente di ricercare caratteri ideografici, come i caratteri Han e Kanji.|

## <a name="see-also"></a>Vedere anche  
- [Ricerca e sostituzione](./search-and-replace.md)
- [Eseguire ricerche di testo con caratteri jolly](./search-text-with-wildcards.md)