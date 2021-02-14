---
title: Impostare le regole di confronto dei database definiti dall'utente in modo che corrispondano a quelle dei database master e model
description: Informazioni su come abilitare un criterio per controllare se le regole di confronto dei database definiti dall'utente e dei database di sistema sono le stesse.
ms.custom: seo-lt-2019
ms.date: 08/31/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Best Practices [Database Engine]
author: dzsquared
ms.author: drskwier
ms.openlocfilehash: 7c196d81609a9bb223caa0dd2ffa055a4e9b2bce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272342"
---
# <a name="set-the-collation-of-user-defined-databases-to-match-master-and-model-databases"></a>Impostare le regole di confronto dei database definiti dall'utente in modo che corrispondano a quelle dei database master e model
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Questa regola consente di controllare se i database definiti dall'utente vengono configurati utilizzando le stesse regole di confronto di quelle per i database master e modello.
  
## <a name="best-practices-recommendations"></a>Raccomandazioni per le procedure consigliate  
 È consigliabile fare in modo che le regole di confronto dei database definiti dall'utente corrispondano a quelle dei database master e modello. In caso contrario, possono verificarsi conflitti relativi alle regole di confronto che potrebbero impedire l'esecuzione del codice. Ad esempio, quando una stored procedure crea un join tra una tabella e una tabella temporanea, SQL Server potrebbe terminare il batch e restituire un errore di conflitto tra regole di confronto se le regole di confronto del database definito dall'utente differiscono da quelle del database model. Questo problema si verifica perché le tabelle temporanee vengono create in tempdb, che basa le proprie regole di confronto su quelle del modello.

  In caso di errori di conflitto tra regole di confronto, considerare una delle soluzioni seguenti:

  - Esportare i dati dal database utente e importarli nelle nuove tabelle che utilizzano le stesse regole di confronto dei database master e modello.

  - Ricompilare i database di sistema in modo che vengano utilizzate regole di confronto corrispondenti a quelle del database utente. Per altre informazioni su come ricompilare i database di sistema, vedere [Ricompilare i database di sistema](../databases/rebuild-system-databases.md).

  - Modificare qualsiasi stored procedure che crea join tra tabelle utente e tabelle in tempdb per creare le tabelle in tempdb utilizzando le regole di confronto del database utente. A tale scopo, aggiungere la clausola COLLATE database_default alle definizioni di colonna della tabella temporanea, come illustrato nell'esempio seguente:
  
    ```
    CREATE TABLE #temp1 ( c1 int, c2 varchar(30) COLLATE database_default )
    ```

## <a name="see-also"></a>Vedere anche
  
 [Impostazione o modifica di regole di confronto del server](../collations/set-or-change-the-server-collation.md)  

 [Impostare o modificare le regole di confronto del database](../collations/set-or-change-the-database-collation.md)

 [Impostare o modificare le regole di confronto delle colonne](../collations/set-or-change-the-column-collation.md)
 
 [Visualizzazione di informazioni sulle regole di confronto](../collations/view-collation-information.md)    
  
  
