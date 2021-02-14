---
title: Configurazione dell'archiviazione per le tabelle con ottimizzazione per la memoria | Microsoft Docs
description: Informazioni su come configurare la capacità di archiviazione e le operazioni di input/output al secondo (IOPS) per le tabelle ottimizzate per la memoria in SQL Server.
ms.custom: ''
ms.date: 1/15/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 6e005de0-3a77-4b91-b497-14cc0f9f6605
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 892ce217acbc07bdd98ca7166aeb0a9211b2adea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350670"
---
# <a name="configuring-storage-for-memory-optimized-tables"></a>Configurazione dell'archiviazione per le tabelle con ottimizzazione per la memoria
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  È necessario configurare la capacità di archiviazione e le operazioni di input/output al secondo (IOPS).  
  
## <a name="storage-capacity"></a>Capacità di archiviazione  

Usare le informazioni in [Stimare i requisiti di memoria delle tabelle ottimizzate per la memoria](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md) per stimare le dimensioni in memoria delle tabelle ottimizzate per la memoria durevoli del database. Poiché gli indici non vengono mantenuti per le tabelle ottimizzate per la memoria, non includere le dimensioni degli indici. 
 
Dopo aver determinato le dimensioni, è necessario fornire spazio su disco sufficiente per contenere i file di checkpoint, che vengono usati per archiviare i dati appena modificati. I dati archiviati contengono non solo il contenuto di nuove righe aggiunte alle tabelle in memoria, ma anche nuove versioni delle righe esistenti. Quando le righe vengono inserite o aggiornate, la quantità di dati archiviati aumenta. Le versioni delle righe vengono unite e le risorse di archiviazione vengono recuperate quando si verifica il troncamento del log. Se per qualsiasi motivo il troncamento del log subisce ritardi, le dimensioni dell'archivio OLTP in memoria aumenteranno.

Un buon punto di partenza per ridimensionare le risorse di archiviazione per questa area consiste nel riservare una quantità di spazio pari a quattro volte le dimensioni delle tabelle durevoli in memoria. Monitorare l'utilizzo dello spazio e prepararsi a espandere le risorse di archiviazione disponibili, se necessario.
  
## <a name="storage-iops"></a>IOPS di archiviazione  
 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] consente di aumentare notevolmente la velocità effettiva del carico di lavoro. Pertanto, è importante verificare che le operazioni di IO non rappresentino un collo di bottiglia.  
  
-   Quando si esegue la migrazione delle tabelle basate su disco nelle tabelle ottimizzate per la memoria, verificare che il log delle transazioni sia in un supporto di archiviazione che supporti l'attività aumentata del log delle transazioni. Ad esempio, se il supporto di archiviazione supporta le operazioni del log delle transazioni a 100 MB/sec e le tabelle ottimizzate per la memoria restituiscono prestazioni cinque volte superiori, anche il supporto di archiviazione del log delle transazioni deve essere in grado di supportare un incremento di cinque volte delle prestazioni, per impedire all'attività del log delle transazioni di diventare un collo di bottiglia.  
  
-   Le tabelle ottimizzate per la memoria sono persistenti nei file di checkpoint distribuiti in uno o più contenitori. In genere è necessario eseguire il mapping di ogni contenitore al relativo dispositivo di archiviazione, che consente di aumentare la capacità di archiviazione e migliorare gli IOPS. È necessario assicurarsi che le operazioni di IOPS sequenziali del supporto di archiviazione possano supportare fino a 3 volte la velocità effettiva del log delle transazioni. Le operazioni di scrittura nei file di checkpoint sono di 256 kB per i file di dati e di 4 kB per i file differenziali.
  
     - Se, ad esempio, le tabelle ottimizzate per la memoria generano 500 MB/sec di attività nel log delle transazioni, l'archiviazione per le tabelle ottimizzate per la memoria deve supportare IOPS da 1,5 GB/sec. La necessità di supportare fino a 3 volte la velocità effettiva del log delle transazioni deriva dall'analisi che le coppie di file di dati e differenziali vengono prima scritti con dati iniziali e quindi devono essere letti e riscritti come parte di un'operazione di merge.  
  
- Un altro fattore per stimare gli IOPS per l'archiviazione è il tempo di recupero per le tabelle ottimizzate per la memoria. I dati delle tabelle durevoli devono essere letti in memoria prima che un database viene reso disponibile alle applicazioni. In genere, il caricamento dei dati nelle tabelle ottimizzate per la memoria può essere eseguito alla velocità delle operazioni di IOPS. Pertanto se l'archiviazione totale per le tabelle ottimizzate per la memoria durevoli è di 60 GB e si desidera essere in grado di caricare i dati in 1 minuto, le operazioni di IOPS per l'archiviazione devono essere impostati su 1 GB/sec.  
  
-   I file di checkpoint vengono in genere distribuiti in modo uniforme in tutti i contenitori, se lo spazio disponibile lo consente. Con SQL Server 2014 è necessario eseguire il provisioning di un numero dispari di contenitori per ottenere una distribuzione uniforme. A partire dalla versione 2016, sia un numero pari che un numero dispari di contenitori permette una distribuzione uniforme.
  
## <a name="encryption"></a>Crittografia  
 In [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive l'archiviazione per le tabelle ottimizzate per la memoria verrà crittografata quando inattive come parte dell'abilitazione di Transparent Data Encryption (TDE) nel database. Per altre informazioni, vedere [Transparent Data Encryption](../../relational-databases/security/encryption/transparent-data-encryption.md). In [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] i file di checkpoint non vengono crittografati, anche se TDE è abilitata nel database.

 I dati nelle tabelle ottimizzate per la memoria [non durevoli](../../relational-databases/in-memory-oltp/defining-durability-for-memory-optimized-objects.md) (SCHEMA_ONLY) non vengono mai scritti su disco. Di conseguenza, TDE non si applica a tali tabelle.
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e gestione dell'archiviazione per gli oggetti ottimizzati per la memoria](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)  
  
  
