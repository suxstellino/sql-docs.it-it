---
description: MSSQLSERVER_21892
title: MSSQLSERVER_21892 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 21892 (Database Engine error)
ms.assetid: 199818e8-28f4-440e-ad85-7bea88f51153
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 56ba58d0166913aab8c789884eb98f1d6ce7fbb6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196372"
---
# <a name="mssqlserver_21892"></a>MSSQLSERVER_21892
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|21892|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|SQLErrorNum21892|  
|Testo del messaggio|Impossibile eseguire una query su sys.availability_replicas nel database primario del gruppo di disponibilità associato al nome di rete virtuale '%s' per i nomi server delle repliche membro: errore = %d, messaggio di errore = %s.'|  
  
## <a name="explanation"></a>Spiegazione  
**sp_validate_replica_hosts_as_publishers** esegue una query sul database primario corrente del gruppo di disponibilità associato al server di pubblicazione reindirizzato per determinare le istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] che ospitano le repliche del membro.  In caso di errore di questa query, viene restituito l'errore 21892.  
  
**sp_validate_replica_hosts_as_publishers** è presente in genere al primo uso del server collegato temporaneo; se si verificano problemi di connettività, quindi, è probabile che si presentino prima con **sp_validate_replica_hosts_as_publishers**. A differenza di **sp_validate_redirected_publisher**, il server collegato usato da **sp_validate_replica_hosts_as_publishers** usa sempre le credenziali del chiamante in caso di connessione a uno degli host di replica del gruppo di disponibilità.  
  
## <a name="user-action"></a>Azione dell'utente  
In caso di esecuzione di questa stored procedure, assicurarsi che sia in esecuzione da un accesso valido su tutte le repliche. L'accesso richiede autorizzazioni sufficienti per eseguire una query sulle tabelle di metadati del gruppo di disponibilità nonché eseguire una query sulle tabelle di metadati delle sottoscrizioni nella replica del database del server di pubblicazione.  
  
Esaminare l'errore originale cui si fa riferimento per determinare la causa dell'errore ed eseguire azioni correttive appropriate.  
  
