---
description: MSSQLSERVER_856
title: MSSQLSERVER_856
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 856 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 3c84818a233fabbe5829339849663c4044f6490e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348703"
---
# <a name="mssqlserver_856"></a>MSSQLSERVER_856
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|Valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|856|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|BAD_MEMORY_CLEAN_DATABASE_PAGE|
|Testo del messaggio|Rilevato danneggiamento della memoria hardware nel database '%ls', ID file: %u, ID pagina: %u, indirizzo memoria: 0x%I64x. La pagina è stata recuperata.|
||

## <a name="explanation"></a>Spiegazione

Questo messaggio indica che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha rilevato una pagina di memoria non valida in un oggetto memorizzato nella cache all'esterno del pool di buffer. Questo messaggio viene generato nei sistemi che supportano la possibilità di eseguire il recupero dagli errori di memoria. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] corregge questi errori di memoria scartando le pagine di memoria danneggiate che non sono state modificate e registra questo messaggio di errore. Se la pagina di memoria danneggiata è stata modificata (dirty), viene generato l'errore 824 ([MSSQLSERVER_824](mssqlserver-824-database-engine-error.md))

## <a name="user-action"></a>Azione utente

È consigliabile eseguire controlli dell'hardware o del sistema per determinare se esistono problemi di memoria o di CPU. Verificare che siano stati applicati al sistema tutti i driver di sistema, gli aggiornamenti del sistema operativo e gli aggiornamenti dell'hardware. Valutare la possibilità di eseguire qualsiasi diagnostica del produttore dell'hardware, inclusi i test correlati alla memoria. Ogni volta che viene visualizzato questo errore, provare a eseguire `DBCC CHECKDB` su tutti i database in questa istanza.

## <a name="more-information"></a>Ulteriori informazioni

Nei computer con hardware più recente e che eseguono Windows Server 2012 o versione successiva, l'hardware può notificare al sistema operativo e alle applicazioni che le pagine di memoria (pagine del sistema operativo) sono contrassegnate come non valide o danneggiate. Le applicazioni come [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possono registrare queste notifiche di pagine di memoria non valide usando il set di API seguente:

- `GetMemoryErrorHandlingCapabilities`
- `RegisterBadMemoryNotification`
- `BadMemoryCallbackRoutine`

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aggiunge il supporto per queste notifiche in Microsoft [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] e versioni successive. Durante l'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] controlla se l'hardware supporta questa nuova funzionalità. Nel log degli errori viene inoltre visualizzato il messaggio seguente:

> \<Datetime> Il computer server supporta il ripristino degli errori di memoria. La protezione della memoria SQL è abilitata per il ripristino dal danneggiamento della memoria.

Attualmente, solo il pool di buffer si mobilita quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] riceve queste notifiche. Quando riceve una notifica, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve scorrere l'intero pool di buffer e individuare l'indirizzo per ogni buffer allocato. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa quindi l'API `QueryWorkingSetEX` per verificare se una delle pagine di memoria a supporto della pagina di dati è contrassegnata come non valida. Nella struttura di output `PSAPI_WORKING_SET_EX_BLOCK` che corrisponde a questa pagina di memoria, il membro non valido sarà impostato su 1 se è stato segnalato un danneggiamento.

Se la pagina di dati o il pool di buffer non è attualmente modificato o non elabora I/O, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può rimuovere la pagina di dati e annullarne il commit. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] registra quindi il messaggio seguente:

> Rilevato danneggiamento della memoria hardware nel database '%ls', ID file: %u, ID pagina: %u, indirizzo memoria: 0x%I64x. La pagina è stata recuperata.

Quando le query richiedono nuovamente tale pagina di dati, il pool di buffer può leggere la pagina di dati dal disco e riportare il contenuto nel pool di buffer. È anche possibile che la versione su disco della pagina sia in uno stato danneggiato. In tal caso, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potrebbe registrare altri errori, ad esempio l'errore 824.

Se la pagina non valida non viene usata dal pool di buffer, ma da un altro oggetto o struttura nella cache, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] registra il messaggio seguente:

> È stato rilevato danneggiamento della memoria hardware non correggibile. Il sistema potrebbe diventare instabile. Per informazioni più dettagliate, vedere il registro eventi di Windows.

Se il server segnala errori di memoria, è necessario contattare il fornitore dell'hardware del computer ed eseguire le azioni appropriate, ad esempio eseguire la diagnostica della memoria, aggiornare il BIOS e il firmware e sostituire i moduli di memoria non validi.

È possibile usare il flag di traccia 849 di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per evitare che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si registri nel sistema operativo per le notifiche degli errori di memoria. Tuttavia, tenere presente che l'abilitazione del flag di traccia 849 impedirà a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di ricevere notifiche di memoria non valida dal sistema operativo. Non è pertanto consigliabile usare questo flag di traccia in condizioni normali.

Tenere anche presente che, per impostazione predefinita, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] riceverà queste notifiche su hardware supportato.

Tenere anche presente che quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si registra per queste notifiche degli errori di memoria, il processo di sistema del Lazywriter non esegue controlli delle pagine costanti. Per altre informazioni sui controlli delle pagine costanti, vedere [Come risolvere un errore Msg 832 (pagina costante modificata) in SQL Server](https://support.microsoft.com/help/2015759).
