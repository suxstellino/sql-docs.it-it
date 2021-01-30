---
description: Segnalibri di lunghezza fissa
title: Segnalibri Fixed-Length | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- backward compatibility [ODBC], bookmarks
- bookmarks [ODBC]
- compatibility [ODBC], bookmarks
- fixed-length bookmarks [ODBC]
ms.assetid: cbd8185e-fb03-408f-b80b-1a2e164534fd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2d5eb7acd97a83be0d150cec8b19a5a3e25a7bf6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194805"
---
# <a name="fixed-length-bookmarks"></a>Segnalibri di lunghezza fissa
Se un driver ODBC *3. x* deve funzionare con un'applicazione ODBC *2. x* che utilizza segnalibri a lunghezza fissa, il driver deve supportare quanto segue:  
  
-   SQL_UB_ON come valore per l'opzione dell'istruzione SQL_USE_BOOKMARKS. (SQL_UB_ON è deprecato in ODBC *3. x*).  
  
-   Opzione dell'istruzione SQL_GET_BOOKMARK.
