---
description: MSSQLSERVER_9692
title: MSSQLSERVER_9692 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 9692 (Database Engine error)
ms.assetid: 02399d6e-ab5e-4f30-8a3e-2bb1e8c135b5
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f086df43d106810e471d8795dab212dcfc0c1dd9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187562"
---
# <a name="mssqlserver_9692"></a>MSSQLSERVER_9692
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|9692|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|SB2_CANT_LISTEN_PORT_IN_USE|  
|Testo del messaggio|Il trasporto di protocollo %S_MSG non può restare in attesa sulla porta %d perché è in uso da un altro processo.|  
  
## <a name="explanation"></a>Spiegazione  
Un altro programma in esecuzione nel computer sta utilizzando la porta TCP indicata.  
  
## <a name="user-action"></a>Azione dell'utente  
Eseguire **netstat -aon** per determinare quale programma sta usando la porta. Disabilitare l'applicazione oppure specificare una porta diversa per Service Broker.  
  
