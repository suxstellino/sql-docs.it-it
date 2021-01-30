---
description: MSSQLSERVER_5180
title: MSSQLSERVER_5180
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 5180 (Database Engine error)
ms.assetid: ''
author: rgward
ms.author: ramakoni
ms.openlocfilehash: f80f4dafad6b0570837728e15a79eebdd7d6f41c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185537"
---
# <a name="mssqlserver_5180"></a>MSSQLSERVER_5180
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|Valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|5180|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|DSK_BAD_FCB_FILEID|
|Testo del messaggio|Impossibile aprire il blocco di controllo file (FCB) per l'ID di file non valido %d nel database '%.*ls'. Verificare il percorso del file. Eseguire DBCC CHECKDB.|
||

## <a name="explanation"></a>Spiegazione

Una query o un'operazione potrebbe non riuscire con un errore 5180 quando il [!INCLUDE[ssDENoVersion](../../includes/ssdenoversion_md.md)] fa riferimento a un ID di file non valido. Questo è un esempio:

> messaggio 5180, livello 22, stato 1, riga 1  
Impossibile aprire il blocco di controllo file (FCB) per l'ID di file non valido %d nel database '%.*ls'. Verificare il percorso del file. Eseguire DBCC CHECKDB.

Poiché l'errore viene generato con gravità 22, la sessione dell'utente verrà disconnessa. Questo messaggio di errore viene scritto nel log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e nel log eventi dell'applicazione di Windows con EventID = 5180.

## <a name="possible-causes"></a>Possibili cause

Il [!INCLUDE[ssDENoVersion](../../includes/ssdenoversion-md.md)] fa riferimento a un ID di file in molte situazioni diverse, principalmente quando si fa riferimento a un ID di pagina (poiché l'ID del file è la prima parte dell'ID della pagina). Se per qualsiasi motivo, l'ID del file a cui si fa riferimento è < 0 o non è un ID di file valido in un database (in base agli ID di file validi elencati nelle viste del catalogo di sistema, ad esempio sys.database_files), è possibile che venga generato un errore 5180.

Una delle possibili cause è che un ID di file archiviato non è valido. Poiché il record inoltrato in una riga fa riferimento a un'altra pagina, quando si accede a tale pagina e l'ID del file non è valido, è possibile che venga generato un errore 5180. Questa condizione potrebbe essere un errore di danneggiamento del database nella pagina con il record inoltrato. (In questo esempio `DBCC CHECKDB` segnalerebbe il messaggio 8993).

Un altro esempio è un ID di file non valido come parte di un ID di pagina archiviato nell'intestazione di pagina per un ID di pagina successiva o precedente. Questo campo viene usato per collegare una serie di pagine, ad esempio in un indice cluster. Se l'ID di file non è valido per la pagina precedente o successiva, quando il motore deve fare riferimento a questo ID per passare alla pagina successiva o precedente, è possibile che venga generato un errore 5180. (In questo esempio `DBCC CHECKDB` segnala il messaggio 8981).

Se il problema non è un ID di file non valido a causa di un ID di pagina archiviato danneggiato, il problema potrebbe essere interno al [!INCLUDE[ssDENoVersion](../../includes/ssdenoversion-md.md)].

## <a name="user-action"></a>Azione utente

Se si verifica questo errore, è consigliabile eseguire `DBCC CHECKDB` sul database come indicato nel messaggio di errore. Se si riscontrano errori, è necessario eseguire il ripristino da un backup valido noto. Se non è possibile eseguire il ripristino da un backup, l'altra opzione consiste nell'usare l'opzione di correzione di `DBCC CHECKDB` come consigliato dall'output. Nella maggior parte dei casi, la correzione di questo tipo di errore causerà una perdita di dati. Per altre informazioni sulle cause dei problemi di danneggiamento del database, vedere l'articolo: [Risoluzione degli errori di coerenza del database segnalati da DBCC CHECKDB](https://support.microsoft.com/kb/2015748).

Se `DBCC CHECKDB` non segnala alcun errore e il problema persiste, contattare il supporto tecnico Microsoft per assistenza. Essere pronti a fornire la query con cui si riscontra l'errore 5180. Vedere la sezione [Ulteriori informazioni](#more-information) su come stabilire quale query ha riscontrato l'errore.

## <a name="more-information"></a>Ulteriori informazioni

Il blocco di controllo file (FCB) è una struttura di memoria interna che tiene traccia di informazioni importanti sul file associato al database. Un ID di file viene usato per identificare in modo univoco un FCB per un database. Se il motore del server tenta di fare riferimento a un ID di file non valido, non è possibile trovare la struttura FCB che attiva la condizione di errore 5180.

Per individuare la query che ha riscontrato questo errore, è possibile usare le tecniche seguenti:

- Per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008 e versioni successive, vedere se la sessione system_health contiene un record dell'errore, che dovrebbe includere il testo della query. Per altre informazioni sulla sessione system_health, vedere la risorsa: [Supporting SQL Server 2008: The system_health session](https://techcommunity.microsoft.com/t5/sql-server-support/supporting-sql-server-2008-the-system-health-session/ba-p/315509) (Supporto di SQL Server 2008: la sessione system_health).
- Usare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler e acquisire gli eventi `SQL:BatchStarting`, `RPC:Starting` e di eccezione. Trovare la query che precede l'evento di eccezione per 5180 per la sessione associata all'evento di eccezione.
