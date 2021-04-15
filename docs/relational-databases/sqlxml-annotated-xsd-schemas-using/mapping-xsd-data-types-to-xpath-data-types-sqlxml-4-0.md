---
title: Mapping dei tipi di dati XSD ai tipi di dati XPath (SQLXML)
description: Informazioni su come eseguire il mapping dei tipi di dati XSD ai tipi di dati XPath quando si esegue una query XPath in SQLXML 4.0.
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- mapping data types [SQLXML]
- data types [SQLXML], converting
- annotated XSD schemas, mapping data types
- XPath queries [SQLXML], mapping data types
- converting data types [SQLXML]
- data types [SQLXML], mapping data types
- XPath data types [SQLXML]
- XSD schemas [SQLXML], mapping data types
ms.assetid: ced1a95e-18d4-4a5a-8da8-dbb6d58bbd45
author: rothja
ms.author: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 69a3cc082e67cdf164d8b651ec0e09fa9a04681b
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492024"
---
# <a name="mapping-xsd-data-types-to-xpath-data-types-sqlxml-40"></a>Mapping dei tipi di dati XSD ai tipi di dati XPath (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Quando viene eseguita una query XPath su uno schema XSD e il tipo XSD viene specificato nell'attributo **xsd:type,** XPath usa il tipo di dati specificato durante l'elaborazione della query.  
  
 Il tipo di dati XPath di un nodo viene derivato dal tipo di dati XSD nello schema, come illustrato nella tabella seguente. A scopo illustrativo, viene utilizzato il nodo EmployeeID.  
  
|Tipo di dati XSD|Tipo di dati XDR|Equivalente<br /><br /> Tipo di dati XPath|SQL Server<br /><br /> utilizzata|  
|-------------------|-------------------|------------------------------------|--------------------------------------------|  
|**Base64Binary**<br /><br /> **HexBinary**|**Nessuno**<br /><br /> **bin.base64bin.hex**|**Non applicabile**|nessuno<br /><br /> EmployeeID|  
|**Boolean**|**boolean**|**boolean**|CONVERT(bit, EmployeeID)|  
|**Decimal, integer, float, byte, short, int, long, float, double, unsignedByte, unsignedShort, unsignedInt, unsignedLong**|**number, int, float, i1, i2, i4, i8,r4, r8ui1, ui2, ui4, ui8**|**number**|CONVERT(float(53), EmployeeID)|  
|**id, idref, idrefsentity, entities, notation, nmtoken, nmtokens, DateTime, string, AnyURI**|**id, idref, idrefsentity, entities, enumeration, notation, nmtoken, nmtokens, char, dateTime, dateTime.tz, string, uri, uuid**|**string**|CONVERT(nvarchar(4000), EmployeeID, 126)|  
|**decimal**|**fixed14.4**|**N/D (in XPath non è disponibile alcun tipo di dati equivalente al tipo di dati XDR fixed14.4).**|CONVERT(money, EmployeeID)|  
|**date**|**date**|**string**|LEFT(CONVERT(nvarchar(4000), EmployeeID, 126), 10)|  
|**time**|**time**<br /><br /> **time.tz**|**string**|SUBSTRING(CONVERT(nvarchar(4000), EmployeeID, 126), 1 + CHARINDEX(N'T', CONVERT(nvarchar(4000), EmployeeID, 126)), 24)|  
  
  
