---
description: MSSQLSERVER_3417
title: MSSQLSERVER_3417 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3417 (Database Engine error)
ms.assetid: 005829c8-cf57-4828-818a-bbe8ee2e00f0
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: bf4dac403156a5fff0b0b5cdb59a98d4a58abd0d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201611"
---
# <a name="mssqlserver_3417"></a>MSSQLSERVER_3417
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|3417|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|REC_BADMASTER|  
|Testo del messaggio|Impossibile recuperare il database master. Impossibile eseguire SQL Server. Ripristinare il master da un backup completo, correggerlo o ricompilarlo. Per ulteriori informazioni sulla ricompilazione del database master, vedere la documentazione online di SQL Server.|  
  
## <a name="explanation"></a>Spiegazione  
Non è possibile avviare il database **master** in SQL Server. Se non è possibile portare online il database **master** o **tempdb**, non è possibile eseguire SQL Server. Questo errore in genere è preceduto da altri errori. Esaminare i log degli errori per individuare la causa radice.  
  
## <a name="user-action"></a>Azione dell'utente  
Ripristinare il backup del database oppure correggere il database.  
  
