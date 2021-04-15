---
title: Introduzione all'uso di query XPath (SQLXML)
description: Informazioni di base sull'uso di query XPath in SQLXML 4.0.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- XPath queries [SQLXML], about XPath queries
- W3C XPath specification
- XPath queries [SQLXML], functionality
ms.assetid: 01050a8e-0ccc-4a02-a4eb-b48be5c3f4f3
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2efd6f59cf0a853fbb357c930ba24d6e4889a2e2
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491828"
---
# <a name="introduction-to-using-xpath-queries-sqlxml-40"></a>Introduzione all'utilizzo di query XPath (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Una query XPath (XML Path Language) può essere specificata come parte di un URL o all'interno di un modello. La struttura del frammento risultante viene determinata dallo schema di mapping mentre i valori vengono recuperati dal database. Questo processo è concettualmente simile alla creazione di viste mediante l'istruzione CREATE VIEW e alla scrittura di query SQL relative a tali viste.  
  
> [!NOTE]  
>  Per comprendere le query XPath in SQLXML 4.0, è necessario avere familiarità con le viste XML e con concetti correlati quali i modelli e lo schema di mapping. Per altre informazioni, vedere [Introduction to Annotated XSD Schemas &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md)e lo standard XPath definito da World Wide Web Consortium (W3C).  
  
 Un documento XML è costituito da nodi, quali un nodo di elemento, un nodo di attributo, un nodo di testo e così via. Si consideri, ad esempio, il documento XML seguente:  
  
```  
<root>  
  <Customer cid= "C1" name="Janine" city="Issaquah">  
      <Order oid="O1" date="1/20/1996" amount="3.5" />  
      <Order oid="O2" date="4/30/1997" amount="13.4">Customer was  
          very satisfied</Order>  
   </Customer>  
   <Customer cid="C2" name="Ursula" city="Oelde" >  
      <Order oid="O3" date="7/14/1999" amount="100" note="Wrap it blue white red">  
          <Urgency>Important</Urgency>  
      </Order>  
      <Order oid="O4" date="1/20/1996" amount="10000"/>  
   </Customer>  
</root>  
```  
  
 In questo documento è **\<Customer>** un nodo elemento, **cid** è un nodo attributo e **"Important"** è un nodo di testo.  
  
 XPath è un linguaggio di navigazione grafica utilizzato per selezionare un set di nodi da un documento XML. Ogni operatore XPath consente di selezionare un set di nodi in base a un set di nodi selezionato da un operatore XPath precedente. Ad esempio, dato un set di nodi, XPath può selezionare tutti i nodi con il valore dell'attributo **\<Customer>** **\<Order>** **date** **"14/7/1999".** Il set di nodi risultante contiene tutti gli ordini con data 7/14/1999.  
  
 Il linguaggio XPath è definito dal World Wide Web Consortium (W3C) come linguaggio di navigazione standard. SQLXML 4.0 implementa un subset della specifica XPath W3C, che si trova in http://www.w3.org/TR/1999/PR-xpath-19991008.html .  
  
 Di seguito vengono elencate alcune delle differenze principali tra l'implementazione di XPath del W3C e l'implementazione di SQLXML 4.0.  
  
-   **Query radice**  
  
     SQLXML 4.0 non supporta la query radice (/). Ogni query XPath deve iniziare da un livello **\<ElementType>** superiore nello schema.  
  
-   **Segnalazione di errori**  
  
     La specifica XPath del W3C non definisce condizioni di errore. Le query XPath che non consentono di selezionare nodi restituiscono un set di nodi vuoto. In SQLXML 4.0 una query può restituire diversi tipi di messaggio di errore.  
  
-   **Ordine dei documenti**  
  
     In SQLXML 4.0 l'ordine dei documenti non è sempre definito. Di conseguenza, i predicati e gli assi numerici che usano l'ordine dei documenti (ad **esempio, quello** seguente) non vengono implementati.  
  
     La mancanza di un ordine dei documenti indica inoltre che il valore di stringa di un nodo può essere valutato solo se il nodo in questione è mappato a una singola colonna in una singola riga. Non è possibile convertire in stringa un elemento con elementi figlio o un nodo IDREFS o NMTOKENS.  
  
    > [!NOTE]  
    >  In alcuni casi, **l'annotazione dei campi chiave** o le chiavi dell'annotazione **di** relazione possono determinare un ordine deterministico del documento. Tuttavia, questo non è l'uso principale di queste annotazioni Per altre informazioni, vedere Identificazione di colonne chiave tramite [sql:key-fields &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/identifying-key-columns-using-sql-key-fields-sqlxml-4-0.md) e Specifica di relazioni tramite [sql:relationship &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-relationships-using-sql-relationship-sqlxml-4-0.md).  
  
-   **Tipi di dati**  
  
     SQLXML 4.0 presenta limitazioni nell'implementazione dei tipi di dati stringa **XPath**, **number** e **boolean.** Per altre informazioni, vedere [Tipi di dati XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/xpath-data-types-sqlxml-4-0.md).  
  
-   **Query di prodotto incrociato**  
  
     SQLXML 4.0 non supporta query XPath di prodotto incrociato, quale `Customers[Order/@OrderDate=Order/@ShipDate]`. In questa query vengono selezionati tutti gli elementi Customer con un elemento Order per il quale OrderDate è uguale a ShipDate di un elemento Order qualsiasi.  
  
     SQLXML 4.0 non supporta invece query come `Customer[Order[@OrderDate=@ShippedDate]]`, in cui vengono selezionati gli elementi Customer con elemento Order per il quale OrderDate è uguale al relativo elemento ShipDate.  
  
-   **Gestione degli errori e sicurezza**  
  
     A seconda dello schema e dell'espressione di query XPath utilizzati, è possibile che gli errori [!INCLUDE[tsql](../../includes/tsql-md.md)] vengano esposti agli utenti in determinate condizioni.  
  
 Nelle tabelle delle sezioni seguenti vengono forniti dettagli su come l'implementazione nelle aree indicate delle query XPath in SQLXML 4.0 sia diversa da quella definita nella specifica del W3C.  
  
## <a name="supported-functionality"></a>Funzionalità supportata  
 Nella tabella seguente vengono mostrate le caratteristiche del linguaggio XPath implementate in SQLXML 4.0.  
  
|Funzionalità|Elemento|Collegamento a query di esempio|  
|-------------|----------|----------------------------|  
|Assi|**attributi,** **elementi figlio,** **padre** e **assi** self|[Specifica degli assi nelle query XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-axes-in-xpath-queries-sqlxml-4-0.md)|  
|Predicati con valori booleani, tra i quali sono inclusi predicati successivi e nidificati||[Specifica degli operatori aritmetici nelle query XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-arithmetic-operators-in-xpath-queries-sqlxml-4-0.md)|  
|Tutti gli operatori relazionali|=, !=, <, \<=, > , >=|[Specifica di operatori relazionali nelle query XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-relational-operators-in-xpath-queries-sqlxml-4-0.md)|  
|Operatori aritmetici|+, -, *, div|[Specifica degli operatori aritmetici nelle query XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-arithmetic-operators-in-xpath-queries-sqlxml-4-0.md)|  
|Funzioni di conversione esplicita|**number()**, **string()**, **Boolean()**|[Specifica di funzioni di conversione esplicita nelle query XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-explicit-conversion-functions-in-xpath-queries-sqlxml-4-0.md)|  
|Operatori booleani|AND, OR|[Specifica di operatori booleani nelle query XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-boolean-operators-in-xpath-queries-sqlxml-4-0.md)|  
|funzioni booleane|**true()**, **false()**, **not()**|[Specifica di funzioni booleane nelle query XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-boolean-functions-in-xpath-queries-sqlxml-4-0.md)|  
|variabili XPath||[Specifica di variabili XPath nelle query XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-xpath-variables-in-xpath-queries-sqlxml-4-0.md)|  
  
## <a name="unsupported-functionality"></a>Funzionalità non supportata  
 Nella tabella seguente vengono mostrate le caratteristiche del linguaggio XPath non implementate in SQLXML 4.0.  
  
|Funzionalità|Elemento|  
|-------------|----------|  
|Assi|**predecessore**, **predecessore-o-self** **,** discendente , **descendant-or-self (//)** **,** dopo , dopo l'elemento di **pari** livello , **spazio** dei nomi **,** precedente , di pari livello **precedente**|  
|Predicati con valori numerici||  
|Operatori aritmetici|mod|  
|Funzioni nodo|**predecessore**, **predecessore-o-self** **,** discendente , **descendant-or-self (//)** **,** dopo , dopo l'elemento di **pari** livello , **spazio** dei nomi **,** precedente , di pari livello **precedente**|  
|Funzioni per i valori stringa|**string()**, **concat()**, **starts-with()**, **contains()**, **substring-before()**, **substring-after()**, **substring()**, **string-length()**, **normalize()**, **translate()**|  
|funzioni booleane|**lang()**|  
|Funzioni numeriche|**sum()**, **floor()**, **ceiling()**, **round()**|  
|Operatore Union|&#124;|  
  
 Quando si specificano query XPath in un modello, si noti il comportamento seguente:  
  
-   XPath può contenere caratteri come < o & che hanno significati speciali in XML (e il modello è un documento XML). È necessario usare caratteri di escape per questi caratteri & codifica XML oppure specificare XPath nell'URL.  
  
## <a name="see-also"></a>Vedere anche  
 [Utilizzo di query XPath in SQLXML 4.0](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/using-xpath-queries-in-sqlxml-4-0.md)  
  
  
