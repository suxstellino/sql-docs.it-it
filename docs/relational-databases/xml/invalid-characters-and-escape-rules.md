---
title: Caratteri non validi e regole di escape | Microsoft Docs
description: Informazioni sul modo in cui i caratteri XML non validi vengono gestiti dalla clausola FOR XML e sulle regole di escape per i caratteri non validi nei nomi XML.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- FOR XML clause, invalid characters
- FOR XML clause, escape rules
ms.assetid: f2e9b997-f400-4963-b225-59d46c6b93e8
author: rothja
ms.author: jroth
ms.openlocfilehash: c5ff0ca695498b940953ff74e85edd92a4f88dcd
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490793"
---
# <a name="invalid-characters-and-escape-rules"></a>Caratteri non validi e regole di escape
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Questo argomento descrive il modo in cui i caratteri XML non validi vengono gestiti dalla clausola FOR XML ed elenca le regole di escape per i caratteri non validi nei nomi XML.  
  
## <a name="for-xml-and-invalid-characters"></a>Per i caratteri XML non validi  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sostituisce caratteri XML non validi con entità quando vengono restituiti all'interno di query FOR XML che non usano la direttiva TYPE.  
  
 Nei parser conformi a XML 1.0 vengono generati errori di analisi indipendentemente dal fatto che tali caratteri vengano sostituiti o meno con entità, tuttavia la forma con entità è maggiormente conforme a XML 1.1 ed è potenzialmente conforme alle versioni future dello standard XML. Semplifica inoltre il debug perché il punto di codice del carattere non valido diventa visibile.  
  
 Per gli utenti degli strumenti XML non è necessaria alcuna soluzione alternativa, perché il parser XML genererà in ogni caso un errore in corrispondenza dei caratteri non validi nel flusso di dati. Se si utilizzano strumenti non XML potrebbe essere necessario aggiornare la logica di programmazione in modo che sia in grado di cercare i caratteri come valori sostituiti con entità.  
  
 I caratteri spazi vuoti indicati di seguito vengono sostituiti da entità diverse nelle query FOR XML per mantenerne la presenza in sequenze di andata e ritorno:  
  
-   Nel contenuto e negli attributi di un elemento: **hex(0D)** (ritorno a capo)  
  
-   Nel contenuto di un attributo: **hex(09)** (tabulazione), **hex(0A)** (avanzamento di riga)  
  
 Questi caratteri vengono mantenuti nell'output e non verranno normalizzati da un parser.  
  
## <a name="escape-rules"></a>Regole di escape  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nomi che includono caratteri non validi per i nomi XML, quali gli spazi, vengono convertiti in nomi XML in modo tale che i caratteri non validi vengano convertiti in codici numerici in sequenza escape.  
  
 In un nome XML possono essere presenti solo due caratteri non alfabetici, ovvero i due punti (:) e il carattere di sottolineatura (_). Poiché il carattere di due punti è già riservato per gli spazi dei nomi, come carattere di escape viene utilizzato il carattere di sottolineatura. Di seguito vengono illustrate le regole di escape utilizzate per la codifica:  
  
-   Per i caratteri UCS-2 non validi per i nomi XML, in base alla specifica XML 1.0, vengono usati i caratteri di escape _xHHHH\_. dove HHHH rappresenta il codice UCS-2 esadecimale a quattro cifre per il carattere in base all'ordine del primo bit più significativo. Ad esempio, il nome della tabella **Order Details** viene convertito in Order_x0020_Details.  
  
-   I caratteri che non rientrano nelle specifiche UCS-2 (le aggiunte UCS-4 dell'intervallo da U+00010000 a U+0010FFFF) vengono convertiti in _xHHHHHHHH_,\_. dove HHHHHHHH rappresenta la codifica UCS-4 esadecimale a otto cifre del carattere, se è attiva la compatibilità con le versioni precedenti con SQL Server 2000. In caso contrario, i caratteri vengono convertiti in _xHHHHHH\_per la compatibilità con lo standard ISO.  
  
-   Per il carattere di sottolineatura è necessario utilizzare caratteri di escape solo se è seguito dal carattere x. Ad esempio, il nome della tabella **Order_Details** non viene codificato.  
  
-   Per il carattere dei due punti negli identificatori non vengono utilizzati caratteri di escape, in modo tale che l'elemento dello spazio dei nomi e i nomi degli attributi possano essere generati dalla query FOR XML. Ad esempio, la query seguente genera un attributo dello spazio dei nomi che include i due punti nel nome.  
  
    ```  
    SELECT 'namespace-urn' as 'xmlns:namespace',   
     1 as 'namespace:a'   
    FOR XML RAW  
    ```  
  
     Il risultato della query è il seguente:  
  
    ```  
    <row xmlns:namespace="namespace-urn" namespace:a="1"/>  
    ```  
  
     Si noti che per aggiungere spazi dei nomi XML è consigliabile utilizzare WITH XMLNAMESPACES.  
  
## <a name="see-also"></a>Vedere anche  
 [FOR XML &#40;SQL Server&#41;](../../relational-databases/xml/for-xml-sql-server.md)  
  
  
