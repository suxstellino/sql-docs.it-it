---
title: Memorizzazione nella cache di modelli, XSL e schemi (SQLXML)
description: Visualizzare informazioni sulla memorizzazione nella cache di modelli, XSL e schemi in SQLXML 4.0.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXML, caching
- cache [SQLXML]
- memory [SQLXML]
ms.assetid: 80b4fa79-243f-442c-9f22-74ad66186501
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 40aab0063614248ba3089d262302d5f5ab5df799
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491974"
---
# <a name="caching-templates-xsl-and-schemas-sqlxml-40"></a>Memorizzazione nella cache di modelli, file XSL e schemi (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Per migliorare le prestazioni,  [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML 4.0 supporta la memorizzazione di modelli, file XSL e schemi nella cache.  
  
 Tutti gli schemi, i modelli e i file XSL (ad eccezione dei file di https:// o ftp:// posizione) vengono memorizzati nella cache. I file memorizzati nella cache restano in memoria mentre il processo è in esecuzione. Al termine del processo, il contenuto della cache va perso. Di conseguenza, se si esegue un processo per query, i vantaggi della memorizzazione nella cache potrebbero essere irrilevanti.  
  
 Negli argomenti di questa sezione vengono fornite ulteriori informazioni sulla memorizzazione nella cache.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
 [Memorizzazione nella cache &#40;modello con SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/caching-templates-xml-schemas/template-caching-sqlxml-4-0.md)  
 Viene illustrata e quindi fornita una chiave del Registro di sistema per la memorizzazione nella cache dei modelli.  
  
 [Memorizzazione nella cache XSL &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/caching-templates-xml-schemas/xsl-caching-sqlxml-4-0.md)  
 Viene illustrata e quindi fornita una chiave del Registro di sistema per la memorizzazione nella cache dei file XSL.  
  
 [Memorizzazione nella cache &#40;schema in SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/caching-templates-xml-schemas/schema-caching-sqlxml-4-0.md)  
 Vengono illustrati i problemi dell'installazione affiancata di SQLXML relativi alla memorizzazione nella cache degli schemi e vengono fornite chiavi del Registro di sistema per la memorizzazione nella cache degli schemi.  
  
  
