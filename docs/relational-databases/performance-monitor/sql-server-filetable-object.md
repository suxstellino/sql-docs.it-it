---
title: Oggetto FileTable di SQL Server | Microsoft Docs
description: Informazioni sull'oggetto prestazioni SQLServer:FileTable, che fornisce i contatori per le statistiche associate a FileTable e all'accesso non in transazioni.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:FileTable
ms.assetid: 325f5e58-1095-450f-9321-dfacfe6fd55f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 310bbb694eb3751d16543c5bc108ccdf2a880b02
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505716"
---
# <a name="sql-server-filetable-object"></a>SQL Server, Oggetto FileTable
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
L'oggetto prestazioni **SQLServer:FileTable** fornisce i contatori per le statistiche associate a FileTable e all'accesso non in transazioni.

La tabella seguente descrive gli oggetti prestazioni **FileTable** di SQL Server.

|**Contatori FileTable di SQL Server**|Descrizione|  
|-------------|-----------------|  
|**Tempo medio necessario per l'eliminazione dell'elemento FileTable**|Tempo medio, in millisecondi, necessario per eliminare un elemento FileTable.|
|**Tempo medio enumerazione FileTable**|Tempo medio, in millisecondi, necessario per una richiesta di enumerazione FileTable.|
|**Tempo medio per il termine di handle FileTable**|Tempo medio, in millisecondi, necessario per terminare un handle FileTable.|
|**Tempo medio necessario per spostare un elemento FileTable**|Tempo medio, in millisecondi, necessario per spostare un elemento FileTable.|
|**Tempo medio per ogni richiesta di I/O file**|Tempo medio, in millisecondi, necessario per la gestione di una richiesta di I/O file in arrivo.|
|**Tempo medio per ogni risposta di I/O file**|Tempo medio, in millisecondi, necessario per la gestione di una risposta di I/O file in uscita.|
|**Tempo medio necessario per rinominare un elemento FileTable**|Tempo medio, in millisecondi, necessario per rinominare un elemento FileTable.|
|**Tempo medio di recupero elemento FileTable**|Tempo medio, in millisecondi, necessario per recuperare un elemento FileTable.|
|**Tempo medio necessario per l'aggiornamento dell'elemento FileTable**|Tempo medio, in millisecondi, necessario per aggiornare un elemento FileTable.|
|**Operazioni db FileTable al secondo**|Numero totale di eventi operativi di database elaborati dal componente dell'archivio FileTable al secondo.|
|**Richieste di enumerazione FileTable al secondo**|Numero totale di richieste di enumerazione FileTable al secondo.|
|**Richieste di I/O file FileTable al secondo**|Numero totale di richieste di I/O file FileTable in arrivo al secondo.|
|**Risposta di I/O file FileTable al secondo**|Numero totale di risposte di I/O file in uscita al secondo.|
|**Richieste di eliminazione elemento FileTable al secondo**|Numero totale di richieste di eliminazione elemento FileTable al secondo.|
|**Richieste get elemento FileTable al secondo**|Numero totale di richieste di recupero elemento FileTable al secondo.|
|**Richieste di spostamento elemento FileTable al secondo**|Numero totale di richieste di spostamento elemento FileTable al secondo.|
|**Richieste di ridenominazione elemento FileTable al secondo**|Numero totale di richieste di ridenominazione FileTable al secondo.|
|**Richieste di aggiornamento elemento FileTable al secondo**|Numero totale di richieste di aggiornamento elemento FileTable al secondo.|
|**Operazioni di termine handle FileTable al secondo**|Numero totale di operazioni di termine handle FileTable al secondo.|
|**Operazioni tabella FileTable al secondo**|Numero totale di eventi operativi di tabella elaborati dal componente dell'archivio FileTable al secondo.|
|**Tempo necessario per l'eliminazione dell'elemento FileTable BASE**|Solo per uso interno.|
|**Tempo necessario per l'enumerazione FileTable BASE**|Solo per uso interno.|
|**Tempo per il termine di handle FileTable BASE**|Solo per uso interno.|
|**Tempo necessario per lo spostamento dell'elemento FileTable BASE**|Solo per uso interno.|
|**Tempo per ogni richiesta di I/O file BASE**|Solo per uso interno.|
|**Tempo per ogni risposta di I/O file BASE**|Solo per uso interno.|
|**Tempo necessario per rinominare l'elemento FileTable BASE**|Solo per uso interno.|
|**Tempo per recuperare l'elemento FileTable BASE**|Solo per uso interno.|
|**Tempo necessario per l'aggiornamento dell'elemento FileTable BASE**|Solo per uso interno.| 
 
## <a name="see-also"></a>Vedere anche  
[Monitoraggio dell'utilizzo delle risorse (Monitor di sistema)](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)
