---
title: Oggetto SqlXmlAdapter (SQLXML)
description: Informazioni sull'oggetto SqlXmlAdapter che fornisce metodi che facilitano l'interazione con il set di dati nel .NET Framework.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- void Update(DataSet ds) method
- SqlXmlAdapter object
- void Fill(DataSet ds) method
- SQLXML Managed Classes, SqlXmlAdapter object
- Managed Classes [SQLXML], SqlXmlAdapter object
ms.assetid: 0a16eddf-fc26-4d92-82d4-359b5fb905d5
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3c11abafdaa6d9ec2786be013550d9d0f8133d1e
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491714"
---
# <a name="sqlxml-managed-classes---sqlxmladapter-object"></a>Classi gestite SQLXML - Oggetto SqlXmlAdapter
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Questo oggetto fornisce metodi che facilitano l'interazione con il set di dati in [!INCLUDE[msCoName](../../../includes/msconame-md.md)].NET Framework. Per un esempio funzionante, vedere [Accesso alla funzionalità SQLXML nell'ambiente .NET.](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/accessing-sqlxml-functionality-in-the-net-environment.md)  
  
 L'oggetto SqlXmlAdapter supporta questi metodi:  
  
 void Fill(DataSet ds)  
 Inserisce nel set di dati di .NET Framework i dati XML recuperati da [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 void Update(DataSet ds)  
 Applica aggiornamenti ai record in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dai dati del set di dati.  
  
 L'oggetto SqlXmlAdapter supporta questi costruttori:  
  
```  
public SqlXmlAdapter(SqlXmlCommand  cmd)   
  
public SqlXmlAdapter(  
                     string commandText,   
                     SqlXmlCommandType cmdType,   
                     string connectionString  
                      )   
  
public SqlXmlAdapter(  
                     Stream commandStream,   
                     SqlXmlCommandType cmdType,   
                     string connectionString  
                     )   
```  
  
## <a name="see-also"></a>Vedere anche  
 [Oggetto SqlXmlCommand &#40;classi gestite SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/sqlxml-managed-classes-sqlxmlcommand-object.md)   
 [Oggetto SqlXmlParameter &#40;classi gestite SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/sqlxml-managed-classes-sqlxmlparameter-object.md)  
  
  
