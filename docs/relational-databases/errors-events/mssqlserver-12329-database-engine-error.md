---
description: MSSQLSERVER_12329
title: MSSQLSERVER_12329 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 12329 (Database Engine error)
ms.assetid: 43f90287-36d5-46c2-ac91-a37202dcf6d3
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 73c58c5c97f71ed355f476bd5395197d5f0b0af1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197060"
---
# <a name="mssqlserver_12329"></a>MSSQLSERVER_12329
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|12329|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|HK_UNSUPPORTED_NON_LATIN_CODEPAGE|  
|Testo del messaggio|Tipi di dati char(n) e varchar(n) che usano regole di confronto con una tabella codici diversa da 1252 non supportati con *construct*.|  
  
## <a name="explanation"></a>Spiegazione  
Non utilizzare i tipi di dati char(n) e varchar(n) in cui vengono utilizzate le regole di confronto con una tabella codici diversa da 1252.  
  
## <a name="user-action"></a>Azione dell'utente  
Una situazione imprevista in grado di generare questo errore è:  
  
```  
BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = 'us_english')  
```  
  
Da utilizzare in alternativa:  
  
```  
BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')  
```  
  
