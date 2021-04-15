---
title: Usare i metodi value() e nodes() con OPENXML | Microsoft Docs
description: Informazioni su come estrarre un set di righe di valori XML in una query SQL usando i metodi value() e nodes() o il metodo OpenXML().
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- OpenXML method [XML in SQL Server]
- value method [XML in SQL Server]
- nodes method [XML in SQL Server]
ms.assetid: c73dbe55-d685-42eb-b0ee-9f3c5b9d97f3
author: rothja
ms.author: jroth
ms.openlocfilehash: f4d227752409443d7a9b96c8499db2e3534e9658
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487049"
---
# <a name="use-the-value-and-nodes-methods-with-openxml"></a>Utilizzare i metodi value() e nodes() con OPENXML
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  È possibile usare più metodi **value()** su un tipo di dati **xml** in una clausola **SELECT** per generare un set di righe di valori estratti. Il metodo **nodes()** crea un riferimento interno per ogni nodo selezionato, che può essere usato per altre query. Se sono presenti numerose colonne e quando per la creazione del set di righe vengono usate espressioni di percorso complesse, si può usare una combinazione dei metodi **nodes()** e **value()** per generare il set di righe in modo più efficiente.  
  
 Il metodo **nodes()** crea istanze di uno speciale tipo di dati **xml** , ognuna delle quali ha il contesto impostato su un nodo selezionato diverso. Questo tipo di istanza XML supporta i metodi **query()** , **value()** , **nodes()** e **exist()** e può essere usato nelle aggregazioni **count(\*)** . Tutti gli altri utilizzi generano un errore.  
  
## <a name="example-using-nodes"></a>Esempio: Utilizzo del metodo nodes()  
 Si supponga di voler estrarre il nome e il cognome degli autori il cui nome è diverso da "David". Si supponga inoltre di voler estrarre tali informazioni sotto forma di un set di righe contenente due colonne, FirstName e LastName. A questo scopo si possono usare i metodi **nodes()** e **value()** , come mostrato nell'esempio seguente:  
  
```  
SELECT nref.value('(first-name/text())[1]', 'nvarchar(50)') FirstName,  
       nref.value('(last-name/text())[1]', 'nvarchar(50)') LastName  
FROM   T CROSS APPLY xCol.nodes('//author') AS R(nref)  
WHERE  nref.exist('first-name[. != "David"]') = 1  
```  
  
 In questo esempio `nodes('//author')` crea un set di righe di riferimenti agli elementi `<author>` per ogni istanza XML. I nomi e cognomi degli autori vengono ottenuti valutando i metodi **value()** in relazione a quei riferimenti.  
  
 SQL Server 2000 consente di generare un set di righe da un'istanza XML usando **OpenXml()** . È possibile specificare lo schema relazionale del set di righe e il mapping tra i valori presenti nell'istanza XML e le colonne nel set di righe.  
  
## <a name="example-using-openxml-on-the-xml-data-type"></a>Esempio: Utilizzo di OpenXml() sul tipo di dati XML  
 È possibile riscrivere la query dell'esempio precedente usando **OpenXml()** come mostrato nell'esempio seguente. A tale scopo viene creato un cursore che legge ogni istanza XML in una variabile XML e quindi applica OpenXML alla variabile:  
  
```  
DECLARE name_cursor CURSOR  
FOR  
   SELECT xCol   
   FROM   T  
OPEN name_cursor  
DECLARE @xmlVal XML  
DECLARE @idoc int  
FETCH NEXT FROM name_cursor INTO @xmlVal  
  
WHILE (@@FETCH_STATUS = 0)  
BEGIN  
   EXEC sp_xml_preparedocument @idoc OUTPUT, @xmlVal  
   SELECT   *  
   FROM   OPENXML (@idoc, '//author')  
          WITH (FirstName  varchar(50) 'first-name',  
                LastName   varchar(50) 'last-name') R  
   WHERE  R.FirstName != 'David'  
  
   EXEC sp_xml_removedocument @idoc  
   FETCH NEXT FROM name_cursor INTO @xmlVal  
END  
CLOSE name_cursor  
DEALLOCATE name_cursor   
```  
  
 **OpenXml()** crea una rappresentazione in memoria e usa tabelle di lavoro anziché Query Processor. Si basa inoltre sul processore XPath versione 1.0 di MSXML versione 3.0, piuttosto che sul motore XQuery. Le tabelle di lavoro non vengono condivise tra più chiamate a **OpenXml()** nemmeno sulla stessa istanza XML. Questo limita la scalabilità del metodo. **OpenXml** consente di accedere a un formato di tabella edge per i dati XML quando non è specificata la clausola WITH e di utilizzare il valore XML rimanente in una colonna di "overflow" separata.  
  
 La combinazione di funzioni **nodes()** e **value()** consente di usare gli indici XML in modo efficiente. e, di conseguenza, offre maggiore scalabilità rispetto a **OpenXml**.  
  
## <a name="see-also"></a>Vedere anche  
 [OPENXML &#40;SQL Server&#41;](../../relational-databases/xml/openxml-sql-server.md)  
  
  
