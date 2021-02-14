---
title: " Opzione di configurazione ADR cleaner retry timeout (min) | Microsoft Docs"
description: Viene illustrata l'impostazione di configurazione dell'istanza di SQL Server per il timeout dei tentativi di pulizia ADR.
ms.custom: ''
ms.date: 06/01/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- ADR cleaner retry timeout (min)
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 05502bbf760b0f8aa18d81739ce89e19e92397cb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347477"
---
# <a name="adr-cleaner-retry-timeout-min-configuration-option"></a>Opzione di configurazione ADR cleaner retry timeout (min)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Introdotta in SQL Server 2019.

Questa impostazione di configurazione è necessaria per il [ripristino accelerato del database](../../relational-databases/accelerated-database-recovery-concepts.md). La pulizia è un processo asincrono che viene riattivato periodicamente e pulisce le versioni di pagina non necessarie.

Occasionalmente, il processo di pulizia riscontra problemi durante l'acquisizione dei blocchi a livello di oggetto a causa di conflitti con il carico di lavoro dell'utente durante la pulizia. Il processo tiene traccia di tali pagine in un elenco separato. L'opzione ADR cleaner retry timeout (valore predefinito 15) controlla la quantità di tempo dedicata dal processo di pulizia esclusivamente a ritentare l'acquisizione del blocco sugli oggetti e la pulizia della pagina prima di abbandonare l'operazione. Il completamento di una pulizia con esito positivo al 100% è essenziale per la crescita delle transazioni interrotte nella mappa delle transazioni interrotte. Se non è possibile pulire l'elenco separato entro il timeout previsto, l'operazione di pulizia corrente verrà abbandonata e verrà avviata l'operazione di pulizia successiva.

## <a name="remarks"></a>Osservazioni  

Il processo di pulizia è a thread singolo in SQL Server 2019, quindi un'istanza di SQL Server può operare su un database alla volta. Se l'istanza include più di un database utente con l'opzione ADR abilitata, non aumentare il timeout a un valore elevato perché potrebbe ritardare la pulizia in un database mentre il tentativo viene eseguito in un altro database.

## <a name="examples"></a>Esempi

Negli esempi seguenti viene impostato il timeout dei tentativi di pulizia.

```tsql
sp_configure 'show advanced options', 1;  
RECONFIGURE;
GO 
sp_configure 'ADR cleaner retry timeout', 15;  
RECONFIGURE;  
GO  
```  

## <a name="see-also"></a>Vedere anche  

- [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)
- [Recupero del database accelerato](../../relational-databases/accelerated-database-recovery-concepts.md)
- [Gestire il ripristino accelerato del database](../../relational-databases/accelerated-database-recovery-management.md)