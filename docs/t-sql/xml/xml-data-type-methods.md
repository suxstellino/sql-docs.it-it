---
description: Metodi con tipo di dati XML
title: Metodi con tipo di dati XML
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- xml data type [SQL Server], methods
- methods [XML in SQL Server]
ms.assetid: d112b9c9-be9f-435c-a9e6-d21b65778fb7
author: rothja
ms.author: jroth
ms.openlocfilehash: 8f693edb056ee2a4880a19956d6165495d80c4af
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490199"
---
# <a name="xml-data-type-methods"></a>Metodi con tipo di dati XML
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  È possibile usare i metodi con tipo di dati **xml** per eseguire query su un'istanza XML archiviata in una variabile o una colonna di tipo **xml**. Negli argomenti di questa sezione viene descritto come usare i metodi con tipo di dati **xml**.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
|Argomento|Descrizione|  
|-----------|-----------------|  
|[Metodo query&#40;&#41; &#40;tipo di dati xml&#41;](../../t-sql/xml/query-method-xml-data-type.md)|Viene descritto come utilizzare il metodo query() per eseguire una query su un'istanza XML.|  
|[Metodo value&#40;&#41; &#40;tipo di dati xml&#41;](../../t-sql/xml/value-method-xml-data-type.md)|Viene descritto come utilizzare il metodo value() per recuperare un valore di tipo SQL da un'istanza XML.|  
|[Metodo exist&#40;&#41; &#40;tipo di dati xml&#41;](../../t-sql/xml/exist-method-xml-data-type.md)|Viene descritto come utilizzare il metodo exist() per determinare se una query restituisce un risultato non vuoto.|  
|[Metodo modify&#40;&#41; &#40;tipo di dati xml&#41;](../../t-sql/xml/modify-method-xml-data-type.md)|Viene descritto come usare il metodo modify() per specificare istruzioni del [Linguaggio XML di manipolazione dei dati &#40;XML DML&#41;](../../t-sql/xml/xml-data-modification-language-xml-dml.md) per l'esecuzione di aggiornamenti.|  
|[Metodo nodes&#40;&#41; &#40;tipo di dati xml&#41;](../../t-sql/xml/nodes-method-xml-data-type.md)|Viene descritto come utilizzare il metodo nodes() per suddividere XML in più righe in modo da propagare sezioni di documenti XML in set di righe.|  
|[Associazione di dati relazionali all'interno di dati XML](../../t-sql/xml/binding-relational-data-inside-xml-data.md)|Viene descritto come eseguire l'associazione di dati non XML in XML.|  
|[Linee guida per l'utilizzo dei metodi con tipo di dati xml](../../t-sql/xml/guidelines-for-using-xml-data-type-methods.md)|Vengono descritte le linee guida per l'uso dei metodi con tipo di dati **xml**.|  
  
 Per chiamare questi metodi, è necessario utilizzare la sintassi di richiamo dei metodi di tipo definito dall'utente. Ad esempio:  
  
```sql
SELECT XmlCol.query(' ... ')  
FROM Table  
```  
  
> [!NOTE]  
>  I metodi con tipo di dati **xml** **query()** , **value()** ed **exist()** restituiscono NULL se eseguiti in un'istanza XML NULL. Inoltre, il metodo **modify()** non restituisce alcun valore mentre il metodo **nodes()** restituisce set di righe tra cui uno vuoto con un input NULL.  
  
## <a name="see-also"></a>Vedere anche  
 [Confronto dati XML tipizzati con dati XML non tipizzati](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Creare istanze di dati XML](../../relational-databases/xml/create-instances-of-xml-data.md)  
  
  
