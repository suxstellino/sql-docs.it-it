---
title: Pool di buffer ibrido | Microsoft Docs
description: Scoprire in che modo il pool di buffer ibridi rende accessibili i dispositivi di memoria permanenti tramite il bus di memoria. Attivare o disattivare questa funzionalità di SQL Server 2019 e visualizzare le procedure consigliate.
ms.custom: ''
ms.date: 10/31/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
ms.assetid: ''
author: briancarrig
ms.author: brcarrig
manager: amitban
ms.openlocfilehash: ccf508d0415bc6baabc9c054a95ec26cf7989880
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236514"
---
# <a name="hybrid-buffer-pool"></a>Pool di buffer ibrido
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Il pool di buffer ibrido consente agli oggetti del pool di fare riferimento alle pagine dati nei file di database presenti nei dispositivi con memoria persistente, anziché a copie delle pagine dati nella memoria DRAM volatile. Questa funzionalità è stata introdotta in [!INCLUDE[sqlv15](../../includes/sssql19-md.md)].

![Pool di buffer ibrido](./media/hybrid-buffer-pool.png)

I dispositivi con memoria persistente (PMEM) sono indirizzabili a byte e, se viene usato un file system con supporto per la memoria persistente (DAX) con accesso diretto (come XFS, EXT4 o NTFS), consentono di accedere ai file nel file system usando le consuete API di file system nel sistema operativo. In alternativa, possono eseguire operazioni di caricamento e archiviazione in base alle mappe di memoria dei file nel dispositivo. In questo modo, le applicazioni in grado di riconoscere i dispositivi con memoria persistente, come SQL Server, possono accedere ai file nel dispositivo senza attraversare lo stack di archiviazione tradizionale.

Il pool di buffer ibrido usa questa funzionalità per eseguire operazioni di caricamento e archiviazione sui file mappati alla memoria, in modo da sfruttare il dispositivo con memoria persistente come cache per il pool di buffer e per l'archiviazione dei file di database. Si verifica così la situazione unica in cui una lettura logica e una lettura fisica sono essenzialmente la stessa operazione. I dispositivi con memoria persistente sono accessibili tramite il bus di memoria esattamente come la normale DRAM volatile.

Nella cache del dispositivo per il pool di buffer ibrido vengono memorizzate solo le pagine di dati clean. Quando una pagina è contrassegnata come dirty, viene prima copiata nel pool di buffer DRAM e poi riscritta nel dispositivo con memoria persistente e contrassegnata nuovamente come clean. Questo si verifica durante le normali operazioni di checkpoint in modo analogo a quanto avviene per un dispositivo a blocchi standard.

La funzionalità di pool di buffer ibrido è disponibile sia per Windows che per Linux. Il dispositivo con memoria persistente deve essere formattato con un file system che supporti DAX (DirectAccess). I file system XFS, EXT4 e NTFS includono tutti il supporto per DAX. SQL Server rileva automaticamente se i file di dati si trovano in un dispositivo con memoria persistente formattato in modo appropriato ed eseguono il mapping della memoria per i file di database all'avvio, quando un nuovo database viene collegato, ripristinato o creato.

Per altre informazioni, vedere:

* [Comprendere e distribuire memoria persistente (Windows)](/windows-server/storage/storage-spaces/deploy-pmem/)
* [Configurare la memoria persistente per SQL Server in Linux](../../linux/sql-server-linux-configure-pmem.md)


## <a name="enable-hybrid-buffer-pool"></a>Abilitare il pool di buffer ibrido

[!INCLUDE[sqlv15](../../includes/sssql19-md.md)] introduce il linguaggio dei dati dinamici per controllare il pool di buffer ibrido.

Nell'esempio seguente viene abilitato il pool di buffer ibrido per un'istanza di SQL Server:

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED HYBRID_BUFFER_POOL = ON;
```

Per impostazione predefinita, il pool di buffer ibrido è disabilitato nell'ambito dell'istanza. Si noti che è necessario riavviare l'istanza di SQL Server per rendere effettiva la modifica dell'impostazione. È necessario un riavvio per facilitare l'allocazione di pagine hash sufficienti tenendo conto della capacità PMEM totale nel server.

Nell'esempio seguente viene abilitato il pool di buffer ibrido per un database specifico.

```sql
ALTER DATABASE <databaseName> SET MEMORY_OPTIMIZED = ON;
```

Per impostazione predefinita, il pool di buffer ibrido è abilitato nell'ambito del database.

## <a name="disable-hybrid-buffer-pool"></a>Disabilitare il pool di buffer ibrido

L'esempio seguente disabilita il pool di buffer ibrido a livello di istanza:

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED HYBRID_BUFFER_POOL = OFF;
```

Per impostazione predefinita, il pool di buffer ibrido è disabilitato a livello di istanza. Affinché la modifica abbia effetto, l'istanza deve essere riavviata. Questo consente di assicurarsi che per il pool di buffer venga allocato un numero sufficiente di pagine hash, poiché in questo caso è necessario tenere in considerazione la capacità della memoria persistente sul server.

Nell'esempio seguente viene disabilitato il pool di buffer ibrido per un database specifico.

```sql
ALTER DATABASE <databaseName> SET MEMORY_OPTIMIZED = OFF;
```

Per impostazione predefinita, il pool di buffer ibrido è abilitato nell'ambito del database.

## <a name="view-hybrid-buffer-pool-configuration"></a>Visualizzare la configurazione pool di buffer ibrido

L'esempio seguente restituisce lo stato corrente della configurazione del pool di buffer ibrido per l'istanza.

```sql
SELECT * FROM
sys.server_memory_optimized_hybrid_buffer_pool_configuration;
```

Nell'esempio seguente vengono elencati i database e l'impostazione del livello di database per il pool di buffer ibrido (`is_memory_optimized_enabled`).

```sql
SELECT name, is_memory_optimized_enabled FROM sys.databases;
```

## <a name="best-practices-for-hybrid-buffer-pool"></a>Procedure consigliate per il pool di buffer ibrido

 - Quando si formatta un dispositivo PMEM in Windows, usare le dimensioni di unità di allocazione più grandi disponibili per NTFS (2 MB in Windows Server 2019) e assicurarsi che il dispositivo sia stato formattato per DAX (Direct Access).

 - Usare [Blocco di pagine in memoria](./enable-the-lock-pages-in-memory-option-windows.md) in Windows.

 - Le dimensioni dei file devono essere un multiplo di 2 MB (modulo 2 MB deve essere uguale a zero).

 - Se l'impostazione con ambito server per il pool di buffer ibrido è disabilitata, la funzionalità non verrà usata da alcun database utente.

 - Se l'impostazione con ambito server per il pool di buffer ibrido è abilitata, sarà possibile usare l'impostazione con ambito database per disabilitare la funzionalità per i singoli database utente.
