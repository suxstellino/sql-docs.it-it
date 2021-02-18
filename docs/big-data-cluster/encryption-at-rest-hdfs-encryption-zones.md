---
title: Guida all'uso delle zone di crittografia HDFS in cluster Big Data di SQL Server
titleSuffix: SQL Server big data clusters
description: Questo articolo illustra come usare la funzionalità delle zone di crittografia HDFS di cluster Big Data di SQL Server
author: DaniBunny
ms.author: dacoelho
ms.reviewer: mihaelab
ms.date: 10/19/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 4c56d065de396a282c97c0723f118e5911617544
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489295"
---
# <a name="sql-server-big-data-clusters-hdfs-encryption-zones-usage-guide"></a>Guida all'uso delle zone di crittografia HDFS in cluster Big Data di SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questa guida illustra come usare le funzionalità di crittografia dei dati inattivi di cluster Big Data di SQL Server per crittografare le cartelle HDFS usando le zone di crittografia. Vengono inoltre illustrate le attività di gestione delle chiavi di HDFS.

Si noti che è presente una zona di crittografia predefinita montata __```/securelake```__ pronta per essere utilizzata. È stata creata con una chiave a 256 bit generata dal sistema denominata __securelakekey__. Questa chiave può essere usata per creare zone di crittografia aggiuntive.

## <a name="prerequisites"></a><a id="prereqs"></a> Prerequisiti

- [SQL Server cluster di Big Data CU8 +](release-notes-big-data-cluster.md) con [Active Directory](active-directory-prerequisites.md) integrazione.
- Utente con privilegi amministrativi.
- [!INCLUDE[azdata](../includes/azure-data-cli-azdata.md)] configurato e connesso al cluster in modalità AD.

## <a name="create-an-encryption-zone-using-the-provided-system-managed-key"></a>Creare una zona di crittografia usando la chiave gestita e fornita dal sistema

1. Creare una cartella HDFS

   ```console
   azdata bdc hdfs mkdir -p /user/zone/folder
   ```

1. Eseguire il comando di creazione della zona di crittografia per crittografare la cartella usando la chiave __securelakekey__.

   ```console
   azdata bdc hdfs encryption-zone create --path /user/zone/folder --keyname securelakekey
   ```

## <a name="create-a-custom-new-key-and-encryption-zone"></a>Creare una chiave e una zona di crittografia nuove

1. Usare il modello seguente per creare una chiave a 256 bit.

   ```console
   azdata bdc hdfs key create -n mydatalakekey
   ```

1. Creare e crittografare un nuovo percorso HDFS usando la chiave utente.

   ```console
   azdata bdc hdfs encryption-zone create --path /user/mydatalake --keyname mydatalakekey
   ```

## <a name="hdfs-key-rotation-and-encryption-zone-re-encryption"></a>Ripetizione della crittografia della chiave HDFS e crittografia della zona di crittografia

1. Viene creata una nuova versione di __securelakekey__ con il nuovo materiale della chiave.

   ```console
   azdata hdfs bdc key roll -n securelakekey
   ```

1. Crittografa nuovamente la zona di crittografia associata alla chiave precedente

   ```console
   azdata bdc hdfs encryption-zone reencrypt --path /securelake --action start
   ```

## <a name="hdfs-key-and-encryption-zone-monitoring"></a>Monitoraggio della chiave e della zona di crittografia HDFS

1. Per monitorare lo stato di una nuova crittografia della zona di crittografia 

   ```console
   azdata bdc hdfs encryption-zone status
   ```

1. Per ottenere le informazioni di crittografia su un file in una zona di crittografia

   ```console
   azdata bdc hdfs encryption-zone get-file-encryption-info --path /securelake/data.csv
   ```

1. Elenco di tutte le zone di crittografia

   ```console
   azdata bdc hdfs encryption-zone list
   ```

1. Per elencare tutte le chiavi disponibili per HDFS

   ```console
   azdata bdc hdfs key list
   ```

1. Per creare una chiave personalizzata per la crittografia HDFS. Le dimensioni possibili sono 128, 192 256. Il valore predefinito è 256

   ```console
   azdata hdfs key create --name key1 --size 256
   ```

## <a name="next-steps"></a>Passaggi successivi

Per informazioni sull'utilizzo dei cluster Big Data, vedere [Che cosa sono i [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).

Usare azdata con i [servizi dati con abilitazione di Azure Arc](/azure/azure-arc/data/)
