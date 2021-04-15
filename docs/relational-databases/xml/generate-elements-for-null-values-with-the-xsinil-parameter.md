---
title: Generare elementi per valori NULL tramite XSINIL | Microsoft Docs
description: Informazioni su come generare elementi XML per i valori NULL usando il parametro XSINIL nella direttiva ELEMENTS.
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- FOR XML clause, null values
- null values [SQL Server], XML
- ELEMENTS directive
- XSINIL parameter
ms.assetid: 2dbc4e48-1cae-4d83-b371-3265da9687cc
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 812fc808942562f0852c00948951581abc44e250
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490823"
---
# <a name="generate-elements-for-null-values-with-the-xsinil-parameter"></a>Generazione di elementi per valori NULL tramite il parametro XSINIL

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

La direttiva **ELEMENTS** costruisce codice XML nel quale viene eseguito il mapping di ogni valore di colonna a un elemento. Per impostazione predefinita, se il valore di colonna è NULL, non viene aggiunto alcun elemento. Ma specificando il parametro facoltativo **XSINIL** nella direttiva ELEMENTS, è possibile richiedere che venga creato un elemento anche per il valore NULL. In questo caso, per ogni valore di colonna NULL viene restituito un elemento con l'attributo **xsi:nil** impostato su TRUE.  
  
## <a name="see-also"></a>Vedere anche

[Usare la modalità RAW con FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md)

[Clausola SELECT - FOR](../../t-sql/queries/select-for-clause-transact-sql.md)
