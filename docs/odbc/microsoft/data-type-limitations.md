---
description: Limitazioni dei tipi di dati
title: Limitazioni dei tipi di dati | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC desktop database drivers [ODBC], data types
- data types [ODBC], desktop database drivers
- desktop database drivers [ODBC], data types
ms.assetid: 81c4eab7-1f6b-47a0-b940-89d6c6a14dae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9081f52e7cf79613b9021ce7b883a1720da468c6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176532"
---
# <a name="data-type-limitations"></a>Limitazioni dei tipi di dati
I driver di database di Microsoft ODBC Desktop impongono le seguenti limitazioni sui tipi di dati:  
  
|Tipo di dati|Descrizione|  
|---------------|-----------------|  
|Tutti i tipi di dati|Gli errori di conversione dei tipi possono comportare l'impostazione della colonna interessata su NULL.|  
|BINARY|La creazione di una colonna BINARIa di lunghezza zero restituisce effettivamente una colonna BINARIa a 255 byte.|  
|DATE|Il tipo di dati DATE non può essere convertito in un altro tipo di dati (o in se stesso) tramite la funzione CONVERT.|  
|DECIMAL (valore numerico esatto)|Non supportata.|  
|Tipi di dati Floating-Point|Il numero di posizioni decimali in un numero a virgola mobile può essere limitato dal formato numerico impostato nella sezione internazionale del pannello di controllo di Windows.|  
|NUMERIC|Supporta la precisione massima e una scala di 28.|  
|timestamp|Il tipo di dati TIMESTAMP non può essere convertito in se stesso dalla funzione CONVERT.|  
|TINYINT|I valori TINYINT sono sempre senza segno.|  
|Stringhe di Zero-Length|Quando si usa dBASE, Microsoft Excel, Paradox o Textdriver, l'inserimento di una stringa di lunghezza zero in una colonna inserisce effettivamente un valore NULL.|
