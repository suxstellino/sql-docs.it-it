---
description: Restituzione di SQL_NO_DATA
title: Restituzione di SQL_NO_DATA | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQL_NO_DATA [ODBC]
- backward compatibility [ODBC], SQL_NO_DATA
- compatibility [ODBC], SQL_NO_DATA
ms.assetid: deed0163-9d1a-4e9b-9342-3f82e64477d2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f61d676897bf7b5633a0c9d37c12e44ae9de1cbe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187162"
---
# <a name="returning-sql_no_data"></a>Restituzione di SQL_NO_DATA
Quando un'applicazione *ODBC 2. x* con un driver ODBC *3. x* chiama **SQLExecDirect**, **SQLExecute** o **SQLParamData** e un'istruzione Update o DELETE ricercata è stata eseguita ma non ha interessato alcuna riga nell'origine dati, il driver ODBC *3. x* deve restituire SQL_SUCCESS. Quando un'applicazione *ODBC 3. x* che utilizza un driver ODBC *3. x* chiama **SQLExecDirect**, **SQLExecute** o **SQLParamData** con lo stesso risultato, il driver ODBC *3. x* deve restituire SQL_NO_DATA.  
  
 Se un'istruzione Update o DELETE cercata in un batch di istruzioni non influisce su alcuna riga nell'origine dati, **SQLMoreResults** restituisce SQL_SUCCESS. Non può restituire SQL_NO_DATA, perché ciò significa che non sono presenti altri risultati, non che è stato generato un risultato da un aggiornamento o da un'eliminazione ricercata che non ha interessato alcuna riga.
