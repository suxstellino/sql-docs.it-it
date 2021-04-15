---
title: Usare la modalità EXPLICIT con FOR XML | Microsoft Docs
description: Informazioni sull'uso della modalità FOR XML EXPLICIT, che offre la massima flessibilità nella generazione del codice XML dal risultato di una query.
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- EXPLICIT FOR XML mode
- FOR XML clause, EXPLICIT mode
- FOR XML EXPLICIT mode
ms.assetid: 8b26e8ce-5465-4e7a-b237-98d0f4578ab1
author: rothja
ms.author: jroth
ms.openlocfilehash: 6e2ca436aab52854f1767b97f6f368d11c8dd681
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487235"
---
# <a name="use-explicit-mode-with-for-xml"></a>Utilizzo della modalità EXPLICIT con FOR XML
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Come illustrato nell'argomento [Costruzione di codice XML tramite la clausola FOR XML](../../relational-databases/xml/for-xml-sql-server.md), le modalità RAW e AUTO forniscono scarso controllo sulla forma del codice XML generato dai risultati della query. La modalità EXPLICIT tuttavia offre maggiore flessibilità nella generazione del codice XML desiderato dal risultato di una query.  
  
 La query in modalità EXPLICIT deve essere formulata in modo tale che le informazioni aggiuntive relative al codice XML, ad esempio la nidificazione desiderata vengano specificate in modo esplicito nell'ambito della query stessa. In base al codice XML desiderato, la formulazione di query in modalità EXPLICIT può essere un'operazione complessa. L' [uso della modalità PATH](../../relational-databases/xml/use-path-mode-with-for-xml.md) con annidamento è un'alternativa più semplice alla scrittura di query in modalità EXPLICIT.  
  
 Poiché in modalità EXPLICIT si descrive il codice XML desiderato nell'ambito della query, è necessario assicurarsi che il codice XML generato sia in formato corretto e valido.  
  
## <a name="rowset-processing-in-explicit-mode"></a>Elaborazione dei set di righe in modalità EXPLICIT  
 La modalità EXPLICIT converte il set di righe risultante dall'esecuzione della query in un documento XML. Affinché la modalità EXPLICIT generi il documento XML, è necessario che il set di righe abbia un formato specifico. A questo scopo è necessario formulare la query SELECT in modo tale che restituisca il set di righe, la **tabella universale**, con un formato specifico in modo che la logica di elaborazione possa creare il codice XML desiderato.  
  
 Innanzitutto, la query deve produrre le due colonne di metadati seguenti:  
  
-   La prima colonna deve contenere il numero di tag e il tipo Integer dell'elemento corrente. Il nome della colonna deve essere **Tag**. La query deve fornire un numero di tag univoco per ogni elemento che verrà creato dal set di righe.  
  
-   La seconda colonna deve contenere il numero di tag dell'elemento padre. Il nome della colonna deve essere **Parent**. In questo modo, le colonne Tag e Parent forniscono informazioni di gerarchia.  
  
 I valori di queste colonne di metadati, con le informazioni contenute nei nomi di colonna, vengono utilizzati per produrre il codice XML desiderato. Si noti che nella query è necessario fornire i nomi delle colonne in modo specifico. Si noti anche che un valore 0 o NULL nella colonna **Parent** indica che l'elemento corrispondente non ha un elemento padre. L'elemento viene aggiunto al codice XML come elemento di livello principale.  
  
 Per comprendere il modo in cui la tabella universale generata da una query viene elaborata per generare il codice XML risultante, si supponga di aver creato una query che produce la tabella universale seguente:  
  
 ![Tabella universale di esempio](../../relational-databases/xml/media/xmlutable.gif "Tabella universale di esempio")  
  
 Dalla tabella universale si noti quanto segue:  
  
-   Le prime due colonne sono **Tag** e **Parent** e sono colonne di metadati. Questi valori determinano la gerarchia.  
  
-   I nomi delle colonne sono formulati in un modo specifico, descritto di seguito in questo argomento.  
  
-   Durante la generazione del codice XML da questa tabella universale, i dati della tabella vengono partizionati verticalmente in gruppi di colonne. Il raggruppamento viene determinato in base al valore di **Tag** e ai nomi delle colonne. Nella generazione del codice XML, la logica di elaborazione seleziona un gruppo di colonne per riga e costruisce un elemento. In questo esempio avviene quanto segue:  
  
    -   Per il valore 1 della colonna **Tag** nella prima riga, le colonne i cui nomi includono lo stesso numero di tag, **Customer!1!cid** e **Customer!1!name**, formano un gruppo. Queste colonne vengono utilizzate per l'elaborazione della riga e si può notare che la forma dell'elemento generato è <`Customer id=... name=...`>. Il formato del nome di colonna è descritto di seguito in questo argomento.  
  
    -   Per le righe con valore 2 nella colonna **Tag**, le colonne **Order!2!id** e **Order!2!date** formano un gruppo che viene quindi usato per la costruzione di elementi, <`Order id=... date=... /`>.  
  
    -   Per le righe con valore 3 nella colonna **Tag** , le colonne **OrderDetail!3!id!id** e **OrderDetail!3!pid!idref** formano un gruppo. Ognuna di queste righe genera un elemento <`OrderDetail id=... pid=...`> da queste colonne.  
  
-   Si noti che nella generazione della gerarchia XML le righe vengono elaborate in ordine. La gerarchia XML viene determinata come descritto di seguito:  
  
    -   La prima riga specifica il valore 1 di **Tag** e il valore NULL di **Parent** . L'elemento corrispondente, <`Customer`>, viene pertanto aggiunto nel codice XML come elemento di livello principale.  
  
        ```  
        <Customer cid="C1" name="Janine">  
        ```  
  
    -   La seconda riga identifica il valore 2 di **Tag** e il valore 1 di **Parent** . L'elemento <`Order`> viene pertanto aggiunto come elemento figlio dell'elemento <`Customer`>.  
  
        ```  
        <Customer cid="C1" name="Janine">  
           <Order id="O1" date="1/20/1996">  
        ```  
  
    -   Le due righe successive identificano il valore 3 di **Tag** e il valore 2 di **Parent** . I due elementi <`OrderDetail`> vengono pertanto aggiunti come elementi figlio dell'elemento <`Order`>.  
  
        ```  
        <Customer cid="C1" name="Janine">  
           <Order id="O1" date="1/20/1996">  
              <OrderDetail id="OD1" pid="P1"/>  
              <OrderDetail id="OD2" pid="P2"/>  
        ```  
  
    -   L'ultima riga identifica 2 come valore di **Tag** e 1 come valore di **Parent** . Un altro elemento figlio <`Order`> viene pertanto aggiunto all'elemento padre <`Customer`>.  
  
        ```  
        <Customer cid="C1" name="Janine">  
           <Order id="O1" date="1/20/1996">  
              <OrderDetail id="OD1" pid="P1"/>  
              <OrderDetail id="OD2" pid="P2"/>  
           </Order>  
           <Order id="O2" date="3/29/1997">  
        </Customer>  
        ```  
  
 Per riassumere, per generare il codice XML desiderato in modalità EXPLICIT vengono usati i valori presenti nelle colonne di metadati **Tag** e **Parent** , le informazioni specificate nei nomi delle colonne e l'ordinamento corretto delle righe.  
  
### <a name="universal-table-row-ordering"></a>Ordinamento delle righe della tabella universale  
 Nella generazione del codice XML, le righe della tabella universale vengono elaborate in ordine. Per recuperare le istanze figlio corrette associate al rispettivo elemento padre, è pertanto necessario che le righe del set vengano ordinate in modo tale che il nodo dell'elemento padre sia seguito immediatamente dagli elementi figlio.  
  
## <a name="specifying-column-names-in-a-universal-table"></a>Specifica dei nomi delle colonne in una tabella universale  
 Quando si scrivono query in modalità EXPLICIT è necessario specificare i nomi delle colonne nel set di righe risultante utilizzando il formato specificato di seguito. I nomi delle colonne forniscono informazioni di trasformazione, fra cui nomi di elementi e attributi e altre informazioni, specificate utilizzando direttive.  
  
 Il formato generale è il seguente:  
  
```  
  
ElementName!TagNumber!AttributeName!Directive  
```  
  
 Di seguito è fornita la descrizione delle parti del formato.  
  
 *ElementName*  
 Identificatore generico risultante dell'elemento. Ad esempio, se **Customers** viene specificato come *ElementName*, verrà generato l'elemento \<Customers>.  
  
 *TagNumber*  
 È un valore di tag univoco assegnato a un elemento. Questo valore, insieme alle due colonne di metadati **Tag** e **Parent**, determina l'annidamento degli elementi nel codice XML risultante.  
  
 *AttributeName*  
 Indica il nome dell'attributo da costruire nell'argomento *ElementName* specificato. Ciò si verifica quando *Directive* non viene specificato.  
  
 Se per *Directive* viene specificato **xml**, **cdata** oppure **element**, questo valore viene usato per costruire un elemento figlio di *ElementName*, al quale viene aggiunto il valore della colonna.  
  
 Se si specifica *Directive*, *AttributeName* può essere vuoto. Ad esempio, ElementName!TagNumber!!Directive. In questo caso, il valore della colonna è direttamente contenuto da *ElementName*.  
  
 *Directive*  
 *Directive* è facoltativo e può essere usato per specificare altre informazioni per la generazione del codice XML. *Directive* svolge due funzioni.  
  
 Il primo è la codifica dei valori come ID, IDREF e IDREFS. È possibile specificare le parole chiave **ID**, **IDREF** e **IDREFS** come valori di *Directive*. Queste direttive sovrascrivono i tipi di attributo, consentendo di creare collegamenti tra più documenti.  
  
 È anche possibile usare *Directive* per definire il mapping tra i dati di tipo stringa e il codice XML. Le parole chiave **hide**, **element, elementxsinil**, **xml**, **xmltext** e **cdata** possono essere usate come valori di *Directive*. La direttiva **hide** nasconde il nodo. È utile quando si recuperano i valori solo a scopo di ordinamento, ma non si desidera che vengano inseriti nel codice XML risultante.  
  
 La direttiva **element** genera un elemento contenuto anziché un attributo. I dati contenuti vengono codificati come entità. Ad esempio, il carattere **<** viene convertito in &lt;. Per i valori di colonna NULL non vengono generati elementi. Se si vuole che venga generato un elemento anche per i valori di colonna NULL è possibile specificare la direttiva **elementxsinil** . Verrà generato un elemento con attributo xsi:nil=TRUE.  
  
 La direttiva **xml** equivale a una direttiva **element** , ma non viene eseguita alcuna codifica di entità. Si noti che è possibile combinare la direttiva **element** con **ID**, **IDREF** o **IDREFS**, mentre la direttiva **xml** può essere usata solo con **hide**.  
  
 La direttiva **cdata** contiene i dati inserendoli in una sezione CDATA. Il contenuto non viene codificato come entità. I dati originali devono essere di tipo testo, ad esempio **varchar**, **nvarchar**, **text** o **ntext**. Questa direttiva può essere usa solo con **hide**. Se si usa questa direttiva, non è necessario specificare *AttributeName* .  
  
 Nella maggior parte dei casi è consentito combinare le direttive tra questi due gruppi, ma non è possibile combinarle tra loro.  
  
 Se *Directive* e *AttributeName* non vengono specificati, ad esempio, **Customer!1**, è implicita una direttiva **element** , ad esempio **Customer!1!!element** e i dati di colonna sono contenuti in *ElementName*.  
  
 Se si specifica la direttiva **xmltext** il contenuto della colonna viene inserito in un unico tag che viene integrato con il resto del documento. Questa direttiva risulta utile nel recupero dei dati XML di overflow (non utilizzati) archiviati in una colonna tramite OPENXML. Per altre informazioni, vedere [OPENXML &#40;SQL Server&#41;](../../relational-databases/xml/openxml-sql-server.md).  
  
 Se si specifica *AttributeName* il nome del tag viene sostituito dal nome specificato. In caso contrario, l'attributo viene aggiunto all'elenco corrente di attributi degli elementi che lo racchiudono inserendo il contenuto all'inizio dell'oggetto di contenimento senza codifica di entità. La colonna con questa direttiva deve essere di tipo testo, ad esempio **varchar**, **nvarchar**, **char**, **nchar**, **text** o **ntext**. Questa direttiva può essere usa solo con **hide**. Risulta particolarmente utile per il recupero dei dati di overflow archiviati in una colonna. Se il contenuto non è in un formato XML corretto, il funzionamento non è prevedibile.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
 Negli esempi seguenti viene illustrato l'utilizzo della modalità EXPLICIT.  
  
-   [Esempio: Recupero di informazioni sui dipendenti](../../relational-databases/xml/example-retrieving-employee-information.md)  
  
-   [Esempio: Specifica della direttiva ELEMENT](../../relational-databases/xml/example-specifying-the-element-directive.md)  
  
-   [Esempio: Specifica della direttiva ELEMENTXSINIL](../../relational-databases/xml/example-specifying-the-elementxsinil-directive.md)  
  
-   [Esempio: Creazione di elementi di pari livello con la modalità EXPLICIT](../../relational-databases/xml/example-constructing-siblings-with-explicit-mode.md)  
  
-   [Esempio: Specifica delle direttive ID e IDREF](../../relational-databases/xml/example-specifying-the-id-and-idref-directives.md)  
  
-   [Esempio: Specifica delle direttive ID e IDREFS](../../relational-databases/xml/example-specifying-the-id-and-idrefs-directives.md)  
  
-   [Esempio: Specifica della direttiva HIDE](../../relational-databases/xml/example-specifying-the-hide-directive.md)  
  
-   [Esempio: Specifica della direttiva ELEMENT e della codifica di entità](../../relational-databases/xml/example-specifying-the-element-directive-and-entity-encoding.md)  
  
-   [Esempio: Specifica della direttiva CDATA](../../relational-databases/xml/example-specifying-the-cdata-directive.md)  
  
-   [Esempio: Specifica della direttiva XMLTEXT](../../relational-databases/xml/example-specifying-the-xmltext-directive.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Utilizzo della modalità RAW con FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md)   
 [Utilizzo della modalità AUTO con FOR XML](../../relational-databases/xml/use-auto-mode-with-for-xml.md)   
 [Utilizzare la modalità PATH con FOR XML](../../relational-databases/xml/use-path-mode-with-for-xml.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [FOR XML &#40;SQL Server&#41;](../../relational-databases/xml/for-xml-sql-server.md)  
  
  
