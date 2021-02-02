---
title: Domande frequenti in PolyBase| Microsoft Docs
description: Confrontare PolyBase e i server collegati e confrontare PolyBase nei cluster Big Data e PolyBase in istanze autonome. Vengono illustrate le novità in PolyBase 2019.
ms.date: 12/02/2020
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.openlocfilehash: 70ffec480513c7a002a8691f2624a604bc82ead0
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235810"
---
# <a name="frequently-asked-questions"></a>Domande frequenti

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="polybase-vs-linked-servers"></a>PolyBase e server collegati
Nella tabella seguente vengono evidenziate le differenze tra PolyBase e le funzionalità per server collegati:

|PolyBase | Server collegati|
|--------------------------|--------------------------|  
|Oggetto con ambito di database|Oggetto con ambito di istanza|
|Usa driver ODBC|Usa provider OLEDB|
|Supporta operazioni di sola lettura per tutte le origini dati e l'operazione di inserimento solo per origini dati HADOOP e pool di dati|Supporta sia operazioni di lettura che di scrittura|
|Le query a un'origine dati remota da una singola connessione possono essere scalate orizzontalmente |Le query a un'origine dati remota da una singola connessione non possono essere scalate orizzontalmente|
|È supportata la distribuzione dei predicati|È supportata la distribuzione dei predicati|
|Non è richiesta una configurazione separata per il gruppo di disponibilità|È richiesta una configurazione separata per ogni istanza nel gruppo di disponibilità|
|Solo autenticazione di base|Autenticazione di base e integrata|
|Adatto per le query analitiche che elaborano un numero elevato di righe|Adatta per le query OLTP che restituiscono singole righe o poche righe|
|Le query che usano una tabella esterna non possono partecipare alle transazioni distribuite|Le query distribuite possono partecipare alle transazioni distribuite|

## <a name="whats-new-in-polybase-2019"></a>Novità in PolyBase 2019 

PolyBase in [!INCLUDE[sssql19](../../includes/sssql19-md.md)] ora può leggere i dati da una più ampia varietà di origini dati. I dati di queste origini dati esterne possono essere archiviati come tabelle esterne in SQL Server. PolyBase supporta anche il calcolo con distribuzione verso queste origini dati esterne, con l'esclusione dei tipi generici ODBC.

### <a name="compatible-data-sources"></a>Origini dati compatibili

- SQL Server
- Oracle
- Teradata
- MongoDB
- Tipi generici ODBC compatibili
  
> [!NOTE]
> PolyBase consente la connessione a origini dati esterne tramite i driver ODBC di terze parti. Questi driver non sono forniti con PolyBase e potrebbero non funzionare come previsto. Per altre informazioni, vedere la [guida](../../relational-databases/polybase/polybase-configure-odbc-generic.md) per la configurazione dei tipi generici ODBC in PolyBase.  

## <a name="polybase-in-big-data-clusters-vs-polybase-in-stand-alone-instances"></a>PolyBase nei cluster Big Data e PolyBase nelle istanze autonome

Nella tabella seguente sono evidenziate le funzionalità di PolyBase disponibili in un'installazione autonoma di [!INCLUDE[sssql19](../../includes/sssql19-md.md)] e in un cluster Big Data di [!INCLUDE[sssql19](../../includes/sssql19-md.md)]:

|Funzionalità |Cluster Big Data|Istanza autonoma|
|--------------------------|--------------------------|---------|   
|Creare un'origine dati esterna per SQL Server, Oracle, Teradata e Mongo DB |X|X|
|Creare origine dati esterna usando un driver ODBC di terze parti compatibile | | X|
|Creare un'origine dati esterna per l'origine dati HADOOP | X| X|
|Creare un'origine dati esterna per Archiviazione BLOB di Azure | X| X|
|Creare una tabella esterna in un pool di dati di SQL Server | X| |
|Creare una tabella esterna in un pool di archiviazione di SQL Server | X| |
|Esecuzione di query con scalabilità orizzontale | X| X (solo Windows) |

> [!NOTE]
>La tabella non descrive le funzionalità disponibili nella versione più recente di [!INCLUDE[sssql19](../../includes/sssql19-md.md)] CTP. Per informazioni sulle funzionalità disponibili, vedi le note sulla versione. Per altre informazioni sulle connessioni tramite il connettore ODBC generico, vedere la [guida pratica alla configurazione dei tipi generici ODBC](polybase-configure-odbc-generic.md).
