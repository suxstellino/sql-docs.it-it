---
title: Espressioni primarie (XQuery) | Microsoft Docs
description: Informazioni sulle espressioni primarie XQuery che includono valori letterali, riferimenti a variabili, espressioni di elementi di contesto, costruttori e chiamate di funzioni.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- variable references [XQuery]
- primary expressions [XQuery]
- function calls [XQuery]
- expressions [XQuery], primary
- literals [XQuery]
- context item expressions [XQuery]
ms.assetid: d4183c3e-12b5-4ca0-8413-edb0230cb159
author: rothja
ms.author: jroth
ms.openlocfilehash: 2f474aae3e764224bec01830b42766216a27bc9d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339952"
---
# <a name="primary-expressions-xquery"></a>Espressioni primarie (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Le espressioni primarie XQuery includono valori letterali, riferimenti a variabili, espressioni per elementi di contesto, costruttori e chiamate di funzioni.  
  
## <a name="literals"></a>Valori letterali  
 I valori letterali XQuery possono essere di tipo numerico o stringa. Un valore letterale stringa può includere riferimenti a entità predefinite, ognuno dei quali è costituito da una sequenza di caratteri. La sequenza inizia con una e commerciale che rappresenta un singolo carattere che altrimenti potrebbe avere un significato sintattico. Di seguito sono indicati i riferimenti a entità predefinite per XQuery.  
  
|Riferimento a entità|Rappresenta|  
|----------------------|----------------|  
|`&lt;`|\<|  
|`&gt;`|>|  
|`&amp;`|&|  
|`&quot;`|"|  
|`&apos;`|'|  
  
 Un valore letterale stringa può contenere inoltre un riferimento a carattere, un riferimento in stile XML a un carattere Unicode, identificato dal relativo punto di codice decimale o esadecimale. Il simbolo euro, ad esempio, può essere rappresentato dal riferimento al carattere "&\# 8364;".  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilizza XML versione 1.0 come base per l'analisi.  
  
### <a name="examples"></a>Esempio  
 Negli esempi seguenti viene illustrato l'utilizzo dei valori letterali, nonché dei riferimenti a entità e di carattere.  
  
 Il codice restituisce un errore, in quanto i caratteri `<'` e `'>` hanno un significato speciale:  
  
```  
DECLARE @var XML  
SET @var = ''  
SELECT @var.query(' <SalaryRange>Salary > 50000 and < 100000</SalaryRange>')  
GO  
```  
  
 Se invece si utilizza un riferimento a entità, la query funziona.  
  
```  
DECLARE @var XML  
SET @var = ''  
SELECT @var.query(' <SalaryRange>Salary &gt; 50000 and &lt; 100000</SalaryRange>')  
GO  
```  
  
 Nell'esempio seguente viene illustrato l'utilizzo di un riferimento a carattere per rappresentare il simbolo dell'euro.  
  
```  
DECLARE @var XML  
SET @var = ''  
SELECT @var.query(' <a>€12.50</a>')  
```  
  
 Di seguito è riportato il risultato.  
  
 `<a>€12.50</a>`  
  
 Nell'esempio seguente, la query è delimitata da apostrofi. Pertanto, l'apostrofo nel valore stringa è rappresentato da due apostrofi adiacenti.  
  
```  
DECLARE @var XML  
SET @var = ''  
SELECT @var.query('<a>I don''t know</a>')  
Go  
```  
  
 Di seguito è riportato il risultato.  
  
 `<a>I don't know</a>`  
  
 Le funzioni booleane predefinite, **true ()** e **false ()**, possono essere usate per rappresentare valori booleani, come illustrato nell'esempio seguente.  
  
```  
DECLARE @var XML  
SET @var = ''  
SELECT @var.query('<a>{true()}</a>')  
GO  
```  
  
 Il costruttore diretto di elementi specifica un'espressione tra parentesi graffe, che viene sostituita dal valore corrispondente nella struttura XML risultante.  
  
 Di seguito è riportato il risultato.  
  
 `<a>true</a>`  
  
## <a name="variable-references"></a>Riferimenti a variabili  
 In XQuery, un riferimento a una variabile è un elemento QName preceduto dal segno di dollaro ($). Tale implementazione supporta solo riferimenti a variabili senza prefisso. Ad esempio, la query seguente definisce la variabile `$i` nell'espressione FLWOR.  
  
```  
DECLARE @var XML  
SET @var = '<root>1</root>'  
SELECT @var.query('  
 for $i in /root return data($i)')  
GO  
```  
  
 La query seguente non funzionerà poiché viene aggiunto un prefisso dello spazio dei nomi al nome della variabile:  
  
```  
DECLARE @var XML  
SET @var = '<root>1</root>'  
SELECT @var.query('  
DECLARE namespace x="https://X";  
for $x:i in /root return data($x:i)')  
GO  
```  
  
 È possibile utilizzare la funzione di estensione SQL: Variable () per fare riferimento alle variabili SQL, come illustrato nella query seguente.  
  
```  
DECLARE @price money  
SET @price=2500  
DECLARE @x xml  
SET @x = ''  
SELECT @x.query('<value>{sql:variable("@price") }</value>')  
```  
  
 Di seguito è riportato il risultato.  
  
 `<value>2500</value>`  
  
#### <a name="implementation-limitations"></a>Limitazioni di implementazione  
 Le limitazioni di implementazione sono le seguenti:  
  
-   Non sono supportate variabili con prefissi dello spazio dei nomi.  
  
-   Non è supportata l'importazione di moduli.  
  
-   Non sono supportate dichiarazioni di variabili esterne. Una soluzione a questo problema consiste nell'usare la [funzione SQL: Variable ()](../xquery/xquery-extension-functions-sql-variable.md).  
  
## <a name="context-item-expressions"></a>Espressioni per elementi di contesto  
 Per elemento di contesto si intende l'elemento corrente elaborato nel contesto di un'espressione di percorso. Viene inizializzato in un'istanza con tipo di dati XML non NULL tramite il nodo di documento. Può anche essere modificato dal metodo nodes () nel contesto di espressioni XPath o dei predicati [].  
  
 L'elemento di contesto viene restituito da un'espressione che contiene un punto (.). Ad esempio, la query seguente valuta ogni elemento <`a`> per la presenza dell'attributo `attr` . Se l'attributo è presente, viene restituito l'elemento. Si noti che la condizione del predicato specifica che il nodo di contesto è specificato da un solo punto.  
  
```  
DECLARE @var XML  
SET @var = '<ROOT>  
<a>1</a>  
<a attr="1">2</a>  
</ROOT>'  
SELECT @var.query('/ROOT[1]/a[./@attr]')  
```  
  
 Di seguito è riportato il risultato.  
  
 `<a attr="1">2</a>`  
  
## <a name="function-calls"></a>Chiamate di funzioni  
 È possibile chiamare le funzioni XQuery predefinite e le [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] funzioni SQL: Variable () e SQL: Column (). Per un elenco delle funzioni implementate, vedere [funzioni XQuery per il tipo di dati XML](../xquery/xquery-functions-against-the-xml-data-type.md).  
  
#### <a name="implementation-limitations"></a>Limitazioni di implementazione  
 Le limitazioni di implementazione sono le seguenti:  
  
-   Non è supportata la dichiarazione di funzione nel prologo XQuery.  
  
-   Non è supportata l'importazione di funzioni.  
  
## <a name="see-also"></a>Vedere anche  
 [Costrutto XML &#40;XQuery&#41;](../xquery/xml-construction-xquery.md)
 
