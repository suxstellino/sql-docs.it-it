---
title: MSSQLSERVER_17132 | Microsoft Docs
description: Il computer di SQL Server non è in grado di elaborare il pacchetto di accesso client. Vedere la descrizione dell'errore e delle possibili soluzioni.
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17132 (Database Engine error)
ms.assetid: d1d198bd-6730-4394-bd5f-28f320c01a38
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 887dde5194364affa8915ab1a30e13220ecb8e2e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196764"
---
# <a name="mssqlserver_17132"></a>MSSQLSERVER_17132
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|17132|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|INIT_NODESSPACE|  
|Testo del messaggio|Impossibile avviare il server. Memoria insufficiente per il descrittore. Ridurre il carico di memoria non essenziale o aumentare la memoria del sistema.|  
  
## <a name="explanation"></a>Spiegazione  
Impossibile allocare memoria sufficiente per archiviare il descrittore interno del server.  
  
## <a name="user-action"></a>Azione dell'utente  
Aggiungere una quantità maggiore di memoria al computer. Potrebbe essere utile eseguire alcuni passaggi generici per la risoluzione dei problemi relativi alla memoria.  
  
