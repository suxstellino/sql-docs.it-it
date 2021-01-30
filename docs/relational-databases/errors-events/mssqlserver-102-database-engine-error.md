---
description: MSSQLSERVER_102
title: MSSQLSERVER_102 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 102 (Database Engine error)
ms.assetid: 264dc1a2-c8a0-4c89-b5f6-951baf950299
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f8ec00fdf017b76574a1d5a9bd9e906bd283c0f8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197506"
---
# <a name="mssqlserver_102"></a>MSSQLSERVER_102
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|102|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|P_SYNTAXERR2|  
|Testo del messaggio|Sintassi non corretta in prossimità di '%.*ls'.|  
  
## <a name="explanation"></a>Spiegazione  
Indica un errore di sintassi. Le informazioni aggiuntive non sono disponibili perché l'errore non consente l'elaborazione dell'istruzione da parte di [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
Questa situazione può essere causata dal tentativo di creare una chiave simmetrica utilizzando la crittografia deprecata RC4 o RC4_128 quando la modalità di compatibilità non è 90 o 100.  
  
## <a name="user-action"></a>Azione dell'utente  
Ricercare errori di sintassi nell'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
Se si crea una chiave simmetrica utilizzando RC4 o RC4_128, selezionare un metodo di crittografia più recente, ad esempio uno degli algoritmi AES (opzione consigliata). Se è necessario utilizzare RC4, eseguire ALTER DATABASE SET COMPATIBILITY_LEVEL per impostare il database sul livello di compatibilità 90 o 100. (Non consigliato.)  
  
