---
description: MSSQLSERVER_8632
title: MSSQLSERVER_8632
ms.custom: ''
ms.date: 10/27/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 8632 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 6755ddfcf85b8e49e00697e94eb91e340d39093c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205736"
---
# <a name="mssqlserver_8632"></a>MSSQLSERVER_8632
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Dettagli

|Attributo|valore|
|---|---|
|Nome prodotto|SQL Server|
|ID evento|8632|
|Origine evento|MSSQLSERVER|
|Componente|SQLEngine|
|Nome simbolico|QUERY_EXPRESSION_TOO_COMPLEX|
|Testo del messaggio|Errore interno: è stato raggiunto un limite per i servizi di gestione delle espressioni. Provare a semplificare la query rimuovendo le espressioni particolarmente complesse.|
||

## <a name="explanation"></a>Spiegazione

L'errore 8632 viene generato quando si esegue una query in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contenente un numero elevato di identificatori e costanti in un'unica espressione. Viene segnalato all'utente un messaggio di errore simile al seguente:

> Server:  Messaggio 8632, livello 17, stato 2, riga 1  
Errore interno: è stato raggiunto un limite per i servizi di gestione delle espressioni. Provare a semplificare la query rimuovendo le espressioni particolarmente complesse.

## <a name="cause"></a>Causa

Questo problema si verifica perché [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] limita il numero di identificatori e costanti che possono essere contenuti in una singola espressione di query. Il limite è 65.535. La query seguente, ad esempio, include solo un'espressione:

```sql
select a, b + c, d + e
```

Questa espressione recupera tutte e cinque le colonne, calcola gli operatori di addizione e invia tre risultati previsti al client.

Il test per il numero di identificatori e costanti viene eseguito dopo che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] espande tutti gli identificatori e le costanti a cui si fa riferimento. Ad esempio, gli elementi seguenti potrebbero essere espansi:

- L'asterisco (*) nell'elenco di selezione
- Una vista
- Una definizione di colonna calcolata

Se il numero dopo l'espansione supera il limite, non sarà possibile eseguire la query.

## <a name="user-action"></a>Azione utente

Per ovviare a questo problema, riscrivere la query. Fare riferimento a un numero inferiore di identificatori e costanti nell'espressione più grande nella query. È necessario assicurarsi che il numero di identificatori e costanti in ogni espressione della query non superi il limite. A tale scopo, potrebbe essere necessario suddividere una query in più query e creare quindi un risultato intermedio temporaneo.
