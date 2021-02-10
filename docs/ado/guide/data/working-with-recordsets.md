---
description: Utilizzo dei recordset
title: Utilizzo di recordset | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Recordset object [ADO]
ms.assetid: bdf9a56a-de4a-44de-9111-2f11ab7b16ea
author: rothja
ms.author: jroth
ms.openlocfilehash: 4e64608adc461534a30630a6b8b8d29d0478544a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032253"
---
# <a name="working-with-recordsets"></a>Utilizzo dei recordset
L'oggetto **Recordset** dispone di funzionalità predefinite che consentono di riorganizzare l'ordine dei dati nel set di risultati, di cercare un record specifico in base ai criteri forniti dall'utente e anche di ottimizzare le operazioni di ricerca usando gli indici. Se queste funzionalità sono disponibili per l'uso dipendono dal provider e in alcuni casi, ad esempio quella della proprietà [index](../../../ado/reference/ado-api/index-property.md) , ovvero la struttura dell'origine dati stessa.  
  
## <a name="arranging-data"></a>Disposizione dei dati  
 Spesso, il modo più efficiente per ordinare i dati nel **Recordset** consiste nel specificare una clausola ORDER BY nel comando SQL usato per restituire i risultati. Potrebbe tuttavia essere necessario modificare l'ordine dei dati in un **Recordset** che è già stato creato. È possibile utilizzare la proprietà **Sort** per stabilire l'ordine in cui vengono attraversate le righe di un **Recordset** . Inoltre, la proprietà **Filter** determina le righe a cui è possibile accedere quando si attraversano le righe.  
  
 La proprietà **Sort** imposta o restituisce un valore **stringa** che indica i nomi dei campi nel **Recordset** in cui eseguire l'ordinamento. Ogni nome è separato da una virgola ed è facoltativamente seguito da uno spazio e dalla parola chiave **ASC** (che ordina il campo in ordine crescente) o **desc** (che ordina il campo in ordine decrescente). Per impostazione predefinita, se non viene specificata alcuna parola chiave, il campo viene ordinato in ordine crescente.  
  
 L'operazione di ordinamento è efficiente perché i dati non vengono ridisposti fisicamente, ma è possibile accedervi nell'ordine specificato da un indice.  
  
 Per la proprietà **Sort** è necessario che la proprietà [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) sia impostata su **adUseClient**. Viene creato un indice temporaneo per ogni campo specificato nella proprietà **Sort** se non esiste già un indice.  
  
 Se si imposta la proprietà **Sort** su una stringa vuota, le righe vengono reimpostate sull'ordine originale ed eliminano gli indici temporanei. Gli indici esistenti non verranno eliminati.  
  
 Si supponga che un **Recordset** contenga tre campi denominati *FirstName*, *middleInitial* e *LastName*. Impostare la proprietà **Sort** sulla stringa " `lastName DESC, firstName ASC` ", che ordina il **Recordset** in base al cognome in ordine decrescente e quindi in base al nome in ordine crescente. Il secondo iniziale viene ignorato.  
  
 Nessun campo a cui viene fatto riferimento in una stringa di criteri di ordinamento può essere denominato "ASC" o "DESC" perché tali nomi sono in conflitto con le parole chiave **ASC** e **desc**. Assegnare un alias a un campo con un nome in conflitto usando la parola chiave **As** nella query che restituisce il **Recordset**.  
  
 Per ulteriori informazioni sul filtro **Recordset** , vedere "filtro dei risultati" più avanti in questo argomento.  
  
## <a name="finding-a-specific-record"></a>Ricerca di un record specifico  
 ADO fornisce i metodi [Find](../../../ado/reference/ado-api/find-method-ado.md) e [Seek](../../../ado/reference/ado-api/seek-method.md) per l'individuazione di un record specifico in un **Recordset**. Il metodo **Find** è supportato da un'ampia gamma di provider, ma è limitato a un singolo criterio di ricerca. Il metodo **Seek** supporta la ricerca su più criteri, ma non è supportato da molti provider.  
  
 Gli indici sui campi possono migliorare significativamente le prestazioni del metodo **Find** e **ordinare** e **filtrare** le proprietà dell'oggetto **Recordset** . È possibile creare un indice interno per un oggetto **campo** impostando la relativa proprietà Dynamic [optimize](../../../ado/reference/ado-api/optimize-property-dynamic-ado.md) . Questa proprietà dinamica viene aggiunta alla raccolta **Properties** dell'oggetto **Field** quando si imposta la proprietà [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) su **adUseClient**. Tenere presente che questo indice è interno a ADO. non è possibile accedervi o usarlo per altri scopi. Inoltre, questo indice è diverso dalla proprietà [index](../../../ado/reference/ado-api/index-property.md) dell'oggetto **Recordset** .  
  
 Il metodo **Find** individua rapidamente un valore all'interno di una colonna (campo) di un **Recordset**. È possibile migliorare spesso la velocità del metodo **Find** su una colonna utilizzando la proprietà **optimize** per creare un indice.  
  
 Il metodo **Find** limita la ricerca al contenuto di un campo. Il metodo **Seek** richiede un indice e presenta anche altre limitazioni. Se è necessario eseguire la ricerca in più campi che non sono la base di un indice o se il provider non supporta gli indici, è possibile limitare i risultati utilizzando la proprietà **Filter** dell'oggetto **Recordset** .  
  
### <a name="find"></a>Find  
 Il metodo **Find** esegue la ricerca di un **Recordset** per la riga che soddisfa un criterio specificato. Facoltativamente, è possibile specificare la direzione della ricerca, la riga iniziale e l'offset dalla riga iniziale. Se il criterio viene raggiunto, la posizione della riga corrente viene impostata sul record trovato; in caso contrario, la posizione viene impostata sull'estremità (o inizio) del **Recordset**, a seconda della direzione di ricerca.  
  
 Per il criterio è possibile specificare solo un nome di colonna singola. In altre parole, questo metodo non supporta le ricerche multicolonna.  
  
 L'operatore di confronto per il criterio può essere " **>** " (maggiore di), "* * \<**" (less than), "=" (equal), "> =" (maggiore o uguale a), "<=" (minore o uguale a), "<>" (non uguale) o "like" (criteri di ricerca).  
  
 Il valore del criterio può essere una stringa, un numero a virgola mobile o una data. I valori stringa sono delimitati da virgolette singole o contrassegni "#" (simbolo di cancelletto), ad esempio "state =' WA '" o "state = #WA #". I valori di data sono delimitati dai contrassegni "#" (simbolo di cancelletto), ad esempio "start_date > #7/22/97 #".  
  
 Se l'operatore di confronto è "like", il valore stringa può contenere un asterisco (*) per trovare una o più occorrenze di qualsiasi carattere o sottostringa. Ad esempio, "state like am" \* corrisponde a Maine e Massachusetts. È anche possibile usare gli asterischi iniziali e finali per trovare una sottostringa contenuta all'interno dei valori. Ad esempio, "state like ' \* As \* '" corrisponde a Alaska, Arkansas e Massachusetts.  
  
 Gli asterischi possono essere utilizzati solo alla fine di una stringa di criteri o insieme sia all'inizio che alla fine di una stringa di criteri, come illustrato in precedenza. Non è possibile usare l'asterisco come carattere jolly (' * Str ') o carattere jolly incorporato (' \* s'). Verrà generato un errore.  
  
### <a name="seek-and-index"></a>Seek e index  
 Utilizzare il metodo **Seek** insieme alla proprietà **index** se il provider sottostante supporta indici nell'oggetto **Recordset** . Utilizzare il metodo [Supports](../../../ado/reference/ado-api/supports-method.md)**(adSeek)** per determinare se il provider sottostante supporta **Seek** e il metodo **Supports (adIndex)** per determinare se il provider supporta gli indici. Ad esempio, il [provider di OLE DB per Microsoft Jet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-microsoft-jet.md) supporta la **ricerca** e l' **Indice**.  
  
 Se **Seek** non trova la riga desiderata, non si verifica alcun errore e la riga viene posizionata alla fine del **Recordset**. Impostare la proprietà **index** sull'indice desiderato prima di eseguire questo metodo.  
  
 Questo metodo è supportato solo con i cursori sul lato server. La ricerca non è supportata quando il valore della proprietà [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) dell'oggetto **Recordset** è **adUseClient**.  
  
 Questo metodo può essere utilizzato solo quando l'oggetto **Recordset** è stato aperto con un valore [CommandTypeEnum](../../../ado/reference/ado-api/commandtypeenum.md) di **adCmdTableDirect**.  
  
## <a name="filtering-the-results"></a>Filtro dei risultati  
 Il metodo **Find** limita la ricerca al contenuto di un campo. Il metodo **Seek** richiede un indice e presenta anche altre limitazioni. Se è necessario eseguire la ricerca in più campi che non sono la base di un indice o se il provider non supporta gli indici, è possibile limitare i risultati utilizzando la proprietà **Filter** dell'oggetto **Recordset** .  
  
 Utilizzare la proprietà **Filter** per schermare selettivamente i record in un oggetto **Recordset** . Il **Recordset** filtrato diventa il cursore corrente, il che significa che i record che non soddisfano i criteri di **filtro** non sono disponibili nel **Recordset** fino a quando il **filtro** non viene rimosso. Sono interessate anche altre proprietà che restituiscono valori basati sul cursore corrente, ad esempio **AbsolutePosition**, **AbsolutePage**, **RecordCount** e **PageCount**. Questo perché impostando la proprietà **Filter** su un valore specifico, il record corrente verrà spostato nel primo record che soddisfa il nuovo valore.  
  
 La proprietà **Filter** accetta un argomento VARIANT. Questo valore rappresenta uno dei tre metodi per l'utilizzo della proprietà **Filter** : una stringa di criteri, una costante **FilterGroupEnum** o una matrice di segnalibri. Per ulteriori informazioni, vedere filtro con una stringa di criteri, filtro con una costante e filtro con segnalibri più avanti in questo argomento.  
  
> [!NOTE]
>  Quando si conoscono i dati che si desidera selezionare, in genere è più efficiente aprire un **Recordset** con un'istruzione SQL che filtra in modo efficace il set di risultati, anziché basarsi sulla proprietà **Filter** .  
  
 Per rimuovere un filtro da un **Recordset**, utilizzare la costante **adFilterNone** . L'impostazione della proprietà **Filter** su una stringa di lunghezza zero ("") ha lo stesso effetto dell'utilizzo della costante **adFilterNone** .  
  
### <a name="filtering-with-a-criteria-string"></a>Filtraggio con una stringa di criteri  
 La stringa dei criteri è costituita da clausole nel formato *FieldName operator value* (ad esempio, `"LastName = 'Smith'"` ). È possibile creare clausole composte concatenando singole clausole con **and** (ad esempio, `"LastName = 'Smith' AND FirstName = 'John'"` ) e **or** (ad esempio, `"LastName = 'Smith' OR LastName = 'Jones'"` ). Usare le linee guida seguenti per le stringhe dei criteri:  
  
-   *FieldName* deve essere un nome di campo valido del **Recordset**. Se il nome del campo contiene spazi, è necessario racchiudere il nome tra parentesi quadre.  
  
-   L' *operatore* deve essere uno dei seguenti: **\<**, **>** , **\<=**, **>=** , **<>** , **=** o **like**.  
  
-   *Value* è il valore con cui si confronteranno i valori dei campi (ad esempio,,, `'Smith'` `#8/24/95#` `12.345` o `$50.00` ). Usare le virgolette singole (') con le stringhe e i segni di cancelletto ( `#` ) con le date. Per i numeri, è possibile usare separatori decimali, simboli del dollaro e notazione scientifica. Se *operator* è **simile a**, *value* può utilizzare caratteri jolly. Solo l'asterisco ( \* ) e il segno di percentuale (%) i caratteri jolly sono consentiti e devono essere l'ultimo carattere della stringa. Il *valore* non può essere null.  
  
    > [!NOTE]
    >  Per includere virgolette singole (') nel *valore* del filtro, usare due virgolette singole per rappresentarne una. Ad esempio, per filtrare in modo da essere impostata su *Malley*, la stringa di criteri deve essere `"col1 = 'O''Malley'"` . Per includere virgolette singole sia all'inizio che alla fine del valore del filtro, racchiudere la stringa nei segni di cancelletto (#). Ad esempio, per applicare un filtro su *"1"*, la stringa di criteri deve essere `"col1 = #'1'#"` .  
  
 Non esiste alcuna precedenza tra **and** e **or**. Le clausole possono essere raggruppate tra parentesi. Tuttavia, non è possibile raggruppare le clausole unite da un oggetto **o** , quindi unire il gruppo a un'altra clausola con e, come indicato di seguito.  
  
```  
(LastName = 'Smith' OR LastName = 'Jones') AND FirstName = 'John'  
```  
  
 Al contrario, è necessario creare questo filtro come indicato di seguito.  
  
```  
(LastName = 'Smith' AND FirstName = 'John') OR (LastName = 'Jones' AND FirstName = 'John')  
```  
  
 In una clausola **like** è possibile usare un carattere jolly all'inizio e alla fine del modello (ad esempio, `LastName Like '*mit*'` ) o solo alla fine del pattern (ad esempio, `LastName Like 'Smit*'` ).  
  
### <a name="filtering-with-a-constant"></a>Filtraggio con una costante  
 Per filtrare i **Recordset** sono disponibili le costanti seguenti.  
  
|Costante|Descrizione|  
|--------------|-----------------|  
|**adFilterAffectedRecords**|Filtri per la visualizzazione dei soli record interessati dall'ultima chiamata a **Delete**, **Resync**, **UpdateBatch** o **CancelBatch** .|  
|**adFilterConflictingRecords**|Filtri per la visualizzazione dei record che non hanno superato l'ultimo aggiornamento batch.|  
|**adFilterFetchedRecords**|Filtri per la visualizzazione dei record nella cache corrente, ovvero i risultati dell'ultima chiamata per recuperare record dal database.|  
|**adFilterNone**|Rimuove il filtro corrente e ripristina tutti i record per la visualizzazione.|  
|**adFilterPendingRecords**|Filtri per la visualizzazione dei soli record che sono stati modificati ma che non sono ancora stati inviati al server. Applicabile solo per la modalità di aggiornamento batch.|  
  
 Le costanti di filtro semplificano la risoluzione dei singoli conflitti di record durante la modalità di aggiornamento batch consentendo di visualizzare, ad esempio, solo i record interessati durante l'ultima chiamata al metodo **UpdateBatch** , come illustrato nell'esempio seguente.  
  
 `Attribute VB_Name = "modExaminingData"`  
  
### <a name="filtering-with-bookmarks"></a>Filtraggio con segnalibri  
 Infine, è possibile passare una matrice Variant di segnalibri alla proprietà **Filter** . Il cursore risultante conterrà solo i record il cui segnalibro è stato passato alla proprietà. Nell'esempio di codice seguente viene creata una matrice di segnalibri dai record di un **Recordset** con "B" nel campo *ProductName* . Passa quindi la matrice alla proprietà **Filter** e visualizza le informazioni sul **Recordset** filtrato risultante.  
  
```  
'BeginFilterBkmk  
Dim vBkmkArray() As Variant  
Dim i As Integer  
  
'Recordset created using "SELECT * FROM Products" as command.  
'So, we will check to see if ProductName has a capital B, and  
'if so, add to the array.  
i = 0  
Do While Not objRs.EOF  
    If InStr(1, objRs("ProductName"), "B") Then  
        ReDim Preserve vBkmkArray(i)  
        vBkmkArray(i) = objRs.Bookmark  
        i = i + 1  
        Debug.Print objRs("ProductName")  
    End If  
    objRs.MoveNext  
Loop  
  
'Filter using the array of bookmarks.  
objRs.Filter = vBkmkArray  
  
objRs.MoveFirst  
Do While Not objRs.EOF  
    Debug.Print objRs("ProductName")  
    objRs.MoveNext  
Loop  
'EndFilterBkmk  
```  
  
## <a name="creating-a-clone-of-a-recordset"></a>Creazione di un clone di un recordset  
 Utilizzare il metodo **Clone** per creare più oggetti **Recordset** duplicati, soprattutto se si desidera mantenere più record correnti in un set di record specificato. L'utilizzo del metodo **Clone** è più efficiente rispetto alla creazione e all'apertura di un nuovo oggetto **Recordset** con la stessa definizione dell'originale.  
  
 Il record corrente di un clone appena creato è impostato originariamente sul primo record. Il puntatore di record corrente in un **Recordset** clonato non è sincronizzato con l'originale o viceversa. È possibile spostarsi in modo indipendente in ogni **Recordset**.  
  
 Le modifiche apportate a un oggetto **Recordset** sono visibili in tutti i relativi cloni indipendentemente dal tipo di cursore. Tuttavia, dopo l'esecuzione di [Requery](../../../ado/reference/ado-api/requery-method.md) sul **Recordset** originale, i cloni non verranno più sincronizzati con quello originale.  
  
 La chiusura del **Recordset** originale non comporta la chiusura delle copie, né la chiusura di una copia chiude l'originale o una delle altre copie.  
  
 È possibile clonare un oggetto **Recordset** solo se supporta segnalibri. I valori dei segnalibri sono intercambiabili; ovvero, un riferimento a un segnalibro da un oggetto **Recordset** fa riferimento allo stesso record in uno dei relativi cloni.
