---
title: Creare indici XML | Microsoft Docs
description: Informazioni su come creare indici XML primari e secondari in SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- indexes [XML in SQL Server]
- XML indexes [SQL Server], creating
ms.assetid: 6ecac598-355d-4408-baf7-1b2e8d4cf7c1
author: rothja
ms.author: jroth
ms.openlocfilehash: 7ef33369b40d0e1530240dfade0c9f3aa0b63ed7
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490941"
---
# <a name="create-xml-indexes"></a>Creazione di indici XML
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Questo argomento descrive come creare, modificare e utilizzare indici XML primari e secondari.  
  
## <a name="creating-a-primary-xml-index"></a>Creazione di un indice XML primario  
 Per creare un indice XML primario, usare l'istruzione DDL [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)]. Negli indici XML non sono supportate tutte le opzioni disponibili per gli indici non XML.  
  
 Per la creazione di un indice XML, si noti quanto segue:  
  
-   Per creare un indice XML primario, è necessario che la tabella contenente la colonna XML da indicizzare, denominata tabella di base, includa un indice cluster nella chiave primaria. In tal modo si garantisce che, nel caso in cui la tabella di base sia partizionata, sia possibile partizionare l'indice XML primario tramite lo stesso schema e la stessa funzione di partizionamento.  
  
-   Se è presente un indice XML, non è possibile modificare la chiave primaria cluster della tabella. Prima di modificarla, sarà necessario eliminare tutti gli indici XML dalla tabella.  
  
-   È possibile creare un indice XML primario in una singola colonna di tipo **xml** . Non è possibile creare nessun altro tipo di indice se la colonna di tipo XML è una colonna chiave. È comunque possibile includere la colonna di tipo L **xml** in un indice non XML. Ogni colonna di tipo **xml** in una tabella può includere l'indice XML primario corrispondente. Tuttavia, è consentito un solo indice XML primario per ogni colonna di tipo **xml** .  
  
-   Gli indici XML si trovano nello stesso spazio dei nomi degli indici non XML. Pertanto, nella stessa tabella non possono esistere un indice XML e un indice non XML con lo stesso nome.  
  
-   Per gli indici XML, le opzioni IGNORE_DUP_KEY e ONLINE sono sempre impostate su OFF. È possibile specificare queste opzioni con il valore OFF.  
  
-   Le informazioni sul filegroup o sul partizionamento della tabella utente vengono applicate all'indice XML. Gli utenti non possono specificare tali informazioni separatamente in un indice XML.  
  
-   L'opzione per gli indici DROP_EXISTING consente di eliminare un indice XML primario e di crearne uno nuovo oppure di eliminare un indice XML secondario e di crearne uno nuovo. Tale opzione, tuttavia, non consente di eliminare un indice XML secondario per creare un nuovo indice XML primario o viceversa.  
  
-   Per i nomi degli indici XML primari vengono applicate le stesse restrizioni valide per i nomi delle viste.  
  
 Non è possibile creare un indice XML in una colonna di tipo **xml** in una vista, in una variabile con valori di **tabella** con colonne di tipo **xml** o in variabili di tipo **xml** .  
  
-   Per modificare una colonna di tipo **xml** da XML non tipizzato a XML tipizzato o viceversa, usando l'opzione ALTER TABLE ALTER COLUMN, nella colonna non deve esistere alcun indice XML. In caso contrario, è necessario eliminarlo prima di provare a modificare il tipo della colonna.  
  
-   Dopo la creazione di un indice XML, è necessario impostare l'opzione ARITHABORT su ON. Per eseguire operazioni di query, inserimento, eliminazione o aggiornamento sui valori della colonna XML utilizzando i metodi con tipo di dati XML, è necessario impostare la stessa opzione nella connessione. In caso contrario, i metodi con tipo di dati XML avranno esito negativo.  
  
    > [!NOTE]  
    >  Le informazioni su un indice XML sono disponibili nelle viste del catalogo. Tuttavia, **sp_helpindex** non è supportato. In questo argomento sono disponibili esempi che illustrano l'esecuzione di query sulle viste del catalogo per trovare informazioni sugli indici XML.  
  
 Quando si crea o si ricrea un indice XML primario in una colonna con tipo di dati XML che contiene valori dei tipi XML Schema **xs:date** o **xs:dateTime** o dei relativi sottotipi, non meno recenti di un anno, la creazione dell'indice non riuscirà in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive. [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] sono consentiti questi valori, quindi questo problema può verificarsi quando si creano indici in un database generato in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Per altre informazioni, vedere [Confrontare dati XML tipizzati con dati XML non tipizzati](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md).  
  
### <a name="example-creating-a-primary-xml-index"></a>Esempio: Creazione di un indice XML primario  
 Nella maggior parte degli esempi viene utilizzata la tabella T (pk INT PRIMARY KEY, xCol XML), che include una colonna XML non tipizzata. Gli esempi possono essere estesi senza difficoltà ai dati XML tipizzati. Per semplicità vengono descritte le query per le istanze di dati XML, come illustrato nel codice seguente:  
  
```  
<book genre="security" publicationdate="2002" ISBN="0-7356-1588-2">  
   <title>Writing Secure Code</title>  
   <author>  
      <first-name>Michael</first-name>  
      <last-name>Howard</last-name>  
   </author>  
   <author>  
      <first-name>David</first-name>  
      <last-name>LeBlanc</last-name>  
   </author>  
   <price>39.99</price>  
</book>  
```  
  
 L'istruzione seguente crea un indice XML chiamato idx_xCol sulla colonna XML chiamata xCol della tabella T:  
  
```  
CREATE PRIMARY XML INDEX idx_xCol on T (xCol)  
```  
  
## <a name="creating-a-secondary-xml-index"></a>Creazione di un indice XML secondario  
 Usare l'istruzione DDL [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] per creare indici XML secondari e specificare il tipo di indice XML secondario desiderato.  
  
 Per la creazione di indici XML secondari, si noti quanto segue:  
  
-   Per gli indici XML secondari sono consentite tutte le opzioni applicate a un indice non cluster, ad eccezione di IGNORE_DUP_KEY e di ONLINE. Per gli indici XML secondari, le due opzioni devono essere impostate sempre su OFF.  
  
-   Gli indici secondari vengono partizionati esattamente come l'indice XML primario.  
  
-   L'opzione DROP_EXISTING consente di eliminare un indice secondario dalla tabella utente e di crearne un altro.  
  
 È possibile eseguire una query nella vista del catalogo **sys.xml_indexes** per recuperare informazioni sull'indice XML. La colonna **seconday_type_desc** nella vista del catalogo **sys.xml_indexes** include il tipo di indice secondario:  
  
```  
SELECT  *   
FROM    sys.xml_indexes;  
```  
  
 I valori restituiti nella colonna **seconday_type_desc** possono essere NULL, PATH, VALUE o PROPERTY. Per l'indice XML primario, il valore restituito è NULL.  
  
### <a name="example-creating-secondary-xml-indexes"></a>Esempio: Creazione di indici XML secondari  
 Nell'esempio seguente viene illustrata la creazione di indici XML secondari. Vengono inoltre fornite informazioni sugli indici XML creati.  
  
```  
CREATE TABLE T (Col1 INT PRIMARY KEY, XmlCol XML);  
GO  
-- Create primary index.  
CREATE PRIMARY XML INDEX PIdx_T_XmlCol   
ON T(XmlCol);  
GO  
-- Create secondary indexes (PATH, VALUE, PROPERTY).  
CREATE XML INDEX PIdx_T_XmlCol_PATH ON T(XmlCol)  
USING XML INDEX PIdx_T_XmlCol  
FOR PATH;  
GO  
CREATE XML INDEX PIdx_T_XmlCol_VALUE ON T(XmlCol)  
USING XML INDEX PIdx_T_XmlCol  
FOR VALUE;  
GO  
CREATE XML INDEX PIdx_T_XmlCol_PROPERTY ON T(XmlCol)  
USING XML INDEX PIdx_T_XmlCol  
FOR PROPERTY;  
GO  
```  
  
 È possibile eseguire query in `sys.xml_indexes` per recuperare informazioni sugli indici XML. La colonna `secondary_type_desc` contiene il tipo di indice secondario.  
  
```  
SELECT  *   
FROM    sys.xml_indexes;  
```  
  
 È inoltre possibile eseguire una query nella vista del catalogo per ottenere informazioni sugli indici.  
  
```  
SELECT *  
FROM sys.xml_indexes  
WHERE object_id = object_id('T');  
```  
  
 È possibile aggiungere dati di esempio e quindi rivedere le informazioni sugli indici XML.  
  
```  
INSERT INTO T VALUES (1,  
'<doc id="123">  
<sections>  
<section num="2">  
<heading>Background</heading>  
</section>  
<section num="3">  
<heading>Sort</heading>  
</section>  
<section num="4">  
<heading>Search</heading>  
</section>  
</sections>  
</doc>');  
GO  
-- Check XML index information.  
SELECT *  
FROM   sys.dm_db_index_physical_stats (db_id(), object_id('T'), NULL, NULL, 'DETAILED');  
GO  
-- Space usage of primary XML index  
DECLARE @index_id int;  
SELECT  @index_id = i.index_id  
FROM    sys.xml_indexes i   
WHERE   i.name = 'PIdx_T_XmlCol' and object_name(i.object_id) = 'T';  
  
SELECT *  
FROM sys.dm_db_index_physical_stats (db_id(), object_id('T') , @index_id, DEFAULT, 'DETAILED');  
go  
--- Space usage of secondary XML index (for example PATH secondary index)  PIdx_T_XmlCol_PATH  
DECLARE @index_id int;  
SELECT  @index_id = i.index_id   
FROM    sys.xml_indexes i   
WHERE  i.name = 'PIdx_T_XmlCol_PATH' and object_name(i.object_id) = 'T';  
  
SELECT *  
FROM sys.dm_db_index_physical_stats (db_id(), object_id('T') , @index_id, DEFAULT, 'DETAILED');  
go  
  
-- Space usage of all secondary XML indexes for a particular table  
SELECT i.name, object_name(i.object_id), stats.*   
FROM   sys.dm_db_index_physical_stats (db_id(), object_id('T'), NULL, DEFAULT, 'DETAILED') stats  
JOIN sys.xml_indexes i ON (stats.object_id = i.object_id and stats.index_id = i.index_id)  
WHERE secondary_type is not null;  
-- Drop secondary indexes.  
DROP INDEX PIdx_T_XmlCol_PATH ON T;  
GO  
DROP INDEX PIdx_T_XmlCol_VALUE ON T;  
GO  
DROP INDEX PIdx_T_XmlCol_PROPERTY ON T;  
GO  
-- Drop primary index.  
DROP INDEX PIdx_T_XmlCol ON T;  
-- Drop table T.  
DROP TABLE T;  
Go  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Indici XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md)   
 [Dati XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)  
  
  
