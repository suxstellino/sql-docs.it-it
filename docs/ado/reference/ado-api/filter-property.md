---
description: Proprietà Filter
title: Proprietà Filter | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 03/20/2018
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::Filter
helpviewer_keywords:
- Filter property
ms.assetid: 80263a7a-5d21-45d1-84fc-34b7a9be4c22
author: rothja
ms.author: jroth
ms.openlocfilehash: 7366ba67d36049f1c5df701bc633d76f56bb9af5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167326"
---
# <a name="filter-property"></a>Proprietà Filter
Indica un filtro per i dati in un [Recordset](./recordset-object-ado.md).  
  
## <a name="settings-and-return-values"></a>Impostazioni e valori restituiti

Imposta o restituisce un valore **Variant** , che può contenere uno degli elementi seguenti:  
  
-   **Stringa criteri:** Stringa costituita da una o più clausole concatenate con gli operatori **and** o **or** .  
  
-   **Matrice di segnalibri:** Matrice di valori di segnalibro univoci che puntano a record nell'oggetto **Recordset** .  
  
-   Valore [FilterGroupEnum](./filtergroupenum.md) .  
  
## <a name="remarks"></a>Commenti

Utilizzare la proprietà **Filter** per schermare selettivamente i record in un oggetto **Recordset** . Il **Recordset** filtrato diventa il cursore corrente. Sono interessate anche altre proprietà che restituiscono valori basati sul **cursore** corrente, ad esempio la [proprietà AbsolutePosition (ADO)](./absoluteposition-property-ado.md), la [Proprietà AbsolutePage (ADO)](./absolutepage-property-ado.md), la [proprietà RecordCount (ADO)](./recordcount-property-ado.md)e la [proprietà PageCount (ADO)](./pagecount-property-ado.md). Se si imposta la proprietà **Filter** su un nuovo valore specifico, il record corrente viene spostato sul primo record che soddisfa il nuovo valore.
  
La stringa di criteri è costituita da clausole nel formato *FieldName-operator-value* (ad esempio, `"LastName = 'Smith'"` ). È possibile creare clausole composte concatenando singole clausole con **e** (ad esempio, `"LastName = 'Smith' AND FirstName = 'John'"` ) o **o** (ad esempio, `"LastName = 'Smith' OR LastName = 'Jones'"` ). Per le stringhe dei criteri, attenersi alle linee guida seguenti:

-   *FieldName* deve essere un nome di campo valido del **Recordset**. Se il nome del campo contiene spazi, è necessario racchiudere il nome tra parentesi quadre.  
  
-   L'operatore deve essere uno dei seguenti: \<, > , \<=, > =,  <>, = o **like**.  
  
-   Value è il valore con cui si confronteranno i valori dei campi (ad esempio,' Smith ', #8/24/95 #, 12,345 o $50,00). Usare le virgolette singole con stringhe e segni di cancelletto (#) con date. Per i numeri, è possibile usare separatori decimali, simboli del dollaro e notazione scientifica. Se l'operatore è **simile a**, il valore può usare caratteri jolly. Solo l'asterisco (*) e il segno di percentuale (%) i caratteri jolly sono consentiti e devono essere l'ultimo carattere della stringa. Il valore non può essere Null.  
  
> [!NOTE]
>  Per includere virgolette singole (') nel valore del filtro, usare due virgolette singole per rappresentarne una. Ad esempio, per filtrare in modo da essere impostata su Malley, la stringa di criteri deve essere `"col1 = 'O''Malley'"` . Per includere virgolette singole sia all'inizio che alla fine del valore del filtro, racchiudere la stringa con i segni di cancelletto (#). Ad esempio, per applicare un filtro su "1", la stringa di criteri deve essere `"col1 = #'1'#"` .  
  
-   Non esiste alcuna precedenza tra AND e OR. Le clausole possono essere raggruppate tra parentesi. Tuttavia, non è possibile raggruppare le clausole unite da un oggetto o, quindi unire il gruppo a un'altra clausola con e, come nel frammento di codice seguente:  
 `(LastName = 'Smith' OR LastName = 'Jones') AND FirstName = 'John'`  
  
-   Al contrario, è necessario creare questo filtro come  
 `(LastName = 'Smith' AND FirstName = 'John') OR (LastName = 'Jones' AND FirstName = 'John')`  
  
-   In una clausola **like** è possibile usare un carattere jolly all'inizio e alla fine del modello. È ad esempio possibile usare `LastName Like '*mit*'`. Oppure con **like** è possibile usare un carattere jolly solo alla fine del modello. Ad esempio: `LastName Like 'Smit*'`.  
  
 Le costanti di filtro semplificano la risoluzione dei singoli conflitti di record durante la modalità di aggiornamento batch consentendo di visualizzare, ad esempio, solo i record interessati durante l'ultima chiamata al metodo [UpdateBatch](./updatebatch-method.md) .  
  
L'impostazione della proprietà **Filter** potrebbe avere esito negativo a causa di un conflitto con i dati sottostanti. Questo errore può verificarsi, ad esempio, quando un record è già stato eliminato da un altro utente. In tal caso, il provider restituisce avvisi alla raccolta [Errors (ADO)](./errors-collection-ado.md) , ma non interrompe l'esecuzione del programma. Un errore in fase di esecuzione si verifica solo se sono presenti conflitti in tutti i record richiesti. Utilizzare la proprietà [Status (recordset ADO)](./status-property-ado-recordset.md) per individuare i record con conflitti.  
  
L'impostazione della proprietà **Filter** su una stringa di lunghezza zero ("") ha lo stesso effetto dell'utilizzo della costante **adFilterNone** .
  
Ogni volta che la proprietà **Filter** è impostata, la posizione corrente del record viene spostata sul primo record del subset filtrato di record nel **Recordset**. Analogamente, quando la proprietà **Filter** viene deselezionata, la posizione corrente del record viene spostata sul primo record del **Recordset**.

Si supponga che un **Recordset** venga filtrato in base a un campo di un tipo Variant, ad esempio il tipo sql_variant. Si verifica un errore (DISP_E_TYPEMISMATCH o 80020005) quando i sottotipi del campo e i valori di filtro utilizzati nella stringa dei criteri non corrispondono. Si supponga ad esempio che:

- Un oggetto **Recordset** (RS) contiene una colonna (C) del tipo di sql_variant.
- A un campo di questa colonna è stato assegnato il valore 1 del tipo i4. La stringa dei criteri è impostata `rs.Filter = "C='A'"` su nel campo.

Questa configurazione genera l'errore in fase di esecuzione. Tuttavia, `rs.Filter = "C=2"` applicato nello stesso campo non verrà generato alcun errore. E il campo viene escluso dal set di record corrente.

Per una spiegazione dei valori di segnalibro da cui è possibile compilare una matrice da usare con la proprietà Filter, vedere la proprietà [Bookmark Property (ADO)](./bookmark-property-ado.md) .

Solo i filtri sotto forma di stringhe di criteri influiscono sul contenuto di un **Recordset** salvato in modo permanente. Un esempio di stringa di criteri è `OrderDate > '12/31/1999'` . I filtri creati con una matrice di segnalibri o con un valore di **FilterGroupEnum** non influiscono sul contenuto del **Recordset** salvato in modo permanente. Queste regole si applicano ai recordset creati con cursori sul lato client o sul lato server.
  
> [!NOTE]
>  Quando si applica il flag adFilterPendingRecords a un **Recordset** filtrato e modificato in modalità di aggiornamento batch, il **Recordset** risultante è vuoto se il filtro è basato sul campo chiave di una tabella a chiave singola ed è stata apportata la modifica ai valori dei campi chiave. Il **Recordset** risultante non sarà vuoto se si verifica una delle seguenti istruzioni:  
  
-   Il filtro è basato su campi non chiave in una tabella a chiave singola.  
  
-   Il filtro è basato su tutti i campi in una tabella con più chiavi.  
  
-   Sono state apportate modifiche ai campi non chiave in una tabella a chiave singola.  
  
-   Sono state apportate modifiche a tutti i campi in una tabella con più chiavi.  
  
Nella tabella seguente sono riepilogati gli effetti di **adFilterPendingRecords** in diverse combinazioni di filtro e modifiche. La colonna sinistra mostra le possibili modifiche. È possibile apportare modifiche in uno qualsiasi dei campi non con chiave, nel campo chiave di una tabella a chiave singola o in uno qualsiasi dei campi chiave di una tabella con più chiavi. La riga superiore mostra il criterio di filtro. I filtri possono essere basati su uno qualsiasi dei campi non con chiave, il campo chiave in una tabella a chiave singola o uno qualsiasi dei campi chiave in una tabella con più chiavi. Le celle di intersezione mostrano i risultati. Un **+** segno più significa che l'applicazione di **adFilterPendingRecords** restituisce un **Recordset** non vuoto. Un **-** segno meno indica un **Recordset** vuoto.  
  
|Combinazioni|Non chiavi|Chiave singola|Più chiavi|
|-|--------------|----------------|-------------------|
|**Non chiavi**|+|+|+|
|**Chiave singola**|+|-|N/D|
|**Più chiavi**|+|N/D|+|
|||||
  
## <a name="applies-to"></a>Si applica a

[Oggetto Recordset (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Vedere anche

[Esempio di proprietà Filter e RecordCount (VB)](./filter-and-recordcount-properties-example-vb.md) 
 [Esempio di proprietà Filter e RecordCount (VC + +)](./filter-and-recordcount-properties-example-vc.md) 
 [Metodo Clear (ADO)](./clear-method-ado.md) 
 [Ottimizza Property-Dynamic (ADO)](./optimize-property-dynamic-ado.md)