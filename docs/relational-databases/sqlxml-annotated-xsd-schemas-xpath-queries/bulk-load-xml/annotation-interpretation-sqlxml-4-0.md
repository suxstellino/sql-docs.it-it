---
title: Interpretazione delle annotazioni (SQLXML)
description: Informazioni su come il caricamento bulk XML in SQLXML 4.0 interpreta le annotazioni negli schemi XSD e XDR.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- annotated XSD schemas, XML Bulk Load
- XML Bulk Load [SQLXML], annotation intepretations
- annotations [SQLXML]
- bulk load [SQLXML], annotation interpretations
- annotated XDR schemas, XML Bulk Load
ms.assetid: 1c46bdb6-2812-4a13-b60b-7101c04b299f
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 172f50d93f99ff95e91a3a4889cc5f1e9b7ec1cb
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490660"
---
# <a name="annotation-interpretation-sqlxml-40"></a>Interpretazione delle annotazioni (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Negli argomenti riportati in questa sezione viene descritto in che modo il caricamento bulk XML interpreta le annotazioni nello schema XSD. Il comportamento descritto è applicabile anche alle annotazioni nello schema XDR.  
  
> [!NOTE]  
>  Nelle informazioni contenute in questi argomenti vengono descritte solo le annotazioni utilizzate durante l'elaborazione del caricamento bulk XML. Per un elenco completo delle annotazioni per lo schema XSD supportate da SQLXML 4.0, vedere Uso delle annotazioni negli schemi [XSD &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-using/using-annotations-in-xsd-schemas-sqlxml-4-0.md). Per un elenco delle annotazioni supportate per gli schemi XDR, vedere [Annotated XDR Schemas &#40;Deprecated in SQLXML 4.0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md).  
  
## <a name="in-this-section"></a>Contenuto della sezione  
 [sql:relationship e la regola di ordinamento delle chiavi &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/annotation-interpretation-sql-relationship-and-key-ordering-rule.md)  
 Descrive come viene **interpretata l'annotazione sql:relationship** nel caricamento bulk XML.  
  
 [sql:mapped &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/annotation-interpretation-sql-mapped.md)  
 Descrive come viene **interpretata l'annotazione sql:mapped** nel caricamento bulk XML.  
  
 [sql:limit-field e sql:limit-value &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/annotation-interpretation-sql-limit-field-and-sql-limit-value.md)  
 Descrive come vengono **interpretate le annotazioni sql:limit-field** e **sql:limit-value** nel caricamento bulk XML.  
  
 [sql:overflow-field &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/annotation-interpretation-sql-overflow-field.md)  
 Descrive come viene **interpretata l'annotazione sql:overflow** nel caricamento bulk XML.  
  
 [Altre annotazioni &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/annotation-interpretation-other-annotations.md)  
 Descrive come vengono interpretate le annotazioni seguenti nel caricamento bulk XML: **sql:id-prefix**, **sql:use-cdata**, **sql:url-encode**, **sql:is-mapping-schema**, **sql:key-fields**.  
  
  
