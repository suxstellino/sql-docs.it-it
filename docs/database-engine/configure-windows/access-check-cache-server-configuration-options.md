---
title: Opzioni di configurazione del server access check cache | Microsoft Docs
description: Informazioni sulla cache dei risultati del controllo dell'accesso e sulle opzioni che controllano il comportamento della cache. Scoprire quando modificare queste opzioni in SQL Server.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- access check cache option
- access check cache bucket count
- access check cache quota
ms.assetid: 0a992ea8-3ec6-4a4d-97b5-460ae7326247
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 893400411c6f3d2faf6313923aa0f6127a3f2cf0
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171073"
---
# <a name="access-check-cache-server-configuration-options"></a>Opzioni di configurazione del server access check cache
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]accede agli oggetti di database, il controllo dell'accesso viene memorizzato nella cache in una struttura interna denominata **cache dei risultati del controllo dell'accesso**. 
  
L'opzione **Conteggio bucket cache controllo di accesso** controlla il numero di bucket di hash usati per la cache dei risultati di controllo accesso. 

L'opzione **Quota della cache controllo di accesso** controlla il numero di voci archiviate nella cache dei risultati di controllo accesso. Quando viene raggiunto il numero massimo di voci, le voci meno recenti vengono rimosse dalla cache dei risultati di controllo accesso.
  
Il valore predefinito 0 indica che le opzioni vengono gestite da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . A partire da [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], i valori predefiniti vengono convertiti nelle configurazioni interne seguenti:
-   Per Conteggio bucket cache controllo di accesso, il valore 0 imposta un valore predefinito di 256 bucket.
-   Per Quota della cache controllo di accesso, il valore 0 imposta un valore predefinito di 1.024 voci.

Raramente la modifica di tali opzioni consente di ottenere prestazioni migliori. Ad esempio, è possibile ridurre le dimensioni della cache dei risultati di controllo accesso se viene usata una quantità eccessiva di memoria. Oppure è possibile aumentare le dimensioni della cache dei risultati di controllo accesso se si riscontra un utilizzo elevato della CPU quando le autorizzazioni vengono ricalcolate.
 
> [!IMPORTANT]
> È consigliabile modificare le opzioni solo se suggerito dal Servizio Supporto Tecnico Clienti Microsoft. Se è necessario modificare i valori di "Conteggio bucket cache controllo di accesso" e di "Quota della cache controllo di accesso", usare un rapporto di 1:4. Se ad esempio si imposta il valore di "Conteggio bucket cache controllo di accesso" su 512, è necessario impostare il valore di "Quota della cache controllo di accesso" su 2.048. 
  
## <a name="see-also"></a>Vedere anche  
 [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
