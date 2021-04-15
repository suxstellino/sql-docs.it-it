---
title: classi gestite SQLXML
description: Informazioni sulle classi gestite microsoft SQLXML che espongono la funzionalità di SQLXML 4.0 all'interno di Microsoft .NET Framework.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- .NET Framework [SQLXML], Managed Classes
- SQL Server .NET Data Provider
- Managed Classes [SQLXML], about managed classes
- providers [SQLXML], SQL Server .NET Data Provider
- data providers [SQLXML], SQL Server .NET Data Provider
- Managed Classes [SQLXML]
- XML [SQLXML]
- SQLXML Managed Classes
- providers [SQLXML], SQLXML Managed Classes
- data providers [SQLXML], SQLXML Managed Classes
- SQLXML, Managed Classes
ms.assetid: 73a5faeb-dabf-4895-acb5-a9651b646065
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2754c46171d1cb4a9c1772ec650ca128c7e14c72
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491747"
---
# <a name="sqlxml-40-net-framework-support---managed-classes"></a>Supporto SQLXML 4.0 per .NET Framework - Classi gestite
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML 4.0 supporta caratteristiche che consentono di scrivere applicazioni per accedere a dati XML da un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], trasferire i dati nell'ambiente [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework, elaborare i dati e inviare gli aggiornamenti a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. 
  
  Le classi gestite [!INCLUDE[msCoName](../../../includes/msconame-md.md)] espongono la funzionalità di SQLXML 4.0 in [!INCLUDE[msCoName](../../../includes/msconame-md.md)].NET Framework. Le classi gestite SQLXML consentono di scrivere un'applicazione C# per accedere ai dati XML da un'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], portare i dati nell'ambiente .NET Framework, elaborare i dati e inviare nuovamente gli aggiornamenti a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] come DiffGram per applicarli. Quando si applicano aggiornamenti a un database di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilizzando le classi gestite SQLXML, è necessario utilizzare uno schema di mapping. Per un esempio funzionante, vedere [Accesso alla funzionalità SQLXML nell'ambiente .NET.](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/accessing-sqlxml-functionality-in-the-net-environment.md)  
  
 Per utilizzare le classi gestite SQLXML con SQLXML 4.0, è necessario installare Microsoft Visual Studio.  
  
> [!NOTE]  
>  .NET Framework include il provider di dati [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .NET. Tale provider può essere utilizzato per accedere a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dall'ambiente .NET. È tuttavia in grado di gestire solo query SQL tradizionali (ovvero query del database relazionale ad eccezione delle query FOR XML). Non è possibile eseguire modelli XML o query XPath sul lato server in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  

 Per informazioni sull'accesso e la modifica dei dati in all'interno del .NET Framework e sull'uso di DiffGram per aggiornare i dati nelle tabelle, vedere Accesso alla funzionalità [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [SQLXML nell'ambiente .NET.](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/accessing-sqlxml-functionality-in-the-net-environment.md)  
  
> [!NOTE]  
>  È anche possibile scrivere applicazioni [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Studio per eseguire il caricamento bulk di documenti XML utilizzando il caricamento bulk XML. Per altre informazioni, vedere Esecuzione del caricamento bulk di dati [XML &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/performing-bulk-load-of-xml-data-sqlxml-4-0.md). È necessario aggiungere all'applicazione un riferimento alla DLL del caricamento bulk XML (Xblkld4.dll). Si tratta di una DLL COM per la quale in Visual Studio .NET viene creata automaticamente la libreria di wrapper.  
  
  In questa sezione vengono fornite applicazioni di esempio che illustrano come utilizzare le [!INCLUDE[msCoName](../../../includes/msconame-md.md)] classi gestite SQLXML:  
 [Esecuzione di query SQL &#40;classi gestite SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-sqlxml-managed-classes.md)  
  [Esecuzione di query SQL tramite il metodo ExecuteXMLReader](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-by-using-the-executexmlreader-method.md)  
  [Elaborazione di XML sul lato client &#40;classi gestite SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/processing-xml-on-the-client-side-sqlxml-managed-classes.md)  
  [Esecuzione di query XPath &#40;classi gestite SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-xpath-queries-sqlxml-managed-classes.md)  
  [Esecuzione di query XPath con spazi dei nomi &#40;classi gestite SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-xpath-queries-with-namespaces-sqlxml-managed-classes.md)  
  [Esecuzione di file modello mediante la proprietà CommandText](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-template-files-by-using-the-commandtext-property.md)  
  [Esecuzione di file modello tramite la proprietà CommandStream](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-template-files-by-using-the-commandstream-property.md)  
  [Applicazione di una trasformazione XSL &#40;classi gestite SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/applying-an-xsl-transformation-sqlxml-managed-classes.md)  
  

  
  
