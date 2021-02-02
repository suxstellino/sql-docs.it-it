---
title: Configurare la memoria persistente
description: Informazioni su come configurare la memoria persistente per SQL Server in Linux, oltre a come creare spazi dei nomi per dispositivi PMEM.
ms.custom: seo-lt-2019
author: briancarrig
ms.author: brcarrig
ms.date: 10/31/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
monikerRange: '>= sql-server-linux-ver15  || >= sql-server-ver15'
ms.openlocfilehash: 5b99f12aebc27d04b384ff0b8d51b5359ca76d19
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236921"
---
# <a name="configure-persistent-memory-pmem-for-sql-server-on-linux"></a>Configurare la memoria persistente per SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo descrive come configurare la memoria persistente per [!INCLUDE[sqlv15](../includes/sssql19-md.md)] in Linux.

## <a name="overview"></a>Panoramica

[!INCLUDE[sqlv15](../includes/sssql19-md.md)] include una serie di funzionalità in memoria che usano la memoria persistente. In questo documento vengono illustrati i passaggi necessari per configurare la memoria persistente per SQL Server in Linux.

> [!NOTE]
> Il termine _consapevolezza_ è stato usato per spiegare il concetto relativo all'uso di un file system in grado di riconoscere la memoria persistente. L'accesso diretto al file system dalle applicazioni dello spazio utente viene facilitato tramite il mapping di memoria (`mmap()`). Quando viene creato un mapping di memoria per un file, l'applicazione può eseguire istruzioni di caricamento/archiviazione che ignorano completamente lo stack di I/O. Questo è considerato un metodo di accesso ai file "consapevole" dal punto di vista dell'applicazione dell'estensione host (ovvero il codice black box che consente a SQLPAL di interagire con il sistema operativo Windows o Linux).

## <a name="create-namespaces-for-pmem-devices"></a>Creare spazi dei nomi per i dispositivi con memoria persistente

### <a name="configure-the-devices"></a>Configurare i dispositivi

In Linux usare l'utilità `ndctl`.

- Installare `ndctl` per configurare il dispositivo con memoria persistente. L'utilità è disponibile [qui](https://docs.pmem.io/getting-started-guide/installing-ndctl).
- Usare `ndctl` per creare uno spazio dei nomi. Gli spazi dei nomi sono interfoliati tra i moduli NVDIMM di memoria persistente e possono offrire diversi tipi di accesso dello spazio utente alle aree di memoria sul dispositivo. `fsdax` è la modalità predefinita e desiderata per SQL Server.

```bash 
ndctl create-namespace -f -e namespace0.0 --mode=fsdax* --map=dev
```

Si noti che è stata scelta la modalità `fsdax` e viene usata la memoria di sistema per archiviare i metadati per pagina. È consigliabile usare `--map=dev`. In questo modo, i metadati vengono archiviati direttamente nello spazio dei nomi. L'archiviazione dei metadati in memoria tramite `--map=mem` è al momento considerata sperimentale.

Usare `ndctl` per verificare lo spazio dei nomi. 
  
Di seguito è riportato output di esempio:

```bash
# ndctl list -N
{
  "dev":"namespace0.0",
  "mode":"fsdax",
  "map":"dev",
  "size":4294967296,
  "sector_size":512,
  "blockdev":"pmem0",
  "numa_node":0
}
```

### <a name="create-and-mount-pmem-device"></a>Creare e montare il dispositivo con memoria persistente

Ad esempio, con XFS

```bash
mkfs.xfs -f /dev/pmem0
mount -o dax,noatime /dev/pmem0 /mnt/dax
xfs_io -c "extsize 2m" /mnt/dax
```

Ad esempio, con EXT4

```bash
mkfs.ext4 -b 4096 -E stride=512 -F /dev/pmem0
mount -o dax,noatime /dev/pmem0 /mnt/dax
```

## <a name="technical-considerations"></a>Considerazioni tecniche

- Bloccare l'allocazione di 2 MB per XFS/EXT4, come descritto sopra
- Il mancato allineamento tra l'allocazione dei blocchi e `mmap` ha come risultato il fallback invisibile a 4 kB
- Le dimensioni dei file devono essere un multiplo di 2 MB (modulo a 2 MB)
- Non disabilitare le pagine di tipo THP (Transparent Huge Page) (abilitate per impostazione predefinita nella maggior parte delle distribuzioni)

Dopo che il dispositivo è stato configurato con `ndctl`, formattato e montato, è possibile inserirvi i file di database oppure creare un nuovo database.

Poiché i dispositivi con memoria persistente sono protetti da O_DIRECT (I/O diretto), è consigliabile abilitare il flag di traccia 3979 per disabilitare il meccanismo di scaricamento forzato. Per altre informazioni, vedere il [supporto di FUA](https://support.microsoft.com/help/4131496/enable-forced-flush-mechanism-in-sql-server-2017-on-linux). Gli elementi interni della funzionalità di accesso forzato alle unità (FUA, Forced Unit Access) sono illustrati in [questo articolo](/archive/blogs/bobsql/sql-server-on-linux-forced-unit-access-fua-internals).

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su SQL Server in Linux, vedere [SQL Server in Linux](sql-server-linux-overview.md).
Per le procedure consigliate relative alle prestazioni per SQL Server in Linux, vedere [Procedure consigliate per le prestazioni](sql-server-linux-performance-best-practices.md).