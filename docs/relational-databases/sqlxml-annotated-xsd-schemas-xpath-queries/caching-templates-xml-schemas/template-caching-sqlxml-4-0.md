---
title: Memorizzazione nella cache dei modelli (SQLXML)
description: Informazioni su come migliorare significativamente le prestazioni quando si eseguono modelli usando la memorizzazione nella cache dei modelli in SQLXML 4.0.
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- registry keys [SQLXML]
- cache [SQLXML]
- templates [SQLXML], caching
ms.assetid: 73e151c6-b24e-4422-a116-51e0846bc6f5
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8330cc3166133fc12ed615f7e4d174b9ad9ef5ad
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491946"
---
# <a name="template-caching-sqlxml-40"></a>Memorizzazione nella cache dei modelli (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  La memorizzazione dei modelli nella cache migliora significativamente le prestazioni. Se è impostata, il modello rimane in memoria fino alla prima esecuzione. In questo modo vengono migliorate le prestazioni per l'esecuzione successiva.  
  
 È possibile impostare le dimensioni della cache dei modelli aggiungendo nel Registro di sistema la chiave seguente:  
  
```  
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSSQLServer\Client\SQLXML4\TemplateCacheSize  
```  
  
> [!CAUTION]  
>  [!INCLUDE[ssNoteRegistry](../../../includes/ssnoteregistry-md.md)]  
  
 È consigliabile impostare le dimensioni del modello in base alla memoria disponibile e al numero di modelli utilizzati. Il valore predefinito **delle dimensioni di TemplateCacheSize** è 31. È possibile aumentare le dimensioni della cache se l'accesso al modello sembra lento o ridurle se la memoria è insufficiente.  
  
 Per prestazioni migliori, è consigliabile impostare **TemplateCacheSize** su un valore superiore al numero di modelli in genere in uso. Se **TemlateCacheSize** è inferiore al numero di modelli disponibili, le prestazioni si riducono con l'aumentare del numero di modelli. **TemplateCacheSize** può essere impostato su un massimo di 128.  
  
 Ogni volta che viene utilizzato un modello memorizzato nella cache, viene controllata l'ora di modifica del file modello per vedere se deve essere aggiornata. Ciò accade in quanto la copia su disco è più recente della copia della cache.  
  
> [!NOTE]  
>  I parametri di modello e le proprietà dei comandi non vengono memorizzati nella cache.  
  
## <a name="see-also"></a>Vedere anche  
 [Memorizzazione nella cache dello schema &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/caching-templates-xml-schemas/schema-caching-sqlxml-4-0.md)   
 [Memorizzazione nella cache XSL &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/caching-templates-xml-schemas/xsl-caching-sqlxml-4-0.md)  
  
  
