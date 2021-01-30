---
description: MSSQLSERVER_2576
title: MSSQLSERVER_2576 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 2576 (Database Engine error)
ms.assetid: b727cc2f-c76c-46f8-bbbe-5e7a05a6eabf
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 96f2ab95fcf96dbd4de214f5a39ca96419ff8911
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209850"
---
# <a name="mssqlserver_2576"></a>MSSQLSERVER_2576
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|2576|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|DBCC_IAM_PARENT_PAGE_WAS_NOT_SEEN|  
|Testo del messaggio|Il puntatore di pagina precedente della pagina IAM (Index Allocation Map) P_ID2 nell'oggetto con ID O_ID, ID di indice I_ID, ID di partizione PN_ID e ID di unità di allocazione A_ID (tipo TYPE) punta alla pagina IAM P_ID1, che non è stata rilevata durante l'analisi.|  
  
## <a name="explanation"></a>Spiegazione  
Una pagina IAM o una voce di metadati non è stata individuata, nonostante esista un riferimento alla pagina come collegamento a un'altra pagina IAM precedente in una catena IAM. Se la pagina *P_ID1* è (0:0), la pagina IAM *P_ID2* è l'inizio di una catena IAM e la voce di metadati della catena IAM è mancante.  
  
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
L'istruzione REPAIR tenterà di ricompilare la catena IAM che interessa la pagina *P_ID2*. La ricompilazione della catena può comportare la rimozione di alcune pagine dalla stessa catena o la rimozione dell'intera catena se i metadati sono danneggiati.  
  
> [!CAUTION]  
> L'operazione di correzione può comportare la perdita di dati.  
  
