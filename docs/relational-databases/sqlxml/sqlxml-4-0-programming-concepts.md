---
title: Concetti relativi alla programmazione SQLXML 4.0
description: Visualizzare informazioni sui concetti di programmazione utilizzati in SQLXML 4.0.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXML, about SQLXML
- SQLXML
ms.assetid: 5a11cda2-b8a3-4453-848f-641afdaa7024
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f9f2b05f8d85a28660632ec724d355bebb7797e0
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490348"
---
# <a name="sqlxml-40-programming-concepts"></a>Concetti relativi alla programmazione SQLXML 4.0
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  SQLXML 3.0 è stato offerto come versione Web per offrire ulteriori miglioramenti e funzionalità XML sul lato client alle caratteristiche esistenti, ad esempio schemi XSD con annotazioni, caricamento bulk XML, supporto per servizi Web (SOAP) e updategram.  
  
 In [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] è stato introdotto SQLXML 4.0, che continua a fornire le stesse funzionalità di SQLXML 3.0 con l'aggiunta tuttavia di nuovi aggiornamenti forniti per supportare le nuove caratteristiche introdotte con [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
 In questa sezione vengono fornite informazioni su SQLXML 4.0.  
  
 [Installazione di SQLXML non inclusa in SQL Server](../../relational-databases/sqlxml/sqlxml-is-not-installed-in-sql-server.md)  
 Viene descritta la procedura per l'installazione di SQLXML 4.0.  
  
 [Novità di SQLXML 4.0 SP1](../../relational-databases/sqlxml/what-s-new-in-sqlxml-4-0-sp1.md)  
 Vengono descritti gli aggiornamenti e i miglioramenti apportati in SQLXML 4.0 e vengono forniti collegamenti ad argomenti correlati inclusi in questa documentazione.  
  
 [Utilizzo di ADO per eseguire query SQLXML 4.0](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)  
 Viene descritto come utilizzare ADO per le query SQLXML. In SQLXML 4.0 ADO ha un ruolo più significativo rispetto alle versioni precedenti.  
  
 [Supporto del tipo di dati xml in SQLXML 4.0](../../relational-databases/sqlxml/xml-data-type-support-in-sqlxml-4-0.md)  
 Viene descritto il supporto per il tipo di dati xml aggiunto per SQLXML 4.0.  
  
 [Requisiti per l'esecuzione di esempi di SQLXML](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md)  
 Vengono descritti i requisiti per la creazione di esempi reali dagli esempi SQLXML forniti.  
  
 [Formattazione lato client e lato server &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml/formatting/client-side-and-server-side-formatting-sqlxml-4-0.md)  
 Viene confrontata la formattazione sul lato client con quella sul lato server e vengono fornite informazioni correlate, incluse informazioni sul comando FOR XML per la costruzione di documenti XML.  
  
 [Schemi XSD con annotazioni in SQLXML 4.0](../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xsd-schemas-in-sqlxml-4-0.md)  
 Vengono fornite informazioni sull'utilizzo degli schemi XSD con annotazioni in SQLXML 4.0 e sugli schemi XDR con annotazioni da utilizzare nelle applicazioni legacy.  
  
 [Utilizzo di query XPath in SQLXML 4.0](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/using-xpath-queries-in-sqlxml-4-0.md)  
 Viene descritto come utilizzare un subset del linguaggio XPath per eseguire query sulle viste XML create da uno schema XSD con annotazioni, con l'ausilio di alcuni esempi.  
  
 [Utilizzo di updategram per modificare dati in SQLXML 4.0](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/using-updategrams-to-modify-data-in-sqlxml-4-0.md)  
 Vengono fornite informazioni sugli updategram che consentono di modificare i dati in un database agendo sulle viste XML fornite dagli schemi XSD o XDR con annotazioni.  
  
 [Esecuzione del caricamento bulk di dati XML &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/performing-bulk-load-of-xml-data-sqlxml-4-0.md)  
 Viene descritto come eseguire il caricamento bulk dei dati XML in SQLXML 4.0.  
  
 [Componenti per l'accesso ai dati SQLXML 4.0](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/sqlxml-4-0-data-access-components-sqlxmloledb-provider.md)  
 Viene descritto il provider SQLXMLOLEDB e vengono forniti collegamenti ad altri componenti per l'accesso ai dati SQLXML 4.0.  
  
 [Supporto XQLXML 4.0 per Microsoft .NET Framework](../sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/sqlxml-4-0-net-framework-support-managed-classes.md)  
 Viene descritto il supporto SQLXML 4.0 per .NET Framework.  
  
 [Memorizzazione nella cache di modelli, XSL e schemi &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/caching-templates-xml-schemas/caching-templates-xsl-and-schemas-sqlxml-4-0.md)  
 Viene descritta la funzionalità di memorizzazione nella cache disponibile in SQLXML per il miglioramento delle prestazioni.  
  
 [Considerazioni sulla sicurezza per SQLXML 4.0](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/sqlxml-4-0-security-considerations.md)  
 Vengono descritti i problemi di sicurezza correlati all'uso di SQLXML 4.0.  
  
 [Linee guida e limitazioni di SQLXML 4.0](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/guidelines-and-limitations-of-sqlxml-4-0.md)  
 Vengono elencati i problemi da ricordare quando si utilizza SQLXML 4.0.  
  
## <a name="see-also"></a>Vedere anche  
 [Dati XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)  
  
