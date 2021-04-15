---
title: Eliminare indici XML | Microsoft Docs
description: Informazioni su come usare l'istruzione Transact-SQL DROP INDEX per eliminare indici XML e non XML primari o secondari esistenti.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- removing indexes
- dropping indexes
- XML indexes [SQL Server], dropping
ms.assetid: 7591ebea-34af-4925-8553-b2adb5b487c2
author: rothja
ms.author: jroth
ms.openlocfilehash: 1b26c8d06a2a51f9822039124b7a4bf18c7f4a2c
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488697"
---
# <a name="drop-xml-indexes"></a>Eliminazione di indici XML
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Per eliminare indici XML e non XML primari o secondari esistenti, è possibile usare l'istruzione [DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)]. Tuttavia, nessuna opzione dell'istruzione DROP INDEX si applica agli indici XML. Se si elimina l'indice XML primario, vengono eliminati anche gli indici secondari presenti.  
  
 La sintassi di DROP con *TableName.IndexName* è obsoleta e non è supportata per gli indici XML.  
  
## <a name="example-creating-and-dropping-a-primary-xml-index"></a>Esempio: Creazione ed eliminazione di un indice XML primario  
 Nell'esempio seguente viene creato un indice XML in una colonna di tipo **xml** .  
  
```  
DROP TABLE T  
GO  
CREATE TABLE T (Col1 INT PRIMARY KEY, XmlCol XML)  
GO  
-- Create Primary XML index   
CREATE PRIMARY XML INDEX PIdx_T_XmlCol   
ON T(XmlCol)  
GO  
-- Verify the index creation.   
-- Note index type is 3 for xml indexes.  
-- Note the type 3 is index on XML type.  
SELECT *  
FROM sys.xml_indexes  
WHERE object_id = object_id('T')  
AND name='PIdx_T_XmlCol'   
-- Drop the index.  
DROP INDEX PIdx_T_XmlCol ON T  
```  
  
 Quando si elimina una tabella, vengono eliminati automaticamente anche tutti i relativi indici XML. Non è tuttavia possibile eliminare una colonna XML da una tabella se in tale colonna è incluso un indice XML.  
  
 Nell'esempio seguente viene creato un indice XML in una colonna di tipo **xml** . Per altre informazioni, vedere [Confrontare dati XML tipizzati con dati XML non tipizzati](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md).  
  
```  
CREATE TABLE TestTable(  
 Col1 int primary key,   
 Col2 xml (Production.ProductDescriptionSchemaCollection))   
GO  
```  
  
 A questo punto, è possibile creare un indice XML primario in `Co12`.  
  
```  
CREATE PRIMARY XML INDEX PIdx_TestTable_Col2   
ON TestTable(Col2)  
GO  
```  
  
## <a name="example-creating-an-xml-index-by-using-the-drop_existing-index-option"></a>Esempio: Creazione di un indice XML tramite l'opzione DROP_EXISTING  
 Nell'esempio seguente un indice XML viene creato in una colonna (`XmlColx`). Successivamente, viene creato un altro indice XML con lo stesso nome in una colonna diversa, (`XmlColy`). Poiché viene specificata l'opzione `DROP_EXISTING` , viene eliminato l'indice XML esistente in (`XmlColx)` ) e creato un nuovo indice XML in (`XmlColy`).  
  
```  
DROP TABLE T  
GO  
CREATE TABLE T(Col1 int primary key, XmlColx xml, XmlColy xml)  
GO  
-- Create XML index on XmlColx.  
CREATE PRIMARY XML INDEX PIdx_T_XmlCol   
ON T(XmlColx)  
GO  
-- Create same name XML index on XmlColy.  
CREATE PRIMARY XML INDEX PIdx_T_XmlCol   
ON T(XmlColy)   
WITH (DROP_EXISTING = ON)  
-- Verify the index is created on XmlColy.d.  
SELECT sc.name   
FROM   sys.xml_indexes si inner join sys.index_columns sic   
ON     sic.object_id=si.object_id and sic.index_id=si.index_id  
INNER  join sys.columns sc on sc.object_id=sic.object_id   
AND    sc.column_id=sic.column_id  
WHERE  si.name='PIdx_T_XmlCol'   
AND    si.object_id=object_id('T')  
```  
  
 Questa query restituisce il nome della colonna in cui viene creato l'indice XML specificato.  
  
## <a name="see-also"></a>Vedere anche  
 [Indici XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md)  
  
  
