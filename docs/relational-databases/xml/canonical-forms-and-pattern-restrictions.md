---
title: Forme canoniche e restrizioni di pattern | Microsoft Docs
description: Informazioni su come evitare i problemi che si verificano quando la rappresentazione canonica dei tipi di valore primitivi non è conforme alle restrizioni di pattern di un facet basato su pattern XSD.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- pattern restrictions
- canonical forms
ms.assetid: 088314ec-7d0b-4a05-8a33-f35da5bfe59c
author: rothja
ms.author: jroth
ms.openlocfilehash: 1d6ed73c10ba71db4afb438a858075735104d098
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491180"
---
# <a name="canonical-forms-and-pattern-restrictions"></a>Forme canoniche e restrizioni di pattern
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Il facet basato su pattern XSD consente la restrizione dello spazio lessicale di tipi semplici. Quando viene applicata una restrizione di pattern a un tipo per il quale esistono più rappresentazioni lessicali possibili, alcuni valori potrebbero causare un comportamento imprevisto al momento della convalida.  
  
 Tale comportamento si verifica in quanto le rappresentazioni lessicali di questi valori non vengono archiviate nel database. Pertanto, i valori vengono convertiti nelle rappresentazioni canoniche corrispondenti quando serializzati come output. Se un documento contiene un valore la cui forma canonica non è conforme alla restrizione di pattern per il tipo corrispondente, il documento verrà rifiutato nel caso in cui un utente tenti di reinserirlo.  
  
 Per evitare tale problema, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rifiuta qualsiasi documento XML contenente valori che non è possibile reinserire, a causa della violazione delle restrizioni di pattern da parte delle forme canoniche corrispondenti. Ad esempio, il valore "33,000" non verrà convalidato in base a un tipo derivato da **xs:decimal** con una restrizione di pattern "33\\,0+". Sebbene il valore "33.000" sia conforme a tale pattern, la forma canonica "33" non lo è.  
  
 Di conseguenza, è necessario prestare attenzione quando si applicano facet basati su modelli a tipi derivati dai seguenti tipi primitivi: **boolean**, **decimal**, **float**, **double**, **dateTime**, **time**, **date**, **hexBinary** e **base64Binary**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene generato un avviso quando tali componenti vengono aggiunti a una raccolta di schemi.  
  
 Per la serializzazione imprecisa di valori a virgola mobile esiste un problema analogo. A causa dell'algoritmo di serializzazione per i valori a virgola mobile utilizzato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è possibile che valori simili condividano la stessa forma canonica. Quando un valore a virgola mobile viene serializzato e quindi reinserito, potrebbe subire una leggera variazione. In casi rari, il risultato può essere un valore che viola uno dei facet seguenti per il tipo corrispondente al momento del reinserimento: **enumeration**, **minInclusive**, **minExclusive**, **maxInclusive** o **maxExclusive**. Per evitare tale problema, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rifiuta i valori di tipi derivati da `xs:float` o `xs:double` che non è possibile da serializzare e reinserire.  
  
## <a name="see-also"></a>Vedere anche  
 [Requisiti e limitazioni per le raccolte di XML Schema nel server](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  
