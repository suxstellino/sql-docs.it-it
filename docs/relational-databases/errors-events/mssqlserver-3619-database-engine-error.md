---
description: MSSQLSERVER_3619
title: MSSQLSERVER_3619 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3619 (Database Engine error)
ms.assetid: 7d94f8d9-65ca-4fde-9c17-7b83e94a3779
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a5f805abd590470884f86bed70358be73447563c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185213"
---
# <a name="mssqlserver_3619"></a>MSSQLSERVER_3619
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|3619|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|SYS_NOLOG|  
|Testo del messaggio|Impossibile scrivere un record di checkpoint nel database con ID %d perché nel log non è disponibile spazio. Rivolgersi all'amministratore di sistema per troncare il log o per allocare più spazio per i file dei log del database.|  
  
## <a name="explanation"></a>Spiegazione  
Lo spazio nel log delle transazioni è esaurito.  
  
## <a name="user-action"></a>Azione dell'utente  
Troncare il log per liberare spazio oppure allocare ulteriore spazio per il log. Per ulteriori informazioni, vedere "Risoluzione dei problemi relativi a un log delle transazioni pieno (Errore 9002)" nella documentazione online di SQL Server.  
  
