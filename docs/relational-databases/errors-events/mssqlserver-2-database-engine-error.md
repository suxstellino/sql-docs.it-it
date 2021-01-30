---
description: MSSQLSERVER_2
title: MSSQLSERVER_2 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
f1_keywords:
- "2"
helpviewer_keywords:
- 2 (Database Engine error)
ms.assetid: 567fb571-7cda-4ce8-a702-cdff2df5d419
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1e9e979847ccf2cf5de202a7f657e98e799e2c53
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196454"
---
# <a name="mssqlserver_2"></a>MSSQLSERVER_2
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|2|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico||  
|Testo del messaggio|Si è verificato un errore durante il tentativo di stabilire una connessione al server.  Quando ci si connette a SQL Server, è possibile che l'errore sia determinato dal fatto che le impostazioni predefinite di SQL Server non consentono le connessioni remote. (provider: Provider named pipe, errore: 40 - Impossibile aprire una connessione a SQL Server) (.Net SqlClient Data Provider)|  
  
## <a name="explanation"></a>Spiegazione  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non ha risposto alla richiesta del client probabilmente perché il server non è stato avviato.  
  
## <a name="user-action"></a>Azione dell'utente  
Verificare che il server sia stato avviato.  
  
## <a name="see-also"></a>Vedere anche  
[Gestire il servizio Motore di database](~/database-engine/configure-windows/manage-the-database-engine-services.md)  
[Configurazione di protocolli client](~/database-engine/configure-windows/configure-client-protocols.md)  
[Protocolli e librerie di rete](~/sql-server/install/network-protocols-and-network-libraries.md)  
[Configurazione di rete dei client](~/database-engine/configure-windows/client-network-configuration.md)  
[Configurazione di protocolli client](~/database-engine/configure-windows/configure-client-protocols.md)  
[Abilitare o disabilitare un protocollo di rete del server](~/database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)  
  
