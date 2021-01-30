---
description: MSSQLSERVER_7995
title: MSSQLSERVER_7995 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 7995 (Database Engine error)
ms.assetid: af6d6322-3cba-43d8-be97-e6ef15f8c933
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f412ba20bd29b0129df6b40403fa1449d9509d64
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197921"
---
# <a name="mssqlserver_7995"></a>MSSQLSERVER_7995
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|7995|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|DBCC2_SYSTEM_CATALOGS_CORRUPT|  
|Testo del messaggio|Database 'NOMEDB': impossibile eseguire ulteriori elaborazioni DBCC NOMECONTROLLO a causa di errori di consistenza nei cataloghi di sistema.|  
  
## <a name="explanation"></a>Spiegazione  
Il processo DBCC CHECKDB è composto dalle tre fasi seguenti:  
  
1.  Controlli di allocazione, che equivalgono all'esecuzione di DBCC CHECKALLOC.  
  
2.  Controlli di consistenza delle tabelle di sistema, che equivalgono all'esecuzione di DBCC CHECKTABLE in un elenco di piccole dimensioni di tabelle di base di sistema necessarie.  
  
3.  Controllo di consistenza dei database completi.  

L'errore MSSQLEngine_7995 viene generato nella fase 2 e indica che DBCC CHECKDB ha rilevato errori che il comando non è in grado di correggere oppure che l'istruzione REPAIR non è stata specificata. DBCC CHECKDB non è in grado di continuare con la fase 3 poiché le tabelle di base di sistema in questione archiviano i metadati di tutti gli oggetti nel database oppure sono danneggiate.  
  
## <a name="user-action"></a>Azione dell'utente  
  
### <a name="look-for-hardware-failure"></a>Individuare errori hardware  
Eseguire gli strumenti di diagnostica hardware e risolvere eventuali problemi. Esaminare inoltre il registro di sistema di [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows e il registro delle applicazioni, nonché il log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per stabilire se l'errore è dovuto a un problema hardware. Risolvere tutti i problemi relativi all'hardware indicati nei log.  
  
In caso di problemi persistenti che provocano il danneggiamento dei dati, provare a sostituire i vari componenti hardware per isolare il problema. Verificare che nel sistema non sia abilitata la memorizzazione nella cache in scrittura sul controller del disco. Se si ritiene che il problema sia dovuto alla memorizzazione nella cache in scrittura, rivolgersi al fornitore dell'hardware.  
  
Infine, potrebbe essere conveniente passare a un nuovo sistema hardware. A tale scopo può essere necessario riformattare le unità disco e reinstallare il sistema operativo.  
  
### <a name="restore-from-backup"></a>Eseguire un ripristino da backup  
Se il problema non è correlato all'hardware ed è disponibile un backup valido noto, ripristinare il database dal backup.  
  
### <a name="run-dbcc-checkdb"></a>Eseguire DBCC CHECKDB  
Se non è disponibile un backup valido, eseguire DBCC CHECKDB senza la clausola REPAIR per determinare l'entità del danno. Verrà automaticamente suggerita la clausola REPAIR da usare. Eseguire quindi DBCC CHECKDB con la clausola REPAIR appropriata per correggere il database.  
  
> [!CAUTION]  
> Se non si è certi dell'effetto prodotto sui dati dall'esecuzione di DBCC CHECKDB con la clausola REPAIR, contattare il personale del supporto tecnico prima di eseguire l'istruzione.  
  
Se l'esecuzione di DBCC CHECKDB con una clausola REPAIR non consente di risolvere il problema, contattare il personale del supporto tecnico.  
  
### <a name="results-of-running-repair-options"></a>Risultati dell'esecuzione delle opzioni REPAIR  
Esaminare l'elenco di errori per vedere quale effetto avrà l'istruzione REPAIR su ognuno di loro.  
  
