---
title: Utilizzo del provider SQLXMLOLEDB (SQLXML)
description: Visualizzare informazioni sull'utilizzo delle proprietà specifiche del provider SQLXMLOLEDB nelle applicazioni ADO.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- sample applications [SQLXML]
- SQLXMLOLEDB Provider, samples
- ClientSideXML property
ms.assetid: fbcefac5-29c9-478b-b0e0-d510b593f446
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bc1f4382bc105aee93c22e17fe9e97772717ac84
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490570"
---
# <a name="using-the-sqlxmloledb-provider-sqlxml-40"></a>Utilizzo del provider SQLXMLOLEDB (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Negli argomenti di questa sezione vengono fornite applicazioni ADO di esempio che illustrano l'utilizzo delle proprietà specifiche del provider SQLXMLOLEDB.  
  
## <a name="application-requirements-for-sqlxmloledb-40-provider"></a>Requisiti dell'applicazione per il provider SQLXMLOLEDB 4.0  
 Per creare esempi reali che utilizzano SQLXMLOLEDB 4.0, è necessario effettuare le operazioni seguenti:  
  
1.  Creare un'applicazione Microsoft Visual Basic con estensione exe e aggiungere uno dei riferimenti seguenti:  
  
    -   Libreria Microsoft ActiveX Data Objects 2.6  
  
    -   Libreria Microsoft ActiveX Data Objects 2.7  
  
    -   Libreria Microsoft ActiveX Data Objects 2.8  
  
2.  Distribuire e installare SQLXML 4.0 e [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client.  

     Per altre informazioni, vedere Concetti relativi alla [programmazione SQLXML 4.0](../../../relational-databases/sqlxml/sqlxml-4-0-programming-concepts.md) e [Installazione SQL Server Native Client](../../../relational-databases/native-client/applications/installing-sql-server-native-client.md).  
  
## <a name="in-this-section"></a>Contenuto della sezione  
 [Esecuzione di query SQL &#40;provider SQLXMLOLEDB&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-sql-queries-sqlxmloledb-provider.md)  
 Viene illustrato l'utilizzo delle proprietà radice ClientSideXML e xml per eseguire query SQL.  
  
 [Esecuzione di modelli che contengono query SQL &#40;provider SQLXMLOLEDB&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-templates-that-contain-sql-queries-sqlxmloledb-provider.md)  
 Viene illustrato l'utilizzo della proprietà ClientSideXML.  
  
 [Esecuzione di query XPath &#40;provider SQLXMLOLEDB&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-xpath-queries-sqlxmloledb-provider.md)  
 Viene illustrato l'utilizzo delle proprietà ClientSideXML, Base Path e Mapping Schema.  
  
 [Esecuzione di query XPath con spazi dei nomi &#40;provider SQLXMLOLEDB&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-xpath-queries-with-namespaces-sqlxmloledb-provider.md)  
 Viene illustrato come eseguire una query sugli schemi qualificati con lo spazio dei nomi.  
  
 [Esecuzione di modelli che contengono query XPath &#40;provider SQLXMLOLEDB&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-templates-that-contain-xpath-queries-sqlxmloledb-provider.md)  
 Viene illustrato come eseguire modelli con query SQL usando le proprietà ClientSideXML, Base Path e Mapping Schema.  
  
 [Applicazione di una trasformazione XSL &#40;provider SQLXMLOLEDB&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/applying-an-xsl-transformation-sqlxmloledb-provider.md)  
 Viene illustrato l'utilizzo delle proprietà ClientSideXML e xsl nell'applicazione di una trasformazione XSL.  
  
## <a name="see-also"></a>Vedere anche  
 [Requisiti di sistema per SQL Server Native Client](../../../relational-databases/native-client/system-requirements-for-sql-server-native-client.md)  
  
  
