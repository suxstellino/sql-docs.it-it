---
description: MSSQLSERVER_7916
title: MSSQLSERVER_7916 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 7916 (Database Engine error)
ms.assetid: 9bac3536-de14-4e98-84c2-bde9a59ba0d1
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 5a0a89019ae85f8d591518ec8a0f5e08cacc0ecc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198037"
---
# <a name="mssqlserver_7916"></a>MSSQLSERVER_7916
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|7916|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|DBCC2_REPAIR_RECORD|  
|Testo del messaggio|Correzione: record eliminato per l'oggetto con ID O_ID, indice con ID I_ID, partizione con ID PN_ID, unità allocazione con ID A_ID (tipo TYPE), a pagina P_ID, slot S_ID. Gli indici verranno ricompilati.|  
  
## <a name="explanation"></a>Spiegazione  
Messaggio informativo inviato da REPAIR che indica che il record specificato è stato eliminato dalla pagina.  
  
## <a name="user-action"></a>Azione dell'utente  
nessuno  
  
