---
title: Impostare l'attributo sql:inverse in sql:relationship (SQLXML)
description: Informazioni su come usare l'attributo sql:inverse nell'elemento sql:relationship per specificare relazioni tra le colonne del database in un'operazione updategram.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- sql:relationship
- bulk load [SQLXML]
- inverse attribute
- relationships [SQLXML], inverse orders
- relationship annotation
- XSD schemas [SQLXML], relationships
- annotated XSD schemas, relationships
- updategrams [SQLXML], relationships
- sql:inverse
ms.assetid: 08904cbd-9c86-493d-90c3-f5e1d13ce59d
author: rothja
ms.author: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bcc10bf5d886679bf5048776154a2548c3bce2d2
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490701"
---
# <a name="specifying-the-sqlinverse-attribute-on-sqlrelationship-sqlxml-40"></a>Specifica dell'attributo sql:inverse in sql:relationship (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  **L'attributo sql:inverse** è utile solo quando lo schema XSD viene usato per il caricamento bulk o per un updategram. **L'attributo sql:inverse** può essere specificato nell'elemento **\<sql:relationship>** . Negli updategram la relativa logica interpreta lo schema nella determinazione delle tabelle e delle colonne aggiornate dall'operazione dell'updategram. Le relazioni padre-figlio specificate nello schema determinano l'ordine di modifica (inserimento o eliminazione) dei record.  
  
 Nel caso di uno schema XSD in cui la relazione padre-figlio viene specificata nell'ordine inverso a quello della relazione chiave primaria/chiave esterna tra le colonne del database corrispondenti, l'operazione di inserimento o eliminazione dell'updategram non riuscirà a causa della violazione della relazione chiave primaria/chiave esterna. In questi casi, l'attributo **sql:inverse** viene specificato (**sql:inverse="true"**) nell'elemento e la logica dell'updategram inversa l'interpretazione della relazione padre-figlio specificata nello **\<sql:relationship>** schema.  
  
 **L'attributo sql:inverse** accetta un valore booleano (0=false, 1=true). I valori possibili sono 0, 1, true e false.  
  
 Per un esempio funzionante che usa **l'annotazione sql:inverse,** vedere Specifica di uno schema di mapping con [annotazioni in un updategram.](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Specifica di relazioni tramite sql:relationship &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-relationships-using-sql-relationship-sqlxml-4-0.md)  
  
  
