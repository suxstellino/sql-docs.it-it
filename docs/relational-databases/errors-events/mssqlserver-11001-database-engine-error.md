---
description: MSSQLSERVER_11001
title: MSSQLSERVER_11001 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
f1_keywords:
- "12001"
helpviewer_keywords:
- 11001 (Database Engine error)
ms.assetid: 53d4d63a-61e3-441f-bfe9-9d44f7a05fd4
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1805acf488eb9559db479a6915450576296f33f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197227"
---
# <a name="mssqlserver_11001"></a>MSSQLSERVER_11001
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|11001|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico||  
|Testo del messaggio|Si è verificato un errore durante il tentativo di stabilire una connessione al server.  Quando ci si connette a SQL Server, è possibile che l'errore sia determinato dal fatto che le impostazioni predefinite di SQL Server non consentono le connessioni remote. (provider: provider TCP, errore: 0 - Host sconosciuto.) (provider di dati SqlClient .Net)|  
  
## <a name="explanation"></a>Spiegazione  
Non è possibile stabilire la connessione tra il client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e il server. L'errore potrebbe verificarsi poiché il client non è in grado di risolvere il nome del server oppure il nome del server non è corretto.  
  
## <a name="user-action"></a>Azione dell'utente  
Verificare di avere immesso il nome corretto del server sul client e che dal client sia possibile risolvere il nome del server. Per verificare la risoluzione dei nomi TCP/IP, con il sistema operativo Windows è possibile usare il comando **ping**.  
  
## <a name="see-also"></a>Vedere anche  
[Protocolli e librerie di rete](~/sql-server/install/network-protocols-and-network-libraries.md)  
[Configurazione di rete dei client](~/database-engine/configure-windows/client-network-configuration.md)  
[Configurazione di protocolli client](~/database-engine/configure-windows/configure-client-protocols.md)  
[Abilitare o disabilitare un protocollo di rete del server](~/database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)  
  
