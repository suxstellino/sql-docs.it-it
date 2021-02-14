---
title: Funzioni di accesso ai dati | Microsoft Docs
description: 'Informazioni su come usare le funzioni di accesso ai dati XQuery fn: data (), FN: String () e Text ().'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- data-accessor functions [XQuery]
ms.assetid: 31bad04f-7c74-4773-9f83-612704fdd21c
author: rothja
ms.author: jroth
ms.openlocfilehash: 53ce941c88e2ab551ac0787126e2cf981d353819
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351072"
---
# <a name="data-accessor-functions"></a>Funzioni di accesso ai dati
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Negli argomenti di questa sezione vengono descritte le funzioni di accesso ai dati e viene fornito codice di esempio.  
  
## <a name="understanding-fndata-fnstring-and-text"></a>Informazioni su fn:data(), fn:string() e text()  
 XQuery ha una funzione **FN: data ()** per estrarre valori scalari e tipizzati dai nodi, un testo di test del nodo **()** per restituire i nodi di testo e la funzione **FN: String ()** che restituisce il valore stringa di un nodo. L'utilizzo di tali funzioni non è tuttavia intuitivo. Di seguito vengono riportate le linee guida da seguire per il corretto utilizzo di tali funzioni in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. L'istanza XML \<age> 12 \</age> viene utilizzata a scopo illustrativo.  
  
-   Dati XML non tipizzati: l'espressione di percorso /age/text() restituisce il nodo di testo "12". La funzione fn:data(/age) restituisce il valore stringa "12", così come la funzione fn:string(/age).  
  
-   XML tipizzato: l'espressione/age/text () restituisce un errore statico per qualsiasi elemento tipizzato semplice \<age> . La funzione fn:data(/age) restituisce invece il valore intero 12, mentre fn:string(/age) restituisce la stringa "12".  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Funzione String &#40;XQuery&#41;](../xquery/data-accessor-functions-string-xquery.md)  
  
-   [Funzione data &#40;XQuery&#41;](../xquery/data-accessor-functions-data-xquery.md)  
  
## <a name="see-also"></a>Vedere anche  
 [Espressioni di percorso &#40;XQuery&#41;](../xquery/path-expressions-xquery.md)  
  
  
