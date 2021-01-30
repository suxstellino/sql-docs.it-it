---
description: MSSQLSERVER_7932
title: MSSQLSERVER_7932 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 7932 (Database Engine error)
ms.assetid: e2ad218a-3249-4f18-8b32-09f0030765a5
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f60466ed9f3f76ad174abfc58585db2f0c7bc270
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198004"
---
# <a name="mssqlserver_7932"></a>MSSQLSERVER_7932
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|7932|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|DBCC2_FS_ROWSET_IN_WRONG_FILEGROUP|  
|Testo del messaggio|Errore di tabella: l'ID di directory FileStream F_ID per l'ID di oggetto O_ID, ID di indice I_ID, ID di partizione PN_ID è nel filegroup FG_ID1, mentre dovrebbe essere nel filegroup FG_ID2.|  
  
## <a name="explanation"></a>Spiegazione  
Durante DBCC CHECKDB, l'archiviazione FILESTREAM per l'oggetto specificato è stata rilevata nel filegroup non corretto. Ciò potrebbe indicare un danneggiamento dei metadati dell'oggetto.  
  
## <a name="user-action"></a>Azione dell'utente  
  
### <a name="look-for-hardware-failure"></a>Individuare errori hardware  
Eseguire gli strumenti di diagnostica hardware e risolvere eventuali problemi. Esaminare inoltre il registro di sistema di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows e il registro delle applicazioni, nonché il log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per stabilire se l'errore è dovuto a un problema hardware. Risolvere tutti i problemi relativi all'hardware indicati nei log.  
  
In caso di problemi persistenti che provocano il danneggiamento dei dati, provare a sostituire i vari componenti hardware per isolare il problema. Verificare che nel sistema non sia abilitata la memorizzazione nella cache in scrittura sul controller del disco. Se si ritiene che il problema sia dovuto alla memorizzazione nella cache in scrittura, rivolgersi al fornitore dell'hardware.  
  
Infine, potrebbe essere conveniente passare a un nuovo sistema hardware. A tale scopo può essere necessario riformattare le unità disco e reinstallare il sistema operativo.  
  
### <a name="restore-from-backup"></a>Eseguire un ripristino da backup  
Se il problema non è correlato all'hardware ed è disponibile un backup valido noto, ripristinare il database dal backup.  
  
### <a name="run-dbcc-checkdb"></a>Eseguire DBCC CHECKDB  
Non applicabile. L'errore non può essere corretto automaticamente. Se non è possibile ripristinare il database da un backup, contattare il Servizio Supporto Tecnico Clienti [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
