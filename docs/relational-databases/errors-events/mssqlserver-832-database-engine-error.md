---
description: MSSQLSERVER_832
title: MSSQLSERVER_832
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 832 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: bba8712825dc3ef14617ad4e8d8f7312b479e936
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180291"
---
# <a name="mssqlserver_832"></a>MSSQLSERVER_832
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|Valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|832|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|B_CONSTPAGECHANGED|
|Testo del messaggio|Una pagina che dovrebbe essere costante è stata modificata (checksum previsto: \<expected value>, checksum effettivo: \<actual value>, database \<dbid>, file \'<filename>', pagina \<pageno>). Questa situazione indica in genere un errore di memoria o altri problemi a livello di hardware o sistema operativo.|
||

## <a name="explanation"></a>Spiegazione

Un fattore esterno ha causato la modifica di una pagina di database al di fuori del normale codice del motore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usato per modificare le pagine del database.  Le condizioni potrebbero essere:  

- Un thread in esecuzione nel processo di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che scrive erroneamente in una pagina di database, spesso noto come "scribbler".
- Un problema relativo all'hardware o al sistema operativo a causa del quale la memoria a supporto della pagina del database viene modificata erroneamente o danneggiata.  

Quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rileva questo comportamento viene generato l'errore 832.

## <a name="user-action"></a>Azione utente

Per individuare la causa dell'errore, prendere in considerazione le opzioni seguenti:

- È consigliabile eseguire i normali controlli hardware o di sistema per determinare se esiste un problema correlato a memoria, CPU o un altro problema correlato all'hardware. Verificare che siano stati applicati al sistema tutti i driver di sistema, gli aggiornamenti del sistema operativo e gli aggiornamenti dell'hardware. Prendere in considerazione l'esecuzione di qualsiasi diagnostica del produttore dell'hardware, inclusi i test correlati alla memoria.
- Valutare quali DLL "esterne" caricate in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potrebbero causare questo problema, incluse le stored procedure estese, gli oggetti COM o altre DLL che potrebbero modificare erroneamente la memoria di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] riservata per le pagine di database.  

Ogni volta che viene visualizzato questo errore, è consigliabile eseguire immediatamente `DBCC CHECKDB` sul database a cui fa riferimento il \<dbid> nel messaggio di errore.

## <a name="more-information"></a>Ulteriori informazioni

Questo errore viene rilevato dall'attività in background spesso denominata LazyWriter. (Il "comando" per questa attività viene considerato LAZY WRITER.) Questo errore non viene pertanto restituito a un'applicazione client. L'errore verrà scritto nel log eventi dell'applicazione di Windows come EventID=832.  

Vengono controllate solo le pagine non attualmente modificate nella cache (o "dirty"). È per questo motivo che il messaggio usa il termine "costante", perché la pagina non è mai stata modificata dopo essere stata letta dal disco. Inoltre, la pagina è stata letta "pulita" dal disco perché contiene un valore di checksum nella pagina e non ha rilevato un errore di checksum (messaggio 824). Tuttavia, la pagina potrebbe essere modificata a un certo punto dopo l'errore e quindi scritta sul disco con la modifica non corretta. Se si verifica questo errore, viene calcolato un nuovo checksum in base a tutte le modifiche prima della scrittura su disco. Pertanto, la pagina potrebbe essere danneggiata sul disco, ma una lettura successiva dal disco potrebbe non generare un errore di checksum. È importante eseguire `DBCC CHECKDB` su tutti i database a cui fa riferimento questo errore.  

È possibile che anche `DBCC CHECKDB` non segnali un errore per una pagina in questo stato dopo la scrittura su disco. Il motivo è che la modifica non corretta potrebbe trovarsi in posizioni della pagina che non contengono dati, né contengono informazioni importanti sulla struttura della pagina o delle righe oppure potrebbe trattarsi di modifiche ai dati che CHECKDB non è in grado di rilevare.  

Per altri dettagli e informazioni sul messaggio 832, vedere anche il white paper [Concetti di base di I/O di SQL Server, capitolo 2](/previous-versions/sql/sql-server-2005/administrator/cc917726(v=technet.10)).