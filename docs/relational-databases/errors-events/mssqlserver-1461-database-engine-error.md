---
description: MSSQLSERVER_1461
title: MSSQLSERVER_1461 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1461 (Database Engine error)
ms.assetid: fce10907-4753-441b-b624-f28e00ed7520
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 2dea7c8af8be207fd164563596d2084b87747acd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196943"
---
# <a name="mssqlserver_1461"></a>MSSQLSERVER_1461
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|1461|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|DBM_SAFETY_MISMATCH|  
|Testo del messaggio|Rilevati livelli di protezione del mirroring del database diversi tra i server per il database "%.*ls". Verrà utilizzato il livello di protezione FULL.|  
  
## <a name="explanation"></a>Spiegazione  
Le connessioni per il mirroring sono state interrotte quando il livello di protezione delle transazioni è stato modificato a causa dell'inconsistenza delle impostazioni di protezione delle transazioni nei database principale e mirror. Verrà utilizzata l'impostazione predefinita del livello di protezione FULL delle transazioni. La sessione verrà eseguita in modalità a protezione elevata.  
  
## <a name="user-action"></a>Azione dell'utente  
Per disattivare la protezione delle transazioni, eseguire nuovamente l'istruzione ALTER DATABASE *nome_database* SET PARTNER SAFETY OFF nel database principale.  
  
