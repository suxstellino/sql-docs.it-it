---
title: Gestione degli errori (XQuery) | Microsoft Docs
description: Informazioni sulla gestione degli errori in XQuery e la visualizzazione di esempi di gestione degli errori dinamici.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- static errors
- errors [XQuery]
- XQuery, error handling
- dynamic errors [XQuery]
ms.assetid: 7dee3c11-aea0-4d10-9126-d54db19448f2
author: rothja
ms.author: jroth
ms.openlocfilehash: 61e6b8b53e4519eb88f94f9521434b2e48aac40d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335864"
---
# <a name="error-handling-xquery"></a>Gestione degli errori (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  La specifica W3C consente la generazione di errori statica oppure dinamica e definisce gli errori statici, dinamici e di tipo.  
  
## <a name="compilation-and-error-handling"></a>Compilazione e gestione degli errori  
 Gli errori di compilazione derivano da espressioni XQuery e istruzioni XML DML con sintassi non corretta. Durante la fase di compilazione viene verificata la correttezza dei tipi statici di espressioni XQuery e istruzioni DML e vengono utilizzati XML Schema per l'inferenza dei tipi per gli XML tipizzati. Viene generato un errore di tipo statico se è possibile che, in fase di esecuzione, un'espressione non riesca a causa di una violazione dell'indipendenza dai tipi. Questo si verifica ad esempio quando una stringa viene sommata a un valore intero oppure si esegue una query per individuare dati tipizzati in un nodo inesistente.  
  
 Diversamente da quanto previsto dallo standard W3C, gli errori di run-time di XQuery vengono convertiti in sequenze vuote, che possono essere propagate come valori vuoti XML o NULL al risultato della query, a seconda del contesto di chiamata.  
  
 Per evitare gli errori statici è possibile eseguire il cast esplicito al tipo corretto, tuttavia gli errori di cast che si verificano in fase di esecuzione vengono convertiti in sequenze vuote.  
  
## <a name="static-errors"></a>Errori statici  
 Gli errori statici vengono restituiti utilizzando il meccanismo di gestione degli errori [!INCLUDE[tsql](../includes/tsql-md.md)]. In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], gli errori di tipo XQuery vengono restituiti in modo statico. Per ulteriori informazioni, vedere [XQuery e tipizzazione statica](../xquery/xquery-and-static-typing.md).  
  
## <a name="dynamic-errors"></a>Errori dinamici  
 In XQuery, nella maggior parte dei casi sugli errori dinamici viene eseguito il mapping a una sequenza vuota ("()"), con due eccezioni: condizioni di overflow nelle funzioni di aggregazione XQuery ed errori di convalida XML DML. Si noti che nella maggior parte dei casi sugli errori dinamici viene eseguito il mapping a una sequenza vuota ("()"). In caso contrario, l'esecuzione della query che utilizza gli indici XML può generare errori imprevisti. Pertanto, per garantire un'esecuzione efficiente senza la generazione di errori imprevisti, [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] esegue il mapping degli errori dinamici a ().  
  
 Nei casi in cui l'errore dinamico verrebbe generato all'interno di un predicato, spesso non generare l'errore non modifica la semantica, poiché () viene eseguito il mapping a False. In alcuni casi tuttavia, la restituzione di () invece di un errore dinamico può causare risultati imprevisti, come illustrato negli esempi seguenti.  
  
### <a name="example-using-the-avg-function-with-a-string"></a>Esempio: utilizzo della funzione avg () con una stringa  
 Nell'esempio seguente viene chiamata la [funzione AVG](../xquery/aggregate-functions-avg.md) per calcolare la media dei tre valori. Uno dei valori è una stringa. Poiché l'istanza XML in questo caso è non tipizzata, tutti i dati al suo interno sono di tipo atomico non tipizzato. La funzione **AVG ()** esegue prima il cast di questi valori in **xs: Double** prima di calcolare la media. `"Hello"`Non è tuttavia possibile eseguire il cast del valore,, a **xs: Double** e viene creato un errore dinamico. In questo caso, anziché restituire un errore dinamico, il cast di `"Hello"` a **xs: Double** causa una sequenza vuota. La funzione **AVG ()** ignora questo valore, calcola la media degli altri due valori e restituisce 150.  
  
```  
DECLARE @x xml  
SET @x=N'<root xmlns:myNS="test">  
 <a>100</a>  
 <b>200</b>  
 <c>Hello</c>  
</root>'  
SELECT @x.query('avg(//*)')  
```  
  
### <a name="example-using-the-not-function"></a>Esempio: utilizzo della funzione not  
 Quando si usa la [funzione not](../xquery/functions-on-boolean-values-not-function.md) in un predicato, ad esempio, `/SomeNode[not(Expression)]` e l'espressione genera un errore dinamico, invece di un errore viene restituita una sequenza vuota. Se si applica **Not ()** alla sequenza vuota, invece di un errore viene restituito true.  
  
### <a name="example-casting-a-string"></a>Esempio: esecuzione del cast di una stringa  
 Nell'esempio seguente viene eseguito il cast della stringa letterale "NaN" a xs:string, quindi a xs:double. Il risultato è un set di righe vuoto. Anche se non è possibile eseguire correttamente il cast della stringa "NaN" a xs:double, questo fatto non può essere determinato fino al momento dell'esecuzione, perché prima viene eseguito il cast della stringa a xs:string.  
  
```  
DECLARE @x XML  
SET @x = ''  
SELECT @x.query(' xs:double(xs:string("NaN")) ')  
GO  
```  
  
 In questo esempio tuttavia, viene generato un errore di tipo statico.  
  
```  
DECLARE @x XML  
SET @x = ''  
SELECT @x.query(' xs:double("NaN") ')  
GO  
```  
  
#### <a name="implementation-limitations"></a>Limitazioni di implementazione  
 La funzione **FN: Error ()** non è supportata.  
  
## <a name="see-also"></a>Vedere anche  
 [Riferimento al linguaggio XQuery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)   
 [Nozioni fondamentali su XQuery](../xquery/xquery-basics.md)  
  
  
