---
description: MSSQLSERVER_854
title: MSSQLSERVER_854
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 854 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 9c3f2071748702b7537d1bc05450f09bb437ab0b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194092"
---
# <a name="mssqlserver_854"></a>MSSQLSERVER_854
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|Valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|854|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|HARDWARE_MEMORY_SCRUBBER|
|Testo del messaggio|Il computer supporta il ripristino degli errori di memoria. La protezione della memoria SQL è abilitata per il ripristino dal danneggiamento della memoria|
||

## <a name="explanation"></a>Spiegazione

Questo messaggio indica che l'hardware nel sistema operativo supporta la possibilità di eseguire il ripristino da errori di memoria. Nei computer con hardware più recente e che eseguono Windows Server 2012 o versione successiva, l'hardware può notificare al sistema operativo e alle applicazioni che le pagine di memoria (pagine del sistema operativo) sono contrassegnate come non valide o danneggiate. Le applicazioni come [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possono registrare queste notifiche di pagine di memoria non valide usando il set di API seguente:

- `GetMemoryErrorHandlingCapabilities`
- `RegisterBadMemoryNotification`
- `BadMemoryCallbackRoutine`

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aggiunge il supporto per queste notifiche in Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012 e versioni successive. Durante l'avvio di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] controlla se l'hardware supporta questa nuova funzionalità. Nel log degli errori viene inoltre visualizzato il messaggio seguente:

> \<Datetime> Il computer server supporta il ripristino degli errori di memoria. La protezione della memoria SQL è abilitata per il ripristino dal danneggiamento della memoria.

## <a name="user-action"></a>Azione utente

Controllare se sono presenti altri errori come 855 e 856 ed eseguire le azioni correttive appropriate.

## <a name="more-information"></a>Ulteriori informazioni

È possibile usare il flag di traccia 849 di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per evitare che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si registri nel sistema operativo per le notifiche degli errori di memoria. Tuttavia, tenere presente che l'abilitazione del flag di traccia 849 impedirà a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di ricevere notifiche di memoria non valida dal sistema operativo. Non è pertanto consigliabile usare questo flag di traccia in condizioni normali.
