---
description: MSSQLSERVER_2577
title: MSSQLSERVER_2577 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 2577 (Database Engine error)
ms.assetid: f53256a2-2fb0-47fd-9ed9-c45389104145
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 8c5656bbe837c18177206a3c97055a84f33d7c39
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209842"
---
# <a name="mssqlserver_2577"></a>MSSQLSERVER_2577
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|2577|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|DBCC_IAM_CHAIN_SEQUENCE_OUT_OF_ORDER|  
|Testo del messaggio|Numeri di sequenza della catena non ordinati nella catena IAM (Index Allocation Map) per l'ID di oggetto O_ID, ID di indice I_ID, ID di partizione PN_ID, ID di unità di allocazione A_ID (tipo TYPE). La pagina P_ID1, con numero di sequenza SEQUENCE1 punta alla pagina P_ID2, con numero di sequenza SEQUENCE2.|  
  
## <a name="explanation"></a>Spiegazione  
Ogni pagina IAM (Index Allocation Map) ha un numero di sequenza. Il numero di sequenza indica la posizione della pagina IAM all'interno della catena IAM. Di norma i numeri di sequenza vengono aumentati di un'unità per ogni pagina IAM. La pagina IAM *P_ID2* ha un numero di sequenza che non segue questa regola.  
  
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
L'esecuzione dell'istruzione REPAIR ricompila la catena IAM. REPAIR divide innanzitutto la catena IAM esistente in due metà. La prima metà della catena termina con una pagina IAM, *P_ID1*. Il puntatore di pagina successiva della pagina *P_ID1* viene impostato su (0:0). La seconda metà della catena inizia con una pagina IAM, *P_ID2*. Il puntatore di pagina precedente della pagina *P_ID2* viene impostato su (0:0).  
  
REPAIR collega quindi le due metà e rigenera i numeri di sequenza della catena IAM. Le pagine IAM che non possono essere corrette verranno deallocate.  
  
> [!CAUTION]  
> L'operazione di correzione può comportare la perdita di dati.  
  
