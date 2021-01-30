---
description: MSSQLSERVER_5228
title: MSSQLSERVER_5228 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 5228 (Database Engine error)
ms.assetid: 5e83c617-4aa2-4755-bcc5-a798c46b97e4
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4f84c27204183ff85303007a0dee6d641c985e6b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185524"
---
# <a name="mssqlserver_5228"></a>MSSQLSERVER_5228
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|5228|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|DBCC4_ANTIMATTER_COLUMN_DETECTED|  
|Testo del messaggio|Errore di tabella: ID oggetto O_ID, ID indice I_ID, ID partizione PN_ID, ID unità di allocazione A_ID (tipo TYPE), pagina PG_ID, riga R_ID. DBCC ha rilevato una pulizia incompleta causata da un'operazione di compilazione di un indice online. Valore della colonna di elenco degli elementi da eliminare VALUE.|  
  
## <a name="explanation"></a>Spiegazione  
È stata rilevata una build di un indice online incompleta per l'oggetto *O_ID*, indice *I_ID* e partizione *PN_ID*. Tale evento è dimostrato dalla presenza di una colonna di elenco degli elementi da eliminare nella riga *R_ID*. Questa colonna viene utilizzata durante la riconciliazione di record di più origini per la compilazione di un indice online. Nel messaggio di errore viene inoltre indicato il valore della colonna di elenco degli elementi da eliminare.  
  
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
L'esecuzione di REPAIR comporta la ricompilazione dell'indice specificato e di tutti gli indici dipendenti.  
  
## <a name="see-also"></a>Vedere anche  
[DBCC CHECKDB &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
  
