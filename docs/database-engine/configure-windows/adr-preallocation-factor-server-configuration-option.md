---
title: Opzione di configurazione ADR Preallocation Factor | Microsoft Docs
description: Viene illustrata l'impostazione di configurazione dell'istanza di SQL Server per il fattore di preallocazione ADR.
ms.custom: ''
ms.date: 06/01/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- ADR Preallocation Factor
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 9469b5de278c01b7b256ee348380ebd1ae19cde4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347460"
---
# <a name="adr-preallocation-factor-configuration-option"></a>Opzione di configurazione ADR Preallocation Factor

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Introdotta in SQL Server 2019.

Questa impostazione di configurazione è necessaria per il [ripristino accelerato del database](../../relational-databases/accelerated-database-recovery-concepts.md).

Il ripristino accelerato del database (ADR, Accelerated Database Recovery) mantiene le versioni dei dati a scopo di ripristino. Queste versioni vengono generate come parte di varie operazioni DML (Data Manipulation Language). Le versioni vengono archiviate in una tabella interna denominata archivio versioni permanente. 

## <a name="remarks"></a>Osservazioni  

Le prestazioni possono peggiorare se le pagine vengono allocate per la tabella di archivio versioni permanente come parte delle operazioni DML dell'utente in primo piano. Per risolvere questo problema, è disponibile un thread in background che prealloca le pagine e le mantiene prontamente disponibili per le transazioni DML. Le prestazioni sono migliori quando il thread in background prealloca un numero sufficiente di pagine e la percentuale di allocazioni alla tabella di archivio versioni permanente è prossima a 0. Il log degli errori contiene voci con il tag `PreallocatePVS` se la percentuale è elevata e influisce sulle prestazioni.

Il numero di pagine preallocate dal thread in background è basato su diverse euristiche del carico di lavoro, ma in gran parte l'allocazione delle pagine avviene in blocchi di 512 pagine. Il fattore di preallocazione ADR è un multiplo del blocco. Per impostazione predefinita, il fattore è 4. Ciò significa la preallocazione di 2048 pagine in contemporanea, quando necessario. 

Mentre il thread in background prende in considerazione i modelli di carico di lavoro, questo fattore può essere aumentato se necessario per migliorare le prestazioni.

> [!CAUTION]
> Se la preallocazione dell'archivio versioni permanente viene aumentata troppo, subentrerà una contesa con altre allocazioni nel sistema e potrebbe effettivamente ridurre le prestazioni complessive.
>
> Prima di modificare questa impostazione, testare le prestazioni complessive del sistema.

## <a name="examples"></a>Esempi  

Nell'esempio seguente il fattore di preallocazione viene impostato su 4.

```tsql
sp_configure 'show advanced options', 1;
RECONFIGURE;
GO 
sp_configure 'ADR Preallocation Factor', 4;
RECONFIGURE;
GO
```

## <a name="see-also"></a>Vedere anche  

- [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)
- [Recupero del database accelerato](../../relational-databases/accelerated-database-recovery-concepts.md)
- [Gestire il ripristino accelerato del database](../../relational-databases/accelerated-database-recovery-management.md)