---
title: Guida di riferimento al linguaggio XQuery (SQL Server) | Microsoft Docs
description: Informazioni sul linguaggio XQuery per SQL Server e sulla visualizzazione di un riferimento completo al linguaggio.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
helpviewer_keywords:
- XQuery
- XQuery, about XQuery
- xml data type [SQL Server], XQuery
- XML [SQL Server], XQuery
- queries [XML in SQL Server], XQuery
ms.assetid: 8a69344f-2990-4357-8160-cb26aac95b91
author: rothja
ms.author: jroth
ms.openlocfilehash: 6fec2edcca6a5a73c8fcdb9bb0629b7b5ed2ff24
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340467"
---
# <a name="xquery-language-reference-sql-server"></a>Riferimento al linguaggio XQuery (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[tsql](../includes/tsql-md.md)] supporta un subset del linguaggio XQuery utilizzato per eseguire query sul tipo di dati **XML** . Questa implementazione di XQuery è allineata con le specifiche Working Draft di XQuery del luglio 2004. Il linguaggio è sviluppato da WC3 (World Wide Web Consortium), con la partecipazione di tutti i principali fornitori di database e di Microsoft. Poiché le specifiche W3C sono soggette a modifiche prima di diventare raccomandazioni ufficiali, questa implementazione può essere diversa dalla raccomandazione W3C finale. In questo argomento vengono descritte la semantica e la sintassi del subset di XQuery supportato in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 Per ulteriori informazioni, vedere la [specifica W3C XQuery 1,0 Language](https://go.microsoft.com/fwlink/?LinkId=48846).  
  
 Il linguaggio XQuery consente di eseguire query su dati XML strutturati e semistrutturati. Con il supporto del tipo di dati **XML** fornito in [!INCLUDE[ssDE](../includes/ssde-md.md)] , i documenti possono essere archiviati in un database e quindi sottoposti a query tramite XQuery.  
  
 Il linguaggio XQuery è basato sul linguaggio di query XPath esistente e supporta inoltre miglioramenti per le iterazioni e l'ordinamento, nonché la possibilità di costruire il codice XML necessario. XQuery opera sul modello di dati XQuery. Si tratta di un modello di astrazione dei documenti XML e i risultati XQuery possono essere tipizzati o non tipizzati. Le informazioni sui tipi sono basate sui tipi definiti nelle specifiche W3C per il linguaggio XML Schema. Se non sono disponibili informazioni di tipizzazione, XQuery gestisce i dati come non tipizzati, analogamente al modo in cui XPath versione 1.0 gestisce i dati XML.  
  
 Per eseguire una query su un'istanza XML archiviata in una variabile o una colonna di tipo **XML** , è possibile utilizzare i [metodi con tipo di dati XML](../t-sql/xml/xml-data-type-methods.md). È ad esempio possibile dichiarare una variabile di tipo **XML** ed eseguire query su di essa utilizzando il metodo **query ()** del tipo di dati **XML** .  
  
```sql
DECLARE @x xml  
SET @x = '<ROOT><a>111</a></ROOT>'  
SELECT @x.query('/ROOT/a')  
```  
  
 Nell'esempio seguente, la query viene specificata sulla colonna Instructions di tipo **XML** nella tabella ProductModel del database AdventureWorks.  
  
```sql
SELECT Instructions.query('declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";           
    /AWMI:root/AWMI:Location[@LocationID=10]  
') as Result   
FROM  Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 In XQuery sono incluse la dichiarazione dello spazio dei nomi, `declare namespace``AWMI=...` , e l'espressione di query `/AWMI:root/AWMI:Location[@LocationID=10]` .  
  
 Si noti che la query XQuery viene specificata sulla colonna Instructions di tipo **XML** . Il [metodo query ()](../t-sql/xml/query-method-xml-data-type.md) del tipo di dati XML viene utilizzato per specificare l'espressione XQuery.  
  
 Nella tabella seguente sono elencati gli argomenti correlati utili per comprendere l'implementazione di XQuery in [!INCLUDE[ssDE](../includes/ssde-md.md)].  
  
|Argomento|Descrizione|  
|-----------|-----------------|  
|[Dati XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)|Viene illustrato il supporto per il tipo di dati **XML** in [!INCLUDE[ssDE](../includes/ssde-md.md)] e i metodi che è possibile utilizzare per questo tipo di dati. Il tipo di dati **XML** costituisce il modello di dati XQuery di input su cui vengono eseguite le espressioni XQuery.|  
|[Raccolte di XML Schema &#40;SQL Server&#41;](../relational-databases/xml/xml-schema-collections-sql-server.md)|Descrive le procedure per tipizzare le istanze XML archiviate in un database, Ciò significa che è possibile associare una raccolta XML Schema alla colonna di tipo **XML** . Tutte le istanze archiviate nella colonna vengono convalidate e tipizzate rispetto allo schema della raccolta e forniscono le informazioni sul tipo per XQuery.|  
|||  
  
> [!NOTE]  
>  L'organizzazione di questa sezione è basata sulle specifiche W3C (World Wide Web Consortium ) per XQuery. Alcuni dei diagrammi disponibili in questa sezione provengono da questa specifica. In questa sezione l'implementazione Microsoft di XQuery viene confrontata alla specifica W3C, vengono descritte le differenze fra Microsoft XQuery e la specifica W3C e vengono illustrate le caratteristiche W3C non supportate. La specifica W3C è disponibile all'indirizzo [http://www.w3.org/TR/2004/WD-xquery-20040723](https://go.microsoft.com/fwlink/?LinkId=48846) .  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
|Argomento|Descrizione|  
|-----------|-----------------|  
|[Nozioni fondamentali su XQuery](../xquery/xquery-basics.md)|Contiene una panoramica di base sui concetti relativi a XQuery, valutazione dell'espressione (contesto statico e dinamico), atomizzazione, valore booleano effettivo, sistema di tipi XQuery, corrispondenza del tipo di sequenza e gestione degli errori.|  
|[Espressioni XQuery](../xquery/xquery-expressions.md)|Descrive le espressioni primarie XQuery, espressioni di percorso, espressioni di sequenza, espressioni di confronto aritmetico e logiche, costruzione XQuery, espressione FLWOR, espressioni condizionali e quantificate e diverse espressioni TypeSequence.|  
|[Moduli e Prolog &#40;XQuery&#41;](../xquery/modules-and-prologs-xquery.md)|Descrive il prologo della XQuery.|  
|[Funzioni XQuery per il tipo di dati XML](../xquery/xquery-functions-against-the-xml-data-type.md)|Contiene un elenco delle funzioni XQuery supportate.|  
|[Operatori di XQuery per il tipo di dati xml](../xquery/xquery-operators-against-the-xml-data-type.md)|Descrive gli operatori XQuery supportati.|  
|[Query XQuery di esempio aggiuntive per il tipo di dati xml](../xquery/additional-sample-xqueries-against-the-xml-data-type.md)|Contiene altri esempi di XQuery.|  
  
## <a name="see-also"></a>Vedere anche  
 [Dati XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Raccolte di XML Schema &#40;SQL Server&#41;](../relational-databases/xml/xml-schema-collections-sql-server.md)   
 [Esempi di importazione ed esportazione bulk di documenti XML &#40;SQL Server&#41;](../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)  
  
  
