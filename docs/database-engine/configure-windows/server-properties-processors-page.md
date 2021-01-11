---
title: Proprietà server (pagina dei processori)
description: Acquisire familiarità con le impostazioni del processore in SQL Server. Informazioni sulle opzioni che controllano il numero di thread di lavoro, l'assegnazione del processore e altre proprietà.
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.serverproperties.processor.f1
ms.assetid: cc1581a2-492b-41f0-bda5-17909b65c4f7
author: markingmyname
ms.author: maghan
ms.reviewer: drskwier, matteot
ms.custom: ''
ms.date: 12/17/2020
ms.openlocfilehash: 874cbbae2b418e9b9e06c7a95d62d34a99af38f5
ms.sourcegitcommit: a81823f20262227454c0b5ce9c8ac607aaf537e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/18/2020
ms.locfileid: "97684215"
---
# <a name="server-properties-processors-page"></a>Proprietà server (pagina Processori)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Utilizzare questa pagina per visualizzare o modificare le opzioni del processore. Le impostazioni Affinità processori sono abilitate solo se sono installati più processori.  

## <a name="options"></a>Opzioni

### <a name="processor-affinity"></a>Affinità processori
Consente di assegnare i processori a thread specifici, in modo da eliminare i ricaricamenti dei processori e ridurre la migrazione dei thread tra i processori. Per altre informazioni, vedere [Opzione di configurazione del server affinity mask](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md).

### <a name="io-affinity"></a>Affinità I/O
Associa l'I/O su disco di [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server a un subset di CPU specificato. Per altre informazioni, vedere [Opzione di configurazione del server affinity I/O mask](../../database-engine/configure-windows/affinity-input-output-mask-server-configuration-option.md).

### <a name="automatically-set-processor-affinity-mask-for-all-processors"></a>Imposta automaticamente maschera di affinità processori per tutti i processori
Consente a SQL Server di impostare l'affinità dei processori.

### <a name="automatically-set-io-affinity-mask-for-all-processors"></a>Imposta automaticamente maschera di affinità di I/O per tutti i processori
Consente a SQL Server di impostare l'affinità di I/O.

### <a name="maximum-worker-threads"></a>Numero massimo thread di lavoro
Il valore 0 consente a SQL Server di impostare dinamicamente il numero di thread di lavoro. Si tratta dell'impostazione ottimale per la maggior parte dei sistemi. A seconda della configurazione di sistema, è tuttavia possibile che l'impostazione di questa opzione su un valore specifico migliori le prestazioni. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server max worker threads](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md).  

### <a name="boost-sql-server-priority"></a>Aumenta priorità di SQL Server
Consente di specificare se eseguire SQL Server con una priorità di pianificazione di Microsoft Windows maggiore rispetto agli altri processi in esecuzione nello stesso computer. Per altre informazioni, vedere [Configurare l'opzione di configurazione del server priority boost](../../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md).  

> [!Note]
> Questa opzione non è disponibile con SSMS 18.x e versioni successive.

### <a name="use-windows-fibers-lightweight-pooling"></a>Usa fiber Windows (lightweight pooling)
Consente di utilizzare i fiber Windows anziché i thread per il servizio SQL Server. Questa opzione è disponibile solo in Windows 2003 Server Edition. Per altre informazioni, vedere [lightweight pooling Server Configuration Option](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md).

> [!Note]
> Questa opzione non è disponibile con SSMS 18.x e versioni successive.

### <a name="configured-values"></a>Valori configurati
Consente di visualizzare i valori configurati per le opzioni nel riquadro. Se si modificano questi valori, selezionare **Valori correnti** per verificare se le modifiche sono diventate effettive. In caso negativo, occorre prima di tutto riavviare l'istanza di SQL Server.

### <a name="running-values"></a>Valori correnti
Consente di visualizzare i valori correnti per le opzioni contenute nel riquadro. I valori sono di sola lettura.

## <a name="see-also"></a>Vedere anche
[Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)  


