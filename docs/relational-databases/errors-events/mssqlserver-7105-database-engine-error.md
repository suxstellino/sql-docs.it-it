---
description: MSSQLSERVER_7105
title: MSSQLSERVER_7105
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 7105 (Database Engine error)
ms.assetid: ''
author: rgward
ms.author: ramakoni
ms.openlocfilehash: 7aa3b39996f8dc282e11d92706743a7ba42998da
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099269"
---
# <a name="mssqlserver_7105"></a>MSSQLSERVER_7105
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|Valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|7105|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|TXT_PGNOTEXIST|
|Testo del messaggio|L'ID database %d, pagina %S_PGID, slot %d per il nodo con tipo di dati LOB non esiste. Questo problema è in genere dovuto a transazioni che possono leggere dati di cui non è stato eseguito il commit da una pagina di dati. Eseguire DBCC CHECKTABLE|
||

## <a name="explanation"></a>Spiegazione

Una query può riscontrare il messaggio 7105 quando non è possibile accedere ai dati LOB (Large Object) a cui fa riferimento una riga di pagina del database.

Poiché il livello di gravità di questo errore è 22, la connessione viene terminata dal server. Questo messaggio di errore viene scritto anche nel file SQL ERRORLOG e nel log eventi dell'applicazione di Windows con EventID=7105.

## <a name="possible-causes"></a>Possibili cause

È possibile che questo errore si verifichi a causa di uno dei motivi seguenti:

- È presente un problema di danneggiamento del database all'interno di una pagina del database o all'interno delle strutture di pagina LOB a cui fa riferimento la pagina del database.
- La query che sta riscontrando l'errore usa l'hint per la query `READ UNCOMMITTED ISOLATION LEVEL` o `NOLOCK`.
- Esiste un problema all'interno del motore di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che causa l'esito negativo della query con questo errore.

Vedere le sezioni Risoluzione e [Ulteriori informazioni](#more-information) per determinare la causa del problema specifico e la soluzione appropriata.

## <a name="user-action"></a>Azione utente

1. Come indicato nel messaggio, il primo passaggio da eseguire consiste nell'eseguire `DBCC CHECKDB` sul database o `DBCC CHECKTABLE` sulla tabella in cui si è verificato il problema.

    - L'ID del database è indicato nel messaggio.
    - Per individuare la tabella interessata esatta senza eseguire `DBCC CHECKDB`, sarà necessario individuare le tabelle a cui ha avuto accesso la query che ha riscontrato l'errore. Un metodo consiste nell'usare SQL Profiler per tracciare la query. Tuttavia, in [!INCLUDE[sskatmai](../../includes/sskatmai-md.md)] e [!INCLUDE[sskatmai](../../includes/sskatmai-md.md)] R2 è possibile trovare la query usando la sessione Eventi estesi system_health. Vedere questo collegamento per altre informazioni sull'uso della sessione system_health: [Usare la sessione system_health](../extended-events/use-the-system-health-session.md).

    - Come per qualsiasi problema di coerenza del database, è possibile risolvere questi errori eseguendo il ripristino da un backup valido noto che non contiene questo problema.

    - Tuttavia, se non è possibile eseguire il ripristino da un backup, seguire le raccomandazioni per `DBCC CHECKDB` o `DBCC CHECKTABLE` per correggere questi errori. È possibile che ciò provochi la perdita di dati. Per altre informazioni sull'uso di CHECKDB e sulle cause dei problemi di danneggiamento del database, vedere l'articolo: [Risoluzione degli errori di coerenza del database segnalati da DBCC CHECKDB](https://support.microsoft.com/kb/2015748).
  
1. È possibile che questo errore si sia verificato perché la query che accede alla tabella usa un livello di isolamento `READ UNCOMMITTED` o l'hint per la query `NOLOCK` (condizione nota anche come lettura dirty).

   - Se `DBCC CHECKDB` o `DBCC CHECKTABLE` non visualizzano alcun errore associato alla tabella e ai dati LOB, la causa più probabile è l'uso di una lettura dirty. In tal caso, è necessario evitare di usare una lettura dirty o ritentare la query.
  
   - Se si ritiene che questa sia la causa dell'errore, non esiste un problema di coerenza del database effettivo.

## <a name="more-information"></a>Ulteriori informazioni

Se il danneggiamento del database è causato da questo problema, `DBCC CHECKDB` e/o `DBCC CHECKTABLE` dovrebbero segnalare errori. Tuttavia, questi comandi non segnaleranno il messaggio 7105. Gli errori rilevati da CHECKDB dipenderanno da ciò che risulta danneggiato nel riferimento alle strutture LOB o nelle strutture LOB.

- Se la riga della pagina del database non fa riferimento correttamente a una pagina LOB valida, è possibile che vengano visualizzati errori simili ai seguenti:

    > Messaggio 8929, livello 16, stato 1, riga 1  
    ID di oggetto 2137058649, ID di indice 0, ID di partizione 72057594038910976, ID di unità di allocazione 72057594039828480 (tipo Dati all'interno di righe): trovati errori nei dati all'esterno di righe con ID 131203072 di proprietà del record dati identificato da RID = (1:179:1)  
    Messaggio 8964, livello 16, stato 1, riga 1  
    Errore di tabella: ID di oggetto 2137058649, ID di indice 0, ID di partizione 72057594038910976, ID di unità di allocazione 72057594039894016 (tipo Dati LOB). Non esistono riferimenti al nodo di dati all'esterno di righe alla pagina (1:177), slot 1, ID di testo 131203072.  
    Messaggio 8965, livello 16, stato 1, riga 1  
    Errore di tabella: ID di oggetto 2137058649, ID di indice 0, ID di partizione 72057594038910976, ID di unità di allocazione 72057594039894016 (tipo Dati LOB). La pagina (255:177), slot 1 contiene un riferimento al nodo di dati all'esterno di righe alla pagina (1:179), slot 1, ID di testo 131203072, che non è stato rilevato durante l'analisi.  

- Scenari diversi per il problema possono causare diverse combinazioni di errori. Esempio:  

    > La pagina del database 1:179, slot 1 fa riferimento a una pagina LOB che non è una pagina valida nel database (pagina 255:177). La pagina (1:177) è una pagina LOB valida ma nessuna pagina di database vi ha mai fatto riferimento. Quindi, in questo caso, il problema è che la riga nello slot 1 della pagina 1:179 fa riferimento alla pagina 255:177 anziché alla pagina 1:177.

- La chiave per determinare se gli errori di `DBCC CHECKDB` sono correlati a problemi relativi alle pagine LOB consiste nel cercare le frasi "dati all'esterno di righe" e "tipo Dati LOB".

    > Il messaggio 8929 è un errore correlato alla pagina del database che fa riferimento a pagine LOB.  
Il messaggio 8964 è un errore che indica che nessuna pagina di database fa riferimento a una pagina LOB.  
Il messaggio 8965 è un errore che indica che una pagina del database fa riferimento a pagine LOB ma non esiste come pagina valida.

    In molte situazioni che coinvolgono questi tipi di errori, la correzione causerà l'eliminazione delle righe che puntano ai dati LOB e dei dati LOB stessi. L'algoritmo di correzione tenterà di rimuovere solo i frammenti LOB che interessano le righe del database in questione, ma ciò non è garantito in tutte le situazioni a seconda degli elementi danneggiati nella struttura ad albero LOB.

- Nell'esempio riportato di seguito, i messaggi restituiti da CHECKTABLE usando `REPAIR_ALLOW_DATA_LOSS` sono simili ai seguenti:

    > Correzione: record eliminato per l'oggetto con ID 2137058649, indice con ID 0, partizione con ID 72057594038910976, unità allocazione con ID 72057594039828480 (tipo Dati all'interno di righe), a pagina (1:179), slot 1. Gli indici verranno ricompilati.  
    Correzione: eliminata colonna di dati all'esterno di righe con ID 131203072 per l'oggetto con 2137058649, indice con ID 0, partizione con ID 72057594038910976, unità di allocazione con ID 72057594039894016 (tipo Dati LOB) a pagina (1:177), slot 1.  
    Messaggio 8929, livello 16, stato 1, riga 1  
    ID di oggetto 2137058649, ID di indice 0, ID di partizione 72057594038910976, ID di unità di allocazione 72057594039828480 (tipo Dati all'interno di righe): trovati errori nei dati all'esterno di righe con ID 131203072 di proprietà del record dati identificato da RID = (1:179:1)  
            L'errore è stato corretto.  
    Messaggio 8964, livello 16, stato 1, riga 1  
    Errore di tabella: ID di oggetto 2137058649, ID di indice 0, ID di partizione 72057594038910976, ID di unità di allocazione 72057594039894016 (tipo Dati LOB). Non esistono riferimenti al nodo di dati all'esterno di righe alla pagina (1:177), slot 1, ID di testo 131203072.  
            L'errore è stato corretto.  
    Messaggio 8965, livello 16, stato 1, riga 1  
    Errore di tabella: ID di oggetto 2137058649, ID di indice 0, ID di partizione 72057594038910976, ID di unità di allocazione 72057594039894016 (tipo Dati LOB). La pagina (255:177), slot 1 contiene un riferimento al nodo di dati all'esterno di righe alla pagina (1:179), slot 1, ID di testo 131203072, che non è stato rilevato durante l'analisi.  
            Impossibile correggere l'errore

    L'ultima parte del messaggio `Could not repair this error` è fuorviante. L'errore è stato corretto perché la riga della pagina del database che punta alla pagina non valida (255:177) è stata eliminata.