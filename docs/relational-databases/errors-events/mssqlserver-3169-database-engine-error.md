---
description: MSSQLSERVER_3169
title: MSSQLSERVER_3169 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
f1_keywords:
- "3169"
helpviewer_keywords:
- 3169 (Database Engine error)
ms.assetid: 7d4dbed6-bb94-4908-bc03-2040a9cf63bc
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 176ca9ec55225ea49dbcf6c7fdf5de6c5a2f2192
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194200"
---
# <a name="mssqlserver_3169"></a>MSSQLSERVER_3169
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|3169|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|ND|  
|Testo del messaggio|Il backup del database è stato eseguito in un server che esegue la versione %ls. Tale versione è incompatibile con il server, che esegue la versione %ls. Ripristinare il database in un server che supporta il backup oppure utilizzare un backup compatibile con questo server.|  
  
## <a name="explanation"></a>Spiegazione  
Alcune caratteristiche di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] influiscono sulla struttura dei file di database. Quando si ripristina un database in un'altra istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è possibile che il formato di file non sia compatibile con una versione diversa di [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
Ad esempio, questo errore può verificarsi quando si usa il formato vardecimalstorage in una versione più recente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e successivamente si tenta di ripristinare i file di database in una versione precedente a [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2.  
  
## <a name="user-action"></a>Azione dell'utente  
Verificare quale versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è in esecuzione nel server di origine. In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] fare clic con il pulsante destro del mouse sul server e quindi scegliere **Proprietà** oppure digitare **SELECT \@\@VERSION** in una finestra Query. Aprire il database utilizzando la versione originale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Verificare quali caratteristiche sono attivate nel database originale nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Modificare tali impostazioni in modo che funzionino con la versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui il database verrà ripristinato.  
  
