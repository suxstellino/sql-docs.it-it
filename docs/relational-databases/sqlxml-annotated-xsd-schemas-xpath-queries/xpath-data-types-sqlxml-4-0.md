---
title: Tipi di dati XPath (SQLXML)
description: Informazioni sui tipi di dati XPath in SQLXML 4.0 e sul confronto con i tipi di dati Microsoft SQL Server e XML Schema (XSD).
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- mapping XDR types to XPath types [SQLXML]
- data types [XPath]
- arithmetic operators
- mapping data types [SQLXML]
- relational operators [SQLXML]
- node-set [SQLXML]
- data types [SQLXML], XPath
- XPath operators [SQLXML]
- XDR data type [SQLXML]
- equality operators [SQLXML]
- XPath conversions [SQLXML]
- converting data types [SQLXML]
- Boolean operators
- XPath queries [SQLXML], data types
- XPath data types [SQLXML]
- operators [SQLXML]
ms.assetid: a90374bf-406f-4384-ba81-59478017db68
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e4f212129c020661eeb9a207e7b5bbe2c02ec15a
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491423"
---
# <a name="xpath-data-types-sqlxml-40"></a>Tipi di dati XPath (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], XPath e XML Schema (XSD) hanno tipi di dati molto diversi. XPath, ad esempio, non include tipi di dati integer o di data, mentre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e XSD ne includono diversi. XSD utilizza una precisione in nanosecondi per i valori di ora, mentre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizza al massimo una precisione di 1/300 secondi. Di conseguenza, il mapping di un tipo di dati a un altro non è sempre possibile. Per altre informazioni sul mapping dei tipi di dati ai tipi di dati XSD, vedere Coercizioni dei tipi di dati e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [annotazione sql:datatype &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/data-type-coercions-and-the-sql-datatype-annotation-sqlxml-4-0.md).  
  
 XPath ha tre tipi di dati: **string**, **number** e **boolean**. Il **tipo di** dati number è sempre a virgola mobile a precisione doppia IEEE 754. Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **tipo di dati float(53)** è il più vicino al numero XPath . Tuttavia, **float(53)** non è esattamente IEEE 754. Ad esempio, non viene utilizzato né un valore diverso da un numero (NaN, Not-a-Number) né un valore infinito. Se si tenta di convertire una stringa non numerica in **number** e si tenta di dividere per zero, viene generato un errore.  
  
## <a name="xpath-conversions"></a>Conversioni XPath  
 Quando si utilizza una query XPath, ad esempio `OrderDetail[@UnitPrice > "10.0"]`, le conversioni dei tipi di dati implicite ed esplicite possono modificare impercettibilmente il significato della query. È pertanto importante comprendere le modalità di implementazione dei tipi di dati XPath. La specifica del linguaggio XPath, XPath (XML Path Language) versione 1.0 W3C Proposed Recommendation 8 ottobre 1999, è disponibile nel sito Web W3C all'indirizzo http://www.w3.org/TR/1999/PR-xpath-19991008.html .  
  
 Gli operatori XPath sono suddivisi in quattro categorie:  
  
-   Operatori booleani (and, or)  
  
-   Operatori relazionali ( \<, > , \<=, > =)  
  
-   Operatori di uguaglianza (=, !=)  
  
-   Operatori aritmetici: (+, -, *, div, mod)  
  
 Ogni categoria di operatore converte in modo diverso gli operandi. Se necessario, gli operatori XPath convertono gli operandi in modo implicito. Gli operatori aritmetici convertono gli operandi in **number** e re come risultato un valore numerico. Gli operatori booleani convertono gli operandi **in valori booleani** e restituiscono un valore booleano. Gli operatori relazionali e gli operatori di uguaglianza restituiscono un valore booleano, ma utilizzano regole di conversione diverse a seconda dei tipi di dati originali degli operandi, come illustrato nella tabella seguente.  
  
|Operando|Operatore relazionale|Operatore di uguaglianza|  
|-------------|-------------------------|-----------------------|  
|Entrambi gli operandi sono set di nodi.|TRUE solo se è presente un nodo in un set e un nodo nel secondo set in modo che il confronto dei **relativi** valori stringa sia TRUE.|Idem|  
|Uno è un set di nodi, l'altro è una **stringa**.|TRUE se e solo se è presente un nodo nel set di nodi in  modo che, quando viene convertito nel numero **,** il confronto tra di esso e la stringa convertita in **numero** sia TRUE.|TRUE se e solo se è presente un nodo nel set di nodi in modo tale che, quando viene convertito in **stringa,** il confronto di tale nodo con la **stringa** sia TRUE.|  
|Uno è un set di nodi, l'altro un **numero**.|TRUE se e solo se è presente un nodo nel set di nodi in modo che, se convertito in numero **,** il confronto con il **numero** sia TRUE.|Idem|  
|Uno è un set di nodi, l'altro è un valore **booleano.**|TRUE se e solo se è presente un nodo nel set di nodi in modo che, quando viene convertito in **booleano** e quindi nel numero **,** il confronto di tale nodo con il valore **booleano** convertito in **numero** è TRUE.|TRUE se e solo se nel set di nodi è presente un nodo tale che, quando viene convertito in un valore **booleano,** il confronto di tale nodo con il **valore booleano** è TRUE.|  
|Nessuno è un set di nodi.|Convertire entrambi gli operandi in **number** e quindi confrontare.|Convertire entrambi gli operandi in un tipo comune e quindi eseguire il confronto. Converte **in boolean** se uno dei due è **booleano,** **number** se uno dei due è **number**; In caso contrario, eseguire la conversione **in stringa**.|  
  
> [!NOTE]  
>  Poiché gli operatori relazionali XPath convertono sempre gli operandi in **number** **,** i confronti tra stringhe non sono possibili. Per includere confronti di data, SQL Server 2000 offre questa variazione rispetto alla specifica  XPath: quando un operatore relazionale confronta una stringa con una stringa **,** un node-set **su** una  stringa o un node-set con valori stringa su un set di nodi con valori stringa, viene eseguito un confronto tra stringhe (non un confronto di numeri).   
  
## <a name="node-set-conversions"></a>Conversioni dei set di nodi  
 Le conversioni dei set di nodi non sono sempre intuitive. Un set di nodi viene convertito in **una stringa** prendendo il valore stringa solo del primo nodo del set. Un set di nodi viene convertito in **numero** converterlo in **stringa** e quindi converte **la stringa** in **number**. Un set di nodi viene convertito in **booleano** verificando la sua esistenza.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non esegue la selezione della posizione nei set di nodi: la query XPath `Customer[3]`, ad esempio, indica il terzo cliente. Questo tipo di selezione della posizione non è supportato in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Di conseguenza, le conversioni node-set-to-string o node-set-to-number come descritto nella specifica XPath non vengono implementate. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizza le semantiche "any" dove la specifica XPath specifica la semantica "first". Ad esempio, in base alla specifica XPath W3C, la query XPath seleziona gli ordini con il primo `Order[OrderDetail/@UnitPrice > 10.0]` **OrderDetail** con un **valore UnitPrice** maggiore di 10,0. In questa query XPath seleziona gli ordini con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **OrderDetail** con **unitPrice** maggiore di 10,0.  
  
 La conversione **in un valore booleano** genera un test di esistenza. Pertanto, la query XPath è equivalente all'espressione `Products[@Discontinued=true()]` SQL "Products.Discontinued is not null", non all'espressione SQL "Products.Discontinued = 1". Per rendere la query equivalente alla seconda espressione SQL, convertire prima il set di nodi in un tipo non **booleano,** ad esempio **number**. Ad esempio: `Products[number(@Discontinued) = true()]`.  
  
 Poiché la maggior parte degli operatori viene definita come TRUE se gli operatori sono TRUE per tutti i nodi nel set di nodi o per uno di essi, queste operazioni restituiscono sempre FALSE se il set di nodi è vuoto. In questo modo, se A è vuoto, sia `A = B` sia `A != B` sono FALSE e `not(A=B)` e `not(A!=B)` sono TRUE.  
  
 In genere, un attributo o un elemento che esegue il mapping a una colonna esiste se il valore di tale colonna nel database non è **Null.** Sono presenti elementi di cui è stato eseguito il mapping a righe se è presente uno qualunque dei figli.  
  
> [!NOTE]  
>  Gli elementi annotati con **is-constant** esistono sempre. Di conseguenza, i predicati XPath non possono essere usati su **elementi is-constant.**  
  
 Quando un set di  nodi viene convertito in stringa o **numero,** il relativo tipo XDR (se presente) viene controllato nello schema con annotazioni e tale tipo viene usato per determinare la conversione necessaria.  
  
## <a name="mapping-xdr-data-types-to-xpath-data-types"></a>Mapping di tipi di dati XDR a tipi di dati XPath  
 Il tipo di dati XPath di un nodo deriva dal tipo di dati XDR nello schema, come illustrato nella tabella seguente (il nodo **EmployeeID** viene usato a scopo illustrativo).  
  
|Tipo di dati XDR|Equivalente<br /><br /> Tipo di dati XPath|Conversione SQL Server utilizzata|  
|-------------------|------------------------------------|--------------------------------|  
|Nonebin.base64bin.hex|N/D|NoneEmployeeID|  
|boolean|boolean|CONVERT(bit, EmployeeID)|  
|number, int, float, i1, i2, i4, i8,r4, r8ui1, ui2, ui4, ui8|d'acquisto|CONVERT(float(53), EmployeeID)|  
|id, idref, idrefsentity, entities, enumerationnotation, nmtoken, nmtokens, chardate, Timedate, Time.tz, string, uri, uuid|string|CONVERT(nvarchar(4000), EmployeeID, 126)|  
|fixed14.4|N/D (in XPath non è disponibile alcun tipo di dati equivalente al tipo di dati XDR fixed14.4).|CONVERT(money, EmployeeID)|  
|data|string|LEFT(CONVERT(nvarchar(4000), EmployeeID, 126), 10)|  
|time<br /><br /> time.tz|string|SUBSTRING(CONVERT(nvarchar(4000), EmployeeID, 126), 1 + CHARINDEX(N'T', CONVERT(nvarchar(4000), EmployeeID, 126)), 24)|  
  
 Le conversioni di data e ora sono progettate per funzionare indipendentemente dal fatto che il valore sia archiviato nel database usando il tipo di dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **datetime** o una **stringa**. Si noti che il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **tipo di dati datetime** non usa il **fuso** orario e ha una precisione inferiore rispetto al tipo di dati **time** XML. Per includere il tipo **di dati del** fuso orario o una precisione aggiuntiva, archiviare i dati in usando un tipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **stringa.**  
  
 Quando un nodo viene convertito dal relativo tipo di dati XDR al tipo di dati XPath, è talvolta necessaria un'ulteriore conversione (da un tipo di dati XPath a un altro tipo di dati XPath). Si consideri, ad esempio, la query XPath seguente:  
  
```  
(@m + 3) = 4  
```  
  
 Se è del tipo di dati @m XDR **fixed14.4,** la conversione dal tipo di dati XDR al tipo di dati XPath viene eseguita utilizzando:  
  
```  
CONVERT(money, m)  
```  
  
 In questa conversione il nodo `m` viene convertito da **fixed14.4** a **money**. Per aggiungere il valore 3, tuttavia, è necessaria un'ulteriore conversione:  
  
```  
CONVERT(float(CONVERT(money, m))  
```  
  
 L'espressione XPath viene valutata nel modo seguente:  
  
```  
CONVERT(float(CONVERT(money, m)) + CONVERT(float(53), 3) = CONVERT(float(53), 3)  
```  
  
 Come illustrato nella tabella seguente, si tratta della stessa conversione applicata per altre espressioni XPath, ad esempio i valori letterali o le espressioni composte.  
  
|   | X non è noto | X è una stringa | X è il numero | X è booleano |
| - | ------------ | ----------- | ----------- | ------------ |
| **string(X)** |CONVERT (nvarchar(4000), X, 126)|-|CONVERT (nvarchar(4000), X, 126)|CASE WHEN X THEN N'true' ELSE N'false' END|  
| **number(X)** |CONVERT (float(53), X)|CONVERT (float(53), X)|-|CASE WHEN X THEN 1 ELSE 0 END|  
| **boolean(X)** |-|LEN(X) > 0|X != 0|-|  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-convert-a-data-type-in-an-xpath-query"></a>R. Convertire un tipo di dati in una query XPath  
 Nella query XPath seguente specificata in base a uno schema XSD con annotazioni, la query seleziona tutti i nodi **Employee** con il valore dell'attributo **EmployeeID** E-1, dove "E-" è il prefisso specificato usando l'annotazione **sql:id-prefix.**  
  
 `Employee[@EmployeeID="E-1"]`  
  
 Il predicato nella query equivale all'espressione SQL:  
  
 `N'E-' + CONVERT(nvarchar(4000), Employees.EmployeeID, 126) = N'E-1'`  
  
 Poiché **EmployeeID** è uno dei valori del tipo di dati **id** (**idref**, **idrefs**, **nmtoken**, **nmtokens** e così via) nello schema XSD, **EmployeeID** viene convertito nel tipo di dati **XPath** della stringa usando le regole di conversione descritte in precedenza.  
  
 `CONVERT(nvarchar(4000), Employees.EmployeeID, 126)`  
  
 Il prefisso "E-" viene aggiunto alla stringa, e il risultato viene quindi confrontato con `N'E-1'`.  
  
### <a name="b-perform-several-data-type-conversions-in-an-xpath-query"></a>B. Eseguire diverse conversioni dei tipi di dati in una query XPath  
 Considerare la seguente query XPath specificata su uno schema XSD con annotazioni: `OrderDetail[@UnitPrice * @OrderQty > 98]`  
  
 Questa query XPath restituisce tutti gli **\<OrderDetail>** elementi che soddisfano il predicato `@UnitPrice * @OrderQty > 98` . Se **unitPrice** è annotato con un tipo di dati **fixed14.4** nello schema con annotazioni, questo predicato è equivalente all'espressione SQL:  
  
 `CONVERT(float(53), CONVERT(money, OrderDetail.UnitPrice)) * CONVERT(float(53), OrderDetail.OrderQty) > CONVERT(float(53), 98)`  
  
 Nel convertire i valori nella query XPath, la prima operazione converte il tipo di dati XDR nel tipo di dati XPath. Poiché il tipo di dati XSD **di UnitPrice** è **fixed14.4,** come descritto nella tabella precedente, si tratta della prima conversione usata:  
  
```  
CONVERT(money, OrderDetail.UnitPrice))   
```  
  
 Poiché gli operatori aritmetici convertono gli operandi nel tipo di dati **XPath** number, viene applicata la seconda conversione (da un tipo di dati XPath a un altro tipo di dati **XPath)** in cui il valore viene convertito in **float(53)** (**float(53)** è vicino al tipo di dati number XPath):  
  
```  
CONVERT(float(53), CONVERT(money, OrderDetail.UnitPrice))   
```  
  
 Supponendo che **l'attributo OrderQty** non abbia un tipo di dati XSD, **OrderQty** viene convertito in un tipo di dati **XPath** number in una singola conversione:  
  
```  
CONVERT(float(53), OrderDetail.OrderQty)  
```  
  
 Analogamente, il valore 98 viene convertito nel **tipo di** dati XPath number:  
  
```  
CONVERT(float(53), 98)  
```  
  
> [!NOTE]  
>  Se il tipo di dati XSD utilizzato nello schema non è compatibile con il tipo di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sottostante nel database o se viene eseguita una conversione del tipo di dati XPath non consentita, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può restituire un errore. Ad esempio, se l'attributo **EmployeeID** viene annotato con l'annotazione **id-prefix,** XPath genera un errore, perché `Employee[@EmployeeID=1]` **EmployeeID** ha l'annotazione **id-prefix** e non può essere convertito in **number**.  
  
  
