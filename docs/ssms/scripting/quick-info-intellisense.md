---
title: Informazioni rapide (IntelliSense)
description: Informazioni su come usare la funzionalità Informazioni rapide di IntelliSense per visualizzare la dichiarazione completa di qualsiasi identificatore nel codice. In SQL Server Management Studio l'opzione è disponibile nell'editor del motore di database e nell'editor di query XML.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Quick Info option [IntelliSense]
- declarations [IntelliSense]
- IntelliSense [SQL Server], Quick Info
- identifier declarations [IntelliSense]
ms.assetid: 3c8b59f4-1922-4bde-844f-5f2306514d96
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ff2473620822f915deab34f67339cec6e4d0ea49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270842"
---
# <a name="quick-info-intellisense"></a>Informazioni rapide (IntelliSense)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  La funzionalità [!INCLUDE[msCoName](../../includes/msconame-md.md)] Informazioni rapide **di** IntelliSense visualizza la dichiarazione completa di qualsiasi identificatore nel codice. Quando si sposta il puntatore del mouse su un identificatore, viene visualizzata la relativa dichiarazione in una finestra popup gialla. In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], **Informazioni rapide** è disponibile nell'editor di query del motore di database e XML.  
  
## <a name="transact-sql-quick-info"></a>Informazioni rapide Transact-SQL  
 **Informazioni rapide** contiene due tipi di informazioni nell'editor di query [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Se non si trova in modalità debug, **Informazioni rapide** visualizza la dichiarazione dell'espressione. In modalità debug, **Informazioni rapide** visualizza invece il nome dell'espressione e il valore corrente.  
  
 Nell'editor di query del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , la funzionalità **Informazioni rapide** è disponibile solo per quelle parti di sintassi [!INCLUDE[tsql](../../includes/tsql-md.md)] che sono supportate da IntelliSense. Ad esempio, se si sposta il puntatore del mouse sull'identificatore di un oggetto che dispone di un tipo di dati non supportato da IntelliSense, la finestra popup **Informazioni rapide** conterrà un messaggio indicante che il tipo di dati non è supportato.  
  
## <a name="see-also"></a>Vedere anche  
 [Sintassi Transact-SQL supportata da IntelliSense](./transact-sql-syntax-supported-by-intellisense.md)  
  
