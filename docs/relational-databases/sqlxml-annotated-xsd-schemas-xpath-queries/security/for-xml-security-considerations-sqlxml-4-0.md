---
title: Considerazioni sulla sicurezza di FOR XML (SQLXML)
description: Informazioni su importanti considerazioni sulla sicurezza quando si specifica la modalità FOR XML AUTO in SQLXML 4.0.
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- NESTED mode
- client-side XML formatting
- FOR XML clause, security
- server-side XML formatting
- AUTO mode
- security [SQLXML], FOR XML
ms.assetid: facba279-df93-475b-ad43-0043dc5bae03
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 129d634f3b717a219c7cd8af8028d1264ae1b9f4
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491621"
---
# <a name="for-xml-security-considerations-sqlxml-40"></a>Considerazioni sulla sicurezza per FOR XML (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  La modalità FOR XML AUTO genera una gerarchia XML in cui viene eseguito il mapping dei nomi di elemento ai nomi di tabella e dei nomi di attributo ai nomi di colonna. In questo modo vengono esposte le informazioni sulle colonne e sulle tabelle di database. È possibile nascondere le informazioni del database quando si utilizza la modalità AUTO (formattazione sul lato server) specificando gli alias di colonne e di tabelle nella query. Questi alias vengono restituiti nel documento XML risultante come nomi di elemento e di attributo.  
  
 Nella query seguente ad esempio viene specificata la modalità AUTO; pertanto, la formattazione XML viene eseguita nel server:  
  
```  
SELECT C.FirstName as F,C.LastName as L   
FROM Person.Contact C   
FOR XML AUTO  
```  
  
 Nel documento XML risultante vengono utilizzati gli alias per i nomi di elemento e di attributo:  
  
```  
<?xml version="1.0" encoding="utf-8" ?>   
<root>  
  <C F="Nancy" L="Fuller" />   
  <CE F="Andrew" L="Peacock" />   
  <C F="Janet" L="Leverling" />   
  ...  
</root>  
```  
  
 Quando si utilizza la modalità NESTED (formattazione sul lato client), gli alias vengono restituiti solo per gli attributi nel documento XML risultante. I nomi delle tabelle di base vengono restituiti sempre come nomi di elemento. Ad esempio, nella query seguente viene specificata la modalità NESTED.  
  
```  
SELECT C.FirstName as F,C.LastName as L   
FROM Person.Contact C   
FOR XML AUTO  
```  
  
 Nel documento XML risultante i nomi delle tabelle di base vengono restituiti come nomi di elemento e gli alias di tabella non vengono utilizzati:  
  
```  
<?xml version="1.0" encoding="utf-8" ?>   
<root>  
  <Person.Contact F="Nancy" L="Fuller" />   
  <Person.Contact F="Andrew" L="Peacock" />   
  <Person.Contact F="Janet" L="Leverling" />   
       ...  
</root>  
```  
  
  
